@startuml

actor Patient
actor Doctor
actor Paramedic
actor Admin

rectangle System {
  usecase "Register and Login" as UC1
  usecase "Request Teleconsultation" as UC2
  usecase "Track Paramedic" as UC3
  usecase "View Medical History" as UC4
  usecase "Conduct Consultation" as UC5
  usecase "Prescribe Medication" as UC6
  usecase "Operate Xray Device" as UC7
  usecase "Log Emergency Actions" as UC8
  usecase "Monitor System Health" as UC9
  usecase "Manage Users" as UC10
}

Patient --> UC1
Patient --> UC2
Patient --> UC3
Patient --> UC4

Doctor --> UC4
Doctor --> UC5
Doctor --> UC6

Paramedic --> UC7
Paramedic --> UC8

Admin --> UC9
Admin --> UC10

@enduml

[[usecase.png]]