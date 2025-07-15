<p align="center">
<img src="https://i.imgur.com/Ucqw15T.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>Preparing AD Infrastructure in Azure</h1>
<p>A hands-on five parts of guide on using Azure Virtual Machines to learn how to build and use Active Directory</p>

<h2>Environments and Technologies to use</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)

<h2>Operating Systems to use</h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of them)***

-----

## Part 1: Set Up the Domain Controller (DC-1)

1. **Create a Resource Group**
   - Go to the Azure Portal: `https://portal.azure.com`
   - In the left menu, click Resource Groups and Create

2. **Choose:**
   - Subscription: Your default one
   - Resource Group Name: `CyberLab`
   - Region: Pick the region closest to you (e.g., East US)

3. **Click Review then click Create**

-----

## Part 2: Create a Virtual Network and Subnet

1. **In the Azure search bar, type Virtual Network and Create**

2. **Fill out:**
   - Name: `CyberLab-VNet`
   - Region: Same as your Resource Group

3. **Under IP Addressing, keep the default range or customize it.**

4. **Under Subnets, click Add subnet**
    - Name: `CyberLab-Subnet`
    - Subnet range: Leave default or pick `10.0.0.0/24`

5. **Click Review and Create**

-----

## Part 3: Create the Domain Controller VM (Windows Server 2022)

1. **In Azure, search Virtual Machines and Create**

2. **Set the following:**
    - Name: `DC-1`
    - Region: Same as VNet
    - Image: Windows Server 2022
    - Size: Use a small size (e.g., B2s)
    - Username: (make any easy username to remember)
    - Password: (make any easy password to remember)

3. **Under Networking:**
     - Select `CyberLab-VNet`
     - Choose the `CyberLab-Subnet`
     - Public IP: You can enable or disable it (for lab, it’s okay to have it on)

4. **Click Review and Create**

-----

## Part 4: Set DC-1’s Private IP to Static
	
1.	**Go to `DC-1` > `Networking`**

2.	**Click on the Network Interface (NIC)**

3.	**Under IP Configurations, click the listed IP address**

4.	**Change Assignment from `Dynamic` to `Static`**

5.	**Click Save**

***(Note down this IP address (you’ll need it for Client-1’s DNS)***

-----

## Part 5: Disable Windows Firewall on DC-1 (for testing)
	
1.	**Go to `DC-1` > `Connect` > `RDP and download the RDP file`**

2.	**Log into the VM using:**
     - Username: `labuser`
     - Password: `Cyberlab123!`

3.	**Open `Start Menu` > Search for `Windows Defender Firewall`**

4.	**Click `Turn Windows Defender Firewall` on or off**

5.	**Disable for both private and public networks (for lab testing only)**

-----

## Part 6: Set Up the Client VM (Client-1)

1. **In Azure, go to Virtual Machines and Create**
    - Name: `Client-1`
    - Image: Windows 10 Pro
    - Username: (make any easy username to remember)
    - Password: (make any easy password to remember)

2.	**In Networking:**
    - Virtual Network: Select `CyberLab-VNet`
    - Subnet: `CyberLab-Subnet`

3.	**Click Review and Create**

-----

## Part 7: Set DNS to Point to DC-1

1.	**Go to `Client-1` > `Networking`**

2.	**Click the `NIC (network interface)`**

3.	**Under `DNS Servers`, select `Custom`**

4.	**Enter `DC-1`’s Private IP Address**

5.	**Click `Save`**

-----

## Part 8: Restart `Client-1`

1.	**Go to `Client-1` VM in Azure**

2.	**Click `Restart`**

-----

## Part 9: Test the Connection to `DC-1`

1. **Connect to `Client-1` via RDP**
   - Use the same username and password

2.	**Open `Command Prompt`**

3.	**Type: `ping` <DC-1 private IP>**
    - Example: `ping 10.0.0.4`

4.	**You should see reply from messages. If so, the connection works!**

-----

## Part 10: Check DNS Settings from PowerShell

1.	**Still on `Client-1`, open `PowerShell`**

2.	**Run: `ipconfig /all`**

3.	**Scroll to `DNS Servers` section**

4.	**It should show `DC-1`’s Private IP address as the DNS server**

-----

## Conclusion

You’ve now created a Domain Controller (DC-1) on Windows Server 2022 and a Client machine (Client-1) on Windows 10. In which, both are in the same Azure virtual network, and Client-1 can reach the DC!

Move onto the next part: [Deploying Active Directory](https://github.com/anumkhanit/deploy-ad)
