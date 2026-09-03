# DAX Measures & Calculations

## 1. Overview

The DAX (Data Analysis Expressions) layer was developed to transform the cleaned Olist dataset into actionable business metrics for sales, logistics, customer satisfaction, seller performance, and operational analysis.

The calculations were designed around the semantic model and its underlying transaction grains rather than relying on a single flattened table.

The DAX layer contains both:

* **Measures** — dynamic calculations evaluated according to the current filter and visual context.
* **Calculated Columns** — row-level calculations created during model development where categorical classification or reusable row-level values were required.

### Key Development Principles

#### Measure Branching

Complex measures reuse existing base measures where appropriate.

For example:

`AOV` uses `[Total Revenue]` and `[OrderVolume]` rather than repeating their underlying calculations.

This improves maintainability and makes business logic easier to audit.

#### Context-Aware Calculations

Measures were designed to respond dynamically to slicers, filters, dimensions, and report interactions.

This allows the same calculation to produce different results across:

* Products
* Categories
* Sellers
* Customers
* Dates
* Order statuses
* Geographic segments

#### Explicit Blank Handling

Blank fulfillment dates were intentionally preserved during data cleaning.

DAX calculations therefore account for undelivered or incomplete orders where necessary rather than assigning artificial values.

#### Appropriate Calculation Location

Row-level transformations that need to exist as reusable fields were implemented as calculated columns, while aggregations and analytical KPIs were primarily implemented as measures.

---

# 2. Sales & Revenue Metrics

Primary tables:

* `order_items`
* `order_payments`
* `orders`

These calculations support analysis of marketplace revenue, transaction value, shipping costs, pricing, and customer spending behavior.

---

## 2.1 Measures

### Total Revenue

**Calculation Type:** Measure

**Business Logic:**
Calculates total product sales value by summing item prices. Freight charges are excluded from this metric.

```DAX
Total Revenue = SUM(order_items[price])
```

---

### Total Freight

**Calculation Type:** Measure

**Business Logic:**
Calculates the total freight/shipping value associated with sold order items.

```DAX
Total Freight = SUM(order_items[freight_value])
```

---

### Average Price

**Calculation Type:** Measure

**Business Logic:**
Calculates the average selling price across individual order items.

```DAX
Average Price = AVERAGE(order_items[price])
```

---

### Freight Ratio

**Calculation Type:** Measure

**Business Logic:**
Measures freight value relative to product revenue.

A higher ratio indicates that shipping costs represent a larger proportion of product value.

```DAX
Freight Ratio = 
DIVIDE(
    [Total Freight],
    [Total Revenue],
    0
)
```

---

### Total Payment Value

**Calculation Type:** Measure

**Business Logic:**
Calculates the total monetary value recorded in the payment transactions table.

```DAX
Total Payment Value = SUM(order_payments[payment_value])
```

---

### AOV

**Calculation Type:** Measure

**Business Logic:**
Calculates Average Order Value by dividing product revenue by the number of distinct orders in the current filter context.

This uses measure branching through `[Total Revenue]` and `[OrderVolume]`.

```DAX
AOV = 
DIVIDE(
    [Total Revenue],
    [OrderVolume],
    0
)
```

---

### Avg Payment Installments

**Calculation Type:** Measure

**Business Logic:**
Calculates the average number of payment installments recorded across payment transactions.

This can be used as an indicator of customer credit and installment-payment behavior.

```DAX
Avg Payment Installments = 
AVERAGE(order_payments[payment_installments])
```

---

# 3. Logistics & SLA Metrics

Primary tables:

* `orders`
* `order_items`

These calculations measure order volume, fulfillment completion, delivery speed, delivery variance, and seller fulfillment performance.

---

# 3.1 Calculated Columns — `orders`

## Days to Deliver

**Calculation Type:** Calculated Column

**Business Logic:**
Calculates the number of calendar days between order purchase and customer delivery.

For orders that do not contain a valid delivery timestamp, the result is returned as `BLANK()`.

