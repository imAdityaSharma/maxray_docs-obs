### **Paramedic Service: Design, Responsibilities, and Implementation**

The **Paramedic Service** focuses on managing paramedic profiles, task assignments, emergency responses, and integration with X-ray operations. It ensures paramedics can efficiently handle tasks assigned to them during emergencies and provides tools for uploading diagnostic images.

---
### **1. Primary Requirements**
1. **Sign-Up and Login**:
    - Paramedics must securely register and log in using unique credentials.
2. **Profile Management**:
    - Create, update, and retrieve paramedic profiles.
    - Manage paramedic availability and status (e.g., `Available`, `On-Task`).
3. **Emergency Task Management**:
    - Receive and manage emergency requests from patients.
    - Track task statuses (e.g., `Pending`, `In-Progress`, `Completed`).
4. **X-ray Operations**:
	- Enable paramedics to capture and upload X-ray images during on-site visits.
    - Associate X-ray images with corresponding consultations.
5. **Emergency Communication**:
    - Enable real-time communication with doctors and patients for emergency coordination.
6. **Notifications**:
    - Receive task updates, alerts, and emergency notifications in real-time.
7. **Integration with Other Services**:
    - Communicate with the Patient Service for emergency details.
    - Update the Notification Service for task assignments and reminders.
    - Store X-ray metadata in the X-ray Service for later doctor review.---
### **Secondary Requirements**
1. **Offline Mode**:
    - Provide offline access to critical task details and patient information.
2. **GPS Tracking**:
    - Enable GPS-based tracking for task location updates.
3. **Feedback Mechanism**:
    - Allow paramedics to provide feedback on emergency handling or system issues.
4. **Task Prioritization**:
    - Automatically prioritize tasks based on urgency or severity.
5. **Audit and Logging**:
    - Log all paramedic actions for traceability and compliance.

### **Technology Stack**
- **Programming Language**: Python ( Flask for REST APIs).
- **Database**: PostgreSQL for structured data (paramedic profiles, emergency tasks).
- **File Storage**: S3-compatible storage for uploading and managing X-ray images.
- **Messaging**: Kafka for event-driven communication with Notification and Patient services.
---
### **High-Level Architecture**
```css
[Client Devices (Web/Mobile)]
   |
   v
[API Gateway] --> [Paramedic Service]
                     |
                     |---> [PostgreSQL (Paramedic Data)]
                     |---> [Patient Service]
                     |---> [X-ray Service]
                     |---> [Notification Service]
```

### **API Endpoints**

|**Method**|**Endpoint**|**Description**|
|---|---|---|
|POST|`/paramedics`|Create a new paramedic profile.|
|GET|`/paramedics/<id>`|Retrieve a paramedic profile.|
|PUT|`/paramedics/<id>`|Update paramedic profile details.|
|GET|`/paramedics/<id>/tasks`|Retrieve all tasks assigned to a paramedic.|
|POST|`/paramedics/<id>/tasks/<task>`|Update the status of a task.|
|POST|`/paramedics/<id>/xray`|Upload an X-ray image captured by paramedic.|
#### **1. Project Structure**

Organize the project directory as follows:
```css
paramedic-service/
├── app.py
├── models/
│   ├── paramedic.py
│   ├── emergency_task.py
│   └── xray.py
├── routes/
│   ├── paramedic.py
│   ├── task.py
│   └── xray.py
├── utils/
│   ├── db.py
│   ├── file_storage.py
│   └── jwt.py
├── requirements.txt
└── Dockerfile
```
### **Interactive Flow for Paramedics**

**UML Use Case Diagram: Paramedic-Platform Interactions**
```UML
@startuml
actor Paramedic
actor Doctor
actor Patient
actor Admin

Paramedic --> (Sign-Up/Login)
Paramedic --> (Profile Management)
Paramedic --> (Receive Emergency Tasks)
Paramedic --> (Capture and Upload X-rays)
Paramedic --> (Update Task Status)
Paramedic --> (Feedback Submission)
Paramedic --> (View Notifications)

Doctor --> (Emergency Coordination)
Admin --> (Task Assignment)
@enduml
```
### **Traceability for Requirements**

