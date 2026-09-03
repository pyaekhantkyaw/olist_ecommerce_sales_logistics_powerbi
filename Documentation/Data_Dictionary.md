# Data Dictionary

## 1. Dataset Overview

This project uses the **[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)**, sourced via Kaggle, to build an end-to-end sales and e-commerce analytics solution in Microsoft Power BI.

The dataset contains transactional, customer, seller, product, payment, review, and geographic information. The data is maintained across multiple related tables, each representing a different business entity or transaction grain.

| **Attribute**       | **Detail**                                                |
| ------------------- | --------------------------------------------------------- |
| **Source**          | Brazilian E-Commerce Public Dataset by Olist (via Kaggle) |
| **Primary Tools**   | Microsoft Power BI, Power Query (M), DAX                  |
| **Storage Format**  | Flat CSV files                                            |
| **Business Domain** | Brazilian E-Commerce / Online Retail                      |
| **Model Type**      | Star Schema with multiple transactional fact tables       |
| **Currency**        | Brazilian Real (BRL)                                      |

---

## 2. Data Grain

Data grain defines what a single row represents within each table. Understanding the grain is critical for accurate aggregation, relationship design, and DAX calculations.

The dataset contains multiple granularities rather than a single universal transaction level.

| **Table**                           | **Row-Level Grain**                                                           |
| ----------------------------------- | ----------------------------------------------------------------------------- |
| `orders`                            | One row represents one customer order                                         |
| `order_items`                       | One row represents one product line item within an order                      |
| `order_payments`                    | One row represents one payment record associated with an order                |
| `order_reviews`                     | One row represents one customer review record                                 |
| `customers`                         | One row represents one customer/order-customer record from the source dataset |
| `sellers`                           | One row represents one seller                                                 |
| `products`                          | One row represents one product                                                |
| `product_category_name_translation` | One row represents one product-category translation mapping                   |
| `geolocation`                       | One row represents a geographic reference associated with a zip-code prefix   |

Because different tables operate at different grains, measures must be designed carefully to avoid double counting when combining transactional information.

---

## 3. Data Types

The following data types are used throughout the Power BI model:

| **Data Type**      | **Usage**                                                                           |
| ------------------ | ----------------------------------------------------------------------------------- |
| **Text**           | IDs, names, categories, locations, descriptions, and other categorical fields       |
| **Whole Number**   | Quantities, payment installments, review scores, and other integer values           |
| **Decimal Number** | Product prices, freight values, payment values, and other monetary/numerical values |
| **Date**           | Calendar dates used for time-based analysis                                         |
| **Date/Time**      | Transaction and fulfillment timestamps where time-of-day information is relevant    |

Financial values are stored as decimal numbers to preserve accurate monetary calculations.

---

## 4. Table Definitions

### 4.1 `orders`

The `orders` table represents the **order-level fulfillment lifecycle** and acts as a central transactional hub for order status and delivery analysis.

Key fields include:

* `order_id` — unique identifier for an order
* `customer_id` — identifier linking the order to the customer record
* `order_status` — current status of the order
* `order_purchase_timestamp` — timestamp when the order was placed
* `order_approved_at` — timestamp when the order was approved
* `order_delivered_carrier_date` — date/time when the order was handed to the carrier
* `order_delivered_customer_date` — date/time when the order was delivered to the customer
* `order_estimated_delivery_date` — estimated customer delivery date

This table is primarily used for **order volume, order status, fulfillment lifecycle, delivery performance, and SLA analysis**.

---

### 4.2 `order_items`

The `order_items` table contains the **product-level transaction details** associated with each order.

Key fields include:

* `order_id` — identifier linking the item to an order
* `order_item_id` — item sequence within the order
* `product_id` — identifier linking the item to the product catalog
* `seller_id` — identifier linking the item to the seller
* `price` — item selling price
* `freight_value` — freight/shipping value associated with the item

This table provides the primary granularity for **product-level sales, revenue, freight, seller performance, and item-level analysis**.

---

### 4.3 `order_payments`

The `order_payments` table contains payment information associated with customer orders.

Key fields include:

* `order_id` — identifier linking the payment to an order
* `payment_type` — method used for payment, such as credit card or boleto
* `payment_installments` — number of payment installments
* `payment_value` — monetary value of the payment

This table supports **payment method, installment, and payment-value analysis**.

---

### 4.4 `order_reviews`

The `order_reviews` table contains customer feedback and review information.

