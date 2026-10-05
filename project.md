ABAP Cloud Portfolio Project
Smart Travel & Booking Management
1. Project Goal

Build one professional, end-to-end SAP ABAP Cloud application that demonstrates practical C_ABAPD knowledge and modern ABAP Cloud development to employers/stakeholders.

The project should demonstrate:

ABAP Cloud

Clean Core

CDS View Entities

ABAP SQL

RAP

Managed Business Objects

Composition

Behavior Definitions

Validations

Determinations

Actions

Feature Control

EML

Draft

Authorization / DCL

OData V4

Fiori Elements

ABAP Unit

ATC

Git/GitHub

Business-oriented application design

2. Business Scenario

Build a Travel Management System for a fictional company:

ABAP Travel Services

Employees create business trips and add bookings such as:

Flight

Hotel

Train

Car Rental

Each travel has a lifecycle:

DRAFT
  ↓
SUBMITTED
  ↓
APPROVED
  ↓
COMPLETED

or

SUBMITTED
  ↓
REJECTED

or

DRAFT
  ↓
CANCELLED

3. Main Business Object
Travel

Fields:

Travel ID
Employee
Destination
Start Date
End Date
Status
Currency
Total Cost
Description
Created By
Created At
Last Changed By
Last Changed At


Example:

Travel:       10001
Employee:     Farhad
Destination:  Berlin
Start Date:   10.11.2026
End Date:     14.11.2026
Status:       Draft
Currency:     EUR
Total Cost:   930.00

4. Child Business Object
Booking

A Travel can contain multiple bookings.

Travel
  │
  └── Booking


Booking fields:

Booking ID
Travel ID
Type
Description
Start Date
End Date
Amount
Currency
Provider


Booking types:

FLIGHT
HOTEL
TRAIN
CAR_RENTAL


Example:

Flight      €250
Hotel       €600
Train        €80
----------------
Total       €930

5. Optional Third Level

Add Passenger if you want to demonstrate deeper composition.

Travel
  │
  └── Booking
       │
       └── Passenger


Passenger:

Passenger ID
First Name
Last Name
Email


This is optional for the first version.

6. CDS Data Model

Create CDS entities such as:

ZI_Travel
ZI_Booking
ZI_Passenger
ZI_Employee
ZI_Hotel


Use:

Associations

Composition

CDS annotations

Semantic keys where appropriate

Associations instead of unnecessary joins

Appropriate data types

Reuse of suitable SAP data elements where available

Conceptual model:

ZI_Travel
   │
   ├── _Employee
   │
   └── _Booking
          │
          ├── _Passenger
          └── _Hotel

7. RAP Business Object

Create a RAP business object around Travel.

Example structure:

Travel
  │
  └── Booking


Use:

Managed RAP
Locking
Authorization
Draft
Composition


The Travel entity is the root.

Booking is a composition child.

8. Behavior Definition

Support:

Create
Update
Delete


for appropriate entities.

Example conceptual behavior:

Travel
 ├── create
 ├── update
 ├── delete
 ├── action submit
 ├── action approve
 ├── action reject
 ├── action cancel
 └── action recalculate

Booking
 ├── create
 ├── update
 └── delete

9. Validation

Implement business validations.

Examples:

Invalid dates
Start Date > End Date


→ Reject.

Negative amount
Booking Amount < 0


→ Reject.

Empty destination
Destination = empty


→ Reject.

Submit without booking
Travel has no bookings


→ Do not allow submission.

Invalid state transition

Do not allow:

APPROVED → SUBMIT


or other inappropriate transitions.

10. Determinations

Use determinations to calculate or derive values.

Main example:

Booking changed
       ↓
Determination
       ↓
Recalculate Travel Total


Example:

Flight      €250
Hotel       €600
Train        €80
----------------
Travel Total €930


Other possible determinations:

Default Currency
Default Status
Calculate Number of Nights
Calculate Total Booking Cost

11. Actions

Create meaningful RAP actions.

Submit
DRAFT → SUBMITTED

Approve
SUBMITTED → APPROVED

Reject
SUBMITTED → REJECTED

Cancel
DRAFT/SUBMITTED → CANCELLED

Recalculate

Recalculate travel costs.

The actions should contain real business logic rather than simply changing a field.

12. Feature Control

Control which operations are available depending on status.

Example:

DRAFT
 ├── Edit       ✓
 ├── Delete     ✓
 ├── Submit     ✓
 └── Approve    ✗

SUBMITTED
 ├── Edit       ✗
 ├── Delete     ✗
 ├── Submit     ✗
 └── Approve    ✓

APPROVED
 ├── Edit       ✗
 ├── Delete     ✗
 └── Cancel     possibly ✓


