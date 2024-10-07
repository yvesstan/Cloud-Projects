# Cloud-Projects

## SUMMARY
In this project "Securing virtual machines"  my main tasks is focus on setting up and protecting a virtual machine to create a testing environment for a small business that provides IT services to other companies. To do this I used JIT, Azure Bastion, and Azure Standard Firewall. I also set up Microsoft Sentinel to monitor the testing environment. This work serves as a solution to the implementation of the project and includes screenshots that demonstrate the steps I had taken along the process. The image below is a conceptual representation of the network configuration that I've created.

## CONTEXT
A small business that provides IT services has a number of systems hosted in Microsoft Azure. The backend systems are hosted on Azure virtual machines, and they need to be securely configured and protected from threats. 

As a security engineer I've been charged to put together a testing environment using the many different Microsoft security services. This testing environment will form the basis for how production VMs will be protected in the future for this IT services provider. 

The client would like you to set up protection for a virtual machine in Azure using JIT, Azure Bastion, and Azure Standard Firewall. Once this has been set up, they would also like to configure Microsoft Sentinel to monitor the testing environment before it is deployed on the production network. 

### Step 1 Virtual machine setup
To start building a testing environment a virtual machine is needed first. Deploy a virtual machine in a new resource group. No public IP will be needed for this VM as Azure Bastion will be used for remote access



