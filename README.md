# Windows-File-and-Folder-Permissions-Lab

## Summary

In this lab, I configured NTFS permissions on a Finance folder in a Windows Server Active Directory environment. I granted the domain user AMTECH\kearl Modify permissions and tested access to the protected file.

I also examined how permissions affect authorized and unauthorized users, using PowerShell and Windows security settings to verify access. The goal was to understand how folder permissions control access to sensitive company files.
## Environment

Hypervisor: VMware Workstation

OS: Windows Server 2025 (ADMIN) &
OS: Windows 11 Pro (USER)

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

<img width="369" height="480" alt="1 companyfinance-advanced" src="https://github.com/user-attachments/assets/66b7b323-25eb-4a5b-9352-ef94546ad1ad" />


<img width="360" height="206" alt="image" src="https://github.com/user-attachments/assets/5c74ed68-d086-48b3-80a9-830043ccbd38" />


### 3. Modify kearl permissions

1. Still in the properties, go to the **Security**
2. You will be in "Permissions"
3. Click on the user kearl@AMTech.local
4. Click Edit
5. Check the following boxes 
  - Read and execute
  - List folders content
  - Modify
6. Click Apply
7. Click OK

<img width="366" height="247" alt="kearl-permissions" src="https://github.com/user-attachments/assets/8a417d00-196a-4d1c-8c86-34f79baaccb1" />


### 4. Verify Via PowerShell

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

## 5. Check on the GUI the Effective Access

- Go to CompanyData\Finance
- Right-Click **properties**
- Go to the **Security** tab
- Click on `kearl`
- **Advanced**
- **Effective Access**
- Select User
- Input `kearl`
- Check Names
- Ok
- **View effective access**

<img width="785" height="634" alt="7 effective user kearl" src="https://github.com/user-attachments/assets/90e77bce-3359-4e2d-ba8e-5ecc4233b9f3" />


<img width="764" height="517" alt="kearl-added-group-policy" src="https://github.com/user-attachments/assets/7a1ed3ac-0c85-4a61-abb5-aaf6438489b2" />

<img width="781" height="638" alt="6 Advanced-Security-Setting-Kearl" src="https://github.com/user-attachments/assets/23640cdb-4248-4392-9abd-52124f6ad7aa" />


## 6. Log into the user `kearl` and use the `whoami`command

Open PowerShell

```powershell
whoami
```

<img width="252" height="42" alt="whoami" src="https://github.com/user-attachments/assets/bc4bb93c-6483-442b-8798-acf0a3ac45e7" />


## 7. Runas command

To test access, I tried launching a session as `kearl` with `runas`:

```powershell
runas /user:AMTECH\kearl cmd
```

**The `runas` command allows you to run a specific program or command using the permissions of a different user account (usually an administrator) without having to log out of your current account.**


<img width="478" height="77" alt="runas-command" src="https://github.com/user-attachments/assets/55108359-4459-4cae-9b49-f645013ecda0" />

# 8. See the secret file exist in powershell and verify contents in the GUI 

```powershell
dir C:\CompanyData\Finance
```

<img width="558" height="249" alt="secret-file" src="https://github.com/user-attachments/assets/7de7704a-4c5e-4811-b16f-3c6cc5215040" />

<img width="788" height="591" alt="file-permission-secret-txt" src="https://github.com/user-attachments/assets/199d5b5f-60f9-482e-9ad7-d060705259e9" />


# 9. Log back to the administrator and see what happens if you look at the `CompanyData/Finance` folder

Access can be granted but they must bypass a Window.

<img width="348" height="180" alt="image" src="https://github.com/user-attachments/assets/ba3911ef-69d2-498e-b7eb-691757f16e6c" />


## Thoughts

The lab was successful. The domain user AMTECH\kearl can access the Finance document based on the NTFS permissions assigned to their account.

The administrator initially encountered restricted access to the folder but was able to proceed by clicking Continue. This demonstrates that NTFS permissions can restrict ordinary access, while an administrator with sufficient privileges can regain access.

This lab focused on basic file and folder permissions. In the next lab, I will explore Group Policy and how it can be used to manage and enforce security settings across domain computers.


## 💡 Summary of Skills Learned

* **NTFS Permissions:** How to give a user access to a folder (`kearl` got Modify on `C:\CompanyData\Finance`).
* **Inheritance:** Permissions on a parent folder pass down to the folders and files inside it. In `icacls`, this shows up as
 `(I)` inherited, `(OI)` applies to files and `(CI)` applies to subfolders.
* **Verifying Access:** How to check permissions with `icacls` and the Effective Access tab in the GUI.


