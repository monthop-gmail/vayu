# VAYU-GUARD Project Plan

## 1. Project Structure

```
vayu-guard/
├── apps/
│   ├── web/                    # Web Application (Dashboard, Back Office, Combat Staff)
│   │   ├── src/
│   │   │   ├── modules/
│   │   │   │   ├── dashboard/      # 1.1 Dashboard Summary
│   │   │   │   ├── backoffice/     # 2. Back Office
│   │   │   │   └── combat-staff/   # 3. Combat Staff
│   │   │   ├── components/
│   │   │   └── lib/
│   │   └── package.json
│   │
│   ├── mobile/                 # 4. Mobile UAV Unit (React Native / Flutter)
│   │   ├── src/
│   │   └── package.json
│   │
│   └── api/                    # API Server
│       ├── src/
│       │   ├── routes/
│       │   ├── services/
│       │   └── middleware/
│       └── package.json
│
├── packages/
│   ├── shared/                 # Shared types, utils
│   └── ai-client/              # AI Server client SDK
│
├── services/
│   ├── streaming/              # Video Streaming Server
│   ├── nvr/                    # NVR Service
│   └── ai/                     # AI Processing Service
│
├── supabase/
│   ├── migrations/
│   └── functions/              # Edge Functions
│
└── docs/
```

## 2. Technology Stack

| Layer | Technology | เหตุผล |
|-------|------------|--------|
| **Frontend** | Next.js 14 + TypeScript | SSR, App Router, รองรับ real-time |
| **Mobile** | React Native / Expo | Cross-platform, share code กับ web |
| **Backend** | Supabase + Edge Functions | Real-time, Auth, Storage ในตัว |
| **Database** | PostgreSQL (Supabase) | PostGIS สำหรับ geolocation |
| **Streaming** | WebRTC / HLS | Low latency video streaming |
| **AI** | Python + FastAPI | ML model serving, OpenCV |
| **Maps** | Mapbox / Leaflet | แสดงเส้นทางบิน, Ortho overlay |
| **Real-time** | Supabase Realtime | WebSocket สำหรับ live updates |

## 3. Database Schema

```sql
-- Organizations & Users
organizations (id, name, parent_id, created_at)
users (id, email, name, role, organization_id)
permissions (id, user_id, resource, action)

-- UAV Management
uavs (id, name, model, serial_number, status, organization_id, specs)
uav_assignments (id, uav_id, operator_id, unit_id, assigned_at)

-- Flight Operations
flight_plans (id, name, mission_type, waypoints, created_by, status)
flights (id, flight_plan_id, uav_id, pilot_id, start_time, end_time, status)
flight_logs (id, flight_id, lat, lng, altitude, heading, timestamp)

-- Target & Detection
targets (id, name, description, last_known_location, priority, status)
detections (id, flight_id, target_id, confidence, bbox, timestamp, video_clip_url)

-- Streaming & Recording
streams (id, flight_id, url, status)
recordings (id, flight_id, file_path, start_time, end_time, size)
```

## 4. API Endpoints

```
# Authentication
POST   /auth/login
POST   /auth/logout
GET    /auth/me

# Back Office
GET    /api/organizations
POST   /api/organizations
GET    /api/users
POST   /api/users
PUT    /api/users/:id/permissions

GET    /api/uavs
POST   /api/uavs
PUT    /api/uavs/:id
POST   /api/uavs/:id/assign

# Combat Staff
GET    /api/flight-plans
POST   /api/flight-plans
PUT    /api/flight-plans/:id
POST   /api/flight-plans/:id/execute

GET    /api/targets
POST   /api/targets
PUT    /api/targets/:id

GET    /api/streams/:flight_id
GET    /api/recordings/:flight_id

# Mobile UAV Unit
GET    /api/mobile/flight-plan
POST   /api/mobile/flight-status
POST   /api/mobile/telemetry
POST   /api/mobile/stream/start

# AI Processing
POST   /api/ai/analyze
GET    /api/detections/:flight_id
```

## 5. Development Phases

| Phase | Module | งานหลัก |
|-------|--------|---------|
| **Phase 1** | Foundation | Supabase setup, Auth, Database schema, Base UI |
| **Phase 2** | Back Office | CRUD หน่วยงาน, ผู้ใช้, สิทธิ์, UAV inventory |
| **Phase 3** | Combat Staff | แผนการบิน, นำเข้าเป้าหมาย, Map integration |
| **Phase 4** | Mobile Unit | App สำหรับ operator, รับแผนการบิน, telemetry |
| **Phase 5** | Streaming | WebRTC/HLS streaming, NVR integration |
| **Phase 6** | AI | Object detection, target matching, JSON output |
| **Phase 7** | Dashboard | Real-time monitoring, รายงาน, PDF export |
| **Phase 8** | Integration | End-to-end testing, Performance tuning |

## 6. Next Steps

1. สร้าง Supabase Project - Setup database และ auth
2. Initialize monorepo - ใช้ Turborepo หรือ Nx
3. สร้าง Database migrations - ใช้ schema ที่ออกแบบไว้
4. พัฒนา Back Office ก่อน - เป็นฐานข้อมูลหลัก
