## Business Requirement Document

### 1. Introduction

The purpose of this document is to outline the business requirements for enhancing the Attendance Records, Project Management, and related modules in Odoo. The modifications include integrating Azure AD OpenID Connect for authentication and enhancing functionality to support attendance tracking, project code generation, purchase order, invoice, and quotation management.

### 2. Project Objectives

The key objectives of this project are:

- To enhance the attendance management functionality with geo-location-based project sign-in.
- To enable integration with Azure AD OpenID Connect for seamless authentication.
- To automate project code generation, reducing manual errors and ensuring consistency.
- To enhance the Purchase Order, Invoice, and Quotation modules for improved linkage with projects.

### 3. Scope

#### In-Scope
- Enhancements to the Attendance Records module to incorporate geo-location functionality for sign-in.
- Integration of Azure AD OpenID Connect for user authentication.
- Development of automated project code generation functionality.
- Modifications to the Project Management, Purchase Order, Invoice, Customer Order and Quotation modules to align with new project code requirements.

#### Out-of-Scope
- Modifications to mobile application functionality.
- Any changes beyond the specified modules.

### 4. Functional Requirements

#### 4.1 Attendance Records

##### 4.1.1 Sign-In
The system shall allow users to sign in to a project(s) they will be working on.

**Key Points**:

- Only authenticated users shall be allowed to sign in.
- There are two types of workers:
    - **Internal User**: Permanent employees of the organization.
    - **External Worker**: Part-time or freelance workers.
- External users must be registered in the user database before they can sign in.
- The project list shall be fetched from the Project Management module.
- When storing attendance data in the database:
    - The system shall calculate late attendance and overtime for internal users.
    - External workers shall only have their worked hours recorded, without late or overtime calculations.

**Sign-In Process**:

1. An authenticated user clicks the `Sign-In` button to initiate the sign-in process.
2. The system shall retrieve the user's geo-location.
3. The system shall list all available projects within a 500-meter radius of the user's current geo-location.
4. The user must select at least one project (multiple selections are allowed).
5. The system shall submit the data to the back-end.
6. The system shall store user information, geo-location, and selected project(s) in the database.
7. The system shall display a `Sign-In Success` prompt to the user.

**Validation Rules**:

- If the user denies permission to access their current location or if the application cannot obtain the geo-location, the operation shall be rejected.
- If no available projects are found within 500 meters of the user's location, the operation shall be rejected.
- If the user does not select any project, the operation shall be rejected.

##### 4.1.2 Modify Sign-In Records
- Authorized users shall be allowed to modify attendance records.
- Any modifications made to sign-in records shall be logged in the activity log.

##### 4.1.3 Salary Calculation
- **Internal Users**: Salaries shall be calculated on a monthly basis, without penalties for late attendance or additional payments for overtime.
- **External Workers**: Salaries shall be calculated on an hourly basis. The hourly pay rate shall be retrieved from the User module.
- If working hours fall between 12:00 AM and 6:00 AM, external workers shall be entitled to double pay for those hours.

##### 4.1.4 Modification on User Module to Support Salary Calculation
To support the new salary calculation requirements, the User module shall be modified as follows:

- The system shall identify whether the user is an internal user or an external worker.
- Internal users shall have their attendance marked for late arrivals, but no penalties or overtime payments shall be applied.
- External workers shall be paid on an hourly basis, without late penalties or overtime calculations.

##### 4.1.5 Supervisor Intervention
In cases where a user cannot sign in due to connectivity issues or other reasons, the project supervisor shall be able to mark the user as signed in. The system shall provide a user interface for this operation.

##### 4.1.6 Attendance Report
- The system shall generate reports for attendance records, including project-wise attendance details, working hours, and salary calculations.

#### 4.2 Integrate Azure AD OpenID Connect
- The system shall be modified to accept Azure AD as an authentication provider using OpenID Connect for user authentication.

### 5. Project Management

#### 5.1 Project Code Generation
- The system shall generate project codes automatically, and manual modifications shall not be allowed.
- The generated project code may vary for each customer, based on the customer profile.
- In some cases, customers may require a different code generation method, which can be overridden through customer profile settings.

**Default Project Code Pattern**: `CCCTDDDDDDV`

