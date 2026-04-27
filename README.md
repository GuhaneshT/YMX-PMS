# YMX-PMS — Technical Documentation

## Overview
**YMX-PMS** is a production-grade backend system developed for **YMX India Pvt. Ltd.** to manage and automate the lifecycle of industrial knitting machinery.

The system is designed with a strong emphasis on **data integrity**, **real-time synchronization**, and **operational clarity**, making it well-suited for small enterprise environments with complex workflows.

It is built using **FastAPI and PostgreSQL**, and features a custom-built **Change Data Capture (CDC) engine** for real-time data replication and auditing.

---

## Links
- **Backend Repository:** [[API Layer](https://github.com/GuhaneshT/mpms-backend)]  
- **Frontend Repository:** [[Frontend](https://github.com/GuhaneshT/mpms-frontend)]  
- **Live Deployment:** [[Live](https://mpms-frontend-zeta.vercel.app/)]
- Test Creds :
    - User : test
    - PWD : 12345

---

## System Architecture


### API Layer
- Built with **FastAPI**
- Uses **Pydantic** for strict request and response validation


### Service Layer
- Encapsulates core business logic and workflows
- Handles:
  - Machine commissioning lifecycle
  - Order state transitions
  - Validation and consistency rules


### Data Layer
- Powered by **PostgreSQL (Supabase)**
- Ensures:
  - High data integrity
  - Reliable relationships via foreign keys
  - Flexible querying for operational use

### Custom CDC Engine
A central component of the system.

- Functions as a standalone **logical replication agent**
- Streams database changes in real time
- Replicates data to:
  - Local PostgreSQL instance
  - JSONL-based audit logs

---

## CDC Synchronization Engine

### Logical Replication
- Uses PostgreSQL **replication slots**
- Reads from the **Write-Ahead Log (WAL)** using the `pgoutput` protocol
- Eliminates inefficient polling mechanisms
- Enables near real-time data synchronization

---

### Topological Dependency Sorting
- Represents schema relationships as a **Directed Acyclic Graph (DAG)**
- Uses **Kahn’s Algorithm** to determine execution order
- Ensures:
  - Parent-child relationships are preserved
  - Foreign key constraints are never violated during replication

---

### Idempotent Synchronization
- Uses **UPSERT (`ON CONFLICT`)** operations across all sinks
- Guarantees:
  - Safe retries
  - Resilience to failures
  - No duplicate or inconsistent data states

---

### Multi-Sink Replication
- **Local PostgreSQL**
  - Provides a fully queryable local copy of production data
- **JSONL Audit Logs**
  - Maintains an immutable record of all changes
  - Useful for debugging, auditing, and traceability

---

## Design Philosophy

This system is intentionally optimized for **high-value, low-volume usage environments** (fewer than 15 users).

### Core Principles
- **Low Cost Higher Throughput**
  - Focus on keeping the building cost low while keeping the quality intact
- **Low latency operations**
  - Real-time updates to support operational workflows
- **Data ownership**
  - Clients maintain a local copy of their data for independence and reliability
- **Operational simplicity**
  - Minimal infrastructure complexity
  - No requirement for dedicated database administration

---

## Tech Stack

| Layer             | Technology |
|------------------|-----------|
| Language          | Python 3.12+ |
| Backend Framework | FastAPI |
| Authentication    | Supabase Auth |
| Database          | PostgreSQL (Supabase) |
| Real-time Sync    | PostgreSQL Logical Replication (`pgoutput`) |
| Deployment        | Render (Backend), Vercel (Frontend) |


---

## Future Enhancements
- Role-Based Access Control (RBAC)  
- Feature Enhancements  

---
