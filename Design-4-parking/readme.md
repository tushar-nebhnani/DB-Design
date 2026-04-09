# Comic-Con Multi-Zone Event Parking System

## 📌 Project Overview

This repository contains the database schema and Entity-Relationship (ER) diagram for a multi-zone event parking system, designed specifically for large-scale conventions like Comic-Con.
The system handles a high volume of diverse traffic, tracking vehicle categories (from bikes to EVs), special access reservations (VIPs, cosplayers, exhibitors), dynamic spot allocation, and complete parking lifecycles (sessions, ticketing, and payments).

## 🗄️ Database Entities

### 1. Classification & Rules

•⁠ ⁠*⁠ Vehicle_Category ⁠*: Defines the types of vehicles allowed (Bike, Car, SUV, Cab, EV) to ensure they are assigned appropriately sized spots.
•⁠ ⁠*⁠ Access_Category ⁠*: Manages reservation tiers such as General, VIP, Exhibitor, Cosplayer, and Staff.

### 2. Infrastructure

•⁠ ⁠*⁠ Parking_Zone ⁠*: Represents the physical layout of the facility (e.g., Level 1, Level 2, North Wing).
•⁠ ⁠*⁠ Parking_Spot ⁠*: The individual parking spaces. Tracks physical constraints (which vehicle category fits), access restrictions, EV charging availability, and real-time occupancy status.

### 3. Core Operations

•⁠ ⁠*⁠ Vehicle ⁠*: Records the details of the vehicles entering the premises.
•⁠ ⁠*⁠ Parking_Session ⁠*: The central transactional entity. It maps a vehicle to a specific spot for a duration of time (entry to exit). This allows a single vehicle to visit multiple times across different days, and a single spot to be reused by different vehicles.

### 4. Transactions

•⁠ ⁠*⁠ Ticket ⁠*: The physical or digital pass generated when a parking session begins.
•⁠ ⁠*⁠ Payment ⁠*: The financial record calculated and completed when a parking session ends.

## 🔗 Key Relationships

•⁠ ⁠*One-to-Many (⁠ 1 < * ⁠)\*:

- _Categories to Vehicles/Spots:_ One vehicle category applies to many vehicles and dictates the size of many parking spots.
- _Access to Spots:_ One access category can be assigned to multiple reserved spots.
- _Zones to Spots:_ One parking zone contains many individual parking spots.
- _Vehicles to Sessions:_ A single vehicle can have multiple parking sessions (e.g., attending a 3-day event).
- _Spots to Sessions:_ A single parking spot hosts many different sessions over time.
  •⁠ ⁠*One-to-One (⁠ 1 - 1 ⁠)*:
- _Session to Ticket:_ Each parking session generates exactly one ticket upon entry.
- _Session to Payment:_ Each parking session results in exactly one payment record upon exit.
