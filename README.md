# 🏢 Enterprise Resource Planning (ERP) System

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.1-000000?style=for-the-badge&logo=flask&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)

**A production-ready, full-featured ERP system for small & medium businesses**

[Features](#-features) • [Architecture](#-system-architecture) • [Modules](#-modules) • [Screenshots](#-screenshots) • [Tech Stack](#️-tech-stack)

</div>

---

## 📋 Overview

A comprehensive **Enterprise Resource Planning (ERP)** system built from scratch using Python Flask and PostgreSQL. This system manages complete business operations including **Sales, Purchase, Inventory, Manufacturing, HR, and Finance** with Indian GST compliance.

### 🎯 Key Highlights

| Metric | Details |
|--------|---------|
| 📊 **Database Models** | 77+ interconnected tables |
| 🔌 **API Modules** | 26 Flask Blueprints |
| 🔐 **Permissions** | 50+ granular permissions |
| 👥 **Multi-User** | LAN-based concurrent access |
| 🇮🇳 **GST Compliant** | E-Invoice & E-Way Bill ready |
| 📱 **Responsive** | Works on desktop, tablet, mobile |

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
        WS["Waitress WSGI<br/>Server"]
    end

    subgraph App["🚀 Flask Application Layer"]
        direction TB
        MW["🛡️ Middleware<br/>(CSRF, Auth, Session)"]
        
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
            GST["GST<br/>Calculator"]
            PDF["PDF<br/>Generator"]
            NOTIF["Notification<br/>Service"]
        end
    end

    subgraph Data["🗄️ Data Layer"]
        direction LR
        PG[("PostgreSQL<br/>Database")]
        CACHE[("Flask-Cache<br/>Simple Cache")]
        FS[("📁 File<br/>Storage")]
    end

    B1 & B2 & B3 --> WS
    WS --> MW
    MW --> Blueprints
    Blueprints --> Services
    Services --> PG & CACHE & FS
