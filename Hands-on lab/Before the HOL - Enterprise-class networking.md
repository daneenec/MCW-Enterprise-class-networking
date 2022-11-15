﻿![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
November 2022
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2022 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Enterprise-class networking in Azure before the hands-on lab setup guide](#enterprise-class-networking-in-azure-before-the-hands-on-lab-setup-guide)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Create a virtual machine to execute the PowerShell commands in the lab](#task-1-create-a-virtual-machine-to-execute-the-powershell-commands-in-the-lab)
    - [Task 2: Download hands-on lab step-by-step support files](#task-2-download-hands-on-lab-step-by-step-support-files)
    - [Task 3: Create a Virtual Network (hub) with Subnets](#task-3-create-a-virtual-network-hub-with-subnets)
    - [Task 4: Use the Azure portal for a template deployment](#task-4-use-the-azure-portal-for-a-template-deployment)
    - [Task 5: Validate the CloudShop application is up after the deployment](#task-5-validate-the-cloudshop-application-is-up-after-the-deployment)

<!-- /TOC -->

# Enterprise-class networking in Azure before the hands-on lab setup guide

## Requirements

You must have a working Azure subscription [without a spending limit](https://learn.microsoft.com/azure/cost-management-billing/manage/spending-limit#why-you-might-want-to-remove-the-spending-limit) to deploy the Barracuda firewall from the Azure Marketplace in order to carry out this step-by-step, hands-on lab.

## Before the hands-on lab

Duration: 15 minutes

### Task 1: Create a virtual machine to execute the PowerShell commands in the lab

If you are working on a machine that cannot run PowerShell, carry out this task. Only do this if you are not running the commands on your local machine and are provisioning a VM to perform the steps.

1. Launch a browser, and navigate to <https://portal.azure.com>. Once prompted, login with your Microsoft Azure credentials. If asked, choose whether your account is an organization account or just a Microsoft Account.

2. Expand the portal's left navigation by clicking **Expand portal menu** on the top left. Select **+Create a resource** on the left navigation, and in the search box, type in **Visual Studio 2022**, and press enter. In the list of results, select **Visual Studio 2022**. From the drop down, select **Visual Studio 2022 Community on Windows Server 2022 (x64)**. Then select **Create**.

    ![In this screenshot, the Visual Studio 2022 Azure Marketplace option is depicted with the 'Select a software plan' dropdown menu open with the required image and Create button selected.](images/Setup/SetupVS.png "Visual Studio image selection")

3. On the **Create a virtual machine** blade, on the **Basics** tab, select the following configuration and select **Next : Disks**:

    - Subscription: **If you have multiple subscriptions, choose the subscription to execute your labs in**.

    - Resource Group: (Create new) **OPSLABRG**

    - Virtual machine name: **LABVM**

    - Region: **Choose the closest Azure region to you**.

    - Availability options: **No infrastructure redundancy required**.

    - Security: **Standard**

    - Image: **Visual Studio 2022 Community on Windows Server 2022 (x64) - Gen 1**

    - VM architecture: **x64**

    - Size: **Standard DS1 v2** or **Standard D2s v3**

    - User name: **demouser**

    - Password/Confirm password: **demo@pass123**

    - Public inbound ports: **Allow selected ports**.

    - Select inbound ports: **RDP (3389)**

    >**Note**: If the Azure Subscription you are using is **NOT** a trial Azure subscription, you may want to choose the **Standard D2s v3** to have more power in this LABVM. If you are using a Trial Subscription or one that you know has a restriction on the number of cores, stick with **Standard DS1 v2**.

4. On the **Create a virtual machine** blade, on the **Disks** tab, select the following configuration and select **Review + create**:

    - OS disk type: **Standard SSD**
    - Delete with VM: **Checked**

5. Ensure that the validation passed and select **Create**. The deployment should begin provisioning. It may take 10+ minutes for the virtual machine to complete provisioning.

    >**Note:** Please wait for the LABVM to be provisioned prior to moving to the next step.

6. Wait for deployment status of **LABVM** to complete. Once the deployment blade displays the message **Your deployment is complete**, select **Go to resource**.

7. On the **LABVM** blade, first select **Connect**, then select **RDP**, and then select **Download RDP File** to establish a Remote Desktop session.

    ![In this screenshot, the Azure portal page of the newly created virtual machine is depicted with the Connect menu expanded. The Connect button and RDP option are highlighted.](images/Setup/image13.png "Azure Portal VM page")

8. Depending on your Remote Desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

9. Log in with the credentials specified during creation:

    - User: **demouser**

    - Password: **demo@pass123**

10. You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Choose **Yes** to continue with the connection.

    ![In this screenshot, the Remote Desktop Connection warning is depicted with the Yes button selected.](images/Setup/image14.png "Remote Desktop Connection warning")

11. Server Manager opens by default. This can be closed.

### Task 2: Download hands-on lab step-by-step support files

1. Within the Remote Desktop session to **LABVM**, open Microsoft Edge (pinned in the taskbar) and download the zipped hands-on lab step-by-step student files by navigating to this link:
    <https://github.com/microsoft/MCW-Enterprise-class-networking/tree/master/Hands-on%20lab/labfiles/ECN-Hackathon.zip>.

2. Extract the downloaded ECN-Hackathon.zip file into the directory **C:\\ECN-Hackathon**.

    ![In File Explorer, the Downloads folder and ECN-Hackathon.zip are selected. The context menu is showing for the ECN-Hackathon.zip file, and "Extract All..." is selected.](images/Setup/image23.png "File Explorer")

    ![In the Extract Files window, Files are being extracted to C:\\ECH-Hackathon, and the Extract button is selected.](images/Setup/image24.png "Extract Files window")

### Task 3: Create a Virtual Network (hub) with Subnets

1. From your **LABVM**, connect to the Azure portal, expand the left navigation select **+ Create a resource**, then in the **Search the Marketplace** box, search for and select **Virtual Network** then select **Create**.

2. On the **Basics** tab of the **Create virtual network** blade, enter the following information:

    - Subscription: **Choose your subscription**.

    - Resource group: Select **Create new**, and enter the name **WGVNetRG2**.

    - Name: **WGVNet2**

    - Region: **(US) South Central US**

3. Upon completion, it should look like the following screenshot. Validate the information is correct, and choose **Next: IP Addresses**.

    ![In this screenshot, the Basics tab of the 'Create virtual machine' blade of the Azure portal is depicted with the required settings selected and the 'Next: IP Addresses' button selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image31.png "Create virtual network basics")

4. On the **IP Addresses** tab of the **Create virtual network** blade. enter the following information:

    - IPv4 address space: **10.8.0.0/20**

    - Select **+ Add subnet** then in the **Add subnet** blade that appears on the right, enter the following information and select **Add**.

       - Subnet name: **AppSubnet**

       - Subnet address range: **10.8.0.0/25**

5. Once complete, click **Review + Create** then once the validation passes, click **Create**.

6. Go to the WGVNetRG2 Resource Group, select the **WGVNet2** blade, and select **Subnets** under **Settings** on the left.

    ![This is a subnet configuration.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image32.png "Subnets blade")

7. In the **Subnets** blade, select **+Subnet**.

    ![In the Subnets blade, the add Subnet button is selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image29.png "Subnets blade")

8. On the **Add subnet** blade, enter the following information:

    - Name: **DataSubnet**

    - Address range: **10.8.1.0/25**

    - NAT gateway: **None**

    - Network security group: **None**

    - Route table: **None**

9. When your dialog looks like the following screenshot, select **Save** to create the subnet.

    ![In this screenshot, the 'Add subnet' blade of the Azure portal is depicted with the required information for the DataSubnet selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image33.png "Add subnet blade")

10. When the subnet has completed its configuration your subnet deployment will look like the following screenshot.

     ![In this screenshot, the Azure portal Subnets blade of the WGVNet2 virtual network is depicted with the newly created subnets listed.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image158.png "Subnets blade")  

### Task 4: Use the Azure portal for a template deployment

> **Note:** If you have not downloaded the student files, complete [Task 2: Download hands-on-step-by-step support files](https://github.com/microsoft/MCW-Enterprise-class-networking/blob/master/Hands-on%20lab/Before%20the%20HOL%20-%20Enterprise-class%20networking.md#task-2-download-hands-on-lab-step-by-step-support-files).

1. On your LABVM, open the **C:\\ECN-Hackathon** folder which contains the student files for this lab.

2. Make sure you are signed into the Azure portal at <http://portal.azure.com>.

3. Expand the left navigation and choose **+ Create a resource**, and search for and select **template deployment (deploy using custom templates)**.

    ![In this screenshot, the 'Create a resource' blade is depicted with 'Template deployment (deploy using custom templates)' entered in the 'Search the Marketplace' box.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image56.png "New blade")

4. On the **Template deployment (deploy using custom templates)** blade, select **Create**.

5. On the Custom deployment blade, select **Build your own template in the editor**.

    ![In this screenshot, the 'Custom deployment' blade of the Azure portal is depicted with the 'Build your own template in the editor' button selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image57.png "Custom deployment blade")

6. Choose **Load file** and select the **CloudShop.json** file from your **C:\\ECN-Hackathon** directory and then select **Save**.

    ![In this screenshot, the 'Edit template' blade of the Azure portal is depicted with the 'Load file' button selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image58.png "Edit template blade")

7. Update the following parameters to reference the **WGVNet2** virtual network in the **WGVNetRG2** resource group and to the **AppSubnet** and **DataSubnet** subnets.

    - Existing Virtual Network Name: **WGVNet2**
  
    - Existing Virtual Network Resource Group: **WGVNetRG2**
  
    - Web Subnet: **AppSubnet**
  
    - Data Subnet: **DataSubnet**

    ![In this screenshot, the 'Custom deployment' blade of the Azure portal is depicted with the parameters listed above selected with their proper values.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image59.png "Template parameters")

8. Update the **Custom deployment** blade using the following inputs, agree to the terms, and select **Review + create** then **Create**. This deployment will take approximately 30-40 minutes.

    - Resource Group: Select **WGVNetRG2** you created earlier.

    - Location: **(US) South Central US** (The same location you used to provision resources earlier in this lab.)

    ![In this screenshot, the 'Custom deployment' blade of the Azure portal is depicted with the parameters listed above selected with their proper values.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image60.png "Template blade")

### Task 5: Validate the CloudShop application is up after the deployment

1. Using the Azure portal, open the **WGVNetRG2** Resource group and review the deployment.

2. Navigate to the **WGWEB1** blade.

3. On the **WGWEB1** blade, first select **Connect**, then select **RDP**, and then choose **Download RDP file** to establish a Remote Desktop session.

    ![In this screenshot, the WGWEB1 virtual machine blade of the Azure portal is depicted with the Connect context menu expanded. The Connect button and the RDP option are selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image61.png "Virtual machine blade")

4. Depending on your Remote Desktop protocol client and browser configuration, you will either be prompted to open an RDP file, or you will need to download it and then open it separately to connect.

5. Log in with the credentials specified during creation:

    - User: **demouser**

    - Password: **demo@pass123**

6. You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Select **Yes** to continue with the connection.

    ![In this screenshot, the Remote Desktop Connection warning is depicted with the Yes button and 'Don't ask me again' box selected.](images/Hands-onlabstep-by-step-Enterprise-classnetworkinginAzureimages/media/image62.png "Remote Desktop Connection warning dialog box")

7. Notice that Server Manager opens by default. Close Server Manager.

8. You will now ensure the CloudShop application is up and running. Open Microsoft Edge from the Start menu, and browse to both CloudShop sites running on the WGWEB1 and WGWEB2 servers using the URLs below. The output will look like the following image.

    - WGWEB1 CloudShop site: <http://wgweb1>
    - WGWEB2 CloudShop site: <http://wgweb2>

    ![In this screenshot, the Cloud Shop app is displayed. WGWEB2 is highlighted at the end of the title 'CloudShop Demo - Products - running on WGWEB2'.](images/bhol-test-cloudshop-app.png "CloudShop app running on WGWEB2")

You should follow all steps provided *before* performing the Hands-on lab.
