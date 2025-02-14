## Introduction
This guide provides detailed steps to do discovery and assessment of an AWS VM using Azure Migrate.

## Prerequisites
Before starting, ensure you have:
- An active Azure subscription.
- Permissions to create VMs and write to Azure managed disks.
- Access to the Azure portal and VMs you want to migrate.
- An AWS VM ready for migration.

## Step-by-Step Guide

### Prepare Azure
- **Create an Azure Migrate project**: In the Azure portal, create a new project under the "Migration and Modernization" tool.



![image](https://github.com/user-attachments/assets/06703932-564d-467d-bd69-ed5fe3e2d643)


- Provide detailled information :
  
    .  **Sucbscription**
  
    .  **Resource group**
  
    .  **Project name**
  
    .  **Geography**
  
    .  **Connectivity method**

- Click on **Create** to finish your project creation



![image](https://github.com/user-attachments/assets/680f42fe-576d-449d-b94f-469c5e9522df)


- **Verify Permissions**: Ensure your Azure account has the necessary permissions.

### Download and Configure the Azure Migrate Appliance
- **Download the Appliance**: Download the Azure Migrate appliance from the Azure portal.



![image](https://github.com/user-attachments/assets/02932575-38e1-4a43-b6b4-df9512e01b96)


- Discover section under **"Azure Migrate | Servers, databases, and web apps**." It provides a guide for discovering on-premises environments using the Azure Migrate appliance. Here are the key steps shown:

  . **Select Virtualization**: The servers are virtualized, with **`Physical or other (AWS,GCP,Xen,etc.`** chosen.

  . **Generate Project Key**: An appliance named **`key1`** , and then click on **Generate key** to generate a new key and then copy and save it, you will need it later.

  . **Download Appliance**: Download the .zip file 500MB, so we will use powershell for the installation process.



![image](https://github.com/user-attachments/assets/38755457-b037-44dc-b6f5-b90099d8662e)


- **Install the Appliance**: Install the appliance on a Windows server host (am using Windows server 2019).


   . Unzip 500MB the file:



![image](https://github.com/user-attachments/assets/953e1376-35e1-4e79-a175-0612854e4d1d)


  . Open the powershell and move to the path where you unziped the appliance installation folder and run: **`.\AzureMigrateInstaller.ps1`**



![image](https://github.com/user-attachments/assets/aa6487dd-8237-4c87-b922-24a1b0441d76)

  
- **Register the Appliance**: Register the appliance with your Azure Migrate project.


- Follow the powershell prompt and I read cearfully to make the right choices.


![image](https://github.com/user-attachments/assets/af9d335e-abd2-4e50-bb5a-1985d60ffd5c)


- After successfully installation copy the  provided url at the end, paste it on your web browser and then continue the procedure.


![image](https://github.com/user-attachments/assets/19ba8adb-ad51-4c97-9faa-4abb34cec581)


### Set up prerequisites

- **Discover VMs**: Use the Azure Migrate appliance to discover the Hyper-V hosts and VMs.

  . Accept the terms of use


![image](https://github.com/user-attachments/assets/1b7d7d9b-81df-4e27-a2d0-ff1711d41127)

  . Provide your project key, the key you have created before in the Azure portal and then click on **Verify**. You have to wait for some minutes (up to 5 minutes).


![image](https://github.com/user-attachments/assets/98841b1e-d68f-49c2-9aba-a08918b8c871)



  . After some minutes, you will have a pop-pup, just clcik **Refresh**



![image](https://github.com/user-attachments/assets/a4984372-0bf8-49f5-a157-8ee09dfc913a)


- Now you need to login with your Azure account credentials. Click on **Login**


![image](https://github.com/user-attachments/assets/4aba956b-7358-4a89-ad64-0fc44429109a)


- Click on **copy code and Login** on the new pop-pup windows that is gonna open.



![image](https://github.com/user-attachments/assets/68515337-c031-4080-9abb-963fa71cc88f)


- Follow the loging procedure, by providing the code and provide your Azure account credentials. At the end you will be successfully loged in. Now you need to wait up to 10 minutes before being able to continue with the next step. Now you're  successfully registered.


![image](https://github.com/user-attachments/assets/a3bf9691-d5ef-46cf-bf72-1d349682fa56)



### Manage credentials and discovery sources

- **Add credentials**: Click on **Add credentials** and provide your server admin credeantials and the click on **Save**.

    . **Source type** : Choose **`Windows Server or Linux Server`** depending on the AWS VM OS you want to migrate.
  
    . **Friendly name** : Choose a name you want (e;g: **MigrateServer**) it doesn't have obligatory to be the same name as AWS VM.
  
    . **Username** :  Administrator (Your AWS VM username)
  
    . **Password** : admin password (Your AWS VM password)



![image](https://github.com/user-attachments/assets/9617aa23-bca9-4f08-96ee-4a48b1305abe)





- **Add Discovery** : Click on **Add discovery**, then clcik on **Add single item**. Now provide the following informations needed and then clcik on **Save**.
- 
  
    . **Discovery soure** : Choose **`Windows Server or Linux Server`** depending on the AWS VM OS you want to migrate.
  
    . **IP address/ FQDN** : Your AWS VM or server domain name or your AWS public IP address


![image](https://github.com/user-attachments/assets/f913bf2e-82f9-4387-b16e-1b86c3f28c17)




  . **Map credentrials** : Choose your previous  added AWS server



![image](https://github.com/user-attachments/assets/e7994443-1c0a-40e5-ab9e-d45fa9eab7b7)


 
 - Validation has failed. This is related to the port **`5985`**.


![image](https://github.com/user-attachments/assets/18f6f003-ea3e-469d-85c7-cad085b4cce1)


- In case you encounter this problem, allow the port **`5985`** in your VPC security group to which your VM is connected, in the inboud rules section.



![image](https://github.com/user-attachments/assets/bc06316f-e0a3-424f-a1ee-a9bc3253c8f8)


- Connect to your AWS VM, edit the firewall rules and then add an inboud rule to allow the port **`5985`**.


![image](https://github.com/user-attachments/assets/42b38f60-70cb-4b6d-8ea2-8fc9c074bb87)



- Go back to the configuration page and then click on **`Revalidate`**, now you can see that the validation is succcessfully.


![image](https://github.com/user-attachments/assets/3c1c9098-ed42-43ce-884a-22bd0576f711)


- Disabled this option for the sake of this demo.


![image](https://github.com/user-attachments/assets/1b956969-2e03-440e-b5ac-5b0f2520d427)



- Now you can click on **Start discovery** to start the discovery.


![image](https://github.com/user-attachments/assets/1c4a4195-0d3a-4ea5-adcb-1f30943fae0c)


- Now you need to wait untill the discovery to finish. After a successfully finished, you can go back to the Azure portal for the rest.



![image](https://github.com/user-attachments/assets/bbe477e0-cd2e-43b1-a806-e775a1768dd7)



### Assess your AWS VMs

- Go back to the Azure portal, then to **Azure migrate** and then your project to see the discovery.



![image](https://github.com/user-attachments/assets/aacedbaf-a58f-482b-a30d-ef6d12b57219)



- As indicated, you will click on **Assess** and choose **Azure VM** to start assessing.



![image](https://github.com/user-attachments/assets/19ecc78d-f51e-455b-8ff7-3aeb74f56b98)


- You need to create a group for you assessment, on the basics tab, click on **edit** just beside **assessment settings**



![image](https://github.com/user-attachments/assets/83127cf0-4ac9-41a4-a082-8830ed56537e)



- In the new windows that is going to open, choose the right detail informations for your migrating server including **Target settings, VM size , Pricing, Azure Hybrid Benefit  and Security**, then click on **Save**.


![image](https://github.com/user-attachments/assets/f8fbec46-3551-4882-8579-39c6db2219dc)


- Selected servers to access : Create a new group by giving it a name, choose your appliance. Make sure your server that you want to migrate is checked and then proceed.


![image](https://github.com/user-attachments/assets/d584108d-7c6d-432b-91ad-a74f429690dd)


- Verify and create assessment : Review to see if everything is correct before clicking on **Review + create assessment**. If there is any change you want to make, you can clcik on **Previous**.


![image](https://github.com/user-attachments/assets/12f7186f-203e-4cb7-9766-ad5aae11a891)



- After successfully assessment, your server is ready as you can.


![image](https://github.com/user-attachments/assets/2964c000-1277-430c-ba0c-492d82050fda)


- So our AWS VM (Server) is now ready for Azure.

![image](https://github.com/user-attachments/assets/6a59b4bf-927b-4049-85af-256c87ec96f6)



- Here we go.

![image](https://github.com/user-attachments/assets/e8bbb8bb-5b36-4e90-8739-e0ef6105a628)


---
## Conclusion

This comprehensive guide has outlined the detailed steps for performing discovery and assessment of an AWS VM using Azure Migrate. 
With your AWS VM now assessed and ready for migration, you are well-positioned to proceed with the next phase: the actual migration process. Azure Migrate simplifies cloud adoption by providing a structured and efficient pathway for transitioning workloads to Azure. Take time to review the assessment results and plan your migration strategy, ensuring a seamless transition while minimizing downtime.
