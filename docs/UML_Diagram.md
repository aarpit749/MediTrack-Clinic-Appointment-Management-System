                         ┌──────────────────────────┐
                         │     MedicalEntity        │◄──────────────┐
                         │──────────────────────────│               │
                         │ - id : String            │               │
                         │ - createdAt : LocalDT    │               │
                         └───────────────▲──────────┘               │
                                         │                          │
                 ┌───────────────────────┴──────────────────────┐   │
                 │                                              │   │
        ┌──────────────────────┐                       ┌──────────────────────┐
        │       Person         │                       │     Appointment      │
        │──────────────────────│                       │──────────────────────│
        │ - name : String      │                       │ - doctorId : String  │
        │ - age : int          │                       │ - patientId : String │
        └───────▲─────▲────────┘                       │ - when : LocalDT     │
                │     │                                │ - status : Enum      │
                │     │                                └───────────┬──────────┘
                │     │                                            │
   ┌────────────┘     └─────────────────────┐                      └──────┐
   │                                        │                             │
┌───────────────────────────┐           ┌──────────────────────┐       ┌──────────────────┐
│    Doctor                 │           │       Patient        │       │       Bill       │
│───────────────────────────│           │──────────────────────│       │──────────────────│
│ - specialization: Enum    │           | - patientNumber: Str │       │ - appointmentId  │
│ - consultationFee : double│           | - contactInfo: Obj   │       │ - base, tax, tot │
│ implements Searchable     │           | implements Searchable│       └───────────┬──────┘
│ implements Payable        │           | implements Cloneable │                   │
└───────────┬───────────────┘           └───────────┬──────────┘                   
            │                                       └───────────┬──────────────────┘  
            |                                                   |uses
            │                                                   ▼
            │                                          ┌──────────────────────┐
            │                                          │    BillSummary       │
            │                                          │ (immutable)          │
            │                                          └──────────────────────┘
            │
   ┌────────▼──────────┐
   │ AppointmentStatus │   (Enum)
   └───────────────────┘
   ┌───────────────────┐
   │ Specialization    │   (Enum)
   └───────────────────┘


Service Layer
─────────────
┌──────────────────────┐
│  PatientService      │
│──────────────────────│
│ + createPatient()    │
│ + searchPatient()    │
│ + list()             │
└──────────┬───────────┘
           │
           │ uses
           ▼
┌──────────────────────┐
│ InMemoryDataStore<T> │ (generic repository)
└──────────────────────┘

┌──────────────────────┐
│  DoctorService       │
│──────────────────────│
│ + createDoctor()     │
│ + filterBySpec()     │
│ + averageFee()       │
└──────────┬───────────┘
           │ uses
           ▼
InMemoryDataStore

┌────────────────────────┐
│ AppointmentService     │
│────────────────────────│
│ + create()             │
│ + cancel()             │
│ + generateBill()       │
└───────────┬────────────┘
            │ uses
            ▼
PatientService, DoctorService


Interfaces
──────────
Searchable  <—— implemented by Doctor, Patient  
Payable     <—— implemented by Doctor


Utilities
──────────
Validator     (static)  
DateUtil      (static)  
CSVUtil       (static)  
IdGenerator   (Singleton)  
DataStore<T>  (interface)  