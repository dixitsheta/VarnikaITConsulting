---
title: "SAP Embedded Analytics (AFO) Services"
description: "Expert S/4HANA embedded analytics, CDS view development, Virtual Data Model implementation, and real-time operational reporting consulting."
date: 2025-11-24
service_category: "Embedded Analytics"
---

# SAP Embedded Analytics (AFO)

## Real-Time Analytics Embedded in S/4HANA

SAP's Analytics Foundation Object (AFO) and embedded analytics capabilities bring powerful real-time reporting directly into S/4HANA business processes. We help you leverage CDS views, Virtual Data Models, and embedded analytics to enable instant insights without data replication.

---

## What is Embedded Analytics?

Embedded analytics in S/4HANA provides:
- **Real-time data access** - No data replication, always current
- **Operational reporting** - Analytics embedded in transactional processes
- **Predefined content** - 10,000+ delivered CDS views and analytics
- **Extensibility** - Build custom CDS views and analytics apps
- **HANA power** - Leverage in-memory computing for speed

---

## Our Embedded Analytics Services

### CDS View Development

Create custom Core Data Services views for analytics:

{{< checkmark-list >}}
- Requirements analysis for custom analytics
- CDS view design (basic, composite, consumption views)
- Annotations for analytical capabilities
- Performance optimization
- Authorization and security
- Integration with SAP Analytics Cloud
- Fiori app integration
- Documentation and knowledge transfer
{{< /checkmark-list >}}

---

### Virtual Data Model (VDM) Implementation

Leverage S/4HANA's Virtual Data Model:

**VDM Layers:**
- **Basic Views** - Direct access to tables/structures
- **Composite Views** - Business logic and calculations
- **Consumption Views** - Ready-to-consume analytics queries

**Our Services:**
- Understand and navigate VDM structure
- Extend delivered VDM content
- Build custom VDM extensions
- Create reusable VDM components
- Optimize VDM query performance

---

### Embedded Analytics Implementation

Deploy real-time analytics in S/4HANA:

#### Fiori Analytics Apps
- Configure delivered analytical apps
- Create custom analytical apps
- KPI tile configuration
- Smart business services
- Drill-down reports

#### Query & Report Development
- SAP Query on CDS views
- Custom analytical queries
- ALV Grid reports enhanced with CDS
- Multidimensional reporting

#### Dashboard Integration
- Embed analytics in Fiori Launchpad
- KPI monitoring dashboards
- Real-time alerts and notifications
- Mobile analytics access

---

### S/4HANA to SAC Integration

Connect embedded analytics to SAP Analytics Cloud:

{{< checkmark-list >}}
- Live SAC connections to S/4HANA
- CDS view exposure to SAC
- Real-time dashboard development
- Planning write-back to S/4HANA
- Blended scenarios (BW + S/4HANA)
{{< /checkmark-list >}}

---

## CDS View Development Process

### 1. Requirements & Design
- Identify analytics use case
- Determine required fields and calculations
- Design view hierarchy (basic → composite → consumption)
- Plan authorizations and security

### 2. Development
- Create basic CDS views on tables
- Build composite views with joins and calculations
- Add consumption views with analytical annotations
- Implement currency/unit conversions
- Add aggregations and measures

### 3. Annotations & Metadata
```abap
@Analytics.dataCategory: #CUBE
@Analytics.internalName: #LOCAL
@AbapCatalog.sqlViewName: 'ZISALESORDER'
@AccessControl.authorizationCheck: #CHECK
define view Z_I_SalesOrderAnalytics
  as select from vbak as SalesOrder
  association [0..1] to I_Customer as _Customer
```

### 4. Testing & Optimization
- Data preview and validation
- Performance testing
- Authorization testing
- SAC integration testing

### 5. Deployment & Training
- Transport to production
- User documentation
- Training for business users
- Monitoring and support

---

## Embedded Analytics Use Cases

### Finance
- Real-time P&L monitoring
- Cash flow analysis
- Accounts receivable aging
- Budget vs. actuals tracking
- Cost center analytics

### Sales & Distribution
- Sales order pipeline analysis
- Customer profitability
- Delivery performance tracking
- Backorder monitoring
- Sales rep performance

### Materials Management
- Inventory turnover analysis
- Stock valuation reports
- Purchase order analytics
- Supplier performance tracking
- Material consumption analysis

### Production
- Production order status
- Shop floor performance
- OEE (Overall Equipment Effectiveness)
- Work center capacity analysis
- Material requirements planning

### Controlling
- Cost center reporting
- Profit center analysis
- Internal orders tracking
- Activity-based costing
- Variance analysis

---

## CDS View Best Practices