Key fields include:

* `review_id` — identifier for the review
* `order_id` — identifier linking the review to the associated order
* `review_score` — customer rating score
* Review text fields — written customer feedback
* Review creation and response timestamps — timing information associated with the review process

This table supports **customer sentiment, review volume, rating distribution, and customer experience analysis**.

Duplicate review records based on `review_id` were removed during the ETL process to prevent inflated review counts and sentiment calculations.

---

### 4.5 `customers`

The `customers` table contains customer identity and geographic information.

Key fields include:

* `customer_id` — transaction-level customer identifier used to link customers to orders
* `customer_unique_id` — identifier representing the actual returning buyer across orders
* `customer_zip_code_prefix` — customer's geographic zip-code prefix
* `customer_city` — customer's city
* `customer_state` — customer's state

The distinction between `customer_id` and `customer_unique_id` is important:

* `customer_id` identifies the customer record associated with an order.
* `customer_unique_id` represents the underlying individual buyer and can therefore be used to identify **repeat customers across multiple orders**.

---

### 4.6 `sellers`

The `sellers` table contains seller/vendor information.

Key fields include:

* `seller_id` — unique seller identifier
* Seller zip-code prefix
* Seller city
* Seller state

This table supports **seller performance and geographic seller analysis**.

---

### 4.7 `products`

The `products` table contains catalog-level product attributes.

Key fields include:

* `product_id` — unique product identifier
* Product category attributes
* Product weight
* Product length
* Product height
* Product width
* Other physical product characteristics

This table provides descriptive product attributes for **product performance, category analysis, and physical-product analysis**.

---

### 4.8 `product_category_name_translation`

This table acts as a localization lookup table.

It maps the original Portuguese product-category names to their English equivalents.

The translation mapping allows product-category analysis to be presented consistently in English while preserving the original source classification.

---

### 4.9 `geolocation`

The `geolocation` table provides geographic information associated with Brazilian zip-code prefixes.

The source contains multiple geographic observations for the same zip-code prefix. The data was grouped by zip-code prefix to consolidate geographic coordinates and provide cleaner geographic mapping within Power BI.

The resulting geographic information supports **customer and seller location analysis and map visualization**.

---

## 5. Business Definitions

The following definitions describe the primary business concepts used throughout the report.

### Sales / Revenue

The monetary value generated from products sold through the e-commerce platform.

Sales-related calculations are primarily derived from the product-level transaction information in `order_items`.

### Quantity

The number of product units represented by the transaction or aggregated across selected transactions.

### Freight Value

The shipping/freight charge associated with an individual order item.

Freight value is analyzed separately from the product price to distinguish product revenue from logistics costs charged at item level.

### Profit

The monetary amount remaining after subtracting the project's defined associated costs from sales revenue.

The exact calculation methodology and DAX implementation are documented separately in `DAX_Measures.md`.

### Profit Margin

Profit expressed as a percentage of sales/revenue.

This metric is used to evaluate profitability relative to the amount of revenue generated.

### Average Order Value (AOV)

Average sales value generated per distinct order.

AOV is calculated using distinct orders rather than raw transaction rows to avoid distortion caused by orders containing multiple product line items.

### Delivery Days

The number of days associated with an order's fulfillment or delivery lifecycle, calculated from the relevant order and delivery timestamps.

### Delivery Delay / Days Late

The difference between the actual customer delivery date and the estimated delivery date.

* **Positive value** → delivery occurred later than estimated
* **Negative value** → delivery occurred earlier than estimated
* **Zero** → delivery occurred on the estimated date

This field is used for delivery-performance and SLA analysis.

### SLA Performance

A business measure used to evaluate whether orders were delivered within the expected estimated delivery timeframe.

The detailed calculation logic is implemented through DAX measures and documented in `DAX_Measures.md`.

---

## 6. Derived Business Fields

Several business concepts used within the report are derived from the source data rather than being directly provided as raw fields.

Examples include:

* Delivery Days
* Days Late / Delivery Delay
* SLA performance indicators
* Average Order Value
* Profit
* Profit Margin
* Other aggregated sales and customer-performance metrics

These calculations are designed to remain dynamic within Power BI so that they respond correctly to filters, slicers, drilldowns, and other report interactions.

The detailed calculation logic for these measures is documented in **`DAX_Measures.md`**.

---

## 7. Data Relationships & Model Architecture

