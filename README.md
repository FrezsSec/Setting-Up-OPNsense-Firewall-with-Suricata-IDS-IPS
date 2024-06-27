# Setting Up OPNsense Firewall with Suricata IDS/IPS

I successfully set up and configured an OPNsense firewall in VirtualBox, integrating Suricata as the Intrusion Prevention System (IPS) and Intrusion Detection System (IDS). This lab project also involved configuring custom rules to detect stealth scans such as SYN scan, XMAS scan, and NULL scan attacks, as well as FTP brute force and SSH brute force attacks. The setup included a network environment comprising OPNsense as the firewall, Kali Linux for security testing, and Metasploitable 2 as a vulnerable machine. The project aimed to validate the effectiveness of our detection system by ensuring timely and accurate alerts.

## Tools

- **Hypervisor**: VirtualBox
- **Firewall**: OPNSense
- **Attacker**: Kali Linux
- **Vulnerable Machine**: Metasploitable 2

## Network Design

![Network diagram example](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/4918b9c5-8d51-4d47-8d83-02d9d91c9843)

## Guide

### Selecting and Downloading a Hypervisor

We will need a Hypervisor to install all of our tools and services.

I'll be using VirtualBox as it is a great free alternative for virtualization.

- [VirtualBox Download](https://www.virtualbox.org/)

## Instructions to Install OPNsense on VirtualBox

1. Download the latest OPNsense ISO image from the [official website](https://opnsense.org/download/). Under "System architecture," select "amd64" for 64-bit systems. For "Image type," choose "dvd". Scroll down to the "Mirrors" section and select a download location nearest to you. After downloading, verify the integrity of the file by comparing its SHA256 checksum with the one provided on the download page.

2. Open VirtualBox and click "New" to create a new virtual machine.
3. Browse the OPNssense CE ISO file and select Next
4. Name your VM (e.g., "OPNsense Firewall"), select `BSD` as the type, and `FreeBSD (64-bit)` as the version.

5. I allocated 2GB of RAM to the virtual machine. If you have limited resources, you can allocate less. You can check the required resources on the [Hardware Sizing & Setup page](https://docs.opnsense.org/manual/hardware.html) on the OPNsense website..
   
![Capture](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/cc723f1d-797a-46e5-b952-0d8ee27c2d5a)

7. I created a virtual hard disk with 8GB of space. You can check the required resources on the [Hardware Sizing & Setup page](https://docs.opnsense.org/manual/hardware.html) on the OPNsense website.

![2](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/f2b5dda0-72e9-4ec6-adea-0bff9de9e77a)

8. Then click next and finish.

 ![3](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/b07294d1-359d-468e-9942-4d7449b144d1)


8. After creating the VM, Navigate to the "Network" settings to set up our adapters. For OPNsense to work correctly, we need to have at least two interfaces on the firewall: one for WAN (connected to the internet) and the other for LAN. I also added an additional processor in the system settings.

    - I set up **Adapter 1** to `NAT`, so Adapter 1 is going to be our WAN.
    - I set up **Adapter 2** to `Internal Network` for the LAN interface.

![4](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/86c69c4c-f785-4f38-a6a8-76587cfbcec6)

![5](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/c2c21187-935e-45ff-95b1-05ec38a53cce)

9. Start the virtual machine. After the setup is complete, you will be prompted with the login screen. Log in with the username `installer` and password `opnsense`.

![8](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/7bafd271-16b2-4b3b-87a3-dbb9d15e03f8)

10. Continue with the default key map.

 ![9](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/5c4ca734-dad2-47f8-a2b9-97809e79f3b7)

11. Choose Install (UFS)

![10](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/a23acaff-f4ba-40f2-8148-df30c0aa42dc)

12. Choose virtual disck(ada0) and hit enter.

![11](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/b7f036fe-a5b6-4338-b079-1ba2ff75cc43)

13. Select `YES` and press `Enter`.


![12](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/efe4d3e2-2cdf-43f8-9976-8e35b13544a9)


14. Once the installation is complete, it will ask you if you would like to set up your own root password. In my case, I will set it up.

![13](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/2f1c5926-a909-4a6a-94f5-5e213ffd89a3)

![14](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/6535c2ea-f342-4914-a792-d5a0bba07c35)

15. Once that is done, select "Complete Install."

![15](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/cdcc4723-1869-485a-af04-f72b9b345b68)

16. Power off OPNsense, go to Settings, navigate to System and Storage, and remove the OPNsense DVD to ensures the system will boot from the virtual hard disk on the next start.

![remove](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/3d817c2c-242e-4a26-b2f7-001c54f259ed)

17. Start the OPNsense virtual machine again, proceed to log in.

![16](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/17a071d3-f761-40e6-922d-cf282635a815)


Now we are going to assign interfaces. By default, the system assigns `em0` as LAN and `em1` as WAN. We need to reassign them to align with our earlier configuration.

19. Use option 1 to assign interfaces:
    - `em0` for WAN
    - `em1` for LAN

![18](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/529f4f85-b9d9-4933-8688-1e048c9d3166)


      
20. Next, we are going to choose IP addresses for each interface. Select option 2. In my lab, I will assign 10.50.50.254 with subnet mask 255.255.255.0 (subnet 24) to the LAN interface.

![19](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/5f79b677-9b12-4889-a3b3-f4aa470c9d3d)


![20](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/54428977-3d0c-4184-a0f8-22730803952b)

21. The WAN interface will automatically obtain an IP address from DHCP.

![22](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/5aaa3f5a-1da2-4bbe-a753-1a5fcb9c9c84)




Now we can access the OPNsense web interface from another device on the same network using the LAN IP address. Before that I Configure Kali Linux to Connect to OPNsense Firewall.

## Connecting Kali Linux to OPNsense Firewall

1. Now, configure your Kali Linux VM to connect to the OPNsense firewall's LAN interface:

   - Ensure your Kali Linux VM has two network adapters:
     - **Adapter 1:** Set to NAT or Bridged (for internet access).
     - **Adapter 2:** Set to Internal Network (same as OPNsense's LAN interface).

   - Boot up your Kali Linux VM.

   - Open a terminal and check your network interfaces:
     ```bash
     ip a
     ```
     You should see `eth0` and `eth1` (or similar names).

   - Edit the network configuration file:
     ```bash
     sudo nano /etc/network/interfaces
     ```
     
     Add the following configuration for `eth1` (adjust the IP address as needed):
     ```plaintext
     auto eth1
     iface eth1 inet static
     address 10.50.50.100
     netmask 255.255.255.0
     gateway 10.50.50.254
     ```
     Ensure the IP address (`10.50.50.100` in this example) is within the LAN subnet of your OPNsense firewall and the gateway (`10.50.50.254`) is set to OPNsense's LAN IP.

   - Save the file and exit the editor.

   - Restart the networking service:
     ```bash
     sudo systemctl restart networking
     ```

   - Verify the connection:
     ```bash
     ping 10.50.50.254
     ```
     Replace `10.50.50.254` with the IP address of your OPNsense LAN interface to verify connectivity.

## Configuring OPNsense Firewall via web interface

1. Access the OPNsense web interface from kali browser using http://10.50.50.254.
   
2. Log in with the credentials (username: `root`, password: `the password set up earlier`).

![24](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/8fd77df5-768c-42f2-8067-e21315719e57)

3. To optimize OPNsense for better performance in VirtualBox, Navigate to the "System" tab and select "Firmware" from the dropdown menu. Check for updates by clicking on "Check for Updates" or a similar option, which will automatically search for available updates, including the VirtualBox Extension if applicable.


![25](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/589d7be2-374a-4878-881f-3117d8631ade)

4. Then, check the "Updates" tab. You will see a list of packages that need to be upgraded. Click "Update" to upgrade all the listed packages.

![28](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/d2b8e709-a1a6-431c-b0c4-3ac452fd9a18)

 ![30](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/bf8ec1fa-ea46-46e5-b771-ddbc584c0acc)

5. Now, in the "Plugins" tab, scroll down to find "os-virtualbox" and install it.

![27](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/50dee26e-e49a-4dc6-a40b-4c252a4ac90a)

7. Follow the OPNsense setup wizard to configure firewall rules, DHCP settings, and any other required configurations.

8. Save your changes and apply them to activate the firewall rules and settings.

## Configuring IPS/IDS on OPNsense

### Instructions

1. Log in to the OPNsense dashboard.
2. Navigate to "Interfaces" and then "Settings".
3. Make sure the offloading features are disbaled to ensure proper operation of the IPS.

![31](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/59f35dba-198a-4e7f-a25d-5177040604a9)

4. Next, Go to "Services" > "Intrusion Detection". Click on "Administration" and select "Advanced Mode".
5. Enable Intrusion Detection by toggling the "Enable" button.
6. Enable IPS mode and promiscuous mode for enhanced detection capabilities.
7. Enable syslog alerts for logging purposes.
8. Select "Hyperscan" under "Pattern Matcher" for modern pattern matching capabilities.
9. Apply these settings to the LAN interface (and optionally to the WAN interface).
10. Enter your LAN subnet in the "Home Network" field.
11. Click "Apply" to save the configuration.

![32](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/dd8d96d8-97ee-4edd-b09a-c1ef71d8deef)

### Downloading Rule Sets

1. Navigate to the Download Tab for the next part of the setup. within the download tab, you can grab some free rule sets to get started. However, it's important to know these free sets aren't ideal for standalone use. They might not be updated as often as some paid, more professional options.

![33](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/473e10bb-76b4-4660-b64a-93d737d23d9a)

2. Selecting and Downloading Rule Sets. Choose the rule sets you wish to use and click "Download" to add them to your OPNsense configuration. Ensure that the selected rule sets align with your security requirements and network environment.

### Adding Custom Rules for IDS/IPS on OPNsense
I have created custom rules for Suricata that will be applied in this lab. OPNsense IDS/IPS utilizes Suricata. These custom rules are designed to detect Nmap SYN scans, Xmas scans, Fin scans, SSH brute-force attacks, and FTP brute-force attacks. To test these rules, I will use Nmap, a port scanning tool, and Hydra for performing brute-force attacks on Metasploitable2.

![34](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/6fccb453-5006-4142-9e6a-b33dcdf4e2c4)

 To add custom Suricata rules to OPNsense, you can follow these steps:

1. Access OPNsense Web Interface. Log in to the OPNsense web interface using your administrator credentials.
2. Navigate to System > Settings > Administration. Enable Secure Shell, permit root user login (not recommended for production environment), allow password authentication, and set the SSH listening interface to LAN. Save the changes to apply the SSH configuration.

![35](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/63bc3abc-6821-4297-9d5d-0e1297bfa679)

3. Alongside the `mycustom.rules` file, create a new document named `mycustom.xml` in the samw folder with the following content:

```xml
<?xml version="1.0"?>
<ruleset documentation_url="http://docs.opnsense.org/">
  <location url="http://10.50.50.100/" prefix="customnmap" />
  <files>
    <file description="customnmap rules">customnmap.rules</file>
    <file description="customnmap" url="inline::rules/customnmap.rules">customnmap.rules</file>
  </files>
</ruleset>
```
This XML file is designed to locate our mycustom.rule file. Next, we will transfer our .xml file using FileZilla. To install FileZilla, you can use the following command:
```
sudo apt-get install filezilla
```

























































