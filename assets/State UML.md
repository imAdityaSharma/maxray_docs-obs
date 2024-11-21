@startuml

[*] --> Scheduled : Create Consultation
Scheduled --> Completed : End Consultation
Scheduled --> Cancelled : Cancel Request
Completed --> Archived : Archive Data

@enduml
[[state.png]]
