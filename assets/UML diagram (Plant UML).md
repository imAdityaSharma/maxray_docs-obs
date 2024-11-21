@startuml

class User {
  + UUID user_id
  + String email
  + String password_hash
  + String role {enum: Patient, Doctor, Paramedic, Admin}
  + Timestamp created_at
  + Timestamp updated_at
}

class Patient {
  + UUID patient_id
  + UUID user_id
  + String upi_id
  + Text medical_history
  + Text insurance_details
  + Timestamp created_at
}

class Doctor {
  + UUID doctor_id
  + UUID user_id
  + String specialization
  + Boolean availability
  + Timestamp created_at
}

class Paramedic {
  + UUID paramedic_id
  + UUID user_id
  + String status {enum: Available, On-Task}
  + Text assigned_tasks
  + Timestamp created_at
}

class Admin {
  + UUID admin_id
  + UUID user_id
  + Text permissions
  + Timestamp created_at
}

class Consultation {
  + UUID consultation_id
  + UUID patient_id
  + UUID doctor_id
  + Timestamp start_time
  + Timestamp end_time
  + Text notes
  + String status {enum: Scheduled, Completed, Cancelled}
}

class Prescription {
  + UUID prescription_id
  + UUID consultation_id
  + Text details
  + Timestamp created_at
}

class XrayImage {
  + UUID xray_id
  + UUID paramedic_id
  + UUID consultation_id
  + String image_path
  + Timestamp uploaded_at
}

class EmergencyRequest {
  + UUID request_id
  + UUID patient_id
  + UUID paramedic_id
  + String status {enum: Pending, In-Progress, Resolved}
  + Timestamp created_at
}

class AuditLog {
  + UUID log_id
  + UUID user_id
  + String action
  + Timestamp timestamp
  + Text details
}

User <|-- Patient
User <|-- Doctor
User <|-- Paramedic
User <|-- Admin

Patient --> Consultation : "1:M"
Patient --> EmergencyRequest : "1:M"
Patient --> XrayImage : "1:M"

Doctor --> Consultation : "1:M"
Doctor --> Prescription : "1:M"

Paramedic --> EmergencyRequest : "M:1"
Paramedic --> XrayImage : "M:1"

Consultation --> Prescription : "1:1"
Consultation --> XrayImage : "1:M"

@enduml

[[class dgm uml.png]]