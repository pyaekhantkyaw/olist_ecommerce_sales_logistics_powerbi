# Report Design

## 1. Design Philosophy

The Power BI report is designed as a **multi-page e-commerce analytics dashboard** that moves progressively from high-level business performance toward detailed operational and seller analysis.

The analytical journey follows:

> **Business Overview → Sales & Geography → Logistics → Customer Satisfaction → Seller Analysis → Seller Details**

The overall design prioritizes:

* Clear KPI-driven summaries.
* Consistent page structure.
* Business-focused analytical questions.
* Interactive filtering.
* A balance between summary visuals and detailed tables.
* Progressive navigation from overview to detailed seller analysis.
* Readability through a light report background, white visual containers, and blue/purple visual accents.

The report contains six pages:

1. **E-Commerce Sales Overview**
2. **Sales & Geographic Performance**
3. **Logistics & Supply Chain Operations**
4. **Customer Satisfaction & Seller Rating**
5. **Seller Analysis**
6. **Seller's Details**

The design allows users to begin with overall business performance and progressively investigate products, geography, delivery performance, customer reviews, and individual sellers.

---

# 2. Report Structure

## 2.1 Page 1 — E-Commerce Sales Overview

### Purpose

The first page acts as the executive-level overview of the e-commerce business.

It provides a concise view of:

* Sales.
* Revenue.
* Order value.
* Customer reviews.
* Delivery punctuality.
* Product-category performance.
* Payment-method distribution.

### Primary KPIs

The page highlights:

* Total Sales
* Total Revenue
* Average Order Value
* Average Review
* Punctuality %

These KPI cards make the most important business metrics immediately visible before users move into detailed analysis.

### Filters

* Date
* Year / Month
* Product Category

These filters allow users to change the reporting context without leaving the overview page.

### Main Visuals

#### Monthly Sales & Orders Trend

A combined trend visual compares sales value with order volume over time.

The visual helps identify:

* Changes in sales activity.
* Changes in order volume.
* Periods of increasing or decreasing performance.
* The relationship between sales value and order volume.

#### Top 10 Product Categories by Revenue

A horizontal bar chart ranks the highest-revenue product categories.

The horizontal layout provides additional space for longer category names and makes category comparisons easier to read.

#### Total Revenue by Payment Type

A doughnut chart presents the distribution of revenue across payment methods.

This provides a quick view of payment-method concentration and highlights the dominant payment method.

### Design Decision

The page follows an executive-dashboard structure:

> **Filters → KPIs → Trend → Category Ranking → Payment Distribution**

This establishes a clear visual hierarchy from headline metrics to supporting analysis.

---

## 2.2 Page 2 — Sales & Geographic Performance

### Purpose

This page shifts the focus from the overall business summary toward:

* Sales volume.
* Transaction activity.
* Geographic performance.
* Product-category performance.
* Shipping cost.
* Installment behavior.

### Primary KPIs

* Unit Sold
* Total Payments
* Average Shipping Cost
* Average Installments
* Average Transaction Value

These metrics provide greater visibility into operational sales activity and transaction behavior.

### Filters

* Date
* Product Category
* State

The geographic filter allows regional performance to be investigated more precisely.

### Main Visuals

#### Total Revenue by Customer State

A treemap compares revenue contribution across customer states.

The relative area of each state makes larger revenue contributors visually prominent and supports quick geographic comparison.

#### Product Category Detail Table

The table provides exact values for:

* Product Category
* Total Units Sold
* Total Revenue
* Average Price

The table complements graphical analysis by providing precise numerical values.

#### Payment Value by Installment Method

A column chart compares payment value across installment counts.

This makes the distribution of payment value across installment methods easier to investigate.

### Design Decision

The page intentionally combines:

* Geographic visualization.
* Exact product-category values.
* Payment and transaction analysis.

This provides both high-level visual comparison and detailed numerical analysis.

---

## 2.3 Page 3 — Logistics & Supply Chain Operations

### Purpose

