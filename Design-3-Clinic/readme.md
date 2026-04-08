# Clinic Management System Database Schema

> A comprehensive relational database schema designed to manage hospital or clinic operations, including patient appointments, doctor consultations, laboratory tests, reporting, and billing.

## Table of Contents

- [Overview](#overview)
- [Database Entities](#database-entities)
- [Entity Relationships](#entity-relationships)
- [Usage](#usage)

## Overview

This database schema is architected to handle the end-to-end flow of a patient's visit to a healthcare facility. It tracks the patient from the initial appointment scheduling, through the doctor's consultation and potential lab tests, to the final generation of medical reports and payment processing.

## Database Entities

The schema consists of 9 core tables categorized by their operational domains:

### 👤 User Management

| Table         | Description                                              | Primary Key    |
| :------------ | :------------------------------------------------------- | :------------- |
| **Patient**   | Stores patient demographic and contact information.      | `patient_id`   |
| **Doctor**    | Stores medical professional details.                     | `doctor_id`    |
| **Specialty** | Lookup table for medical specialties (e.g., Cardiology). | `specialty_id` |

### 📅 Visit & Clinical Data

| Table            | Description                                              | Primary Key       |
| :--------------- | :------------------------------------------------------- | :---------------- |
| **Appointment**  | Records scheduled visits between patients and doctors.   | `appointment_id`  |
| **Consultation** | Stores the clinical notes, diagnosis, and time of visit. | `consultation_id` |

### 🔬 Laboratory & Diagnostics

| Table               | Description                                              | Primary Key       |
| :------------------ | :------------------------------------------------------- | :---------------- |
| **Test_Catalog**    | Master list of available medical tests and their prices. | `test_id`         |
| **Prescribed_Test** | Maps specific tests ordered during a consultation.       | `prescription_id` |
| **Report**          | Stores test results, summaries, and document URLs.       | `report_id`       |

### 💳 Billing

| Table       | Description                                         | Primary Key  |
| :---------- | :-------------------------------------------------- | :----------- |
| **Payment** | Tracks financial transactions tied to appointments. | `payment_id` |

---

## Entity Relationships

The database relies on a normalized relational structure to ensure data integrity.

- **Doctors & Specialties:** \* A **Specialty** can have many **Doctors** (`Specialty 1 : N Doctor`).
- **Appointments:** \* A **Patient** can book many **Appointments** (`Patient 1 : N Appointment`).
  - A **Doctor** can have many **Appointments** (`Doctor 1 : N Appointment`).
- **Consultations & Billing:**
  - An **Appointment** results in exactly one **Consultation** (`Appointment 1 : 1 Consultation`).
  - An **Appointment** can generate multiple **Payments** (e.g., split payments or installments) (`Appointment 1 : N Payment`).
- **Laboratory Flow:**
  - A **Consultation** can result in multiple **Prescribed Tests** (`Consultation 1 : N Prescribed_Test`).
  - A **Test_Catalog** item can be prescribed many times (`Test_Catalog 1 : N Prescribed_Test`).
  - Each **Prescribed Test** yields exactly one **Report** (`Prescribed_Test 1 : 1 Report`).

---

## Usage

This schema is written in DBML (Database Markup Language). You can use this code to automatically generate SQL scripts or visualize the database architecture.

**To visualize this schema:**

1. Copy the DBML code.
2. Navigate to a schema visualization tool like [dbdiagram.io](https://dbdiagram.io/).
3. Paste the code into the editor to generate an interactive Entity-Relationship (ER) diagram.

**To convert to SQL:**
You can use the `dbml2sql` CLI tool to generate a `.sql` file for PostgreSQL, MySQL, or SQL Server.
