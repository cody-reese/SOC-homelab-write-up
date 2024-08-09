# pfSense Installation Guide on VirtualBox

This guide will walk you through installing and configuring pfSense on VirtualBox to act as a firewall for segmenting your private homelab network. In this setup, pfSense will only be accessible from your Kali Linux machine.
Prerequisites

  1. VirtualBox: Ensure you have VirtualBox installed on your host machine. If you need installation instructions, refer to the VirtualBox Installation Guide.

  2. pfSense ISO File: Download the free pfSense ISO file from the pfSense download page. (https://www.pfsense.org/download/)
     
  4. The official download page will redirect you to the Netgate Installer webpage. Select ISO IPMI/Virtual Machines from the drop down menu then select add to cart.

![image](https://github.com/user-attachments/assets/3c4554d8-cfef-4402-ba4f-ace37a4e0826)


   4. Netgate will have you log in or create an account in order to complete the download process. The webpage will prompt you with a download box that will look something like this.

![image](https://github.com/user-attachments/assets/571bb4c2-0eb6-4d28-83c7-aa337aaa85ed)


   5. Create a folder in your filesystem for VM images and extract the contents of the zipped netgate-installer .iso file to that filepath.

## Installation Steps
1. Set Up pfSense in VirtualBox

    Create a New Virtual Machine:
        Open VirtualBox and click New to create a new virtual machine.
        Name the VM (e.g., "pfSense") and select BSD as the type and FreeBSD (64-bit) as the version.

    Allocate Resources:
        Memory: Increase the memory to 2 GB.
        Disk: Create a virtual hard disk with a size of 20 GB. Ensure that the “Split virtual disk into multiple files” option is selected.

    Configure the Virtual Machine:
        Go to Settings and configure the network adapters:
            Adapter 1: Set to NAT (for internet access).
            Adapter 2: Set to Internal Network.
            Adapter 3: Set to Internal Network.
            Adapter 4: Set to Internal Network.
            Adapter 5: Set to Internal Network.

    Attach the pfSense ISO:
        In the Settings menu, go to Storage and attach the downloaded pfSense ISO to the virtual CD/DVD drive.

    Start the Virtual Machine:
        Boot the VM and follow the on-screen instructions to install pfSense.

2. Initial Configuration

    Complete the Installation:
        The pfSense machine will start with the initial setup screen. Accept all default settings for the installation process. pfSense will configure itself and reboot.

    Console Configuration:
        After reboot, you will see a console menu. Enter option 1 to proceed.
        For VLAN setup, choose n when asked, “Should VLANS be set up now [y]?”.
        Enter the network interfaces as follows: em0, em1, em2, em3, em4, and em5 when prompted for each interface.

    Proceed with Configuration:
        When asked, “Do you want to proceed [y]?”, enter y.
        Enter option 2 to configure the LAN interface.

    LAN Interface Setup:
        Configure the LAN interface with the following settings:
            IP Address: 192.168.1.1 (used to access the pfSense WebGUI via the Kali machine).

## Verification

  Access pfSense WebGUI:
        From your Kali Linux machine, access the pfSense WebGUI using the IP address http://192.168.1.1.

  Test Network Segmentation:
        Ensure that the network segments are properly isolated according to your configuration.

  Verify Access Control:
        Confirm that pfSense is reachable only from the designated Kali Linux machine.

Congratulations! You have successfully set up pfSense on VirtualBox, and your firewall is configured to segment your homelab network.
