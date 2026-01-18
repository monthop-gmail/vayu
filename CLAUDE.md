# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

VAYU-GUARD - ระบบควบคุมและจัดการ UAV (Unmanned Aerial Vehicle) สำหรับภารกิจค้นหาเป้าหมายและตรวจการณ์ชายแดน

## MCP Servers

โปรเจกต์นี้ใช้ MCP servers:
- **SSH**: เชื่อมต่อกับ remote server
- **Supabase**: ฐานข้อมูลและ backend services
- **GitHub**: จัดการ repository

## Technology Stack

| Layer | Technology |
|-------|------------|
| Frontend | Next.js 14 + TypeScript |
| Mobile | React Native / Expo |
| Backend | Supabase + Edge Functions |
| Database | PostgreSQL (Supabase) + PostGIS |
| Streaming | WebRTC / HLS |
| AI | Python + FastAPI |
| Maps | Mapbox / Leaflet |

## Architecture

```
vayu-guard/
├── apps/
│   ├── web/           # Dashboard, Back Office, Combat Staff (Next.js)
│   ├── mobile/        # Mobile UAV Unit (React Native)
│   └── api/           # API Server
├── packages/
│   ├── shared/        # Shared types, utils
│   └── ai-client/     # AI Server client SDK
├── services/
│   ├── streaming/     # Video Streaming Server
│   ├── nvr/           # NVR Service
│   └── ai/            # AI Processing (Python/FastAPI)
└── supabase/
    ├── migrations/
    └── functions/     # Edge Functions
```

### Modules
- **Dashboard** - รายงานผลและสรุปข้อมูล (ผู้บริหาร, ผบ.หน่วย)
- **Back Office** - จัดการข้อมูลพื้นฐาน, สิทธิ์, UAV inventory
- **Combat Staff** - แผนการบิน, นำเข้าเป้าหมาย, ตรวจสอบ VDO (Commander/Operator)
- **Mobile UAV Unit** - บินอัตโนมัติตามแผน

## Data Flow

1. Combat Staff สร้างแผนการบิน → Web Service
2. Web Service ส่งแผนไปยัง Mobile UAV Unit
3. UAV บินและ Streaming video
4. AI วิเคราะห์ video → ส่ง JSON กลับ (flight_id, target_id, timestamps)
5. ผลลัพธ์แสดงบน Dashboard

## Database Tables

```
organizations, users, permissions
uavs, uav_assignments
flight_plans, flights, flight_logs
targets, detections
streams, recordings
```

## Development

ดูรายละเอียดเพิ่มเติมใน `docs/project-plan.md`
