# Power Automate Workflow Suite: Microsoft Forms ➝ SharePoint, Dataverse, Power Apps Login & D365 Invoice Approval

This repository contains a complete low-code automation solution for invoice processing using Power Automate, SharePoint, Dataverse, Power Apps, and (D365 F&O). It includes:

- ZIP File extraction & document storage
- AI processing of invoices for data capture
- Power Apps user login and approval
- Dataverse-integrated user management
- Multi-level invoice approval via email from D365 workflows

---

## Workflow Overview
#### **Flow 1: Customer Details**
- Trigger: New Microsoft Form response
- Actions:
  - Extract form data and uploaded files
  - Store metadata in SharePoint List
  - Store file content in Dataverse binary column
  - Upload PDFs to SharePoint Library

#### **Flow 2: Invoice Approval (Triggers after Flow 1/Flow 3)**
- Trigger: File saved in SharePoint Document Library
- Fetch approvers dynamically from SharePoint `ApprovalMatrix`
- Loop through approval levels sequentially:
  - Waits for comment input
  - Skips levels based on amount or logic
  - Resends if approval if the comment is missing

#### **Flow 3: Cloud Unzip for Zip Invoices**
- Trigger: New email with `.zip` attachment
- Uploads the zip to OneDrive
- Uses Encodian connector to extract multiple PDF invoices
- Moves invoices to SharePoint Library

---

## Power Apps: User Login & Workflow Dashboard

### 🔹 Power Apps Features:
- **Login screen**:
  - Username and password inputs
- **Forgot password**:
  - Reset mechanism with email validation using a time-limited OTP
  - Ensures a previous password cant be used
- **Sign-up screen**:
  - Email and password inputs
  - Password strength checker
- **Workflow Dashboard**:
  - Shows invoice approval status
  - View recent approvals linked by Workflow ID

###  Dataverse Integration:
- Table: User Data (All login related details), Business Events (All approval workflow related details)
- Used in Power Apps to:
  - Authenticate users
  - Fetch related records for display

---

##  D365 F&O Flow: Business Event-Driven Invoice Approval

###  Trigger:
- D365 Business Event: `Workflow_VendInvoiceRecordingApproval_WorkItem`
- Legal Entity: USMF

###  Actions:
1. **Parse business event payload**
2. **Validate invoice work item** using `WorkflowWorkItems.validate`
3. **Conditionally send approvals**:
   - If subject = `Record returned` → send email with "review required"
   - If subject = `Change requested` → send approval using "Start and wait for an approval"
4. **Handle Approvals**:
   - Loop through responses
   - Update D365 using `WorkflowWorkItems.complete`
   - Store status and details in Dataverse table (`cre96_businessevents`)

###  Email Notifications:
- Sent to approver with:
  - Workflow instructions
  - Last comment
  - Direct link to D365 work item

##  Components Used

| Component                        | Purpose                                                   |
|----------------------------------|-----------------------------------------------------------|
| Microsoft Forms                 | Customer input                                            |
| SharePoint Online               | Metadata & Document storage                               |
| OneDrive for Business           | Temporary file handling (zip extractions)                 |
| Microsoft Dataverse             | Structured storage for login, logs, and invoice data      |
| Power Apps                      | Login interface and workflow dashboard                    |
| Power Automate (Cloud Flows)    | Orchestration and automation backbone                     |
| Dynamics 365 F&O (Business Events) | Source for invoice approvals based on workflow events |

---

##  Benefits

- Centralized document storage and approval history
- Secure Power Apps login using Dataverse
- Real-time invoice processing from SharePoint or D365
- Multi-level approval logic with email + approval tracking
- Reusable building blocks for future invoice/HR/workflows

---

##  Prerequisites

- Microsoft 365 tenant with:
  - SharePoint Online
  - Power Apps
  - Power Automate
  - Microsoft Forms
  - Dataverse
  - Dynamics 365 F&O (for Flow 4)

---