Demonstrate that the UI automatically reflects the business state.

13. Draft

Enable draft processing.

User:

Create Travel
      ↓
Enter information
      ↓
Add bookings
      ↓
Close application
      ↓
Return later
      ↓
Continue editing draft


Demonstrate that incomplete business transactions can be safely stored as drafts.

14. EML

Use Entity Manipulation Language where appropriate.

Demonstrate:

READ ENTITIES
MODIFY ENTITIES
COMMIT ENTITIES


Use EML in a realistic scenario, for example:

Create Travel
      ↓
Create Booking
      ↓
Read Travel
      ↓
Calculate total
      ↓
Update Travel


Also demonstrate how EML interacts with RAP transactional behavior.

15. Authorization

Implement authorization according to business roles.

Example:

Employee

Can:

View own travels
Create own travel
Edit own draft
Submit own travel

Manager

Can:

View department travels
Approve/reject submitted travels

Travel Administrator

Can:

View all travels
Maintain travel-related master data


Use appropriate RAP authorization concepts and CDS/DCL where applicable.

16. CDS/DCL Security

Demonstrate that authorization is not only a UI concept.

Conceptually:

User
  ↓
Authorization
  ↓
DCL / RAP Authorization
  ↓
Allowed business data


For example:

Employee A
   ↓
Can see Employee A's travels

Manager A
   ↓
Can see department travels

17. Projection Layer

Create transactional and projection layers appropriately.

Conceptual structure:

Interface CDS
       ↓
Behavior
       ↓
Projection CDS
       ↓
Projection Behavior
       ↓
Service


Example:

ZI_Travel
ZC_Travel


Use the interface layer for the business model and projection layer for the consumption scenario.

18. Service Definition

Create a service definition exposing the required projection entities.

Example concept:

ZUI_TRAVEL


Expose:

Travel
Booking
Passenger


only where necessary.

Do not expose internal implementation entities unnecessarily.

19. Service Binding

Create an OData V4 service binding.

Conceptually:

RAP
 ↓
Projection
 ↓
Service Definition
 ↓
Service Binding
 ↓
OData V4


Use the appropriate binding type available in your development environment.

20. Fiori Elements

If your environment supports it, create a Fiori Elements application.

Main list:

Travel Management

ID       Employee   Destination   Status
10001    Farhad     Berlin        Draft
10002    Anna       Paris         Approved
10003    John       Munich        Submitted


Object page:

Travel #10001

Employee:      Farhad
Destination:   Berlin
Start:         10.11.2026
End:           14.11.2026
Status:        Draft
Total:         €930

Bookings
--------------------------------
Flight          €250
Hotel           €600
Train            €80
--------------------------------
Total           €930

[Submit]
[Delete]
[Recalculate]


Use CDS/UI annotations where appropriate rather than unnecessarily coding a custom UI.

21. Business Rules

Implement realistic rules.

Examples:

Start Date cannot be after End Date.

Booking amount cannot be negative.

Hotel booking requires start/end dates.

Travel must contain at least one booking before submission.

Only submitted travels can be approved.

Only authorized users can approve.

Approved travel cannot be freely edited.

Total cost is derived from bookings.

Currency must be consistent where required.

22. Optional Advanced Feature

Add a travel-budget warning.

Example:

Total > €1,000
       ↓
Warning
       ↓
"Travel exceeds standard travel budget."


Or:

Hotel > €200/night
       ↓
Warning


This can demonstrate additional business logic.

Do not add AI merely for the sake of saying the project contains AI.

A clean, well-designed RAP application is more valuable.

23. ABAP SQL

Demonstrate modern ABAP SQL where appropriate.

Examples:

SELECT
JOIN
WHERE
GROUP BY
ORDER BY
Aggregations


Use CDS for reusable data models and ABAP SQL where application-level data access is appropriate.

Demonstrate understanding of code pushdown and why logic belongs in CDS/database versus ABAP.

24. ABAP Unit

Create meaningful tests.

Test:

Valid travel dates
Invalid travel dates
Negative booking
Total calculation
Submit action
Cancel action
Invalid status transition
Business rules


Target example:

Tests:   25
Passed:  25
Failed:   0


Do not write tests only to increase the test count.

Each test should verify a real business rule.

25. ATC / Quality

Run appropriate ATC checks.

Fix:

Syntax problems
Cloud restrictions
Released API violations
Performance problems
Quality warnings


The project should demonstrate that you understand:

ABAP Cloud
+
Clean Core
+
Released APIs
+
Quality checks

26. Clean Core

Make this a major project principle.

Document:

No SAP standard modifications
No unnecessary unreleased API usage
Use released APIs
Use extension mechanisms
Use ABAP Cloud principles
Use RAP
Use CDS


