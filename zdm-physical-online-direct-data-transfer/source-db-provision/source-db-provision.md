# Provision source database

## Introduction

Estimated Time: 20 minutes

### Objectives

In this lab

* You will configure a Virtual Cloud Network in Oracle Cloud Infrastructure.
* You will create a Subnet in the Virtual Cloud Network.
* You will provision source database using Oracle Marketplace image available in Oracle Cloud Infrastructure.

### Prerequisites

* All previous labs have been successfully completed.

## Task 1 : Confgure Virtual Cloud Network and Subnet

In this task you will create a new Virtual Cloud Network , create a private subnet inside the VCN and also configure it . You will use this later to host source database compute , target database system and ZDM service host.

1. Login to your Oracle Cloud Console.

2. Create Virtual Cloud Network.

   Select the **Navigation Menu** in the upper left, navigate to **Networking** and then select **Virtual Cloud Networks**.
   
   ![Image showing navigation to VCN](./images/lab2-t1-s2-a.png " ")
 
   On the new window (as seen below) select **Create VCN** to bring up **Create a Virtual Cloud Network** window.

   ![Image showing Create VCN option](./images/lab2-t1-s2-b.png " ")

   Enter below details on the **Create a Virtual Cloud Network** window and select **Create VCN** to initiate VCN creation.

   Name: zdm-vcn
   
   IPv4 CIDR Block: 10.20.17.0/24

   ![Image showing VCN options for creation](./images/lab2-t1-s2-c.png " ")
   

3. Create Subnet.

   Navigate to **Subnets** under new VCN and select **Create Subnet** 

   ![Image showing Create Subnet option](./images/lab2-t1-s3-a.png)

   Provide below details on the **Create Subnet** window

   Name: zdm-vcn-priv
   
   IPv4 CIDR Block: 10.20.17.0/24

   Select Default for Route Table , DHCP options and Security list.


   ![Image showing Subnet details](./images/lab2-t1-s3-b.png " ")
   ![Image showing Subnet details](./images/lab2-t1-s3-c.png " ")
   ![Image showing Subnet details](./images/lab2-t1-s3-d.png " ")


   Select **Create Subnet** once the required detailed are provided.

4. Create Servive Gateway 

   Navigate to **Gateways** under new VCN and select **Create Service Gateway** under **Service Gateways**.

   ![Image showing Create Service Gateway Option](./images/lab2-t1-s4-a.png " ")

   On **Create Service Gateway** screen , enter below details

   Name: **zdm-vcn-sg**

   Services: All <Region ID> Services in Oracle Services Network

   ![Image showing Service Gateway details](./images/lab2-t1-s4-b.png " ")

   Select **Create Service Gateway** to proceed with service gateway creation.

5. Configure Route

   Navigate to **Routing** under new VCN and select **Default Route Table for zdm-vcn**.

     ![Image showing Service Gateway details](./images/lab2-t1-s5-a.png " ")

   Enter below details On the **Add Route Rules** window
   
   Target Type: Service Gateway

   Destinaton Service: All <Region ID> Services in Oracle Services Network.

   Compartnet: Select the same compartment as the Service Gateway created earlier.

   Target Service Gateway: Select Service Gateway created in earlier step.

     ![Image showing Service Gateway details](./images/lab2-t1-s5-b.png " ")


   Select **Add Route Rules** to add routes.

 

## Task 2 : Provision Source Database

1. Login to your Oracle Cloud Console.

2. Click the **Navigation Menu** in the upper left, navigate to **Marketplace** and then select **All Applications**.

   ![Image showing navigation to Marketplace](./images/lab2-t2-s2.png " ")

3. Type **Oracle Database** in search bar and press Enter.

   ![Image showing search bar for Marketplace](./images/lab2-t2-s3.png " ")

4. Click on the listed **Oracle Database** Image.

   ![Image showing Oracle Database Marketplace Image](./images/lab2-t2-s4.png " ")

5. Click on Launch Instance.
  
   ![Image showing Oracle Database Marketplace image with Launch Instance option](./images/lab2-t2-s5.png " ")

6. Select a Database image 

   There will be multiple options including choice of OS.
    
   Please choose latest 19c database version on Oracle Linux 8 since your Target DB system will be on Oracle Linux 8 for this lab.

   ![Image showing available Marketplace Database Images](./images/lab2-t2-s6.png " ")

   Please make a note of the exact DB version with Release Update since it will be required in next Lab.

   Ensure to select the correct compartment in your tenancy , accept the agreement  and then click on **Launch Instance**.

7. On the **Create compute instance** page , update **Name** as **zdm-source-db**.

   ![Image showing Compute instance Name Prompt](./images/lab2-t2-s7-a.png)

   You can leave the Placement , Image  and shape to default .

   ![Image showing compute image ](./images/lab2-t2-s7-b.png)

   ![Image showing compute shape](./images/lab2-t2-s7-c.png)

   Proceed to **Security** by selecting **Next**

8. Leave all **Security** parameters to default and proceed to **Networking** by selecting **Next**

   ![Image showing Network selection](./images/lab2-t2-s8.png " ")

9. On the Networking page , select the VCN and subnet created earlier in the lab.

   ![Image showing Network selection](./images/lab2-t2-s9-a.png " ")

   You can leave the remaining selection with the default values.

   ![Image showing network selection details](./images/lab2-t2-s9-b.png)
   ![Image showing network selection details](./images/lab2-t2-s9-c.png)

10. Under **Add SSH keys**.

     Browse and provide the public ssh key generated in Lab 1.

     ![Image showing SSK key details](./images/lab2-t2-s10.png " ")

     Select **Next** to proceed to Storage.

11. Leave the **Storage** parameters to default and proceed to review by selecting **Next**.

    ![Image showing storage parameters](./images/lab2-t2-s11.png " ")

12. Review the details and select **Create** to start the provisioning of source database.

    ![Image showing final page for compute creation](./images/lab2-t2-s12.png " ")

13. In few minutes , compute instance with database will be provisioned and running as below.
   
    ![Image showing provisioned compute instance](./images/lab2-t2-s13.png " ")

14. Note down the private IP of compute instance.

     Click the **Navigation Menu** in the upper left, navigate to **Compute** and then select **Instances**.

     ![Image showing navigation to compute instance](./images/lab2-t2-s14-a.png " ")

     Make a note of the privte IP of the source database compute.

     ![Image showing private IP of  compute instance](./images/lab2-t2-s14-b.png " ")


15. Collect the value of SYS password.

    Login to the source database server using the private IP and private ssh key.

    Please refer the value of RACPASSWORD variable in file /u01/ocidb/params.ini for SYS,SYSTEM,PDBADMIN and SYSMAN password.

    
    
You may now **proceed to the next lab**.

## Acknowledgements
* **Author** - Amalraj Puthenchira, Data Modernisation Black Belt, EMEA Technology Cloud Engineering
* **Last Updated By/Date** - Amalraj Puthenchira, November 2026



