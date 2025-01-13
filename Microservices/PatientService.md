### **Patient Service: Design, Responsibilities, and Implementation**

The **Patient Service** is a microservice dedicated to managing patient-related functionalities, including profile management, medical history, teleconsultation requests, and emergency alerts. It interacts with other microservices like Doctor Service, Paramedic Service, and X-ray Service to provide a seamless experience for patients.

---
### **Primary Requirements**

1. **Sign-Up and Login**:
	-  Create, read, update, and delete (CRUD) patient profiles.
    - Manage patient-specific details like UPI ID, insurance, and contact information.
2. **Profile Management**:
    - Patients can view and update their personal details, including contact information.
3. **Consultation Booking and Tele-consultation Requests**:
    - Allow patients to request tele-consultations with available doctors.
    - Track the status of consultations (e.g., Scheduled, Ongoing, Completed).
    - Patients can schedule tele-consultations with doctors based on availability.
4. **Medical Records Access / history**:
    - Patients can access their medical history, including prescriptions, X-rays, and consultation summaries.
    - Store and retrieve patient medical history, including past consultations and test results.
    - Link medical records with X-ray images and prescriptions.
5. **Emergency Services**:
    - Process patient-generated emergency requests.
    - Notify the Paramedic Service for task assignments and tracking.
6. **Notifications**:
    - Patients receive real-time updates for upcoming consultations, emergencies, and health-related alerts.
7. **Interaction with Other Services**:
    - Integrate with Doctor Service to fetch available doctors.
    - Communicate with Notification Service for appointment reminders.
    - Collaborate with X-ray Service for medical image access.

### **Secondary Requirements**
1. **Feedback Mechanism**:
    - Patients can provide feedback on consultations and overall platform usability.
2. **Offline Access**:
    - Enable access to critical medical records and consultation history in offline mode.
3. **Health Insights**:
    - Provide patients with insights based on their health data, such as reminders for follow-ups.
4. **Family Member Profiles**:
    - Allow patients to manage and book consultations for family members.
5. **Billing and Payments**:
    - Patients can view and settle bills for consultations and services.
6. **Custom Notifications**:
    - Manage preferences for specific types of alerts and reminders.
---
### **Technology Stack**
- **Programming Language**: Python (FastAPI or Flask for REST APIs).
- **Database**: PostgreSQL for structured patient data.
- **Caching**: Redis for frequently accessed data (e.g., consultation statuses).
- **Messaging**: RabbitMQ or Kafka for event-driven communication (e.g., sending consultation requests to Doctor Service).
---
### **Interactive Flow for Patients**
**UML Use Case Diagram: Patient-Platform Interactions**
```uml
@startuml
actor Patient
actor Doctor
actor Paramedic
actor Admin

Patient --> (Sign-Up/Login)
Patient --> (Profile Management)
Patient --> (Book Consultation)
Patient --> (Access Medical Records)
Patient --> (Request Emergency Services)
Patient --> (Provide Feedback)
Patient --> (View Notifications)

Doctor --> (Consultation)
Paramedic --> (Emergency Response)
Admin --> (Profile Management)
@enduml
```
### **High-Level Architecture**
```
[Client Devices (Mobile/Web)]
   |
   v
[API Gateway] --> [Patient Service]
                     |
                     |---> [PostgreSQL (Patient Data)]
                     |---> [Doctor Service]
                     |---> [Notification Service]
                     |---> [Paramedic Service]
                     |---> [X-ray Service]

```

### **API Endpoints**

| **Method** | **Endpoint**                  | **Description**                           |
| ---------- | ----------------------------- | ----------------------------------------- |
| POST       | `/patients`                   | Create a new patient profile.             |
| GET        | `/patients/<id>`              | Retrieve a patient profile.               |
| PUT        | `/patients/<id>`              | Update patient profile details.           |
| DELETE     | `/patients/<id>`              | Delete a patient profile.                 |
| GET        | `/patients/<id>/history`      | Fetch patient medical history.            |
| POST       | `/patients/<id>/consultation` | Request a teleconsultation with a doctor. |
| POST       | `/patients/<id>/emergency`    | Create an emergency request.              |
**Project Structure**
```
patient-service/
├── app.py
├── models/
│   └── patient.py
├── routes/
│   ├── patient.py
│   ├── consultation.py
│   └── emergency.py
├── utils/
│   └── db.py
├── requirements.txt
└── Dockerfile

```
### **Traceability for Requirements**

