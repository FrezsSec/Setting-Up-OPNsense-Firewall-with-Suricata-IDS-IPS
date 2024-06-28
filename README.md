# Setting Up OPNsense Firewall with Suricata IDS/IPS

I successfully set up and configured an OPNsense firewall in VirtualBox, integrating Suricata as the Intrusion Prevention System (IPS) and Intrusion Detection System (IDS). This lab project also involved configuring custom rules to detect stealth scans. The setup included a network environment comprising OPNsense as the firewall, Kali Linux for security testing, and Metasploitable 2 as a vulnerable machine. The project aimed to validate the effectiveness of our detection system by ensuring timely and accurate alerts.

## Tools

- **Hypervisor**: VirtualBox
- **Firewall**: OPNSense
- **Attacker**: Kali Linux
- **Vulnerable Machine**: Metasploitable 2

## Network Design
The network diagram depicts a simple setup with OPNsense firewall configured with two interfaces: one WAN (Internet-facing) and one LAN. The LAN includes Kali Linux for conducting security testing and Metasploitable 2 as a vulnerable machine.

![4 drawio](https://github.com/FrezsSec/Setting-Up-OPNsense-Firewall-with-Suricata-IDS-IPS/assets/173344802/fe4ab9a1-5073-45b6-848a-9256eede6399)


## Guide

### Selecting and Downloading a Hypervisor

To install all of our tools and services, a Hypervisor is required.

I'll be using VirtualBox, a robust free virtualization alternative.

- [VirtualBox Download](https://www.virtualbox.org/)

## Instructions to Install OPNsense on VirtualBox

1. Download the latest OPNsense ISO image from the [official website](https://opnsense.org/download/). Under "System architecture," select "amd64" for 64-bit systems. For "Image type," choose "dvd". Scroll down to the "Mirrors" section and select a download location nearest to you. After downloading, verify the integrity of the file by comparing its SHA256 checksum with the one provided on the download page.

2. Open VirtualBox and click "New" to create a new virtual machine.
3. Browse the OPNssense CE ISO file and select Next.
4. Name your VM (e.g., "OPNsense Firewall"), select `BSD` as the type, and `FreeBSD (64-bit)` as the version.

5. Allocate 2GB of RAM to the virtual machine. Adjust based on available resources. Refer to the [Hardware Sizing & Setup page](https://docs.opnsense.org/manual/hardware.html) on the OPNsense website for guidance.
   
![Capture](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/cc723f1d-797a-46e5-b952-0d8ee27c2d5a)

6. Create a virtual hard disk with 8GB of space, or as per your requirements.

![2](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/f2b5dda0-72e9-4ec6-adea-0bff9de9e77a)

7. Then click next and finish.

 ![3](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/b07294d1-359d-468e-9942-4d7449b144d1)


8. After creating the VM, Navigate to the "Network" settings to set up our adapters. For OPNsense to work correctly, we need to have at least two interfaces on the firewall: one for WAN (connected to the internet) and the other for LAN. 

    - set up **Adapter 1** to `NAT`, so Adapter 1 is going to be our WAN.
    - set up **Adapter 2** to `Internal Network` for the LAN interface.

![4](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/86c69c4c-f785-4f38-a6a8-76587cfbcec6)

![5](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/c2c21187-935e-45ff-95b1-05ec38a53cce)

9. Start the virtual machine. After the setup is complete, you will be prompted with the login screen. Log in with the username `installer` and password `opnsense`.

![7](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/2a93fee1-e10d-429e-abe3-bc277756273b)


10. Continue with the default key map.

 ![9](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/5c4ca734-dad2-47f8-a2b9-97809e79f3b7)

11. Select Install (UFS)

![10](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/a23acaff-f4ba-40f2-8148-df30c0aa42dc)

12. Choose the virtual disck(ada0) and hit enter.

![11](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/b7f036fe-a5b6-4338-b079-1ba2ff75cc43)

13. Confirm with `YES` and continue..


![12](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/efe4d3e2-2cdf-43f8-9976-8e35b13544a9)


14. Once installation completes, set your root password as prompted.

![13](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/2f1c5926-a909-4a6a-94f5-5e213ffd89a3)

![14](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/6535c2ea-f342-4914-a792-d5a0bba07c35)

15. Once that is done, select "Complete Install."

![15](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/cdcc4723-1869-485a-af04-f72b9b345b68)

16. Power off OPNsense, go to Settings, navigate to System and Storage, and remove the OPNsense DVD to ensures the system will boot from the virtual hard disk on the next start.

![remove](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/3d817c2c-242e-4a26-b2f7-001c54f259ed)

17. Start the OPNsense virtual machine again, proceed to log in.

![16](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/e06bf795-5f64-4d0c-a09b-37990dd67222)



Now we are going to assign interfaces. By default, the system assigns `em0` as LAN and `em1` as WAN. We need to reassign them to align with our earlier configuration.

18. Use option 1 to assign interfaces:
    - `em0` for WAN
    - `em1` for LAN

![18](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/529f4f85-b9d9-4933-8688-1e048c9d3166)


      
19. Next, we are going to choose IP addresses for each interface. Select option 2. In my lab, I assigned 10.50.50.254 with subnet mask 255.255.255.0 (subnet 24) to the LAN interface.

![19](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/5f79b677-9b12-4889-a3b3-f4aa470c9d3d)


![20](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/54428977-3d0c-4184-a0f8-22730803952b)

20. The WAN interface will automatically obtain an IP address from DHCP.

![21](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/1e782a8a-a858-4449-bf25-28f415b6ff51)


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
   
2. Log in with the credentials (username: `root`, password: `[the password you set during installation]`.

![24](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/8fd77df5-768c-42f2-8067-e21315719e57)

3. To optimize OPNsense for better performance in VirtualBox, Navigate to the "System" tab and select "Firmware" from the dropdown menu. Check for updates by clicking on "Check for Updates" or a similar option, which will automatically search for available updates, including the VirtualBox Extension if applicable.


![25](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/589d7be2-374a-4878-881f-3117d8631ade)

4. Go to the "Updates" tab. If there are packages that need to be upgraded, click "Update" to install them.

![28](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/d2b8e709-a1a6-431c-b0c4-3ac452fd9a18)

 ![30](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/bf8ec1fa-ea46-46e5-b771-ddbc584c0acc)

5. Now, in the "Plugins" tab, scroll down to find "os-virtualbox" and install it.

![27](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/50dee26e-e49a-4dc6-a40b-4c252a4ac90a)

6. Follow the setup wizard prompts to configure firewall rules, DHCP settings, and any other necessary configurations according to your lab requirements.

7. Save your changes and apply them to activate the firewall rules and settings.

## Configuring IPS/IDS on OPNsense

### Instructions

1. Log in to the OPNsense dashboard.
2. Navigate to "Interfaces" and then "Settings".
3. Ensure that offloading features are disabled to ensure proper operation of the IPS.

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

1. Navigate to the Download Tab for the next part of the setup. Within the "Download" tab, you can access various rule sets to enhance your OPNsense configuration. It's important to note that the free rule sets available may not be as comprehensive or frequently updated as paid options.

![33](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/473e10bb-76b4-4660-b64a-93d737d23d9a)

2. Select the rule sets that best fit your security requirements and network environment. Click "Download" or the appropriate action to add these rule sets to your OPNsense configuration.

### Adding Custom Rules for IDS/IPS on OPNsense
Here is a custom rule to detect SYN scans using Suricata:

```alert tcp any any -> $HOME_NET any (msg:"Possible Nmap SYN Scan"; flow:stateless; flags:S; threshold:type limit, track by_src, count 50, seconds 1; priority:5; classtype:attempted-recon; sid:101; rev:4;)```


 ### Steps to Add Custom Suricata Rules to OPNsense

1. Access OPNsense Web Interface. Log in to the OPNsense web interface using your administrator credentials.
2. Navigate to System > Settings > Administration. Enable Secure Shell, permit root user login (not recommended for production environment), allow password authentication, and set the SSH listening interface to LAN. Save the changes to apply the SSH configuration.

![35](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/63bc3abc-6821-4297-9d5d-0e1297bfa679)

3. Alongside the `custom-suricata-rules.rules` file, create a new document named `custom-suricata-rules.xml` in the same folder with the following content:

```
<?xml version="1.0"?>
<ruleset documentation_url="http://docs.opnsense.org/">
  <location url="http://10.50.50.100/" prefix="custom-suricata-rules" />
  <files>
    <file description="Custom Suricata Rules">custom-suricata-rules.rules</file>
    <file description="Custom Suricata Rules" url="inline::rules/custom-suricata-rules.rules">custom-suricata-rules.rules</file>
  </files>
</ruleset>
```
This XML file is designed to locate our `custom-suricata-rules.rules` file on our Kali Linux virtual machine. Next, we will transfer our .xml file using FileZilla. To install FileZilla, you can use the following command:
```
sudo apt-get install filezilla
```
4. Open FileZilla and connect to your OPNsense firewall using SFTP (sftp://10.50.50.254), username, password, and port.

5. Transfer the custom-suricata-rules.xml file to the directory: `usr/local/opnsense/scripts/suricata/metadata/rules`.

![22](https://github.com/FrezsSec/Setting-Up-OPNsense-Firewall-with-Suricata-IDS-IPS/assets/173344802/6228e57d-206d-4a6e-b669-6110be93e610)

6. In the directory where your files are stored, set up an HTTP server to deliver the .rules file. This server's purpose is to deliver our .rules file to OPNsense whenever it's requested by the .xml file. To start the server, run:

``` python3 -m http.server 80 ```

![38](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/8af92f5f-b3f5-4ae6-a064-8577e5eb2414)

7. Navigate to "Services" -> "Intrusion Detection" -> "Administration" -> "Download" on OPNsense web interface. At the top, click on "Restart Service" to apply changes. After restarting, find the section for `custom-suricata-rules/Custom Suricata Rules rules`, select it, and click Enable Selected. Click Download & Update Rules to ensure the custom rules are updated and activated.

![41](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/28e71fa2-8465-4c7a-8ea5-4ce0ba37b7d1)

8. Check the "Rules" section in OPNsense to confirm that your custom rule has been successfully added.

![last](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/32a35a58-c699-4314-913d-73211b56c82d)

## Testing
To assess our firewall's ability to detect intrusion attempts, we will use Nmap. Since we have configured our rule to detect stealth scans, we can perform a scan and verify if the system alerts us to this activity.
Open a terminal on your Kali Linux VM. Run the following command to perform a stealth SYN scan:

``` sudo nmap -Pn -sS -p- 10.50.50.20 ```

Replace `[IP_ADDRESS]` with the IP address of the device you are scanning. This should be an IP address on your network that the OPNsense firewall is protecting.

![45](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/1b474e0e-c124-432e-a0de-3302852a179a)

Return to the OPNsense web interface. Navigate to the Alerts section. Refresh the Alerts page to see if any alerts related to the Nmap scan appear.

![last1](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/c741f67d-cd43-49be-8fb5-d250bf635d8f)

## Conclusion

In this lab exercise, we installed and configured an OPNsense firewall on VirtualBox. We proceeded to set up IDS/IPS using Suricata on the OPNsense platform. Additionally, we created a custom rule tailored to detect specific network reconnaissance activities, such as SYN scans. By conducting a controlled port scan using Nmap against a device on our network, we confirmed that our custom rule effectively triggered alerts, demonstrating the firewall's ability to detect and respond to potential intrusion attempts.

## Resources
A big shout-out to [LS111](https://www.youtube.com/@ls111cyberEd) whose videos were incredibly helpful throughout this lab. Their clear explanations and demos made everything click!





































