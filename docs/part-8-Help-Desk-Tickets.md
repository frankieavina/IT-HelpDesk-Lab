# Part 8 - Jira Ticketing System: Setup and Help Desk Tickets

## Overview
In this section, we set up a Jira project to simulate a real-world IT help desk ticketing system. We create 5 realistic help desk tickets, work through each one in our Active Directory lab environment, and resolve and document them in Jira — just like a real IT help desk professional would.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM (Ceres-DC-01) | Domain Controller / File Server | 10.0.0.10 |
| Windows 11 VM | Domain-joined Workstation | 10.0.0.x |
| Domain | example.com | — |
| Ticketing System | Jira (Atlassian) | Cloud-based |

---

## Part 1 — Setting Up Jira

### Step 1 — Create a Free Jira Account

1. Go to **[atlassian.com](https://www.atlassian.com)** and click **"Get it free"**
2. Select **Jira Software**
3. Sign up with your email or Google account
4. Choose a site name (e.g. `yourname.atlassian.net`)
5. Click **"Agree and Sign Up"**

### Step 2 — Create a New Project

1. Click **"Create project"**
2. Select **Kanban** — best fit for a help desk ticketing workflow
3. Select **"Team-managed project"**
4. Name the project **IT HelpDesk Lab**
5. Click **"Create project"**

### Step 3 — Set Up the Board

1. Click the **Board** tab at the top of your project
2. Set up the following columns:

| Column | Purpose |
|---|---|
| To Do | Ticket submitted, not yet started |
| In Progress | Actively being worked on |
| Pending User | Waiting on response from the user |
| Done | Issue resolved and confirmed |

---

## Part 2 — Help Desk Tickets

Below are the 5 tickets created and resolved in this lab. Each ticket follows the same workflow:
1. Create the ticket in Jira and set status to **In Progress**
2. Resolve the issue in the Active Directory lab environment
3. Add a resolution comment in Jira and mark as **Done**

<img width="1238" height="689" alt="Screenshot 2026-03-17 at 6 26 43 PM" src="https://github.com/user-attachments/assets/67a1f2e5-5631-48d5-9944-5efadbc47ccd" />


---

### HD-001 — User Account Locked Out

| Field | Value |
|---|---|
| Priority | High |
| Reporter | mjones (HR) |
| Assignee | ajohnson (IT) |

**Issue:** User Maria Jones could not log into her workstation after exceeding the account lockout threshold of 3 failed login attempts set in Group Policy.

**Resolution Steps:**
1. Opened Active Directory Users and Computers on Ceres-DC-01
2. Located mjones in the HR OU
3. Right-clicked → Properties → Account tab
4. Unchecked "Account is locked out" → Apply
5. Reset password and enabled "User must change password at next logon"
6. Verified user could log in successfully on Windows 11

**Jira Comment:**
> Unlocked account for mjones via Active Directory Users and Computers. Reset password and enabled "must change at next logon". User confirmed access restored.

**Status:** Done ✅

---

### HD-002 — New Employee Needs Access to Finance Shared Folder

| Field | Value |
|---|---|
| Priority | High |
| Reporter | jwilson (Management) |
| Assignee | bcarter (IT) |

**Issue:** New Finance employee Rachel Turner (rturner) could not access the Finance shared folder at \\Ceres-DC-01\Finance. She had not yet been added to the GRP-Finance security group.

**Resolution Steps:**
1. Opened Active Directory Users and Computers on Ceres-DC-01
2. Located GRP-Finance in the Finance OU
3. Right-clicked → Properties → Members tab → Added rturner
4. Had rturner log out and back in for group policy to apply
5. Verified rturner could access \\Ceres-DC-01\Finance
6. Mapped the Finance share as network drive Z: with "Reconnect at sign-in" enabled

**Jira Comment:**
> Added rturner to GRP-Finance security group in Active Directory. User confirmed access to \\Ceres-DC-01\Finance. Network drive mapped to Z: with reconnect at sign-in enabled.

**Status:** Done ✅

---

### HD-003 — User Forgot Password and Cannot Log In

| Field | Value |
|---|---|
| Priority | Medium |
| Reporter | slee (HR) |
| Assignee | ajohnson (IT) |

**Issue:** Sandra Lee (slee) forgot her domain password after returning from a 2 week vacation and was unable to log into her workstation or access any company resources.

**Resolution Steps:**
1. Verified user identity before making any changes
2. Opened Active Directory Users and Computers on Ceres-DC-01
3. Located slee in the HR OU
4. Right-clicked → Reset Password
5. Set a temporary password and checked "User must change password at next logon"
6. Communicated temporary password to user securely
7. Verified user logged in successfully and was prompted to set a new password

**Jira Comment:**
> Reset password for slee via Active Directory Users and Computers. Temporary password issued and "User must change password at next logon" enabled. User confirmed successful login and set new password.

**Status:** Done ✅

---

### HD-004 — Disable Account for Departing Employee

| Field | Value |
|---|---|
| Priority | Medium |
| Reporter | jwilson (Management) |
| Assignee | bcarter (IT) |

**Issue:** Employee David Kim (dkim) from the Finance department was leaving the company. Per company policy his account needed to be disabled immediately and all access to shared resources revoked.

**Resolution Steps:**
1. Opened Active Directory Users and Computers on Ceres-DC-01
2. Located dkim in the Finance OU
3. Right-clicked → Disable Account
4. Confirmed the downward arrow icon appeared on the account
5. Opened GRP-Finance → Properties → Members tab → Removed dkim
6. Verified dkim could no longer log in (received account disabled error)
7. Documented date and time of account disable for audit purposes
8. Notified manager jwilson that the account had been disabled

**Jira Comment:**
> Disabled account for dkim via Active Directory Users and Computers. Confirmed downward arrow icon on account. Removed dkim from GRP-Finance security group. Verified account cannot log in. Manager jwilson notified.

**Status:** Done ✅

---

### HD-005 — Network Drive Not Reconnecting After Reboot

| Field | Value |
|---|---|
| Priority | Low |
| Reporter | bcarter (IT) |
| Assignee | ajohnson (IT) |

**Issue:** Brian Carter (bcarter) reported that his mapped network drive to \\Ceres-DC-01\IT-HelpDesk was disappearing every time he restarted his workstation. The drive had been mapped without the "Reconnect at sign-in" option enabled.

**Resolution Steps:**
1. Logged into Windows 11 as bcarter
2. Disconnected the existing broken mapped drive via File Explorer
3. Right-clicked This PC → Map Network Drive
4. Entered path \\Ceres-DC-01\IT-HelpDesk
5. Checked "Reconnect at sign-in" and "Connect using different credentials"
6. Entered domain credentials (example\bcarter) when prompted
7. Restarted Windows 11 and verified the drive persisted after reboot

**Jira Comment:**
> Disconnected broken mapped drive and remapped \\Ceres-DC-01\IT-HelpDesk with "Reconnect at sign-in" and domain credentials enabled. Verified drive persists after reboot. Issue resolved.

**Status:** Done ✅

---

## Ticket Summary

| Ticket | Issue | Priority | Skill |
|---|---|---|---|
| HD-001 | Account lockout | High | Unlock AD account |
| HD-002 | New employee access request | High | Security group provisioning |
| HD-003 | Forgot password | Medium | Password reset with forced change |
| HD-004 | Employee offboarding | Medium | Disable account and revoke access |
| HD-005 | Network drive not persisting | Low | Map network drive troubleshooting |

---

## Key Takeaways

- A ticketing system like Jira is used in virtually every IT environment to track, prioritize, and document issues
- Every ticket should include a clear description of the issue, the steps taken to resolve it, and a confirmation that the user verified the fix
- Documentation in tickets is critical for audits, knowledge sharing, and identifying recurring issues
- The most common help desk tickets involve account lockouts, password resets, access requests, and offboarding — all of which were covered in this section

---
