# Provision source database

## Introduction

Estimated Time: 20 minutes

### Objectives

In this lab

* You will provision a Virtual Cloud Network in Oracle Cloud Infrastructure.
* You will provision source database using Oracle Marketplace image available in Oracle Cloud Infrastructure.

### Prerequisites

* All previous labs have been successfully completed.

## Task 1 : Prepare Virtual Cloud Network

In this task you will create a new Virtual Cloud Network which will be used to host source database compute , target database system and ZDM service host.

1. Login to your Oracle Cloud Console.

2. Click the **Navigation Menu** in the upper left, navigate to **Networking** and then select **Virtual Cloud Networks**.
   
   ![Image showing navigation to VCN](./images/lab2-t1-s1.png " ")
 
3. Click on **Create VCN**.

   ![Image showing navigation to VCN](./images/lab2-t1-s2.png " ")

4. Enter below details on the new window and select **Create VCN** to initiate VCN creation.

   Name: zdm-vcn
   
   IPv4 CIDR Block: 10.20.17.0/24

   ![Image showing VCN options for creation](./images/lab2-t1-s3.png " ")

5. Navigate to **Subnets** under new VCN and select **Create Subnet** 

   ![Image showing VCN Name prompt](./images/lab2-t1-s4.png)

6. Provide below details on the new screen.

   Name: zdm-vcn-priv
   
   IPv4 CIDR Block: 10.20.17.0/24

   Select Default for Route Table , DHCP options and Security list.


   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s5-a.png " ")
   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s5-b.png " ")
   ![Image showing VCN and Subnet CIDR](./images/lab2-t1-s5-c.png " ")


   Once details are entered , click on **Create Subnet**.

7. Navigate to **Gateways** under new VCN and select **Create Service Gateway** under Service Gateway.

   ![Image showing VCN creation options selected](./images/lab2-t1-s6.png " ")

8. On **Create Service Gateway** screen , enter below details

   Name: zdm-vcn-sg

   Services: All <Region ID> Services in Oracle Services Network

   ![Image showing VCN summary after creation](./images/lab2-t1-s7.png " ")

   Select **Create Service Gateway** to proceed with service gateway creation.

## Task 2 : Provision Source Database

1. Login to your Oracle Cloud Console.

2. Click the **Navigation Menu** in the upper left, navigate to **Marketplace** and then select **All Applications**.

   ![Image showing navigation to Marketplace](./images/navigate-to-marketplace.png " ")

3. Type **Oracle Database** in search bar.

   ![Image showing search bar for Marketplace](./images/search-marketplace.png " ")

4. Click on the listed **Oracle Database** Image.

   ![Image showing Oracle Database Marketplace Image](./images/oracle-database-image.png " ")

5. Click on Launch Instance.
  
   ![Image showing Oracle Database Marketplace image with Launch Instance option](./images/db-image-launch-option.png " ")

6. Select an Oracle Database version ( There will be multiple options including choice of OS).
    
   Please choose database version on Oracle Linux 8 since your Target DB system will be on Oracle Linux 8 for this lab.

   ![Image showing available Marketplace Database Images](./images/db-image-options.png " ")

   Please make a note of the exact database version with Release Update since it will be required in next Lab.

7. Ensure to select the correct compartment in your tenancy , accept the agreement  and then click on **Launch Instance**.

   ![Image showing selection for compartment](./images/instance-compartment.png)

8. On the **Create compute instance** page , update **Name** for compute as **zdm-source-db** and leave the placement to default.

   ![Image showing Compute instance Name Prompt](./images/compute-name-prompt.png)

   You can leave the Image  and shape to default .

   ![Image showing compute image ](./images/compute-image.png)

   ![Image showing compute shape](./images/compute-shape.png)

9. Navigate to **Networking** by clicking **Next** two times. Select the VCN created earlier in the lab and also select the subnet.

   ![Image showing Network selection](./images/network-details-1.png " ")

   You can leave the remaining selection with the default values.

   ![Image showing network selection details](./images/network-details-2.png)

10. Under **Add SSH keys**.

     Browse and provide the public ssh key generated in Lab 1.

     ![Image showing SSK key details](./images/ssh-key-upload.png " ")

     Click Next to proceed to Storage.

11. Leave the Storage parameters to default and Click on **Next** to bring up the review screen.

    ![Image showing storage parameters](./images/storage-parameters.png " ")

12. Review the detail and click on **Create** to start the provisioning.

    ![Image showing final page for compute creation](./images/compute-creation.png " ")

13. In few minutes , compute instance with database will be provisioned and running as below.
   
    ![Image showing provisioned compute instance](./images/prov-final.png)

14. Take a note of the private IP address of the compute instance which will used to login to source database system.

15. Collect the value of SYS password.

    Login to the source database server using the Public IP and private ssh key.

    Please refer the value of RACPASSWORD variable in file /u01/ocidb/params.ini for SYS,SYSTEM,PDBADMIN and SYSMAN password.

    
    
You may now **proceed to the next lab**.

## Acknowledgements
* **Author** - Amalraj Puthenchira, Data Modernisation Black Belt, EMEA Technology Cloud Engineering
* **Last Updated By/Date** - Amalraj Puthenchira, April 2026



