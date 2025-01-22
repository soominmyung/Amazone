# A shopping mall database in MongoDB with PyMongo

#### Group project at the University of Manchester, 2023

## Project Overview

The **Amazone Database** project is a comprehensive NoSQL database designed for an online delivery system. The primary goal is to leverage the flexibility of NoSQL to handle unstructured and semi-structured data while optimizing data storage, retrieval, and application functionalities for a dynamic delivery service platform.

### Key Features:
- Flexible data modeling using multiple schemas.
- Real-time geospatial queries for delivery optimization.
- Comprehensive product recommendation system.
- Dynamic shipping cost calculation based on product type and quantity.
- Managerial queries for data visualization and insights.

## Table of Contents
1. [Introduction](#introduction)
2. [Database Schema](#database-schema)
3. [Application Functions](#application-functions)
4. [Data Visualization](#data-visualization)
5. [Sample Queries and Results](#sample-queries-and-results)
6. [References](#references)

---

## Introduction

This project was developed as part of the DATA70141 course on understanding databases. It explores the creation of a NoSQL database for the online delivery platform **Amazone**, emphasizing:
- The benefits of using NoSQL over traditional relational databases.
- The rationale behind schema design.
- Implementation of a working application and demonstration of its capabilities.

---

## Database Schema

### Schema Overview

The schema has 15 collections, considering the flexibility of MongoDB(NoSQL) database.
Especially, products can take various form according to its category (Mobile Phone, CD, Book, Home Appliance, Fresh Product).

And each collection was dividened into two types (Frequently Read, Frequently Written) to decide whether to use Embedding or Referencing.

![image](https://github.com/user-attachments/assets/cdd9da35-1807-4127-8fe4-34ff93103e07)

The final design is as below (See 'Appendix' in 'Amazone.pdf' file for JSON Schema).

![image](https://github.com/user-attachments/assets/9e85e9c6-25ae-4a72-9040-1d3209ae575c)


### Geospatial Queries / Operations

- **Geospatial Queries**: Used for locating the nearest partners and warehouses.
- **2DSPHERE Indexes**: Optimized for radial geospatial operations.


### Bucket / Computed / Extended Reference Patterns

Each pattern is used for certain collections for higher efficiency of the database.

* 'DailyIntentoryLevel' collection follows the bucket pattern as it records inventory level according to time with additional information.
The pattern makes it easier to track the inventory level by improving index performance and leveraging pre-aggregation which simplifies data access.

* Computed pattern is used for costs and ratings across multiple collections (ShoppingBasket, CurrentOrdersFresh, CurrentOrdersOther, PastOrders, Partners, PartnerStatus).
The pattern provides increased calculation performance and allows for a reduction in CPU workload.

* Extended Reference pattern is used for shipping address over CurrentOrdersFresh, CurrentOrdersOther, and Pastorders collection
  as the address is repetitively and frequently indexed.
  Instead of copying all the information on the customer, this pattern allows only to copy the address field, reducing the workload thus enhancing the performance.

---

## Application Functions

### Core Functionalities:

1. **Add to Basket**
   - Function: `addToBasket(CustomerID, ProductID, Quantity)`
   - Allows customers to add items to their basket with dynamic updates for fresh and other product categories.

2. **Shipping Cost Calculation**
   - `shipping_cost_fresh` and `shipping_cost_other`: Dynamically calculate shipping costs based on basket contents.

3. **Order Management**
   - `basketToOrder`: Moves items from the basket to current orders, assigns drivers, and calculates ETAs.

4. **Product Recommendations**
   - Utilizes customer history and product tags for personalized suggestions.

5. **Managerial Insights**
   - Functions like `Total_Sales_Over_Time` and `Top_10_Products_by_Revenue` provide valuable business insights.

---

## Data Visualization

The project includes several visualizations for managerial decision-making:
- **Total Sales Over Time**: Monthly sales trends.
- **Top 10 Products by Revenue**: Products generating the highest revenue.
- **Total Revenue by Customer**: Identifies top customers by purchase value.
- **Revenue by Tags**: Highlights top-performing and underperforming product categories.

---

## Sample Queries and Results

1. **Top 10 Products by Revenue**
   ```python
   Top_10_Products_by_Revenue(PastOrders)
   ```
   Generates a bar chart displaying the top revenue-generating products.

2. **Geospatial Query**: Finding the nearest Morrison with stock.
   ```python
   FindMorrisonWithStock(item_id, quantity)
   ```
   Identifies available stock and the nearest warehouse for delivery.

3. **Revenue by Tags**:
   ```python
   Revenue_by_tag(Products, 'Top')
   ```
   Visualizes the revenue contribution by product tags.

---

## References

- Sadalage, P.J., & Fowler, M. (2013). *NoSQL Distilled: A Brief Guide to the Emerging World of Polyglot Persistence.*
- MongoDB Blog (2021). [How MongoDB Makes Custom E-commerce Easy](https://www.mongodb.com/blog).
- InfoQ (2020). [Sample E-Commerce System with MongoDB](https://www.infoq.com/articles/data-model-mongodb).

---

This README provides an overview of the project, its objectives, and implementation details. For more information, refer to the codebase and the full project report.




