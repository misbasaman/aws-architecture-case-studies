# FHIR Model – Telehealth Architecture

## Overview
This document describes which **FHIR R4 resources** are used in the telehealth solution, and how they map to system capabilities (appointments, prescriptions, medical notes, history).

---

## Core FHIR Resources

| FHIR Resource         | Usage in Telehealth System |
|------------------------|----------------------------|
| **Patient**           | Patient demographic & identifiers |
| **Practitioner**      | Provider information (doctor, nurse) |
| **Encounter**         | Each telehealth session (virtual visit) |
| **Appointment**       | Scheduling resource linking patient & provider |
| **Medication**        | Reference to a prescribed drug |
| **MedicationRequest** | Prescription orders issued by providers |
| **Observation**       | Vitals, lab values, visit outcomes |
| **Condition**         | Diagnosis or chronic illness |
| **DocumentReference** | Storage of attached notes, lab PDFs, or scans (points to S3 object) |
| **Provenance**        | Tracks authorship of notes & updates |
| **AuditEvent**        | Captures system interactions for compliance logging |

---

## Data Flow Examples

### 1. Appointment Booking
- Patient books an appointment (Aurora transactional entry)  
- FHIR `Appointment` resource created in HealthLake linking `Patient` + `Practitioner`  

### 2. Telehealth Visit
- Provider starts a Chime session (metadata logged)  
- FHIR `Encounter` created, referencing the `Appointment`  
- Clinical notes recorded as `DocumentReference`, optionally linked to S3 scans  

### 3. Prescription
- Provider issues medication order → `MedicationRequest` resource created  
- Workflow entry stored in Aurora for pharmacy integration  

### 4. Notes & History
- Each visit note is attached as a `DocumentReference`  
- FHIR versioning ensures immutable history of clinical records  

---

## Compliance & Audit
- **Versioning:** FHIR resource history in HealthLake ensures changes are tracked  
- **Provenance:** Links actions (who, when, how) for clinical accountability  
- **AuditEvent:** Captures access events (view, modify, delete) for compliance  

---

## References
- [HL7 FHIR R4 Resources](https://www.hl7.org/fhir/resourcelist.html)  
- [Amazon HealthLake – FHIR Support](https://docs.aws.amazon.com/healthlake/latest/devguide/what-is-healthlake.html)  
- [AWS Healthcare & Life Sciences Lens](https://docs.aws.amazon.com/wellarchitected/latest/healthcare-industry-lens/healthcare-industry-lens.html)  

