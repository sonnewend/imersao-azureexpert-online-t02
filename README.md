# Imersao Azure Expert Online

Hands-on Lab

## Day 1
## Exercise #01 - Azure Calculator (15 minutes)

1. In a browser, navigate to the [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) webpage.

2. Add the inventory data.

   ![Screenshot of the inventory data](/AllFiles/Images/IMG01.png)

**Important Notes**
- Development environment licensing will be purchased by Marketplace
- Include Reserved instances and Azure Hybrid Benefit in the Production Environment 
- Include VPN Gateway, Load Balancers, Public IP Address and 500 GB Outbound Data Transfer.

3. Scroll to the bottom of the Azure Pricing Calculator webpage to view total Estimated monthly cost.

4. Change the currency to Brazilian Real (R$), then select Export to download a copy of the estimate for offline viewing in Microsoft Excel (.xlsx) format.

## Lab #01 - Resource Groups (15 minutes)

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Search for and select **Resource groups**. 

1. On the **Resource groups** blade, click **+ Create** and create a resource group with the following settings:

    |Setting|Value|
    |---|---|
    |Subscription| the name of the Azure subscription you will use in this lab |
    |Resource Group| **RG-TAE-VMs**|
    |Region| East US 2 |
    |Tags| project: azureexpert |
    | | |

1. Repeat and create the Resources groups name "RG-TAE-Networks" and "RG-TAE-Storage".

1. Click **Review + Create** and then click **Create**.

1. Explore properties to Resource groups.

## Lab #02 - Virtual Networks (10 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Create**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | the name of a resource group **RG-TAE-Networks** |
    | Name | **VNET-TAE-Hub** |
    | Region | the name of any Azure region available in the subscription you will use in this lab |
    | IPv4 address space | **10.1.0.0/16** |
    | Subnet name | **Default** |
    | Subnet address range | **10.1.1.0/24** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. On the virtual network blade, click **Subnets** and then click **+ Subnet**. 

1. Create a subnet with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **FrontEnd** |
    | Address range (CIDR block) | **10.1.100.0/24** |
    | Network security group | **None** |
    | Route table | **None** |
    | | |

## Lab #03 - Virtual Machine (20 minutes)

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, click **+ Create**.

1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-VMs** |
    | Virtual machine name | **VMHUB01** |
    | Region | select same region the Resouce group | 
    | Availability options | **Availability sets** |
    | Availability set | **AS-VM** |
    | Image | **Windows Server 2019 Datacenter - Gen1** |
    | Azure Spot instance | **No** |
    | Size | **Standard_B2s** |
    | Username | **admaz** |
    | Password | **Azur3Exp3rt*** |
    | Public inbound ports | **RDP (3389) and HTTP (80)** |
    | Would you like to use an existing Windows Server license? | **No** |
    | | |

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | OS disk type | **Standard SSD** |
    | Enable Ultra Disk compatibility | **No** |

1. Click **OK** and, back on the **Networking** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Virtual Network | **VNET-TAE-HUB** |
    | Subnet | **FrontEnd** |
    | Public IP | **VMHUB01-PI** |
    | NIC network security group | **Basic** |
	| Inbound Ports | **RDP (3389)** and HTTP (80)|
    | Place this virtual machine behind an existing load balancing solution? | **No** |
    | | |

1. Click **Next: Management >** and, on the **Management** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Boot diagnostics | **Enable with custom storage account** |
    | Diagnostics storage account | create new |
    | Properties storage account | Name: **sataediag###**, Account kind: StorageV2, Performance: Standard, Replication: Locally-redundant-storage (LRS) |
    | Enable auto-shutdown | off    
    
1. Click **Next: Advanced >**, on the **Advanced** tab of the **Create a virtual machine** blade, review the available settings without modifying any of them, and click **Review + Create**.

1. On the **Review + Create** blade, click **Create**.

1. Connect Virtual machine.

1. On the virtual machine blade, click **Disks**, Under **Data disks** click **+ Create and attach a new disk**. 

1. Create a managed disk with the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Disk name | **VMHUB01-DataDisk01** |
    | Source type | **None** |
    | Account type | **Standard HD** |
    | Size | **50 GiB** |
    | | |

1. Connect Virtual machine and start disk.

1. Install the Web-Server feature in the virtual machine by running the following command in the **Administrator Windows PowerShell** command prompt. You can copy and paste this command.

   ```powershell
   Install-WindowsFeature -name Web-Server -IncludeManagementTools
   ```
1. Back in the portal, navigate back to the Overview blade of myVM and, use the Click to clipboard button to copy the public IP address, open a new browser tab, paste the public IP address into the URL text box, and press the Enter key to browse to it.

1. Explore properties to Virtual machines.

## Lab #04 - Configure Azure DNS for internal name resolution (15 minutes)

1. In the Azure portal, search for and select **Private DNS zones** and, on the **Private DNS zones** blade, click **+ Create**.

1. Create a private DNS zone with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **azureexpert.corp** |

1. Click Review and Create. Let validation occur, and hit Create again to submit your deployment.

    >**Note**: Wait for the private DNS zone to be created. This should take about 2 minutes.

1. Click **Go to resource** to open the **azureexpert.corp** DNS private zone blade.

1. On the **azureexpert.corp** private DNS zone blade, in the **Settings** section, click **Virtual network links**

1. Click **+ Add** to create a virtual network link with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Link name | **VNL-Hub** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Hub** |
    | Enable auto registration | enabled |

1. Click **OK**.

    >**Note:** Wait for the virtual network link to be created. This should take less than 1 minute.

1. On the **azureexpert.corp** private DNS zone blade, in the sidebar, click **Overview**

1. Verify that the DNS records for **VMHUB01** appear in the list of record sets as **Auto registered**.

    >**Note:** You might need to wait a few minutes and refresh the page if the record sets are not listed.

1. Switch to the RDP session to **VMHUB01**.

1. In the Terminal console, run the following to test internal name resolution of the **VMHUB01** DNS record set in the newly created private DNS zone:

   ```powershell
   nslookup vmhub01.azureexpert.corp
   ```

1. Verify that the output of the command includes the private IP address of **VMHub01**.

## Lab #05 - Network Security groups (15 minutes)

1. In the Azure portal, search for and select **Network security groups**, and, on the **Network security groups** blade, click **+ Create**.

1. Create a network security group with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **NSG-WEB** |
    | Region | the name of the Azure region where you deployed all other resources in this lab |

1. On the deployment blade, click **Go to resource** to open the **NSG-WEB** network security group blade. 

1. On the **NSG-WEB** network security group blade, in the **Settings** section, click **Inbound security rules**. 

