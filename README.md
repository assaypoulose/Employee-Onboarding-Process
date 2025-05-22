# Salesforce HR Employee Onboarding Flow

An automated onboarding system built using Salesforce **Screen Flows**, **Approval Processes**, and **Record-Triggered Flows** to streamline the hiring journey for managers and HR.

---

## 📌 Project Overview

**Project Name:** Employee Onboarding Process (Flow & Approval Process)  
**Goal:** Automate employee onboarding with custom forms, task creation, and approval routing.

### 🔑 Features:
- Custom Object: `Employee__c`
- Screen Flow: Employee Data Collection
- Approval Process: Hiring Approval Workflow
- Auto Task Creation: Onboarding Task Assignments

---

## 🛠 STEP-BY-STEP BUILD GUIDE

### 🔹 STEP 1: Create Custom Object – Employee

1. Setup → Object Manager → Create → Custom Object  
2. Label: `Employee` | Plural: `Employees` | API: `Employee__c`  
3. Enable:
   - Allow Reports  
   - Track Field History  
   - Allow in Activities

---

### 🔹 STEP 2: Add Custom Fields

Add the following fields to `Employee__c`:

- **Full Name** (Text)  
- **Email** (Email)  
- **Phone Number** (Phone)  
- **Department** (Picklist)  
- **Start Date** (Date)  
- **Manager** (Lookup → User)  
- **HR Representative** (Lookup → User)  
- **Status** (Picklist: Draft, Submitted, Approved, Rejected)

#### 🎯 Department Picklist Values:

```
1. Human Resources
2. Finance
3. Sales
4. Marketing
5. Product
6. Engineering
7. Customer Support
8. Legal
9. IT
10. Operations
11. Design
12. Business Development
13. Administration
14. Quality Assurance
15. Procurement
```

---

---

### 🔹 STEP 3: Create Screen Flow – Employee Onboarding

#### ✅ 1. Setup the Flow
- Go to Setup → Flows → New Flow  
- Select: **Screen Flow** → Freeform Canvas

#### ✅ 2. Screen 1: Employee Details
- **Label:** `Employee Details`  
- **Inputs:**
  - Full Name (Text)  
  - Email (Email)  
  - Phone (Phone)  
  - Department (Picklist – Choice Set from `Employee__c.Department__c`)  
  - Start Date (Date)

#### ✅ 3. Screen 2: Assign Manager & HR Rep
- **Label:** `Assign Manager and HR`  
- **Inputs:**
  - Manager (Lookup → User)  
  - HR Representative (Lookup → User)

#### ✅ 4. Create Record – `Employee__c`
- Add a **Create Records** element  
- Map all input fields from screens (use variables if needed)  
- Set `Status__c = 'Submitted'`

#### ✅ 5. Add Confirmation Screen
- Add a **Screen** element labeled `Success`  
- Add display text: _"Employee onboarding request has been submitted for approval."_

#### ✅ 6. Submit for Approval (Entry Criteria-Based)
- Approval Process for `Employee__c` should have entry criteria:  
  `Status__c = 'Submitted'`

#### ✅ 7. Save & Activate the Flow
- Flow Label: `Employee Onboarding Flow`  
- API Name: `Employee_Onboarding_Flow`  
- Click **Activate**

---
## 📊 Final Flow Diagram

```
Start
→ Employee Details (Screen)
→ Assign Manager and HR (Screen)
→ Create Employee Record (Create Records)
→ Success (Screen)
→ Submit for Approval (via Status__c = ‘Submitted’)
→ End
```

---

### 🔹 STEP 4: Create Approval Process – Employee__c

1. Setup → Approval Processes → New Approval Process  
2. Object: `Employee__c`  
3. Steps:
   - Initial Submission Action: Lock Record  
   - Approver: Field = `Manager__c`  
   - Final Approval Action: Update `Status__c = Approved`  
   - Final Rejection Action: Update `Status__c = Rejected`

---

### 🔹 STEP 5: Auto-Create Onboarding Tasks

1. Setup → Flows → New Flow → Record-Triggered Flow  
2. Trigger: Object = `Employee__c`  
   - Entry Condition: `Status__c Is Changed To 'Approved'`
3. Add **Create Records** actions for each onboarding task:
   - Task: Send Welcome Email  
   - Task: Create Salesforce Account  
   - Task: Assign Laptop  
   - Task: Add to Slack/Teams  
4. Object: `Task`  
5. Field Mapping Example:
   - `Subject`: Task title  
   - `Status`: Not Started  
   - `Priority`: Normal  
   - `WhatId`: Employee__c.Id  
   - `OwnerId`: HR Representative (or fixed user/queue)

---

### 🔹 STEP 6: Permissions & Page Layout

- Add `Employee__c` object to Manager and HR Profile permissions  
- Add Flow component to Record Page (optional)  
- Add **Approval History** related list to the page layout

---

## ✅ When Entry Criteria Is Enough

- Use when submission is purely based on field values (e.g., `Status = Submitted`)  
- No logic branching required

---

## 👏 Contribute

Pull requests are welcome!  
For major changes, please open an issue to discuss what you'd like to change.

---

## 📄 License

This project is open source and available under the **MIT License**.

---

## 🧑‍💻 Author

**Assay Poulose Peenikkaparamban**  
Salesforce Admin | Trailhead Explorer  
📩 [assaypoulose16@gmail.com]  
🌐 [https://www.linkedin.com/in/assay-poulose-peenikkaparamban-961911179/](https://www.linkedin.com/in/assay-poulose-peenikkaparamban-961911179/)