

The **Emergency Request Service** handles the creation, management, and resolution of emergency requests, coordinating between patients, doctors, and paramedics.

---
#### **1. Primary Requirements**
1. **Request Creation**:
    - Allow patients to raise emergency requests with minimal input.
2. **Task Assignment**:
    - Automatically assign tasks to paramedics or doctors based on availability and proximity.
3. **Real-Time Tracking**:
    - Enable real-time location tracking for emergency tasks.
4. **Notification Integration**:
    - Notify relevant stakeholders immediately upon request creation or updates.
5. **Status Updates**:
    - Track and update the status of emergency requests (e.g., `Pending`, `In-Progress`, `Resolved`).
---
#### **2. Secondary Requirements**
1. **Escalation Mechanisms**:
    - Escalate unresolved requests after a defined time period.
2. **Priority Classification**:
    - Categorize emergencies based on severity (e.g., critical, non-critical).
3. **Analytics**:
    - Provide insights into response times, resolution rates, and task distribution.
---
#### **3. Interactive Flow**

**UML Use Case Diagram: Emergency Request Service Interactions**
```UML
@startuml
actor Patient
actor Paramedic
actor Doctor
actor System

Patient --> (Raise Emergency Request)
System --> (Assign Task to Paramedic)
System --> (Notify Doctor)
Paramedic --> (Update Task Status)
Doctor --> (Join Emergency Teleconsultation)
@enduml
```

### **Emergency Request Service Traceability**

|**Req ID**|**Description**|**Traceability**|**Test ID**|**Verification & Validation**|
|---|---|---|---|---|
|RQ-ERS001|Enable patients to raise emergency requests.|Linked to SYS.2, SWE.1|TST-ERS001|Verify request creation with required details.|
|RQ-ERS002|Assign tasks to paramedics based on proximity.|Linked to SYS.3, SWE.4|TST-ERS002|Test task assignment logic with location data.|
|RQ-ERS003|Notify doctors and paramedics about emergencies.|Linked to SYS.3, SWE.2|TST-ERS003|Validate notifications for assigned tasks.|
|RQ-ERS004|Allow paramedics to update emergency task statuses.|Linked to SYS.5, SWE.2|TST-ERS004|Ensure real-time status updates.|
|RQ-ERS005|Provide real-time location tracking for tasks.|Linked to SYS.3, SWE.4|TST-ERS005|Validate GPS updates and task progress.|
|RQ-ERS006|Log all actions related to emergency requests.|Linked to SYS.5, SWE.2|TST-ERS006|Ensure logs are complete, immutable, and secure.|
|RQ-ERS007|Escalate unresolved requests after a time period.|Linked to SYS.3, SWE.4|TST-ERS007|Test escalation logic for unresolved cases.|
#### **Feature Testing**
**Test Cases**:

| **Test ID** | **Description**                             | **Expected Result**                |
| ----------- | ------------------------------------------- | ---------------------------------- |
| TST-ERS001  | Patient raises an emergency request.        | Request is created successfully.   |
| TST-ERS002  | Paramedic updates emergency request status. | Status is updated in real-time.    |
| TST-ERS003  | Doctor joins emergency teleconsultation.    | Session is initiated successfully. |

---
#### **Functional Safety Considerations**
1. **Immediate Response**:
    - Ensure requests are processed and assigned in under a specified time (e.g., 5 seconds).
    - Testing: Simulate high request volumes to verify response times.
2. **Data Privacy**:
    - Encrypt patient and task details during transmission.
    - Testing: Verify encryption during all API calls.
### **Tools for Implementation**

| **Category**            | **Notification Service**         | **Emergency Request Service**  |
| ----------------------- | -------------------------------- | ------------------------------ |
| Notification Frameworks | Firebase Cloud Messaging, Twilio | Same for integration           |
| Real-Time Communication | Pub/Sub (Kafka, RabbitMQ)        | Pub/Sub (Kafka, RabbitMQ)      |
| Monitoring & Logging    | ELK Stack, Prometheus, Grafana   | ELK Stack, Prometheus, Grafana |
| Mapping Services        | Not Needed                       | Google Maps, OpenStreetMap     |