This page changes the analytical perspective from sales performance to **fulfillment and logistics operations**.

The main analytical areas are:

* Delivery time.
* On-time delivery.
* SLA performance.
* Actual versus estimated delivery time.
* Shipping cost.
* Product weight.

### Primary KPIs

* Actual Delivery Days
* Average Delivery Time
* On-Time Delivery Rate
* Average Days Late

The KPI arrangement gives delivery performance immediate visibility.

### Filters

* Date
* Product Category
* Customer State

These filters allow logistics performance to be investigated by time, product category, and geography.

### Main Visuals

#### Shipping Cost vs Product Weight

A scatter plot compares product weight with shipping cost.

This visual supports investigation into whether heavier products are associated with higher shipping costs and helps identify the distribution of observations.

A trend line provides an additional indication of the overall relationship.

#### Delivery Performance Against SLA Goal

A gauge visual presents delivery performance against a reference scale.

This provides a compact way to assess the reported delivery-performance level.

#### Estimated vs Actual Delivery Days by State

A clustered horizontal bar chart compares:

* Actual Delivery Days.
* Estimated Delivery Days.

The comparison is presented by customer state to help identify geographic differences in delivery performance.

### Design Decision

Different visual forms are used for different analytical questions:

| Analytical Question                                  | Visual               |
| ---------------------------------------------------- | -------------------- |
| What is the current logistics performance?           | KPI Cards            |
| Is product weight associated with shipping cost?     | Scatter Plot         |
| How is delivery performing against the target/scale? | Gauge                |
| How do actual and estimated delivery times compare?  | Horizontal Bar Chart |

---

## 2.4 Page 4 — Customer Satisfaction & Seller Rating

### Purpose

This page connects customer review performance with:

* Product categories.
* Delivery time.
* Seller performance.

It provides both aggregate customer-satisfaction analysis and seller-level performance investigation.

### Primary KPIs

* Total Reviews
* Average Review
* 5-Star Rating %
* 1-Star Rating %

Using both positive and negative rating indicators provides a more balanced view of customer satisfaction.

### Filters

* Review Score
* Month Name
* Response Time Range

These filters allow satisfaction and support-response results to be investigated from multiple perspectives.

### Main Visuals

#### Review of Top 10 Category by Revenue

A 100% stacked horizontal bar chart displays the distribution of review scores across selected high-revenue product categories.

The five rating levels are represented separately:

* 1 Star
* 2 Star
* 3 Star
* 4 Star
* 5 Star

The 100% stacked structure emphasizes the composition of ratings and makes category-level distributions easier to compare.

#### Impact of Delivery Time on Reviews

A line chart examines review scores across different delivery times.

This supports investigation into whether customer ratings vary with the number of days required for delivery.

#### Sellers with Perfect Ratings & 80+ Sales

A table identifies sellers that meet the defined high-rating and sales-volume criteria.

Displayed fields include:

* Seller Short ID
* Completed Orders
* Average Review Score

#### Lowest-Rated Sellers: 1-Star, 80+ Sales

A second table identifies sellers with very low ratings while maintaining a significant completed-order volume.

This provides a useful contrast with the high-performing seller group and helps identify sellers that may require further investigation.

### Design Decision

The page combines aggregate customer-satisfaction analysis with seller-level investigation.

The upper portion provides overall satisfaction patterns, while the lower section highlights specific sellers for recognition or further investigation.

---

## 2.5 Page 5 — Seller Analysis

### Purpose

The fifth page provides a dedicated seller-selection view.

The page is intentionally simpler than the other analytical pages because its primary purpose is to support seller selection and navigation toward deeper seller analysis.

### Visible Element

* Seller ID list

The design provides a focused entry point into individual seller analysis.

---

## 2.6 Page 6 — Seller's Details

### Purpose

This page provides a detailed seller-level analytical view.

The page moves from aggregated seller performance into:

* Seller metrics.
* Product-category performance.
* Customer review information.
* Order-level delivery analysis.

### Filters / Selection Controls

* Seller ID
* Year
* Product Category
* Order Status

