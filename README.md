# TheSpot Mechanic 🚗🔧🛡️

> **Breakdown Anywhere. Mechanic Everywhere.**  
> A full-stack, responsive on-demand roadside vehicle repair platform built with React, TypeScript, Firebase, Leaflet (Free OpenStreetMap & OSRM Routing), Express, and Razorpay.

---

## 🌟 Architecture & Key Features

1. **User (Motorist) Portal**:
   - Live GPS breakdown detection with fallback to interactive manual map-pin placement and reverse geocoding.
   - Vehicle management (Two-Wheelers & Four-Wheelers with model & license plate tracking).
   - Problem diagnosis with emergency kit part matching (Puncture, Battery, Engine, Brake, Chain, Spark Plug, Towing).
   - Real-time nearby mechanic radar map with search radius filters (2 km to 25 km).
   - Broadcast to eligible nearby mechanics or direct booking of a selected workshop.
   - **Active Request Live HUD**:
     - Status progression stepper (`Searching -> Accepted -> On The Way -> Arrived -> Repairing -> Awaiting Payment -> Completed`).
     - Real-time GPS tracking of the dispatched mechanic via Firebase Realtime Database.
     - Dynamic driving route and live ETA calculation using the free OSRM routing engine.
     - In-app real-time encrypted messaging with the assigned mechanic.
     - Itemized invoice review and secure online payment via Razorpay.
     - 5-Star rating and review system with interactive confetti celebration.
     - Printable & downloadable formal roadside assistance tax invoice receipt.

2. **Mechanic Terminal**:
   - Workshop management with online/offline availability toggle synchronized to Realtime Database presence.
   - Live incoming roadside calls feed matching shop service radius and vehicle capabilities.
   - **Concurrency-safe atomic job claiming**: prevents race conditions or double assignment.
   - Field operations HUD with "Share Live GPS" streaming browser geolocation fixes.
   - Workflow step management: `Accepted -> On The Way -> Arrived -> Repairing -> Submit Final Invoice -> Completed`.
   - Custom invoice builder allowing mechanics to specify labor charges and parts costs.
   - One-click GPS turn-by-turn navigation opening directions to the customer's exact coordinates.
   - Workshop earnings & payout analytics (90% mechanic net payout after 10% platform fee).

3. **Admin Console**:
   - Real-time platform KPI dashboard: Total Requests, Completed Jobs, Verified Shops ratio, Platform Revenue collected.
   - Workshop verification and compliance governance: Approve, Reject, and Suspend workshops.
   - All repair requests monitor with real-time filtering and administrative intervention.
   - Immutable security audit logs tracking admin actions.

---

## 🆓 Free API Stack (Zero Billing / No Credit Card Required)

