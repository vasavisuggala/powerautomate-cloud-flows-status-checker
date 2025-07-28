# 🚦 Power Automate – Cloud Flows Status Checker

![Power Platform](https://img.shields.io/badge/built%20for-Power%20Platform-purple?logo=microsoft&logoColor=white)
![Power Automate](https://img.shields.io/badge/type-Power%20Automate-blue?logo=microsoft-power-automate&logoColor=white)
![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)

A scheduled admin flow that scans all cloud flows in a specific environment and alerts admins when any flows are unexpectedly **turned off**.

---

## 🧩 Problem Statement

In enterprise environments, **cloud flows may silently stop** due to:
- Ownership changes (e.g., creator account disabled)
- License expiry or reallocation
- Connector authentication failures
- Unhandled runtime errors or system auto-disable

Without proactive monitoring, **critical automations may fail silently**, resulting in missed business operations, compliance gaps, or customer dissatisfaction.

---

## ✅ Proposed Solution

This Power Automate flow:

- ✅ **Lists** all flows across environments using **List Flows as Admin (V2)**
- 🔍 **Identifies** flows that are currently **disabled or turned off**
- 📧 **Sends automated email notifications** with an HTML table showing the affected flows
- 🔁 **Runs on a scheduled basis** (e.g., daily or weekly)
- 🎯 Helps Admins **take early action** before business teams are impacted

---

## 💼 Business Value

| Benefit        | Description                            |
|----------------|----------------------------------------|
| 🛡️ Governance   | Helps enforce platform health checks    |
| 📣 Awareness    | Reduces risk of unnoticed failures      |
| ⏱️ Time-Saving  | No need to manually inspect every flow |
| 🧠 Insightful   | Can be extended with analytics and Teams alerts |

---

## 🔧 Flow Architecture

### 🔁 Trigger
- `Recurrence`: Daily or weekly

### 🔄 Actions

| Step | Action                      | Description                              |
|------|-----------------------------|------------------------------------------|
| 1    | **List Flows as Admin (V2)**| Lists all flows in the tenant            |
| 2    | **Filter Array**            | Limits results to a specific environment |
| 3    | **Filter Array (Disabled)** | Captures flows with status = `"Stopped"` |
| 4    | **Compose**                 | Holds filtered flow details              |
| 5    | **Condition**               | If disabled flows exist...               |
| 6a   | ✅ True Branch               | Parse JSON → Create HTML Table → Send Email |
| 6b   | ❌ False Branch              | Do nothing                               |

---

## 📧 Sample Email Output

An HTML table of disabled flows is sent:

| Flow Name       | Owner Email       | Status   |
|-----------------|-------------------|----------|
| `Sync to ERP`   | `user1@org.com`   | Disabled |
| `Email Digest`  | `admin@org.com`   | Disabled |

---

## 🧠 Flow Pseudo Code

```plaintext
🔁 Trigger
-------------
Start flow on a scheduled recurrence (e.g., daily at 9:00 AM)

🧩 Step 1: List All Flows
-------------------------
Use action: "List Flows as Admin (V2)"
→ Fetch all flows across all environments in the tenant

🔍 Step 2: Filter by Environment
-------------------------------
Use Filter Array:
→ Condition: Environment.DisplayName == '<YourEnvironmentName>'

🚫 Step 3: Filter for Disabled Flows
-----------------------------------
Use Filter Array:
→ Condition: Flow.properties.state == 'Stopped'

🔀 Step 4: Check for Any Disabled Flows
--------------------------------------
IF length of filtered list > 0 THEN

    ✨ Step 4.1: Parse and Format Flow Details
    ------------------------------------------
    → Use 'Compose' or 'Parse JSON' to extract:
        - Flow Name
        - Owner Email
        - Status

    🧾 Step 4.2: Create HTML Table
    -----------------------------
    → Use 'Create HTML Table' action

    📧 Step 4.3: Send Notification Email
    -----------------------------------
    → Use 'Send an email (V2)' with:
        - Subject: "[ALERT] One or more flows are disabled"
        - Body: HTML-formatted table of flow details

ELSE

    ✅ Step 5: No Disabled Flows Found
    ----------------------------------
    → Optionally log: "All flows are active"

🛑 End Flow
```


---

## 👥 Contributors

- [Vasavi](https://github.com/vasavisuggala)  
- [Dileep](https://www.linkedin.com/in/dileepsuggala/)