| **Req ID** | **Description**                                  | **Traceability**       | **Test ID** | **Verification & Validation**                    |
| ---------- | ------------------------------------------------ | ---------------------- | ----------- | ------------------------------------------------ |
| RQ-PR001   | Paramedics must securely register and log in.    | Linked to SYS.2, SWE.1 | TST-PR001   | Verify login credentials and MFA setup.          |
| RQ-PR002   | Paramedics can update their availability status. | Linked to SYS.3, SWE.1 | TST-PR002   | Validate status updates on the profile.          |
| RQ-PR003   | Emergency tasks are assigned and updated.        | Linked to SYS.3, SWE.2 | TST-PR003   | Ensure task updates are logged and synced.       |
| RQ-PR004   | X-ray images are securely captured and uploaded. | Linked to SYS.3, SWE.4 | TST-PR004   | Validate file encryption, upload, and retrieval. |
| RQ-PR005   | Notifications are received in real-time.         | Linked to SYS.5, SWE.4 | TST-PR005   | Test notification triggers and delivery.         |
| RQ-PR006   | Offline mode must allow access to critical data. | Linked to SYS.2, SWE.3 | TST-PR006   | Verify offline access and syncing mechanisms.    |
| RQ-PR007   | GPS tracking must update task locations.         | Linked to SYS.2, SWE.2 | TST-PR007   | Validate location updates and accuracy.          |

---
### **Feature Testing and Test Cases**
#### **Feature 1: Sign-Up and Login**
**Test Cases**:

| **Test ID** | **Test Case Description**                   | **Expected Result**                    |
| ----------- | ------------------------------------------- | -------------------------------------- |
| TST-PR001-1 | Paramedic registers with valid credentials. | Account is created, and email is sent. |
| TST-PR001-2 | Paramedic logs in with valid credentials.   | Login is successful.                   |
| TST-PR001-3 | MFA prompt after successful login.          | MFA code is required for access.       |

---
#### **Feature 2: Emergency Task Management**
**Test Cases**:

| **Test ID** | **Test Case Description**                       | **Expected Result**                     |
| ----------- | ----------------------------------------------- | --------------------------------------- |
| TST-PR003-1 | Paramedic receives a new emergency task.        | Task is displayed in the app.           |
| TST-PR003-2 | Paramedic updates task status to `In-Progress`. | Status is updated and logged.           |
| TST-PR003-3 | Task completion triggers a notification.        | Notification is sent to relevant users. |

---
#### **Feature 3: X-ray Uploads**
**Test Cases**:

| **Test ID** | **Test Case Description**                         | **Expected Result**                 |
| ----------- | ------------------------------------------------- | ----------------------------------- |
| TST-PR004-1 | Paramedic captures and uploads an X-ray.          | X-ray is uploaded securely.         |
| TST-PR004-2 | Simulate network failure during upload.           | Retry mechanism ensures completion. |
| TST-PR004-3 | X-ray metadata is linked to the task and patient. | Metadata is stored and accessible.  |

---
#### **Feature 4: Notifications**
**Test Cases**:

| **Test ID** | **Test Case Description**                          | **Expected Result**                  |
| ----------- | -------------------------------------------------- | ------------------------------------ |
| TST-PR005-1 | Paramedic receives notification for a task update. | Notification arrives promptly.       |
| TST-PR005-2 | Emergency notification is sent to the paramedic.   | Notification is delivered instantly. |

---
#### **Feature 5: GPS Tracking**
**Test Cases**:

| **Test ID** | **Test Case Description**                  | **Expected Result**              |
| ----------- | ------------------------------------------ | -------------------------------- |
| TST-PR007-1 | Task location is updated based on GPS.     | Location is accurate and timely. |
| TST-PR007-2 | Simulate GPS unavailability during a task. | Fallback mechanism activates.    |

---
### **Functional Safety Considerations**

1. **Emergency Data Access**:
    - **Fail-Safe Mechanism**: Paramedics must access critical data offline in emergencies.
    - **Testing**: Validate offline caching and automatic syncing.
2. **Task Prioritization**:
    - Ensure high-priority tasks are highlighted and assigned first.
    - **Testing**: Simulate multiple tasks and confirm correct prioritization.
3. **Data Encryption**:
    - Encrypt X-ray images and sensitive task data during storage and transmission.
    - **Testing**: Verify encrypted uploads and access logs.
4. **Error Handling**:
    - Implement retries for failed X-ray uploads and notifications.
    - **Testing**: Introduce network faults and ensure recovery mechanisms.
5. **Audit Logging**:
    - Log all paramedic actions (e.g., task updates, data access) for compliance.
    - **Testing**: Validate logs are complete, immutable, and accessible.