A back-arrow navigation control supports movement back toward the preceding seller-analysis context.

### Primary KPIs

* Average Rating
* Average Days Late
* Total Sales
* Completed Orders

These KPIs provide an immediate summary of individual seller performance.

### Main Visuals and Tables

#### Seller's Details Review Table

A review-related table provides qualitative customer feedback, including:

* Review Score
* Review Comment Title
* Review Comment Message
* Related review information

This complements numerical seller metrics with customer-level feedback.

#### Product Category Performance Table

The table includes:

* Product Category
* Completed Orders
* Average Review Score
* Average Freight Cost per Order

This allows seller performance to be investigated across product categories.

#### Delivery Speed vs SLA

A horizontal bar comparison shows actual delivery days against average estimated delivery days for individual orders.

This supports order-level investigation of delivery performance.

### Design Decision

The seller detail page follows a diagnostic structure:

> **Seller Selection → Headline Performance → Category Detail → Customer Feedback → Order-Level Delivery Analysis**

This makes the page suitable for investigating an individual seller after higher-level analysis identifies a seller of interest.

---

# 3. Visual Design

## 3.1 Overall Theme

The report uses a light visual presentation with a consistent visual language across the six pages.

Key design characteristics include:

* Light or near-white report backgrounds.
* White visual containers.
* Dark text for headings and labels.
* Blue as a prominent analytical accent.
* Additional categorical accents such as purple, pink, orange, and green where multiple categories need to be differentiated.

The visual language is kept consistent across pages so that users can focus on the analytical content rather than learning a different interface on each page.

---

## 3.2 Typography

The report uses:

* Large, prominent page titles.
* Smaller visual titles.
* Concise and descriptive chart titles.

Examples include:

* `Monthly Sales & Orders Trend`
* `Top 10 Product Categories by Revenue`
* `Total Revenue by Customer State`
* `Shipping Cost vs Product Weight`
* `Impact of Delivery Time on Reviews`
* `Delivery Speed vs SLA`

Descriptive visual titles communicate the analytical purpose without requiring additional explanation.

---

## 3.3 KPI Cards

KPI cards are used consistently across the report.

Their general structure includes:

* Large metric value.
* Clear metric label.
* Compact card container.
* Strong visual separation from surrounding charts.

The KPI cards are positioned prominently near the top or left side of analytical pages.

This establishes a consistent hierarchy:

> **What is the current performance?**
> ↓
> **Why is it changing?**
> ↓
> **Where is the performance coming from?**
> ↓
> **What requires deeper investigation?**

---

## 3.4 Chart Selection

Chart types were selected according to the analytical question being addressed.

| Analytical Question                                         | Visual Used            |
| ----------------------------------------------------------- | ---------------------- |
| How do sales and order volume change over time?             | Line / Trend Chart     |
| Which categories generate the most revenue?                 | Horizontal Bar Chart   |
| How is revenue distributed by payment method?               | Doughnut Chart         |
| Which states contribute the most revenue?                   | Treemap                |
| What are the exact category values?                         | Table                  |
| How does payment value vary by installments?                | Column Chart           |
| Is product weight related to shipping cost?                 | Scatter Plot           |
| How does delivery compare against an SLA/performance scale? | Gauge                  |
| How do actual and estimated delivery times compare?         | Horizontal Bar Chart   |
| How are review scores distributed across categories?        | 100% Stacked Bar Chart |
| Does delivery time relate to review scores?                 | Line Chart             |
| Which sellers meet specific performance criteria?           | Table                  |

The report therefore avoids relying on a single visual type and instead matches visualization methods to analytical requirements.

---

## 3.5 Tables

Tables are used where exact values or detailed records are more useful than graphical representation.

Examples include:

* Product-category sales detail.
* High-performing sellers.
* Lowest-rated sellers.
* Seller category performance.
* Seller review information.
* Order-level delivery information.

The combination of charts and tables provides both:

* Fast visual interpretation.
* Exact numerical or record-level inspection.

