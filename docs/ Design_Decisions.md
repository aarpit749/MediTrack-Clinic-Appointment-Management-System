# Design Decisions – MediTrack

## 1. Architecture

The project follows a **layered architecture** to keep responsibilities clearly separated:

* **Entity Layer (`entity`)** – Domain models such as `Doctor`, `Patient`, `Appointment`, and `Bill`.
* **Service Layer (`service`)** – Contains business logic for managing entities.
* **Utility Layer (`util`)** – Helper classes such as `CSVUtil`, `Validator`, `DateUtil`, `IdGenerator`, and `InMemoryDataStore`.
* **Interfaces (`interfaces`)** – Defines reusable behaviors (`Payable`, `Searchable`).
* **Exception Layer (`exception`)** – Custom exceptions for clearer error handling.
* **Application Layer (`Main`, `TestRunner`)** – CLI interface and testing.

This separation improves **maintainability, readability, and extensibility**.

---

# 2. Inheritance Hierarchy

A small inheritance hierarchy is used to avoid duplication.

```
MedicalEntity
   │
   └── Person
         │
         ├── Doctor
         └── Patient
```

`MedicalEntity` provides common fields such as **id and creation timestamp**, while `Person` adds shared attributes like **name and age**. This ensures common logic is reused across entities.

---

# 3. Interface-Based Design

Interfaces are used to represent **capabilities rather than types**.

### Payable

Defines billing-related behavior.

```
double baseAmount();
default double withTax()
```

`Doctor` implements this interface, allowing billing logic to operate generically on any payable entity.

### Searchable

Defines a contract for searchable entities.
Implemented by `Doctor` and `Patient` to enable flexible search functionality.

---

# 4. Generic Data Storage

Data storage is implemented using a **generic in-memory repository**.

```
DataStore<T> → InMemoryDataStore<T>
```

Key decisions:

* Uses **Java Generics** to support multiple entity types.
* Stores data in a `HashMap<String, T>`.
* Uses **reflection** to dynamically call `getId()`.

This approach allows the same datastore implementation to be reused across all services.

---

# 5. Singleton Pattern for ID Generation

The `IdGenerator` class follows the **Singleton design pattern**.

Purpose:

* Ensure a **single shared ID sequence** across the application.
* Prevent ID collisions across entities.

Example generated IDs:

```
DOC-1
PAT-2
APT-3
BILL-4
```

Double-checked locking is used to maintain **thread safety**.

---

# 6. CSV-Based Persistence

Simple persistence is implemented using CSV files.

Supported in:

* `DoctorService`
* `PatientService`
* `AppointmentService`

`CSVUtil` handles file reading and writing while services convert objects to CSV rows.

This provides lightweight persistence without requiring a database.

---

# 7. Stream API Usage

Java Streams and Lambdas are used in the service layer for analytics and filtering.

Examples include:

* Filtering doctors by specialization
* Calculating average consultation fee

This improves code readability and reduces boilerplate loops.

---

# 8. Validation and Error Handling

Input validation is centralized in the `Validator` utility class.
Custom exceptions (`InvalidDataException`, `AppointmentNotFoundException`) provide clearer error handling and maintain consistent validation across services.

---

# 9. Additional Design Choices (Summary)

* **Encapsulation** – Entity fields are private with getters/setters.
* **Immutable Object** – `BillSummary` is immutable to prevent accidental modification.
* **Deep Copy** – `Patient.clone()` performs deep copying of `ContactInfo`.
* **Utility Classes** – Helper classes (`DateUtil`, `CSVUtil`, `Validator`, `Constants`) use static methods and private constructors.
* **Command-Line Interface** – The `Main` class provides a simple menu-driven CLI for interacting with the system.

---

# Conclusion

The MediTrack system is designed using core **object-oriented principles, generic programming, and modular architecture**. The combination of layered design, reusable utilities, and interface-based abstractions keeps the code clean, maintainable, and extensible for future enhancements.