1. Add an inbound rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Destination port ranges | **80,443** |
    | Protocol | **TCP** |
    | Action | **Allow** |
    | Priority | **200** |
    | Name | **Allow-Port_80-443** |

1. Go to the Azure portal to view your **Network security groups**. Search for and select Network security groups is created.

    >**Note**: If another Network security groups rule is already attached to the Network interface, it is necessary to remove it.

1. On the **NSG-WEB** network security group blade, in the **Settings** section, click **Network interfaces** and then click **+ Associate**.

1. Associate in the **VMHUB01** network security group with the **Network interface**.

    >**Note**: It may take up to 5 minutes for the rules from the newly created Network Security Group to be applied to the Network Interface Card.

1. In the menu bar of the network security group, under Settings, you can view the Inbound security rules, Outbound security rules, Network interfaces, and Subnets that the network security group is associated to.

1. Under **Support + troubleshooting**, you can view Effective security rules.

1. Navigate back to your computer.

1. In the Virtual Machine Connection window, start Windows PowerShell and, in the **Administrator: Windows PowerShell** window run the following to set connection test. 

   ```powershell
   Test-NetConnection -ComputerName PUBLICIPADDRESS-VMHUB01 -Port 80 -InformationLevel 'Detailed'
   ```
1. Examine the output of the command and verify that the connection was successful.

1. Within the computer, start Browser and navigate to **PUBLICIPADDPRESS-VM**.

1. Examine the navegate was successful.

## Lab #06 - Azure Storage Blobs (20 minutes)

1. In the Azure portal, search for and select **Storage accounts** and, on the **Storage accounts** blade, select **+ Create**.

1. On the **Basics** tab of the **Create storage account** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-Storage** |
    | Storage account name | any globally unique name between 3 and 24 in length consisting of letters and digits **sataeblob###** |
    | Location | the name of an Azure region where you can create an Azure Storage account  |
    | Performance | **Standard** |
    | Account kind | **StorageV2 (general purpose v2)** |
    | Replication | **Locally redundant storage (LRS)** |

1. Select **Next: Networking >**, on the **Networking** tab of the **Create storage account** blade, review the available options, accept the default option **Public endpoint (all networks}** and select **Next: Data protection >**.

1. On the **Data protection** tab of the **Create storage account** blade, review the available options, accept the defaults, select **Next: Advanced >**.

1. On the **Advanced** tab of the **Create storage account** blade, review the available options, accept the defaults, select **Review + Create**, wait for the validation process to complete and select **Create**.

1. On the Storage account blade, in the Blob service section, click Containers.

1. Select **Create Blob Container**, and use the empty text box to set the container name to **azureexpert**.

1. Select **azureexpert**, in the **azureexpert** pane, select **Upload**, and in the drop-down list, select **Upload Files**.

1. In the **Upload Files** window, select the ellipsis button next to the **Selected files** label, in the **Choose files to upload** window, select **files** select random files your computer, and select **Open**.

1. Back in the **Upload Files** window, select **Upload**

1. Within the Remote Desktop session to **Virtual machine**, in the Server Manager window, select **Local Server**, select the **On** link next to the **IE Enhanced Security Configuration** label, and, in the **IE Enhanced Security Configuration** dialog box, select both **Off** options.

1. Within the Remote Desktop session, start Browser and navigate to the download page of [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/)

1. Within the Remote Desktop session, download and install Azure Storage Explorer with the default settings. 

