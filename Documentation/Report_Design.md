# Report Design

## 1. Design Philosophy

The report is designed as a multi-page e-commerce analytics dashboard
that moves from **high-level business performance** to **sales and
geographic analysis**, then to **logistics**, **customer satisfaction**,
and finally **seller-level detail**.

The overall design prioritizes:

-   Clear KPI-driven summaries
-   Consistent page structure
-   Business-focused analytical questions
-   Interactive filtering
-   A balance between summary visuals and detailed tables
-   Progressive navigation from overview to detailed seller analysis
-   Readability through a light report background, white visual
    containers, and blue/purple visual accents

The report contains six pages:

1.  E-Commerce Sales Overview
2.  Sales & Geographic Performance
3.  Logistics & Supply Chain Operations
4.  Customer Satisfaction & Seller Rating
5.  Seller Analysis
6.  Seller's Details

The design is intended to allow a user to start with overall business
performance and progressively investigate products, locations, delivery
performance, customer reviews, and individual sellers.

------------------------------------------------------------------------

## 2. Report Structure

### Page 1 --- E-Commerce Sales Overview

**Purpose**

The first page acts as the executive-level overview of the e-commerce
business. It provides a concise view of sales, revenue, order value,
customer reviews, delivery punctuality, product-category performance,
and payment-method distribution.

**Primary KPIs**

The page places key performance indicators near the top of the report:

-   Total Sales
-   Total Revenue
-   Average Order Value
-   Average Review
-   Punctuality %

The KPI cards make the most important metrics immediately visible before
the user moves into detailed visuals.

**Filters**

The page provides filters for:

-   Date
-   Year / Month
-   Product Category

These filters allow the user to change the reporting context without
leaving the overview page.

**Main Visuals**

#### Monthly Sales & Orders Trend

A combined trend visual compares monetary sales value with order volume
over time.

This visual is useful for identifying:

-   Changes in sales activity
-   Changes in order volume
-   Periods of increasing or decreasing performance
-   The relationship between sales value and order volume

#### Top 10 Product Categories by Revenue

A horizontal bar chart ranks the highest-revenue product categories.

The horizontal orientation is appropriate because product-category names
can be relatively long and are easier to read along the vertical axis.

#### Total Revenue by Payment Type

A doughnut chart presents the distribution of revenue across payment
methods.

This provides a quick view of payment-method concentration and makes the
dominant payment method immediately visible.

**Design Decision**

The page follows an executive-dashboard structure:

> Filters → KPIs → trend → category ranking → payment distribution

This establishes a clear visual hierarchy from headline metrics to
supporting analysis.

------------------------------------------------------------------------

### Page 2 --- Sales & Geographic Performance

**Purpose**

This page focuses on sales volume, transaction activity, geographic
performance, product-category performance, shipping cost, and
installment behavior.

**Primary KPIs**

The KPI section contains:

-   Unit Sold
-   Total Payments
-   Average Shipping Cost
-   Average Installments
-   Average Transaction Value

These metrics shift the analytical focus from the overall revenue
summary toward operational sales activity.

**Filters**

The page includes:

-   Date
-   Product Category
-   State

The geographic filter supports more focused regional analysis.

**Main Visuals**

#### Total Revenue by Customer State

A treemap is used to compare revenue contribution across customer
states.

The relative area of each state makes large revenue contributors
visually prominent.

This is useful for quickly identifying geographically important markets.

#### Product Category Detail Table

A table provides:

-   Product Category
-   Total Units Sold
-   Total Revenue
-   Average Price

The table complements the visual analysis by providing exact values
rather than relying only on graphical comparisons.

#### Payment Value by Installment Method

A column chart compares payment value across installment counts.

The visual makes the concentration of payment value among lower
installment counts easy to observe.

**Design Decision**

This page intentionally combines:

-   A visual geographic overview
-   Exact product-level values
-   Transaction/payment analysis

This gives the user both visual comparison and detailed numerical
information.

------------------------------------------------------------------------

### Page 3 --- Logistics & Supply Chain Operations

**Purpose**

This page changes the analytical perspective from sales to fulfillment
and logistics performance.

The page focuses on:

-   Delivery time
-   On-time delivery
-   Delivery performance against an SLA goal
-   Actual versus estimated delivery time
-   Shipping cost and product weight

**Filters**

The page provides:

-   Date
-   Product Category
-   Customer State

These filters allow logistics performance to be investigated by time,
product category, and location.

**Primary KPIs**

The page highlights:

-   Actual Delivery Days
-   Average Delivery Time
-   On-Time Delivery Rate
-   Average Days Late

The KPI arrangement gives delivery performance immediate visibility.

**Main Visuals**

#### Shipping Cost vs Product Weight

A scatter plot compares product weight with shipping cost.

The visual is appropriate for exploring whether heavier products are
associated with higher shipping costs and for identifying the
distribution of observations.

A trend line is also visible, providing an additional indication of the
overall relationship.

#### Delivery Performance Against SLA Goal

A gauge visual presents the delivery performance value against a
reference scale.

This gives the user a compact way to assess the reported
delivery-performance level.

#### Estimated vs Actual Delivery Days by State

A clustered horizontal bar chart compares:

-   Actual Delivery Days
-   Estimated Delivery Days

by customer state.

The side-by-side comparison makes differences between expected and
actual delivery performance easier to investigate.

**Design Decision**

The page uses different visual forms for different analytical questions:

-   KPI cards for headline logistics metrics
-   Scatter plot for relationship analysis
-   Gauge for performance against a target/scale
-   Horizontal bars for actual-versus-estimated comparison

------------------------------------------------------------------------

### Page 4 --- Customer Satisfaction & Seller Rating

**Purpose**

This page connects customer review performance with product categories,
delivery time, and seller performance.

**Filters**

The page provides:

-   Review Score
-   Month Name
-   Response Time Range

These filters allow customer satisfaction results to be explored from
multiple perspectives.

**Primary KPIs**

The page highlights:

-   Total Reviews
-   Average Review
-   5-Star Rating %
-   1-Star Rating %

The use of both positive and negative rating indicators helps provide a
balanced view of customer satisfaction.

**Main Visuals**

#### Review of Top 10 Category by Revenue

A 100% stacked horizontal bar chart displays the distribution of review
scores across selected high-revenue product categories.

The five rating levels are shown separately:

-   1 Star
-   2 Star
-   3 Star
-   4 Star
-   5 Star

Using a 100% stacked structure makes the composition of reviews easier
to compare between categories.

#### Impact of Delivery Time on Reviews

A line chart examines review scores across different delivery times.

This visual supports investigation into whether customer ratings vary
with the number of days required for delivery.

#### Sellers with Perfect Ratings & 80+ Sales

A table identifies sellers meeting the high-rating and sales-volume
criteria.

The table displays:

-   Seller Short ID
-   Completed Orders
-   Average Review Score

#### Lowest-Rated Sellers: 1-Star, 80+ Sales

A second table identifies sellers with very low ratings while
maintaining a significant completed-order volume.

This creates a useful contrast with the high-performing seller table.

**Design Decision**

The page combines customer-level and seller-level analysis. The top
section provides aggregate customer satisfaction information, while the
lower section highlights specific sellers requiring attention or
recognition.

------------------------------------------------------------------------

### Page 5 --- Seller Analysis

**Purpose**

The fifth page provides a seller-selection view.

The page presents seller IDs in a dedicated list, allowing the user to
identify/select a seller for deeper investigation.

The design is intentionally simple compared with the other analytical
pages because its role is primarily seller navigation/selection rather
than broad KPI analysis.

**Visible Element**

-   Seller ID list

The PDF export does not expose the complete interaction configuration
behind this page. Therefore, details such as the exact selection
behavior, visual-level interactions, or whether the page is used as a
navigation step cannot be confirmed from the PDF alone.

------------------------------------------------------------------------

### Page 6 --- Seller's Details

**Purpose**

This page provides a detailed seller-level view after a seller has been
selected.

The page moves from aggregated seller performance into individual seller
metrics, product-category performance, customer review information, and
order-level delivery analysis.

**Filters / Selection Controls**

The page contains:

-   Seller ID
-   Year
-   Product Category
-   Order Status

A back-arrow navigation control is also visible, supporting movement
back to the preceding report context.