- **CCC**: Customer code, generated using the first three characters of the customer's name (e.g., "Benning Limited" becomes "BEN"). Overrides shall be allowed in the Customer Management module if multiple customers share the same first three characters.
- **T**: Project type (e.g., “T” for trading, “I” for installation).
- **DDDDDD**: Current date in 'YYMMDD' format.
- **V**: Sequence number of projects created on that date (e.g., 1 = A, 2 = B).

#### 5.2 Create Project
- Users shall be able to create any number of projects.
- Users shall select a customer from the customer list when creating a project.
- The project code shall be generated when the project is saved.
- **Required Information**:
    - Customer
    - Project name
    - Project type (e.g., trading, installation)
    - Project supervisor (multiple internal users allowed)
- **Optional Information**:
    - Site location(s) (multiple locations allowed)

#### 5.3 Modify Project
- Authorized users shall be able to modify any field of a project except the project code.

#### 5.4 Project Dashboard
The project dashboard shall display summarized information and serve as an entry point for submodules such as Purchase Orders, Invoices, Quotations, Expenses, Customer Order and Worker details.

- The dashboard shall have a visual user interface.
- It shall display:
    - Overall project cost.
    - Total working days and number of workers.
    - Total salary expenditure.
    - List of Purchase Orders (POs).
    - List of Invoices.
    - List of Quotations.

### 6. Purchase Order Integration
Modify the Purchase Order (PO) module:

- Each PO shall be linked to a Project.
- Users shall select a project when creating or modifying a PO.
- The project code shall not be entered manually; it must be selected using a drop-down list or auto-complete control.
- **PO Number Pattern**: `POCCCTDDDDDDV-S`
    - **PO**: Fixed prefix.
    - **CCCTDDDDDDV**: Project code.
    - **-S**: Sequence number of the PO for that project.

### 7. Invoice Integration
Modify the Invoice module:

- Each invoice shall be linked to a Project.
- Users shall select a project when creating or modifying an invoice.
- The project code shall not be entered manually; it must be selected using a drop-down list or auto-complete control.
- **Invoice Number Pattern**: `INCCCTDDDDDDV-S`
    - **IN**: Fixed prefix.
    - **CCCTDDDDDDV**: Project code.
    - **-S**: Sequence number of the invoice for that project.

### 8. Quotation Integration
Modify the Quotation module:

- Each quotation shall be linked to a Project.
- Users shall select a project when creating or modifying a quotation.
- The project code shall not be entered manually; it must be selected using a drop-down list or auto-complete control.
- **Quotation Number Pattern**: `INCCCTDDDDDDV-S`
    - **IN**: Fixed prefix.
    - **CCCTDDDDDDV**: Project code.
    - **-S**: Sequence number of the quotation for that project.

### 9. Customer Order Integration
Modify the Customer Order module:

- Each Customer Order shall be linked to a Project.
- Users shall select a project when creating or modifying a Customer Order.
- The project code shall not be entered manually; it must be selected using a drop-down list or auto-complete control.
- **Customer Order Number Pattern**: `COCCCTDDDDDDV-S`
    - **CO**: Fixed prefix.
    - **CCCTDDDDDDV**: Project code.
    - **-S**: Sequence number of the quotation for that project.

### 10. Non-Functional Requirements

#### 10.1 Performance
- The system shall respond to user interactions within 2 seconds for all operations.
- Geo-location checks shall be completed within 3 seconds during sign-in.

#### 10.2 Security
- All user information, including geo-location data, shall be encrypted in transit.
- The system shall ensure role-based access control for attendance modifications and project creation.

#### 10.3 Usability
- The user interface shall be intuitive and easy to navigate for both internal and external users.
- Error messages shall be clear and provide guidance on how to resolve issues.

### 11. Risks and Assumptions

#### 11.1 Risks
- **Geo-location Accuracy**: Users may experience issues with geo-location accuracy, affecting their ability to sign in.
- **Connectivity**: Users may not always have internet connectivity, impacting real-time attendance recording.

#### 11.2 Assumptions
- Users shall have internet access during sign-in operations.
- The project locations are accurately stored and maintained in the system.

### 12. Dependencies
- Integration with Azure AD OpenID Connect for authentication.
- Accurate project geo-location data in the Project Management module.

### 13. Timeline
- **Requirement Gathering**: 1 week
- **Development**: 3 weeks
- **Testing and Quality Assurance**: 2 weeks
- **Deployment**: 1 week

### 14. Budget
The budget for this project includes development, testing, and deployment costs, as well as any infrastructure enhancements required to support new functionality.
