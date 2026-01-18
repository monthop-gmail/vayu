# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

VAYU-GUARD - ระบบควบคุมและจัดการ UAV (Unmanned Aerial Vehicle) สำหรับภารกิจค้นหาเป้าหมายและตรวจการณ์ชายแดน

## MCP Servers

โปรเจกต์นี้ใช้ MCP servers ดังนี้:
- **SSH**: เชื่อมต่อกับ server ระยะไกล (thaisummary.com)
- **Supabase**: ฐานข้อมูลและ backend services
- **GitHub**: จัดการ repository และ version control

## Architecture

```
VAYU-GUARD Server
├── Web Service (API endpoints)
├── API Server (business logic)
├── NVR (video recording)
└── AI Processing (target detection)

Modules:
├── Dashboard - รายงานผลและสรุปข้อมูล
├── Back Office - จัดการข้อมูลพื้นฐาน, สิทธิ์, UAV inventory
├── Combat Staff - แผนการบิน, นำเข้าเป้าหมาย, ตรวจสอบ VDO
└── Mobile UAV Unit - บินอัตโนมัติตามแผน
```

## Data Flow

1. Combat Staff สร้างแผนการบิน → Web Service
2. Web Service ส่งแผนไปยัง Mobile UAV Unit
3. UAV บินและ Streaming video
4. AI วิเคราะห์ video → ส่ง JSON กลับ (flight_id, target_id, timestamps)
5. ผลลัพธ์แสดงบน Dashboard

## User Roles

- **ผู้บริหาร / ผบ.หน่วย**: ดู Dashboard, เข้าถึง C2
- **Commander / Operator**: ควบคุมผ่าน Combat Staff, กำหนดแผนการบิน
- **ผู้ปฏิบัติ**: ดำเนินการตามสิทธิ์ที่ได้รับ
