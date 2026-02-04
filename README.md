# 🏢 Enterprise Resource Planning (ERP) System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.1-000000?style=for-the-badge&logo=flask&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)

**A production-ready, full-featured ERP system for small & medium businesses**

[Features](#-key-features) • [Architecture](#-system-architecture) • [Modules](#-module-overview) • [Screenshots](#-screenshots) • [Tech Stack](#️-tech-stack)

</div>

---

## 📋 Overview

A comprehensive **Enterprise Resource Planning (ERP)** system built from scratch using Python Flask and PostgreSQL. This system manages complete business operations including **Sales, Purchase, Inventory, Manufacturing, HR, and Finance** modules with multi-user LAN support.

### 🎯 Key Highlights

|
 Metric 
|
 Details 
|
|
--------
|
---------
|
|
 📊 
**
Database Models
**
|
 77+ interconnected tables 
|
|
 🔌 
**
API Modules
**
|
 26 Flask Blueprints 
|
|
 🔐 
**
Permissions
**
|
 50+ granular permissions 
|
|
 👥 
**
Multi-User
**
|
 LAN-based concurrent access 
|
|
 📱 
**
Responsive
**
|
 Works on desktop, tablet, mobile 
|
|
 🖨️ 
**
Reports
**
|
 PDF & Excel export support 
|

---

## 🏗 System Architecture

### High-Level Overview

```mermaid
flowchart TB
    subgraph Client["🌐 Client Layer"]
        B1[("🖥️ Desktop<br/>Browser")]
        B2[("📱 Mobile<br/>Browser")]
        B3[("📟 Tablet<br/>Browser")]
    end

    subgraph LB["⚖️ Load Balancer"]
        WS["Waitress WSGI<br/>Server :5000"]
    end

    subgraph App["🚀 Flask Application Layer"]
        direction TB
        MW["🛡️ Middleware<br/>CSRF | Auth | Session"]
        
        subgraph Blueprints["📦 26 Blueprint Modules"]
            direction LR
            AUTH["🔐 Auth"]
            SALES["💰 Sales"]
            PURCH["🛒 Purchase"]
            INV["📦 Inventory"]
            MFG["🏭 Manufacturing"]
            HR["👥 HR"]
            FIN["💳 Finance"]
            RPT["📊 Reports"]
        end
        
        subgraph Services["⚙️ Business Services"]
            direction LR
            NUM["Number<br/>Series"]
            PDF["PDF<br/>Generator"]
            NOTIF["Notification<br/>Service"]
            IMG["Image<br/>Service"]
        end
    end

    subgraph Data["🗄️ Data Layer"]
        direction LR
        PG[("PostgreSQL<br/>Database")]
        CACHE[("Flask-Cache")]
        FS[("📁 File<br/>Storage")]
    end

    B1 & B2 & B3 --> WS
    WS --> MW
    MW --> Blueprints
    Blueprints --> Services
    Services --> PG & CACHE & FS
Request-Response Flow
mermaid
sequenceDiagram
    autonumber
    participant C as 🌐 Client Browser
    participant M as 🛡️ Middleware
    participant R as 📦 Route/Blueprint
    participant S as ⚙️ Service Layer
    participant D as 🗄️ PostgreSQL

    C->>M: HTTP Request
    M->>M: CSRF Token Validation
    M->>M: Session Authentication
    M->>M: Permission Check
    
    alt Unauthorized
        M-->>C: 401/403 Error Page
    else Authorized
        M->>R: Forward Request
        R->>S: Execute Business Logic
        S->>D: Database Query/Update
        D-->>S: Return Result
        S-->>R: Processed Data
        R-->>C: HTML/JSON Response
    end
🔐 Authentication System
Login Flow with 2FA Support
mermaid
flowchart TD
    A["🔑 Login Page"] --> B{Valid<br/>Credentials?}
    B -->|No| C["❌ Increment<br/>Failed Count"]
    C --> D{Failed >= 5?}
    D -->|Yes| E["🔒 Lock Account<br/>for 30 minutes"]
    D -->|No| A
    
    B -->|Yes| F{2FA<br/>Enabled?}
    F -->|No| G["✅ Create Session"]
    F -->|Yes| H["📱 Enter OTP"]
    H --> I{OTP Valid?}
    I -->|No| J["❌ Invalid OTP"]
    J --> H
    I -->|Yes| G
    
    G --> K["🏠 Redirect to Dashboard"]
    
    E --> L["⏰ Wait 30 minutes"]
    L --> A

    style A fill:#e1f5fe
    style G fill:#c8e6c9
    style E fill:#ffcdd2
    style K fill:#c8e6c9
Role-Based Access Control (RBAC)
mermaid
flowchart LR
    subgraph Users["👥 Users"]
        U1["Admin"]
        U2["Sales Manager"]
        U3["Purchase Manager"]
        U4["Inventory Staff"]
        U5["HR Manager"]
        U6["Accountant"]
    end

    subgraph Roles["🎭 Roles"]
        R1["Admin Role"]
        R2["Sales Role"]
        R3["Purchase Role"]
        R4["Inventory Role"]
        R5["HR Role"]
        R6["Finance Role"]
    end

    subgraph Permissions["🔑 Permissions (50+)"]
        P1["view_sales<br/>create_sales<br/>approve_sales"]
        P2["view_purchase<br/>create_po<br/>approve_po"]
        P3["view_inventory<br/>stock_adjustment"]
        P4["view_hr<br/>manage_payroll"]
        P5["view_finance<br/>process_payment"]
    end

    U1 --> R1
    U2 --> R2
    U3 --> R3
    U4 --> R4
    U5 --> R5
    U6 --> R6

    R1 -->|ALL PERMISSIONS| P1 & P2 & P3 & P4 & P5
    R2 --> P1
    R3 --> P2
    R4 --> P3
    R5 --> P4
    R6 --> P5
📦 Module Overview
Module Interaction Diagram
mermaid
flowchart TB
    subgraph Sales["💰 SALES MODULE"]
        CUS[("👤 Customers")]
        QUO["📝 Quotations"]
        SO["📋 Sales Orders"]
        INV_S["🧾 Invoices"]
        PAY["💵 Payments"]
    end

    subgraph Purchase["🛒 PURCHASE MODULE"]
        SUP[("🏭 Suppliers")]
        PR["📄 Purchase Req"]
        SQ["📊 Supplier Quotes"]
        PO["📦 Purchase Orders"]
        GRN["✅ GRN"]
    end

    subgraph Inventory["📦 INVENTORY MODULE"]
        PRD[("📱 Products")]
        MAT[("🔧 Materials")]
        STK["📊 Stock"]
        MOV["🔄 Movements"]
        BAR["🏷️ Barcodes"]
    end

    subgraph Manufacturing["🏭 MANUFACTURING"]
        PRJ["📋 Projects"]
        BOM["📑 BOM"]
        WO["🔨 Work Orders"]
        PROD["⚙️ Production"]
    end

    subgraph HR["👥 HR MODULE"]
        EMP[("👤 Employees")]
        ATT["📅 Attendance"]
        SAL["💰 Payroll"]
        LVE["🏖️ Leave Mgmt"]
    end

    subgraph Finance["💳 FINANCE"]
        ACC["📊 Accounts"]
        PYMNT["💵 Payments"]
        RPTS["📈 Reports"]
    end

    CUS --> QUO --> SO --> INV_S --> PAY
    SO --> PRJ
    PRJ --> BOM --> WO --> PROD
    BOM --> PR --> SQ --> PO --> GRN
    GRN --> STK
    PROD --> STK
    STK --> MOV
    PRD & MAT --> STK
    INV_S --> PYMNT --> RPTS
    EMP --> ATT --> SAL
    EMP --> LVE
Detailed Module Breakdown
mermaid
mindmap
  root((🏢 ERP System))
    💰 Sales
      👤 Customer Management
      📝 Quotation with Revisions
      📋 Sales Order
      🧾 Invoice Generation
      💵 Payment Tracking
      📄 Debit/Credit Notes
    🛒 Purchase
      🏭 Supplier Management
      📄 Purchase Requisition
      📊 Supplier Quotation
      📦 Purchase Order
      ✅ GRN - Goods Receipt
      ⭐ Vendor Performance
    📦 Inventory
      📱 Product Master
      🔧 Material Master
      🏪 Multi-Location Stock
      🔄 Stock Movements
      🏷️ Barcode Generation
      📊 Low Stock Alerts
    🏭 Manufacturing
      📋 Project Management
      📑 Bill of Materials
      🔨 Work Orders
      ⚙️ Production Entry
      📉 Material Consumption
    👥 HR
      👤 Employee Master
      📅 Attendance Tracking
      🏖️ Leave Management
      💰 Payroll & Salary
      📄 HR Documents
      🎓 Training Programs
      🚪 Exit Process
    ⚡ Electrical Inventory
      🔌 Material Master
      📥 Stock Entry
      🤝 Handover Tracking
      ↩️ Return Processing
      📜 Transaction Log
    📊 Reports
      📈 Dashboard Analytics
      📄 PDF Generation
      📊 Excel Export
      🔍 Custom Reports
🔄 Business Process Flows
Order to Cash (O2C) Flow
mermaid
flowchart LR
    A["👤 Customer<br/>Inquiry"] --> B["📝 Create<br/>Quotation"]
    B --> C{"Customer<br/>Approved?"}
    C -->|No| D["📝 Revise<br/>Quotation"]
    D --> B
    C -->|Yes| E["📋 Create<br/>Sales Order"]
    E --> F["🏭 Create<br/>Work Order"]
    F --> G["⚙️ Start<br/>Production"]
    G --> H["📦 Stock<br/>Ready"]
    H --> I["🧾 Generate<br/>Invoice"]
    I --> J["🚚 Dispatch<br/>Goods"]
    J --> K["💵 Receive<br/>Payment"]
    K --> L["✅ Close<br/>Order"]

    style A fill:#e3f2fd
    style L fill:#c8e6c9
Procure to Pay (P2P) Flow
mermaid
flowchart LR
    A["📋 Material<br/>Requirement"] --> B["📄 Create<br/>PR"]
    B --> C{"Level 1<br/>Approval"}
    C -->|Reject| D["❌ Return to<br/>Creator"]
    C -->|Approve| E{"Level 2<br/>Approval"}
    E -->|Reject| D
    E -->|Approve| F["📊 Request<br/>Supplier Quotes"]
    F --> G["📈 Compare<br/>& Select"]
    G --> H["📦 Create<br/>PO"]
    H --> I["📧 Send to<br/>Supplier"]
    I --> J["📬 Receive<br/>Goods"]
    J --> K["✅ Create<br/>GRN"]
    K --> L["🔍 Quality<br/>Check"]
    L --> M["📦 Update<br/>Inventory"]
    M --> N["💵 Process<br/>Payment"]

    style A fill:#fff3e0
    style N fill:#c8e6c9
Manufacturing Workflow
mermaid
flowchart TD
    A["📋 Sales Order<br/>Received"] --> B["📁 Create/Link<br/>Project"]
    B --> C["📑 Create or<br/>Select BOM"]
    C --> D["📊 Check Material<br/>Availability"]
    D --> E{"Materials<br/>Available?"}
    E -->|No| F["📄 Generate<br/>Purchase Requisition"]
    F --> G["🛒 Complete<br/>Purchase Process"]
    G --> D
    E -->|Yes| H["🔨 Create<br/>Work Order"]
    H --> I["📤 Issue<br/>Materials"]
    I --> J["⚙️ Start<br/>Production"]
    J --> K["📝 Record<br/>Production Entry"]
    K --> L["✅ Quality<br/>Check"]
    L --> M{"QC<br/>Passed?"}
    M -->|No| N["🔄 Rework<br/>Required"]
    N --> J
    M -->|Yes| O["📦 Add to<br/>Finished Goods"]
    O --> P["✅ Complete<br/>Work Order"]

    style A fill:#e3f2fd
    style P fill:#c8e6c9
    style N fill:#ffcdd2
HR - Payroll Process Flow
mermaid
flowchart LR
    A["📅 Month<br/>End"] --> B["📊 Fetch<br/>Attendance"]
    B --> C["🏖️ Calculate<br/>Leave Days"]
    C --> D["💰 Apply Salary<br/>Structure"]
    D --> E["➕ Add<br/>Earnings"]
    E --> F["➖ Apply<br/>Deductions"]
    F --> G["🧮 Calculate<br/>Net Salary"]
    G --> H["📄 Generate<br/>Salary Slip"]
    H --> I["✅ Approve<br/>Payroll"]
    I --> J["💵 Process<br/>Payment"]
    J --> K["📧 Send Slip<br/>to Employee"]

    style A fill:#e8eaf6
    style K fill:#c8e6c9
🗄️ Database Schema
Core Entity Relationship
mermaid
erDiagram
    USERS ||--o{ ROLES : has
    USERS {
        int id PK
        string username UK
        string email UK
        string password_hash
        int role_id FK
        boolean is_admin
        boolean two_factor_enabled
        datetime created_at
    }
    
    ROLES ||--o{ ROLE_PERMISSIONS : has
    ROLES {
        int id PK
        string name UK
        string description
        boolean is_active
    }
    
    PERMISSIONS ||--o{ ROLE_PERMISSIONS : assigned_to
    PERMISSIONS {
        string name PK
        string description
        string category
    }
    
    CUSTOMERS ||--o{ QUOTATIONS : places
    CUSTOMERS ||--o{ SALES_ORDERS : places
    CUSTOMERS ||--o{ INVOICES : receives
    CUSTOMERS {
        int id PK
        string company_name
        string gstin
        string email
        string phone
        decimal credit_limit
    }
    
    QUOTATIONS ||--o{ QUOTATION_ITEMS : contains
    QUOTATIONS ||--o{ SALES_ORDERS : converts_to
    QUOTATIONS {
        int id PK
        string quotation_number UK
        int customer_id FK
        decimal total_amount
        enum status
        int revision_number
        int parent_quotation_id FK
    }
    
    SALES_ORDERS ||--o{ SO_ITEMS : contains
    SALES_ORDERS ||--o{ INVOICES : generates
    SALES_ORDERS ||--o{ PROJECTS : links_to
    SALES_ORDERS {
        int id PK
        string order_number UK
        int customer_id FK
        int quotation_id FK
        decimal grand_total
        enum status
    }
    
    PRODUCTS ||--o{ INVENTORY : stored_in
    PRODUCTS ||--o{ BOM_ITEMS : used_in
    PRODUCTS {
        int id PK
        string sku UK
        string name
        decimal selling_price
        decimal cost_price
        int quantity_on_hand
        string barcode UK
    }
    
    SUPPLIERS ||--o{ PURCHASE_ORDERS : receives
    SUPPLIERS ||--o{ SUPPLIER_QUOTATIONS : provides
    SUPPLIERS {
        int id PK
        string company_name
        string gstin
        string email
        string payment_terms
    }
    
    PURCHASE_ORDERS ||--o{ PO_ITEMS : contains
    PURCHASE_ORDERS ||--o{ GRN : receives
    PURCHASE_ORDERS {
        int id PK
        string po_number UK
        int supplier_id FK
        int requisition_id FK
        decimal grand_total
        enum status
    }
    
    EMPLOYEES ||--o{ ATTENDANCE : records
    EMPLOYEES ||--o{ LEAVE_APPLICATIONS : applies
    EMPLOYEES ||--o{ SALARY_SLIPS : receives
    EMPLOYEES {
        int id PK
        string employee_code UK
        string first_name
        string last_name
        date date_of_joining
        string department
        string designation
        enum employment_status
    }
Database Statistics
Category	Tables	Key Models
Auth & Users	7	User, Role, Permission, OTP, LoginActivity, AuditLog
Sales	8	Customer, Quotation, SalesOrder, Invoice, Payment, DebitCreditNote
Purchase	10	Supplier, PurchaseRequisition, SupplierQuotation, PurchaseOrder, GRN, VendorPerformance
Inventory	6	Product, Material, Inventory, StockMovement, InventoryLocation, BarcodeSequence
Manufacturing	8	Project, BillOfMaterial, BOMItem, WorkOrder, ProductionEntry, MaterialIssue, MaterialConsumption
HR	20	Employee, Attendance, LeaveBalance, LeaveApplication, SalaryStructure, SalarySlip, EmployeeDocument, Training, ExitProcess
Electrical Inventory	8	ElectricalMaterial, ElectricalCategory, ElectricalBrand, StockEntry, Handover, Return, TransactionLog
System	10	NumberSeries, CompanySettings, Notification, ReportConfig, DocumentAttachment
Total	77+	Complete ERP Coverage
🛠️ Tech Stack
Architecture Layers
mermaid
flowchart TB
    subgraph Frontend["🎨 Frontend Layer"]
        HTML["HTML5"]
        CSS["CSS3 + Bootstrap 5"]
        JS["JavaScript + jQuery"]
        JINJA["Jinja2 Templates"]
        CHART["Chart.js"]
        DT["DataTables"]
    end

    subgraph Backend["⚙️ Backend Layer"]
        FLASK["Flask 3.1"]
        LOGIN["Flask-Login"]
        WTF["Flask-WTF"]
        MAIL["Flask-Mail"]
        CACHE["Flask-Caching"]
        MOMENT["Flask-Moment"]
    end

    subgraph ORM["🔗 ORM Layer"]
        SQLA["SQLAlchemy 2.0"]
        MIGRATE["Flask-Migrate / Alembic"]
    end

    subgraph Database["🗄️ Database Layer"]
        PG["PostgreSQL 15+"]
        SQLITE["SQLite (Dev)"]
    end

    subgraph Services["🔧 Service Layer"]
        PDF["WeasyPrint + ReportLab"]
        EXCEL["Pandas + OpenPyXL"]
        BARCODE["python-barcode + qrcode"]
        OTP["PyOTP (2FA)"]
    end

    subgraph Server["🚀 Server Layer"]
        WAITRESS["Waitress WSGI"]
        LINUX["Linux Server"]
    end

    Frontend --> Backend
    Backend --> ORM
    ORM --> Database
    Backend --> Services
    Backend --> Server
Technology Details
Layer	Technology	Version	Purpose
Backend	Python	3.11+	Core Language
Flask	3.1.1	Web Framework
SQLAlchemy	2.0.42	ORM
Flask-Login	0.6.3	Authentication
Flask-WTF	1.2.2	Forms & CSRF
Flask-Mail	0.10.0	Email Service
Flask-Caching	2.0+	Caching
Database	PostgreSQL	15+	Production DB
SQLite	3.x	Development DB
Flask-Migrate	4.1.0	Migrations
Frontend	Bootstrap	5.3	UI Framework
Jinja2	3.1.6	Templating
Chart.js	4.x	Charts
DataTables	1.x	Table Management
PDF/Excel	WeasyPrint	66.0	PDF Generation
ReportLab	4.4	PDF Reports
Pandas	2.3	Data Processing
OpenPyXL	3.1	Excel Export
Security	PyOTP	2.9	2FA/OTP
Werkzeug	3.1	Password Hashing
Server	Waitress	3.0.2	Production WSGI
🚀 Deployment Architecture
LAN Deployment Setup
mermaid
flowchart TB
    subgraph LAN["🏢 Local Area Network"]
        subgraph Server["🖥️ Linux Server"]
            direction TB
            WS["Waitress WSGI<br/>Server :5000"]
            APP["Flask Application<br/>(26 Blueprints)"]
            PG[("PostgreSQL<br/>Database<br/>77+ Tables")]
            FS[("📁 File Storage<br/>Uploads, PDFs")]
            BACKUP[("💾 Backup<br/>Storage")]
        end
        
        subgraph Clients["💻 Client Machines (5-10 Users)"]
            C1["🖥️ PC - Sales"]
            C2["🖥️ PC - Purchase"]
            C3["🖥️ PC - Inventory"]
            C4["🖥️ PC - Accounts"]
            C5["💻 Laptop - Manager"]
            C6["📱 Mobile - Admin"]
        end
    end

    C1 & C2 & C3 & C4 & C5 & C6 -->|"HTTP :5000"| WS
    WS --> APP
    APP --> PG
    APP --> FS
    PG -.->|"Daily Backup"| BACKUP
Server Requirements
Component	Minimum	Recommended
OS	Ubuntu 20.04 LTS	Ubuntu 22.04 LTS
RAM	4 GB	8 GB
Storage	50 GB SSD	100 GB SSD
CPU	2 Cores	4 Cores
Concurrent Users	5	10+
Network	100 Mbps LAN	1 Gbps LAN
⚡ Key Features
✅ Authentication & Security
 Secure password hashing with Werkzeug
 Two-Factor Authentication (OTP based)
 Role-based access control (RBAC)
 50+ granular permissions
 Failed login tracking & auto account lockout
 Session management with timeout
 CSRF protection on all forms
 Complete audit logging
✅ Sales Management
 Customer master with credit limit tracking
 Quotation with revision system (R1, R2, R3...)
 Convert quotation to sales order
 Sales order with delivery tracking
 Invoice generation with PDF export
 Payment tracking (partial/full)
 Debit/Credit note management
✅ Purchase Management
 Supplier master with performance rating
 Purchase requisition with multi-level approval
 Supplier quotation comparison
 Purchase order auto-generation
 Goods Receipt Note (GRN) with quality check
 Vendor performance analytics
✅ Inventory Management
 Product & material master separation
 Multi-location stock tracking
 Barcode & QR code generation
 8 types of stock movements
 Batch & expiry date tracking
 Low stock alerts & notifications
 Stock reservation for orders
✅ Manufacturing
 Project management with timeline
 Bill of Materials (BOM) with cost calculation
 Work order management
 Production entry with quality check
 Material issue & return tracking
 Consumption variance analysis
 Link projects to quotations & sales orders
✅ HR Management
 Complete employee master
 Daily attendance tracking
 Multiple leave types & approval workflow
 Salary structure with components
 Automatic salary slip generation
 Tax declaration management
 HR document generation (Offer letter, Experience certificate)
 Training program management
 Exit process & clearance
✅ Electrical Inventory (Specialized Module)
 Material master with part codes
 Brand & category management
 Cupboard/Rack location tracking
 Stock entry with person tracking
 Handover to person/project/machine
 Return with condition assessment
 Complete transaction audit log
✅ Reports & Analytics
 Real-time dashboard with KPIs
 PDF report generation
 Excel export functionality
 Custom report builder
 Date-wise filtering
 Module-wise reports
📸 Screenshots
Dashboard
Dashboard
Main dashboard with KPIs, charts, pending tasks, and quick actions

Login Page
Login
Secure login with 2FA support

Quotation Management
Quotation
Create and manage quotations with revision tracking

Sales Order
Sales Order
Sales order management with item details and status tracking

Purchase Order
Purchase Order
Purchase order with supplier details and approval workflow

Inventory Management
Inventory
Stock management with barcode support and location tracking

HR - Employee Management
HR Employee
Complete employee information management

Salary Slip
Salary Slip
Auto-generated salary slip with all components

Reports
Reports
Report generation with PDF and Excel export options

📁 Project Structure
text
ERP-Application/
│
├── 📂 app/
│   ├── 📂 routes/                 # 26 Blueprint modules
│   │   ├── auth.py                # Authentication routes
│   │   ├── dashboard.py           # Dashboard & analytics
│   │   ├── customer.py            # Customer management
│   │   ├── supplier.py            # Supplier management
│   │   ├── product.py             # Product master
│   │   ├── material.py            # Material master
│   │   ├── quotation.py           # Quotation management
│   │   ├── sales_order_routes.py  # Sales orders
│   │   ├── purchase_order.py      # Purchase orders
│   │   ├── purchase_requisition.py# Purchase requisitions
│   │   ├── supplier_quotation.py  # Supplier quotations
│   │   ├── grn.py                 # Goods receipt
│   │   ├── invoice.py             # Invoice management
│   │   ├── payment.py             # Payment tracking
│   │   ├── inventory.py           # Inventory management
│   │   ├── project.py             # Project management
│   │   ├── bom.py                 # Bill of materials
│   │   ├── work_order.py          # Work orders
│   │   ├── hr.py                  # HR management
│   │   ├── electrical_inventory.py# Electrical inventory
│   │   ├── reports.py             # Reports module
│   │   ├── users.py               # User management
│   │   ├── role_permission.py     # Role & permissions
│   │   ├── company_settings.py    # Company config
│   │   └── backup.py              # Backup & restore
│   │
│   ├── 📂 templates/              # 150+ Jinja2 templates
│   │   ├── 📂 auth/
│   │   ├── 📂 dashboard/
│   │   ├── 📂 sales/
│   │   ├── 📂 purchase/
│   │   ├── 📂 inventory/
│   │   ├── 📂 manufacturing/
│   │   ├── 📂 hr/
│   │   ├── 📂 reports/
│   │   └── 📂 errors/
│   │
│   └── 📂 static/                 # Static assets
│       ├── 📂 css/
│       ├── 📂 js/
│       ├── 📂 images/
│       └── 📂 uploads/
│
├── 📂 utils/                      # Utility functions
│   ├── helpers.py
│   ├── timezone.py
│   ├── image_service.py
│   └── amount_to_words.py
│
├── 📄 run.py                      # Application entry point
├── 📄 models.py                   # 77+ Database models
├── 📄 config.py                   # Configuration
└── 📄 requirements.txt            # Dependencies (50+ packages)
📊 Project Statistics
text
╔═══════════════════════════════════════════════════════════════════╗
║                      📈 PROJECT METRICS                            ║
╠═══════════════════════════════════════════════════════════════════╣
║  📦 Database Models              │  77+                           ║
║  🔌 Flask Blueprints             │  26                            ║
║  🔐 Permission Types             │  50+                           ║
║  📋 Number Series Types          │  25+                           ║
║  🏷️ Enum Types                   │  15+                           ║
║  ⚡ SQLAlchemy Event Listeners   │  10+                           ║
║  🛠️ Helper Functions             │  100+                          ║
║  📄 HTML Templates               │  150+                          ║
║  🎨 Lines of Python Code         │  15,000+                       ║
║  📚 Total Dependencies           │  50+                           ║
╚═══════════════════════════════════════════════════════════════════╝
🔧 Local Development Setup
bash
# Clone repository
git clone https://github.com/yourusername/erp-system.git
cd erp-system

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# or
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt

# Set environment variables
cp .env.example .env
# Edit .env with your database credentials

# Initialize database
flask db upgrade
flask init-db

# Run development server
python run.py

# Access application
# Open browser: http://localhost:5000
# Default login: admin / admin123
👨‍💻 Developer
Pawan Yadav
Full-Stack Developer | ERP Specialist | Python Backend Engineer

LinkedIn
GitHub
Email

📝 Important Note
⚠️ Privacy Notice

This repository contains a sanitized documentation version of the ERP system developed for production use. The actual source code is maintained in a private repository to protect:

Client-specific business logic
Sensitive configuration details
Proprietary algorithms
This portfolio demonstrates the architecture, design patterns, technical capabilities, and development expertise involved in building this comprehensive ERP solution.

🏆 What This Project Demonstrates
Skill	Implementation
Full-Stack Development	Python Flask backend + Bootstrap frontend
Database Design	77+ normalized tables with proper relationships
Authentication	Role-based access with 2FA support
Complex Business Logic	Multi-level approvals, workflows, calculations
API Design	26 modular blueprints with RESTful patterns
PDF Generation	Dynamic report generation with WeasyPrint
Excel Processing	Data export with Pandas & OpenPyXL
Security	CSRF, password hashing, session management
Code Organization	Clean architecture with separation of concerns
Production Deployment	LAN-based multi-user system
📄 License
This project documentation is available under the MIT License.

⭐ If you find this project impressive, please consider giving it a star!

Built with ❤️ using Python, Flask & PostgreSQL

Made with Python
PRs Welcome

```
📋 Quick Setup Checklist
After copying this README to your new repository:

text
✅ Step 1: Create new public repo named "erp-system-portfolio" or "erp-architecture"
✅ Step 2: Copy this README.md content
✅ Step 3: Create "screenshots" folder
✅ Step 4: Add 8-10 clean screenshots (use dummy data!)
✅ Step 5: Add MIT LICENSE file
✅ Step 6: Update your GitHub username in LinkedIn/GitHub links
✅ Step 7: Add repository topics: flask, python, erp, postgresql, enterprise-software
✅ Step 8: Pin this repository to your GitHub profile
✅ Step 9: Add this project link to your LinkedIn