1. Navigate to the [Azure portal](https://portal.azure.com), and sign-in by providing credentials of the user account with the Owner role in the subscription you are using in this lab.

1. Navigate to the blade of the newly created storage account, select **Access keys** and review the settings of the target blade.

1. On the storage account blade, select **Shared access signature** and review the settings of the target blade.

1. On the resulting blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Allowed services | **Blob** |
    | Allowed resource types | **Service**, **Container** and **Object** |
    | Allowed permissions | **Read**, **List** |
    | Blob versioning permissions | disabled |
    | Start | 24 hours before the current time in your current time zone | 
    | End | 24 hours after the current time in your current time zone |
    | Allowed protocols | **HTTPS only** |
    | Signing key | **key1** |

1. Select **Generate SAS and connection string**.

1. Copy the value of **Blob service SAS URL** into Clipboard.

1. Within the Remote Desktop session, start Azure Storage Explorer. 

1. In the Azure Storage Explorer window, in the **Connect to Azure Storage** window, select **Use a shared access signature (SAS) URI** and select **Next**.

1. In the **Attach with SAS URI** window, in the **Display name** text box, type **sataeblob###**, in the **URI** text box, paste the value you copied into Clipboard, and select **Next**. 

    >**Note**: This should automatically populate the value of **Blob endpoint** text box.

1. In the **Connection Summary** window, select **Connect**. 

1. Select **Storage account" and "Blob containers", Open and download uploaded files

1. Leave the Azure Storage Explorer window open.

## Lab #07 - Azure Files (15 minutes)

1. In the Azure portal, search for and select **Storage accounts** and, on the **Storage accounts** blade and create a new Storage account **sataefiles**, in the **File service** section, click **File shares**.

1. Click **+ File share** and create a file share with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **fs-azureexpert** |
    | Quota | **1024** |
    | Tiers | hot |

1. Click the newly created file share and click **Connect**.

1. On the **Connect** blade, ensure that the **Windows** tab is selected, and click **Copy to clipboard**.

1. In the Azure portal, search for and select **Virtual machines**, and, in the list of virtual machines.

1. In the Virtual Machine Connection window, start Windows PowerShell and, in the **Administrator: Windows PowerShell** window run the following to set map share. 

1. Verify that the script completed successfully. 

1. Connect share and upload files.

## Lab #08 - Azure VNET Peering (30 minutes)

1. In the Azure portal, search for and select **Virtual networks**, and, on the **Virtual networks** blade, click **+ Createt**.

1. Create a virtual network with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource Group | the name of a resource group **RG-TAE-Spoke1** |
    | Name | **VNET-TAE-Spoke1** |
    | Region | west us 2 |
    | IPv4 address space | **10.10.0.0/16** |
    | Subnet name | **Default** |
    | Subnet address range | **10.10.1.0/24** |
     | | |
      
1. Accept the defaults and click **Review and Create**. Let validation occur, and hit **Create** again to submit your deployment.

1. Once the deployment completes browse for **Virtual Networks** in the portal search bar. Within **Virtual networks** blade, click on the newly created virtual network.

1. In the Azure portal, search for and select **Virtual machines**

1. On the **Virtual machines**, click **Create** and create a new Virtual machine, on the **VMSPOKE01**.

1. In the Azure portal, search for and select **Virtual networks**.

1. In the list of virtual networks, click **VNET-TAE-Spoke1**.

1. On the **VNET-TAE-Spoke1** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Specify the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | This virtual network: Peering link name | **To-Hub** |
    | This virtual network: Traffic to remote virtual network | **Allow (default)** |
    | This virtual network: Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |
    | Remote virtual network: Peering link name | **To-Spoke1** |    
    | Virtual network deployment model | **Resource manager** |
    | I know my resource ID | unselected |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Hub** |
    | Traffic to remote virtual network | **Allow (default)** |
    | Traffic forwarded from remote virtual network | **None** |
    | Virtual network gateway | **Use this virtual network's gateway** |

1. On the **VNET-TAE-HUB** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | This virtual network: Peering link name | **To-Spoke1** |
    | This virtual network: Traffic to remote virtual network | **Allow (default)** |
    | This virtual network: Traffic forwarded from remote virtual network | ****Allow (default)**** |
    | Virtual network gateway | **None*** |
    | Remote virtual network: Peering link name | **To-Hub** |    
    | Virtual network deployment model | **Resource manager** |
    | I know my resource ID | unselected |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **VNET-TAE-Spoke1** |
    | Traffic to remote virtual network | **Allow (default)** |
    | Traffic forwarded from remote virtual network | **Allow (default)** |
    | Virtual network gateway | **None** |

1. At the top of the Azure portal, enter the name of a **VMSPOKE1** that is in the running state, in the search box. When the name of the VM appears in the search results, select it.

1. Under Settings on the left, select **Networking**, and navigate to the network interface resource by selecting its name. View network interfaces.

1. On the left, select **Effective routes**. The effective routes for a network interface are shown.
    
1. In the Azure portal, search for and select **Virtual machines** on the Virtual Network Hub.

1. On the **Virtual machine** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

1. Within the Remote Desktop session to **Virtual machine**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **VMSPOKE01**.

   ```pwsh
   Test-NetConnection -ComputerName IPADDRESS-VMHUB01 -Port 3389 -InformationLevel 'Detailed'
   ```
    >**Note**: The test uses TCP 3389 since this is this port is allowed by default by operating system firewall. 

1. Examine the output of the command and verify that the connection was successful.

1. In the Windows PowerShell console window, run the following to test connectivity to **VMHUB01** 

   ```pwsh
   Test-NetConnection -ComputerName IPADDRESS-VMSPOKE01 -Port 3389 -InformationLevel 'Detailed'
   ```
1. Examine the output of the command and verify that the connection was successful.

## Project #01 - Hub-spoke Archicture (60 minutes)

Implement a Hub-spoke topology

   ![Screenshot of the Hub-spoke](/AllFiles/Images/Hub-Spoke.png)

**Important Notes**
- Three Virtual Networks;
- Virtual machines on the Spoke2 network running Ubuntu server;
- VNET Peering connection Hub to spoke and vice verse
- VNET Peering connections to allow forwarded;
- Azure Firewall on the Hub network;
- Azure Bastion on the Hub network;
- Custom Route tables to address prefix "Address space Spoke 1 and Spoke 2" and next hop type to virtual applicance **IP Azure Firewall**;
- Network rule Azure Firewall all traffic.

References: [Hub-spoke network topology](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)

1. End of day 1.

## Day 2
## Lab #01 - Azure Load Balancer (30 minutes)

1. Sign in to the [Azure portal](http://portal.azure.com).

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, click **+ Create**.

1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab |
    | Resource group | the name of a new resource group **RG-Network** |
    | Virtual machine name | **VMTAEWEB01** and **VMTAEWEB02** |
    | Region | select one of the regions that support availability zones and where you can provision Azure virtual machines | 
    | Availability options | **Availability zone** |
    | Availability zone | **1** and **2** |
    | Image | **Ubuntu Server 18.04 LTS - Gen1** |
    | Azure Spot instance | **No** |
    | Size | **Standard B1ms** |
    | Username | **admaz** |
    | Password | **Azur3Exp3rt*** |
    | Public inbound ports | **SSH (22)** |
    | Would you like to use an existing Windows Server license? | **No** |

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | OS disk type | **Standard SSD** |
    | Enable Ultra Disk compatibility | **No** |

1. Click **OK** and, back on the **Networking** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Virtual Network | the name of a virtual network **VNET-TAE-Hub** |
    | Subnet | **Default** |
    | Public IP | **VMTAEWEB01-PI** and **VMTAEWEB02-PI** |
    | NIC network security group | **None** |
    | Accelerated networking | **Off** |
    | Place this virtual machine behind an existing load balancing solution? | **No** |

1. Click **Next: Management >** and, on the **Management** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Boot diagnostics | **Enable with custom storage account** |
    | Diagnostics storage account | create new |
    | Properties storage account | Name: **sataediag####**, Account kind: StorageV2, Performance: Standard, Replication: Locally-redundant-storage (LRS) |
    | Enable auto-shutdown | off |   

1. Click **Next: Advanced >**, on the **Advanced** tab of the **Custom data and cloud init** blade, add script for install NGINX Web Server.

1. Copy and paste script, on the Create cloud-init config, [click here](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-load-balancer#create-virtual-machines)

1. Click **Review + Create**.

1. On the **Review + Create** blade, click **Create**.

1. Repeat a new Virtual Machine **VMTAEWEB02**

1. Check two Virtual machines create successful.

1. Connect the Virtual machines.

**Note**
Test open Browser to IP Address the Virtual machines.

1. In the Azure portal, search and select **Load balancers** and, on the **Load balancers** blade, click **+ Create**.

1. Create a load balancer with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | **RG-TAE-Networks** |
    | Name | **ALBTAEWEB** |
    | Region| name of the Azure region into which you deployed all other resources in this lab |
    | Type | **Public** |
    | SKU | **Standard** |
    | Public IP address | **Create new** |
    | Public IP address name | **ALBTAEWEB-PI** |
    | Availability zone | **Zone-redundant** |
    | Add a public IPv6 address | **No** |

    > **Note**: Wait for the Azure load balancer to be provisioned. This should take about 2 minutes. 

1. On the deployment blade, click **Go to resource**.

1. On the **ALBTAEWEB** load balancer blade, click **Backend pools** and click **+ Add**.

1. Add a backend pool with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **BP-WEB** |
    | Virtual network | **VNET-TAE-Hub** |
    | IP version | **IPv4** |
    | Virtual machine | **VMTAEWEB01** | 
    | Virtual machine IP address | associate IP address |
    | Virtual machine | **VMTAEWEB02** |
    | Virtual machine IP address | associate IP address |

1. Wait for the backend pool to be created, click **Health probes**, and then click **+ Add**.

1. Add a health probe with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **HP-WEB** |
    | Protocol | **TCP** |
    | Port | **80** |
    | Interval | **5** |
    | Unhealthy threshold | **2** |

1. Wait for the health probe to be created, click **Load balancing rules**, and then click **+ Add**.

1. Add a load balancing rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **LBR-WEB** |
    | IP Version | **IPv4** |
    | Protocol | **TCP** |
    | Port | **80** |
    | Backend port | **80** |
    | Backend pool | **BP-WEB** |
    | Health probe | **HP-WEB** |
    | Session persistence | **None** |
    | Idle timeout (minutes) | **4** |
    | TCP reset | **Disabled** |
    | Floating IP (direct server return) | **Disabled** |
    | Use outbound rules to provide backend pool members access to the internet. | **Enabled** |

1. Wait for the load balancing rule to be created, click **Overview**, and note the value of the **Public IP address**.

1. In the Azure portal, search for and select **Network security groups**, and, on the **Network security groups** blade, click **+ Add**.

1. Create a network security group with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | **RG-TAE-Networks** |
    | Name | **NSG-TAE-ALB-WEB** |
    | Region | the name of the Azure region where you deployed all other resources in this lab |

1. On the deployment blade, click **Go to resource** to open the **NSG-ALB-WEB** network security group blade. 

1. On the **NSG-TAE-ALB-WEB** network security group blade, in the **Settings** section, click **Inbound security rules**. 

1. Add an inbound rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Destination port ranges | **80** |
    | Protocol | **TCP** |
    | Action | **Allow** |
    | Priority | **100** |
    | Name | **Allow-Port_80** |

1. On the **NSG-ALB-WEB** network security group blade, in the **Settings** section, click **Network interfaces** and then click **+ Associate**.

1. Associate the **NSG-TAE-ALB-WEB** network security group with the Network interfaces **VMTAEWEB01 and VMTAEWEB02**.

    >**Note**: It may take up to 5 minutes for the rules from the newly created Network Security Group to be applied to the Network Interface Card.

1. Go to the Azure portal to view your **Network security groups**. Search for and select Network security groups.

1. Select the name of your Network security group.

1. Start browser window and navigate to the IP address you identified in the previous step.

1. Verify that the browser window displays the message **Static page and servername**.

1. In Virtual machines, select **VMTAEWEB01 or VMTAEWEB02** and click **Stop**. 

1. Open another browser window but this time by using InPrivate mode and verify whether the target vm changes (as indicated by the message).

## Lab #02 - Azure Virtual Machine Scale Sets (30 minutes)

1. Log in to the **Azure portal** at https://portal.azure.com.

1. Type **Scale set** in the search box. In the results, under **Marketplace**, select **Virtual machine scale sets**. Select **Create** on the **Virtual machine scale sets** page, which will open the **Create a virtual machine scale set** page.

1. In the **Basics** tab, under **Project details**, make sure the correct subscription is selected and then choose to **Create new** resource group. Type **RG-TAE-VMSS** for the name and then select **OK**.

1. Type **VMSSTAEWEB** as the name for your scale set.

1. In **Region**, select a region that is close to your area.

1. Leave the default value of **Uniform** for **Orchestration mode**.

1. Select a marketplace image **Windows Server 2019 Datacenter - Gen1** for **Image**.

1. Enter your desired username, and password.

1. Select **Next** to move the the other pages. 

1. Leave the defaults for the **Instance** and **Disks** pages.

1. On the **Networking** page, under **Load balancing**, select **Yes** to put the scale set instances behind a load balancer. 

1. In **Load balancing options**, select **Azure load balancer**.

1. In **Select a load balancer**, create a new Azure Load Balancer **ALBTAEVMSSWEB**.

1. For **Select a backend pool**, select **Create new**, type **BP-VMSSWEB**, then select **Create**.

1. On the **Scaling** page, under **Instance**, select **2** and **Scaling policy**, select **Manual**. 

1. On the **Management** page, under **Monitoring**, select **Enable with custom storage account**.

1. When you are done, select **Review + create**. 

1. After it passes validation, select **Create** to deploy the scale set.

    > **Note**: You might need to wait a few minutes.

1. In **Virtual Machine Scale Sets**, select **Instances** and connect on Virtual machines.

1. Back in the portal, navigate back to the Overview blade of **Virtual Machine Scale Sets** and, use the Click to clipboard button to copy the public IP address.

1. In a **Browser**, navigate to the Public IP Address the VMSS.
    
    > **Note**: You didn't notice that you won't connect because you don't have a Web Server.

1. Sign in to the [**Azure portal**](http://portal.azure.com) and open Azure Cloud Shell.

1. In the Azure portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    >**Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and click **Create storage**. 

1. In Cloud Shell, start the code editor and run the following command **code Add-CustomExtension-VMSS.ps1**.

1. Add the following text to the script file:

  ```powershell
    # Set Script 
$customConfig = @{ 
  "fileUris" = (,"https://raw.githubusercontent.com/maiaacademy/azureexpert/master/Mod06-AllFiles/VMSS-Install-IIS_v1.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File VMSS-Install-IIS_v1.ps1" 
} 

    # Set VMSS variables
$rgname = "rg-tae-vmss"
$vmssname = "vmsstaeweb"
 
    # Get VMSS object 
$vmss = Get-AzVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
 
    # Add VMSS extension 
$vmss = Add-AzVmssExtension -Name "CustomScript" -VirtualMachineScaleSet $vmss -Publisher "Microsoft.Compute" -Type "CustomScriptExtension" -TypeHandlerVersion "1.9" -Setting $customConfig

    # Update VMSS 
Update-AzVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss
   ```

1. Press Ctrl+S to save the file. Then press Ctrl+Q to close the code editor.

1. Run the following command **./Add-CustomExtension-VMSS.ps1**.

1. In **VMSSTAESWEB**, select **Extensions** and check a new extension.

1. Select **Instances** in **VMSSTAEWEB**, click **Upgrade** for all instances.

    > **Note**: You might need to wait a few minutes.

1. Test the Public IP address in Browser.

    > **Note**: You will need to open port 80 (HTTP) internet access in Network Security Groups in Virtual Machine Scale Sets "VMSSTAEWEB"

1. In the Azure portal, navigate to the **Virtual Machine Scale Sets** entry, and on the **VMSSWEB** blade, select **Scaling**. 

1. On the **Scaling** blade, select the **Custom autoscale** option.

1. In the **Custom autoscale** section, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Scaling mode | **Scale based on a metric** |
    | Instance limits Minimum | **1** |
    | Instance limits Maximum | **3** |
    | Instance limits Default | **1** |

1. Select **+ Add a rule**.

1. On the **Scale rule** blade, specify the following settings and select **Add** (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Time aggregation | **Maximum** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Network In Total** |
    | VMName Operator | **=** |
    | Operator | **Greater than** |
    | Metric threshold to trigger scale action | **10** |
    | Duration (in minutes) | **1** |
    | Time grain statistics | **Maximum** |
    | Operation | **Increase count by** |
    | Instance count | **1** |
    | Cool down (minutes) | **5** |

1. Back on the **Scaling** blade, select **+ Add a rule**.

1. On the **Scale rule** blade, specify the following settings and select **Add** (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Time aggregation | **Average** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Network In Total** |
    | VMName Operator | **=** |
    | Operator | **Less than** |
    | Metric threshold to trigger scale action | **1** |
    | Duration (in minutes) | **1** |
    | Time grain statistics | **Minimum** |
    | Operation | **Decrease count by** |
    | Instance count | **1** |
    | Cool down (minutes) | **5** |

1. Back on the **Scaling** blade, select **Save**.

1. In the Azure portal, start a new **Bash** session in the Cloud Shell pane. 

1. From the Cloud Shell pane, run the following to trigger autoscaling of the Azure VM Scale Set instances in the backend pool of the Azure Application Gateway (replace the `<lb_IP_address>` placeholder with the IP address of the front end of the gateway you identified earlier):

   ```Bash
   for (( ; ; )); do curl -s <lb_IP_address>?[1-10]; done
   ```
1. In the Azure portal, on the **VMSSWEB** blade, review the **CPU (average)** chart and verify that the CPU utilization of the Application Gateway increased sufficiently to trigger scaling out.

    > **Note**: You might need to wait a few minutes.

## Lab #03 - Azure App Service (15 minutes)

1. Sign in to the [**Azure portal**](http://portal.azure.com).

1. In the Azure portal, search for and select **App services**, and, on the **App Services** blade, click **+ Add**.

1. On the **Basics** tab of the **Web App** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | ---|
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-App** |
    | Web app name | any globally unique name |
    | Publish | **Code** |
    | Runtime stack | **PHP 7.3** |
    | Operating system | **Windows** |
    | Region | the name of an Azure region where you can provision Azure web apps |
    | App service plan | accept the default configuration |

1. Click **Next : Monitoring >**, on the **Monitoring** tab of the **Web App** blade, set the **Enable Application Insights** switch to **No**, click **Review + create**, and then click **Create**. 

    >**Note**: Typically, you would want to enable **Application Insights**, however, its functionality is not used in this lab.

    >**Note**: Wait until the web app is created before you proceed to the next task. This should take about a minute. 

1. On the deployment blade, click **Go to resource**.

1. On the blade of the newly deployed web app, click the **URL** link to display the default web page in a new browser tab.

1. Close the new browser tab and, back in the Azure portal, in the **Deployment** section of the web app blade, click **Deployment slots**. 

    >**Note**: The web app, at this point, has a single deployment slot labeled **PRODUCTION**. 

1. Click **+ Add slot**, and add a new slot with the following settings: 

    | Setting | Value |
    | --- | ---|
    | Name | **staging** |
    | Clone settings from | **Do not clone settings**|

1. Back on the **Deployment slots** blade of the web app, click the entry representing the newly created staging slot. 

    >**Note**: This will open the blade displaying the properties of the staging slot. 

1. Review the staging slot blade and note that its URL differs from the one assigned to the production slot.

1. On the staging deployment slot blade, in the **Deployment** section, click **Deployment Center**.

    >**Note:** Make sure you are on the staging slot blade (rather than the production slot).

1. In the **Continuous Deployment (CI/CD)** section, select **Local Git**, and then click **Save**.

1. Copy the resulting **Git Clone Url** to Notepad.

    >**Note:** You will need the Git Clone Url value in the next task of this lab.

1. Click **Local Git/FTPS credentials** toolbar icon to display **Deployment Credentials** pane. 

1. Click **User scope**.

1. Complete the required information, and then click **Save Credentials**. 

    | Setting | Value |
    | --- | ---|
    | User name | any unique name (must not contain `@` character) |
    | Password | any password that satisfies complexity requirements |
    
    >**Note:** The password must be at least eight characters long, with two of the following three elements: letters, numbers, and non-alphanumeric characters.

    >**Note:** You will need these credentials in the next task of this lab.

1. In the Azure portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    >**Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and click **Create storage**. 

1. From the Cloud Shell pane, run the following to clone the remote repository containing the code for the web app.

   ```pwsh
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```
 
1. From the Cloud Shell pane, run the following to set the current location to the newly created clone of the local repository containing the sample web app code.

   ```
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. From the Cloud Shell pane, run the following to add the remote git (make sure to replace the `[deployment_user_name]` and `[git_clone_url]` placeholders with the value of the **Deployment Credentials** user name and **Git Clone Url**, respectively, which you identified in previous task):

   ```
   git remote add [deployment_user_name] [git_clone_url]
   ```

    >**Note**: The value following `git remote add` does not have to match the **Deployment Credentials** user name, but has to be unique

1. From the Cloud Shell pane, run the following to push the sample web app code from the local repository to the Azure web app staging deployment slot (make sure to replace the `[deployment_user_name]` placeholder with the value of the **Deployment Credentials** user name, which you identified in previous task):
   ```
   git push [deployment_user_name] master
   ```

1. If prompted to authenticate, type the `[deployment_user_name]` and the corresponding password (which you set in the previous task).

1. Close the Cloud Shell pane.

1. On the staging slot blade, click **Overview** and then click the **URL** link to display the default web page in a new browser tab.

1. Verify that the browser page displays the **Hello World!** message and close the new tab.

In this task, you will swap the staging slot with the production slot

1. Navigate back to the blade displaying the production slot of the web app.

1. In the **Deployment** section, click **Deployment slots** and then, click **Swap** toolbar icon.

1. On the **Swap** blade, review the default settings and click **Swap**. 

1. Click **Overview** on the production slot blade of the web app and then click the **URL** link to display the web site home page in a new browser tab.

1. Verify the default web page has been replaced with the **Hello World!** page.

## Lab #04 - Azure Container Instances (15 minutes)

1. Sign in to the [Azure portal](https://portal.azure.com).

1. In the Azure portal, search for locate **Container instances** and then, on the **Container instances** blade, click **+ Add**. 

1. On the **Basics** tab of the **Create container instance** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-Container** |
    | Container name | **acitaeweb** |
    | Region | the name of a region where you can provision Azure container instances |
    | Image Source | **Quickstart images** |
    | Image | **microsoft/aci-helloworld (Linux)** |

1. Click **Next: Networking >** and, on the **Networking** tab of the **Create container instance** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | DNS name label | any valid, globally unique DNS host name |
	
    >**Note**: Your container will be publicly reachable at dns-name-label.region.azurecontainer.io. If you receive a **DNS name label not available** error message, specify a different value.

1. Click **Next: Advanced >**, review the settings on the **Advanced** tab of the **Create container instance** blade without making any changes, click **Review + Create**, and then click **Create**. 

    >**Note**: Wait for the deployment to complete. This should take about 3 minutes.

    >**Note**: While you wait, you may be interested in viewing the [code behind the sample application](https://github.com/Azure-Samples/aci-helloworld). To view it, browse the \app folder. 

1. On the deployment blade, click the **Go to resource** link.

1. On the **Overview** blade of the container instance, verify that **Status** is reported as **Running**. 

1. Copy the value of the container instance **FQDN**, open a new browser tab, and navigate to the corresponding URL.

1. Verify that the **Welcome to Azure Container Instance** page is displayed.

1. Close the new browser tab, back in the Azure portal, in the **Settings** section of the container instance blade, click **Containers**, and then click **Logs**. 

1. Verify that you see the log entries representing the HTTP GET request generated by displaying the application in the browser.

## Lab #05 - Azure AD (15 minutes)

1. In the Azure portal, search for and select **Azure Active Directory**.

1. Click **+ Create a tenant** and specify the following setting:

    | Setting | Value |
    | --- | --- |
    | Directory type | **Azure Active Directory** |
    | Organization name | **Azure Expert** |
    | Initial domain name | any valid DNS name consisting of lower case letters and digits and starting with a letter | 
    | Country/Region | **United States** |

   > **Note**: The green check mark in the **Initial domain name** text box will indicate that the domain name you typed in is valid and unique.

1. Click **Review + create** and then click **Create**.

1. Display the blade of the newly created Azure AD tenant by using the **Click here to navigate to your new directory: Contoso Lab** link or the **Directory + Subscription** button (directly to the right of the Cloud Shell button) in the Azure portal toolbar.

 Navigate back to the **Users - All users** blade, and then click **+ New user**.

1. Create a new user with the following settings (leave others with their defaults):

    | Setting | Value |
    | --- | --- |
    | User name | **AzureExpert** |
    | Name | **Azure Expert** |
    | Let me create the password | enabled |
    | Initial password | **Azur3Exp3rt*** |
    | Usage location | **United States** |
    | Job title | **Azure Expert** |
    | Department | **IT** |

    >**Note**: **Copy to clipboard** the full **User Principal Name** (user name plus domain). You will need it later in this task.

1. In the list of users, click the newly created user account to display its blade.

1. Review the options available in the **Manage** section and note that you can identify the Azure AD roles assigned to the user account as well as the user account's permissions to Azure resources.

1. In the **Manage** section, click **Assigned roles**, then click **+ Add assignment** button and assign the **User administrator** role to user.

    >**Note**: You also have the option of assigning Azure AD roles when provisioning a new user.

1. Open an **InPrivate** browser window and sign in to the [Azure portal](https://portal.azure.com) using the newly created user account. When prompted to update the password, change the password for the user.

    >**Note**: Rather than typing the user name (including the domain name), you can paste the content of Clipboard.

1. In the **InPrivate** browser window, in the Azure portal, search for and select **Azure Active Directory**.

1. In the **InPrivate** browser window, on the Azure AD blade, scroll down to the **Manage** section, click **User settings**, and note that you do not have permissions to modify any configuration options.

1. In the Azure portal, navigate back to the Azure AD tenant blade and click **Groups**.

1. Use the **+ New group** button to create a new group with the following settings:

    | Setting | Value |
    | --- | --- |
    | Group type | **Security** |
    | Group name | **GS-AzureExpert** |
    | Group description | **Azure Expert Team** |
    | Membership type | **Assigned** |

1. Click **No members selected**.

1. From the **Add members** blade, search and select the **AzureExpert** appears in the list of user members.

## Lab #06 - Azure RBAC (10 minutes)

1. In the Azure portal, search for and select **Azure Active Directory**, on the Azure Active Directory blade, click **Users**, and then click **+ New user**.

1. Create a new user with the following settings (leave others with their defaults):

    | Setting | Value |
    | --- | --- |
    | User name | **azure-support**|
    | Name | **Azure-Support**|
    | Let me create the password | enabled |
    | Initial password | **Pa55w.rd124** |

    >**Note**: **Copy to clipboard** the full **User name**. You will need it later in this lab.

1. In the Azure portal, navigate back to the **Subscritions** and display its **details**.

1. Click **Access control (IAM)**, click **+ Add** followed by **Role assignment**, and assign the **Support Request Contributor (Custom)** role to the newly created user account.

1. Open an **InPrivate** browser window and sign in to the [Azure portal](https://portal.azure.com) using the newly created user account. When prompted to update the password, change the password for the user.

    >**Note**: Rather than typing the user name, you can paste the content of Clipboard.

1. In the **InPrivate** browser window, in the Azure portal, search and select **Resource groups** to verify that the user can see all resource groups.

1. In the **InPrivate** browser window, in the Azure portal, search and select **All resources** to verify that the user cannot see any resources.

1. In the **InPrivate** browser window, in the Azure portal, search and select **Help + support** and then click **+ New support request**. 

1. In the **InPrivate** browser window, on the **Basic** tab of the **Help + support - New support request** blade, select the **Service and subscription limits (quotas)** issue type and note that the subscription you are using in this lab is listed in the **Subscription** drop-down list.

    >**Note**: The presence of the subscription you are using in this lab in the **Subscription** drop-down list indicates that the account you are using has the permissions required to create the subscription-specific support request.

    >**Note**: If you do not see the **Service and subscription limits (quotas)** option, sign out from the Azure portal and sign in back.

1. Do not continue with creating the support request. Instead, sign out as the user from the Azure portal and close the InPrivate browser window.

## Lab #07 - Azure Backup (15 minutes)

1. In the Azure portal, search for and select **Recovery Services vaults** and, on the **Recovery Services vaults** blade, click **+ Create**.

1. On the **Create Recovery Services vault** blade, specify the following settings:

    | Settings | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-Backup** |
    | Name | **RSV-TAE-Backup** |
    | Region | the name of a region where you deployed the two virtual machines in the previous task |

    >**Note**: Make sure that you specify the same region into which you deployed virtual machines.

1. Click **Review + Create** and then click **Create**.

    >**Note**: Wait for the deployment to complete. The deployment should take less than 1 minute.

1. When the deployment is completed, click **Go to Resource**. 

1. On the **RSV-TAE-Backup** Recovery Services vault blade, in the **Settings** section, click **Properties**.

1. On the **RSV-TAE-Backup - Properties** blade, click the **Update** link under **Backup Configuration** label.

1. On the **Backup Configuration** blade, note that you can set the **Storage replication type** to either **Locally-redundant** or **Geo-redundant**. Leave the default setting of **Geo-redundant** in place and close the blade.

    >**Note**: This setting can be configured only if there are no existing backup items.

1. Back on the **RSV-TAE-Backup - Properties** blade, click the **Update** link under **Security Settings** label. 

1. On the **Security Settings** blade, note that **Soft Delete (For Azure Virtual Machines)** is **Enabled**.

1. Close the **Security Settings** blade and, back on the **RSV-TAE-Backup** Recovery Services vault blade, click **Overview**.

1. On the **RSV-TAE-Backup** Recovery Services vault blade, click **+ Backup**.

1. On the **Backup Goal** blade, specify the folowing settings:

    | Settings | Value |
    | --- | --- |
    | Where is your workload running? | **Azure** |
    | What do you want to backup? | **Virtual machine** |

1. On the **Backup Goal** blade, click **Backup**.

1. On the **Backup policy**, review the **DefaultPolicy** settings and select **Create a new policy**.

1. Define a new backup policy with the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Policy name | **BP-VMS** |
    | Frequency | **Daily** |
    | Time | **22:00 PM** |
    | Timezone | the name of your local time zone |
    | Retain instant recovery snapshot(s) for | **2** Days(s) |

1. Click **OK** to create the policy and then, in the **Virtual Machines** section, select **Add**.

1. On the **Select virtual machines** blade, select **VMTAEHUB01**, click **OK**, and, back on the **Backup** blade, click **Enable backup**.

    >**Note**: Wait for the backup to be enabled. This should take about 2 minutes. 

1. Navigate back to the **RSV-TAE-Backup** Recovery Services vault blade, in the **Protected items** section, click **Backup items**, and then click the **Azure virtual machines** entry.

1. On the **Backup Items (Azure Virtual Machine)** blade of **VMTAEHUB01**, review the values of the **Backup Pre-Check** and **Last Backup Status** entries, and click the **VMTAEHUB01** entry.

1. On the **VMTAEHUB01** Backup Item blade, click **Backup now**, accept the default value in the **Retain Backup Till** drop-down list, and click **OK**.

 >**Note**: Do not wait for the backup to complete but instead proceed.

## Lab #05 - Azure Monitor (15 minutes)

1. From the Cloud Shell pane, run the following to register the Microsoft.Insights resource providers.

   ```pwsh
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
    ```

1. In the Azure portal, search for and select **Log Analytics workspaces** and, on the **Log Analytics workspaces** blade, click **+ Add**.

1. On the **Basics** tab of the **Create Log Analytics workspace** blade, the following settings, click **Review + Create** and then click **Create**:

    | Settings | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TA-Monitor** |
    | Log Analytics Workspace | any unique name **lawtaemon** |    
    | Region | the name of the Azure region into which you deployed the virtual machine in the previous task |

    >**Note**: Wait for the deployment to complete. The deployment should take about 1 minute.

1. In the Azure portal, search for and select **Virtual machines**, and on the **Virtual machines** blade, click **VMTAEHUB01**.

1. On the **VMTAEHUB01** blade, in the **Monitoring** section, click **Metrics**.

1. On the **VMTAEHUB01 | Metrics** blade, on the default chart, note that the only available **Metrics Namespace** is **Virtual Machine Host**.

    >**Note**: This is expected, since no guest-level diagnostic settings have been configured yet. You do have, however, the option of enabling guest memory metrics directly from the **Metrics Namespace** drop down-list. You will enable it later in this exercise.

1. In the **Metric** drop-down list, review the list of available metrics.

    >**Note**: The list includes a range of CPU, disk, and network-related metrics that can be collected from the virtual machine host, without having access into guest-level metrics.

1. In the **Metric** drop-down list, select **Percentage CPU**, in the **Aggregation** drop-down list, select **Avg**, and review the resulting chart. 

1. On the **VMTAEHUB01** blade, in the **Monitoring** section, click **Diagnostic settings**.

1. On the **Overview** tab of the **VMTAEHUB01 | Diagnostic settings** blade, click **Enable guest-level monitoring**.

    >**Note**: Wait for the operation to take effect. This might take about 3 minutes.

1. Switch to the **Performance counters** tab of the **VMTAEHUB01 | Diagnostic settings** blade and review the available counters.

    >**Note**: By default, CPU, memory, disk, and network counters are enabled. You can switch to the **Custom** view for more detailed listing.

1. Switch to the **Logs** tab of the **VMTAEHUB01 | Diagnostic settings** blade and review the available event log collection options.

    >**Note**: By default, log collection includes critical, error, and warning entries from the Application Log and System log, as well as Audit failure entries from the Security log. Here as well you can switch to the **Custom** view for more detailed configuration settings.

1. On the **VMTAEHUB01** blade, in the **Monitoring** section, click **Logs** and then click **Enable**. 

1. On the **VMTAEHUB01 - Logs** blade, ensure that the Log Analytics workspace you created earlier in this lab is selected in the **Choose a Log Analytics Workspace** drop-down list and click **Enable**.

    >**Note**: Do not wait for the operation to complete but instead proceed to the next step. The operation might take about 5 minutes.

1. On the **VMTAEHUB01 | Logs** blade, in the **Monitoring** section, click **Metrics**.

1. On the **VMTAEHUB01 | Metrics** blade, on the default chart, note that at this point, the **Metrics Namespace** drop-down list, in addition to the **Virtual Machine Host** entry includes also the **Guest (classic)** entry.

    >**Note**: This is expected, since you enabled guest-level diagnostic settings. You also have the option to **Enable new guest memory metrics**.

1. In the **Metrics Namespace** drop-down list, select  the **Guest (classic)** entry.

1. In the **Metric** drop-down list, review the list of available metrics.

    >**Note**: The list includes additional guest-level metrics not available when relying on the host-level monitoring only. 

1. In the **Metric** drop-down list, select **Memory\Available Bytes**, in the **Aggregation** drop-down list, select **Max**, and review the resulting chart. 
   
1. In the Azure portal, search for and select **Monitor** and, on the **Monitor | Overview** blade, click **Metrics**.

1. On the **Select a scope** blade, on the **Browse** tab, navigate to the Virtual machine.

1. In the **Metric** drop-down list, select **Percentage CPU**, in the **Aggregation** drop-down list, select **Avg**, and review the resulting chart.

In the Azure portal, navigate back to the **Monitor** blade, click **Logs**. 

    >**Note**: You might need to click **Get Started** if this is the first time you access Log Analytics.

1. If necessary, click **Select scope**, on the **Select a scope** blade, select the **Recent** tab, select **VMNAME**, and click **Apply**.

1. In the query window, paste the following query, click **Run**, and review the resulting chart:

   ```
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

1. Click **Queries** in the toolbar, on the **Queries** pane, locate the **Track VM availability** tile, click the **Run** command button in the tile, and review the results. 

1. On the **New Query 1** tab, select the **Tables** header, and review the list of tables in the **Virtual machines** section.

    >**Note**: The names of several tables correspond to the solutions you installed earlier in this lab.

1. Hover the mouse over the **VMTAEHUB01** entry and click the **Preview data** icon.  

1. If any data is available, in the **Update** pane, click **See in query editor**.

    >**Note**: You might need to wait a few minutes before the update data becomes available.

## Project #02 - Azure Kubernetes Service (60 minutes)

Kubernetes architecture.

   ![Screenshot of the Kubernetes archicture](/AllFiles/Images/IMG03.png)

#### Register the Microsoft Kubernetes resource providers.

1. Sign in to the [Azure portal](https://portal.azure.com).

1. In the Azure portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    >**Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and click **Create storage**. 

1. From the Cloud Shell pane, run the following to register the Microsoft.Kubernetes and Microsoft.KubernetesConfiguration resource providers.

   ```pwsh
   Register-AzResourceProvider -ProviderNamespace Microsoft.Kubernetes
   
   Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
   ```

1. Close the Cloud Shell pane.

#### Deploy an Azure Kubernetes Service cluster

1. In the Azure portal, search for locate **Kubernetes services** and then, on the **Kubernetes services** blade, click **+ Add**, and then click **+ Add Kubernetes cluster**. 

1. On the **Basics** tab of the **Create Kubernetes cluster** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of a new resource group **RG-TAE-AKS** |
    | Kubernetes cluster name | **akstaecl** |
    | Region | the name of a region where you can provision a Kubernetes cluster |
    | Kubernetes version | accept the default |
    | Node size | accept the default |
    | Node count | **1** |

1. Click **Next: Node Pools >** and, on the **Node Pools** tab of the **Create Kubernetes cluster** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Virtual nodes | **Disabled** |
    | VM scale sets | **Enabled** |
	
1. Click **Next: Authentication >** and, on the **Authentication** tab of the **Create Kubernetes cluster** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Service principal | accept the default |
    | Enable RBAC | **Yes** |

1. Click **Next: Networking >** and, on the **Networking** tab of the **Create Kubernetes cluster** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Network configuration | **kubenet** |
    | DNS name prefix | any valid, globally unique DNS host name |

1. Click **Next: Integration >**, on the **Integration** tab of the **Create Kubernetes cluster** blade, set **Container monitoring** to **Disabled**, click **Review + create** and then click **Create**. 

    >**Note**: In production scenarios, you would want to enable monitoring. Monitoring is disabled in this case since it is not covered in the lab. 

    >**Note**: Wait for the deployment to complete. This should take about 10 minutes.

#### Deploy pods into the Azure Kubernetes Service cluster

1. On the deployment blade, click the **Go to resource** link.

1. On the **akstaecl** Kubernetes service blade, in the **Settings** section, click **Node pools**.

1. On the **akstaecl - Node pools** blade, verify that the cluster consists of a single pool with one node.

1. In the Azure portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

1. Switch the **Azure Cloud Shell** to **Bash** (black background).

1. From the Cloud Shell pane, run the following to retrieve the credentials to access the AKS cluster:

    ```sh
    RESOURCE_GROUP='rg-tae-aks'

    AKS_CLUSTER='akstaecl'

    az aks get-credentials --resource-group $rgname--name $aksclname
    ``` 

1. From the **Cloud Shell** pane, run the following to verify connectivity to the AKS cluster:

    ```sh
    kubectl get nodes
    ```

1. In the **Cloud Shell** pane, review the output and verify that the one node which the cluster consists of at this point is reporting the **Ready** status. 

1. From the **Cloud Shell** pane, run the following to deploy the **nginx** image from the Docker Hub:

    ```sh
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **Note**: Make sure to use lower case letters when typing the name of the deployment (nginx-deployment)

1. From the **Cloud Shell** pane, run the following to verify that a Kubernetes pod has been created:

    ```sh
    kubectl get pods
    ```

1. From the **Cloud Shell** pane, run the following to identify the state of the deployment:

    ```sh
    kubectl get deployment
    ```

1. From the **Cloud Shell** pane, run the following to make the pod available from Internet:

    ```sh
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

1. From the **Cloud Shell** pane, run the following to identify whether a public IP address has been provisioned:

    ```sh
    kubectl get service
    ```

1. Re-run the command until the value in the **EXTERNAL-IP** column for the **nginx-deployment** entry changes from **\<pending\>** to a public IP address. Note the public IP address in the **EXTERNAL-IP** column for **nginx-deployment**.

1. Open a browser window and navigate to the IP address you obtained in the previous step. Verify that the browser page displays the **Welcome to nginx!** message.

#### Scale containerized workloads in the Azure Kubernetes service cluster

1. From the **Cloud Shell** pane, run the following to scale the deployment by increasing of the number of pods to 2:

    ```sh
    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

1. From the **Cloud Shell** pane, run the following to verify the outcome of scaling the deployment:

    ```sh
    kubectl get pods
    ```

    > **Note**: Review the output of the command and verify that the number of pods increased to 2.

1. From the **Cloud Shell** pane, run the following to scale out the cluster by increasing the number of nodes to 2:

    ```sh
    az aks scale --resource-group $rgname --name $aksclname --node-count 2
    ```

    > **Note**: Wait for the provisioning of the additional node to complete. This might take about 3 minutes. If it fails, rerun the `az aks scale` command.

1. From the **Cloud Shell** pane, run the following to verify the outcome of scaling the cluster:

    ```sh
    kubectl get nodes
    ```

    > **Note**: Review the output of the command and verify that the number of nodes increased to 2.

1. From the **Cloud Shell** pane, run the following to scale the deployment:

    ```
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

1. From the **Cloud Shell** pane, run the following to verify the outcome of scaling the deployment:

    ```
    kubectl get pods
    ```

    > **Note**: Review the output of the command and verify that the number of pods increased to 10.

1. From the **Cloud Shell** pane, run the following to review the pods distribution across cluster nodes:

    ```
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **Note**: Review the output of the command and verify that the pods are distributed across both nodes.

1. From the **Cloud Shell** pane, run the following to delete the deployment:

    ```
    kubectl delete deployment nginx-deployment
    ```
1. Close the **Cloud Shell** pane.

1. End of day 2 and **Imersao Azure Azure Expert Online**.

1. Continue in the **Mentoria Arquiteto Cloud**.









