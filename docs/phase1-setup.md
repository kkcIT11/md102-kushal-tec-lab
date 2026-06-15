# Entra ID — User and Group Management

**Lab:** Kushaltec Simulation Environment  
**Tenant:** Microsoft 365 E5 Trial  
**Portals:** Microsoft Entra Admin Center · Microsoft 365 Admin Center  
**Exam objective:** MD-102 — Manage identity and access (Entra ID)

---

## Prerequisites

- Global Administrator or User Administrator role in Entra ID
- Access to [entra.microsoft.com](https://entra.microsoft.com)
- Access to [admin.microsoft.com](https://admin.microsoft.com)
- Available M365 E5 licences (Intune + Entra ID P2)
- KT-Grp-Allstaff dynamic group already created in the tenant

---

## Part 1 — Create a New User in Microsoft Entra ID

---

### Step 1 — Navigate to Users and open the New User menu

Go to **Entra ID > Users > All users**, then click **New user > Create new user**.

![Step 1](Screenshots/1.png)

- **Users (1)** selected in the left nav under Entra ID
- **All users (2)** active in the sub-nav
- **Create new user (3)** visible in the dropdown — creates an internal organisational user
- Existing users already in tenant: Kushal KC and Oliver Smith
- Breadcrumb shows path came from **KT-Grp-Allstaff | Members** — navigated here from within the dynamic group context

---

### Step 2 — Fill in the Basics tab

Complete all identity fields on the **Basics** tab.

![Step 2](Screenshots/2.png)

| Field | Value |
|---|---|
| User principal name | `AlexChen` @ [tenant domain] |
| Mail nickname | `AlexChen` — auto-derived (checkbox ticked) |
| Display name | `Alex Chen` |
| Password | Auto-generated (checkbox ticked) |
| Account enabled | Yes (checkbox ticked) |

> Leave **Derive from user principal name** ticked unless you need a custom mail alias. The mail nickname stays in sync with the UPN prefix automatically.

---

### Step 3 — Fill in the Properties tab

Click **Next: Properties** and enter identity and job information.

![Step 3](Screenshots/3.JPG)

| Field | Value |
|---|---|
| First name | `Alex` |
| Last name | `chen` |
| User type | `Member` (default) |
| Job title | *(left blank for this lab)* |
| Department | *(left blank for this lab)* |
| Employee ID | *(left blank for this lab)* |
| Office location | *(left blank for this lab)* |

> User type **Member** = standard internal user. **Guest** is used for external B2B collaborators. In production always populate Department and Job title — these attributes drive dynamic group membership rules.

---

### Step 4 — Review the Assignments tab

Click **Next: Assignments**. No group or role assignments are added at this stage.

![Step 4](Screenshots/4.JPG)

The tab shows **No assignments to display**. This is intentional:

- KT-Grp-Allstaff is a **dynamic group** — Entra ID automatically adds Alex Chen once it evaluates the membership rule against his attributes
- KT-Grp-Operations does not exist yet — it will be created in Part 3

> Up to 20 group or role assignments can be made during user creation. For dynamic groups skip this tab — manual membership added here would be overridden by the dynamic rule anyway.

---

### Step 5 — Review + Create

Click **Next: Review + create** and verify all values before submitting.

![Step 5](Screenshots/5.jpg)

| Field | Value |
|---|---|
| User principal name | `AlexChen` @ [tenant domain] |
| Display name | `Alex Chen` |
| Mail nickname | `AlexChen` |
| Account enabled | Yes |
| First name | Alex |
| Last name | chen |
| User type | Member |
| Groups assigned | None |
| Roles assigned | None |

Click **Create**. The user account is provisioned immediately in the tenant.

---

### Step 6 — Verify the user appears in All users

Navigate back to **Users > All users** to confirm Alex Chen is listed.

![Step 6](Screenshots/6.JPG)

The tenant now shows **3 users found**:

| Display name | User type |
|---|---|
| Alex Chen | Member — newly created |
| Kushal KC | Member — tenant admin |
| Oliver Smith | Member — existing user |

---

## Part 2 — Assign a Microsoft 365 Licence

---

### Step 7 — Assign a licence via Microsoft 365 Admin Center

Switch to **admin.microsoft.com** and assign the licence to Alex Chen.

![Step 7](Screenshots/7.jpg)

- Open **Microsoft 365 Admin Center (1)** at admin.microsoft.com
- Click **Users > Active users (2)** in the left nav
- Check the checkbox next to **Alex Chen (3)** — right-hand panel opens
- Click the **Licenses and apps (4)** tab in the panel

| Field | Value |
|---|---|
| Select location | `Australia` — set this FIRST, required before licence assignment |
| Intune | 23 of 25 licences available — tick to assign |
| Microsoft Entra ID P2 | 23 of 25 licences available — tick to assign |

Tick the required licences and click **Save changes**.

> A usage location is mandatory before any M365 licence can be assigned. Without it the assignment will fail. For Australian tenants always set **Australia** for data residency compliance.

---

## Part 3 — Create a New Security Group in Microsoft Entra ID

---

### Step 8 — Navigate to Groups and click New group

Go to **Entra ID > Groups**, then click **New group** in the toolbar.

![Step 8](Screenshots/8.JPG)

Groups Overview stats visible in this screenshot:

| Metric | Count |
|---|---|
| Total groups | 2 |
| Dynamic groups | 1 (KT-Grp-Allstaff) |
| M365 groups | 0 |
| Security groups | 2 |
| Cloud groups | 2 |
| On-premises groups | 0 |

---

### Step 9 — Configure the new group

Fill in all fields on the **New Group** form.

![Step 9](Screenshots/9.jpg)

| Field | Value |
|---|---|
| Group type | `Security` |
| Group name | `KT-Grp-Operations` |
| Group description | `KT-Grp-Operations` |
| Entra roles assignable | `No` |
| Membership type | `Assigned` |
| Owners | 1 owner selected — Kushal KC |
| Members | 1 member selected |

- **Membership type: Assigned** — members are added and removed manually by an admin. Different from KT-Grp-Allstaff which uses **Dynamic** membership driven by attribute rules.
- **Entra roles assignable: No** — correct for a standard departmental group. Set Yes only for privileged access groups that need to hold Entra directory roles. Requires Entra ID P2.
- **This setting cannot be changed after the group is created** — double-check before clicking Create.

Click **Create**.

---

### Step 10 — Verify both groups in All groups

Navigate to **Groups > All groups** to confirm both groups exist.

![Step 10](Screenshots/10.JPG)

| Name | Group type | Membership type |
|---|---|---|
| KT-Grp-Allstaff | Security | Dynamic |
| KT-Grp-Operations | Security | Assigned |

The tenant now has 2 security groups — dynamic for all-staff policy targeting and assigned for operations team management.

---

## Key Concepts

### Dynamic vs Assigned Membership

| | Assigned | Dynamic |
|---|---|---|
| Membership | Admin manages manually | Entra ID manages via attribute rules |
| Licence required | Entra ID Free | Entra ID P1 or P2 |
| Use case | Small teams, project groups | All-staff, department-based targeting |

### UPN vs Mail Nickname

- **UPN** is the sign-in identity (`AlexChen@domain.com`) used for Entra ID authentication
- **Mail nickname** is the local part of the email address
- Mail nickname auto-derives from UPN prefix by default — only uncheck for a custom alias
- UPN must be unique across the entire tenant

### Usage Location

- Required before any M365 licence can be assigned
- Determines data residency and service availability by country
- For Australian tenants always set **Australia**

### Entra Roles Assignable to Group

- When Yes, the group can hold Entra ID directory roles (e.g. Global Admin, User Admin)
- Requires Entra ID P2 — only enable for privileged access groups
- **Cannot be changed after group creation**

---

## Lab Completion Summary

| Task | Status |
|---|---|
| Created user Alex Chen in Microsoft Entra ID | ✅ |
| Set UPN, display name, and auto-generated password | ✅ |
| Reviewed all wizard tabs before submission | ✅ |
| Verified Alex Chen in All users (3 users total) | ✅ |
| Set usage location to Australia | ✅ |
| Assigned M365 licences to Alex Chen | ✅ |
| Created KT-Grp-Operations — Assigned Security group | ✅ |
| Verified both groups in All groups list | ✅ |

---

*Kushaltec Lab · MD-102 Endpoint Administrator · Phase 1 Setup*  
*Tenant domain, UPNs, and Object IDs redacted.*
