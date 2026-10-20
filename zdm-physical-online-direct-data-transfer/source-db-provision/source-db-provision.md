# Provision source database

## Introduction

Estimated Time: 20 minutes

### Objectives

In this lab

* You will configure a Virtual Cloud Network in Oracle Cloud Infrastructure.
* You will provision source database using Oracle Marketplace image available in Oracle Cloud Infrastructure.

### Prerequisites

* All previous labs have been successfully completed.

## Task 1 : Confgure Virtual Cloud Network

In this task you will create a new Virtual Cloud Network and configure it . You will use this later to host source database compute , target database system and ZDM service host.

1. Login to your Oracle Cloud Console.

2. Click the **Navigation Menu** in the upper left, navigate to **Networking** and then select **Virtual Cloud Networks**.
   
   ![Image showing navigation to VCN](./images/lab2-t1-s2.png " ")
 
3. Select **Create VCN** to bring up Create VCN window.

   ![Image showing navigation to VCN](./images/lab2-t1-s3.png " ")

4. Enter below details on the new window and select **Create VCN** to initiate VCN creation.

   Name: zdm-vcn
   
   IPv4 CIDR Block: 10.20.17.0/24

   ![Image showing VCN options for creation](./images/lab2-t1-s4.png " ")
   

5. Navigate to **Subnets** under new VCN and select **Create Subnet** 

   ![Image showing VCN Name prompt](./images/lab2-t1-s5.png)

6. Provide below details on the new screen.

   Name: zdm-vcn-priv
   
   IPv4 CIDR Block: 10.20.17.0/24

   Select Default for Route Table , DHCP options and Security list.


   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s6-a.png " ")
   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s6-b.png " ")
   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s6-c.png " ")


   Once details are entered , click on **Create Subnet**.

7. Navigate to **Gateways** under new VCN and select **Create Service Gateway** under Service Gateway.

   ![Image showing VCN creation options selected](./images/lab2-t1-s7.png " ")

8. On **Create Service Gateway** screen , enter below details

   Name: zdm-vcn-sg

   Services: All <Region ID> Services in Oracle Services Network

   ![Image showing VCN summary after creation](./images/lab2-t1-s8.png " ")

   Select **Create Service Gateway** to proceed with service gateway creation.

## Task 2 : Provision Source Database

1. Login to your Oracle Cloud Console.

2. Click the **Navigation Menu** in the upper left, navigate to **Marketplace** and then select **All Applications**.

   ![Image showing navigation to Marketplace](./images/lab2-t2-s2.png " ")

3. Type **Oracle Database** in search bar.

   ![Image showing search bar for Marketplace](./images/lab2-t2-s3.png " ")

4. Click on the listed **Oracle Database** Image.

   ![Image showing Oracle Database Marketplace Image](./images/lab2-t2-s4.png " ")

5. Click on Launch Instance.
  
   ![Image showing Oracle Database Marketplace image with Launch Instance option](./images/lab2-t2-s5.png " ")

6. Select an Image (There will be multiple options including choice of OS).
    
   Please choose latest 19c database version on Oracle Linux 8 since your Target DB system will be on Oracle Linux 8 for this lab.

   ![Image showing available Marketplace Database Images](./images/lab2-t2-s6.png " ")

   Please make a note of the exact OS and DB version with Release Update since it will be required in next Lab.

   Ensure to select the correct compartment in your tenancy , accept the agreement  and then click on **Launch Instance**.

7. On the **Create compute instance** page , update **Name** as **zdm-source-db**.

   ![Image showing Compute instance Name Prompt](./images/lab2-t2-s7-a.png)

   You can leave the Placement , Image  and shape to default .

   ![Image showing compute image ](./images/lab2-t2-s7-b.png)

   ![Image showing compute shape](./images/lab2-t2-s7-c.png)

   Proceed to Security by selecting **Next**

8. Leave all security parameters to default and Proceed to Networking by selecting **Next**

   ![Image showing Network selection](./images/lab2-t2-s8.png " ")

9. On the Networking page , select the VCN created earlier in the lab and also select the subnet.

   ![Image showing Network selection](./images/lab2-t2-s9-a.png " ")

   You can leave the remaining selection with the default values.

   ![Image showing network selection details](./images/lab2-t2-s9-b.png)
   ![Image showing network selection details](./images/lab2-t2-s9-c.png)

10. Under **Add SSH keys**.

     Browse and provide the public ssh key generated in Lab 1.

     ![Image showing SSK key details](./images/lab2-t2-s10.png " ")

     Click Next to proceed to Storage.

11. Leave the Storage parameters to default and Proceed to review by selecting **Next**.

    ![Image showing storage parameters](./images/lab2-t2-s11.png " ")

12. Review the detail and click on **Create** to start the provisioning.

    ![Image showing final page for compute creation](./images/lab2-t2-s12.png " ")

13. In few minutes , compute instance with database will be provisioned and running as below.
   
    ![Image showing provisioned compute instance](./images/lab2-t2-s13.png)

14. Take a note of the private IP address of the compute instance which will used to login to source database system.

15. Collect the value of SYS password.

    Login to the source database server using the private IP and private ssh key.

    Please refer the value of RACPASSWORD variable in file /u01/ocidb/params.ini for SYS,SYSTEM,PDBADMIN and SYSMAN password.

    
    
You may now **proceed to the next lab**.

## Acknowledgements
* **Author** - Amalraj Puthenchira, Data Modernisation Black Belt, EMEA Technology Cloud Engineering
* **Last Updated By/Date** - Amalraj Puthenchira, November 2026



