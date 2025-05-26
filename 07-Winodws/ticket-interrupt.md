Here is your fully updated and corrected Markdown version with all relevant steps, including the corrected GUI path using `Computer Management`, `lusrmgr.msc`, and a note on `wmic` (which is deprecated in modern Windows systems):

---

# 🧾 Ticket Summary

## 🔧 Request:

* Add a local user named **"Sly"** to a Windows machine
* Require the user to change password at first login
* Add the user to the **local Administrators group**

---

## 🖱️ Option 1: Using the GUI via Computer Management

### Step 1: Create the Local User

1. Press **Windows + X** and select **Computer Management**
   *(Or press **Start**, type `Computer Management`, and open it)*
2. Navigate to **System Tools > Local Users and Groups > Users**
3. Right-click **Users** and choose **New User…**
4. Enter the following:

   * **Username:** `Sly`
   * **Password:** Temporary password (e.g., `TempPass123!`)
   * ✔️ Check **"User must change password at next logon"**
   * ❌ Uncheck **"User cannot change password"** and **"Password never expires"**
5. Click **Create**, then **Close**

### Step 2: Add User to Local Administrators

1. Double-click the user **Sly** in the Users list
2. Go to the **Member Of** tab
3. Click **Add…**

   * Type `Administrators`
   * Click **Check Names**, then **OK**

---

## 🖥️ Option 2: Using `lusrmgr.msc`

### Step 1: Open Local Users and Groups

1. Press **Windows + R** to open the **Run** dialog
2. Type `lusrmgr.msc` and press **Enter**

### Step 2: Create and Configure User

Follow the same steps as in the Computer Management method:

* Right-click **Users > New User…**
* Fill in details
* Add to **Administrators** group via the **Member Of** tab

---

## 💻 Option 3: Using Command Line (CMD or PowerShell)

> ✅ *Run these commands in an elevated Command Prompt or PowerShell window (Run as Administrator)*

### Step 1: Create the User

```powershell
net user Sly TempPass123! /add
```

### Step 2: Force Password Change on First Login

> ⚠️ `wmic` is deprecated starting with Windows 10 version 21H1 and Windows 11. Use PowerShell or ADSI instead.

**Option A: (Deprecated - WMIC)**

```cmd
wmic useraccount where name='Sly' set PasswordExpires=True
```

**Option B: (Recommended - PowerShell/ADSI)**

```powershell
$user = [ADSI]"WinNT://./Sly,user"
$user.PasswordExpired = 1
$user.SetInfo()
```

### Step 3: Add User to Administrators Group

```powershell
net localgroup Administrators Sly /add
```

---

## 📝 Sample Ticket Update (Post-Completion)

> **Ticket #001 – Update (5/23/25 by Helpdesk)**

* Local user account **"Sly"** created
* Temporary password set
* User required to change password on first login
* User added to **Local Administrators group**
* Task completed via **\[GUI | Command Line]** method

---

## ⚙️ PowerShell Script – Create Local Admin User (Password Reset Required)

```powershell
# Define variables
$username = "Sly"
$password = "TempPass123!"  # Ensure this meets complexity requirements
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force

# Check if user already exists
if (Get-LocalUser -Name $username -ErrorAction SilentlyContinue) {
    Write-Output "User '$username' already exists."
} else {
    # Create the user
    New-LocalUser -Name $username -Password $securePassword -FullName "Sly" -Description "Local admin user"
    Write-Output "User '$username' created."

    # Force password change at next login
    $user = [ADSI]"WinNT://./$username,user"
    $user.PasswordExpired = 1
    $user.SetInfo()
    Write-Output "Password set to expire at next logon."

    # Add user to Administrators group
    Add-LocalGroupMember -Group "Administrators" -Member $username
    Write-Output "User '$username' added to the Administrators group."
}
```

---

## 📄 Ticket Update: Script Execution Log

> **Ticket #001 – Automated Script Execution Log (5/23/25)**

PowerShell script executed to fulfill request:

* ✅ Created local user account: **Sly**
* ✅ Temporary password set
* ✅ Password expiration enabled (user will change at first login)
* ✅ User added to **local Administrators group**

**Script validated and completed successfully.**

> 💡 *Compatible with Windows 10/11 and Windows Server 2016+. Logging and advanced error handling versions available upon request.*


