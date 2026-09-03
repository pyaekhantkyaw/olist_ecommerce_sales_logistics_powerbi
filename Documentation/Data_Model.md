# Data Model

## 1. Model Architecture

The Power BI semantic model follows a **multi-fact Star Schema architecture** designed to handle the different levels of granularity present in the [Brazilian E-Commerce (Olist) dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

The model separates transactional data from descriptive attributes to support:

* Accurate aggregation across different transaction grains.
* Predictable filter propagation.
* Efficient DAX calculations.
* Time-intelligence analysis.
* Product, customer, seller, payment, review, and logistics analysis.
* Clear and maintainable Power BI relationships.

Rather than combining all transactional data into a single flattened table, the model preserves the original business grains of the Olist dataset.

The architecture is centered around two primary transaction areas:

* **`orders`** — Order-level lifecycle, fulfillment, and logistics.
* **`order_items`** — Product-level sales, pricing, freight, and seller information.

Additional transactional tables provide supporting detail for payments and customer reviews.

---

## 2. Fact / Transaction Tables

### 2.1 `orders`

The `orders` table represents the **order-level transaction grain**.

It acts as the primary hub for the order lifecycle, containing information such as:

* `order_id`
* `customer_id`
* `order_status`
* `order_purchase_timestamp`
* `order_approved_at`
* `order_delivered_carrier_date`
* `order_delivered_customer_date`
* `order_estimated_delivery_date`

This table is primarily used for:

* Order volume analysis.
* Order-status analysis.
* Fulfillment lifecycle analysis.
* Delivery performance.
* SLA and delay analysis.

---

### 2.2 `order_items`

The `order_items` table represents the **line-item grain**, where each row corresponds to a product item within an order.

Key attributes include:

* `order_id`
* `order_item_id`
* `product_id`
* `seller_id`
* `price`
* `freight_value`

This table is the primary source for:

* Product sales analysis.
* Revenue calculations.
* Freight analysis.
* Seller performance.
* Product and category performance.

Because multiple items can belong to a single order, this table must not be aggregated as if each row represented a unique order.

---

### 2.3 `order_payments`

The `order_payments` table represents individual payment records associated with an order.

It contains information such as:

* `order_id`
* `payment_type`
* `payment_installments`
* `payment_value`

This table supports analysis of:

* Payment methods.
* Payment value.
* Installment behavior.
* Payment distribution across orders.

The table retains its original payment-level grain rather than being unnecessarily flattened into the `orders` table.

---

### 2.4 `order_reviews`

The `order_reviews` table contains customer review and feedback information.

Important attributes include:

* `review_id`
* `order_id`
* `review_score`
* `review_comment_title`
* `review_comment_message`
* `review_creation_date`
* `review_answer_timestamp`

This table supports:

* Customer satisfaction analysis.
* Review-score analysis.
* Review volume.
* Customer feedback analysis.
* Response-time analysis.

Duplicate `review_id` records identified during the cleaning process were removed before loading the final model.

---

## 3. Dimension & Supporting Tables

### 3.1 `DateTable`

`DateTable` is a dedicated calendar dimension created using DAX.

It provides a continuous date structure for time-based analysis and supports Power BI Time Intelligence calculations such as:

* Year-to-Date (YTD).
* Previous Year.
* Year-over-Year (YoY).
* Monthly trends.
* Quarterly analysis.
* Calendar-year comparisons.

The date table is connected to the relevant order date used for the primary sales and order timeline.

---

### 3.2 `customers`

The `customers` table provides customer-level attributes, including:

* `customer_id`
* `customer_unique_id`
* Customer location information.
* Geographic identifiers.

Two customer identifiers have different analytical purposes:

* **`customer_id`** represents the customer record associated with an order.
* **`customer_unique_id`** represents the underlying customer identity and can be used to identify repeat purchasing behavior across orders.

This distinction is important when calculating metrics such as unique customers and repeat customers.

---

### 3.3 `sellers`

The `sellers` table provides descriptive information about marketplace sellers.

It includes:

* `seller_id`
* Seller location information.
* Geographic identifiers.

The table provides the seller context required for seller-level performance analysis without duplicating seller attributes across every order-item record.

---

### 3.4 `products`

The `products` table contains product-level attributes, including:

* `product_id`
* Product category.
* Product dimensions.
* Product weight.
* Physical characteristics.

This table provides descriptive product information for analysis of product and category performance.

---

### 3.5 `product_category_name_english`

This supporting lookup table provides the English translation of the original Portuguese product-category names.

It is connected to the product category field in `products` and allows the report to present standardized English category labels.

Example:

`beleza_saude` → `health_beauty`

---

### 3.6 `geolocation`

The `geolocation` table provides geographic reference information using zip-code prefixes and latitude/longitude coordinates.

The dataset was grouped by zip-code prefix during Power Query transformation to reduce repeated geographic observations and provide a cleaner geographic reference for map-based analysis.

---

## 4. Relationship Architecture

The model primarily uses **one-to-many (`1:*`) relationships** with single-direction filtering from descriptive tables toward transactional tables.

This approach reduces unnecessary filter ambiguity and provides predictable behavior across report visuals.

### Relationship Matrix

| **Primary / Lookup Table (1)**  | **Related Table (*)** | **Key**                 | **Cross-Filter Direction** |
| ------------------------------- | --------------------- | ----------------------- | -------------------------- |
| `DateTable`                     | `orders`              | Date → Order Date       | Single                     |
| `customers`                     | `orders`              | `customer_id`           | Both (1:1)                     |
| `orders`                        | `order_items`         | `order_id`              | Single / Both*             |
| `orders`                        | `order_payments`      | `order_id`              | Single                     |
| `orders`                        | `order_reviews`       | `order_id`              | Single                     |
| `sellers`                       | `order_items`         | `seller_id`             | Single                     |
| `products`                      | `order_items`         | `product_id`            | Single                     |
| `product_category_name_english` | `products`            | `product_category_name` | Single                     |
| `geolocation`                   | `customers`           | `zip_code_prefix`       | Single                     |

> **Note:** `Both*` is intentionally configured in the final Power BI model for a specific analytical requirement. Otherwise, single-direction filtering is preferred.

---

## 5. Filter Propagation Strategy

The model was designed to keep filter propagation as predictable as possible.

Typical filtering flows from dimensions into transactional data.

For example:

```text
DateTable
    ↓
orders
    ↓
order_items
    ↓
Product / Seller analysis
```

Another analytical path can be represented as:

```text
Customer
    ↓
Orders
    ↓
Order Items
    ↓
Product / Seller
```

This structure allows report users to analyze transactional performance from multiple business perspectives while preserving the different grains of the source tables.

Bidirectional relationships are limited to situations where they provide a specific analytical benefit and where the resulting filter behavior has been validated.

---

## 6. Grain Management

One of the key modeling considerations is maintaining the correct grain of each transactional table.

| **Table**        | **Grain**                                | **Primary Analytical Purpose**    |
| ---------------- | ---------------------------------------- | --------------------------------- |
| `orders`         | One row per order                        | Orders, fulfillment, delivery     |
| `order_items`    | One row per order item                   | Sales, products, sellers, freight |
| `order_payments` | One row per payment record               | Payment behavior                  |
| `order_reviews`  | One row per review record                | Customer satisfaction             |
| `customers`      | One row per customer record              | Customer attributes               |
| `sellers`        | One row per seller                       | Seller attributes                 |
| `products`       | One row per product                      | Product attributes                |
| `DateTable`      | One row per calendar date                | Time intelligence                 |
| `geolocation`    | One row per grouped geographic reference | Geographic analysis               |

Maintaining these grains prevents double counting when measures are calculated across different transaction tables.

For example, **order count** should be calculated from the `orders` table rather than counting rows in `order_items`, because one order can contain multiple line items.

---

## 7. Star Schema Design Rationale

The model intentionally avoids flattening all tables into one large dataset.

Separating the transactional tables provides several advantages:

### Accuracy

Different business events occur at different grains.

For example:

* An order represents a purchase.
* An order item represents a product within that purchase.
* A payment represents a payment record.
* A review represents customer feedback.

Keeping these grains separate reduces the risk of double counting.

### Maintainability

Business logic can be developed around clearly defined tables and relationships rather than a single heavily duplicated dataset.

### Analytical Flexibility

The model supports analysis across multiple dimensions, including:

* Time.
* Customers.
* Products.
* Categories.
* Sellers.
* Geography.
* Payments.
* Reviews.

### DAX Compatibility

A structured semantic model provides predictable filter context for DAX measures and makes calculations easier to maintain.

---

## 8. Incremental Refresh Architecture

The model was prepared for **Incremental Refresh** using Power Query parameters:

* `RangeStart`
* `RangeEnd`

These parameters are applied to the relevant order timestamp field:

`orders[order_purchase_timestamp]`

The purpose is to allow Power BI Service to partition historical data and refresh only the required period during subsequent refresh operations.

This approach is particularly useful when working with larger datasets or when the model is extended with additional historical data.

Detailed Incremental Refresh configuration and Power BI Service deployment steps are documented separately in:

`Documentation/PowerBI_Service.md`

---

## 9. Model Validation

Before finalizing the semantic model, the following areas were validated:

* Relationship keys.
* Relationship cardinality.
* Cross-filter direction.
* Duplicate key behavior.
* Table grain.
* Date-table connectivity.
* Product and seller relationships.
* Customer relationship behavior.
* Geographic relationships.
* Potential double-counting scenarios.
* DAX behavior across related fact tables.

The model was tested through report interactions and DAX calculations to ensure that filters produced expected results across the different transaction grains.

---

## 10. Final Model Outcome

The final semantic model provides a structured foundation for analyzing the Brazilian E-Commerce dataset across:

* Sales and revenue.
* Orders and fulfillment.
* Products and categories.
* Customers.
* Sellers.
* Payments.
* Reviews.
* Delivery performance.
* Geographic distribution.

By preserving the original transaction grains and separating facts from descriptive attributes, the model provides a maintainable foundation for the project's **DAX measures, interactive report pages, and Power BI Service deployment**.

The next layer of the analytical architecture is documented in:

`Documentation/DAX_Measures.md`
## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