| **Req ID** | **Description**                                  | **Traceability**       | **Test ID** | **Verification & Validation**                     |
| ---------- | ------------------------------------------------ | ---------------------- | ----------- | ------------------------------------------------- |
| RQ-PT001   | Patients must securely register and log in.      | Linked to SYS.2, SWE.1 | TST-PT001   | Verify login credentials and MFA setup.           |
| RQ-PT002   | Patients can update their profile details.       | Linked to SYS.3, SWE.1 | TST-PT002   | Validate CRUD operations on profiles.             |
| RQ-PT003   | Patients can book teleconsultations.             | Linked to SYS.3, SWE.2 | TST-PT003   | Ensure scheduling and availability checks.        |
| RQ-PT004   | Patients can access medical records securely.    | Linked to SYS.5, SWE.4 | TST-PT004   | Validate access permissions and record retrieval. |
| RQ-PT005   | Patients can request emergency services.         | Linked to SYS.3, SWE.2 | TST-PT005   | Verify emergency request creation and updates.    |
| RQ-PT006   | Notifications are received in real-time.         | Linked to SYS.5, SWE.4 | TST-PT006   | Test notification triggers and delivery.          |
| RQ-PT007   | Patients can access data offline in emergencies. | Linked to SYS.2, SWE.3 | TST-PT007   | Verify offline access and data sync.              |
| RQ-PT008   | Patients can settle bills securely.              | Linked to SYS.3, SWE.4 | TST-PT008   | Validate payment processing and audit trails.     |
### **Feature Testing and Test Cases**
#### **Feature 1: Sign-Up and Login**
**Test Cases**:

| **Test ID** | **Test Case Description**                   | **Expected Result**                    |
| ----------- | ------------------------------------------- | -------------------------------------- |
| TST-PT001-1 | Patient registers with valid credentials.   | Account is created, and email is sent. |
| TST-PT001-2 | Patient logs in with valid credentials.     | Login is successful.                   |
| TST-PT001-3 | Patient logs in with incorrect credentials. | Login fails with an error message.     |
| TST-PT001-4 | MFA prompt after successful login.          | MFA code is required for access.       |
#### **Feature 2: Consultation Booking**
**Test Cases**:

| **Test ID** | **Test Case Description**                                           | **Expected Result**                        |
| ----------- | ------------------------------------------------------------------- | ------------------------------------------ |
| TST-PT003-1 | Patient books a consultation with an available doctor.              | Booking is successful.                     |
| TST-PT003-2 | Patient attempts to book a consultation with an unavailable doctor. | Booking fails with an appropriate message. |
| TST-PT003-3 | Patient cancels a booked consultation.                              | Consultation is marked as cancelled.       |
#### **Feature 3: Medical Records Access**
**Test Cases**:

| **Test ID** | **Test Case Description**                        | **Expected Result**                    |
| ----------- | ------------------------------------------------ | -------------------------------------- |
| TST-PT004-1 | Patient accesses medical history securely.       | Medical records are displayed.         |
| TST-PT004-2 | Patient attempts to access unauthorized records. | Access is denied with an error.        |
| TST-PT004-3 | Patient downloads an X-ray from their record.    | X-ray file is downloaded successfully. |
#### **Feature 4: Emergency Services**
**Test Cases**:

| **Test ID** | **Test Case Description**                             | **Expected Result**                        |
| ----------- | ----------------------------------------------------- | ------------------------------------------ |
| TST-PT005-1 | Patient creates an emergency request.                 | Request is logged and dispatched.          |
| TST-PT005-2 | Patient tracks the status of their emergency request. | Status updates are displayed in real-time. |

#### **Feature 5: Notifications**
**Test Cases**:

| **Test ID** | **Test Case Description**                                     | **Expected Result**                    |
| ----------- | ------------------------------------------------------------- | -------------------------------------- |
| TST-PT006-1 | Patient receives a notification for an upcoming consultation. | Notification arrives on time.          |
| TST-PT006-2 | Emergency notifications are sent to the patient.              | Notifications are delivered instantly. |
#### **Feature 6: Offline Access**
**Test Cases**:

| **Test ID** | **Test Case Description**                        | **Expected Result**               |
| ----------- | ------------------------------------------------ | --------------------------------- |
| TST-PT007-1 | Patient views medical records offline.           | Records are displayed from cache. |
| TST-PT007-2 | Patient syncs data after regaining connectivity. | Data is updated successfully.     |

-----
### **Functional Safety Considerations**
1. **Emergency Request Handling**:
    - Fail-safe mechanism to ensure emergency requests are logged even during partial system failures.
    - Testing: Simulate server or network outages and verify fallback mechanisms.
2. **Data Privacy and Access**:
    - Encrypt all patient records in storage and transmission.
    - Testing: Verify encryption during record retrieval and file downloads.
3. **Task Prioritization**:
    - Ensure emergency requests take precedence over regular tasks.
    - Testing: Simulate multiple requests and validate prioritization logic.
4. **Audit Logging**:
    - Log all actions related to critical operations (e.g., emergency requests, record access).
    - Testing: Validate completeness and immutability of logs.
---
