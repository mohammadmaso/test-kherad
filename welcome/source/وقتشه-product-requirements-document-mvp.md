# وقتشه — Product Requirements Document (MVP)

**Product:** وقتشه (Vaghtesheh) — Appointment scheduling and clinic management  
**Target audience:** Medical clinics and doctors' offices in Iran  
**Status:** MVP — early development  
**Tech stack:** Python (Django/FastAPI) + React + PostgreSQL  
**Language:** Persian (Farsi) — RTL UI

---

## 1. Overview

وقتشه is a web-based appointment scheduling and clinic management platform for Iranian medical clinics. It replaces messy WhatsApp conversations and paper-based scheduling with an online system where patients can book appointments, clinics can manage their schedule, and reminders reduce no-shows.

### 1.1 Key differentiators

- **No installation needed** — fully web-based
- **3-minute setup** for clinics
- **Free first month**, no credit card required
- **Iranian-market focused** — SMS via local providers, Persian RTL UI, Iranian pricing

---

## 2. User Roles

The system supports three distinct roles:

| Role | Description | Auth method |
|---|---|---|
| **Clinic Admin / Owner** | Manages the clinic, services, calendar, and staff | Email + password |
| **Employee (Doctor / Staff)** | Views their own schedule, manages their appointments | Email + password |
| **Patient / Customer** | Books appointments, receives reminders | Phone + OTP |

---

## 3. MVP Feature Scope

The MVP focuses on three core areas. Employee management (shifts, per-employee panels) and advanced reporting are planned for V2.

### 3.1 Online Booking (patient-facing)

**Flow:**
1. Patient lands on the clinic's booking page (shared link)
2. Selects a date
3. Sees available time slots (filtered by doctor availability)
4. Picks a time slot
5. Enters name and phone number
6. Receives confirmation via SMS and/or in-app/web notification

**Key requirements:**
- Slot configuration is **per doctor** — each doctor has their own schedule and slot duration
- Real-time availability — booked slots are immediately removed
- No payment collection in MVP (free booking)
- Mobile-responsive design (patients may book from phone)

### 3.2 Auto Reminders

- Automated reminder sent to patient before their appointment
- Channel: **SMS** (via Iranian SMS provider) + **in-app/web notification**
- Timing: configurable (e.g., 24 hours and/or 2 hours before)

### 3.3 Admin Panel

The admin panel includes:

| Screen | Features |
|---|---|
| **Dashboard** | Overview of today's appointments, quick stats |
| **Appointments (manage)** | View all appointments, add new ones manually, cancel/reschedule |
| **Calendar view** | Visual day/week view of appointments |
| **Services (manage)** | Define the services/visit types the clinic offers (e.g., "General checkup", "Consultation") — including name and duration |

### 3.4 Out of scope for MVP (V2+)

- Employee management (per-employee logins, shift scheduling)
- Advanced reporting and analytics
- Multi-clinic / enterprise features
- Payments / online transactions
- WhatsApp API integration

---

## 4. Authentication & Authorization

| Role | Auth method | Access scope |
|---|---|---|
| Admin | Email + password | Full access to all clinic data |
| Employee | Email + password | View own appointments only (V2) |
| Patient | Phone + OTP | Book/cancel own appointments only |

---

## 5. Pricing Model (from landing page)

The landing page presents three tiers. The pricing structure decision for MVP launch:

| Plan | Price | Employee limit | Notes |
|---|---|---|---|
| Starter | 350,000 toman/month | Up to 2 | Basic features |
| Growth | 600,000 toman/month | Up to 5 | (Most popular) |
| Professional | 1,200,000 toman/month | Up to 10 | Advanced features |
| Enterprise | Custom | 10+ | Contact via WhatsApp |

> **Note:** Pricing implementation approach (single plan vs. all tiers at MVP launch) is yet to be finalized.

- **Free first month** for all plans
- **No credit card required** to start
- Onboarding via WhatsApp

---

## 6. Technical Architecture

### 6.1 Stack

| Layer | Technology |
|---|---|
| Backend | Python (Django or FastAPI — to be confirmed) |
| Frontend | React |
| Database | PostgreSQL |
| Authentication | JWT-based (email+password for admin/employee, phone+OTP for patients) |
| SMS | Iranian SMS provider integration |
| Deployment | Web-based (no native mobile app in MVP) |

### 6.2 High-level components

```
┌─────────────────────┐      ┌─────────────────────┐
│   Patient Web UI    │      │  Admin/Employee UI  │
│  (React - RTL)      │      │  (React - RTL)      │
└─────────┬───────────┘      └──────────┬───────────┘
          │                             │
          └──────────┬──────────────────┘
                     │
          ┌──────────▼──────────┐
          │   API Layer         │
          │  (Django/FastAPI)   │
          └──────────┬──────────┘
                     │
          ┌──────────▼──────────┐
          │   PostgreSQL        │
          └─────────────────────┘
```

### 6.3 Key data entities (conceptual)

- **Clinic** — name, address, contact info, settings
- **Doctor / Employee** — name, specialty, schedule (per-doctor slots & duration)
- **Service** — name, duration, price (optional in MVP)
- **Appointment** — patient info, doctor, service, date/time, status (confirmed/cancelled)
- **Patient** — name, phone number, booking history

---

## 7. User Stories (MVP)

### Patient
- As a patient, I can visit a booking link and see available dates/times
- As a patient, I can select a doctor (if multiple available) and pick a time slot
- As a patient, I can enter my name and phone to confirm my booking
- As a patient, I receive an SMS confirmation of my appointment
- As a patient, I receive a reminder before my appointment

### Admin
- As an admin, I can log in with email and password
- As an admin, I can see a dashboard with today's appointments
- As an admin, I can view all appointments in a calendar
- As an admin, I can manually add, cancel, or reschedule appointments
- As an admin, I can define the services my clinic offers
- As an admin, I can set each doctor's working hours and slot duration

---

## 8. Non-functional Requirements

- **RTL support** — Full Persian (Farsi) RTL layout
- **Mobile-responsive** — Booking flow must work on mobile browsers
- **Performance** — Booking page should load in under 2 seconds
- **Reliability** — No double-booking allowed (concurrency handled at DB level)
- **SMS delivery** — Reliable integration with Iranian SMS gateways (e.g., Kavenegar, Farazsms)

---

## 9. Future Roadmap (V2+)

- Employee login with restricted access (each doctor sees only their schedule)
- Shift management for employees
- Advanced reporting (revenue, occupancy rate, patient trends)
- WhatsApp API integration for booking and reminders
- Online payment integration (Iranian payment gateways)
- Multi-clinic / enterprise management
- Native mobile apps (Android/iOS)