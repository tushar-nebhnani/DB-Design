# Smart Elevator Management System - ER Diagram

## 📌 Project Overview
This document outlines the database design for a multi-building Smart Elevator Management System. The architecture models the physical infrastructure of buildings and elevators, alongside the dynamic operations of processing ride requests, dispatching elevators, and tracking maintenance schedules. 

The system is designed to be highly scalable, supporting complex routing rules (like express elevators or restricted floors) and maintaining a clear historical log of all rides and repairs.

---

## 🗄️ Detailed Table Explanations

The database is divided into three logical operational areas: Infrastructure, Ride Operations, and Maintenance.

### 1. Infrastructure Core
These tables represent the physical assets managed by the system.

* **`Building`**
  This is the foundational table of the system. It stores the core details of the physical properties being managed, including the building's name, city, address, and the total number of floors it contains. 
* **`Floor`**
  This table details the individual levels within a specific building. Aside from the numerical floor value, it includes an `alias` attribute to handle custom naming conventions (e.g., "Lobby", "Mezzanine", "Penthouse", "Parking Level 1").
* **`Elevator`**
  This table represents the actual elevator cars operating within a building. It tracks the elevator's identifier (label), its current operational status (e.g., "Active", "Idle", "Out of Service"), and strictly monitors its real-time location by tracking which floor it is currently stationed on.
* **`Elevator_Floor_Coverage`**
  Not all elevators serve all floors. This is a crucial mapping table that defines the specific floors a particular elevator is authorized or physically capable of reaching. This handles scenarios for "Express" elevators that only serve high floors, or "Service" elevators with access to maintenance levels.

### 2. Ride Operations
These tables handle the real-time logic of users requesting rides and the system fulfilling them.

* **`Ride_Request`**
  This table captures the user's intent. Whenever a user presses a button in the hallway or inside the cabin, a record is created. It logs the exact building, the floor the user is currently on (source), the floor they want to go to (destination), their intended direction (Up/Down), and the precise timestamp of the request.
* **`Ride_Allocation`**
  This table represents the elevator dispatch system's decision. Once a `Ride_Request` is generated, the system creates a `Ride_Allocation` to assign a specific elevator to fulfill that request. It tracks the exact lifecycle of the ride, logging when the elevator was dispatched and when the ride was successfully completed.

### 3. Maintenance & Support
This area handles the upkeep and safety of the physical assets.

* **`Maintenance_Log`**
  This table acts as the service history ledger. Whenever an elevator breaks down or undergoes routine service, a record is created here. It details the issue description, tracks the timestamps for when the repair started and ended, logs the current status of the repair, and records the ID of the technician responsible.

---

## 🔗 Table Relationships

The tables interact with each other to form a cohesive, normalized workflow. Here is a detailed breakdown of how they connect:

### Building Infrastructure Connections
* **Building to Floors (1-to-Many):** A single `Building` contains multiple `Floors`. Every floor must belong to one specific building.
* **Building to Elevators (1-to-Many):** A single `Building` houses multiple `Elevators`. Every elevator is strictly bound to the building it is installed in.
* **Building to Ride Requests (1-to-Many):** All user `Ride_Requests` are tied to the specific building where the request was made, allowing the system to isolate traffic and logic per property.

### Elevator Routing and Location
* **Elevator to Current Floor (1-to-1 at any given time):** The `Elevator` table relies on the `Floor` table to establish its current real-time location. An elevator can only be at one current floor at a time.
* **Elevator to Floor Coverage (Many-to-Many):** An elevator can serve many floors, and a floor can be served by many elevators. The `Elevator_Floor_Coverage` table acts as the bridge connecting the `Elevator` and `Floor` tables to map out exactly which car can stop at which level.

### Ride Request and Fulfillment Workflow
* **Ride Request to Floors (Many-to-1):** A single `Ride_Request` relies on the `Floor` table twice: once to establish the `source_floor` (where the user is) and once to establish the `destination_floor` (where the user is going).
* **Ride Request to Ride Allocation (1-to-1):** Every individual user `Ride_Request` results in exactly one `Ride_Allocation`. The request is the problem, and the allocation is the system's solution.
* **Elevator to Ride Allocation (1-to-Many):** Over its lifetime, a single `Elevator` will be assigned to fulfill thousands of `Ride_Allocations`. This relationship links the physical car to the historical record of the trips it has made.

### Maintenance Tracking
* **Elevator to Maintenance Log (1-to-Many):** A single `Elevator` will have multiple `Maintenance_Log` records over its lifespan, allowing facility managers to track the complete repair history and uptime of a specific car.
