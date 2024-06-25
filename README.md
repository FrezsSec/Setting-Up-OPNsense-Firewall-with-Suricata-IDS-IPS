# Network Security Lab Setup

Network Security Lab Setup with a Firewall, IDS/IPS, Exploitable machines, and an Attacker.

## Introduction

In this project, I will set up a secure network lab to practice and demonstrate various network security concepts and configurations. The lab will include an attack machine (Kali Linux), a vulnerable target (Metasploitable 2), and a pfSense firewall with IDS/IPS capabilities.

## Tools

- **Hypervisor**: VirtualBox
- **Firewall**: OPNSense
- **Attacker**: Kali Linux
- **Vulnerable Machine**: Metasploitable 2

## Network Design

![Network diagram example](https://github.com/FrezsSec/Building-a-Secure-Network-Lab-Firewall-and-IDS-IPS-with-pfSense-and-Metasploitable-2/assets/173344802/4918b9c5-8d51-4d47-8d83-02d9d91c9843)

![Network Topology](URL_TO_YOUR_IMAGE)

## Guide

### Selecting and Downloading a Hypervisor

We will need a Hypervisor to install all of our tools and services.

I'll be using VirtualBox as it is a great free alternative for virtualization.

- [VirtualBox Download](https://www.virtualbox.org/)

## Instructions to Install OPNsense on VirtualBox

1. Download the latest OPNsense ISO image from the [official website](https://opnsense.org).

2. Open VirtualBox and click "New" to create a new virtual machine.

3. Name your VM (e.g., "OPNsense Firewall"), select `BSD` as the type, and `FreeBSD (64-bit)` as the version.

4. Allocate at least 1GB of RAM (4GB recommended) to the virtual machine.

5. Create a virtual hard disk with at least 20GB of space.

6. Configure network adapters:
    - **Adapter 1**: Set as `Bridged Adapter` for WAN interface.
    - **Adapter 2**: Set as `Internal Network` for LAN interface.

7. Start the virtual machine and select the OPNsense ISO as the boot media.

8. When the OPNsense boot menu appears, select "Boot Multi User" and press `Enter`.

9. Log in with the username `installer` and password `opnsense`.

10. Select "Install (ZFS)" for the installation type.

11. Choose the virtual hard disk for installation and confirm.

12. Select "stripe" for the ZFS configuration.

13. Wait for the installation to complete, then remove the ISO and reboot the virtual machine.

14. After rebooting, OPNsense will start and display the console menu.

15. Use option 1 to assign interfaces:
    - `em0` for WAN
    - `em1` for LAN

16. Use option 2 to set the LAN IP address (default is `192.168.1.1`).

17. Access the OPNsense web interface from another device on the same network using the LAN IP address.

18. Log in to the web interface with the default credentials:
    - **Username**: `root`
    - **Password**: `opnsense`

19. Complete the initial setup wizard to configure basic settings.

> Remember to adjust network settings and allocate resources based on your specific needs and available system resources.



### Installing and Configuring the IDS - Security Onion

Security Onion is an open-source IDS, Security Monitoring, and Log Management solution.

- [Security Onion Download](https://securityonion.net/)

Follow these steps to install and configure Security Onion:

1. Open VirtualBox and create a new VM.
2. Browse and select the Security Onion ISO file.
3. Choose Linux in Guest Operating System and CentOS 7 64-bit.
4. Name the VM "SecurityOnion".
5. Specify disk size (minimum 200GB) and store it as a single file.
6. Customize hardware: Increase Processor to 2, change memory to 8GB, and add 2 Network Adapters (assign them to VMnet4 & VMnet5).

After creating the VM:

1. Boot the 'SecurityOnion' VM and follow the on-screen instructions.
2. Set up an administrator account and proceed with the standard installation.
3. Configure the management interface and monitor interface as per your network design.
4. Complete the installation and note the Web Interface IP address.

To access the Security Onion Interface from the Kali machine:

1. Run the command `sudo so-allow` on Security Onion.
2. Add the Kali machine IP address to allow access to the Web Interface.

### Configuring Kali Linux Machine

Kali Linux will be used as an attack machine to perform reconnaissance and exploitation against the Metasploitable 2 and pfSense firewall.

- [Kali Linux Download](https://www.kali.org/get-kali/)
- [Kali Linux Installation Guide on VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)

After installing Kali Linux as a new VM in VirtualBox, configure the network adapter and memory as required.

### Configuring Metasploitable 2 Machine

Metasploitable 2 is a vulnerable machine used as the target for attacks.

- [Metasploitable 2 Download](https://sourceforge.net/projects/metasploitable/)

Follow the installation guide to set up Metasploitable 2 on VirtualBox.

## Conclusion

This Network Security Lab will provide a hands-on environment to learn and practice network security configurations, attack methods, and defense strategies. As you proceed with each component, document the steps and configurations to create a comprehensive guide for future reference.