```DAX
Days to Deliver = 
IF (
    NOT ISBLANK ( 'orders'[order_delivered_customer_date] ) 
        && NOT ISBLANK ( 'orders'[order_purchase_timestamp] ),
    DATEDIFF (
        'orders'[order_purchase_timestamp],
        'orders'[order_delivered_customer_date],
        DAY
    ),
    BLANK ()
)
```

---

## Order Purchase Date

**Calculation Type:** Calculated Column

**Business Logic:**
Converts the order purchase timestamp into a date-only value used to establish the relationship with the dedicated date dimension.

```DAX
orderpurchasedate = 
INT(orders[order_purchase_timestamp])
```

The resulting field is formatted as a Date in the Power BI model.

---

## OrderStatus

**Calculation Type:** Calculated Column

**Business Logic:**
Categorizes delivered orders according to whether the actual customer delivery occurred on or before the estimated delivery date.

Undelivered orders return `BLANK()`.

```DAX
OrderStatus = 
VAR DeliveryDate = orders[order_delivered_customer_date] 
VAR EstimateDate = orders[order_estimated_delivery_date] 
RETURN 
IF ( 
    ISBLANK(DeliveryDate), 
    BLANK(), 
    IF(
        DeliveryDate <= EstimateDate,
        "On Time",
        "Late"
    ) 
)
```

---

# 3.2 Measures

## OrderVolume

**Calculation Type:** Measure

**Business Logic:**
Calculates the number of distinct orders within the current filter context.

```DAX
OrderVolume = 
DISTINCTCOUNT(orders[order_id])
```

Using `DISTINCTCOUNT` ensures that multiple order-item rows do not artificially increase the order count.

---

## Completed Orders

**Calculation Type:** Measure

**Business Logic:**
Calculates the number of orders whose final order status is `delivered`.

```DAX
Completed Orders = 
CALCULATE (
    DISTINCTCOUNT ( 'orders'[order_id] ),
    'orders'[order_status] = "delivered"
)
```

---

## SLA_Performance

**Calculation Type:** Measure

**Business Logic:**
Calculates the percentage of delivered orders that arrived on or before the estimated delivery date.

Undelivered orders are excluded from both the numerator and denominator.

```DAX
SLA_Performance = 
DIVIDE ( 
    CALCULATE(
        COUNTROWS(orders),
        orders[order_delivered_customer_date] <= orders[order_estimated_delivery_date],
        NOT ISBLANK(orders[order_delivered_customer_date])
    ),
    CALCULATE(
        COUNTROWS(orders),
        NOT ISBLANK(orders[order_delivered_customer_date])
    ),
    0 
)
```

### Interpretation

* `100%` = all delivered orders met the estimated delivery date.
* Lower values indicate weaker on-time delivery performance.

---

## ActualDeliveryDays

**Calculation Type:** Measure

**Business Logic:**
Calculates the average actual delivery duration across orders that have a valid delivery duration.

The measure references the `Days to Deliver` calculated column.

```DAX
ActualDeliveryDays = 
AVERAGE(orders[Days to Deliver])
```

---

## AverageDeliveryTime

**Calculation Type:** Measure

**Business Logic:**
Calculates the average number of days between payment approval and customer delivery.

Orders without a valid approval or delivery timestamp are excluded.

```DAX
AverageDeliveryTime = 
AVERAGEX(
    FILTER(
        orders,
        NOT ISBLANK(orders[order_approved_at]) 
            && NOT ISBLANK(orders[order_delivered_customer_date])
    ),
    DATEDIFF(
        orders[order_approved_at],
        orders[order_delivered_customer_date],
        DAY
    )
)
```

This metric focuses on the period after order approval rather than the complete purchase-to-delivery lifecycle.

---

## Avg Estimated Delivery Days

**Calculation Type:** Measure

**Business Logic:**
Calculates the average number of days between purchase and the estimated delivery date.

This represents the delivery timeframe communicated in the source data.

```DAX
Avg Estimated Delivery Days = 
AVERAGEX(
    FILTER(
        'orders',
        NOT ISBLANK('orders'[order_estimated_delivery_date]) 
            && NOT ISBLANK('orders'[order_purchase_timestamp])
    ),
    DATEDIFF(
        'orders'[order_purchase_timestamp],
        'orders'[order_estimated_delivery_date],
        DAY
    )
)
```

