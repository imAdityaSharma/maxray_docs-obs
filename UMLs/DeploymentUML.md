Diagram-> [Deployment](/UMLs/assets/deployment.png)

``` Sql
@startuml

node "Client Device" {
  [Mobile App]
  [Web App]
}

node "Application Server" {
  component "API Gateway"
  component "PatientService"
  component "DoctorService"
  component "ParamedicService"
  component "AdminService"
}

node "Database Server" {
  database "PostgreSQL" {
    [User]
    [Patient]
    [Consultation]
    [Prescription]
    [XrayImage]
    [AuditLog]
  }
  component "Object Storage" {
    [Xray Images]
  }
}

"Mobile App" --> "API Gateway"
"Web App" --> "API Gateway"
"API Gateway" --> "PatientService"
"API Gateway" --> "DoctorService"
"API Gateway" --> "ParamedicService"
"API Gateway" --> "AdminService"
"PatientService" --> "Database Server"
"DoctorService" --> "Database Server"
"ParamedicService" --> "Database Server"
"AdminService" --> "Database Server"

@enduml
```

