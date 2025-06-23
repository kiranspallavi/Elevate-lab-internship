# Elevate-lab-internship
# Online Bookstore Database

A comprehensive MySQL database schema designed for managing an online bookstore system. This database handles user management, book inventory, order processing, and payment tracking.

## 📋 Table of Contents

- [Overview](#overview)
- [Database Schema](#database-schema)
- [Installation](#installation)
- [Usage](#usage)
- [Database Structure](#database-structure)
- [Relationships](#relationships)
- [Sample Queries](#sample-queries)
- [Contributing](#contributing)

## 🎯 Overview

The Online Bookstore Database is designed to support a full-featured e-commerce bookstore with the following capabilities:

- User registration and authentication
- Author and book management
- Category-based book organization
- Order processing and tracking
- Payment handling
- Inventory management

## 🗄️ Database Schema

The database consists of 8 main tables:

| Table | Purpose |
|-------|---------|
| `Users` | Store customer information and credentials |
| `Authors` | Manage book authors and their biographical information |
| `Categories` | Define book categories for organization |
| `Books` | Store book details, pricing, and inventory |
| `BookCategories` | Many-to-many relationship between books and categories |
| `Orders` | Track customer orders and their status |
| `OrderItems` | Store individual items within each order |
| `Payments` | Record payment transactions for orders |

## 🚀 Installation

### Prerequisites

- MySQL 5.7 or higher
- MySQL client or GUI tool (MySQL Workbench, phpMyAdmin, etc.)

### Setup Instructions

1. **Clone or download** the SQL script
2. **Connect to your MySQL server**
3. **Execute the script** to create the database and tables:

```sql
mysql -u your_username -p < online_bookstore.sql
```

Or copy and paste the SQL commands directly into your MySQL client.

## 💻 Usage

### Creating the Database

```sql
CREATE DATABASE OnlineBookstore;
USE OnlineBookstore;
```

### Running the Schema

Execute the provided SQL script to create all tables with their relationships and constraints.

### Sample Data Insertion

Here are some example INSERT statements to get you started:

```sql
-- Insert sample authors
INSERT INTO Authors (name, bio) VALUES 
('J.K. Rowling', 'British author best known for the Harry Potter series'),
('Stephen King', 'American author of horror, supernatural fiction, suspense, and fantasy novels');

-- Insert sample categories
INSERT INTO Categories (name) VALUES 
('Fiction'), ('Fantasy'), ('Horror'), ('Mystery');

-- Insert sample books
INSERT INTO Books (title, price, stock, author_id) VALUES 
('Harry Potter and the Philosopher\'s Stone', 12.99, 50, 1),
('The Shining', 14.99, 30, 2);
```

## 🏗️ Database Structure

### Core Entities

**Users Table**
- Stores customer registration information
- Includes email validation through UNIQUE constraint
- Tracks account creation timestamp

**Books Table**
- Central inventory management
- Links to authors through foreign key
- Manages pricing and stock levels

**Orders & OrderItems**
- Two-table design for flexible order management
- Supports multiple items per order
- Preserves historical pricing in OrderItems

### Supporting Tables

**Authors & Categories**
- Reference tables for book metadata
- Authors include biographical information
- Categories enable flexible book classification

**BookCategories**
- Junction table enabling many-to-many relationships
- Books can belong to multiple categories
- Categories can contain multiple books

**Payments**
- Financial transaction tracking
- Links to orders for complete audit trail
- Supports multiple payment methods

## 🔗 Relationships

```
Users (1) ──── (M) Orders
Authors (1) ──── (M) Books
Books (M) ──── (M) Categories [via BookCategories]
Orders (1) ──── (M) OrderItems
Books (1) ──── (M) OrderItems
Orders (1) ──── (M) Payments
```

### Key Foreign Key Relationships:

- `Books.author_id` → `Authors.author_id`
- `Orders.user_id` → `Users.user_id`
- `OrderItems.order_id` → `Orders.order_id`
- `OrderItems.book_id` → `Books.book_id`
- `Payments.order_id` → `Orders.order_id`
- `BookCategories.book_id` → `Books.book_id`
- `BookCategories.category_id` → `Categories.category_id`

## 🔍 Sample Queries

### Find all books by a specific author
```sql
SELECT b.title, b.price, b.stock 
FROM Books b 
JOIN Authors a ON b.author_id = a.author_id 
WHERE a.name = 'J.K. Rowling';
```

### Get order details with customer information
```sql
SELECT u.name, u.email, o.order_date, o.status, 
       SUM(oi.quantity * oi.price) as total_amount
FROM Users u
JOIN Orders o ON u.user_id = o.user_id
JOIN OrderItems oi ON o.order_id = oi.order_id
GROUP BY u.user_id, o.order_id;
```

### Find books in specific categories
```sql
SELECT b.title, c.name as category
FROM Books b
JOIN BookCategories bc ON b.book_id = bc.book_id
JOIN Categories c ON bc.category_id = c.category_id
WHERE c.name IN ('Fiction', 'Fantasy');
```

### Check inventory levels
```sql
SELECT title, stock,
       CASE 
           WHEN stock < 10 THEN 'Low Stock'
           WHEN stock < 5 THEN 'Critical'
           ELSE 'Good'
       END as stock_status
FROM Books
ORDER BY stock ASC;
```

## 🛠️ Database Features

### Data Integrity
- Primary keys on all tables
- Foreign key constraints maintain referential integrity
- UNIQUE constraint on user emails prevents duplicates

### Scalability Considerations
- Separate OrderItems table allows flexible order composition
- Many-to-many BookCategories supports complex categorization
- Indexed primary keys for optimal query performance

### Audit Trail
- Timestamp fields track creation and transaction times
- Historical pricing preserved in OrderItems
- Payment records maintain complete financial audit trail

## 📊 ERD Visualization

To visualize the database structure:

1. Visit [dbdiagram.io](https://dbdiagram.io)
2. Use the provided dbdiagram.io schema code
3. Generate an interactive Entity Relationship Diagram

## 🤝 Contributing

Contributions are welcome! Please consider the following:

1. **Fork** the repository
2. **Create** a feature branch
3. **Test** your changes thoroughly
4. **Submit** a pull request with detailed description

### Potential Enhancements

- Add user roles and permissions
- Implement book ratings and reviews
- Add shopping cart functionality
- Include discount and coupon systems
- Add inventory alerts and reordering

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 📞 Support

For questions or issues:

- Create an issue in the repository
- Review the sample queries section
- Check the database relationships diagram

---

**Note**: Remember to secure your database with proper user permissions and never store plain text passwords in production environments. Consider implementing password hashing and proper authentication mechanisms.
