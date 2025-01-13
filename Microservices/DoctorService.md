### **Doctor Service: Design, Responsibilities, and Implementation**

The **Doctor Service** manages the profiles, availability, and activities of doctors on the MaxRay platform. It facilitates tele-consultations, allows doctors to access patient histories and diagnostic data (like X-rays), and supports prescription creation.

---
### **1. Primary Requirements**

These are the core functionalities essential for doctors to perform their roles effectively on the platform:

1. **Sign-Up and Login**:
    - Create, update, and retrieve doctor profiles.
    - Manage doctor availability and specialties.
2. **Profile Management**:
    - View and edit their profiles, including availability and specialization.
3. **Patient Management**:
    - Access assigned patient data, including medical history, prescriptions, and diagnostic images (e.g., X-rays).
4. **Consultation Management**:
    - Conduct teleconsultations with patients, review diagnostic data, and generate prescriptions.
5. **Prescription Management**:
    - Allow doctors to create, edit, and finalize prescriptions during consultations.
    - Store prescriptions linked to consultations and patients.
6. **Teleconference Access**:
    - Handle consultation scheduling and status updates.
    - Provide interfaces for accessing patient medical records and diagnostic images.
7. **Notifications**:
    - Receive real-time notifications about scheduled consultations or emergency cases.
4. **Integration with Other Services**:
    - Fetch patient details from the Patient Service.
    - Collaborate with Teleconference Service for virtual consultations.
    - Update the Notification Service for reminders and updates.
---
### **2. Secondary Requirements**

These enhance usability and extend functionality:

1. **Multi-Factor Authentication (MFA)**:
    - Enable MFA for added security during login.
2. **Dashboard**:
    - View upcoming consultations, pending tasks, and analytics on patient interactions.
3. **Emergency Handling**:
    - Handle emergency cases by reviewing critical patient data and collaborating with paramedics.
4. **Feedback Mechanism**:
    - Provide feedback on platform usability and report issues.
5. **Offline Mode**:
    - Access critical patient data and past consultation records offline during emergencies.
6. **Custom Notifications**:
    - Manage preferences for receiving alerts (e.g., consultation reminders, system updates).

---
### **4. Technology Stack**
- **Programming Language**: Python (FastAPI or Flask for REST APIs).
- **Database**: PostgreSQL for structured data (doctor profiles, consultations, prescriptions).
- **Caching**: Redis for temporary storage (e.g., doctor availability, ongoing consultations).
- **Messaging**: RabbitMQ or Kafka for communication with Notification and Teleconference services.
---
### **5. High-Level Architecture**
```css
[Client Devices (Web/Mobile)]
   |
   v
[API Gateway] --> [Doctor Service]
                     |
                     |---> [PostgreSQL (Doctor Data)]
                     |---> [Patient Service]
                     |---> [Teleconference Service]
                     |---> [Notification Service]
                     |---> [Prescription Service]

```

### **6. API Endpoints**

| **Method** | **Endpoint**                  | **Description**                           |
| ---------- | ----------------------------- | ----------------------------------------- |
| POST       | `/doctors`                    | Create a new doctor profile.              |
| GET        | `/doctors/<id>`               | Retrieve a doctor's profile.              |
| PUT        | `/doctors/<id>`               | Update doctor profile details.            |
| GET        | `/doctors/<id>/availability`  | Fetch the doctor's availability.          |
| POST       | `/doctors/<id>/consultation`  | Start a new consultation.                 |
| GET        | `/doctors/<id>/consultations` | Retrieve past and ongoing consultations.  |
| POST       | `/doctors/<id>/prescription`  | Create a prescription for a consultation. |
**Project Structure**
```css
doctor-service/
├── app.py
├── models/
│   ├── doctor.py
│   └── consultation.py
├── routes/
│   ├── doctor.py
│   ├── consultation.py
│   └── prescription.py
├── utils/
│   ├── db.py
│   └── jwt.py
├── requirements.txt
└── Dockerfile
```

### **7. Interactive Flow for Doctor and Feature Interactions**
**UML Use Case Diagram: Doctor-Platform Interactions**
```PlantUML
@startuml
actor Doctor
actor Patient
actor Paramedic
actor Admin

Doctor --> (Sign-Up/Login)
Doctor --> (Profile Management)
Doctor --> (Access Patient Data)
Doctor --> (Review X-rays)
Doctor --> (Teleconsultation)
Doctor --> (Generate Prescription)
Doctor --> (Emergency Handling)
Doctor --> (Feedback Submission)
Doctor --> (View Notifications)

(Patient) --> (Teleconsultation)
(Paramedic) --> (Emergency Handling)
(Admin) --> (Profile Management)
@enduml
```
### **8. Traceability for Requirements**

