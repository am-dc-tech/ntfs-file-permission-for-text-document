# Windows-File-and-Folder-Permissions-Lab

## Summary

In this lab I practiced creating company folders, adding permissions and testing who has access to these folders


## Environment

Hypervisor: VMware Workstation

OS: Windows Server 2025

Server: AM-DC-01 (Domain Controller)

Domain: AM-Tech.local


## Steps

### 1. Create The Company Folders

#### Open Powershell as Administrator and run:

```powershell
New-Item -Path "C:\CompanyData" -ItemType Directory
```

#### Create a test file 

```powershell
New-Item -Path "C:\CompanyData\Finance\employee-info.txt" -ItemType File
```

### 2. Add the User "kearl"

#### How to Update Folder Permissions

1. Find the Folder **"CompanyData/Finance"**
2. Right-click **Properties**
3. Go to the **Security** tab
4. Click **Edit**
5. Click **Add**
6. Enter **"kearl"**
7. Click **OK**

<img width="360" height="206" alt="image" src="https://github.com/user-attachments/assets/5c74ed68-d086-48b3-80a9-830043ccbd38" />


### 3. Modify kearl permissions

1. Still in the properties, go to "edit"
2. You will be in "Permissions"
3. Click on the user kearl@AMTech.local
4. Click Edit
5. Check the following boxes 
  - Read and execute
  - List folders content
  - Modify
6. Click Apply
7. Click Ok

<img width="781" height="638" alt="kearlpermissions" src="https://github.com/user-attachments/assets/eef32c79-ae29-42fc-9ebd-b7cb51288d9d" />


### 4 Verify Via PowerShell

  1. Open PowerShell as Administrator
  2. Run the following command to check the Access Control List (ACL) for the folder

```powershell
icacls "C:\CompanyData\Finance"
```
  3. Verify that kearl appears in the list with the Modify (M) or the respective permission checks


<img width="868" height="564" alt="acl" src="https://github.com/user-attachments/assets/2047b868-2644-4105-a69b-93f5a4bc62ee" />


## Breakdown on the Permission Flags in Powershell

 * `AMTECH\kearl`: Assigned Domain User
 * `(I)`: **Inherited** from parent directory
 * `(OI)`: **Object Inherit** (applies to child files)
 * `(CI)`: **Container Inherit** (applies to child subfolders)
 * `(M)`: **Modify** Permission (Full Read/Write/Delete access)
   

<img width="786" height="639" alt="properties-security-advanced" src="https://github.com/user-attachments/assets/3865ea14-2766-41b8-9383-25e2f0799ffa" />

<img width="783" height="632" alt="effectiveuser-select user" src="https://github.com/user-attachments/assets/4ee6e66b-2191-4efa-a9b9-cbdb78611731" />

<img width="785" height="634" alt="effective user kearl" src="https://github.com/user-attachments/assets/f49677e2-de43-4414-9107-60b0bc5f0937" />

<img width="781" height="638" alt="kearlpermissions" src="https://github.com/user-attachments/assets/a7057e6a-bbe8-4200-8f9f-acf628b7ec01" />

## Validating The User Permissions

To test access, I tried launching a session as `kearl` with `runas`:

```powershell
runas /user:AMTECH\kearl cmd
```

This failed with **Error 1385: "Logon failure: the user has not been granted the requested logon type at this computer."**

`runas` starts an interactive logon on the machine where it's run. I ran it on **AM-DC-01, a domain controller**. By default, the Default Domain Controllers Policy only allows administrative groups (such as Administrators, Account Operators, and Backup Operators) to log on locally to a DC. `kearl` is a standard domain user, so Windows denied the logon, even though `kearl` has valid credentials.

This is a logon restriction, not a file permission problem. `kearl` still holds NTFS Modify permissions on `C:\CompanyData\Finance`, as verified by `icacls` and the Effective Access tab.

<img width="839" height="142" alt="Screenshot 2026-10-22 122051" src="https://github.com/user-attachments/assets/09f14be1-6eec-4b27-b196-7ea7b550d5a2" />

## Thoughts

The permissions were set up correctly, but I couldn't test a real login as `kearl` on the domain controller. Next time I'll join a Windows client to the domain and log in as `kearl` to confirm. 

## 💡 Summary of Skills Learned

* **NTFS Permissions:** How to give a user access to a folder (`kearl` got Modify on `C:\CompanyData`).
* **Inheritance:** Permissions on a parent folder pass down to the folders and files inside it. In `icacls`, this shows up as
 `(I)` inherited, `(OI)` applies to files and `(CI)` applies to subfolders.
* **Verifying Access:** How to check permissions with `icacls` and the Effective Access tab.
* **Logon Rights:** Regular domain users can't log in on a domain controller by default (Error 1385). This is separate from file permissions.
* **Logon Rights:** Learned that regular domain users can't log in on a domain controller by default (Error 1385), which is separate from file permissions.


