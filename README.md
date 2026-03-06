# Techinical Document

## 0. Overview

Hệ thống Quản lý Bệnh nhân là một ứng dụng CRUD gọn nhẹ được thiết kế để đơn giản hóa việc lưu trữ hồ sơ y tế. Nó cho phép người dùng quản lý danh sách bệnh nhân và theo dõi lịch sử thăm khám của từng bệnh nhân.


## 1. Project architecture
Ứng dụng này sử dụng mô hình Client-Server. Giao diện người dùng (Frontend - Angular) yêu cầu dữ liệu từ máy chủ (Backend - ASP.NET Core), máy chủ này tương tác với cơ sở dữ liệu.

* Frontend: http://localhost:4200
* Backend: http://localhost:7282

## 2. Page flow (Frontend)

| Page | Name | Purpose | Actions |
| :--- | :--- | :--- | :--- |
| **1** | Home | Entry point | A single button to "Enter System" |
| **2** | Patient List | Manage Patients | View List, Add Patient, Delete Patient |
| **3** | Patient Visit History | Manage patient visit | View List, Add and delete history for specific patient |

## 3. API documentation (Backend) 

### Patient CRUD (5 APIs)

| Endpoint | Method | Action | Example JSON Body |
| :--- | :--- | :--- | :--- |
| `/api/Patients` | `GET` | Get all patients | N/A |
| `/api/Patients/{id}` | `GET` | Get patient by Id | N/A |
| `/api/Patients` | `POST` | Add a patient | `{"id":"...", "name": "Brandon", "age": 20, "address":"188 Nguyen Xi"}` |
| `/api/Patients/{id}` | `DELETE`| Remove a patient | N/A |
| `/api/Patients/{id}` | `PUT`| Update a patient | `{"id":"...", "name": "Brandon", "age": 20, "address":"188 Nguyen Xi"}` |

### Patient history CRUD (5 APIs)

| Endpoint | Method | Action | Example JSON Body |
| :--- | :--- | :--- | :--- |
| `/api/PatientHistory/patient/{patientId}` | `GET` | Get visit histories by patientId | N/A |
| `/api/PatientHistory/{id}` | `GET` | Get visit history by Id | N/A |
| `/api/PatientHistory` | `POST` | Add a patient visit history | `{"id":"...", "patientId": "...", "visitedTime":"2026-04-12"}` |
| `/api/PatientHistory/{id}` | `DELETE`| Remove a visit history | N/A |
| `/api/PatientHistory/{id}` | `PUT`| Update a visit history | `{"id":"...", "patientId": "...", "visitedTime":"2026-04-12"}` |

## 4. Data Models
### Patient object

`
{
  "id": "string",
  "name": "string",
  "age": "int",
  "address": "string"
}
`
### Patient history object

`
{
  "id": "string",
  "patientId": "string",
  "visitedTime": "Date"
}
`

## 5. Database schema 

| Table | Column | Type | Description |
| :--- | :--- | :--- | :--- |
| **Patients** | `Id` | nvarchar(max) (PK) | Unique ID for each patient |
| | `Name` | nvarchar(max) | Full name |
| | `Age` | int | Age |
| | `Address` | nvarchar(max) | Address |
| **PatientHistory** | `Id` | nvarchar(max) (PK) | Unique ID for the visit |
| | `PatientId` | nvarchar(max) | Links to Patients table |
| | `VisitedDate` | datetime | Date of the check-up |

## 6. Store procedures

`sp_InsertPatient`
* **Input:** `@Id, @Name, @Age, @Address`
* **Logic:** Thêm bản ghi thông tin bệnh nhân mới vào cơ sở dữ liệu.

`sp_InsertPatientHistory`
* **Input:** `@Id, @PatientId, @VisitedTime`
* **Logic:** Tạo một bản ghi lịch sử khám bệnh mới liên kết với bệnh nhân cụ thể.

`sp_GetPatients`
* **Input:** `@Id`
* **Logic:** Nếu `@Id` để trống, truy xuất danh sách toàn bộ bệnh nhân. Nếu có `@Id`, chỉ trả về thông tin của bệnh nhân đó.

`sp_GetPatientHistory`
* **Input:** `@Id`
* **Logic:** Lấy thông tin chi tiết của một bản ghi lịch sử thăm khám cụ thể dựa trên mã ID.

`sp_GetPatientHistoryByPatient`
* **Input:** `@PatientId`
* **Logic:** Lấy toàn bộ danh sách các lần khám bệnh của một bệnh nhân nhất định dựa trên mã bệnh nhân.

## 7. Development Steps
* __Backend__: Tạo các model Patient và PatientHistory bằng C#.

* __Database__: Thiết lập các bảng SQL và store procedures.

* __Controllers__: Viết 10 phương thức [HttpGet], [HttpPost], v.v.

* __Frontend__: Tạo 3 component Angular và liên kết chúng với Router.
