# Cloud-Projects --> MICROSOFT AZURE

## SUMMARY
In this project "Securing virtual machines"  my main tasks is focus on setting up and protecting a virtual machine to create a testing environment for a small business that provides IT services to other companies. To do this I used JIT, Azure Bastion, and Azure Standard Firewall. I also set up Microsoft Sentinel to monitor the testing environment. This work serves as a solution to the implementation of the project and includes screenshots that demonstrate the steps I had taken along the process. The image below is a conceptual representation of the network configuration that I've created.

## CONTEXT
A small business that provides IT services has a number of systems hosted in Microsoft Azure. The backend systems are hosted on Azure virtual machines, and they need to be securely configured and protected from threats. 

As a security engineer I've been charged to put together a testing environment using the many different Microsoft security services. This testing environment will form the basis for how production VMs will be protected in the future for this IT services provider. 

The client would like you to set up protection for a virtual machine in Azure using JIT, Azure Bastion, and Azure Standard Firewall. Once this has been set up, they would also like to configure Microsoft Sentinel to monitor the testing environment before it is deployed on the production network. 

### Step 1 Virtual machine setup
To start building a testing environment a virtual machine is needed first. Deploy a virtual machine in a new resource group. No public IP will be needed for this VM as Azure Bastion will be used for remote access.

1.  Sign into the <a href="https://portal.azure.com/#home"> Azure portal</a> with your credentials.
2.   In the Azure portal menu, select the Create a resource button located on the left-hand side of the screen.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Azure%20Portal.PNG)

3.  Next, search for “virtual network” in the search bar, select Virtual machine from the results and then select Create.
   
![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Create.PNG)

4.  In the Basics tab of the Create a virtual machine wizard, fill out the following information:

Subscription: select your subscription.

Resource group: Create new and enter "Services_Test" as the name of the new resource group.

Name: Enter "ServicesVM" as the name of the virtual machine.

Region: Select the region that is closest to you.

Image: Windows Server 2022 Datacenter: Azure Edition.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/newVM.PNG)

Scroll down and complete the rest of the details.

Size: Standard_DS1_v2

Username:AzAdmin

Password:P@$$@1234567

Confirm password: P@$$@1234567

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/User.PNG) 

5.  Select Next: Disks.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Nextt.PNG)

6.  Select Next: Networking.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Networking.PNG)

7.  Select Create New for the virtual network.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/cnew.PNG)

8.  Fill in the following details on the Create virtual network page:

Name: "Services_Test_Network"

Address space address range: 172.16.0.0/16

Subnets Subnet name: "VMs"

Subnets Address range: 172.16.1.0/24

9.  Select OK.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/CVN.PNG)

10.  For public IP select None.

11.  Select the Review + create button to review the settings.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/publicIP.PNG)

12.  Select the Create button to create the virtual network.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/E41ViewFT363Tz5nivgO4w_ffee0818149f4d38ae77cddefaa720e1_image.png)

13.  The virtual machine will now deploy.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Winss.png)


### Step 2: Hub Network with VNet peering. 
Create a hub network with VNet peering to the Service_Test_Network, all inside a new resource group ready for an Azure Standard firewall deployment. 

1.  On the Azure home page, search and select the Resource groups service.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Ressource%20grup.png)

2. Select Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/RGROUP.png)

3.  Select the following details:

Resource group: "Service_Security"

Virtual network name: "Services_Hub"

4.  Select Review + create, and then Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Ssec.png)

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Ssec1.png)

5.  Go back to the Azure home page by selecting Home in the top left-hand corner.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/RGPS.png)

6.  On the Azure home page search for Virtual networks and select the Virtual networks service.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/VNetworkss.png)

7.  Select Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/VNetCreate.png)

8.  Select your subscription and resource group Services_Security.

9.  Select Next.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Azsub.png)

10.  Select Next again.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/NAgain.png)

11.  Edit the IP address space with the following enter 192.168.1.0 and for the address space size, enter /24.

12.  Select default under Subnets.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/SubNet.png)

13.   Fill in the subnet template: Azure Firewall.

14.  Fill in a new starting address: 192.168.1.0.

15.  Select Save.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/save.png)

16. Select Review + Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Rpluscreate.png)

17.  Select Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/JTCreate.png)

18.  Select Go to resource.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/SHUB.png)

19.  Select Peerings either on the left or right-hand side of the page.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Peerings.png)

20.  Select Add to add a new peering.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/PPrE.png)

21.  Name the peering "Hub_Test".

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/AddP.png)

22.  Scroll down, leave the default settings and under Peering link name type "Test_Hub".

23.  Under the Virtual network dropdown, select Services_Test_Network.

24.  Select Add and the network peering will be set up.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/THub1.png)

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/THub2.png)

### Step 3: Azure Standard Firewall deployment 
Now we'll deploy an Azure Standard Firewall within the hub network. 

