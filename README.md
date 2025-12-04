# Blood Donation Management System

A comprehensive, full-stack blood donation management system that connects donors, patients, medical staff, organizers, and administrators through a robust digital platform. The system features a Flutter-based cross-platform frontend, FastAPI backend, and Oracle Database for reliable data management.

---

## 📋 Table of Contents

- [Overview](#overview)
- [System Architecture](#system-architecture)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Database Schema](#database-schema)
- [User Dashboards](#user-dashboards)
- [Installation Guide](#installation-guide)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Security Features](#security-features)
- [Contributing](#contributing)
- [Demo Video](#demo-video)

---

## 🎯 Overview

The Blood Donation Management System is designed to streamline the entire blood donation process, from donor registration and health screening to blood inventory management and patient requests. The system ensures efficient blood bank operations, tracks donation camps, manages blood compatibility, and facilitates seamless communication between all stakeholders.

### Problem Statement
Traditional blood donation systems face challenges including:
- Manual tracking of blood inventory
- Difficulty matching donors with recipients
- Lack of real-time availability information
- Inefficient donation camp management
- Complex blood compatibility verification

### Solution
This system provides:
- Real-time blood inventory tracking across multiple blood banks
- Automated blood compatibility checking
- Centralized donor and patient management
- Digital donation camp organization
- Role-based access control for different user types

---

## 🏗️ System Architecture

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Flutter Frontend                          │
│              (Cross-Platform Mobile & Web)                   │
│  ┌──────────┬──────────┬──────────┬──────────┬──────────┐  │
│  │ Patient  │  Donor   │ Medical  │Organizer │  Admin   │  │
│  │Dashboard │Dashboard │  Staff   │Dashboard │Dashboard │  │
│  │          │          │Dashboard │          │          │  │
│  └──────────┴──────────┴──────────┴──────────┴──────────┘  │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP/REST API
                         │ JSON
┌────────────────────────▼────────────────────────────────────┐
│                  FastAPI Backend (Python)                    │
│  ┌──────────────────────────────────────────────────────┐  │
│  │   Routers (Endpoints)     │   Business Logic         │  │
│  │   - User Management       │   - Request Processing   │  │
│  │   - Patient Requests      │   - Data Validation      │  │
│  │   - Donor Requests        │   - Error Handling       │  │
│  │   - Medical Staff         │   - CORS Middleware      │  │
│  │   - Organizer Requests    │   - Session Pooling      │  │
│  │   - Blood Bank Operations │                          │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────┘
                         │ cx_Oracle Driver
                         │ Connection Pooling
┌────────────────────────▼────────────────────────────────────┐
│              Oracle Database (Local Instance)                │
│  ┌──────────────────────────────────────────────────────┐  │
│  │   Tables (20+)           │   Database Objects         │  │
│  │   - User Accounts        │   - Stored Procedures      │  │
│  │   - Patients & Donors    │   - Triggers               │  │
│  │   - Blood Inventory      │   - Views                  │  │
│  │   - Donation Camps       │   - Constraints            │  │
│  │   - Payments             │   - Sequences (Auto PKs)   │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Communication Flow

1. **Frontend Layer**: Flutter application handles UI/UX and user interactions
2. **API Layer**: FastAPI processes requests, validates data, and enforces business logic
3. **Database Layer**: Oracle Database stores and manages all persistent data
4. **Connection**: cx_Oracle driver with connection pooling for efficient database access

---

## ✨ Key Features

### 🔐 Authentication & Authorization
- **Role-Based Access Control**: 5 distinct user roles with specific permissions
- **Secure Login**: SHA-256 password hashing
- **Session Management**: Persistent user sessions using shared preferences

### 👥 User Management
- User registration with email and phone validation
- Profile management for all user types
- Contact information updates

### 🩸 Blood Inventory Management
- Real-time blood inventory tracking
- Multi-location blood bank support
- Blood group categorization (A+, A-, B+, B-, AB+, AB-, O+, O-)
- Units available tracking
- Cost per unit management
- Rare blood type flagging

### 🤝 Donor Features
- Donor health condition tracking
- Donation history
- Last donation date tracking
- Donation camp location finder
- Total donations counter
- Health eligibility verification

### 🏥 Patient Features
- Blood availability checking by location
- Blood compatibility verification
- Medical history management
- Emergency contact management
- Blood reservation system
- Donation request creation

### 🏕️ Donation Camp Management
- Camp creation and scheduling
- Location-based camp search
- Organizer assignment
- Donor participation tracking
- Camp details updating

### 💰 Payment Processing
- Multiple payment methods support
- Payment tracking for blood requests
- Transaction history
- Amount calculation based on blood type and units

### 🔄 Exchange System
- Blood exchange request management
- Donor-recipient matching
- Health test verification
- Exchange tracking

### 📊 Reporting & Analytics
- Blood inventory reports
- Donation statistics
- User role summaries
- Compatibility matrices

---

## 💻 Technology Stack

### Frontend
| Technology | Version | Purpose |
|------------|---------|---------|
| **Flutter** | SDK >=2.12.0 <4.0.0 | Cross-platform UI framework |
| **Dart** | Latest | Programming language |
| **http** | ^1.4.0 | HTTP client for API calls |
| **provider** | ^6.0.5 | State management |
| **shared_preferences** | ^2.2.2 | Local storage for sessions |
| **crypto** | ^3.0.0 | Password hashing (SHA-256) |
| **intl** | ^0.18.1 | Date formatting and localization |

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| **FastAPI** | 0.100.0 | High-performance web framework |
| **Python** | 3.8+ | Programming language |
| **Uvicorn** | 0.22.0 | ASGI server |
| **cx_Oracle** | 8.3.0 | Oracle Database driver |
| **Pydantic** | 1.10.7 | Data validation |
| **SQLAlchemy** | 2.0.17 | ORM (optional usage) |
| **python-dotenv** | 1.0.0 | Environment variable management |

### Database
| Technology | Purpose |
|------------|---------|
| **Oracle Database** | Primary data storage |
| **PL/SQL** | Stored procedures and triggers |
| **SQL** | Data definition and manipulation |

### Development Tools
- **VS Code** / **Android Studio** - IDEs
- **Postman** - API testing
- **Oracle SQL Developer** - Database management
- **Git** - Version control

---

## 🗃️ Database Schema

### Core Tables (20+)

#### 1. **PUser_Account**
Primary table for all user authentication and basic information.
```sql
- user_id (PK)
- username (UNIQUE)
- password (SHA-256 hashed)
- email (UNIQUE)
- phone
- role (patient | donor | medical staff | administrator | organizer)
```

#### 2. **PPatient**
Patient-specific information linked to user accounts.
```sql
- patient_id (PK)
- user_id (FK → PUser_Account)
- blood_group
- medical_history
```

#### 3. **PDonor**
Donor information and donation tracking.
```sql
- donor_id (PK)
- user_id (FK → PUser_Account)
- blood_group
- last_donation_date
```

#### 4. **PBlood_Bank**
Blood bank locations and details.
```sql
- bank_id (PK)
- bank_name (UNIQUE)
- bank_location (UNIQUE)
```

#### 5. **PBlood_Inventory**
Real-time blood inventory tracking.
```sql
- inventory_id (PK)
- bank_id (FK → PBlood_Bank)
- blood_group (FK → PBlood_Type_Info)
- units_available (CHECK >= 0)
```

#### 6. **PDonation_Camp**
Donation camp organization and scheduling.
```sql
- camp_id (PK)
- camp_name
- camp_date
- camp_location
- organizer_id (FK → POrganizer)
```

#### 7. **PDonation**
Individual donation records.
```sql
- donation_id (PK)
- donor_id (FK → PDonor)
- camp_id (FK → PDonation_Camp)
- donation_date
- units_donated (CHECK > 0)
```

#### 8. **PDonation_Request**
Patient blood requests.
```sql
- request_id (PK)
- patient_id (FK → PPatient)
- blood_group
- units_required (CHECK > 0)
- request_date
- fulfilled (Y/N)
```

#### 9. **PBlood_Type_Info**
Blood type metadata.
```sql
- blood_group (PK)
- cost_per_unit (CHECK >= 0)
- rare_type (Y/N)
```

#### 10. **PBlood_Compatibility**
Blood donation compatibility matrix.
```sql
- donor_group (PK, Composite)
- recipient_group (PK, Composite)
```

### Additional Tables
- **PPatient_Contact** - Emergency contacts
- **PDonor_HealthCondition** - Donor health tracking
- **PAdministrator** - Admin user details
- **PMedical_Staff** - Medical staff qualifications
- **POrganizer** - Organizer organizations
- **PExchange_Request** - Blood exchange requests
- **PExchange_Donor** - Exchange donor details
- **PPayment_Method** - Payment options
- **PPayment** - Payment transactions
- **BloodReservation** - Blood unit reservations

### Database Features

#### Stored Procedures
- `FetchPatientDetails` - Retrieves patient information
- `UpdatePatientDetails` - Updates patient data
- `UpdatePhoneNumber` - Updates user contact
- `GetBloodBankLocations` - Lists blood bank locations
- `GetDonationCampLocations` - Lists camp locations
- `GetCampsByLocation` - Filters camps by location
- `GetDonationsByUser` - Retrieves user donation history
- `UpdateMedicalStaffQualification` - Updates staff info
- `UpdateCampDetails` - Modifies camp information
- `FetchCampsByUser` - Gets user-specific camps
- `add_donor_with_condition` - Registers donor with health conditions

#### Views
- **VUserRoles** - Consolidated user information with roles
- **VBloodInventoryDetails** - Comprehensive inventory view with bank details

#### Triggers & Sequences
- Auto-increment primary keys using sequences
- Data validation triggers
- Audit trail triggers

---

## 👨‍💼 User Dashboards

### 1. 🏥 Patient Dashboard

**Primary Functions:**
- **Profile Management**
  - View and update personal information
  - Manage blood group and medical history
  - Update contact phone number

- **Blood Availability**
  - Search blood availability by location and blood group
  - View units available and cost per unit
  - Check rare blood type information

- **Blood Compatibility**
  - Check compatible blood types for recipient
  - View compatibility matrix
  - Get donor-recipient matching information

- **Blood Reservation**
  - Reserve blood units from specific inventory
  - Track reservation status
  - View reservation history

- **Request Management**
  - Create donation requests
  - Track request fulfillment status
  - View request history

**Key Features:**
- Real-time blood availability checking
- Multiple blood bank location search
- Emergency contact management
- Medical history tracking

---

### 2. 🩸 Donor Dashboard

**Primary Functions:**
- **Profile Management**
  - View personal information
  - Track donation statistics
  - Update contact details

- **Health Information**
  - View blood group
  - Track last donation date
  - View health conditions
  - Check eligibility for donation

- **Donation History**
  - View complete donation history
  - Track total donations count
  - See donation dates and locations
  - View units donated per session

- **Camp Discovery**
  - Browse donation camp locations
  - View camps by location
  - Check camp dates and details
  - Register for upcoming camps

**Key Features:**
- Total donations counter
- Last donation date tracking
- Health condition monitoring
- Location-based camp finder

---

### 3. 🏥 Medical Staff Dashboard

**Primary Functions:**
- **Profile Management**
  - View staff credentials
  - Update qualification information
  - Manage contact details

- **Blood Inventory Viewing**
  - Check blood bank inventory levels
  - View stock by blood group
  - Monitor rare blood types
  - Track inventory across locations

- **Donation Camp Information**
  - View upcoming camps
  - Access camp locations and schedules
  - Check donor participation

- **Patient Management**
  - View patient requests
  - Verify blood compatibility
  - Assist in fulfillment process

**Key Features:**
- Qualification management
- Multi-location inventory access
- Real-time stock monitoring
- Patient request visibility

---

### 4. 🎪 Organizer Dashboard

**Primary Functions:**
- **Profile Management**
  - View organization details
  - Update organization name
  - Manage contact information

- **Camp Management**
  - Create new donation camps
  - Update camp details (name, date, location)
  - View camps organized by user
  - Schedule camp activities

- **Blood Bank Operations**
  - Create new blood banks
  - Add blood inventory
  - Update inventory levels
  - Manage bank locations

- **Donor Registration**
  - Register new donors with health conditions
  - Add donor information
  - Track donor health status
  - Assign blood groups

- **Donation Recording**
  - Record donations from camps
  - Link donations to specific camps
  - Track units donated
  - Update inventory after donations

**Key Features:**
- Comprehensive camp creation and editing
- Blood bank setup and management
- Donor onboarding with health screening
- Donation tracking and inventory updates

---

### 5. 👨‍💼 Administrator Dashboard

**Primary Functions:**
- **Profile Management**
  - View admin credentials
  - Update contact information
  - Manage designation

- **System Overview**
  - View all user roles
  - Monitor system activity
  - Access comprehensive reports

- **User Management**
  - View all registered users
  - Check user roles and permissions
  - Monitor user activity

- **Inventory Oversight**
  - View complete blood inventory across all banks
  - Monitor stock levels system-wide
  - Track blood type availability
  - Oversee inventory management

- **Donation Monitoring**
  - View all donations
  - Track donation requests
  - Monitor fulfillment rates
  - Generate reports

**Key Features:**
- System-wide visibility
- User role management
- Comprehensive inventory access
- Administrative controls
- Reporting capabilities

---

## 📥 Installation Guide

### Prerequisites

#### System Requirements
- **Operating System**: Windows 10+, macOS 10.14+, or Linux
- **RAM**: Minimum 8GB (16GB recommended)
- **Storage**: 10GB free space

#### Software Requirements
1. **Oracle Database**
   - Oracle Database 12c or higher
   - Oracle Instant Client
   - SQL Developer (optional, for database management)

2. **Python 3.8+**
   - Download from [python.org](https://www.python.org/downloads/)

3. **Flutter SDK**
   - Download from [flutter.dev](https://flutter.dev/docs/get-started/install)

4. **IDE** (Choose one)
   - VS Code with Flutter and Python extensions
   - Android Studio with Flutter plugin
   - PyCharm Professional

---

### Step 1: Database Setup

#### 1.1 Install Oracle Database
```bash
# Download Oracle Database Express Edition or Standard Edition
# Follow Oracle's installation guide for your operating system
```

#### 1.2 Configure Oracle Connection
```bash
# Set Oracle environment variables
export ORACLE_HOME=/path/to/oracle
export PATH=$ORACLE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
```

#### 1.3 Create Database Schema
```bash
# Connect to Oracle SQL*Plus or SQL Developer
sqlplus SYSTEM/oracle@localhost:1521/ORCLCDB

# Run the SQL scripts in order:
@SQL/Table Creation.sql      # Creates all tables
@SQL/Automate PKs.sql        # Sets up sequences and triggers
@SQL/Data Insertion.sql      # Inserts sample data
@SQL/Views.sql               # Creates database views
```

#### 1.4 Verify Database Setup
```sql
-- Check tables created
SELECT table_name FROM user_tables;

-- Verify data
SELECT COUNT(*) FROM PUser_Account;
SELECT COUNT(*) FROM PBlood_Bank;
SELECT COUNT(*) FROM PBlood_Inventory;
```

---

### Step 2: Backend Setup (FastAPI)

#### 2.1 Navigate to Backend Directory
```bash
cd "Python Backend"
```

#### 2.2 Create Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

#### 2.3 Install Dependencies
```bash
pip install -r requirements.txt
```

**requirements.txt contents:**
```
fastapi==0.100.0
uvicorn[standard]==0.22.0
sqlalchemy==2.0.17
pydantic[email]==1.10.7
cx_Oracle==8.3.0
python-dotenv==1.0.0
email-validator==1.3.1
```

#### 2.4 Configure Database Connection

Create `.env` file in the backend directory:
```env
DB_USERNAME=SYSTEM
DB_PASSWORD=your_oracle_password
DB_DSN=localhost/ORCLCDB
```

Or modify `config.py`:
```python
DB_USERNAME = "SYSTEM"
DB_PASSWORD = "your_password"
DB_DSN = "localhost/ORCLCDB"
```

#### 2.5 Start FastAPI Server
```bash
# Development mode with auto-reload
uvicorn main:app --reload --host 127.0.0.1 --port 8000

# Production mode
uvicorn main:app --host 0.0.0.0 --port 8000
```

#### 2.6 Verify Backend
```bash
# Open browser and navigate to:
http://127.0.0.1:8000/docs       # Swagger UI
http://127.0.0.1:8000/redoc      # ReDoc UI
http://127.0.0.1:8000/           # Welcome message
```

---

### Step 3: Frontend Setup (Flutter)

#### 3.1 Install Flutter
```bash
# Verify Flutter installation
flutter doctor

# If issues found, follow Flutter's guidance to fix
```

#### 3.2 Navigate to Frontend Directory
```bash
cd "Flutter frontend/blood_donation_ui"
```

#### 3.3 Install Flutter Dependencies
```bash
# Get all packages
flutter pub get

# Verify dependencies
flutter pub outdated
```

#### 3.4 Configure API Endpoint

Update the base URL in dashboard files if needed:
```dart
// lib/DashBoards/patient_dashboard.dart (and other dashboards)
final String baseUrl = 'http://127.0.0.1:8000';

// For mobile devices on same network:
// final String baseUrl = 'http://YOUR_IP_ADDRESS:8000';
```

#### 3.5 Run Flutter Application

**For Desktop (macOS):**
```bash
flutter run -d macos
```

**For Web:**
```bash
flutter run -d chrome
```

**For Android:**
```bash
flutter run -d android
```

**For iOS:**
```bash
flutter run -d ios
```

**Build for Production:**
```bash
# macOS
flutter build macos

# Web
flutter build web

# Android APK
flutter build apk

# iOS
flutter build ios
```

---

### Step 4: Verify Installation

#### 4.1 Test Registration
1. Open the Flutter application
2. Click "Register"
3. Fill in user details
4. Select a role (patient, donor, etc.)
5. Complete registration

#### 4.2 Test Login
1. Use registered credentials
2. Select correct role
3. Login should redirect to appropriate dashboard

#### 4.3 Test API Connection
1. From patient dashboard, try checking blood availability
2. From donor dashboard, view donation history
3. Verify data is loading from backend

---

### Troubleshooting

#### Common Issues

**1. Oracle Connection Error**
```
Error: DPI-1047: Cannot locate a 64-bit Oracle Client library
```
**Solution:**
- Install Oracle Instant Client
- Set environment variables correctly
- Verify `LD_LIBRARY_PATH` (Linux/Mac) or `PATH` (Windows)

**2. FastAPI Import Error**
```
ModuleNotFoundError: No module named 'cx_Oracle'
```
**Solution:**
```bash
pip install cx_Oracle
# Or reinstall all dependencies
pip install -r requirements.txt --force-reinstall
```

**3. Flutter Build Error**
```
Error: Couldn't resolve the package 'http'
```
**Solution:**
```bash
flutter clean
flutter pub get
flutter pub upgrade
```

**4. API Connection Refused**
```
SocketException: Connection refused
```
**Solution:**
- Verify FastAPI server is running
- Check firewall settings
- Use correct IP address for mobile devices
- Verify CORS settings in FastAPI

**5. Database Connection Pool Error**
```
ORA-12514: TNS:listener does not currently know of service requested
```
**Solution:**
- Verify Oracle database is running
- Check DSN format in config.py
- Use `tnsping` to test connection
- Verify listener is running

---

## 📡 API Documentation

### Authentication Endpoints

#### Login
```http
POST /users/login
Content-Type: application/json

{
  "username": "string",
  "password": "string",  // SHA-256 hashed
  "role": "patient | donor | medical staff | administrator | organizer"
}

Response: 200 OK
{
  "user_id": 1,
  "message": "Login successful",
  "role": "patient"
}
```

#### Register
```http
POST /users/
Content-Type: application/json

{
  "username": "string",
  "password": "string",  // SHA-256 hashed
  "email": "user@example.com",
  "phone": "1234567890",
  "role": "string"
}

Response: 201 Created
{
  "user_id": 1,
  "message": "User created successfully"
}
```

---

### Patient Endpoints

#### Get Patient Details
```http
GET /patients/patients/{user_id}

Response: 200 OK
{
  "patient_id": 1,
  "user_id": 1,
  "blood_group": "A+",
  "medical_history": "No major conditions"
}
```

#### Update Patient Details
```http
PUT /patients/patients/{user_id}
Content-Type: application/json

{
  "blood_group": "A+",
  "medical_history": "Updated history"
}

Response: 200 OK
{
  "message": "Patient details updated successfully"
}
```

#### Check Blood Availability
```http
POST /patients/blood-availability
Content-Type: application/json

{
  "location": "Downtown Hospital",
  "blood_group": "O+"
}

Response: 200 OK
{
  "location": "Downtown Hospital",
  "blood_group": "O+",
  "units_available": 45,
  "cost_per_unit": 250.00,
  "rare_type": "N"
}
```

#### Get Blood Bank Locations
```http
GET /patients/blood-bank-locations

Response: 200 OK
[
  "Downtown Hospital",
  "City Medical Center",
  "Regional Blood Bank"
]
```

---

### Donor Endpoints

#### Get Donation Camp Locations
```http
GET /donors/donation-camp-locations

Response: 200 OK
[
  "Central Park",
  "Community Center",
  "University Campus"
]
```

#### Get Camps by Location
```http
GET /donors/camps/by-location?location=Central Park

Response: 200 OK
[
  {
    "camp_name": "Summer Blood Drive",
    "camp_date": "2024-12-15"
  }
]
```

#### Get Donations by User
```http
GET /donors/donations/by-user?user_id=1

Response: 200 OK
[
  {
    "donation_id": 1,
    "donation_date": "2024-11-01",
    "units_donated": 1,
    "camp_name": "Summer Blood Drive",
    "camp_date": "2024-11-01",
    "camp_location": "Central Park"
  }
]
```

#### Get Donor Health Info
```http
GET /donors/donor-health/{user_id}

Response: 200 OK
{
  "blood_group": "O+",
  "last_donation_date": "2024-11-01",
  "health_conditions": [
    {
      "condition_name": "Diabetes",
      "condition_details": "Controlled with medication"
    }
  ]
}
```

#### Get Total Donations
```http
GET /donors/donors/by-user/{user_id}/total-donations

Response: 200 OK
{
  "user_id": 1,
  "total_donations": 12
}
```

---

### Medical Staff Endpoints

#### Update Qualification
```http
PUT /medical-staff/{user_id}/qualification
Content-Type: application/json

{
  "new_qualification": "MBBS, MD"
}

Response: 200 OK
{
  "message": "Qualification updated successfully."
}
```

---

### Organizer Endpoints

#### Create Donation Camp
```http
POST /camps/
Content-Type: application/json

{
  "camp_name": "Winter Blood Drive",
  "camp_date": "2024-12-20",
  "camp_location": "City Hall",
  "organizer_id": 1
}

Response: 201 Created
{
  "camp_id": 5,
  "message": "Camp created successfully"
}
```

#### Update Camp Details
```http
PUT /organizer/camps/{camp_id}
Content-Type: application/json

{
  "camp_name": "Updated Camp Name",
  "camp_date": "2024-12-25",
  "camp_location": "New Location"
}

Response: 200 OK
{
  "message": "Camp details updated successfully"
}
```

#### Get Camps by User
```http
GET /organizer/users/{user_id}/camps

Response: 200 OK
[
  {
    "camp_id": 1,
    "camp_name": "Summer Drive",
    "camp_date": "2024-12-15",
    "camp_location": "Central Park"
  }
]
```

#### Add Donor with Health Condition
```http
PUT /organizer/donors/add-with-condition
Content-Type: application/json

{
  "username": "john_doe",
  "blood_group": "A+",
  "condition_name": "Hypertension",
  "condition_detail": "Controlled with medication"
}

Response: 200 OK
{
  "message": "Donor and health condition added successfully.",
  "donor_id": 15
}
```

---

### Blood Inventory Endpoints

#### Get Blood Inventory
```http
GET /blood_inventory/

Response: 200 OK
[
  {
    "inventory_id": 1,
    "bank_id": 1,
    "blood_group": "A+",
    "units_available": 50
  }
]
```

#### Update Inventory
```http
PUT /blood_inventory/{inventory_id}
Content-Type: application/json

{
  "units_available": 75
}

Response: 200 OK
{
  "message": "Inventory updated successfully"
}
```

---

### Views Endpoints

#### Get All Users with Roles
```http
GET /users/roles

Response: 200 OK
[
  {
    "user_id": 1,
    "username": "john_patient",
    "email": "john@email.com",
    "phone": "1234567890",
    "role": "patient"
  }
]
```

#### Get Blood Inventory Details
```http
GET /inventory/details

Response: 200 OK
[
  {
    "inventory_id": 1,
    "bank_name": "Downtown Hospital Blood Bank",
    "bank_location": "123 Main St",
    "blood_group": "A+",
    "cost_per_unit": 250.00,
    "rare_type": "N",
    "units_available": 50
  }
]
```

---

### Error Responses

All endpoints may return the following error responses:

```http
400 Bad Request
{
  "detail": "Invalid input data"
}

401 Unauthorized
{
  "detail": "Invalid credentials"
}

404 Not Found
{
  "detail": "Resource not found"
}

500 Internal Server Error
{
  "detail": "Database error: ORA-00001"
}
```

---

## 📁 Project Structure

```
Blood Donation System/
│
├── Flutter frontend/                   # Flutter mobile/web application
│   └── blood_donation_ui/
│       ├── lib/
│       │   ├── main.dart              # Application entry point
│       │   ├── DashBoards/            # User-specific dashboards
│       │   │   ├── administrator_dashboard.dart
│       │   │   ├── donor_dashboard.dart
│       │   │   ├── medical_staff_dashboard.dart
│       │   │   ├── organizer_dashboard.dart
│       │   │   └── patient_dashboard.dart
│       │   ├── screens/               # Authentication screens
│       │   │   ├── welcome_screen.dart
│       │   │   ├── login_screen.dart
│       │   │   └── registration_screen.dart
│       │   └── services/              # API and session services
│       │       ├── api_service.dart   # HTTP client wrapper
│       │       └── User_Session.dart  # Session management
│       ├── android/                   # Android platform files
│       ├── ios/                       # iOS platform files
│       ├── macos/                     # macOS platform files
│       ├── web/                       # Web platform files
│       ├── windows/                   # Windows platform files
│       └── pubspec.yaml              # Flutter dependencies
│
├── Python Backend/                    # FastAPI backend server
│   ├── main.py                       # FastAPI application entry
│   ├── config.py                     # Database configuration
│   ├── database.py                   # Oracle connection pooling
│   ├── views.py                      # Database view endpoints
│   ├── requirements.txt              # Python dependencies
│   ├── Requests/                     # Business logic endpoints
│   │   ├── patient_requests.py       # Patient-specific operations
│   │   ├── donor_requests.py         # Donor-specific operations
│   │   ├── medical_staff.py          # Medical staff operations
│   │   └── organizer_requests.py     # Organizer operations
│   └── tables/                       # CRUD endpoints for tables
│       ├── user_account.py
│       ├── patient.py
│       ├── donor.py
│       ├── blood_bank.py
│       ├── blood_inventory.py
│       ├── donation_request.py
│       ├── donation.py
│       ├── Donation_Camp.py
│       ├── payment.py
│       └── ... (more table routers)
│
├── SQL/                              # Database scripts
│   ├── Table Creation.sql           # Schema definition
│   ├── Automate PKs.sql             # Sequences and triggers
│   ├── Data Insertion.sql           # Sample data
│   ├── Views.sql                    # Database views
│   ├── Donor.sql                    # Donor-specific procedures
│   ├── pateint.sql                  # Patient-specific procedures
│   ├── medical.sql                  # Medical staff procedures
│   └── organizer.sql                # Organizer procedures
│
├── README.md                         # Project documentation (this file)
└── video.mp4                         # Demo video
```

### Key Directories Explained

#### Frontend Structure
- **DashBoards/**: Contains 5 role-specific dashboards with unique functionality
- **screens/**: Authentication and onboarding screens
- **services/**: API communication and session management utilities

#### Backend Structure
- **Requests/**: High-level business logic endpoints organized by user role
- **tables/**: Low-level CRUD operations for each database table
- **database.py**: Connection pooling for efficient Oracle connections

#### Database Structure
- **Table Creation.sql**: Complete schema with constraints and relationships
- **Automate PKs.sql**: Auto-increment functionality using sequences and triggers
- **Data Insertion.sql**: Sample data for testing and development
- **Views.sql**: Complex queries encapsulated as views

---

## 🔒 Security Features

### 1. Password Security
- **SHA-256 Hashing**: All passwords hashed before storage
- **No Plain Text**: Passwords never stored or transmitted in plain text
- **Client-Side Hashing**: Passwords hashed in Flutter before API call

```dart
String hashPassword(String password) {
  final bytes = utf8.encode(password);
  final digest = sha256.convert(bytes);
  return digest.toString().toUpperCase();
}
```

### 2. Role-Based Access Control (RBAC)
- Each user assigned exactly one role
- Dashboards enforce role-specific permissions
- Backend validates user role before processing requests
- Database constraints ensure role integrity

### 3. Session Management
- Persistent sessions using Flutter's shared_preferences
- User ID and role stored securely on device
- Session cleared on logout
- Session validation on each API request

### 4. Database Security
- **Connection Pooling**: Prevents connection exhaustion attacks
- **Parameterized Queries**: Protection against SQL injection
- **Stored Procedures**: Business logic encapsulated in database
- **Foreign Key Constraints**: Data integrity enforcement
- **Check Constraints**: Data validation at database level

### 5. API Security
- **CORS Configuration**: Controlled cross-origin requests
- **Input Validation**: Pydantic models validate all inputs
- **Error Handling**: Sensitive information not exposed in errors
- **Request Logging**: API activity tracking

### 6. Data Validation
- **Email Validation**: Format checking with email-validator
- **Phone Number Validation**: Length and format constraints
- **Blood Group Validation**: Only valid blood types accepted
- **Positive Integer Checks**: Units, amounts must be positive

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Getting Started
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Make your changes
4. Test thoroughly
5. Commit with descriptive messages (`git commit -m 'Add amazing feature'`)
6. Push to your branch (`git push origin feature/AmazingFeature`)
7. Open a Pull Request

### Code Standards

#### Python (Backend)
- Follow PEP 8 style guide
- Use type hints
- Document functions with docstrings
- Write unit tests for new endpoints

#### Dart (Frontend)
- Follow Dart style guide
- Use meaningful variable names
- Comment complex logic
- Test UI components

#### SQL (Database)
- Use consistent naming conventions
- Add comments for complex queries
- Test procedures before committing
- Document any schema changes

### Testing
- Test all new features locally
- Verify backend endpoints with Postman
- Test Flutter UI on multiple platforms
- Check database migrations

---

## 📄 License

This project is licensed under the MIT License.

---

## 👥 Authors & Acknowledgments

**Development Team:**
- Backend Development: FastAPI and Oracle Database integration
- Frontend Development: Flutter cross-platform UI
- Database Design: Comprehensive schema with stored procedures
- Documentation: Complete system documentation

**Technologies Used:**
- Oracle Database for enterprise-grade data management
- FastAPI for high-performance API development
- Flutter for beautiful cross-platform UI
- cx_Oracle for efficient database connectivity

---

## 📞 Support & Contact

For issues, questions, or contributions:
- Open an issue on GitHub
- Review the troubleshooting section
- Check API documentation at `/docs` endpoint

---

## 🎬 Demo Video

Watch the complete demonstration of the Blood Donation Management System, showcasing all five user dashboards and key features:

<video width="100%" controls>
  <source src="video.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

**Video Contents:**
- System overview and architecture
- User registration and authentication
- Patient dashboard walkthrough
- Donor dashboard features
- Medical staff functionality
- Organizer camp management
- Administrator system controls
- Blood inventory management
- Real-time availability checking
- Donation camp organization
- Payment processing
- Complete user workflow demonstrations

---

## 🚀 Future Enhancements

Potential features for future releases:
- Mobile push notifications for urgent blood requests
- SMS integration for appointment reminders
- Blood donation appointment scheduling
- Real-time chat between donors and organizers
- Analytics dashboard with charts and graphs
- Export functionality for reports (PDF, Excel)
- Multi-language support
- Integration with national blood banks
- Blockchain for donation history transparency
- AI-based donor-patient matching
- Mobile app optimization for offline functionality

---

**Last Updated:** December 4, 2025  
**Version:** 1.0.0  
**Status:** Production Ready

---

*This Blood Donation Management System aims to save lives by making blood donation more accessible, efficient, and transparent for all stakeholders.*