---

## AvgDaysLate

**Calculation Type:** Measure

**Business Logic:**
Calculates the average signed difference between estimated delivery and actual customer delivery.

The result can be interpreted as:

* **Negative** = delivered earlier than estimated.
* **Zero** = delivered on the estimated date.
* **Positive** = delivered later than estimated.

Only orders containing an actual delivery timestamp are included.

```DAX
AvgDaysLate = 
AVERAGEX(
    FILTER(
        orders,
        NOT ISBLANK(orders[order_delivered_customer_date])
    ),
    DATEDIFF(
        orders[order_estimated_delivery_date],
        orders[order_delivered_customer_date],
        DAY
    )
)
```

This provides a more informative operational view than a simple on-time/late classification because it measures the magnitude of delivery variance.

---

## AvgDaysLateComment

**Calculation Type:** Measure

**Business Logic:**
Generates dynamic text for cards or tooltip visuals based on the direction of the average delivery variance.

The wording automatically changes between deliveries being earlier or later than expected.

```DAX
AvgDaysLateComment = 
"On average, deliveries are " &
FORMAT(ABS([AvgDaysLate]), "0.00") &
IF(
    [AvgDaysLate] < 0,
    " days earlier than expected.",
    " days late."
)
```

---

## OnTimeRate_Seller

**Calculation Type:** Measure

**Business Logic:**
Measures seller-level on-time fulfillment performance using order-item context and the related order delivery dates.

```DAX
OnTimeRate_Seller = 
DIVIDE(
    CALCULATE(
        COUNTROWS(order_items),
        RELATED(orders[order_delivered_customer_date]) <= 
            RELATED(orders[order_estimated_delivery_date]),
        NOT ISBLANK(
            RELATED(orders[order_delivered_customer_date])
        )
    ),
    COUNTROWS(order_items),
    0
)
```

This allows seller performance to be evaluated through the order items associated with each seller.

---

# 4. Customer Sentiment & Support Metrics

Primary table:

`order_reviews`

These calculations support customer satisfaction, review distribution, and response-time analysis.

---

# 4.1 Calculated Columns

## ResponseTimeDays

**Calculation Type:** Calculated Column

**Business Logic:**
Calculates the number of days between review creation and review response timestamps.

```DAX
ResponseTimeDays = 
DATEDIFF(
    'order_reviews'[review_creation_date],
    'order_reviews'[review_answer_timestamp],
    DAY
)
```

---

## Response Time Range

**Calculation Type:** Calculated Column

**Business Logic:**
Groups review response times into operational categories for easier visualization and filtering.

```DAX
Response Time Range = 
SWITCH(
    TRUE(),
    'order_reviews'[ResponseTimeDays] = 0, "Same Day",
    'order_reviews'[ResponseTimeDays] <= 2, "1-2 Days",
    'order_reviews'[ResponseTimeDays] <= 5, "3-5 Days",
    "6+ Days"
)
```

### Categories

| **Response Time** | **Category** |
| ----------------- | ------------ |
| 0 days            | Same Day     |
| 1–2 days          | 1-2 Days     |
| 3–5 days          | 3-5 Days     |
| 6+ days           | 6+ Days      |

---

## Review Rating Label

**Calculation Type:** Calculated Column

**Business Logic:**
Transforms the numeric review score into a user-friendly descriptive label.

```DAX
Review Rating Label = 
SWITCH(
    'order_reviews'[review_score],
    5, "5 - Excellent",
    4, "4 - Good",
    3, "3 - Average",
    2, "2 - Poor",
    1, "1 - Terrible",
    "Unrated"
)
```

---

# 4.2 Measures

## Average_Review_Score

**Calculation Type:** Measure

**Business Logic:**
Calculates the average customer review score in the current filter context.

```DAX
Average_Review_Score = 
AVERAGE(order_reviews[review_score])
```

---

## Total Reviews

**Calculation Type:** Measure

