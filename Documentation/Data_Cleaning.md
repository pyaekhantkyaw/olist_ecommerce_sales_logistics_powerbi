# Data Cleaning & Transformation

## 1. Objective

The purpose of the data-cleaning process was to transform the raw [Brazilian E-Commerce (Olist)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) CSV datasets into a reliable, analysis-ready dataset for Power BI.

The cleaning and transformation process was performed primarily using **Power Query (M)**.

The main objectives were to:

* Enforce appropriate data types for identifiers, financial values, timestamps, and numerical fields.
* Intentionally manage missing values to preserve real-world order and fulfillment states.
* Remove confirmed duplicate records that could distort analytical results.
* Standardize geographic and localized data for consistent reporting.
* Create structurally necessary transformation fields.
* Prepare the dataset for **Incremental Refresh** in Power BI Service.
* Produce a clean and consistent foundation for the Power BI semantic model.

---

## 2. Raw Data Assessment

The original CSV files were profiled in Power Query to identify potential data-quality issues across the different tables.

The assessment focused on:

* Duplicate review records.
* Null timestamps within the order fulfillment lifecycle.
* Missing product-category values.
* Repeated geographic coordinates associated with the same zip-code prefix.
* Data-type inconsistencies in identifiers and financial fields.
* The different levels of granularity across transactional tables.

Because the dataset contains multiple tables with different grains, transformations were applied according to the business meaning of each table rather than using a single cleaning rule across the entire dataset.

---

## 3. Data Cleaning Steps

### 3.1 Column Names

Column names were retained using the original `snake_case` naming convention, such as:

* `order_purchase_timestamp`
* `customer_id`
* `product_id`
* `freight_value`

Retaining the source naming convention improves traceability between the original dataset, Power Query transformations, and the final Power BI model.

---

### 3.2 Data Types

Each field was assigned an appropriate Power BI data type to support accurate calculations, relationships, filtering, and aggregation.

| **Field Type**         | **Examples**                                               | **Power BI Data Type** |
| ---------------------- | ---------------------------------------------------------- | ---------------------- |
| Identifiers / Keys     | `order_id`, `customer_id`, `review_id`                     | Text                   |
| Financial Values       | `price`, `freight_value`, `payment_value`                  | Decimal Number         |
| Timestamps             | `order_purchase_timestamp`, `review_answer_timestamp`      | Date/Time              |
| Integer / Count Fields | `product_weight_g`, `payment_installments`, `review_score` | Whole Number           |

Financial values were explicitly converted to **Decimal Number**, while identifier fields were stored as **Text** to prevent unintended numerical aggregation.

---

### 3.3 Missing Values

Missing values were evaluated according to their business meaning rather than being removed automatically.

#### Fulfillment Dates

Null values in fields such as:

* `order_approved_at`
* `order_delivered_customer_date`

were intentionally retained.

These blanks can represent orders that were pending, canceled, lost, or did not reach a particular stage of the fulfillment lifecycle.

Removing these records would potentially distort order-volume and fulfillment-performance analysis.

#### Product Categories

Null values in `product_category_name` were retained rather than removing the associated products.

Removing these records could exclude valid historical transactions and artificially reduce revenue or order-related metrics.

DAX calculations were designed to handle these blank values appropriately where necessary.

---

### 3.4 Duplicate Records

Duplicate records were investigated according to the expected behavior and grain of each table.

#### Order Reviews

Duplicate records based on `review_id` were removed to prevent duplicate review records from inflating:

* Review volume
* Average review scores
* Sentiment-related analysis

The deduplication was performed at the `review_id` level rather than simply removing rows based on the entire record.

#### Geolocation

The geolocation data contains repeated geographic observations associated with the same zip-code prefix.

A **Group By** transformation was applied to the zip-code prefix to consolidate latitude and longitude information into a cleaner geographic reference for Power BI map visualizations.

---

### 3.5 Text Standardization & Localization

The product-category translation table was integrated to standardize the original Portuguese product-category names into English.

For example:

`beleza_saude` → `health_beauty`

This allows the report to present consistent English-language category labels while preserving the classification provided by the original Olist dataset.

Additional concise identifiers were also engineered where required for report usability.

For example:

`Seller Short ID = "-" & UPPER(LEFT([seller_id], 6))`

This was used to improve readability in selected dashboard tooltips and matrix visuals.

---

### 3.6 Date Validation & Optimization

Date and timestamp fields were explicitly converted to appropriate **Date/Time** types.

Time information was preserved because some analyses require more precise timestamps for:

* Order processing time
* Delivery lifecycle analysis
* Customer response time
* SLA-related calculations

