# pfSense Installation Guide on VirtualBox

This guide provides step-by-step instructions for installing and configuring pfSense on VirtualBox to serve as a firewall for segmenting your private homelab network. In this configuration, pfSense will be accessible exclusively from your Kali Linux machine.

## Prerequisites

1. **VirtualBox:**
   - Ensure that VirtualBox is installed on your host machine. For installation instructions, refer to the [VirtualBox Installation Guide](https://github.com/cody-reese/SOC-homelab-write-up/blob/main/VirtualBox/installation.md).

2. **pfSense ISO File:**
   - Download the pfSense ISO file from the [pfSense download page](https://www.pfsense.org/download/).

   1. On the download page, select **ISO IPMI/Virtual Machines** from the dropdown menu and add it to your cart.
   
   ![Download Selection](https://github.com/user-attachments/assets/3c4554d8-cfef-4402-ba4f-ace37a4e0826)

   2. You will be prompted to log in or create an account to complete the download. The download box will appear as shown below:
   
   ![Download Box](https://github.com/user-attachments/assets/571bb4c2-0eb6-4d28-83c7-aa337aaa85ed)

   3. Create a directory on your filesystem for VM images and extract the contents of the zipped `netgate-installer.iso` file to this directory.

## Installation Steps

### 1. Setting Up pfSense in VirtualBox

- **Create a New Virtual Machine:**
  1. Open VirtualBox and click **New** to initiate the creation of a new virtual machine.
  2. Name the VM (e.g., "pfSense") and select **BSD** as the type and **FreeBSD (64-bit)** as the version.

   ![New VM](https://github.com/user-attachments/assets/171252b9-fc8c-41dd-8fce-bedbfb0ab19c)

- **Allocate Resources:**
  - **Base Memory:** Assign 2 GB of memory.
  - **Processors:** Use the default setting of 1 CPU.

  ![Resource Allocation](https://github.com/user-attachments/assets/07a9ccd8-5883-48a9-ac0b-716c0230b53e)

  - **Virtual Disk:** Allocate at least 20 GB of storage.

   ![Virtual Disk](https://github.com/user-attachments/assets/85d80936-e2ba-4ac6-8078-0f42f00091da)

  - **VM Summary**

   ![VM Summary](https://github.com/user-attachments/assets/56dfff1e-bb01-48d3-808e-a74d666f7500)


- **Configure Additional Network Adapters Via the Command Line:**
  - By default, VirtualBox allows configuring up to four network adapters via the GUI. To configure six network adapters, we will need to modify the VM configuration through the VirtualBox command line manager.
  - Follow these steps to add additional network adapters:
    1. **Shut down** the virtual machine if it is running.
    2. Open a command prompt or terminal window and run the following commands:


    ```shell
    VBoxManage modifyvm "pfSense" --nic1 nat
    VBoxManage modifyvm "pfSense" --nic2 intnet --intnet2 "KaliNet"
    VBoxManage modifyvm "pfSense" --nic3 intnet --intnet3 "SecurityOnionNet"
    VBoxManage modifyvm "pfSense" --nic4 intnet --intnet4 "DomainControllerNet"
    VBoxManage modifyvm "pfSense" --nic5 intnet --intnet5 "SplunkNet"
    VBoxManage modifyvm "pfSense" --nic6 intnet --intnet6 "HostPCNet"
    ```
    3. Restart VirtualBox and open your VM's settings to verify that the additional network adapters are now available for configuration. The adapters listed in the network section should look like this:
       
    ![image](https://github.com/user-attachments/assets/5ead69b6-def1-432b-90b4-6e4a8718488f)

**Now we are able to start the Virtual Machine and follow the on-screen instructions to complete the pfSense installation.**

### 2. Initial Configuration

- **Complete the Installation:**
  - Upon boot, pfSense will present the initial setup screen. Accept all default settings to proceed with the installation. pfSense will automatically configure itself and reboot.
    
    ![image](https://github.com/user-attachments/assets/a7182236-1889-4f79-979f-e27b0e07bd61)
    
  - Install pfSense
    
    ![image](https://github.com/user-attachments/assets/1143a323-f0de-4fe4-9fd1-1a30b0275203)
    
  - Select Guided Root-on-ZFS
    
    ![image](https://github.com/user-attachments/assets/30aaaeae-ebc0-45b8-a16b-3e043c70f0a0)
    
  - Proceed with installation
    
    ![image](https://github.com/user-attachments/assets/f1b3e1b9-22e3-4b00-89a7-b76eec55b1a9)
    
  - We are installing pfSense on a single disk so we will select stripe and continue
    
    ![image](https://github.com/user-attachments/assets/779ed61b-c827-4cec-b5ca-0fc9101ceb5c)

  - Press space to select ada0 and continue

    ![image](https://github.com/user-attachments/assets/c40ba4d0-5a66-4642-9d7c-3dd04c480645)

  - pfSense will confirm that you wish to reformat ada0. Press tab to select yes, then continue.

    ![image](https://github.com/user-attachments/assets/ff7de12b-4c0c-4e97-842e-10ea1e0dee53)

  - Before rebooting, we need to unmount the .iso file from VirtualBox, otherwise the installation process will be restarted when the machines reboots.

    ![Screenshot 2024-08-13 191649](https://github.com/user-attachments/assets/318e9231-3dc5-4bf3-a183-24c989ed5a9b)

  - Select the pfSense .iso file. VirtualBox with prompt you with a dialogue box pops up asking if you would like to eject the virtual disk select "Force Unmount"

  - Reboot

    ![image](https://github.com/user-attachments/assets/3c8e712a-86dd-42bf-8908-0e70db507c0f)


- **Console Configuration:**
  - After rebooting, the console menu will appear. Select option 1 to proceed.

    ![image](https://github.com/user-attachments/assets/042b7ff2-2293-4e7a-9285-b6d231bd5aef)

  - For VLAN configuration, respond with `n` when asked, “Should VLANs be set up now [y]?”.
  - Enter the network interfaces as follows: `em0`, `em1`, `em2`, `em3`, `em4`, and `em5` when prompted.

- **Proceed with Configuration:**
  - When prompted, “Do you want to proceed [y]?”, enter `y`.
  - Select option 2 to configure the LAN interface.

- **LAN Interface Setup:**
  - Configure the LAN interface with the following settings:
    - **IP Address:** `192.168.1.1` (used to access the pfSense WebGUI from the Kali machine).

## Verification

- **Access pfSense WebGUI:**
  - From your Kali Linux machine, access the pfSense WebGUI using the IP address: [http://192.168.1.1](http://192.168.1.1).

- **Test Network Segmentation:**
  - Verify that the network segments are properly isolated as per your configuration.

- **Verify Access Control:**
  - Ensure that pfSense is accessible only from the designated Kali Linux machine.

---

Congratulations! You have successfully set up pfSense on VirtualBox, and your firewall is configured to segment your homelab network.


