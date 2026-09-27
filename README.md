# SOX 404 ITGC User Access Review (UAR) Reconciliation Model
Automated SOX 404 ITGC User Access Review (UAR) reconciliation model using Excel XLOOKUP, dynamic risk-flagging logic using nested IF, and an executive audit sign-off memo.

## Executive Overview
This repository contains an end-to-end **User Access Review (UAR)** audit reconciliation model designed to satisfy 
**SOX 404 IT General Controls (ITGC)** and **Identity & Access Management (IAM)** compliance requirements.

The primary control objective is to verify that system user permissions across production databases (ERP/Active Directory)
align with human resources master records, enforcing the **Principle of Least Privilege** and ensuring prompt credential
revocation upon employee termination.

---

## Key Compliance Controls Tested
* **Deprovisioning & Timely Revocation:** Identifying terminated employees retaining active system access post-departure date.
* **Orphan & Unmapped Accounts:** Flagging active system user IDs that do not map to an authorized HR employee record.
* **Privileged Access Governance:** Isolating administrative and elevated roles ('ERP_Admin') for secondary audit review.
* **Audit-Defensible Documentation:** Generating an executive sign-off summary memo suitable for external audit review (e.g., Big 4/PCAOB standards). 

---

## Workbook Architecture & Structure

The accompanying Excel workbook ('SOX_404_UAR_Reconciliation_Template_2026.xlsx') is structured across four functional worksheets:

| Worksheet | Role | Description |
| :--- | :--- | :--- |
| '1. HR_Master_Roster' | Source of Truth | Master employee directory containing employee status ('Active'/'Terminated') and official separation dates. |
| '2. System_Access_Dump' | Target System Extract | Production system entitlement export containing User IDs, assigned security roles, and last login timestamps. |
| '3. UAR Reconciliation' | Audit Working Paper | Core testing sheet utilizing dynamic cross-sheet lookup formulas, multi-condition risk logic, and remediation tagging. |
| '4. Audit_Summary_Signoff' | Executive Deliverable | Aggregated dashboard and formal audit memorandum summarizing findings, testing metrics, and control sign-off. |

---

## Technical Excel Implementation & Formulas

### 1. Cross-Sheet Data Mining ('XLOOKUP')
To map target system accounts dynamically against the HR master roster without relying on legacy 'VLOOKUP':

Excel

=XLOOKUP(B2, HR_Table[Emp_ID], HR_Table[Full_Name], "UNKNOWN / NO HR RECORD")

### 2. Automated Compliance Risk-Flagging Logic
A nested conditional formula evaluates user status, security role, and HR mapping to assign automated risk tiers:

Excel

=IF(E2="Terminated", "CRITICAL: Terminated User Active", 
  IF(E2="UNMAPPED", "HIGH: Orphan/Service Account", 
   IF(AND(C2="ERP_Admin", E2="Active"), "REVIEW: Privileged Access", "PASS: Valid Access")))

### 3. Risk Classification Output Matrix

| Risk Tier | Trigger Condition | Automated Flag | Required Remediation Action |
| :--- | :--- | :--- | :--- |
| Critical | Employee HR Status='Terminated' | 'CRITICAL: Terminated User Active' | Immediate revocation ticket issued to Helpdesk |
| High | Account Missing from HR Roster | 'HIGH: Orphan/Service Account' | Quarantine account: Request vendor or service owner validation |
| Medium | Active User assigned 'ERP_Admin' role | 'REVIEW: Privileged Access' | Escalated to System Owner for privilege re-authorization |
| Low | Active Employee with Standard Access | 'PASS: Valid Access' | Retain access |

## How to Review this Portfolio Deliverable

1. Download the primary Excel model: "SOX_404_UAR_Reconciliation_Project.xlsx".
2. Inspect the structured table references and dynamic formulas on the "UAR_Reconciliation" tab.
3. Review the executive-ready PDF report: "SOX_404_UAR_Audit_Project.pdf"