1.  Search for firewalls and select Firewalls.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ServT.png)

2.  Select Create Firewall.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Cfire.png)

3.  Fill in the following details:

Subscription: Select your subscription

Resource group: Services_Security

Give the firewall instance the name "ServicesFirewall".

Region: Select the same location that you have used previously.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/AZzz.png)

Firewall SKU: Select Standard.

Firewall management: Use Firewall rules (classic) to manage this firewall.

Virtual network: select Use existing and select Services_Security_Network.

For the public IP address select Add new and give it the name "Services". 

4.  Click Review + create then Create and the firewall will be deployed.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/CFB1.png)

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/CFB2.png)

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/CFB3.png)

### Step 4: Just-in-time access (JIT) setup
NOw we have to enable  JIT access on the Services_VM. RDP access is enabled by default. 

1.  Search for and select Virtual machines.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/JTAcces.png)

2.  Select the Services_VM virtual machine.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ServivesVM.png)

3.  Select Configuration from the left-hand side menu.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/SVM.png)

4.  Select Enable just-in-time.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/JTJ.png)

### Step 5: Azure Bastion configuration
Configure Azure Bastion for the ServicesVM.

1.  Use the search bar and search for Virtual networks and select Virtual networks.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/VNETT.png)

2.  Select the Services_Test_Network network.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/TestNet.png)

3.  On the page for the virtual network, in the left pane, select Bastion to open the Bastion page.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Basion.png)

4.  On the Bastion page, select Configure manually.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/servicTest.png)

5.  On the Create a Bastion page, use the following settings:

Instance Name: "Services_Bastion". 

Virtual network: "Services_Test_Network". 

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Instance.png)

6.  Scroll down and to configure the AzureBastionSubnet select Manage subnet configuration.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/createB.png)

7.  On the subnets page select + Subnet.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/iop.png)

8.  Create the AzureBastionSubnet subnet using the following values. 

Subnet name: "AzureBastionSubnet". 

Subnet address range: 172.16.2.0/24. 

Select Save at the bottom of the page to save your values.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ert.png)

9.  At the top of the Subnets page, select Create a Bastion to return to the Bastion configuration page.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ABS.png)

10.  Select Create new under Public IP address. Leave the default naming suggestion. Select Review + Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/STNIP.png)

11.  Select Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Previous.png)

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/remote.png)

### Step 6: Testing remote connectivity
Now we'll use Azure Bastion with JIT and connect to the Services VM using RDP, to confirm that the deployment is working. 

1.  Search and select Virtual machines.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/IAM.png)

2.  Select the Services_VM.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/SVO.png)

3.  At the top of the page, select Connect

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ATOP.png)

4.  Scroll down on the right scroll bar and select Request access.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/RDP.png)

5.  At the top of the page, select Bastion to go to the Bastion page.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/DUR.png)

6.  Select Use Bastion.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Union.png)

7.  Complete the required authentication values, for the Services_VM:

Username AzAdmin

Password P@$$@1234567

8.  Click Connect to connect to the VM

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/ServivesVM.png)

The connection to this virtual machine, via Bastion, will open directly in the Azure portal (over HTML5) using port 443 and the Bastion service.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/AD.png)

Step 7: Implement Microsoft Sentinel
We'll now implement Microsoft Sentinel ready for testing and training.

1.  From the Azure portal home page, search for and select Microsoft Sentinel.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/senti.png)

2.  Select Create Microsoft Sentinel.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/sentinel.png)

3.  Select Create a new workspace.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/HBG.png)

4.  Fill in the following deployment details for a log analytics workspace. Your current subscription will be already selected.

Resource group: "Services_Test"

Instance name: "ServiceSentinel"

5.  Select Review + Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/Analytics.png)

6.  Select Create.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/afsent.png)

7.  After a few seconds a new workspace is created. Select Service_Sentinel.

8.  Select Add.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/workspace.png)

9.  Select OK to the Microsoft Sentinel free trail.

![](https://github.com/yvesstan/Cloud-Projects/blob/main/LabPic/NewGuide.png)

### Clean-up 
If you are using your own Azure subscription, it is recommended that you follow the clean-up instructions to stop compute resources. When you're working in your own subscription, it's also a good idea at the end of a project to identify whether you still need the resources you created. Resources left running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.
Remember to also delete the Standard Firewall as it cannot be powered off and you will continue being charged if it stays active.

### Conclusion
Completing this final course project allowed me to put into practice concepts I have learned about securing virtual machines. By setting up a testing environment for a small business it help understanding how to configure and deploy key Microsoft services. I've started by deploying a virtual machine, before moving on to protecting the network by deploying an Azure firewall. After that,  I've continued by protecting management ports on that VM by using JIT and Azure Bastion combined. And finally, I have deployed Microsoft Sentinel ready for different data connectors to be added in the future.