**Business Logic:**
Calculates the number of review records in the cleaned review table.

```DAX
Total Reviews = 
COUNT(order_reviews[review_id])
```

The measure operates on the deduplicated review data produced during Power Query transformation.

---

## pct_1stars

**Calculation Type:** Measure

**Business Logic:**
Calculates the percentage of reviews receiving a 1-star rating.

This metric is used as an indicator of severe customer dissatisfaction.

```DAX
pct_1stars = 
DIVIDE(
    CALCULATE(
        COUNT(order_reviews[review_id]),
        order_reviews[review_score] = 1
    ),
    [Total Reviews],
    0
)
```

---

## pct_5stars

**Calculation Type:** Measure

**Business Logic:**
Calculates the percentage of reviews receiving a 5-star rating.

This provides an indicator of highly positive customer feedback.

```DAX
pct_5stars = 
DIVIDE(
    CALCULATE(
        COUNT(order_reviews[review_id]),
        order_reviews[review_score] = 5
    ),
    [Total Reviews],
    0
)
```

---

## Avg Response Time (Days)

**Calculation Type:** Measure

**Business Logic:**
Calculates the average number of days between review creation and review response.

```DAX
Avg Response Time (Days) = 
AVERAGE(order_reviews[ResponseTimeDays])
```

---

# 5. Dimension Helpers & Physical Attributes

Primary tables:

* `sellers`
* `products`

These calculations provide supporting fields for report readability and product-level analysis.

---

# 5.1 Seller Short ID

**Calculation Type:** Calculated Column — `sellers`

**Business Logic:**
Creates a shorter seller identifier for use in report visuals where the original GUID-style identifier is too long for practical display.

The first six characters are converted to uppercase and prefixed with `-`.

```DAX
Seller Short ID = 
"-" & UPPER(
    LEFT('sellers'[seller_id], 6)
)
```

---

# 5.2 ProductWeight

**Calculation Type:** Measure — `products`

**Business Logic:**
Calculates the total product weight in grams across the products included in the current filter context.

```DAX
ProductWeight = 
SUM(products[product_weight_g])
```

This can be used to support product physical-attribute analysis and selected logistics-related visuals.

---

# 6. Calculation Inventory

The following table summarizes the complete DAX calculation layer.

| **Calculation**               | **Type**          | **Primary Table** | **Purpose**                      |
| ----------------------------- | ----------------- | ----------------- | -------------------------------- |
| `Total Revenue`               | Measure           | `order_items`     | Product revenue                  |
| `Total Freight`               | Measure           | `order_items`     | Shipping value                   |
| `Average Price`               | Measure           | `order_items`     | Average item price               |
| `Freight Ratio`               | Measure           | `order_items`     | Freight relative to revenue      |
| `Total Payment Value`         | Measure           | `order_payments`  | Total payment value              |
| `AOV`                         | Measure           | Multiple          | Average order value              |
| `Avg Payment Installments`    | Measure           | `order_payments`  | Installment behavior             |
| `Days to Deliver`             | Calculated Column | `orders`          | Purchase-to-delivery duration    |
| `orderpurchasedate`           | Calculated Column | `orders`          | Date-only purchase field         |
| `OrderStatus`                 | Calculated Column | `orders`          | On-time / late classification    |
| `OrderVolume`                 | Measure           | `orders`          | Distinct order count             |
| `Completed Orders`            | Measure           | `orders`          | Delivered order count            |
| `SLA_Performance`             | Measure           | `orders`          | On-time delivery rate            |
| `ActualDeliveryDays`          | Measure           | `orders`          | Average actual delivery duration |
| `AverageDeliveryTime`         | Measure           | `orders`          | Approval-to-delivery duration    |
| `Avg Estimated Delivery Days` | Measure           | `orders`          | Promised delivery duration       |
| `AvgDaysLate`                 | Measure           | `orders`          | Delivery variance                |
| `AvgDaysLateComment`          | Measure           | `orders`          | Dynamic delivery commentary      |
| `OnTimeRate_Seller`           | Measure           | `order_items`     | Seller on-time rate              |
| `ResponseTimeDays`            | Calculated Column | `order_reviews`   | Review response duration         |
| `Response Time Range`         | Calculated Column | `order_reviews`   | Response-time bucket             |
| `Review Rating Label`         | Calculated Column | `order_reviews`   | Human-readable rating            |
| `Average_Review_Score`        | Measure           | `order_reviews`   | Average review rating            |
| `Total Reviews`               | Measure           | `order_reviews`   | Review volume                    |
| `pct_1stars`                  | Measure           | `order_reviews`   | 1-star review percentage         |
| `pct_5stars`                  | Measure           | `order_reviews`   | 5-star review percentage         |
| `Avg Response Time (Days)`    | Measure           | `order_reviews`   | Average response duration        |
| `Seller Short ID`             | Calculated Column | `sellers`         | Compact seller identifier        |
| `ProductWeight`               | Measure           | `products`        | Product weight aggregation       |

