<p align="center">
<img src="https://i.imgur.com/Ucqw15T.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>🏗️ Preparing Active Directory Infrastructure in Azure (Part 1)</h1>
<p>A hands-on lab using Microsoft Azure Virtual Machines to set up a foundational Active Directory environment. This is part one of a five-part learning series.</p>

<h1>🧠 Overview</h1>
<p>In this guide, you’ll learn how to deploy two virtual machines in Azure, which is a Domain Controller (DC-1) running Windows Server 2022 and a Client machine (Client-1) running Windows 10 Pro. You’ll place both machines on the same Virtual Network, configure private IP addressing and DNS, and verify communication between them—laying the groundwork for building a full AD lab.</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

## 🔧 Part 1: Create a Resource Group

1. Go to Azure Portal
2. In the left menu, click `Resource Groups` > `Create`
3. Set:
  - Name: `CyberLab`
  - Region: Closest to your location (e.g., East US)
4. Click `Review` + `Create` > `Create`

-----

## 🌐 Part 2: Create a Virtual Network & Subnet

1. In the Azure search bar, type `Virtual Network` > `Create`
2. Fill in:
    - Name: `CyberLab-VNet`
    - Region: Same as your Resource Group
3. Under IP Addressing: Leave default or set custom range
4. Under Subnets:
    - Click `Add Subnet`
    - Name: `CyberLab-Subnet`
    - Address range: `10.0.0.0/24`
5. Click `Review` + `Create` > `Create`

-----

## 🖥️ Part 3: Create the Domain Controller VM (DC-1)

1. Go to `Virtual Machines` > `Create`
2. Set the following:
    - Name: `DC-1`
    - Image: `Windows Server 2022`
    - Size: `B2s` (or similar small instance)
    - Username: `labuser`
    - Password: `Cyberlab123!`
3. Under Networking:
    - VNet: `CyberLab-VNet`
    - Subnet: `CyberLab-Subnet`
    - Public IP: `Enable` or `disable` (for labs, you can keep it on)
4. Click `Review` + `Create` > `Create`

-----

## 🔒 Part 4: Assign a Static Private IP to DC-1
	
1. Navigate to `DC-1` > `Networking`
2. Click on the `Network Interface (NIC)`
3. Under `IP Configurations`, click the listed `IP address`
4. Change Assignment from `Dynamic` to `Static`
5. Click `Save`

***(Note down this IP address (you’ll need it for Client-1’s DNS)***

-----

## 🔥 Part 5: Disable Windows Firewall on DC-1 (for testing only)
	
1. Go to `DC-1` > `Connect` > `RDP` and download the `RDP file`
2. Log in with:
     - Username: `labuser`
     - Password: `Cyberlab123!`
3. Search for `Windows Defender Firewall` in the `Start` menu
4. Click `Turn Windows Defender Firewall` on or off
5. Disable firewall for both Private and Public networks

***🚨 Important: This is only for lab testing. Re-enable the firewall later in production environments.***

-----

## 💻 Part 6: Create the Client VM (Client-1)

1. In Azure, go to `Virtual Machines` > `Create`
2. Set:
    - Name: `Client-1`
    - Image: `Windows 10 Pro`
    - Username/Password: Choose credentials you’ll remember
3. Under Networking:
    - VNet: `CyberLab-VNet`
    - Subnet: `CyberLab-Subnet`
4. Click `Review` + `Create` > `Create`

-----

## 🧭 Part 7: Point Client DNS to the Domain Controller

1. Go to `Client-1` > `Networking`
2. Click the `NIC (Network Interface)`
3. Under `DNS Servers`, select `Custom`
4. Enter the private IP address of DC-1
5. Click `Save`

-----

## 🔁 Part 8: Restart Client-1

1. In Azure, go to the `Client-1 VM`
2. Click `Restart`

-----

## 🧪 Part 9: Test Connectivity Between VMs

1. Connect to Client-1 via RDP
2. Open Command Prompt
3. Type: `ping` <DC-1 private IP>**
    - Example: `ping 10.0.0.4`

***You should see reply from messages. If so, the connection works!***

-----

## 🧠 Part 10: Check DNS Configuration

1. On Client-1, open PowerShell
2. Run: `ipconfig /all`
3. Scroll to `DNS Servers`
4. You should see the private IP address of DC-1

***✅ This confirms that Client-1 is pointing to DC-1 for DNS resolution.***

-----

### ✅ Conclusion

You’ve successfully:

- Set up a Windows Server 2022 Domain Controller
- Created a Windows 10 Client VM
- Connected both VMs in a private Azure Virtual Network
- Configured DNS and verified network communication

👉 You’re now ready to move on to [Part 2: Deploying Active Directory](https://github.com/anumkhanit/deploy-ad) where you’ll promote DC-1 as a domain controller and begin domain management.

-----

### 🧠 Want to Learn More?

- [Azure Virtual Machines Docs](https://learn.microsoft.com/en-us/azure/virtual-machines/)

- [Active Directory Basics](https://serveracademy.com/blog/active-directory-101-a-step-by-step-tutorial-for-beginners/)
