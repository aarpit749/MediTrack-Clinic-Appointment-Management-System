# MediTrack-Clinic-Appointment-Management-System
MediTrack is a modular, object-oriented Clinic &amp; Appointment Management System implemented in Core Java. The system models patients, doctors, appointments, and billing; demonstrates strong OOP design, SOLID principles, standard Java features (collections, exceptions, I/O, serialization). 
Features

Patients & Doctors: Create, list, search (ID/name/age/specialization)
Appointments: Create, list, cancel, status tracking (PENDING, CONFIRMED, CANCELLED)
Billing: Generate bill with tax; immutable BillSummary
Persistence (CSV): Save/Load doctors, patients, appointments
Streams & Lambdas: Filter doctors by specialization, average fee analytics
Clean Architecture: Entities, Services, Utilities, Exceptions, Interfaces, Constants
Manual Tests: TestRunner to run quick smoke checks
JavaDoc HTML (Bonus): Generate documentation under docs/javadoc/
Project Structure
MediTrack/
├─ src/
│  └─ main/java/com/airtribe/meditrack/
│     ├─ Main.java
│     ├─ constants/Constants.java
│     ├─ entity/ (Person, Doctor, Patient, Appointment, Bill, BillSummary, Enums)
│     ├─ exception/ (InvalidDataException, AppointmentNotFoundException)
│     ├─ interfaces/ (Searchable, Payable)
│     ├─ service/ (PatientService, DoctorService, AppointmentService)
│     ├─ util/ (Validator, CSVUtil, DateUtil, IdGenerator, DataStore, InMemoryDataStore)
│     └─ test/TestRunner.java
├─ docs/
│  ├─ Setup_Instructions.md
│  ├─ JVM_Report.md
│  ├─ Design_Decisions.md
│  └─ javadoc/ (generated HTML docs → index.html)
├─ data/
│  ├─ doctors.csv
│  ├─ patients.csv
│  └─ appointments.csv
└─ README.md