---

# 7. DAX Design Principles

The calculation layer follows several design principles.

### 7.1 Separate Row-Level Logic from Aggregation Logic

Calculated columns are used where a value must exist at row level, such as:

* Delivery categorization.
* Response-time buckets.
* Rating labels.
* Date extraction.
* Short identifiers.

Measures are used for dynamic aggregations such as:

* Revenue.
* Order volume.
* Delivery averages.
* SLA performance.
* Review percentages.

This separation helps maintain a cleaner Power BI semantic model.

---

### 7.2 Use `DIVIDE()` for Ratio Calculations

Ratio-based metrics use the `DIVIDE()` function instead of direct division.

For example:

```DAX
DIVIDE(
    [Total Freight],
    [Total Revenue],
    0
)
```

This provides safer handling when the denominator is zero or unavailable.

---

### 7.3 Preserve Meaningful Blanks

Blank delivery timestamps are treated as meaningful data states.

Calculations involving completed delivery therefore check for valid delivery dates before calculating durations or SLA results.

This prevents undelivered orders from being assigned misleading performance values.

---

### 7.4 Maintain Business-Meaningful Filter Context

Measures are designed to respond dynamically to report filters instead of producing fixed global values.

For example, `[Total Revenue]` can be evaluated at:

* Overall marketplace level.
* Product level.
* Category level.
* Seller level.
* Time period.
* Geographic segment.

This allows the same DAX measure to serve multiple report pages.

---

### 7.5 Reuse Measures Where Practical

Measure branching reduces repeated logic and makes the calculation layer easier to maintain.

For example:

```DAX
AOV = 
DIVIDE(
    [Total Revenue],
    [OrderVolume],
    0
)
```

Instead of repeating the complete revenue and distinct-order calculations, the measure references the existing business metrics.

---

# 8. DAX Validation

The calculations were validated through report visuals, slicers, filter interactions, and cross-table analysis.

Validation focused on:

* Revenue aggregation.
* Distinct order counts.
* Delivered-order counts.
* Delivery-duration calculations.
* SLA percentages.
* Delivery variance.
* Review-score aggregation.
* Review percentage calculations.
* Seller-level filtering.
* Product-level filtering.
* Time-based filtering.
* Blank handling.
* Potential double-counting across transaction tables.

The purpose of validation was to ensure that measures responded correctly to the semantic model and maintained consistent business definitions across different report contexts.

---

# 9. Final Calculation Layer

The completed DAX layer provides the analytical logic required for the project's Power BI reports.

It supports analysis across:

* Sales performance.
* Revenue and freight.
* Order volume.
* Payment behavior.
* Delivery performance.
* SLA compliance.
* Seller fulfillment.
* Customer satisfaction.
* Review distribution.
* Response times.
* Product attributes.

Together with the Power Query transformation layer and Star Schema semantic model, these DAX calculations form the core analytical engine of the Power BI solution.

The resulting measures are consumed by the report pages and interactive features documented in:

`Documentation/Report_Design.md`

Power BI Service deployment, refresh, RLS, alerts, and application publishing are documented separately in:

`Documentation/PowerBI_Service.md`
## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
