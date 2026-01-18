# Flow #1 - Architecture / Structure Diagram

แสดงโครงสร้างระบบและผู้ใช้งาน (ใครทำอะไรได้บ้าง)

```mermaid
graph TD

%% Core Systems
AI[AI Server]
COM[UAV Communications]
APP[Application Software / Streaming Server / NVR<br/>ควบคุมการบิน (VAYU-GUARD)]

AI <--> APP
COM <--> APP

%% Main Modules
DASH[1.1 Dashboard Summary]
BACK[2 Back office]
COMBAT[3 Combat Staff]
MOBILE[4 Mobile UAV Unit]

APP --> DASH
APP --> BACK
APP --> COMBAT
APP --> MOBILE

%% Dashboard Users
DASH --> EXEC[ผู้บริหาร]
DASH --> HEAD[ผบ.หน่วย]
HEAD --> SUBOP[ผู้ปฏิบัติ<br/>ภายใต้หน่วย]

%% Command & Control
COMBAT --> CMD[Commander]
COMBAT --> OPR[Operator]

CMD --> C2[C2<br/>กำหนดแผนการบิน / สั่งการ / ควบคุม / กำหนดสิทธิ์]
OPR --> C2

EXEC --> C2
SUBOP --> C2

%% Mobile UAV
MOBILE --> M1[บินค้นหาเป้าหมาย]
MOBILE --> M2[บินตรวจการณ์ชายแดน]

M1 --> UAV[UAV]
M2 --> UAV

%% Back Office
BACK --> ORG[หน่วยงาน]
ORG --> PERM[กำหนดสิทธิ์การใช้]
PERM --> AUDIT[ตรวจสอบสิทธิ์]

BACK --> LIST[รายการ UAV]
LIST --> SPEC[คุณสมบัติ]
SPEC --> UNIT[หน่วยถือ-ปฏิบัติงาน]
```

## สรุป

- **ประเภท**: Architecture / Structure Diagram
- **จุดเน้น**: โครงสร้างระบบและผู้ใช้งาน
- **มุมมอง**: "ใครทำอะไรได้บ้าง"

### Components หลัก
1. **Core Systems**: AI Server, UAV Communications, Application Software
2. **Dashboard**: สำหรับผู้บริหาร และ ผบ.หน่วย
3. **Back Office**: จัดการหน่วยงาน, สิทธิ์, รายการ UAV
4. **Combat Staff**: Commander และ Operator ควบคุมผ่าน C2
5. **Mobile UAV Unit**: บินค้นหาเป้าหมาย, ตรวจการณ์ชายแดน