The Power BI semantic model follows a **star-schema architecture** designed to separate transactional data from descriptive dimensions and provide controlled filter propagation.

### Fact / Transactional Tables

The main transactional tables are:

* `orders` — order-level fulfillment lifecycle
* `order_items` — product-level sales and freight transactions
* `order_payments` — payment-level transactions
* `order_reviews` — customer review transactions

These tables retain their respective grains rather than unnecessarily flattening all transactional information into a single table.

### Dimension / Supporting Tables

The model contains descriptive and lookup tables including:

* Date dimension
* Customer dimension
* Seller dimension
* Product dimension
* Geolocation dimension
* Product category translation mapping

The **Date table is created using DAX** and provides the calendar structure required for time-based analysis.

The detailed implementation of the Date table is documented in `DAX_Measures.md`.

### Relationship Behavior

Relationships are primarily configured as **one-to-many (`1:*`) relationships** with **single-direction cross-filtering**.

This approach is used to:

* Reduce ambiguity
* Maintain predictable filter propagation
* Support efficient analytical queries
* Reduce unintended many-to-many filtering behavior

Bidirectional cross-filtering is strictly limited to cases where it is required by the model design.

Overall, the model follows a star-schema architecture optimized for **Power BI analytical performance, accurate aggregation, and controlled DAX evaluation**.

---

## 8. Assumptions & Business Rules

### Missing Fulfillment Timestamps

Blank values in order milestone fields such as:

* `order_approved_at`
* `order_delivered_customer_date`

are intentionally retained.

These blanks can represent orders that are pending, canceled, lost, or otherwise did not reach a particular stage of the fulfillment lifecycle.

Removing these records would distort fulfillment and delivery metrics, so they remain in the dataset.

---

### Customer Identification

`customer_id` and `customer_unique_id` represent different business concepts.

`customer_id` is used as the transaction-level customer identifier associated with orders, while `customer_unique_id` represents the underlying individual buyer and is used when analyzing returning customers.

---

### Duplicate Reviews

Duplicate entries in `order_reviews` based on `review_id` are treated as data-quality/system duplication rather than multiple valid reviews for the same review identifier.

These duplicates were removed during ETL to prevent inflated review counts and inaccurate sentiment calculations.

---

### Transactional Grain

Different transactional tables operate at different grains.

Measures involving revenue, quantity, payments, reviews, or orders must therefore account for the appropriate grain and use distinct counting where required.

For example, order-level metrics should use distinct `order_id` rather than counting every row of `order_items`.

---

### Currency

All monetary values, including product price, freight value, and payment value, represent the original dataset currency:

**Brazilian Real (BRL).**

No currency conversion is applied unless explicitly documented elsewhere in the project.

---

### Product Category Localization

Product categories are evaluated using the provided Portuguese-to-English translation mapping to ensure consistent English-language reporting.

---

### Dynamic Metrics

Business performance metrics, including delivery performance, sales KPIs, and other analytical calculations, are computed dynamically using **DAX measures** wherever aggregation and report interactivity require it.

Detailed formulas and calculation logic are documented separately in `DAX_Measures.md`.

---

## 9. ETL Summary

The data preparation process was performed primarily using **Power Query (M)**.

Key preparation activities included:

* Data type enforcement
* Financial field conversion to Decimal Number
* ID fields converted to Text
* Date/time fields converted to appropriate Date/Time types
* Intentional retention of meaningful null values
* Duplicate review removal based on `review_id`
* Geographic grouping by zip-code prefix
* Preparation of data for the Power BI semantic model

Detailed transformation logic and the reasoning behind each cleaning decision are documented in **`Data_Cleaning.md`**.

---

## 10. Documentation Scope

This document focuses on the **meaning, grain, business definitions, assumptions, and structure of the dataset**.

The supporting project documentation provides further technical detail:

| **Document**         | **Purpose**                                                             |
| -------------------- | ----------------------------------------------------------------------- |
| `Data_Dictionary.md` | Dataset meaning, grain, definitions, relationships, and assumptions     |
| `Data_Cleaning.md`   | Power Query transformations and data-quality decisions                  |
| `Data_Model.md`      | Detailed semantic model and relationship architecture                   |
| `DAX_Measures.md`    | DAX formulas and calculation logic                                      |
| `Report_Design.md`   | Dashboard design, visual choices, and user experience                   |
| `PowerBI_Service.md` | Power BI Service deployment, refresh, security, alerts, and application |


## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