README statement:

This project follows ABAP Cloud and Clean Core principles and avoids modifications to SAP standard objects.

27. Git / GitHub

Create a professional repository:

abap-cloud-travel-management/
│
├── README.md
│
├── architecture/
│   ├── architecture.png
│   └── data-model.png
│
├── docs/
│   ├── business-process.md
│   ├── authorization.md
│   ├── testing.md
│   └── design-decisions.md
│
└── src/
    ├── CDS
    ├── RAP
    └── Service


Use meaningful commits.

Examples:

Initial travel data model
Add travel CDS entities
Implement RAP behavior
Add booking composition
Add validations
Add determinations
Add travel actions
Add authorization
Add draft
Add OData service
Add ABAP Unit tests
Fix ATC findings

28. Architecture Diagram

Create one clean architecture diagram:

                 Fiori Elements
                       │
                       ▼
                    OData V4
                       │
                       ▼
                Service Binding
                       │
                       ▼
                Service Definition
                       │
                       ▼
               Projection Layer
                       │
                       ▼
                 RAP Business
                    Object
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
     Validation   Determination   Actions
          │            │            │
          └────────────┼────────────┘
                       ▼
                     EML
                       │
                       ▼
                 CDS Data Model
                       │
                       ▼
                  HANA Database


Put this diagram in the GitHub README.

29. README

The README should contain:

# ABAP Cloud Travel Management

## Business Problem

## Solution

## Features

## Architecture

## Data Model

## RAP Business Object

## Business Rules

## Authorization

## Clean Core

## Testing

## ATC

## Screenshots

## Technology Stack

## Design Decisions

## Lessons Learned

30. Five-Minute Stakeholder Demo

Prepare a short demonstration.

Minute 1 — Business Problem

Explain:

Employees need to create and manage business trips and bookings while managers approve submitted trips.

Minute 2 — Create

Create:

Travel
Berlin
10–14 November


Add:

Flight €250
Hotel €600
Train €80


Show:

Total = €930

Minute 3 — Business Logic

Enter invalid data.

Show validation.

Change booking.

Show determination recalculating the total.

Minute 4 — Workflow

Show:

Draft
 ↓
Submit
 ↓
Submitted
 ↓
Approve
 ↓
Approved


Demonstrate feature control.

Minute 5 — Architecture

Show:

CDS
 ↓
RAP
 ↓
Behavior
 ↓
EML
 ↓
Service
 ↓
OData V4
 ↓
Fiori Elements


Explain that the application follows ABAP Cloud and Clean Core principles.

31. Skills Demonstrated
Skill	Demonstrated
Modern ABAP	Yes
ABAP Cloud	Yes
CDS	Yes
ABAP SQL	Yes
RAP	Yes
Managed BO	Yes
Composition	Yes
Validation	Yes
Determination	Yes
Actions	Yes
Feature Control	Yes
Authorization	Yes
DCL	Yes
Draft	Yes
EML	Yes
OData V4	Yes
Fiori Elements	Yes
ABAP Unit	Yes
ATC	Yes
Clean Core	Yes
Git/GitHub	Yes
Architecture	Yes
Business Analysis	Yes
32. Recommended Development Order

Do NOT build everything simultaneously.

Build in this order:

1. Business requirements
        ↓
2. Data model
        ↓
3. CDS entities
        ↓
4. Associations
        ↓
5. RAP root/child BO
        ↓
6. Basic CRUD
        ↓
7. Validations
        ↓
8. Determinations
        ↓
9. Actions
        ↓
10. Feature control
        ↓
11. Draft
        ↓
12. Authorization
        ↓
13. EML
        ↓
14. Projection
        ↓
15. Service definition
        ↓
16. Service binding
        ↓
17. Fiori Elements
        ↓
18. ABAP Unit
        ↓
19. ATC
        ↓
20. Documentation
        ↓
21. GitHub
        ↓
22. Stakeholder demo

33. Final Definition of Done

The project is finished when you can honestly say:

I designed and implemented a business application using SAP ABAP Cloud and RAP.

And you can demonstrate:

✓ CDS data model
✓ RAP business object
✓ Composition
✓ CRUD
✓ Validation
✓ Determination
✓ Actions
✓ Feature control
✓ Draft
✓ Authorization
✓ DCL
✓ EML
✓ Projection
✓ OData V4
✓ Fiori Elements
✓ ABAP Unit
✓ ATC
✓ Clean Core
✓ GitHub documentation


Most importantly:

You should be able to explain why every major design decision was made.

The objective is not to create the biggest project.

The objective is to create one small but professional application that proves you understand modern ABAP Cloud development.
