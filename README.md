# Windows-File-and-Folder-Permissions-Lab

## Summary

In this lab a secret file will only be accessible for a user and not even the administrator using file permissions

## Environment

Hypervisor: VMware Workstation

OS: Windows Server 2025 (ADMIN) &
OS: Windows 11 Home (USER)

Server: AM-DC-01 (Domain Controller)

Administrator: AMTECH\Administrator

User: AMTECH\kearl


## Steps

### 1. Create The Company Folders with Administrator

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
   

<img width="360" height="482" alt="Kearl-Added-Properties" src="https://github.com/user-attachments/assets/416c5ea6-8e37-4acf-b5f8-3e98a965f80c" />

<img width="764" height="517" alt="kearl-added-group-policy" src="https://github.com/user-attachments/assets/7a1ed3ac-0c85-4a61-abb5-aaf6438489b2" />


## 5 Log into the user `kearl` and use the `whoami` 

```powershell
whoami
```

<img width="252" height="42" alt="whoami" src="https://github.com/user-attachments/assets/bc4bb93c-6483-442b-8798-acf0a3ac45e7" />


## 6 Runas command

To test access, I tried launching a session as `kearl` with `runas`:

```powershell
runas /user:AMTECH\kearl cmd
```

<img width="478" height="77" alt="runas-command" src="https://github.com/user-attachments/assets/55108359-4459-4cae-9b49-f645013ecda0" />

# 7 See the secret file exist in powershell and verify contents in the GUI 

```powershell
dir C:\CompanyData/Finance

```

<img width="558" height="249" alt="secret-file" src="https://github.com/user-attachments/assets/7de7704a-4c5e-4811-b16f-3c6cc5215040" />

<img width="788" height="591" alt="file-permission-secret-txt" src="https://github.com/user-attachments/assets/199d5b5f-60f9-482e-9ad7-d060705259e9" />


# 8 Log back to the administrator and see what happens if you look at the `CompanyData/Finance` folder

<img width="348" height="180" alt="image" src="https://github.com/user-attachments/assets/ba3911ef-69d2-498e-b7eb-691757f16e6c" />


## Thoughts

The lab was a success, the user kearl can access the document with the user file permissions and group policy.

## 💡 Summary of Skills Learned

* **NTFS Permissions:** How to give a user access to a folder (`kearl` got Modify on `C:\CompanyData`).
* **Inheritance:** Permissions on a parent folder pass down to the folders and files inside it. In `icacls`, this shows up as
 `(I)` inherited, `(OI)` applies to files and `(CI)` applies to subfolders.
* **Verifying Access:** How to check permissions with `icacls` and the Effective Access tab.


