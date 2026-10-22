# Windows-File-and-Folder-Permissions-Lab

## Summary

This lab will be practicing creating company folders, adding permissions and testing who has access to these folders


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

### 2. Create the User "kearl"

#### How to Update Folder Permissions

1. Find the Folder **"CompanyData"**
2. Right-click the **HR** folder -> **Properties**
3. Go to the **Security** tab
4. Click **Edit**
5. Click **Add**
6. Enter **"kearl"**
7. Click **OK**


### 3. Modify kearl permissions

1. Still in the properties, go to "edit"
2. You will be in "Permissions"
3. Click on the user kearl@AMTech.local
4. Click Edit
5. Check the following boxes 
  - Read and execute
  - List folders content
  - Read
  - Write
  - Modify
 7. Click Apply
8. Click Ok

### 4 Verify Via PowerShell

  1. Open PowerShell as Administrator
  2. Run the following command to check the Access Control List (ACL) for the folder

     ```powershell
  icals "C:\CompanyData\Finance"
     ```


    





