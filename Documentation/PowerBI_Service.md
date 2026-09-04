# Power BI Service — Deployment & Administration

## 1. Overview

The Power BI Service layer was used to deploy, manage, refresh, secure, monitor, and distribute the completed e-commerce analytics solution.

The implementation extended the Power BI Desktop report into a complete BI workflow covering:

* Workspace management
* Semantic model configuration
* On-premises data gateway connectivity
* Data source credentials
* Scheduled refresh
* Semantic model endorsement and discovery
* Dashboard creation
* Dashboard subscriptions
* Data alerts
* Mobile layout
* Power BI App deployment
* Access management
* Row-Level Security (RLS)

The objective was to demonstrate practical experience beyond report development by implementing the operational and distribution capabilities available through Power BI Service.

---

# 2. Workspace Configuration

A dedicated Power BI workspace was created for the e-commerce analytics solution.

**Workspace:** `Retail E-commerce Analytics`

The workspace acts as the central environment for managing the project's:

* Semantic model
* Report
* Dashboard
* App
* Refresh configuration
* Security settings
* Distribution settings

Using a dedicated workspace provides a structured environment for managing the complete BI solution.

---

# 3. Semantic Model & Gateway Configuration

After publishing the report from Power BI Desktop, the semantic model was configured through Power BI Service.

The semantic model acts as the central analytical data layer connecting the published report to the underlying CSV data sources.

An **On-premises Data Gateway** was configured to provide connectivity between Power BI Service and the local CSV files used by the project.

The gateway enables Power BI Service to access the configured data sources when performing scheduled refresh operations.

The configuration included:

* Gateway connection
* Data source mapping
* Data source authentication
* Semantic model connection

This allowed the published solution to remain connected to its underlying data sources after deployment.

---

# 4. Data Source Credentials

Individual credentials and privacy levels were configured for the CSV data sources used by the semantic model.

| Data Source                   | Privacy Level  |
| ----------------------------- | -------------- |
| Customers                     | Private        |
| Geolocation                   | Organizational |
| Order Items                   | Organizational |
| Order Payments                | Private        |
| Order Reviews                 | Private        |
| Orders                        | Organizational |
| Product Category Name English | Public         |
| Products                      | Organizational |
| Sellers                       | Private        |

The privacy levels were configured according to the intended classification of each source.

This configuration supports controlled access to the underlying data sources during Power BI Service refresh operations.

---

# 5. Scheduled Refresh

A scheduled refresh was configured for the semantic model.

### Refresh Configuration

| Setting   | Configuration              |
| --------- | -------------------------- |
| Frequency | Weekly                     |
| Day       | Sunday                     |
| Time      | 6:30 AM                    |
| Time Zone | UTC+6:30 (Myanmar/Rangoon) |

The scheduled refresh allows Power BI Service to retrieve updated data from the configured sources without requiring the report to be manually republished from Power BI Desktop.

This demonstrates the transition from a static desktop report into a maintainable reporting solution.

---

# 6. Semantic Model Endorsement & Discovery

The semantic model was configured for broader discovery and reuse within the organization.

The model was **promoted/endorsed** as a trusted analytical asset so that other users can more easily identify it as a recommended source for relevant analysis.

Discovery settings were also enabled to improve the visibility and accessibility of the published analytical content.

This supports the reuse of the semantic model as a centralized analytical layer rather than treating it only as a backend for a single report.

---

# 7. Dashboard & Tiles

A Power BI Service dashboard was created using selected visuals from the published report.

The dashboard was designed as a concise business-monitoring layer rather than reproducing every visual available in the full report.

### Executive KPI Tiles

The main KPI tiles include:

* Total Sales
* Total Units Sold
* Average Order Value
* Average Review
* Punctuality KPI

### Analytical Tiles

Additional dashboard tiles include:

* Monthly Sales & Order Trend
* Top 10 Product Categories by Revenue
* Estimated vs. Actual Delivery Performance by State
* Impact of Delivery Time on Reviews

The dashboard provides a high-level view of business performance while the underlying report remains available for detailed exploration.

---

# 8. Mobile Layout

A mobile-optimized layout was created for the dashboard.

The mobile layout rearranges the selected dashboard content to improve usability on smaller screens.

This ensures that the solution is not limited to desktop consumption and demonstrates consideration for multiple reporting environments.

The solution therefore supports:

* Desktop dashboard consumption
* Mobile dashboard consumption

---

# 9. Subscriptions & Data Alerts

Power BI Service subscriptions and data alerts were configured to support recurring monitoring of important business metrics.

## Dashboard Subscription

A recurring subscription was configured to provide users with scheduled access to the dashboard.