| **Req ID** | **Description**                               | **Test ID** | **Verification & Validation**                   |
| ---------- | --------------------------------------------- | ----------- | ----------------------------------------------- |
| RQ-D001    | Doctors must securely register and log in.    | TST-D001    | Verify login credentials and MFA setup.         |
| RQ-D002    | Doctors can manage their profiles.            | TST-D002    | Validate CRUD operations on doctor profiles.    |
| RQ-D003    | Doctors can access patient records securely.  | TST-D003    | Ensure data access is restricted and logged.    |
| RQ-D004    | Doctors can conduct teleconsultations.        | TST-D004    | Test audio, video, and screen-sharing features. |
| RQ-D005    | Doctors can generate prescriptions digitally. | TST-D005    | Verify prescription creation and sharing.       |
| RQ-D006    | Doctors receive real-time notifications.      | TST-D006    | Validate notification triggers and delivery.    |
| RQ-D007    | Doctors can handle emergency requests.        | TST-D007    | Test workflow for accessing emergency data.     |
| RQ-D008    | Doctors can provide feedback on the platform. | TST-D008    | Ensure feedback is captured and stored.         |

#### Traceability for Functional Safety Requirements

| **Req ID** | **Description**                                   | **Traceability**              | **Test ID** | **Validation**                          |     |
| ---------- | ------------------------------------------------- | ----------------------------- | ----------- | --------------------------------------- | --- |
| RQ-FS001   | Emergency notifications must be delivered in 10s. | Linked to SYS.2, SWE.1, SWE.4 | TST-FS001   | Verify under normal and peak load.      |     |
| RQ-FS002   | X-ray uploads must have retry mechanisms.         | Linked to SYS.3, SWE.2        | TST-FS002   | Validate retry behavior for failures.   |     |
| RQ-FS003   | Teleconsultation must have end-to-end encryption. | Linked to SWE.1, SWE.2        | TST-FS003   | Test encryption of video streams.       |     |
| RQ-FS004   | Emergency data must load in <5s.                  | Linked to SYS.2, SWE.2, SWE.4 | TST-FS004   | Measure response times for emergencies. |     |

---
### **9. Feature Testing and Test Cases**

###### **Test Plan**

- **Unit Testing**:
    - Test safety-critical components (e.g., notification dispatch, encryption) in isolation.
- **Integration Testing**:
    - Validate end-to-end workflows for emergency handling and teleconsultation.
- **Stress Testing**:
    - Simulate peak loads to ensure performance under extreme conditions.
- **Failure Injection Testing**:
    - Introduce faults (e.g., network failures) to validate recovery mechanisms.

#### **Feature 1: Sign-Up and Login**
**Test Cases**:

| **Test ID** | **Test Case Description**                  | **Expected Result**                    |
| ----------- | ------------------------------------------ | -------------------------------------- |
| TST-D001-1  | Doctor registers with valid credentials.   | Account is created, and email is sent. |
| TST-D001-2  | Doctor logs in with valid credentials.     | Login is successful.                   |
| TST-D001-3  | Doctor logs in with incorrect credentials. | Login fails with error message.        |
| TST-D001-4  | MFA prompt after successful login.         | MFA code is required for access.       |

---
#### **Feature 2: Access Patient Records**
**Test Cases**:

| **Test ID** | **Test Case Description**                              | **Expected Result**                 |
| ----------- | ------------------------------------------------------ | ----------------------------------- |
| TST-D003-1  | Doctor accesses patient data with valid permissions.   | Patient data is displayed securely. |
| TST-D003-2  | Doctor attempts to access unauthorized patient data.   | Access is denied with an error.     |
| TST-D003-3  | Doctor accesses X-ray images linked to a consultation. | X-ray images load correctly.        |

---
#### **Feature 3: Teleconsultation**
**Test Cases**:

| **Test ID** | **Test Case Description**                         | **Expected Result**               |
| ----------- | ------------------------------------------------- | --------------------------------- |
| TST-D004-1  | Doctor initiates a teleconsultation.              | Session starts successfully.      |
| TST-D004-2  | Patient joins the teleconsultation.               | Session updates with participant. |
| TST-D004-3  | Teleconsultation ends properly.                   | Session is marked as completed.   |
| TST-D004-4  | Video, audio, and chat features work as expected. | Features function without issues. |

---
#### **Feature 4: Prescription Management**
**Test Cases**:

| **Test ID** | **Test Case Description**                              | **Expected Result**                |
| ----------- | ------------------------------------------------------ | ---------------------------------- |
| TST-D005-1  | Doctor generates a prescription during a consultation. | Prescription is saved and linked.  |
| TST-D005-2  | Doctor edits an existing prescription.                 | Changes are saved successfully.    |
| TST-D005-3  | Prescription is shared with the patient.               | Patient receives the prescription. |

---
#### **Feature 5: Notifications**
**Test Cases**:

| **Test ID** | **Test Case Description**                             | **Expected Result**                    |
| ----------- | ----------------------------------------------------- | -------------------------------------- |
| TST-D006-1  | Doctor receives notification for an upcoming session. | Notification arrives on time.          |
| TST-D006-2  | Emergency notifications are sent to the doctor.       | Notifications are delivered instantly. |

---
#### **Feature 6: Feedback**
**Test Cases**:

| **Test ID** | **Test Case Description**                              | **Expected Result**             |
| ----------- | ------------------------------------------------------ | ------------------------------- |
| TST-D008-1  | Doctor submits feedback on platform usability.         | Feedback is saved successfully. |
| TST-D008-2  | Doctor submits feedback with missing mandatory fields. | Submission fails with an error. |