**Primary KPIs**

The seller detail page highlights:

-   Average Rating
-   Average Days Late
-   Total Sales
-   Completed Orders

These KPIs provide an immediate seller-performance summary.

**Main Visuals and Tables**

#### Seller's Details Review Table

A review-related table displays information including:

-   Review score
-   Review comment title
-   Review comment message
-   Related review information

This provides qualitative detail in addition to numerical seller
metrics.

#### Product Category Performance Table

The table includes:

-   Product category
-   Completed orders
-   Average review score
-   Average freight cost per order

This allows the seller's performance to be investigated by product
category.

#### Delivery Speed vs SLA

A horizontal bar comparison shows actual delivery days against average
estimated delivery days for individual orders.

This supports order-level investigation of delivery performance.

**Design Decision**

The seller detail page follows a diagnostic layout:

> Seller selection → headline performance → category detail → customer
> feedback → order-level delivery analysis

This makes the page appropriate for investigating an individual seller
after a higher-level analysis has identified a seller of interest.

------------------------------------------------------------------------

## 3. Visual Design

### 3.1 Overall Theme

The report uses a light visual presentation.

Across the pages, the design consistently uses:

-   Light/near-white report backgrounds
-   White visual containers
-   Dark text for headings and labels
-   Blue as a prominent analytical accent
-   Additional purple, pink, orange, green, and other categorical
    accents where multiple categories need to be distinguished

The PDF export demonstrates a consistent visual language across the six
pages.

> Note: The exact Power BI theme JSON, font family, and color hex codes
> cannot be confirmed from the PDF export alone.

------------------------------------------------------------------------

### 3.2 Typography

The report uses large, prominent page titles and smaller visual titles
underneath.

The page title is visually separated from the analytical content, making
the purpose of each page immediately clear.

Visual titles are concise and descriptive, for example:

-   "Monthly Sales & Orders Trend"
-   "Top 10 Product Categories by Revenue"
-   "Total Revenue by Customer State"
-   "Shipping Cost vs Product Weight"
-   "Impact of Delivery Time on Reviews"
-   "Delivery Speed vs SLA"

This naming approach helps the user understand the analytical purpose of
each visual without needing additional explanation.

The exact font family cannot be confirmed from the PDF export.

------------------------------------------------------------------------

### 3.3 KPI Cards

KPI cards are used consistently across the report.

Their general design includes:

-   Large metric value
-   Clear metric label
-   Compact card container
-   Strong visual separation from surrounding charts

The KPI cards are positioned prominently near the top or left side of
analytical pages.

This establishes a consistent hierarchy:

1.  What is the current performance?
2.  Why is it changing?
3.  Where is the performance coming from?
4.  What requires deeper investigation?

------------------------------------------------------------------------

### 3.4 Charts

Chart selection is generally aligned with the analytical question.

  -----------------------------------------------------------------------
  Analytical Question                 Visual Used
  ----------------------------------- -----------------------------------
  How do sales and order volume       Line/trend chart
  change over time?                   

  Which categories generate the most  Horizontal bar chart
  revenue?                            

  How is revenue distributed by       Doughnut chart
  payment method?                     

  Which states contribute the most    Treemap
  revenue?                            

  What are the exact category values? Table

  How does payment value vary by      Column chart
  installments?                       

  Is product weight related to        Scatter plot
  shipping cost?                      

  How does delivery compare with an   Gauge
  SLA/performance scale?              

  How do actual and estimated         Horizontal bar chart
  delivery times compare?             

  How are review scores distributed   100% stacked bar chart
  across categories?                  

  Does delivery time relate to review Line chart
  scores?                             

  Which sellers meet specific         Table
  performance criteria?               

  How does a seller perform by        Table and horizontal bar chart
  category/order?                     
  -----------------------------------------------------------------------

The report therefore avoids relying on a single visual type and instead
uses different visual forms according to the analytical requirement.

------------------------------------------------------------------------

### 3.5 Tables

Tables are used where exact values and detailed records are more useful
than graphical representation.

Examples include:

-   Product-category sales detail
-   High-performing sellers
-   Lowest-rated sellers
-   Seller category performance
-   Seller review information
-   Order-level delivery information

