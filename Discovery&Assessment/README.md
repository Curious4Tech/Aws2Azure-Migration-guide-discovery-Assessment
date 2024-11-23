## Introduction
This guide provides detailed steps to do discovery and assessment of an AWS VM using Azure Migrate.

## Prerequisites
Before starting, ensure you have:
- An active Azure subscription.
- Permissions to create VMs and write to Azure managed disks.
- Access to the Azure portal and VMs you want to migrate.

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



![image](https://github.com/user-attachments/assets/ac880238-9497-45f9-bdcd-2053072d6997)



- **Verify Permissions**: Ensure your Azure account has the necessary permissions.

### Download and Configure the Azure Migrate Appliance
- **Download the Appliance**: Download the Azure Migrate appliance from the Azure portal.



![image](https://github.com/user-attachments/assets/02932575-38e1-4a43-b6b4-df9512e01b96)


- Discover section under **"Azure Migrate | Servers, databases, and web apps**." It provides a guide for discovering on-premises environments using the Azure Migrate appliance. Here are the key steps shown:

  . **Select Virtualization**: The servers are virtualized, with **`Physical or other (AWS,GCP,Xen,etc.`** chosen.

  . **Generate Project Key**: An appliance named **`key1`** , and then click on **generate key** to generate a new key and then copy and save it, you will need it later.

  . **Download Appliance**: Download the .zip file (500MB), so I will use powershell for the installation process.



![image](https://github.com/user-attachments/assets/38755457-b037-44dc-b6f5-b90099d8662e)


- **Install the Appliance**: Install the appliance on a Hyper-V host.


   . Unzip the file:



![image](https://github.com/user-attachments/assets/953e1376-35e1-4e79-a175-0612854e4d1d)


  . Open the powershell and move to the path where you unziped the appliance installation folder and run: **`.\AzureMigrateInstaller.ps1`**



![image](https://github.com/user-attachments/assets/aa6487dd-8237-4c87-b922-24a1b0441d76)

  
- **Register the Appliance**: Register the appliance with your Azure Migrate project.


- Follow the powershell prompt and I read cearfully to make the right choices.

- After successfully installation copy the  provided url at the end, paste it on your web browser and then continue the procedure.



![image](https://github.com/user-attachments/assets/af9d335e-abd2-4e50-bb5a-1985d60ffd5c)



### Set up prerequisites

- **Discover VMs**: Use the Azure Migrate appliance to discover the Hyper-V hosts and VMs.

  . Accept the terms of use


![image](https://github.com/user-attachments/assets/16e1527b-a9f4-4ca2-a219-60749c2a5fb1)


  . Provide your project key, the key you have created before in the Azure portal and then click on **Verify**. You have to wait for some minutes (up to 5 minutes).



![image](https://github.com/user-attachments/assets/49bf123e-5d9c-4def-90de-ab958991c080)



  
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
  
    . **Discovery soure** : Choose **`Windows Server or Linux Server`** depending on the AWS VM OS you want to migrate.
  
    . **IP address/ FQDN** : Your AWS VM or server domain name or your AWS public IP address



![image](https://github.com/user-attachments/assets/01618c1d-cea0-4baf-bb0f-bcfa45cb1100)



  . **Map credentrials** : Choose your previous  added AWS server



![image](https://github.com/user-attachments/assets/e7994443-1c0a-40e5-ab9e-d45fa9eab7b7)


 
 - Validation has failed


![image](https://github.com/user-attachments/assets/942e679a-70d9-4b8b-a720-789723b56c9a)


- In case you encounter this problem, allow the port **`5985`** in VPC security group inboud rules.



![image](https://github.com/user-attachments/assets/9c840f34-fe12-4ec8-b0a3-6315a50d4d42)

- connect to the AWS VM, edit the firewall rules and then add an inboud rule to allow port 5985.


![image](https://github.com/user-attachments/assets/c7580d06-efe2-4c36-b31b-150a040bed07)


- Now try to click validate and then

- You can also do the same for the sql server but it is optional for this demo. So I disabled this option for the sake of this demo.



![image](https://github.com/user-attachments/assets/1b956969-2e03-440e-b5ac-5b0f2520d427)




- Now you can click on **Start discovery** to start the discovery.



![image](https://github.com/user-attachments/assets/1c4a4195-0d3a-4ea5-adcb-1f30943fae0c)


- Now you need to wait untill the discovery to finish. After a successfully finished, you can go back to the Azure portal for the rest.



![image](https://github.com/user-attachments/assets/bbe477e0-cd2e-43b1-a806-e775a1768dd7)



### Assess Hyper-V VMs

- Go back to the Azure portal, then to **Azure migrate** and then your project to see the discovery.



![image](https://github.com/user-attachments/assets/aacedbaf-a58f-482b-a30d-ef6d12b57219)



- As indicated, you will click on **Assess** and choose **Azure VM** to start assessing.



![image](https://github.com/user-attachments/assets/19ecc78d-f51e-455b-8ff7-3aeb74f56b98)


- You need to create a group for you assessment, on the basics tab, click on **edit** just beside **assessment settings**



![image](https://github.com/user-attachments/assets/83127cf0-4ac9-41a4-a082-8830ed56537e)



- In the new windows that is going to open, choose the right detail informations for your migrating server including **Target settings, VM size , Pricing, Azure Hybrid Benefit  and Security**, then click on **Save**.


