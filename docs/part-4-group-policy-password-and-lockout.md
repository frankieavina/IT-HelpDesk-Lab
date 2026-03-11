# Part 4 - Group Policy Overview, Password Policy and Account Lockout

## Overview
In this section, we explore Group Policy Management in Active Directory. We cover how to reset user passwords, disable and enable accounts, configure logon hours, and enforce domain-wide password and account lockout policies using the Group Policy Management Console (GPMC).

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM | Domain Controller / AD DS / DNS | 10.0.0.10 |
| Windows 11 VM | Domain-joined Workstation | 10.0.0.x |
| Domain | example.com | — |

---

## Part 1 — Managing User Accounts in Active Directory

### Resetting a User Password

1. On **Windows Server 2022**, open **Server Manager**
2. Click **Tools** → **Active Directory Users and Computers**
3. Expand **`example.com`** → click **Users**
4. Right-click the user → **Reset Password**
5. Enter the new password and confirm it
6. Optionally check **"User must change password at next logon"** for security
7. Click **OK**

> 💡 **Help Desk Tip:** Password resets are one of the most common IT help desk tickets in a real enterprise environment. Being able to do this quickly and correctly is a core skill.

---

### Disabling and Enabling a User Account

**To Disable:**
1. In **Active Directory Users and Computers**, right-click the user → **Disable Account**
2. Click **OK** to confirm

**To Enable:**
1. Right-click the disabled user → **Enable Account**
2. Click **OK** to confirm

> 💡 **Note:** When an account is disabled, you will notice a **downward arrow icon** on the user's account in Active Directory Users and Computers. This is a quick visual indicator that the account is inactive. Disabled accounts are commonly used when an employee goes on leave or is terminated, rather than deleting the account entirely.

---

### Configuring Logon Hours

Logon Hours restricts when a user is allowed to log into the domain — useful for limiting access outside of business hours.

1. In **Active Directory Users and Computers**, right-click the user → **Properties**
2. Go to the **Account** tab
3. Click **Logon Hours**
4. Select the days and hours you want to **allow or deny** access
5. Click **OK** → **Apply**

> 💡 **Help Desk Tip:** In a real environment, logon hour restrictions are often used for contractors or part-time employees who should only have access during specific shifts.

---

## Part 2 — Group Policy Management

### Opening Group Policy Management Console

1. On **Windows Server 2022**, open **Server Manager**
2. Click **Tools** → **Group Policy Management**
3. In the left panel expand:
   - **Forest** → **Domains** → **`example.com`**
   - Click **Group Policy Objects**
   - Right-click **Default Domain Policy** → **Edit**

This opens the **Group Policy Management Editor**.

---

### Configuring Password Policy

1. In the Group Policy Management Editor, navigate to:

```
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Account Policies
                └── Password Policy
```

2. Configure the following settings:

| Policy Setting | Recommended Lab Value | Description |
|---|---|---|
| Maximum Password Age | 30 days | Forces users to change password every 30 days |
| Minimum Password Age | 1 day | Prevents users from immediately changing back |
| Minimum Password Length | 8 characters | Requires passwords to be at least 8 characters |
| Password Must Meet Complexity Requirements | Enabled | Requires uppercase, lowercase, numbers, symbols |
| Enforce Password History | 5 passwords | Prevents reuse of the last 5 passwords |

3. Double-click each setting → **Define this policy setting** → enter your value → **OK**

---

### Configuring Account Lockout Policy

1. In the Group Policy Management Editor, navigate to:

```
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Account Policies
                └── Account Lockout Policy
```

2. Configure the following settings:

| Policy Setting | Recommended Lab Value | Description |
|---|---|---|
| Account Lockout Threshold | 3 invalid logon attempts | Locks account after 3 failed login attempts |
| Account Lockout Duration | 15 minutes | How long the account stays locked |
| Reset Account Lockout Counter After | 15 minutes | Resets the failed attempt counter after 15 min |

> 💡 **Help Desk Tip:** Account lockouts are another extremely common help desk ticket. In production environments the lockout threshold is typically set between 3–5 failed attempts to balance security with usability.

---

### Enforcing the Group Policy

After making changes, enforce the policy so it applies across the entire domain:

1. Go back to **Group Policy Management**
2. Right-click **Default Domain Policy** → **Enforce**
3. Click **OK** to confirm

---

## Part 3 — Verifying the Policy Was Applied

### Option 1 — Command Line (net user)

Open **CMD** on Windows Server or the Windows 11 VM and run:

```cmd
net user username /domain
```

Replace `username` with an actual AD user (e.g. `jsmith`). This displays the user's password and lockout settings, confirming the policy is applied.

### Option 2 — gpresult Command

Open **CMD as Administrator** and run:

```cmd
gpresult /r
```

This shows all Group Policies currently applied to the machine and user.

### Option 3 — Group Policy Management Console

1. Open **Group Policy Management**
2. Click **Default Domain Policy**
3. Select the **Settings** tab
4. Review all configured policy settings listed here

<img width="1282" height="918" alt="Screenshot 2026-03-10 at 5 50 02 PM" src="https://github.com/user-attachments/assets/41f35ce1-0398-4384-abfc-a6b970b9dcc8" />


---

## Summary

By the end of this section you should have:

- ✅ Reset a domain user password in Active Directory Users and Computers
- ✅ Disabled and enabled a user account (noted the downward arrow icon on disabled accounts)
- ✅ Configured Logon Hours to restrict when a user can log in
- ✅ Opened Group Policy Management and edited the Default Domain Policy
- ✅ Configured Password Policy (e.g. 30 day maximum password age)
- ✅ Set Account Lockout Threshold to 3 failed logon attempts
- ✅ Enforced the Default Domain Policy across the domain
- ✅ Verified policy changes using `net user`, `gpresult`, and the GPMC Settings tab

---