The combination of charts and tables provides both:

-   Fast visual interpretation
-   Exact numerical/detail-level inspection

------------------------------------------------------------------------

### 3.6 Slicers and Filters

Slicers are positioned consistently near the top of the relevant pages.

Examples include:

-   Date
-   Year / Month
-   Product Category
-   State
-   Customer State
-   Review Score
-   Month Name
-   Response Time Range
-   Seller ID
-   Order Status

This placement keeps the filtering controls visible while preserving the
main analytical area for KPIs and visuals.

The slicer choices also change according to the business purpose of each
page rather than using the same filter set everywhere.

------------------------------------------------------------------------

## 4. Interactivity

The PDF export demonstrates the presence of multiple interactive report
elements, particularly slicers and seller selection/detail navigation.

### 4.1 Filtering

Filtering is a major part of the report design.

Users can filter the analysis by dimensions such as:

-   Time
-   Product category
-   Geographic state
-   Review score
-   Seller
-   Order status

The filters allow the same report pages to answer multiple analytical
questions without requiring separate static reports.

------------------------------------------------------------------------

### 4.2 Seller-Level Navigation

The report contains a dedicated Seller Analysis page and a Seller's
Details page.

The Seller's Details page contains a visible back-arrow control,
indicating a navigation path back toward the previous report context.

The design supports a progression from:

**Seller list → selected seller → seller details**

The exact Power BI interaction configuration should be verified in the
`.pbix` file before documenting it as a specific drillthrough or
bookmark implementation.

------------------------------------------------------------------------

### 4.3 Drill-Down

The PDF shows hierarchical fields and detailed visual analysis, but a
static PDF cannot reliably prove the exact drill-down configuration used
in the Power BI file.

Therefore, drill-down should be documented here only after confirming
the hierarchy and interaction settings in the `.pbix` file.

------------------------------------------------------------------------

### 4.4 Drillthrough

The report structure strongly supports a detailed seller-analysis
workflow, particularly through the Seller Analysis and Seller's Details
pages.

However, the PDF alone does not expose the page-level drillthrough
configuration.

If the `.pbix` confirms that Seller's Details is configured as a
drillthrough page, this section can be updated to state the exact
drillthrough field and workflow.

------------------------------------------------------------------------

### 4.5 Tooltips

Tooltips are not independently visible in the static PDF export.

Their configuration should therefore be confirmed in the `.pbix` before
being documented as an implemented feature.

------------------------------------------------------------------------

### 4.6 Bookmarks

Bookmarks cannot be reliably identified from the PDF export.

If bookmarks are used in the Power BI report, document the specific
bookmark names and their purpose after checking the `.pbix` file.

------------------------------------------------------------------------

## 5. Accessibility & Usability

The report design includes several usability-oriented choices visible in
the exported report.

### Clear Page Titles

Each page has a descriptive title that communicates the page's business
purpose.

### Consistent Layout

The report maintains a consistent visual structure across pages:

-   Page title area
-   Filter area
-   KPI area
-   Main analytical visuals
-   Detailed tables where required

This consistency reduces the amount of time users need to spend learning
the interface.

### Visual Hierarchy

Large KPI values attract attention first, followed by chart titles and
analytical visuals.

This makes the report suitable for both quick executive review and
deeper analysis.

### Color-Based Category Differentiation

Several visuals use different colors to distinguish categories, payment
methods, review scores, or other groups.

For review analysis, the five rating levels are explicitly separated,
helping users compare the distribution of customer ratings.

### Readability

The use of white visual containers against the light page background
creates separation between individual analytical components.

Long category and seller labels are supported by horizontal charts and
tables where appropriate.

### Potential Accessibility Improvements

The PDF alone cannot confirm settings such as:

-   Alt text
-   Tab order
-   High-contrast configuration
-   Screen-reader optimization
-   Exact color-contrast ratios

These should be checked directly in Power BI Desktop if accessibility is
being formally documented.

------------------------------------------------------------------------

## 6. Design Decisions

### Decision 1 --- Use KPI Cards for Immediate Performance Visibility

Important measures are surfaced through KPI cards so users can
understand overall performance before analyzing individual charts.

### Decision 2 --- Organize the Report by Business Function

