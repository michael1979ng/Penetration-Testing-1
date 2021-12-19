Week 16 Homework Submission File: Penetration Testing 1
Step 1: Google Dorking
Using Google, can you identify who the Chief Executive Officer of Altoro Mutual is:

Found the information on following website: demo.testfire.net Inside Altoro -> About Altoro Mutual -> Executives & Management
Executives & Management - Altoro Mutual

Karl Fitzgerald, Chairman & Chief Executive Officer of Altoro Mutual

![image](https://user-images.githubusercontent.com/93474690/146691813-ae70f954-84a1-4bda-ba64-88edad16e1ed.png)

How can this information be helpful to an attacker:

This information is very useful to a Hacker (attacker) who wants to send phishing email directly to the executive members.
Step 2: DNS and Domain Discovery
Enter the IP address for demo.testfire.net into Domain Dossier and answer the following questions based on the results:

Where is the company located:
Sunnyvale, CA, 94085 - USA

What is the NetRange IP address:
65.61.137.64 - 65.61.137.127

What is the company they use to store their infrastructure:

CustName:       Rackspace Backbone Engineering
Address:        9725 Datapoint Drive, Suite 100
City:           San Antonio
StateProv:      TX
PostalCode:     78229
Country:        US
RegDate:        2015-06-08
Updated:        2015-06-08
Ref:            https://rdap.arin.net/registry/entity/C05762718

![image](https://user-images.githubusercontent.com/93474690/146691853-f08ecad8-9b24-48da-80ad-e626fa919c31.png)

What is the IP address of the DNS server:
65.61.137.117

![image](https://user-images.githubusercontent.com/93474690/146691870-bdff3459-5c74-4ffa-b499-f7600c4d0533.png)

Step 3: Shodan
What open ports and running services did Shodan find:
Open Ports: 80, 443, 8080 on the following website: https://www.shodan.io/host/65.61.137.117

![image](https://user-images.githubusercontent.com/93474690/146691891-529d34fe-3dd3-4678-a958-136b5dae93a0.png)

Step 4: Recon-ng
Install the Recon module xssed.
Search the module xssed by entering the command marketplace search xssed
Install the module xssed by entering the command marketplace install recon/domains-vulnerabilities/xssed
Load the module xssed by entering the command modules load recon/domains-vulnerableilities/xssed

![image](https://user-images.githubusercontent.com/93474690/146691909-a895181b-65b4-420f-bbf1-c439c4a1707c.png)

Set the source to demo.testfire.net.
Check the details of the module xssed by entering info
To change the SOURCE from default to demo.testfire.net enter command options set SOURCE demo.testfire.net

![image](https://user-images.githubusercontent.com/93474690/146691928-9f4084c1-421b-4777-aaec-ec90a0d1f00b.png)

Run the module.
run

![image](https://user-images.githubusercontent.com/93474690/146691953-bca38aa6-f668-4cd1-a485-abca37c30e2b.png)

s Altoro Mutual vulnerable to XSS: Yes it was the only vulnerablities found, as shown above image

Enter the following script in the Search bar on the website: <script>alert("Hello")</script>

![image](https://user-images.githubusercontent.com/93474690/146691969-be39f262-7c7c-4a31-9c0d-59e81b6f251d.png)

Step 5: Zenmap
Your client has asked that you help identify any vulnerabilities with their file-sharing server. Using the Metasploitable machine to act as your client's server, complete the following:

Command for Zenmap to run a service scan against the Metasploitable machine:
Run the RDP to start the Pentesting Lab.
Open the Hyper-V-Manager to view all the active VM's.
Double click the Kali VM to start.
Start the Terminal
From the Terminal enter the command zenmap
The Metasploitable machine IP address is 192.168.0.10
Input Target 192.168.0.10
Profile: Quick scan
The raw nmap Command is nmap -T4 -A 192.168.0.10
Click the SCAN button to get the results.

![image](https://user-images.githubusercontent.com/93474690/146691991-407de5a9-192f-41ce-863a-593c516a4e27.png)

Bonus command to output results into a new text file named zenmapscan.txt:
To save the results in the zenmapscan.txt add the following on the command line: -oN zenmapscan.txt so it should look as following: nmap -T4 -A -oN zenmapscan.txt 192.168.0.10
You can also enter this in the Terminal command line: nmap -T4 -A -oN zenmapscan.txt 192.168.0.10

![image](https://user-images.githubusercontent.com/93474690/146692004-6360764f-e6b7-4f1a-b649-63033a77edac.png)

zenmapscan.txt - file

Zenmap vulnerability script command:

There are two scrips that can be located in Zenmap for vulnerability associated with the service running on the 139/445 port.
From the Profile tab select Edit Selected Profile
Select the Scripting tab and view all the scripts.
Check the box off for the ftp-vsftpd-backdoor and smb-enum-shares.
Save Changes to save the profile settings.
The raw command should look like:
nmap -T4 -F --script ftp-vsftpd-backdoor,smb-enum-shares 192.168.0.10

-T4: -T<0-5>: Set timing template (higher is faster)
-F: Fast mode - Scan fewer ports than the default scan
--script: Runs the scripted scan that is followed.
ftp-vsftpd-backdoor: This script attempts to exploit the backdoor using the innocuous id command by default, but that can be changed with the exploit.cmd or ftp-vsftpd-backdoor.cmd ...
smb-enum-shares: Host script results: | smb-enum-shares: | account_used: WORKGROUP\​Administrator | ADMIN$ | Type: STYPE_DISKTREE_HIDDEN | Comment: Remote Admin ...
192.168.0.10: IP address of host that will be scanned.
Once you have identified this vulnerability, answer the following questions for your client:

What is the vulnerability:

As per the below snapshots Zenmap was able to enumerate the vulnerable service for port 139/445.

![image](https://user-images.githubusercontent.com/93474690/146692026-b8bf751c-d771-4645-833f-8058556db2c1.png)

![image](https://user-images.githubusercontent.com/93474690/146692033-6c1d1a5a-25da-4c6e-b609-7ed8491fdc7e.png)

Why is it dangerous:

This is dangerous due to the VSFTPD 2.3.4 backdoor attack can be applied on port 21 via a malicious code, if successful execution, opens the backdoor on port 6200.

This backdoor was introduced into the vsftpd-2.3.4.tar.gz archive between June 30th 2011 and July 1st 2011 according to the most recent information available. This backdoor was removed on July 3rd 2011. (1)
The concept of the attack on VSFTPD 2.3.4 is to trigger the malicious vsf_sysutil_extra(); function by sending a sequence of specific bytes on port 21, which, on successful execution, results in opening the backdoor on port 6200 of the system and running as root. (2)
The Windows Server Message Block (SMB) get access through the organization's networks, the SMB protocols used by PC's for file and printer sharing, along with the remote access services.

SMB vulnerabilities allow their payloads to spread laterally through connected systems. (3)
What mitigation strategies can you recommendations for the client to protect their server:

The vsFPTD 2.3.4 patch was released on July 3, 2011 with the patch constantly monitered and updated.

The vsFTPD v2.3.4 backdoor reported on 2011-07-04 (CVE-2011-2523). (4)
The SMB (CVE-2017-0145) patch was released by Microsoft MS17-010 , and the SAMBA (CVE-2017-0145) patches were released by Red Had for Linux RHSA-2017:1390 (5, 6 & 7)
