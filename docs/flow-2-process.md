# Flow #2 - Process / Data Flow Diagram

แสดงขั้นตอนการทำงานและการไหลของข้อมูล (ข้อมูลไหลอย่างไร)

```mermaid
graph TD

%% =======================
%% VAYU-GUARD SERVER
%% =======================
subgraph L1["VAYU-GUARD Server"]
    WS1[Web Service]
    API[API Server]
    NVR[NVR]

    FIND[ค้นหาเป้า]
    WATCH[ตรวจการณ์]

    WS2[Web Service<br/>กำหนด icon บนแผนที่ / ภาพ Ortho]
end

%% =======================
%% BACK OFFICE
%% =======================
subgraph L2["Back Office"]
    BO1[จัดการข้อมูลพื้นฐาน]
end

%% =======================
%% COMBAT STAFF
%% =======================
subgraph L3["Combat Staff"]
    PLAN_CS[แผนการบิน]
    ADD_TARGET[นำเข้าเป้าในการค้นหา]
    CHECK_VDO[ตรวจด้วยสายตาจาก VDO Streaming]
    REPORT_PDF[รายงานผลทางหน้าจอ<br/>PDF Report]
end

%% =======================
%% MOBILE UAV UNIT
%% =======================
subgraph L4["Mobile UAV Unit"]
    PLAN_MU[แผนการบิน]
    AUTO_FLY[บินตามขั้นตอน]
end

%% =======================
%% UAV & COMMUNICATION
%% =======================
subgraph L5["UAV & Communication"]
    UAV[UAV]
    STREAM[Streaming & Capture Image]
    OBS[ตรวจการณ์<br/>Streaming / Streaming + Ortho]
end

%% =======================
%% AI PROCESSING
%% =======================
subgraph L6["AI Processing"]
    AI_ANALYZE[วิเคราะห์ VDO Streaming<br/>กรณีใกล้เคียงเป้าหมาย]
    JSON[ส่ง JSON<br/>flight_id / target_id<br/>start_time / end_time]
end

%% =======================
%% DASHBOARD
%% =======================
subgraph L7["Dashboard"]
    DASH_REPORT[รายงานผลทางหน้าจอ]
end

%% =======================
%% FLOW CONNECTION
%% =======================

PLAN_CS --> WS1
WS1 --> PLAN_MU
PLAN_MU --> AUTO_FLY
AUTO_FLY --> UAV

UAV --> STREAM
STREAM --> OBS
OBS --> WATCH
OBS --> FIND

FIND --> ADD_TARGET
WATCH --> CHECK_VDO

CHECK_VDO --> AI_ANALYZE
AI_ANALYZE --> JSON
JSON --> WS1

WS2 --> DASH_REPORT
REPORT_PDF --> DASH_REPORT
```

## สรุป

- **ประเภท**: Process / Data Flow Diagram
- **จุดเน้น**: ขั้นตอนการทำงานและการไหลของข้อมูล
- **มุมมอง**: "ข้อมูลไหลอย่างไร"

### Data Flow หลัก

1. **Combat Staff** สร้างแผนการบิน → Web Service
2. **Web Service** ส่งแผนไปยัง Mobile UAV Unit
3. **Mobile UAV Unit** บินอัตโนมัติตามแผน
4. **UAV** ทำการ Streaming & Capture Image
5. **ตรวจการณ์** แยกเป็น 2 โหมด: ค้นหาเป้า / ตรวจการณ์
6. **AI Processing** วิเคราะห์ VDO และส่ง JSON กลับ
7. **Dashboard** แสดงรายงานผล

### JSON Output จาก AI
```json
{
  "flight_id": "...",
  "target_id": "...",
  "start_time": "...",
  "end_time": "..."
}
```
