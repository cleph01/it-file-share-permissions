<p align="center">
  <img src="https://tresorit.com/blog/content/images/size/w2000/2023/07/Secure-file-sharing_withoutlogo@2x.png" alt="Network File Share Diagram" width="300">
</p>

# Azure Network File Shares & Permissions Lab

This project showcases the practical implementation of network file sharing and robust permission management within a realistic Microsoft Azure environment. 

Designed to demonstrate a strong understanding of fundamental network administration principles crucial for a Helpdesk role, this lab simulates a small business network with Active Directory, a Windows Server, and a Windows 10 client machine.

The core focus is on effectively sharing resources over the network and configuring NTFS and share permissions to control access for individual mock users and groups. 

This hands-on experience demonstrates the ability to:

- Create and manage network file shares with varying levels of access (Read, Write, Deny).
- Implement and troubleshoot user and group-based permissions using Active Directory.
- Gain practical experience with Windows Server and Windows 10 in a domain-joined environment.
- Develop a foundational understanding of security best practices related to file sharing.

---

## Using Powershell (administrator mode) to create the 4 folders


**Googled the code for Powershell command**
<p>
  <img src="https://github.com/user-attachments/assets/740e14b9-c001-49bd-b151-0e9be0ebdddd">
</p>

**Shows result. 4 new directories in the C: directory**
<p>
  <img src="https://github.com/user-attachments/assets/8cc46eaf-645b-4f9f-842d-575bed38cda3">
</p>

### Set the following permissions (share the folder)

- Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
    - right-click -> properties -> sharing -> share -> "domain users" -> add -> (defaults to "read" only)

<p>
  <img src="https://github.com/user-attachments/assets/c9c0429f-a6ae-4766-9469-469cc827948b">
</p>

<p>
  
</p>

- Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”
    - right-click -> properties -> sharing -> share -> "domain users" -> add -> "Read/Write"

<p>
  <img src="https://github.com/user-attachments/assets/0a7ed96b-f5ba-442f-ace0-e2f4abe3217b">
</p>

- Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
    - right-click -> properties -> sharing -> share -> "domain admins" -> add -> "Read/Write"
        - Grants "read/write" permissions to Admins-only, effectively allowing No-Access to regular Domain-Users 

<p>
  <img src="https://github.com/user-attachments/assets/8b3394d1-066d-4af3-803a-30baf402f6aa">
</p>

(Skip accounting for now)

### Attempt to access file shares as a normal user (from non-admin user account)
On Client-1, navigate to the shared folder (start, run, \\dc-1)
<p>
  <img src="https://github.com/user-attachments/assets/7d83b0a9-8899-4d37-b873-2570f880f46a">
</p>

Try to access the folders you just created. Which folders can you access? Which folders can you create stuff in? Does it make sense?

- Tried accessing read-access
    - Opened ok
    - Doesn't allow to Add a text file
 
<p>
  <img src="https://github.com/user-attachments/assets/9d016a0e-e437-46d9-98a9-51bbc51f9e4c">
</p>
<p>
  <img src="https://github.com/user-attachments/assets/d7304e74-3a29-4b3a-b018-7673fb93cde1">
</p>

- Tried accessing and writing to read-write
    - Opened ok
    - Allows to create a text file and edit the file

<p>
  <img src="https://github.com/user-attachments/assets/811668f0-e764-4607-84ea-a397f74f4add">
</p>
<p>
  <img src="https://github.com/user-attachments/assets/7cb97343-e548-4f5f-ada2-2b024306c3c5">
</p>

**allows Editing**

<p>
  <img src="https://github.com/user-attachments/assets/15719c5a-2ebe-4495-ab28-eca543ff4691">
</p>

- Tried accessing "no-access"
    - Access Denied

<p>
  <img src="https://github.com/user-attachments/assets/b71ea1f2-a95a-4d14-8534-8a95b04e3f4e">
</p>

      
#### Create an “ACCOUNTANTS” Security Group, assign permissions, an test access

Go back to DC-1, in Active Directory, create a security group called “ACCOUNTANTS”
- in Users and Computers, creat a new Organizational Unit (OU) called _GROUPS

<p>
  <img src="https://github.com/user-attachments/assets/a2ee5ed9-3ec7-46a8-8925-bdebd833dbb8">
</p>

- Create a new Security Group in that OU

<p>
  <img src="https://github.com/user-attachments/assets/45f5ec58-8225-4494-ad01-2ca9c52ca906">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/851d699b-09cc-4a07-b3f6-09568c9cdadc">
</p>

- On the “accounting” folder you created earlier, set the following permissions:
    - Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”

<p>
  <img src="https://github.com/user-attachments/assets/16b2e645-2840-4d0c-aaa1-d5490bcbe972">
</p>

- On Client-1, as  <someuser>, try to access the accountants folder. It should fail.

<p>
  <img src="https://github.com/user-attachments/assets/f950234f-6e51-4901-b971-e7788f060dda">
</p>

- Log out of Client-1 as  <someuser>

- On DC-1, make <someuser> a member of the “ACCOUNTANTS”  Security Group

<p>
  <img src="https://github.com/user-attachments/assets/fb957d24-31e7-4fee-a8ba-3b6362fda285">
</p>


- Sign back into Client-1 as <someuser> and try to access the “accounting” share in \\DC-1\ - Does it work now?

**Now have Read/Write access to "accounting" folder
<p>
  <img src="https://github.com/user-attachments/assets/89ed2a56-b7f5-4cf7-b00e-11159d7b5c47">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/47cd1245-d3eb-4ba8-86fb-048ba7dfa2e2">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/005eb77e-f68c-4240-8791-e441d1f6107b">
</p>

