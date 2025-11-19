# Design and Implementation of a Patient Data Normalisation Platform for Multi-Clinic Interoperability in Ghana
## Background & Rationale

Ghana’s healthcare ecosystem comprises a mix of public hospitals, private clinics, pharmacies, Community-Based Health Planning and Services (CHPS) centres, diagnostic centres, and specialist facilities. These institutions often use different systems for recording patient information, ranging from Excel/CSV files to proprietary EMRs, HL7 (Health Level Seven) outputs from laboratory equipment, and manually digitised paper records.
This results in significant challenges:
- Patient information cannot easily move between facilities.
- National reporting (e.g., DHIMS2) relies on manual entry or inconsistent formats.
- Referral handovers between hospitals and clinics lack standardisation.
- Analytics, disease surveillance, and continuity of care become difficult.

With Ghana pursuing digital health transformation through the Ministry of Health (MoH), Ghana Health Service (GHS), and NHIS, the need to normalise and standardise health data is becoming central. A lightweight, open-source, standards-based platform is essential for practical interoperability across diverse clinical environments, particularly resource-constrained ones.

## Problem Statement
Healthcare facilities in Ghana produce and store patient data in different formats and structures with no unified interoperability standard. This makes it challenging to share patient information across institutions or support national-level health analytics initiatives.
There is a lack of a low-cost, flexible platform that transforms heterogeneous datasets into standardised Fast Healthcare Interoperability Resources (FHIR) based datasets.

## Project Goal
To build a working prototype of an integration and data-normalisation platform capable of transforming disparate health data formats into standardised FHIR resources, using Ghana-ready data workflows.

### Process Flow Sequence Diagram

' mermaid
sequenceDiagram
    autonumber

    participant Clinic as Clinic System (Simulated)
    participant Transport as Transport Layer (SFTP/API/Folder)
    participant Mirth as Mirth Connect
    participant Mapper as Mapping & FHIR Normaliser
    participant FHIR as HAPI FHIR Server
    participant Viewer as Analytics / Viewer

    Clinic->>Transport: Export patient data (CSV, JSON, HL7, Excel)
    Transport->>Mirth: Send file/upload/API request
    Mirth->>Mirth: Detect new inbound data
    Mirth->>Mapper: Pass raw message for processing

    Mapper->>Mapper: Clean & validate fields<br/>Check required values<br/>Resolve Ghana-specific formats
    Mapper->>Mapper: Map to FHIR resources<br/>Patient / Encounter / Observation
    Mapper-->>Mirth: Return FHIR Bundle (JSON)

    Mirth->>FHIR: POST Bundle to server
    FHIR-->>Mirth: Response (201 Created or Error)

    Mirth->>Clinic: Optional success/failure notification
    FHIR->>Viewer: Provide unified patient view query

    Viewer-->>FHIR: Request dashboard data
    FHIR-->>Viewer: Return normalised patient data
    '

