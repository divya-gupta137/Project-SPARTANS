# SPARTANS - Soldier Health Monitoring System

SPARTANS is an advanced real-time soldier health monitoring and position tracking system designed for military operations. It integrates IoT sensor data from deployed soldiers with a professional command center interface, enabling commanders to monitor vital signs, track GPS locations, manage critical alerts, and communicate with field personnel via Telegram.

**Live Demo:** [https://v0-soldier-monitoring-system.vercel.app](https://v0-soldier-monitoring-system.vercel.app)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Project Workflow](#project-workflow)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Endpoints](#api-endpoints)
- [Usage Guide](#usage-guide)

---

## Overview

SPARTANS is a comprehensive command-and-control platform that provides real-time monitoring of deployed soldiers with the following capabilities:

- **Real-time Vital Signs Monitoring**: Heart rate, body temperature, blood oxygen, battery level, and humidity
- **GPS Location Tracking**: Precise soldier positioning on interactive maps
- **Critical Alert System**: Automatic alerts when vital signs exceed safe thresholds
- **Secure Communication**: Telegram-based messaging with email notifications
- **Professional Dashboard**: Intuitive interface for rapid decision-making
- **Historical Data Analysis**: Chart-based pulse trend analysis
- **Multi-soldier Management**: Monitor multiple soldiers simultaneously

---

## Features

### 1. Authentication & Security
- Secure login/signup with password strength validation
- Password policy enforcement (minimum 8 characters with uppercase, lowercase, numbers)
- Profile management with user details and roles
- Session management with logout confirmation

### 2. Real-time Soldier Monitoring
- **Dashboard**: Displays all deployed soldiers with live status indicators
- **Searchable List**: Filter soldiers by name, rank, or status (active/inactive/critical)
- **Individual Soldier Details**: Comprehensive vital signs and location information
- **Auto-Status Detection**: Soldiers marked inactive after 1 minute of no data from IoT sensors
- **Status Colors**: Green (normal), Yellow (warning), Red (critical)

### 3. IoT Data Integration
- **ThingSpeak Cloud Integration**: Real-time data fetching from IoT sensors
- **Sensor Fields**: Pulse (bpm), Temperature (°C), GPS (latitude/longitude), Battery (%), Humidity (%)
- **Automatic Data Refresh**: Updates every 2 seconds for live monitoring
- **Critical Value Thresholds**: Automatic alerts for dangerous vital signs

### 4. Critical Alert Management
- **Alert Panel**: Real-time critical alerts with soldier information
- **Alert Details**: Timestamp, vital reading, soldier ID, and status
- **Alert Resolution**: Mark alerts as acknowledged from dashboard
- **Expandable Cards**: Detailed alert information on demand
- **Unread Badges**: Visual notification of new alerts

### 5. Telegram Bot Integration
- **Message Receiving**: Soldiers send messages via Telegram bot
- **Message Storage**: Secure server-side storage in-memory
- **Web Inbox**: Messages displayed in website with read/unread status
- **Email Notifications**: Automatic Gmail notifications with soldier vitals
- **Response System**: Send replies from website back to Telegram
- **Soldier Mapping**: Automatic linking of Telegram username to soldier profile

### 6. Data Visualization
- **Historical Pulse Charts**: 50-reading trend analysis
- **Chronological Display**: Oldest readings on left, newest on right
- **Interactive Charts**: Hover details, zoom, and analysis
- **Vital Status Indicators**: Color-coded health metrics

### 7. Professional Dashboard Layout
- **Header**: Logo, soldier search, alerts, messages, profile, logout
- **Sidebar**: Soldiers list with real-time status
- **Main Panel**: Detailed view of selected soldier
- **Responsive Design**: Works on all screen sizes

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   IoT LAYER                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │  DHT22 Sensor│  │ GPS Module   │  │ Heart Rate   │   │
│  │ (Temp/Humid)│  │ (Location)   │  │ Sensor       │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│              THINGSPEAK CLOUD PLATFORM                   │
│  Channel: 3159678 | Read API: WPVV0KMER5BVWIB4         │
│  (Field1: Pulse, Field2: Temp, Field3: Lat, Field4: Lng)│
└─────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│              BACKEND LAYER (Next.js APIs)               │
│  ┌─────────────────────────────────────────────────┐   │
│  │ /api/soldiers          - Get all soldiers       │   │
│  │ /api/soldiers/[id]     - Get soldier detail     │   │
│  │ /api/alerts            - Get critical alerts    │   │
│  │ /api/telegram/webhook  - Receive bot messages   │   │
│  │ /api/telegram/send     - Send replies to Telegram│  │
│  │ /api/telegram/messages - Get stored messages    │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│  Frontend Layer  │ │ Telegram Bot API │ │ Gmail SMTP       │
│ (React/Next.js)  │ │  (Bot Tokens)    │ │ (Email Alerts)   │
└──────────────────┘ └──────────────────┘ └──────────────────┘
        │                   │                   │
        └───────────────────┴───────────────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ User Interface   │
                   │ - Dashboard      │
                   │ - Soldier List   │
                   │ - Messages       │
                   │ - Profile        │
                   └──────────────────┘
```

---

## Project Workflow

### 1. User Journey

```
START
  │
  ├─► Animated SPARTANS Logo (5 seconds)
  │
  ├─► Home Page
  │   ├─► Learn More (smooth scroll to features)
  │   ├─► Sign Up
  │   │   ├─► Email
  │   │   ├─► Password (with strength indicator)
  │   │   ├─► Confirm Password
  │   │   ├─► Role Selection
  │   │   └─► Create Account
  │   │
  │   └─► Login
  │       ├─► Email
  │       ├─► Password
  │       └─► Sign In
  │
  ├─► Dashboard
  │   ├─► Soldiers List (searchable)
  │   │
  │   ├─► Click Soldier
  │   │   ├─► Vital Signs Display
  │   │   ├─► GPS Location Map
  │   │   ├─► Pulse Analysis Chart
  │   │   └─► Unit Information
  │   │
  │   ├─► Messages
  │   │   ├─► Inbox
  │   │   ├─► View Message
  │   │   └─► Reply to Soldier
  │   │
  │   ├─► Alerts
  │   │   ├─► Critical Alert List
  │   │   └─► Resolve Alert
  │   │
  │   ├─► Profile
  │   │   ├─► View User Info
  │   │   └─► Security Status
  │   │
  │   └─► Logout (with confirmation)
  │       └─► Back to Home & Landing
  │
  END
```

### 2. Data Flow: IoT to Dashboard

```
ESP32 + DHT22
    │
    ├─ Read Sensors (every 10 seconds)
    │   ├─ Temperature (°C)
    │   ├─ Humidity (%)
    │   └─ GPS Coordinates
    │
    └─► Send to ThingSpeak
            │
            ├─ Field 1: Pulse (bpm)
            ├─ Field 2: Temperature (°C)
            ├─ Field 3: Latitude
            ├─ Field 4: Longitude
            ├─ Field 5: Battery (%)
            └─ Field 6: Humidity (%)
                │
                └─► SPARTANS Backend
                        │
                        ├─ Fetch latest reading
                        ├─ Calculate health status
                        ├─ Generate alerts if critical
                        └─ Store in memory
                            │
                            └─► Dashboard Auto-Updates
                                    │
                                    ├─ Refresh every 2 seconds
                                    ├─ Update soldier vitals
                                    ├─ Change status color
                                    └─ Trigger alerts
```

### 3. Alert Generation Logic

```
Vital Reading Received
    │
    ├─ Check Pulse
    │   ├─ < 40 or > 120 bpm? ─► CRITICAL ALERT
    │   ├─ 50-60 or 100-110 bpm? ─► WARNING
    │   └─ 60-100 bpm? ─► NORMAL
    │
    ├─ Check Temperature
    │   ├─ > 39°C or < 35°C? ─► CRITICAL ALERT
    │   ├─ 37.5-39°C? ─► WARNING
    │   └─ 36.5-37.5°C? ─► NORMAL
    │
    ├─ Check Blood Oxygen
    │   ├─ < 90%? ─► CRITICAL ALERT
    │   ├─ 90-95%? ─► WARNING
    │   └─ > 95%? ─► NORMAL
    │
    └─ Check Data Freshness
        ├─ No data for > 1 minute? ─► MARK AS INACTIVE
        └─ New data received? ─► MARK AS ACTIVE
```

### 4. Telegram Bot Integration Workflow

```
Soldier Sends Message via Telegram
    │
    └─► Telegram API
        └─► Webhook Endpoint: /api/telegram/webhook
            │
            ├─ Parse Message
            │   ├─ Extract Username (@Parv_M)
            │   ├─ Extract Message Text
            │   └─ Get Current Timestamp
            │
            ├─ Fetch Soldier Data
            │   ├─ Get Latest Vitals from ThingSpeak
            │   ├─ Get Soldier Details
            │   └─ Calculate Health Status
            │
            ├─ Store in Memory
            │   └─ Save with unread status
            │
            ├─ Send Email Notification
            │   └─ To: spartans01project@gmail.com
            │       Subject: New Message from [Soldier]
            │       Body: Vitals + Message Text
            │
            └─ Appear in Website Inbox
                │
                └─► Commander Views Message
                    └─► Types Reply
                        └─► Clicks Send
                            │
                            └─► /api/telegram/send
                                ├─ Send message via Telegram Bot API
                                ├─ Update message status to "replied"
                                └─ Close message in UI
```

### 5. Status Update Logic

```
Every 2 Seconds:
    │
    ├─ Fetch all soldiers from /api/soldiers
    │   │
    │   ├─ Check data freshness
    │   │   ├─ Last update < 1 minute? ─► Status = "active"
    │   │   └─ Last update > 1 minute? ─► Status = "inactive"
    │   │
    │   └─ Check vitals thresholds
    │       ├─ Critical values? ─► Status = "critical"
    │       ├─ Warning values? ─► Status = "warning"
    │       └─ Normal values? ─► Status = "active"
    │
    ├─ Update Dashboard UI
    │   ├─ Color-code soldiers in list
    │   ├─ Update selected soldier details
    │   ├─ Refresh charts (if open)
    │   └─ Trigger alerts (if any)
    │
    └─ Maintain Selection
        └─ Keep previously selected soldier selected
```

---

## Tech Stack

### Frontend
- **Next.js 16** - React framework with App Router
- **TypeScript** - Type-safe code
- **Tailwind CSS v4** - Utility-first styling with dark theme
- **Recharts** - Data visualization
- **shadcn/ui** - Component library
- **React Hooks** - State management

### Backend
- **Next.js API Routes** - Serverless backend
- **Node.js** - JavaScript runtime
- **Nodemailer** - Email service with Gmail SMTP
- **In-Memory Storage** - Message and data persistence

### External Services
- **ThingSpeak** - IoT cloud platform for sensor data
- **Telegram Bot API** - Messaging and notifications
- **Gmail SMTP** - Email notifications
- **Vercel** - Hosting and deployment

### Hardware (IoT)
- **ESP32** - Microcontroller with WiFi
- **DHT22** - Temperature and humidity sensor
- **NEO-6M GPS** - GPS positioning module
- **Max30100** - Heart rate pulse sensor

---

## Installation

### Prerequisites
- Node.js 18+ and npm
- Git
- Telegram Bot Token (from @BotFather)
- Gmail App Password
- ThingSpeak Channel with Read API Key

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/theparvmahajan/v0-soldier-monitoring-system.git
   cd v0-soldier-monitoring-system
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create `.env.local` file**
   ```env
   NEXT_PUBLIC_APP_URL=https://your-vercel-app-url
   ```

4. **Run development server**
   ```bash
   npm run dev
   ```

5. **Open in browser**
   ```
   http://localhost:3000
   ```

---

## Configuration

### ThingSpeak Setup

1. Create channel on ThingSpeak.com
2. Configure fields:
   - Field 1: Pulse (bpm)
   - Field 2: Temperature (°C)
   - Field 3: Latitude
   - Field 4: Longitude
   - Field 5: Battery (%)
   - Field 6: Humidity (%)
3. Get Read API Key
4. Update `/lib/thingspeak.ts`:
   ```typescript
   const CHANNEL_ID = "YOUR_CHANNEL_ID"
   const READ_API_KEY = "YOUR_READ_API_KEY"
   ```

### Telegram Bot Setup

1. Message @BotFather on Telegram
2. Create new bot with `/newbot`
3. Copy bot token
4. Update `/app/api/telegram/webhook/route.ts`:
   ```typescript
   const TELEGRAM_BOT_TOKEN = "YOUR_BOT_TOKEN"
   ```
5. Set webhook by visiting:
   ```
   https://your-app-url/api/telegram/setup
   ```

### Gmail Configuration

1. Go to Google Account settings
2. Enable 2-Step Verification
3. Generate App Password
4. Update `/lib/email-service.tsx`:
   ```typescript
   const EMAIL_ADDRESS = "your-gmail@gmail.com"
   const APP_PASSWORD = "your-16-char-app-password"
   ```

---

## API Endpoints

### Soldiers
- `GET /api/soldiers` - Get all soldiers with vitals
- `GET /api/soldiers/[id]` - Get specific soldier details
- `GET /api/soldiers/[id]/history` - Get pulse history for chart

### Alerts
- `GET /api/alerts` - Get all critical alerts
- `POST /api/alerts/[id]/resolve` - Mark alert as resolved

### Telegram
- `POST /api/telegram/webhook` - Receive messages from Telegram
- `GET /api/telegram/setup` - Configure webhook
- `GET /api/telegram/messages` - Get stored messages
- `POST /api/telegram/send` - Send reply to Telegram

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/signup` - User registration

---

## Usage Guide

### Login to Dashboard
1. Enter email and password
2. Select user role (Field Commander, Medical Officer, System Operator)
3. Click "Sign In"

### Monitor Soldiers
1. View soldiers list on sidebar
2. Click soldier name to view details
3. Check vital signs indicators:
   - **Green**: Normal
   - **Yellow**: Warning
   - **Red**: Critical
4. View GPS location on map

### Analyze Pulse Trends
1. Click "Analyze" button near pulse reading
2. View 50-reading historical chart
3. Close dialog by clicking outside or Close button

### Manage Messages
1. Click "Messages" in header
2. View all incoming soldier messages
3. Click message to select
4. Type reply and click "Send Reply"
5. Check Telegram for response

### Handle Alerts
1. Click alerts bell icon
2. View all critical alerts
3. Click "Resolve" to acknowledge
4. Alert disappears from list

### Profile & Settings
1. Click avatar in top right
2. View your profile information
3. See security status and join date

---

## Military Status Indicators

| Status | Color | Meaning |
|--------|-------|---------|
| Active | Green | Normal operation, all vitals within range |
| Warning | Yellow | One or more vitals approaching critical levels |
| Critical | Red | One or more vitals exceed safe thresholds |
| Inactive | Gray | No data from soldier for > 1 minute |

---

## Vital Sign Thresholds

| Vital | Normal | Warning | Critical |
|-------|--------|---------|----------|
| Pulse | 60-100 bpm | 50-60 or 100-110 bpm | <40 or >120 bpm |
| Temp | 36.5-37.5°C | 37.5-39°C | >39°C or <35°C |
| O₂ | >95% | 90-95% | <90% |
| Battery | >20% | 10-20% | <10% |

---

**SPARTANS - Protecting Our Heroes with Technology**
