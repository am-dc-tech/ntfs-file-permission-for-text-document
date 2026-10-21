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
New-Item -Path "C:\CompanyData\Finance" -ItemType Directory
```

#### Create a test file 

```powershell
New-Item -Path "C:\CompanyData\Finance\employee-info.txt" -ItemType File
```

### 2. Create the User "kearl"

#### 1. Find the Folder "CompanyData"

#### 2. Right-Click the HR folder -> Properties

#### 3. Go to the Security tab

#### 4. Click Edit 

#### 5. Click Add

#### 6. Enter "kearl"

#### 7. Click Ok

### 3. Modify kearl permissions

#### Still in the properties, go to "edit"

#### You will be Permissions

#### Click on the user kearl@AMTech.local

#### Click Edit

#### Check 
  - Read and execute
  - List folders content
  - Read
  - Write
  - Modify

#### Click Apply

#### Click Ok




    





