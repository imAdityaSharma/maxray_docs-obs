Diagram-> [Interaction](/UMLs/assets/Interaction.png)
```Sql
@startuml

actor Patient
participant "API Gateway" as AG
participant "PatientService" as PS
participant "DoctorService" as DS
participant "TeleconferenceService" as TS
participant "Database Server" as DB

Patient -> AG: Request Teleconsultation
AG -> PS: Validate Request
PS -> DB: Check Patient Data
DB --> PS: Patient Data Found
PS -> DS: Notify Doctor
DS -> Doctor: Alert for Consultation
Doctor -> TS: Initiate Teleconsultation
TS -> Patient: Connect Session

@enduml
```

