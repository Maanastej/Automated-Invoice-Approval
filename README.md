#  Power Automate Flow: Microsoft Forms to SharePoint, Dataverse, and File Uploads

This repository contains two integrated Power Automate flows that trigger sequentially upon Microsoft Forms submission. These flows automate the process of data extraction, document handling, storage in SharePoint and Dataverse, and managing approvals.

---

##  Overview

###  Trigger: Microsoft Form Submission
A Microsoft Form submission starts the entire process.

###  Flow 1: **Customer Details**
- Triggered when a new form response is submitted.
- Retrieves details from the submitted form.
- Extracts file references from a file upload question.
- Parses uploaded files (OneDrive location).
- Adds the user’s data and uploaded documents to:
  - A SharePoint List
  - Microsoft Dataverse (CRM system)
  - SharePoint Library (for approval)

###  Flow 2: **Invoice Approval**
- Starts once Flow 1 or Flow 3 completes and stores files in SharePoint.
- Sends a sequential approval request based on logic (e.g., invoice total or other criteria).
- Loops through levels of approval until final confirmation is received.


### Flow 3: **Cloud Unzip**
- Starts when an email arrives on Outlook with a zip file containing multiple invoices.
- It uploads the file to OneDrive and from there uses an Encodian connector to extract the individual invoices.
- It finally puts these files into the same Sharepoint folder which then triggers the invoice approval.

---
##  Components Used

| Component                  | Usage                                                   |
|---------------------------|----------------------------------------------------------|
| Microsoft Forms           | Source of customer/form data input                      |
| OneDrive for Business     | Temporary storage for uploaded files                     |
| SharePoint Online         | Stores metadata (List) and files (Document Library)      |
| Microsoft Dataverse       | Stores structured CRM/customer data                     |
| Power Automate (Flow)     | Automation backbone                                     |

---

##  Flow 1: Form Response Handler

### Key Steps:
1. **Trigger**: New Form Response Submitted
2. **Get Form Response Details**
3. **Extract and Parse Uploaded Files**
4. **Create SharePoint List Item**
5. **Upload Documents to:**
   - SharePoint List (as attachments)
   - Dataverse Table (as binary field)
   - SharePoint Document Library (for approval)

### Conditions:
- If uploaded files are present → run full process
- If no files are uploaded → skip file handling and proceed with SharePoint and Dataverse entries only

---

##  Flow 2: Approval Workflow (Triggered After Flow 1)

### Key Features:
- Dynamically fetches approver levels from a SharePoint List (`ApprovalMatrix`)
- Sends approval requests **one by one**, level by level
- Uses condition checks:
  - If approver doesn't provide a comment → resends the request
  - If comment is valid → proceeds to next level or completes approval

---

##  Benefits
- No manual entry or file download/upload
- Centralized customer and file data in SharePoint and Dataverse
- Enforced audit trail via mandatory comment approvals
- Streamlined multi-level approval without repetition

---

##  Prerequisites

- Microsoft 365 with:
  - Power Automate access
  - SharePoint Online
  - Microsoft Forms
  - Dataverse (optional if CRM integration is desired)

---

##  Security Note
Ensure:
- Proper access rights are granted to Power Automate connections.
- OneDrive paths used in the flow are accessible.
- SharePoint list and library permissions are managed for contributors and approvers.

---

##  Customization Ideas

- Add Power BI dashboards to visualize submissions and approval rates.
- Send confirmation emails to submitters.
- Add adaptive cards in Teams for real-time approvals.

---

##  Contact

For questions, issues, or customization help, reach out to the flow owner or contributor.

---

