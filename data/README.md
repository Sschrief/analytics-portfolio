# Star Schema Dataset Documentation

## 📊 Dataset Overview
Enterprise-grade dimensional model with **50,000+ transaction records** spanning 2020-2024, designed for advanced Power BI analytics and executive reporting.

**Total Files:** 7 CSV files (1 fact table + 6 dimension tables)  
**Data Volume:** ~15MB total, production-scale dataset  
**Time Range:** 5 years (2020-2024) with seasonal patterns and YoY growth

## 🏗️ Star Schema Design
┌─────────────────┐
                │   Fact_Orders   │
                │─────────────────│
                │ OrderID (PK)    │
        ┌───────│ OrderDate (FK)  │─────────┐
        │       │ CustomerID (FK) │         │
        │       │ ProductID (FK)  │         │
        │       │ GeographyID(FK) │         │
        │       │ SalesRepID (FK) │         │
        │       │ ChannelID (FK)  │         │
        │       │ Revenue         │         │
        │       │ Cost            │         │
        │       │ GrossProfit     │         │
        │       │ Quantity        │         │
        │       └─────────────────┘         │
        │                │                  │
┌───────▼─────┐  ┌───────▼─────┐  ┌───────▼─────┐
│ Dim_Date    │  │Dim_Customer │  │ Dim_Product │
│─────────────│  │─────────────│  │─────────────│
│ Date (PK)   │  │CustomerID(PK)│  │ProductID(PK)│
│ Year        │  │CustomerName │  │ProductName  │
│ Quarter     │  │Industry     │  │ProductLine  │
│ FiscalYear  │  │CompanySize  │  │Category     │
└─────────────┘  │CustomerTier │  │BasePrice    │
                 └─────────────┘  └─────────────┘
                 ## 📋 Table Specifications

### 📊 **Fact_Orders.csv** (~50,000 records)
**Primary fact table containing all business transactions**

| Column | Type | Description | Sample |
|--------|------|-------------|---------|
| OrderID | String | Unique order identifier | ORD-0000001 |
| OrderDate | Date | Transaction date (YYYY-MM-DD) | 2023-05-15 |
| CustomerID | String | Foreign key to customer | CUST-0001 |
| ProductID | String | Foreign key to product | PROD-0001 |
| GeographyID | String | Foreign key to geography | GEO-0001 |
| SalesRepID | String | Foreign key to sales rep | REP-001 |
| ChannelID | String | Foreign key to channel | CHN-001 |
| Quantity | Integer | Units sold | 5 |
| UnitPrice | Currency | Price per unit | 25000 |
| Revenue | Currency | Total order revenue | 125000 |
| Cost | Currency | Total order cost | 43750 |
| GrossProfit | Currency | Revenue minus cost | 81250 |
| GrossMarginPercent | Decimal | Profit margin percentage | 65.00 |

### 📅 **Dim_Date.csv** (1,827 records)
**Complete date dimension supporting fiscal calendar**

| Column | Type | Description | Business Use |
|--------|------|-------------|--------------|
| Date | Date | Calendar date | Time intelligence |
| Year | Integer | Calendar year | YoY analysis |
| Quarter | Integer | Calendar quarter (1-4) | Quarterly reporting |
| FiscalYear | Integer | Fiscal year (Feb-Jan) | Budget alignment |
| FiscalQuarter | Integer | Fiscal quarter | Financial reporting |
| IsBusinessDay | Boolean | Excludes weekends | Revenue forecasting |

### 👥 **Dim_Customer.csv** (500 records)
**Customer master with hierarchical segmentation**

| Column | Type | Description | Hierarchy Level |
|--------|------|-------------|-----------------|
| CustomerID | String | Primary key | - |
| CustomerName | String | Company name | Level 4 |
| Industry | String | Business sector | Level 1 |
| CompanySize | String | Enterprise/Mid-Market/SMB | Level 2 |
| CustomerTier | String | Platinum/Gold/Silver/Bronze | Level 3 |

**Industries:** Technology, Healthcare, Financial Services, Manufacturing, Retail, Energy

### 📦 **Dim_Product.csv** (60 records)
**Product hierarchy enabling category analysis**

| Column | Type | Description | Hierarchy Level |
|--------|------|-------------|-----------------|
| ProductID | String | Primary key | - |
| ProductName | String | Specific product | Level 4 |
| ProductLine | String | Product family | Level 3 |
| Category | String | Major category | Level 1 |
| BasePrice | Currency | Standard pricing | - |

**Categories:** Network Infrastructure, Cloud Solutions, Security Platforms, Analytics Software

### 🌍 **Dim_Geography.csv** (35 records)
**Geographic hierarchy for regional analysis**

| Column | Type | Description | Hierarchy Level |
|--------|------|-------------|-----------------|
| GeographyID | String | Primary key | - |
| City | String | Specific location | Level 3 |
| Country | String | Nation | Level 2 |
| Region | String | Global region | Level 1 |

**Regions:** North America, Europe, Asia Pacific, Latin America

### 👤 **Dim_SalesRep.csv** (25 records)
**Sales team structure and performance tracking**

| Column | Type | Description | Use Case |
|--------|------|-------------|----------|
| SalesRepID | String | Primary key | - |
| RepName | String | Representative name | Individual tracking |
| RepTeam | String | Team assignment | Team performance |
| RepRegion | String | Territory coverage | Regional analysis |

### 🛒 **Dim_Channel.csv** (4 records)
**Sales channel definitions and cost structures**

| Column | Type | Description | Business Impact |
|--------|------|-------------|-----------------|
| ChannelID | String | Primary key | - |
| ChannelName | String | Channel description | Channel effectiveness |
| ChannelType | String | Direct/Indirect/Digital | Strategy analysis |
| CostPercent | Decimal | Channel cost factor | Profitability analysis |

## 🎯 Power BI Implementation Guide

### **Relationship Setup**
```dax
// Create these relationships in Power BI Model view:
Fact_Orders[OrderDate] → Dim_Date[Date] (Many-to-One)
Fact_Orders[CustomerID] → Dim_Customer[CustomerID] (Many-to-One)
Fact_Orders[ProductID] → Dim_Product[ProductID] (Many-to-One)
Fact_Orders[GeographyID] → Dim_Geography[GeographyID] (Many-to-One)
Fact_Orders[SalesRepID] → Dim_SalesRep[SalesRepID] (Many-to-One)
Fact_Orders[ChannelID] → Dim_Channel[ChannelID] (Many-to-One)
