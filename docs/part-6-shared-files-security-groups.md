# Part 6 - Shared Files, Permissions, Security Groups, and Testing

## Overview
In this section, we create and configure shared folders on Windows Server 2022, manage permissions manually and through Security Groups, and verify access from the Windows 11 workstation. We also cover how to map a network drive — a common interview topic for IT help desk roles.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM (Ceres-DC-01) | Domain Controller / File Server | 10.0.0.10 |
| Windows 11 VM | Domain-joined Workstation | 10.0.0.x |
| Domain | example.com | — |

---

## Part 1 — Creating a Shared Folder

### Step 1 — Open File and Storage Services

1. On **Windows Server 2022**, open **Server Manager**
2. Click **File and Storage Services** in the left panel
3. Click **Shares**
4. Right-click the shares window → **New Share**

### Step 2 — Configure the New Share

1. Select **SMB Share - Quick** → click **Next**
2. Confirm the correct drive is selected → click **Next**
3. Enter a **Share Name** → click **Next**
4. Leave **Allow caching of share** selected → click **Next**
5. Click **Customize Permissions** before finishing

### Step 3 — Customize Permissions

1. In the permissions window, click **Disable Inheritance**
2. Select **"Convert inherited permissions into explicit permissions on this object"**
3. Remove all users that are **not** the following:
   - `Administrator`
   - `SYSTEM`
   - `CREATOR OWNER`
4. Click **Apply** → **OK**
5. Click **Next** → **Create**

---

## Part 2 — Setting Up the Tech Folder

### Step 1 — Create the Tech Folder

1. Navigate to the share location:
```
This PC > Local Disk (C:) > Shares > [Your Share Name]
```
2. Create a new folder inside named **`Tech`**

### Step 2 — Enable Advanced Sharing on the Tech Folder

1. Right-click the **Tech** folder → **Properties**
2. Go to the **Sharing** tab → click **Advanced Sharing**
3. Check **"Share this Folder"** → click **Permissions**
4. By default **Everyone** is listed — click **Remove**
5. Click **Apply** → **OK**

### Step 3 — Add a Specific User via Security Tab

1. Still in the **Tech** folder properties, go to the **Security** tab
2. Click **Edit** → **Add**
3. In the box labeled **"Enter the object names to select"**, type the username of the person you want to give access to
4. Click **Check Names** → **OK**
5. Click **Apply** → **OK**

### Step 4 — Set Share Permissions for the User

1. Go back to the **Sharing** tab → click **Share**
2. Find the user you just added
3. Change their **Permission Level** to **Read/Write**
4. Click **Share** to confirm

---

## Part 3 — Testing the Share from Windows 11

### Step 1 — Access the Share

1. On **Windows 11**, open **File Explorer**
2. In the address bar type the server path:
```
\\Ceres-DC-01
```
3. You should see the shared folder you created
4. Navigate into it and confirm you only have access to the **Tech** folder

### Step 2 — Add as Quick Access (Optional)

1. Right-click the **Tech** folder in File Explorer
2. Select **"Pin to Quick Access"** for easy access going forward

---

## Part 4 — Mapping a Network Drive

> 💡 **Interview Tip:** Knowing how to map a network drive is a very common IT help desk interview question. Make sure you know this step by step.

1. Open **File Explorer** → right-click **This PC**
2. Select **Map Network Drive**
3. Choose a **Drive Letter** (e.g. `Z:`)
4. Enter the folder path:
```
\\Ceres-DC-01\Tech
```
5. Check **"Reconnect at sign-in"** if you want it to persist after reboot
6. Click **Finish**

The **Tech** folder will now appear as a mapped drive under **This PC** in File Explorer.

---

## Part 5 — Using Security Groups for Folder Access

Security Groups allow you to manage folder access at a group level rather than adding and removing individual users — much more efficient in a real enterprise environment.

### Step 1 — Create a Security Group in Active Directory

1. On **Windows Server 2022**, open **Active Directory Users and Computers**
2. Right-click **Users** → **New** → **Group**
3. Fill in the details:

| Field | Example Value |
|---|---|
| Group Name | Tech |
| Group Scope | Global |
| Group Type | Security |

4. Click **OK**

### Step 2 — Add Members to the Security Group

1. In **Active Directory Users and Computers**, find the **Tech** security group
2. Right-click → **Properties** → go to the **Members** tab
3. Click **Add** → enter the usernames you want to add to the group
4. Click **OK** → **Apply**

### Step 3 — Document the Group's Purpose

1. Still in the **Tech** group properties, go to the **General** tab
2. In the **Description** field, add the network path the group controls:
```
Folder access to \\Ceres-DC-01\Tech
```
3. Click **Apply** → **OK**
   
<img width="695" height="480" alt="Screenshot 2026-03-17 at 2 24 20 PM" src="https://github.com/user-attachments/assets/a4aff00a-baec-46ea-a116-56c8dabe5cd8" />

> 💡 **Note:** Adding a description is good practice in a real environment so any IT admin can quickly understand what the group is used for without having to investigate.

### Step 4 — Remove the Manually Added User

Now that we have a Security Group, we can remove the individual user we added earlier and replace them with the group:

1. Right-click the **Tech** folder → **Properties** → **Sharing** tab
2. Click **Share**
3. Find the manually added user → click **Remove**
4. Click **Share** to confirm

### Step 5 — Grant the Security Group Access to the Folder

1. Right-click the **Tech** folder → **Properties**
2. Click **Share**
3. In the search box type the name of your Security Group (e.g. `Tech`)
4. Set the **Permission Level** to **Read/Write**
5. Click **Share** to confirm

<img width="831" height="528" alt="Screenshot 2026-03-17 at 2 26 41 PM" src="https://github.com/user-attachments/assets/ae55bb41-2fe3-4098-9bb0-ef3740e432f7" />


### Step 6 — Verify via Security Tab

1. Right-click the **Tech** folder → **Properties** → **Security** tab
2. Click **Edit** and confirm the **Tech** security group is listed with the correct permissions

### Step 7 — Advanced Permissions (Optional)

For more granular control over folder permissions:

1. Right-click the **Tech** folder → **Properties**
2. Go to **Security** tab → click **Advanced**
3. Click **Permissions**
4. From here you can **Add**, **Remove**, or **Edit** specific permissions for any user or group

---

## Summary

By the end of this section you should have:

- ✅ Created a shared folder using File and Storage Services in Server Manager
- ✅ Customized permissions by disabling inheritance and removing unnecessary users
- ✅ Created a **Tech** subfolder and configured advanced sharing
- ✅ Manually added a user to the share via the Security tab
- ✅ Tested share access from the Windows 11 VM using `\\Ceres-DC-01`
- ✅ Mapped the Tech folder as a network drive using a drive letter
- ✅ Created a **Tech** Security Group in Active Directory
- ✅ Added members to the Security Group and documented its purpose
- ✅ Replaced manual user access with Security Group access
- ✅ Verified permissions via the Security tab and Advanced Permissions

---

## Key Takeaway

There are two ways to manage shared folder access:

| Method | Best Used When |
|---|---|
| **Manually adding users** | Quick one-off access for a single person |
| **Security Groups** | Managing access for teams or departments at scale |

In a real enterprise environment, **Security Groups are always preferred** because they are easier to manage, audit, and scale as the organization grows.

---