The date range and timestamp fields were also prepared for Power BI Service refresh requirements.

---

### 3.7 Incremental Refresh Preparation

The `orders[order_purchase_timestamp]` field was configured to support **Incremental Refresh** using `RangeStart` and `RangeEnd` parameters.

These parameters define the refresh window used by Power BI Service and allow historical data to be retained while limiting the amount of data processed during subsequent refreshes.

This prepares the model for more efficient refresh operations as the dataset grows.

---

### 3.8 Derived Columns

Structurally necessary columns were created during the transformation/modeling process where required.

One example is:

**`Days to Deliver`**

This field calculates the number of days associated with the order delivery lifecycle using the relevant order and delivery timestamps.

For orders that have not been delivered, the calculation returns `BLANK()` rather than assigning an artificial delivery duration.

More complex analytical calculations, such as aggregated delivery metrics and SLA performance, are implemented as **DAX measures** and documented separately in `DAX_Measures.md`.

---

## 4. Power Query Transformations Summary

The primary Power Query (M) transformations applied across the dataset included:

1. Connecting to the raw CSV sources.
2. Promoting and validating headers.
3. Enforcing appropriate data types.
4. Preserving meaningful null values.
5. Removing confirmed duplicate review records.
6. Grouping geolocation data by zip-code prefix.
7. Merging product-category translation data.
8. Standardizing selected text fields.
9. Creating structurally necessary transformation columns.
10. Configuring `RangeStart` and `RangeEnd` parameters for Incremental Refresh.
11. Validating the transformed data before loading it into the Power BI model.

---

## 5. Data Quality Validation

After the transformation process, the resulting tables were reviewed to verify that the cleaning operations produced an analysis-ready dataset.

Validation checks included:

* Data-type verification.
* Duplicate `review_id` validation.
* Null-value inspection for important fields.
* Validation of relationship keys.
* Financial-value inspection.
* Date and timestamp validation.
* Product-category translation completeness.
* Geographic grouping validation.
* Verification that undelivered orders do not receive an artificial delivery duration.
* Verification that the transformed tables remain compatible with the intended Power BI data model.

---

## 6. Before vs After

| **Area**               | **Before**                                                 | **After**                                                  |
| ---------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| Review Data            | Duplicate `review_id` records could inflate review metrics | Duplicate review records removed                           |
| Geographic Data        | Repeated coordinates for the same zip-code prefix          | Consolidated geographic reference                          |
| Missing Logistics Data | Null fulfillment timestamps present                        | Meaningful nulls retained and handled by calculations      |
| Product Categories     | Original Portuguese category names                         | English category mapping applied                           |
| Identifiers            | Potentially inconsistent source typing                     | Keys explicitly stored as Text                             |
| Financial Values       | Source numeric fields required validation                  | Financial fields stored as Decimal Number                  |
| Date/Time Fields       | Source timestamp formatting                                | Explicit Date/Time data types                              |
| Refresh Architecture   | Full dataset processing                                    | `RangeStart` / `RangeEnd` prepared for Incremental Refresh |

---

## 7. Data Cleaning Principles

The ETL process was guided by three core principles.

### 7.1 Preserve Business Reality

Data was not removed simply because it contained blanks.

Meaningful null values, particularly within order-fulfillment timestamps, were preserved because they represent valid states within the e-commerce lifecycle.

Only records identified as duplicate system records were removed.

### 7.2 Transform Intentionally

Every transformation was performed for a specific analytical or technical purpose.

Examples include:

* Category translation for consistent reporting.
* Geographic grouping for cleaner map visualization.
* Data-type enforcement for accurate calculations.
* Derived fields for fulfillment analysis.
* Incremental Refresh parameters for scalable refresh operations.

### 7.3 Optimize for Scale

The dataset was prepared with Power BI Service refresh requirements in mind.

The use of `RangeStart` and `RangeEnd` parameters provides the structural foundation for Incremental Refresh, allowing the model to process relevant periods rather than unnecessarily reprocessing the entire historical dataset during every refresh.

---

## 8. Final Outcome

The final dataset was transformed into a structured and validated analytical foundation for Power BI.

The resulting model supports analysis across:

* Revenue and sales performance
* Product and category performance
* Customer behavior
* Seller performance
* Payment behavior
* Customer reviews and sentiment
* Delivery and fulfillment performance
* Geographic distribution

The cleaned data is then used as the foundation for the project's **Star Schema semantic model**, DAX calculations, interactive reports, and Power BI Service deployment.

Detailed model architecture is documented in `Data_Model.md`, while analytical calculations are documented in `DAX_Measures.md`.

## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