This reduces the need for users to manually open the Power BI dashboard to check for updates.

## Data Alerts

Two KPI-based alerts were configured.

### Total Sales Alert

An alert was configured for the Total Sales KPI when the value increases above:

**120K**

### Review Alert

An alert was configured for the Average Review KPI when the value falls to:

**3**

These alerts provide automated monitoring of important business conditions.

The sales alert identifies significant sales performance, while the review alert helps identify a potential customer-satisfaction issue.

---

# 10. Power BI App & Access Management

A Power BI App was created to package and distribute the completed analytical solution to intended users.

The App provides a controlled consumption layer over the workspace content.

The application was shared with selected organizational users rather than being made broadly accessible.

The distribution structure can therefore be represented as:

```text
Power BI Workspace
       ↓
   Power BI App
       ↓
Authorized Users
```

This separates the development and management environment from the environment used by report consumers.

---

# 11. Row-Level Security (RLS)

Row-Level Security was implemented to restrict the data visible to individual users.

The security scenario was designed around **seller-level access**.

Each authorized seller user is associated with a seller identity, allowing Power BI to filter the report according to the seller assigned to that user.

### RLS Access Flow

```text
User Email
     ↓
Seller Access Mapping
     ↓
Seller ID
     ↓
Related Sales / Order Data
     ↓
Filtered Report
```

For example, when a seller accesses the report, the intended result is that the seller can view only the relevant sales, orders, products, reviews, and operational information associated with that seller.

User identities were configured using email addresses to support identity-based filtering.

This demonstrates how Power BI Service can be used as a governed reporting environment with user-specific data visibility.

---

# 12. End-to-End Service Architecture

The Power BI Service implementation can be summarized as:

```text
CSV Data Sources
       ↓
On-Premises Data Gateway
       ↓
Data Source Credentials
       ↓
Semantic Model
       ↓
Scheduled Refresh
       ↓
Power BI Report
       ↓
Power BI Dashboard
       ↓
 ┌───────────────┬────────────────┬─────────────────┐
 ↓               ↓                ↓
Alerts       Subscriptions     Mobile Layout
                                     
       ↓
Power BI App
       ↓
Authorized Users
       ↓
Row-Level Security
       ↓
Seller-Specific Data Access
```

This workflow demonstrates the progression from raw source data to a deployed, monitored, distributed, and secured BI solution.

---

# 13. Service-Level Validation

The Power BI Service implementation was validated across the major deployment and administration components.

### Validation Checklist

* [x] Dedicated workspace created
* [x] Semantic model published
* [x] On-premises data gateway configured
* [x] Data-source credentials configured
* [x] Scheduled refresh configured
* [x] Semantic model promoted/endorsed
* [x] Discovery enabled
* [x] Dashboard created
* [x] KPI tiles configured
* [x] Analytical dashboard tiles configured
* [x] Mobile layout created
* [x] Dashboard subscription configured
* [x] KPI alerts configured
* [x] Power BI App created
* [x] App access restricted to intended users
* [x] Row-Level Security configured
* [x] Seller-specific access tested

---

# 14. Limitations & Future Improvements

The current solution uses CSV files as its underlying data sources.

For a larger production environment, the architecture could be further developed by moving the source data into a centralized database, data warehouse, or cloud-based storage platform.

Potential future improvements include:

* Migration from CSV sources to a centralized SQL or cloud data source
* More granular RLS roles
* Additional user and role management
* Automated deployment pipelines
* Source-control integration
* Advanced refresh monitoring
* Additional KPI alerts
* Automated CI/CD workflows
* More extensive Power BI governance

These improvements would allow the solution to scale toward a larger enterprise BI environment.

---

# 15. Final Outcome

The Power BI Service implementation transformed the Power BI Desktop report into a deployable and managed business intelligence solution.

The final workflow covers:

**Data Connectivity → Semantic Model → Automated Refresh → Report → Dashboard → Monitoring → Distribution → Security**

Through this implementation, the project demonstrates practical experience with:

* Power BI workspace administration
* Semantic model management
* On-premises data gateway
* Data-source credentials
* Scheduled refresh
* Semantic model endorsement
* Dashboard development
* KPI monitoring
* Data alerts
* Subscriptions
* Mobile layouts
* Power BI Apps
* Access management
* Row-Level Security

The Service layer therefore completes the end-to-end Power BI workflow, extending the project beyond visualization into deployment, operational management, monitoring, distribution, and data governance.

## Author
[Pyae Khant Kyaw](https://www.linkedin.com/in/pyae-khant-kyaw-591726390/)

## Source Data
[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
