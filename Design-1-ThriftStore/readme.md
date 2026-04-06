# 🛒 Thrift & Handmade E-Commerce Database Schema

![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Database](https://img.shields.io/badge/Database-Relational-blue)

## 📖 Overview

This repository contains the database architecture for an e-commerce platform specializing in unique items, specifically **handmade** and **thrifted** goods. The schema is highly modular, designed to handle user profiles, unique product inventory, order fulfillment, and financial transactions.

## 📑 Table of Contents

- [Overview](#-overview)
- [Modules](#%EF%B8%8F-modules)
  - [User Management](#1-user-management)
  - [Product Catalog & Inventory](#2-product-catalog--inventory)
  - [Order Management](#3-order-management)
  - [Transactions & Payments](#4-transactions--payments)
  - [Fulfillment & Feedback](#5-fulfillment--feedback)
- [Entity Relationships](#-entity-relationships)
- [Getting Started](#-getting-started)
- [Contributing](#-contributing)

---

## 🗄️ Modules

### 1. User Management

Handles customer accounts and physical addresses.

- **`users`**: Central record for all customers (PII, email, contact, creation date).
- **`userAddress`**: Delivery locations (street, city, landmark, state, pincode).

### 2. Product Catalog & Inventory

Manages items available for sale, attributes, and stock (tailored for unique items).

- **`category`**: Defines item type (`handmade` or `thrifted`).
- **`products`**: Core catalog table (name, description, base price).
- **`productAttributes`**: Physical traits (shape, size, physical description) and an `isUsed` boolean flag.
- **`inventory`**: Tracks stock levels, including a critical `isUnique` boolean flag.

### 3. Order Management

Tracks customer purchases and item line-details.

- **`orders`**: Purchase record tracking total amount, platform (`website`, `offline`, `quick commerce`), and status.
- **`orderItems`**: Line-item details mapping specific products to an order.

### 4. Transactions & Payments

Manages the financial processing of orders.

- **`transaction`**: Records payment mode (`UPI`, `Card`, `EMI`) and status.
- **`transactionDetails`**: Auditing table tracking bank name and routing entities.

### 5. Fulfillment & Feedback

Handles the post-purchase lifecycle.

- **`shipping`**: Tracks physical delivery, dates, and transit status.
- **`reviews`**: Customer feedback on shipping, product quality, and value for money.

---

## 🔗 Entity Relationships

### User ↔️ Interactions

- `userAddress.userID` > `users.userID` (One-to-Many)
- `users.userID` < `orders.userID` (One-to-Many)
- `users.userID` - `shipping.userID` (One-to-One)
- `users.userID` - `reviews.userID` (One-to-One/Many)

### Order ↔️ Processing

- `orders.orderId` < `orderItems.orderId` (One-to-Many)
- `orders.orderId` - `transaction.orderId` (One-to-One)
- `orders.orderId` - `shipping.orderId` (One-to-One)

### Product ↔️ Details

- `products.productID` > `category.categoryId` (Many-to-One)
- `products.orderId` > `orders.orderId` (Many-to-One)
- `products.productAttributeId` - `productAttributes.productAttributeId` (One-to-One)
- `products.productID` - `inventory.productID` (One-to-One)

### Transaction ↔️ Audit

- `transaction.transactionId` - `transactionDetails.transactionId` (One-to-One)

---

## 🚀 Getting Started

_(Add instructions here on how to run your SQL scripts, e.g., how to spin up the database using Docker, or how to import the `.sql` files into PostgreSQL/MySQL.)_

1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
   ```
