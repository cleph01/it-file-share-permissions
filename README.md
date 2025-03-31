<p align="center">
  <img src="https://tresorit.com/blog/content/images/size/w2000/2023/07/Secure-file-sharing_withoutlogo@2x.png" alt="Network File Share Diagram" width="300">
</p>

# Azure Network File Shares and Permissions Management

## Overview

This lab demonstrates the practical implementation of network file sharing and permission management within a Microsoft Azure environment. 

Simulates a small business network with Active Directory, a Windows Server (acting as a Domain Controller), and a Windows 10 client machine.

The core focus is on effectively sharing resources over the network and configuring NTFS and share permissions to control access for simulated users and groups managed by Active Directory.

This hands-on experience demonstrates the ability to:

- Create and manage network file shares with varying levels of access (Read, Write, Deny).
- Implement and troubleshoot user and group-based permissions using Active Directory.
- Gain practical experience with Windows Server and Windows 10 in a domain-joined environment.
- Develop a foundational understanding of security best practices related to file sharing.

---

## Prerequisites

This lab assumes the following environment is already set up in Azure:

- An Azure Virtual Network.
- An Active Directory Domain Controller (e.g., `DC-1`) running Windows Server.
- A domain-joined Windows 10 client machine (e.g., `Client-1`).
- At least one standard domain user account (e.g., `<someuser>`).

## Part 1: Creating Local Folders on the File Server

This section details the creation of local folders on the Windows Server that will be shared over the network.

