# Azure-Sentinel-honeypot-Lab
 In this lab by Josh Madakor, I set up Azure Sentinel (SIEM) and connected it to a live VM acting as a honeypot to observe real-time RDP brute force attacks from around the world. We’ll use a custom PowerShell script to find attackers' geolocations and plot them on the Azure Sentinel map.


Step 1: Creating the Virtual Machine in Azure

To set up the virtual machine (VM) that will serve as our honeypot, follow these steps:

1. Navigate to the Azure portal and search for virtual machines.
2. Click on "Create" to initiate the creation process.
3. Choose a suitable resource group name, such as "honeypot-lab", to logically group all resources related to this project.
4. Enter a name for the virtual machine, for example, "honeypot-vm", and select the appropriate region.
5. Keep the default settings for image and size, but ensure to specify a username and password for accessing the VM later.
6. Proceed to the next steps, leaving the settings as default unless specific customization is required.
7. Under "Networking", configure the network security group (NSG), which acts as a firewall. Click on "Advanced" to access more options.
![VMscreenshot](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/7aec5bb3-1abd-48fc-b86a-2f3bd59849b2)
![CreatingHoneyPot](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/882d4734-dc73-46e1-b7d2-49b80e2a46cb)


Step 2: Allowing All in Firewall Settings

To open our logging server to potential attacks for the purpose of this lab, we configure the firewall to allow all inbound traffic from the public internet. We start by creating a new inbound rule in the network security group (NSG) associated with our virtual machine. This rule will essentially allow any type of traffic to reach the VM. By setting the destination port to "*", specifying the action as "allow", and assigning a low priority, such as 100, we ensure that all traffic is permitted. While this configuration is not recommended for production environments, it serves the purpose of our lab, where we aim to make the virtual machine easily discoverable to simulate real-world attack scenarios. After creating the rule, we proceed with deploying the changes, making our honeypot accessible to potential attackers
![honeypotconfiguration](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/36b22935-7ecb-4d9e-82d5-33113e463b6d)



Step 3: Creating Log Analytics Workspace

To begin collecting and analyzing logs from our honeypot VM, we need to create a Log Analytics Workspace (LAW). This workspace serves as the central repository for storing and analyzing log data, including Windows event logs and the custom logs containing geographic information about potential attackers. By creating this workspace, we establish the foundation for our SIEM implementation and enable Microsoft Sentinel to visualize geodata on a map.

1. Navigate to the Azure portal and search "Log Analytics Workspace".
2. Choose the resource group that was created earlier and provide a name for the workspace, such as "la-honeypot-1".
3. Select the region and proceed to review and create the workspace.
4. After creating the workspace, enable the gathering of VM logs in Microsoft Defender For Cloud. Navigate to Environment, enable gathering logs from the VM into the Log Analytics Workspace, and configure the data collection settings to "All Events".
5. Connect the Log Analytics Workspace to the VM by selecting the workspace and navigating to the virtual machine tab. Click "Connect" to establish the connection.
![creatinglawanalytics](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/65e11617-35db-44bb-9aab-ba1bbff45b8d)
![enablinggatheringvmsinWS](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/11acc08a-4c43-41d7-85bf-a54cbe60d979)
![Connectingloganalyticstovm](https://github.com/JacobAragon1/Azure-Sentinel-honeypot-Lab/assets/72784819/14c81490-3043-4272-a670-43cdbe4586c2)



Step 4: Setting up Azure Sentinel

In this step, we configure Azure Sentinel, our SIEM tool, to visualize and analyze the attack data collected by our Log Analytics Workspace. Azure Sentinel provides advanced threat detection and response capabilities, allowing us to monitor and visualize security events effectively.

1. Navigate to the Azure portal and search for "Azure Sentinel".
2. Click on "Microsoft Sentinel" and then "Create".
3. Select the Log Analytics Workspace we created earlier (e.g., "la-honeypot-1").
4. Add Microsoft Sentinel to this workspace and let it initialize.
While Azure Sentinel is being set up, ensure the Log Analytics Workspace is connected to the VM.

Navigate back to the Log Analytics Workspace and ensure the connection to the VM is established.
Browse to the virtual machine, click on the public IP address, and log into the VM using Remote Desktop Protocol (RDP).