As mandated, TheSpot Mechanic defaults to **100% free open-source mapping and routing services**:
- **Interactive Maps**: [Leaflet.js](https://leafletjs.com/) with free [OpenStreetMap](https://www.openstreetmap.org/) cartography.
- **Turn-by-Turn Driving Routes & Polyline**: Free public [OSRM (Open Source Routing Machine)](https://project-osrm.org/) API (`https://router.project-osrm.org/route/v1/driving/...`).
- **Human-Readable Reverse Geocoding**: Free [OpenStreetMap Nominatim](https://nominatim.org/) API (`https://nominatim.openstreetmap.org/reverse?...`).
- **Real-Time GPS Tracking**: Firebase Realtime Database (`/locations/{requestId}`).

*(Optional Google Maps integration instructions are provided in section 6 below if a user wishes to switch to Google Maps Platform).*

---

## 🚀 Quick Start & Installation

### 1. Prerequisites
- Node.js v18+ (tested on Node v24.15.0)
- npm v9+

### 2. Setup
```bash
# Clone or navigate to the project directory
cd c:/Users/Admin/Downloads/spotmechanic

# Dependencies are already installed, or run:
npm install
```

### 3. Environment Variables
A `.env` and `.env.example` file are pre-configured with the project's Firebase credentials:
```env
# Firebase Project (spotmechanic-bb7ab)
VITE_FIREBASE_API_KEY=AIzaSyBIBxJsoABAx6fuuryYoXB14fPubQ7EovA
VITE_FIREBASE_AUTH_DOMAIN=spotmechanic-bb7ab.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=spotmechanic-bb7ab
VITE_FIREBASE_STORAGE_BUCKET=spotmechanic-bb7ab.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=595783414372
VITE_FIREBASE_APP_ID=1:595783414372:web:a3a8b8cc8c50dc6d95a571
VITE_FIREBASE_MEASUREMENT_ID=G-G6LV5XTGSP
VITE_FIREBASE_DATABASE_URL=https://spotmechanic-bb7ab-default-rtdb.firebaseio.com

# Razorpay Test Configuration
RAZORPAY_KEY_ID=rzp_test_spotmechanic2026
RAZORPAY_KEY_SECRET=spot_sec_99x88q77p66
PORT=5000
```

### 4. Running the Application
Run both the React Vite frontend and the secure Express backend simultaneously:
```bash
npm start
```
- **Frontend**: `http://localhost:3000`
- **Backend API**: `http://localhost:5000`

---

## 🧪 Automated Test Suites

The platform includes automated tests covering request concurrency, payment verification, and security rules:

```bash
# 1. Test Atomic Concurrency (Simulates two mechanics accepting the same request simultaneously)
npm run test:concurrency

# 2. Test Razorpay HMAC SHA256 Signature Verification & Tamper Detection
npm run test:payments

# 3. Test Role-Based Access Control (RBAC) Security Rules
npm run test:rules
```

**Test Results**:
```
✓ Mechanic [shop_sri_murugan] successfully claimed the job.
✓ Competing mechanic was correctly rejected: "This repair job has already been claimed by another mechanic."
🎉 Concurrency Test PASSED: No duplicate assignments or race condition vulnerabilities!

✓ Genuine Razorpay HMAC SHA256 signature verified successfully.
✓ Spoofed / fake signature was blocked successfully.
✓ Tampered transaction reference was caught and rejected.
✓ Financial math & 10% platform fee accounting verified.
🎉 Payment Security Tests PASSED!

✓ User can read their own repair request.
✓ Cross-user request snooping is blocked.
✓ Assigned mechanic can access active assigned job.
✓ Mechanics can access open broadcast requests.
✓ Direct client mutation of payment collection is strictly blocked.
🎉 Security Rules Policy Tests PASSED!
```

---

## 👥 Instant Test Accounts & Demo Workflow

Click **"Sign In"** in the top navbar to reveal 1-click demo evaluation buttons:
- **Motorist**: Rahul Sharma (`user.demo@thespotmechanic.com`)
- **Mechanic**: Sri Murugan Garage (`mechanic.murugan@thespotmechanic.com`)
- **Admin**: TheSpot Admin (`admin.super@thespotmechanic.com`)

### Recommended E2E Testing Walkthrough:
1. **Motorist Portal**:
   - Review the GPS breakdown location or click "Pin on Map".
   - Select vehicle & problem (e.g., "Puncture / Flat Tyre").
   - Click **"Broadcast Request to 4 Nearby Shops"**.
   - Notice the status transition to `Broadcasting to Mechanics...`.
2. **Mechanic Terminal**:
   - Switch to the "Mechanic Terminal" tab.
   - Under "Incoming Roadside Calls", view the live SOS and click **"Accept Job"**.
   - Click **"Share Live GPS"** to begin broadcasting continuous coordinates.
   - Step through the repair status: `Mark: On The Way` -> `Mark: Arrived at Location` -> `Mark: Repairing Vehicle`.
   - Click **"Submit Final Invoice & Bill"** (enter labor and parts, e.g. ₹400).
   - Click **"Send Invoice to Customer"**.
3. **Motorist Portal**:
   - Switch back to the "Motorist Portal" tab.
   - Review the live invoice and click **"Pay ₹400 Securely Online"**.
   - Complete checkout via Razorpay test gateway.
   - Enjoy the confetti celebration and rating prompt!
   - Click **"View Receipt"** to view and print the formal invoice.
4. **Admin Console**:
   - Switch to the "Admin Console" tab.
   - View updated KPI counters, completed repairs volume, and collected platform fees.
   - Toggle workshop verification states (Approve / Suspend).

---

## 🔒 Security & Firebase Rules

### Firestore Collections
- `/users/{userId}`: User profiles & saved vehicles.
- `/shops/{shopId}`: Verified mechanic workshops, kit parts, and ratings.
- `/requests/{requestId}`: Roadside jobs with concurrency-guarded status updates.
- `/requests/{requestId}/messages/{messageId}`: Real-time chat messages between motorist and mechanic.
- `/payments/{paymentId}`: Cryptographically verified transactions.
- `/auditLogs/{logId}`: Immutable security audit trail.

### Deploying Rules to Firebase
```bash
# Deploy Firestore, Realtime Database, and Cloud Storage security rules:
firebase deploy --only firestore:rules,database,storage
```

---

## 🗺️ Google Maps Platform Setup (Optional Alternative)

If you prefer using Google Maps JavaScript API instead of the default free Leaflet/OSM stack:
1. Go to [Google Cloud Console](https://console.cloud.google.com/) and enable:
   - Maps JavaScript API
   - Places API (New)
   - Directions API
2. Create an API key with HTTP referrer restrictions matching your domain (e.g. `localhost:3000/*` and your production URL).
3. Add `VITE_GOOGLE_MAPS_API_KEY=YOUR_KEY` to `.env`.
4. The system is designed with a pluggable map adapter interface to seamlessly swap between Leaflet and Google Maps.

---

## 📂 Project Structure

```
spotmechanic/
├── .env                         # Active environment variables
├── .env.example                 # Template environment variables
├── firestore.rules              # Production Cloud Firestore security rules
├── database.rules.json          # Production Realtime Database security rules
├── storage.rules                # Production Cloud Storage security rules
├── package.json                 # Dependencies & scripts
├── tsconfig.json                # TypeScript compiler configuration
├── vite.config.ts               # Vite configuration with API proxy
├── index.html                   # HTML entry point with fonts & Razorpay SDK
├── server/
│   └── index.ts                 # Express backend: atomic acceptance & payment verification
├── src/
│   ├── main.tsx                 # React app root
│   ├── App.tsx                  # Unified dashboard with User, Mechanic, and Admin tabs
│   ├── index.css                # Responsive Orange-branded design system & dark mode
│   ├── config/
│   │   ├── firebase.ts          # Firebase Modular SDK initialization
│   │   └── constants.ts         # Breakdown categories, kit parts, fee configuration
│   ├── types/
│   │   └── index.ts             # TypeScript interfaces for full data model
│   ├── services/
│   │   ├── authService.ts       # Firebase Auth & demo accounts
│   │   ├── shopService.ts       # Firestore shops, geospatial distance filtering
│   │   ├── requestService.ts    # Concurrency-safe transactions & status progression
│   │   ├── trackingService.ts   # Realtime Database live GPS streaming & presence
│   │   ├── routingService.ts    # Free OSRM road routing & Nominatim geocoding
│   │   ├── chatService.ts       # Real-time job messaging
│   │   ├── paymentService.ts    # Razorpay checkout & cryptographic verification
│   │   └── adminService.ts      # Workshop verification, stats & audit logs
│   └── components/
│       ├── Navbar.tsx           # Brand header, role indicator, theme switcher
│       ├── Map/
│       │   ├── LeafletMap.tsx   # Free OSM map with live moving markers & route lines
│       │   └── ManualPinModal.tsx # Manual breakdown pin picker
│       ├── User/
│       │   ├── BreakdownForm.tsx # Vehicle, issue, photo & location picker
│       │   ├── ShopList.tsx      # Ranked workshops list & broadcast button
│       │   ├── ActiveRequestHUD.tsx # Live tracking HUD, Razorpay pay & ratings
│       │   └── SavedVehiclesModal.tsx # Vehicle management
│       ├── Mechanic/
│       │   ├── ShopProfileCard.tsx # Workshop controls & parts inventory toggles
│       │   ├── IncomingRequestsFeed.tsx # Live broadcast incoming calls & claim button
│       │   ├── ActiveJobCard.tsx # Field control HUD, live GPS stream, invoice builder
│       │   └── EarningsCard.tsx  # Workshop revenue & completed jobs history
│       ├── Admin/
│       │   ├── AdminStats.tsx    # Platform KPI counters
│       │   ├── ShopsManagement.tsx # Workshop verification & suspension controls
│       │   ├── RequestsMonitor.tsx # All requests filter table & cancel controls
│       │   └── DisputesAuditLogs.tsx # Immutable administrative audit trail
│       └── Common/
│           ├── AuthModal.tsx     # Sign In / Sign Up with 1-click test accounts
│           ├── ChatModal.tsx     # In-app messaging modal
│           └── ReceiptModal.tsx  # Printable tax invoice receipt
└── tests/
    ├── concurrency.test.ts      # Double-assignment race condition test
    ├── payments.test.ts         # HMAC SHA256 payment signature verification test
    └── rules.test.ts            # RBAC security rules test
```

---

## 📜 Original Prototype Preservation

The original prototype HTML file (`TheSpot Mechanic.html`) has been strictly preserved in the root directory. All of its original problem types, parts kits, status stages, and shop profiles (`Sri Murugan Garage`, `Ravi Two-Wheeler Care`, `Highway Auto Works`, etc.) have been preserved, elevated, and connected to persistent cloud databases.