1.  **Creating Folders using PowerShell:**
    * Opened PowerShell on the Windows Server (Domain Controller - `DC-1`) with administrator privileges.
    * Executed the following PowerShell commands (code obtained through research) to create four new directories in the `C:\` drive:
        ```powershell
        New-Item -Path "read-access", "write-access", "no-access", "accounting" -ItemType Directory
        ```

    <p align="center">
      <img src="https://github.com/user-attachments/assets/740e14b9-c001-49bd-b151-0e9be0ebdddd" alt="PowerShell Commands to Create Folders">
    </p>
    <p align="center">
      <em>Screenshot showing the PowerShell commands executed to create the four new directories.</em>
    </p>

2.  **Verification of Folder Creation:**
    * The successful creation of the four new directories (`read-access`, `write-access`, `no-access`, `accounting`) in the `C:\` directory was verified using File Explorer.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/8cc46eaf-645b-4f9f-842d-575bed38cda3" alt="Verification of Created Folders in File Explorer">
    </p>
    <p align="center">
      <em>Screenshot showing the four newly created directories in the C:\ drive of the Windows Server.</em>
    </p>

## Part 2: Configuring Share Permissions

This section outlines the configuration of share permissions for each of the created folders.

1.  **Setting Share Permissions for "read-access":**
    * Right-clicked on the "read-access" folder in File Explorer.
    * Selected "Properties" and navigated to the "Sharing" tab.
    * Clicked the "Share..." button.
    * In the "Add people to share with" window, typed "Domain Users", selected the group, and clicked "Add".
    * The default permission level for "Domain Users" is "Read". Clicked "Share" and then "Done".

    <p align="center">
      <img src="https://github.com/user-attachments/assets/c9c0429f-a6ae-4766-9469-469cc827948b" alt="Share Permissions for read-access - Domain Users Read">
    </p>
    <p align="center">
      <em>Screenshot showing the share permissions for the "read-access" folder, granting "Read" access to the "Domain Users" group.</em>
    </p>

2.  **Setting Share Permissions for "write-access":**
    * Right-clicked on the "write-access" folder, selected "Properties", and navigated to the "Sharing" tab.
    * Clicked "Share...".
    * Added the "Domain Users" group and changed the permission level for "Domain Users" from "Read" to "Read/Write" using the dropdown menu. Clicked "Share" and then "Done".

    <p align="center">
      <img src="https://github.com/user-attachments/assets/0a7ed96b-f5ba-442f-ace0-e2f4abe3217b" alt="Share Permissions for write-access - Domain Users Read/Write">
    </p>
    <p align="center">
      <em>Screenshot showing the share permissions for the "write-access" folder, granting "Read/Write" access to the "Domain Users" group.</em>
    </p>

3.  **Setting Share Permissions for "no-access":**
    * Right-clicked on the "no-access" folder, selected "Properties", and navigated to the "Sharing" tab.
    * Clicked "Share...".
    * Added the "Domain Admins" group and ensured their permission level was set to "Read/Write".
    * **Note:** By granting "Read/Write" permissions only to "Domain Admins" at the share level, regular "Domain Users" will effectively be denied access to this share. Clicked "Share" and then "Done".

    <p align="center">
      <img src="https://github.com/user-attachments/assets/8b3394d1-066d-4af3-803a-30baf402f6aa" alt="Share Permissions for no-access - Domain Admins Read/Write">
    </p>
    <p align="center">
      <em>Screenshot showing the share permissions for the "no-access" folder, granting "Read/Write" access only to the "Domain Admins" group.</em>
    </p>

## Part 3: Testing Share Access as a Standard Domain User

This section details the attempts to access the newly created file shares from a domain-joined Windows 10 client machine (`Client-1`) using a standard domain user account.

1.  **Navigating to Shared Folders:**
    * Logged into `Client-1` using a standard domain user account (`<someuser>`).
    * Opened File Explorer and in the address bar, typed `\\DC-1` (the UNC path to the file server) and pressed Enter to view the available network shares.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/7d83b0a9-8899-4d37-b873-2570f880f46a" alt="Network Shares on DC-1 as Standard User">
    </p>
    <p align="center">
      <em>Screenshot showing the visible network shares on `\\DC-1` when accessed by a standard domain user.</em>
    </p>

2.  **Accessing "read-access" Folder:**
    * Attempted to open the "read-access" folder.
    * **Result:** The folder opened successfully, indicating that the "Read" share permission for "Domain Users" is working.
    * Attempted to create a new text file within the "read-access" folder.
    * **Result:** The attempt failed, and an error message indicating insufficient permissions was displayed. This confirms that the "Read" share permission only allows viewing files and folders, not modification.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/9d016a0e-e437-46d9-98a9-51bbc51f9e4c" alt="Successfully Opened read-access Folder">
    </p>
    <p align="center">
      <em>Screenshot showing the "read-access" folder successfully opened by the standard domain user.</em>
    </p>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/d7304e74-3a29-4b3a-b018-7673fb93cde1" alt="Attempt to Create File in read-access - Permission Denied">
    </p>
    <p align="center">
      <em>Screenshot showing the error message encountered when attempting to create a new file within the "read-access" folder, indicating a lack of write permissions.</em>
    </p>

3.  **Accessing and Writing to "write-access" Folder:**
    * Attempted to open the "write-access" folder.
    * **Result:** The folder opened successfully.
    * Attempted to create a new text file within the "write-access" folder.
    * **Result:** The file was created successfully, and it could be opened and edited. This confirms that the "Read/Write" share permission for "Domain Users" is functioning as expected.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/811668f0-e764-4607-84ea-a397f74f4add" alt="Successfully Opened write-access Folder">
    </p>
    <p align="center">
      <em>Screenshot showing the "write-access" folder successfully opened by the standard domain user.</em>
    </p>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/7cb97343-e548-4f5f-ada2-2b024306c3c5" alt="Successfully Created File in write-access Folder">
    </p>
    <p align="center">
      <em>Screenshot showing a new text file successfully created within the "write-access" folder.</em>
    </p>
    <p align="center">
      <img src="https://github.com/user-attachments/assets/15719c5a-2ebe-4495-ab28-eca543ff4691" alt="Successfully Edited File in write-access Folder">
    </p>
    <p align="center">
      <em>Screenshot showing the standard domain user successfully editing the content of the newly created file in the "write-access" folder.</em>
    </p>

4.  **Accessing "no-access" Folder:**
    * Attempted to open the "\\\\DC-1\\no-access" share.
    * **Result:** An "Access Denied" error message was displayed. This confirms that because the standard "Domain Users" group does not have any share permissions on this folder (only "Domain Admins" do), access is correctly denied.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/b71ea1f2-a95a-4d14-8534-8a95b04e3f4e" alt="Access Denied to no-access Folder">
    </p>
    <p align="center">
      <em>Screenshot showing the "Access Denied" error message when the standard domain user attempted to access the "\\\\DC-1\\no-access" share.</em>
    </p>

## Part 4: Implementing Group-Based Permissions with Active Directory

This section demonstrates the creation of a security group in Active Directory and the assignment of permissions to a specific shared folder.

1.  **Creating the "ACCOUNTANTS" Security Group:**
    * Logged back into the Domain Controller (`DC-1`) with administrative credentials.
    * Opened "Active Directory Users and Computers".
    * Created a new Organizational Unit (OU) named "_GROUPS" for better organization of groups.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/a2ee5ed9-3ec7-46a8-8925-bdebd833dbb8" alt="Creating _GROUPS OU in Active Directory">
    </p>
    <p align="center">
      <em>Screenshot showing the creation of the "_GROUPS" Organizational Unit in Active Directory Users and Computers.</em>
    </p>

    * Within the "_GROUPS" OU, created a new Security Group named "ACCOUNTANTS" with a Global scope.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/45f5ec58-8225-4494-ad01-2ca9c52ca906" alt="Creating ACCOUNTANTS Security Group - Step 1">
    </p>
    <p align="center">
      <em>Screenshot showing the initial steps of creating the "ACCOUNTANTS" security group.</em>
    </p>

    <p align="center">
      <img src="https://github.com/user-attachments/assets/851d699b-09cc-4a07-b3f6-09568c9cdadc" alt="Creating ACCOUNTANTS Security Group - Step 2">
    </p>
    <p align="center">
      <em>Screenshot showing the properties of the newly created "ACCOUNTANTS" security group.</em>
    </p>

2.  **Setting Share Permissions for "accounting" Folder:**
    * Right-clicked on the "accounting" folder on `DC-1`, selected "Properties", and navigated to the "Sharing" tab.
    * Clicked "Share...".
    * Removed the "Domain Users" group (if it was present).
    * Added the "ACCOUNTANTS" security group and granted them "Read/Write" permission. Clicked "Share" and then "Done".

    <p align="center">
      <img src="https://github.com/user-attachments/assets/16b2e645-2840-4d0c-aaa1-d5490bcbe972" alt="Share Permissions for accounting - ACCOUNTANTS Read/Write">
    </p>
    <p align="center">
      <em>Screenshot showing the share permissions for the "accounting" folder, granting "Read/Write" access to the "ACCOUNTANTS" security group.</em>
    </p>

3.  **Testing Access Before Group Membership:**
    * Logged back into `Client-1` as the standard user (`<someuser>`).
    * Attempted to access the "\\\\DC-1\\accounting" share.
    * **Result:** Access was denied because the user `<someuser>` is not yet a member of the "ACCOUNTANTS" group.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/f950234f-6e51-4901-b971-e7788f060dda" alt="Access Denied to accounting Folder Before Group Membership">
    </p>
    <p align="center">
      <em>Screenshot showing the "Access Denied" error message when the standard domain user attempted to access the "\\\\DC-1\\accounting" share before being added to the "ACCOUNTANTS" group.</em>
    </p>

4.  **Adding User to the "ACCOUNTANTS" Security Group:**
    * Logged back into the Domain Controller (`DC-1`) with administrative credentials.
    * Opened "Active Directory Users and Computers".
    * Located the user account `<someuser>` and added them as a member of the "ACCOUNTANTS" security group.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/fb957d24-31e7-4fee-a8ba-3b6362fda285" alt="Adding User to ACCOUNTANTS Security Group">
    </p>
    <p align="center">
      <em>Screenshot showing the process of adding the standard domain user to the "ACCOUNTANTS" security group in Active Directory.</em>
    </p>

5.  **Testing Access After Group Membership:**
    * Logged out of `Client-1` and logged back in as the standard user (`<someuser>`).
    * Attempted to access the "\\\\DC-1\\accounting" share again.
    * **Result:** The user now had "Read/Write" access to the "accounting" folder. The user could open the folder, create new files, and edit existing ones. This demonstrates the successful implementation of group-based share permissions using Active Directory.

    <p align="center">
      <img src="https://github.com/user-attachments/assets/89ed2a56-b7f5-4cf7-b00e-11159d7b5c47" alt="Successfully Accessed accounting Folder After Group Membership">
    </p>
    <p align="center">
      <em>Screenshot showing the "accounting folder successfully opened by the standard domain user after being added to the "ACCOUNTANTS" security group.</em>
    </p>

    <p align="center">
      <img src="https://github.com/user-attachments/assets/47cd1245-d3eb-4ba8-86fb-048ba7dfa2e2" alt="Created File in accounting Folder After Group Membership">
    </p>
    <p align="center">
      <em>Screenshot showing a new file successfully created within the "accounting" folder by the standard domain user.</em>
    </p>

    <p align="center">
      <img src="https://github.com/user-attachments/assets/005eb77e-f68c-4240-8791-e441d1f6107b" alt="Edited File in accounting Folder After Group Membership">
    </p>
    <p align="center">
      <em>Screenshot showing the standard domain user successfully editing the content of a file within the "accounting" folder.</em>
    </p>

---

## Conclusion

This lab successfully demonstrated the creation and management of network file shares with varying permission levels in an Azure-based Active Directory environment. By configuring share permissions for individual folders and leveraging Active Directory security groups, we effectively controlled access for a standard domain user. The results of the access tests aligned with the configured permissions, showcasing the fundamental principles of network file sharing and permission management. This exercise provides valuable hands-on experience relevant to IT support and help desk roles, highlighting the ability to implement and troubleshoot file access in a domain environment.

<br />