{{< checkmark-list >}}
- **Naming Conventions** - Follow SAP standards (I_ for interface, C_ for consumption)
- **Reusability** - Design for reuse across multiple consumption scenarios
- **Performance** - Use appropriate joins, avoid SELECT * patterns
- **Authorizations** - Implement DCL (Data Control Language) for security
- **Documentation** - Annotate views with descriptions and metadata
- **Testing** - Validate data accuracy and performance before production
- **VDM Alignment** - Align with delivered VDM where possible
{{< /checkmark-list >}}

---

## Why Choose Varnika for Embedded Analytics?

{{< columns >}}
{{< column >}}
### S/4HANA Expertise
Deep understanding of S/4HANA data model, tables, and business logic.
{{< /column >}}

{{< column >}}
### ABAP CDS Skills
Proficient in ABAP CDS development, annotations, and optimization.
{{< /column >}}

{{< column >}}
### Analytics Integration
Connect embedded analytics to SAC for complete analytics solution.
{{< /column >}}
{{< /columns >}}

---

## Technologies & Skills

- SAP S/4HANA (on-premise and Cloud)
- ABAP CDS (Core Data Services)
- ABAP RESTful Application Programming Model (RAP)
- SAP HANA calculation views
- SAP Fiori Elements
- SAP Analytics Cloud (SAC)
- ODATA services
- SAP Gateway

---

## Embedded Analytics Architecture

```
┌─────────────────────────────────────┐
│     SAP Analytics Cloud (SAC)       │  ← Live Connection
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│  Consumption CDS Views (C_*)        │  ← Analytics-Ready
├─────────────────────────────────────┤
│  Composite CDS Views (I_*)          │  ← Business Logic
├─────────────────────────────────────┤
│  Basic CDS Views (I_*)              │  ← Table Access
├─────────────────────────────────────┤
│  S/4HANA Tables & Structures        │  ← Database Layer
└─────────────────────────────────────┘
```

---

## Delivered Content Activation

S/4HANA ships with 10,000+ predefined CDS views and analytics:

### Finance Analytics
- 1,500+ delivered CDS views for finance
- General Ledger, AP, AR analytics
- Asset Accounting reports
- Controlling analytics

### Logistics Analytics
- Materials management
- Sales and distribution
- Production planning
- Quality management

**We Help You:**
- Identify relevant delivered content
- Activate and configure standard analytics
- Extend with custom fields and logic
- Optimize performance

---

## Pricing Models

### CDS View Development
Fixed-price per view or view set (basic + composite + consumption).

### Embedded Analytics Project
Time & materials for comprehensive implementation with discovery and design.

### VDM Extension
Fixed-price for specific VDM extension requirements.

### Support Retainer
Ongoing support for CDS view maintenance and optimization.

---

## Case Study Preview

> **Manufacturing Company**  
> *Challenge:* Real-time production monitoring needed, legacy reports slow  
> *Solution:* Custom CDS views on production orders with SAC real-time dashboard  
> *Results:* Instant visibility into shop floor, 100% real-time data, eliminated batch jobs

[View Full Case Study →](/case-studies/)

---

## Embedded Analytics vs. BW/4HANA

| Aspect | Embedded Analytics | BW/4HANA |
|--------|-------------------|----------|
| **Data Latency** | Real-time | Near real-time (scheduled loads) |
| **Use Case** | Operational reporting | Strategic analytics, history |
| **Complexity** | Simple to moderate | Complex multi-source analytics |
| **Data Volume** | Transaction-level detail | Aggregated historical data |
| **Development** | CDS views (ABAP) | Data models (BW) |

*Optimal strategy often combines both - we help you architect the right mix.*

---

## Ready to Enable Real-Time S/4HANA Analytics?

Let's discuss your S/4HANA embedded analytics requirements and how we can help you leverage real-time insights.

[Schedule Free Consultation](/contact/) | [View All Services](/services/) | [Read Embedded Analytics Blog](/blog/)

---

## Frequently Asked Questions

**Q: What's the difference between CDS views and HANA calculation views?**  
A: CDS views are ABAP-based, benefit from S/4HANA semantics, and integrate with VDM. HANA views are SQL-based, more flexible but less integrated. We use both as appropriate.

**Q: Can we use embedded analytics without SAP Analytics Cloud?**  
A: Yes, embedded analytics work with Fiori apps, SAP Query, custom ABAP programs. SAC provides enhanced dashboards but isn't required.

**Q: How do authorizations work with CDS views?**  
A: Through DCL (Data Control Language) combined with S/4HANA authorization objects. We implement proper row-level security.

**Q: Can we write data back through CDS views?**  
A: CDS views are read-only. For write-back, we use ABAP RAP (RESTful Application Programming Model) or standard BAPIs/function modules.

**Q: Will CDS views impact S/4HANA performance?**  
A: If designed properly, no. We optimize views using HANA-specific features, appropriate indexes, and efficient joins.

---

*Varnika IT Consulting - Your S/4HANA Embedded Analytics Partner*
