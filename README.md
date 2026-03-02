🚀 Leave Management System (OIC + ATP + VBCS)
📌 Project Overview

This project implements a complete Leave Management System using:

Oracle Integration Cloud (OIC) – Backend orchestration

Autonomous Transaction Processing (ATP) – Database

Visual Builder Cloud Service (VBCS) – Frontend UI

The system supports:

Employee Leave Submission

View Leave History

Manager Pending Approvals

Approve / Reject Workflow

This project demonstrates enterprise integration patterns and clean orchestration design.

🏗 Architecture
VBCS (UI)
     ↓ REST
OIC (App Driven Orchestration)
     ↓ SQL
ATP (Oracle Autonomous Database)
🛠 Tech Stack
Layer	Technology
Frontend	Oracle VBCS
Integration	Oracle Integration Cloud
Database	Oracle ATP
API Style	REST (App Driven Orchestration)
Identity	OIC Runtime Instance ID as Primary Key
🗄 Database Design
EMPLOYEE_T
Column	Type
EMP_ID	NUMBER
NAME	VARCHAR
EMAIL	VARCHAR
MANAGER_ID	NUMBER
ROLE	VARCHAR
LEAVE_REQUEST_T
Column	Type
OIC_RUN_ID	VARCHAR (Primary Key)
EMP_ID	NUMBER
FROM_DATE	DATE
TO_DATE	DATE
REASON	VARCHAR
STATUS	VARCHAR
MANAGER_COMMENT	VARCHAR
CREATED_DATE	DATE
🔄 OIC Integration Design
Integration Type

App Driven Orchestration

REST Request Contract
{
  "action": "",
  "empId": 0,
  "fromDate": "",
  "toDate": "",
  "reason": "",
  "status": "",
  "managerComment": "",
  "oicRunId": ""
}
REST Response Contract
{
  "status": "",
  "message": "",
  "oicRunId": "",
  "leaveList": [
    {
      "oicRunId": "",
      "empId": 0,
      "fromDate": "",
      "toDate": "",
      "reason": "",
      "status": "",
      "managerComment": "",
      "createdDate": ""
    }
  ]
}
🔀 Supported Actions
1️⃣ SUBMIT

Creates a new leave request.

Generates OIC_RUN_ID using $integrationInstanceId

Sets STATUS = "PENDING"

Sample Request
{
  "action": "SUBMIT",
  "empId": 1002,
  "fromDate": "2026-03-15",
  "toDate": "2026-03-17",
  "reason": "Family function",
  "status": "",
  "managerComment": "",
  "oicRunId": ""
}
2️⃣ GET_MY_LEAVES

Returns leave history for employee.

Uses bind variable:

WHERE EMP_ID = #empId
3️⃣ GET_PENDING

Returns pending leave requests for a manager.

JOIN EMPLOYEE_T E ON LR.EMP_ID = E.EMP_ID
WHERE E.MANAGER_ID = #managerId
AND LR.STATUS = 'PENDING'
4️⃣ UPDATE_STATUS

Updates leave status (APPROVED / REJECTED).

UPDATE LEAVE_REQUEST_T
SET STATUS = #status,
    MANAGER_COMMENT = #managerComment
WHERE OIC_RUN_ID = #oicRunId
🧠 Key Design Pattern
Wrapper Response Pattern

A global variable FINAL_RESPONSE is used to:

Populate response per branch

Avoid multiple response mappings

Keep final mapper clean

Final mapping:

FINAL_RESPONSE → REST Response
🎓 Enterprise Learnings Demonstrated

App Driven Orchestration design

Multi-branch switch routing

Structured response wrapper pattern

Bind variable usage (#variable)

Repeating node mapping

Clean separation of UI and backend

Runtime ID as business key

Join queries in OIC

Update operations with business logic

🚀 How To Run
Step 1 – Setup ATP

Run SQL scripts in /docs/database-schema.sql

Step 2 – Import OIC Integration

Import .iar file from /oic

Step 3 – Activate Integration
Step 4 – Test APIs

Use payloads in /samples

Step 5 – Import VBCS App

Deploy UI from /vbcs

🔮 Future Enhancements

Role-based authentication

Leave balance tracking

Email notification integration

Pagination support

Error handling framework

Audit logging

Deployment automation

📌 Project Status

✔ Backend complete
✔ Integration stable
✔ UI connected
✔ Full approval workflow implemented

👨‍💻 Author

Amit Kumar Ram
Oracle Integration | VBCS | Cloud Architecture
