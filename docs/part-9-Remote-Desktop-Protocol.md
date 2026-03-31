# Part 9A - Remote Desktop Protocol (RDP)

## Overview
In this section, we configure and use Remote Desktop Protocol (RDP) to remotely connect from Windows Server 2022 (Ceres-DC-01) to the Windows 11 workstation (frankie-vm1). RDP is one of the most commonly used tools in IT support, allowing help desk technicians to remotely access and troubleshoot user machines without physically being present.

---

## Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server 2022 VM (Ceres-DC-01) | Domain Controller / RDP Client | 10.0.0.10 |
| Windows 11 VM (frankie-vm1) | Domain-joined Workstation / RDP Host | 10.0.0.x |
| Domain | example.com | — |

---

## What is RDP?
Remote Desktop Protocol (RDP) is a Microsoft protocol that allows a user to connect to and control another computer over a network. In a real enterprise environment, IT help desk technicians use RDP to:

- Remotely troubleshoot user issues without leaving their desk
- Install or configure software on remote machines
- Access servers for administration and maintenance
- Support remote or hybrid workers

---

## Part 1 — Enable RDP on Windows 11

### Step 1 — Check if RDP is Currently Enabled

On **Windows 11 (frankie-vm1)** open **PowerShell as Administrator** and run:

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections"
```

- `fDenyTSConnections : 1` → RDP is **disabled**
- `fDenyTSConnections : 0` → RDP is **enabled** ✅

### Step 2 — Enable RDP via PowerShell

```powershell
# Enable RDP
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" -Value 0

# Allow RDP through Windows Firewall
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

# Verify RDP is now enabled
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections"
```

Expected result: `fDenyTSConnections : 0` ✅

### Step 3 — Enable RDP via GUI (Alternative Method)

1. Right-click **Start** → **System**
2. Click **"Remote Desktop"** on the left panel
3. Toggle **"Enable Remote Desktop"** to **On**
4. Click **Confirm**
5. Click **"Select users that can remotely access this PC"**
6. Click **Add** → type the domain users you want to allow → **OK**

### Step 4 — Allow Domain Users to Use RDP

By default only Administrators can RDP into a machine. To allow domain users:

```powershell
Add-LocalGroupMember -Group "Remote Desktop Users" -Member "example\ajohnson"
Add-LocalGroupMember -Group "Remote Desktop Users" -Member "example\bcarter"
```

---

## Part 2 — Connect via RDP from Ceres-DC-01

### Step 1 — Open Remote Desktop Connection

On **Ceres-DC-01** use one of these methods:

- Press **Windows Key + R** → type `mstsc` → press Enter
- Or search **"Remote Desktop Connection"** in the Start menu

### Step 2 — Enter Connection Details

In the Remote Desktop Connection window:

| Field | Value |
|---|---|
| Computer | 10.0.0.x or frankie-vm1 |

Click **Connect**

### Step 3 — Enter Credentials

When prompted enter your domain credentials:

```
Username: example\Administrator
Password: <your domain admin password>
```

Click **Yes** on the certificate warning to proceed.
<img width="765" height="683" alt="Screenshot 2026-03-30 at 7 32 50 PM" src="https://github.com/user-attachments/assets/5e2edb9c-d4a5-4d61-bc05-f2bdb89281f9" />

### Step 4 — Successful Connection

You should now see the **Windows 11 desktop** displayed inside a window on your Server — you are now remotely controlling frankie-vm1 from Ceres-DC-01. ✅

```
Ceres-DC-01 (your screen)
└── RDP Window
      └── Windows 11 Desktop (frankie-vm1)
            └── Full remote control of this machine
```
<img width="1267" height="792" alt="Screenshot 2026-03-30 at 7 41 40 PM" src="https://github.com/user-attachments/assets/c2e3273b-90f7-4cd9-ba28-521ce401031c" />

---

## Part 3 — What You Can Do Over RDP

Once connected via RDP you have full control of the remote machine. Common help desk tasks performed over RDP include:

| Task | How |
|---|---|
| Troubleshoot software issues | Open and interact with applications directly |
| Check Event Viewer logs | Start → Event Viewer → review error logs |
| Install software | Run installers remotely just like sitting at the machine |
| Fix network settings | Open Network Settings and make changes |
| Reset local passwords | Open Computer Management → Local Users |
| Map network drives | Right-click This PC → Map Network Drive |
| Check system performance | Open Task Manager → review CPU/RAM usage |

---

## Troubleshooting Common RDP Issues

| Issue | Likely Cause | Fix |
|---|---|---|
| Connection refused | RDP not enabled on target machine | Enable RDP via PowerShell or System Settings |
| Credentials rejected | Wrong username format | Use `example\username` not just `username` |
| Blank/black screen | Display driver issue | Wait 30 seconds or disconnect and reconnect |
| Very slow performance | Not enough RAM/CPU allocated to VMs | Increase VM RAM in Unraid VM settings |
| Firewall blocking | Windows Firewall blocking RDP port 3389 | Run `Enable-NetFirewallRule -DisplayGroup "Remote Desktop"` |

> 💡 **Note:** RDP uses **port 3389** by default. In a real enterprise environment this port is often restricted and only accessible via VPN for security reasons.

---

## Summary

By the end of this section you should have:

- ✅ Checked and enabled RDP on Windows 11 (frankie-vm1)
- ✅ Allowed domain users access to RDP via the Remote Desktop Users group
- ✅ Opened Remote Desktop Connection on Ceres-DC-01 using `mstsc`
- ✅ Successfully connected to frankie-vm1 remotely from the Server
- ✅ Understood common RDP use cases and troubleshooting steps

---

## Key Takeaway

> In most enterprise environments, IT help desk technicians **never physically walk to a user's desk** to fix an issue. RDP allows you to remote into any machine on the network, troubleshoot the problem, and close the ticket — all from your own workstation. Mastering RDP is one of the most practical and immediately useful skills for any IT help desk role.

---

## What's Next — Part 9B

In the next part we will cover:
- DNS troubleshooting using `nslookup` and `ipconfig /flushdns`
- Setting up DHCP on Windows Server 2022 for automatic IP assignment