Rather than placing all metrics on one page, the report separates
analysis into:

-   Sales
-   Geography
-   Logistics
-   Customer satisfaction
-   Seller performance
-   Seller detail

This reduces information overload and creates a logical analytical
journey.

### Decision 3 --- Combine Summary and Detail

Charts provide rapid pattern recognition, while tables provide exact
values and detailed records.

This is particularly useful for seller and product-category analysis.

### Decision 4 --- Use Horizontal Bars for Long Category Names

Product categories and seller identifiers can be difficult to read in
narrow vertical layouts.

Horizontal bars and tables provide more space for these labels.

### Decision 5 --- Use Dedicated Filters by Analytical Context

The filters change according to the purpose of each page.

For example:

-   Sales pages emphasize date, product category, and state.
-   Logistics emphasizes date, product category, and customer state.
-   Customer satisfaction emphasizes review score, month, and response
    time.
-   Seller detail emphasizes seller, year, category, and order status.

This keeps filtering relevant instead of overwhelming the user with
unnecessary controls.

### Decision 6 --- Progress from Overview to Detail

The report follows a natural analytical workflow:

**Business overview → sales/geography → logistics → customer
satisfaction → seller analysis → seller details**

This supports both high-level monitoring and root-cause investigation.

------------------------------------------------------------------------

## 7. Page-Level Design Summary

  -----------------------------------------------------------------------
  Page                    Primary Purpose         Main Design Pattern
  ----------------------- ----------------------- -----------------------
  E-Commerce Sales        Executive sales and     KPI + trend + ranking +
  Overview                business overview       payment mix

  Sales & Geographic      Sales distribution and  KPI + geography +
  Performance             transaction analysis    table + installment
                                                  analysis

  Logistics & Supply      Delivery and shipping   KPI + scatter + gauge +
  Chain Operations        performance             comparison

  Customer Satisfaction & Reviews and seller      KPI + rating
  Seller Rating           quality                 distribution + trend +
                                                  seller tables

  Seller Analysis         Seller selection        Seller list

  Seller's Details        Individual seller       KPI + review detail +
                          investigation           category table + order
                                                  delivery
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 8. Design Strengths

The current report design has several strengths:

1.  **Strong business-oriented page organization**\
    Each page has a clear analytical purpose.

2.  **Consistent KPI usage**\
    Important metrics are surfaced prominently.

3.  **Good combination of visual and tabular analysis**\
    Users can see trends and comparisons while still accessing exact
    values.

4.  **Progressive detail**\
    The report moves from high-level performance to individual
    seller/order analysis.

5.  **Context-specific filtering**\
    Filters are relevant to the purpose of each page.

6.  **Variety of visual types**\
    Line charts, bar charts, treemaps, scatter plots, gauges, doughnut
    charts, and tables are used for different analytical questions.

7.  **Seller investigation workflow**\
    The seller analysis/detail structure supports deeper investigation
    of individual seller performance.

------------------------------------------------------------------------

## 9. Design Limitations / Future Improvements

The following points are potential areas for future refinement:

-   Verify and document the exact Power BI theme and color palette from
    the `.pbix`.
-   Confirm whether drillthrough is configured and document the exact
    drillthrough field.
-   Document any implemented bookmarks after checking the `.pbix`.
-   Document custom tooltip pages if they exist.
-   Review visual spacing and label density on pages containing many
    categories or long identifiers.
-   Consider replacing or supplementing visuals that require scrolling
    with more focused Top-N views where appropriate.
-   Verify accessibility settings such as alt text, tab order, and color
    contrast.
-   Where appropriate, add explanatory subtitles or short annotations to
    help non-technical users interpret complex visuals.

These are improvement/documentation considerations rather than claims
that the existing report is incorrect.

------------------------------------------------------------------------

## 10. Summary

The report uses a structured, business-focused Power BI design that
moves from overall e-commerce performance toward increasingly detailed
operational and seller-level analysis.

Its main design pattern is:

> **Filter → KPI → Visual Analysis → Detailed Investigation**

The six-page structure provides a logical progression from sales
performance to geographic performance, logistics, customer satisfaction,
seller analysis, and individual seller details.

## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

