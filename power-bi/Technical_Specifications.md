# Technical Specifications
> Detailed architecture and implementation specifications for the Executive Finance Dashboard

## 🏗️ Data Model Architecture

### **Star Schema Design**
Fact Table: Fact_Orders (50,847 records)
├── Dimension: Dim_Date (1,827 records)
├── Dimension: Dim_Customer (500 records)
├── Dimension: Dim_Product (60 records)
├── Dimension: Dim_Geography (35 records)
├── Dimension: Dim_SalesRep (25 records)
└── Dimension: Dim_Channel (4 records)
### **Relationship Configuration**
| From Table | From Column | To Table | To Column | Cardinality | Cross Filter |
|-------------|-------------|----------|-----------|-------------|--------------|
| Fact_Orders | OrderDate | Dim_Date | Date | Many-to-One | Single |
| Fact_Orders | CustomerID | Dim_Customer | CustomerID | Many-to-One | Single |
| Fact_Orders | ProductID | Dim_Product | ProductID | Many-to-One | Single |
| Fact_Orders | GeographyID | Dim_Geography | GeographyID | Many-to-One | Single |
| Fact_Orders | SalesRepID | Dim_SalesRep | SalesRepID | Many-to-One | Single |
| Fact_Orders | ChannelID | Dim_Channel | ChannelID | Many-to-One | Single |

### **Date Table Configuration**
- **Mark as Date Table:** ✅ Enabled
- **Date Column:** Dim_Date[Date]
- **Auto Date/Time:** ❌ Disabled (using custom date table)
- **Fiscal Year Start:** February 1st
- **Calendar Type:** Hybrid (Standard + Fiscal)

## 📊 Dashboard Layout Specifications

### **Page 1: Executive Overview (16:9 Aspect Ratio)**

**Grid Layout (1280x720 pixels):**
┌─────────────────────────────────────────────────────────────┐
│ 📊 EXECUTIVE DASHBOARD - FY2024 PERFORMANCE    [Filters] │ ← Header (0,0 - 1280,60)
├─────────────────────────────────────────────────────────────┤
│[Revenue KPI] [Growth KPI] [Margin KPI] [Orders KPI]        │ ← KPI Row (0,60 - 1280,180)
│   $1.87B       +13.1%       68.5%       25.2K             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│           MONTHLY REVENUE TREND (2023 vs 2024)             │ ← Main Chart (0,180 - 860,480)
│      📈 Clustered Column + Line Chart                       │
│                                                             │
├────────────────────────────────────┬────────────────────────┤
│    REVENUE BY CATEGORY             │  REGIONAL PERFORMANCE  │ ← Bottom Row
│      🍩 Donut Chart                │   📊 Clustered Bar     │ (0,480 - 1280,720)
│                                    │                        │
└────────────────────────────────────┴────────────────────────┘
**Visual Specifications:**

#### **KPI Cards (4 cards)**
- **Dimensions:** 300x100 pixels each
- **Spacing:** 20px between cards
- **Background:** White (#FFFFFF)
- **Border:** 1px solid #E2E8F0
- **Border Radius:** 8px
- **Shadow:** 0 2px 4px rgba(0,0,0,0.1)

**KPI Card Layout:**
┌─────────────────────────────────────┐
│ 💰 [Icon]              📈 [Trend]  │ ← Icons (24x24px)
│                                     │
│ $1.87B                             │ ← Value (32px, Bold)
│ Total Revenue                       │ ← Label (14px, #64748B)
│ +13.1% vs PY                       │ ← Growth (12px, #10B981)
└─────────────────────────────────────┘
#### **Main Revenue Chart**
- **Type:** Clustered Column + Line Chart
- **Dimensions:** 840x280 pixels
- **Data:** Monthly revenue by year with trend line
- **Colors:** 
  - 2023: #94A3B8 (Light Blue)
  - 2024: #3B82F6 (Primary Blue)
  - Trend: #EF4444 (Red line)
- **Axis:** X=Month, Y1=Revenue, Y2=Growth%

#### **Category Donut Chart**
- **Dimensions:** 300x220 pixels
- **Center Label:** Total Revenue
- **Data Labels:** Category name + percentage
- **Colors:** Sequential blues (#1E40AF to #BFDBFE)

#### **Regional Bar Chart**
- **Dimensions:** 300x220 pixels
- **Orientation:** Horizontal
- **Bars:** Current year vs Previous year
- **Sort:** Descending by current year revenue

### **Filter Panel Configuration**
**Location:** Top right (1000,10 - 1270,50)
**Filters:**
- **Fiscal Year:** Slicer (buttons style)
- **Region:** Dropdown (multi-select)
- **Customer Segment:** Radio buttons
- **Reset Button:** Clear all filters

## 🎨 Design System

### **Color Palette**
```css
/* Primary Colors */
--primary-blue: #1E40AF;
--secondary-blue: #3B82F6;
--light-blue: #93C5FD;

/* Status Colors */
--success-green: #10B981;
--warning-yellow: #F59E0B;
--error-red: #EF4444;

/* Neutral Colors */
--gray-900: #111827;  /* Headers */
--gray-600: #4B5563;  /* Body text */
--gray-400: #9CA3AF;  /* Labels */
--gray-100: #F3F4F6;  /* Backgrounds */
--white: #FFFFFF;     /* Cards */
Typography Scale
/* Font Family */
font-family: 'Segoe UI', -apple-system, sans-serif;

/* Text Sizes */
--text-4xl: 36px;  /* KPI Values */
--text-2xl: 24px;  /* Page Headers */
--text-xl: 20px;   /* Section Headers */
--text-lg: 18px;   /* Chart Titles */
--text-base: 16px; /* Body Text */
--text-sm: 14px;   /* Labels */
--text-xs: 12px;   /* Captions */

/* Font Weights */
--font-bold: 700;     /* KPIs, Headers */
--font-semibold: 600; /* Subheaders */
--font-medium: 500;   /* Labels */
--font-normal: 400;   /* Body text */