---

## 3.6 Slicers and Filters

Slicers are positioned consistently near the top of relevant pages.

Examples include:

* Date
* Year / Month
* Product Category
* State
* Customer State
* Review Score
* Month Name
* Response Time Range
* Seller ID
* Order Status

Filter selection changes according to the business purpose of each page rather than applying one identical filter set everywhere.

This keeps the interaction model relevant without overwhelming the user with unnecessary controls.

---

# 4. Interactivity

## 4.1 Filtering

Filtering is a core component of the report design.

Users can change the analytical context using dimensions such as:

* Time.
* Product category.
* Geographic state.
* Review score.
* Seller.
* Order status.

This allows a single report to answer multiple analytical questions without requiring separate static reports.

---

## 4.2 Seller-Level Navigation

The report contains both a **Seller Analysis** page and a **Seller's Details** page.

The Seller's Details page includes a visible back-arrow control, supporting navigation back toward the previous seller-analysis context.

The intended analytical workflow is:

> **Seller List → Selected Seller → Seller Details**

The exact underlying Power BI interaction mechanism should match the actual `.pbix` configuration.

---

## 4.3 Drill-Down

Hierarchical analysis is used within the report where applicable.

Any documented drill-down hierarchy should correspond directly to the hierarchy configured in Power BI Desktop.

---

## 4.4 Drillthrough

The report structure supports detailed investigation of seller performance through the Seller Analysis and Seller's Details pages.

Where the `.pbix` contains an explicit drillthrough configuration, the implementation should be documented using the actual drillthrough field and destination page.

---

## 4.5 Tooltips

Tooltips provide additional context where configured.

Examples may include compact seller identifiers or additional supporting values that would otherwise create unnecessary visual clutter.

Any custom tooltip page should be documented according to the actual configuration in the `.pbix`.

---

## 4.6 Bookmarks

Bookmarks can be used to provide controlled navigation, alternate report states, or focused analytical views where implemented.

The final documentation should use the actual bookmark names and purposes configured in Power BI Desktop.

---

# 5. Accessibility & Usability

The report includes several usability-oriented design choices.

## 5.1 Clear Page Titles

Each page uses a descriptive title that communicates the business purpose of the page.

---

## 5.2 Consistent Layout

The report maintains a consistent structural pattern:

* Page title area.
* Filter area.
* KPI area.
* Main analytical visuals.
* Detailed tables where appropriate.

This consistency reduces the amount of time users need to spend learning the interface.

---

## 5.3 Visual Hierarchy

Large KPI values attract attention first, followed by visual titles and analytical charts.

This allows the report to support both quick executive review and deeper analytical investigation.

---

## 5.4 Color-Based Category Differentiation

Different colors are used to distinguish categories, payment methods, review scores, and other groups where appropriate.

For review analysis, the five rating levels are separated visually to support comparison of customer-rating distributions.

---

## 5.5 Readability

White visual containers against the light report background provide separation between analytical components.

Long category names and seller identifiers are supported through horizontal charts and tables where appropriate.

---

## 5.6 Accessibility Considerations

Accessibility should be reviewed in Power BI Desktop with particular attention to:

* Alt text.
* Tab order.
* Keyboard navigation.
* Color contrast.
* High-contrast requirements.
* Screen-reader compatibility.

These items form part of the usability and accessibility considerations for future refinement.

---

# 6. Design Decisions

## 6.1 Use KPI Cards for Immediate Performance Visibility

Important business measures are surfaced through KPI cards so users can understand current performance before analyzing individual charts.

---

## 6.2 Organize the Report by Business Function

Rather than placing all metrics on one page, the report separates analysis into:

* Sales.
* Geography.
* Logistics.
* Customer satisfaction.
* Seller performance.
* Seller detail.

This reduces information overload and creates a logical analytical journey.

---

## 6.3 Combine Summary and Detail

Charts provide rapid pattern recognition, while tables provide exact values and detailed records.

This is particularly useful for seller and product-category analysis.

---

## 6.4 Use Horizontal Bars for Long Category Names

Product categories and seller identifiers can be difficult to read in narrow vertical layouts.

Horizontal bar charts and tables provide additional space for these labels.

---

## 6.5 Use Dedicated Filters by Analytical Context

Filter selections are adapted to the purpose of each page.

Examples include:

* Sales pages emphasize date, product category, and state.
* Logistics emphasizes date, product category, and customer state.
* Customer satisfaction emphasizes review score, month, and response time.
* Seller detail emphasizes seller, year, category, and order status.

This keeps filtering relevant instead of overwhelming the user with unnecessary controls.

---

## 6.6 Progress from Overview to Detail

The report follows a natural analytical workflow:

> **Business Overview → Sales & Geography → Logistics → Customer Satisfaction → Seller Analysis → Seller Details**

This supports both high-level monitoring and deeper root-cause investigation.

---

# 7. Page-Level Design Summary

| Page                                  | Primary Purpose                             | Main Design Pattern                                       |
| ------------------------------------- | ------------------------------------------- | --------------------------------------------------------- |
| E-Commerce Sales Overview             | Executive sales and business overview       | KPI + Trend + Ranking + Payment Mix                       |
| Sales & Geographic Performance        | Sales distribution and transaction analysis | KPI + Geography + Table + Installment Analysis            |
| Logistics & Supply Chain Operations   | Delivery and shipping performance           | KPI + Scatter + Gauge + Comparison                        |
| Customer Satisfaction & Seller Rating | Reviews and seller quality                  | KPI + Rating Distribution + Trend + Seller Tables         |
| Seller Analysis                       | Seller selection                            | Seller List                                               |
| Seller's Details                      | Individual seller investigation             | KPI + Category Detail + Review Detail + Delivery Analysis |

---

# 8. Design Strengths

The current report design provides several strengths.

### 8.1 Strong Business-Oriented Page Organization

Each page has a clear analytical purpose rather than combining unrelated metrics.

### 8.2 Consistent KPI Usage

Important measures are surfaced prominently so users can understand performance quickly.

### 8.3 Combination of Visual and Tabular Analysis

Charts provide pattern recognition while tables provide exact values and detailed records.

### 8.4 Progressive Detail

The report moves from high-level business performance toward individual seller and order analysis.

### 8.5 Context-Specific Filtering

Filters are selected according to the analytical purpose of each page.

### 8.6 Variety of Visual Types

Line charts, bar charts, treemaps, scatter plots, gauges, doughnut charts, and tables are used according to different analytical requirements.

### 8.7 Seller Investigation Workflow

The Seller Analysis and Seller's Details pages provide a logical progression for deeper seller investigation.

---

# 9. Design Limitations & Future Improvements

Potential areas for future refinement include:

* Verify and document the exact Power BI theme, font, and color palette.
* Confirm the precise drillthrough configuration and drillthrough fields.
* Document implemented bookmarks and their purposes.
* Document custom tooltip pages where applicable.
* Review visual spacing and label density on pages containing many categories or long identifiers.
* Consider focused Top-N views where dense visuals become difficult to interpret.
* Review accessibility settings such as alt text, tab order, and color contrast.
* Add explanatory subtitles or annotations where complex visuals could benefit from additional context.

These points represent opportunities for refinement rather than claims that the existing report is incorrect.

---

# 10. Final Design Summary

The report uses a structured, business-focused Power BI design that moves from overall e-commerce performance toward increasingly detailed operational and seller-level analysis.

The primary interaction pattern is:

> **Filter → KPI → Visual Analysis → Detailed Investigation**

The six-page structure provides a logical progression from:

> **Sales Performance → Geographic Performance → Logistics → Customer Satisfaction → Seller Analysis → Seller Details**

This design provides both high-level business visibility and the ability to investigate operational drivers at increasingly granular levels.

The report design therefore serves as the presentation layer of the Power BI solution, building on the cleaned dataset, semantic model, and DAX calculation layer documented elsewhere in the project.


## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

