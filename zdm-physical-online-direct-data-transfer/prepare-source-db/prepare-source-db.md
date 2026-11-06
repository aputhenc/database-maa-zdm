# Prepare source database

## Introduction

Estimated Time: 30 minutes

### Objectives

In this lab

* You will check the source database to ensure it meets ZDM Physical Online Database Migration prerequisites.

* You will perform the necessary steps to modify the source database when required so that it meets the migration prerequisites.

### Prerequisites

* All previous labs have been successfully completed.

## Task 1 : Prepare source database

1. Establish connection to source database.

  Most of the steps in this lab requires a connection to source database.

  Please follow below steps to establish connection to source database using SQL*PLUS.

  Login to source database server using private IP and ssh key.

  Switch user to **oracle** using below command.

  **sudo su - oracle**

  Set the environment to connect to your database.

  Type **. oraenv** and press **Enter**. 
    
  Press **Enter** when asked for **ORACLE\_SID**.

  Type **sqlplus "/as sysdba"**  and press **Enter** to connect to source database as SYS user.

  Please find below snippet of the connection steps.

  ![Image showing sqlplus connection to source cdb](./images/lab5-t1-s1.png)

2. Establish connection to target database.

  Login to target database server using private IP and ssh key.

  Switch user to **oracle** using below command.

  **sudo su - oracle**

  Set the environment to connect to your database.

  Type **. oraenv** and press **Enter**. 
    
  Press Enter when asked for **ORACLE\_SID**.

  Type **sqlplus "/as sysdba"**  and press **Enter** to connect to target database as SYS user.

  Please find below snippet of the connection steps.

  ![Image showing sqlplus connection to target cdb](./images/lab5-t1-s2.png)

3. Check the COMPATIBLE parameter on source database.

  Execute below statement on the source database connection already established using step 1.

    ```text
    <copy>
    show parameter compatible
    </copy>
    ```

  Below is the sample output.

  ![Image showing output of compatible parameter check on source](./images/lab5-t1-s3.png)

4. Check the COMPATIBLE parameter on target database.
   
  Execute below statement on the target database connection already established using step 2.

    ```text
    <copy>
    show parameter compatible
    </copy>
    ```
   
  Below is the sample output.

  ![Image showing output of compatible parameter check on target](./images/lab5-t1-s4.png)

5. Ensure COMPATIBLE parameter on source and target database is set to same value.

   Compare the source and target database COMPATIBLE parameter values collected in steps 3 and 4.

   For this lab , you will see that COMPATIBLE parameter is set to same value on source and target and you can proceed to next step.

   Follow below actions if you are using the lab with your specific source and target.

   Please proceed to the next step if the COMPATIBLE parameter on the source and target has the same value.

   If the values are different, then you need to modify the COMPATIBLE parameter on source or target so that both values match.

   Refer below to understand more about COMPATIBLE parameter.

   https://docs.oracle.com/en/database/oracle/oracle-database/21/upgrd/what-is-oracle-database-compatibility.html#GUID-26CB83FD-F2B3-4BD1-AF39-9C2DC90D698C

   Please note that changing compatible parameter can't be reversed unless you restore the entire database backup, so plan accordingly for your production source databases.
   
6. Ensure source database is using SPFILE.

   Please ignore this step if you have provisioned the source database as per the instructions in this lab.

   Follow the below steps for the source database provisioned using steps not mentioned in this livelab.

   Execute below statement after connecting to  source database to check whether SPFILE is in use.

     ```text
     <copy>
     show parameter spfile
     </copy>
     ```

   If the above query output shows a value for the SPFILE parameter, it means the SPFILE is already in use.

   Sample output with SPFILE in use is shown below.

   ![Image showing output of spfile check](./images/lab5-t1-s6.png)

   If SPFILE is not in use, then use the below link to configure SPFILE for your database.

   https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/creating-and-configuring-an-oracle-database.html#GUID-1C90AAE6-1E89-47B9-B218-C2B0ED659B60

7. Enable database ARCHIVELOG mode.

   The source database provisioned in this livelab is not running in ARCHIVELOG mode by default. 
   
   Please follow below document and enable ARCHIVELOG mode for source database.

   https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-archived-redo-log-files.html#GUID-C12EA833-4717-430A-8919-5AEA747087B9
   
8. Configure TDE Wallet on Oracle Database 12c Release 2 and later.

   Please note down the below requirement in terms of TDE wallet.

   For Oracle Database 12c Release 2 and later, if the source database does not have Transparent Data Encryption (TDE) enabled, then it is mandatory that you configure the TDE wallet before migration begins. You need not encrypt the data in the source database; the data is encrypted at target using the wallet setup in the source database. The WALLET_TYPE can be AUTOLOGIN (preferred) or PASSWORD based.
    
   For a multitenant database, ensure that the wallet is open on all PDBs as well as the CDB, and the master key is set for all PDBs and the CDB.
    
   Please execute below query to check the status of TDE wallet in source database (using connection established in step 1).
   
     ```text
     <copy>
     select CON_ID,WALLET_TYPE,STATUS from v$encryption_wallet;
     </copy>
     ```
   
   If the query output shows WALLET\_TYPE as **UNKNOWN** and STATUS as **NOT\_AVAILABLE**, then the TDE wallet is not configured.

   Sample query output is shown below.

   ![Image showing TDE status of source database](./images/lab5-t1-s8.png)

   Please follow the below steps to enable TDE wallet for the source database provisioned in this lab.

   a. Set **ENCRYPTION\_WALLET\_LOCATION** in the $ORACLE_HOME/network/admin/sqlnet.ora file.

   Insert the below line in sqlnet.ora (Replace the **/u01/app/oracle/product/19c/dbhome\_1/network/admin/** path with your $ORACLE\_HOME/network/admin path).   
       
     ```text
     <copy>
     ENCRYPTION_WALLET_LOCATION=(SOURCE=(METHOD=FILE)(METHOD_DATA=(DIRECTORY=/u01/app/oracle/product/19c/dbhome_1/network/admin/)))
     </copy>
     ```

   Below is sample output of the contents of sqlnet.ora after the required modification.

   ![Image showing contents of sqlnet.ora in source database](./images/lab5-t1-s8-a.png)

   For an Oracle RAC instance, also set **ENCRYPTION\_WALLET\_LOCATION** in the second Oracle RAC node (Not applicable for the source database provisioned in this lab).
      
   b. Create and configure the keystore.

   Connect to source database (refer step 1 if required) for all below steps.

   Please note that you should be connected to container database when your source is a CDB.

   i. Create the keystore.      
     
   Execute the below command after modifying the keystore location (same as the encryption wallet location provided in step a) and the password.

      ```text
      <copy>
      ADMINISTER KEY MANAGEMENT CREATE KEYSTORE '/u01/app/oracle/product/19c/dbhome_1/network/admin' identified by password;
      </copy>
      ```
   Below is sample output.

   ![Image showing output of create keystore command](./images/lab5-t1-s8-b-i.png)

   ii. Open the keystore.

   For a CDB environment (source database in this lab is CDB ),  run the following command after updating with your password.

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT SET KEYSTORE OPEN IDENTIFIED BY password container = ALL;
     </copy>
     ```
   For a non-CDB environment, run the following command.

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT SET KEYSTORE OPEN IDENTIFIED BY password;
     </copy>
     ```
   Below is sample output.

   ![Image showing output of open keystore command](./images/lab5-t1-s8-b-ii.png)

   iii. Create and activate the master encryption key.

   For a CDB environment, run the following command (ensure to update the password).

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT SET KEY IDENTIFIED BY password with backup container = ALL;
     </copy>
     ```
   For a non-CDB environment, run the following command.

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT SET KEY IDENTIFIED BY password with backup;
     </copy>
     ```
   Below is sample output.

   ![Image showing output of activate keystore command](./images/lab5-t1-s8-b-iii.png)

   iv. Query V$ENCRYPTION_WALLET to get the keystore status, keystore type, and keystore location.

     ```text
     <copy>
     col WRL_PARAMETER for a55
     set lines 150
     select WRL_TYPE,WRL_PARAMETER,STATUS,WALLET_TYPE from v$encryption_wallet;
     </copy>
     ```

   If the query output shows STATUS as OPEN and WALLET_TYPE as PASSWORD, it means the configuration of the password-based keystore is complete at this stage.

   Below is sample output.

   ![Image showing status of password based keystore](images/lab5-t1-s8-b-iv.png)

   You will use an auto-login keystore in this lab which requires additional steps as mentioned below.
   
   c. Creation of auto-login keystore.
   
   i. Create the auto-login keystore.

   Execute below query after modifying keystore location (same as the keystore location specified in step b) and password.

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT CREATE AUTO_LOGIN KEYSTORE FROM KEYSTORE '/u01/app/oracle/product/19c/dbhome_1/network/admin/' IDENTIFIED BY password;
     </copy>
     ```

   Below is sample output.

   ![Image showing creation of auto login keystore](images/lab5-t1-s8-c-i.png)

   ii. Close the password-based keystore.

   Execute the below statement after replacing PASSWORD with your password.

     ```text
     <copy>
     ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE IDENTIFIED BY PASSWORD;
     </copy>
     ```
   Below is sample output.

   ![Image showing output of password-based keystore close command](images/lab5-t1-s8-c-ii.png)

   iii. Query V$ENCRYPTION_WALLET to get the keystore status, keystore type, and keystore location.

   Execute below statement.

     ```text
     <copy>
     col WRL_PARAMETER for a55
     set lines 150
     select WRL_TYPE,WRL_PARAMETER,STATUS,WALLET_TYPE from v$encryption_wallet;
     </copy>
     ```
   In the query output, verify that the TDE keystore STATUS is OPEN and WALLET_TYPE is AUTOLOGIN, otherwise the auto-login keystore is not set up correctly.
     
   Sample output is shown below.
   ![Image showing auto login keystore status](./images/lab5-t1-s8-c-iii.png)

   d. Copy the keystore files to the second Oracle RAC node.

   This step does not apply to the source database you have provisioned in this lab.

   You also don't have to take any action for an Oracle RAC source database configured with keystore on a shared file system.

   Follow the below additional step for enabling TDE wallet for an Oracle RAC source database with out shared access to keystore.

   Copy the following files from the RAC node where you enabled TDE wallet (steps a,b and c) to the same location on other RAC node.

   /u01/app/oracle/product/19c/dbhome_1/network/admin/ew*

   /u01/app/oracle/product/19c/dbhome_1/network/admin/cw*

9. Check SQL*Net connectivity.

   This livelab requires below SQL*Net connectivity.
   
   SQL\*Net connectivity from source to target database server.
   
   SQL\*Net connectivity from target to source database server.

   Please follow below steps to enable required SQL\*Net connectivity.

   a. Allow incoming connection on required port in Virtual Cloud Network.

   You have deployed source database , target database and ZDM service host into the same private subnet in **zdm-vcn** for the purpose of this lab.

   You need to ensure that incoming connections on port 1521(or the configured listener port) are not blocked on VCN level.

   Follow below steps to enable incoming connection on 1521 (For simplicity , you will open port for all IPs in the private subnet , however you can restrict as you wish).

   Select the **Navigation Menu** in the upper left, navigate to **Networking** and then select **Virtual Cloud Networks** as shown below.

   ![Image showing navigation to VCN in OCI ](./images/lab5-t1-s9-a-1.png)

   Select **zdm-vcn** under the list of Virtual Cloud Network in your compartment as shown below.

   ![Image showing list of VCN in OCI ](./images/lab5-t1-s9-a-2.png)

   Navigate to **Subnets** and select **zdm-vcn-priv**.

   ![Image showing list of subnets in ZDM-VCN ](./images/lab5-t1-s9-a-3.png)

   Navigate to **Security** and select **Default Security List for zdm-vcn** as shown below.

   ![Image showing default security list of private Subnet ](./images/lab5-t1-s9-a-4.png)

   Navigate to **Security rules** and select **Add Ingress Rules** to bring up a window for adding rules.

   ![Image showing option to add Ingress rules ](./images/lab5-t1-s9-a-5.png)

   Populate the rules with below details , please enter the source CIDR same as the private subnet CIDR for this lab.

   Source Type: CIDR

   Source CIDR: 10.20.17.0/25

   IP Protocol: TCP

   Source Port Range: Leave it null.

   Destination Port Range: 1521

   Description: This is optional.

   ![Image showing option to add Ingress rules ](./images/lab5-t1-s9-a-6.png)

   Select **Add Ingress Rules** to add the rule to Security list.
   
   You have added necessary rule to allow incoming traffic on 1521.

   b. Configure connectivity from source database server to target database server.

   We are using SQL*Net connectivity for this lab which requires SCAN name of target database server resolvable from source database server.

   Please follow below steps to enable SQL*Net connectivity from source to target.

   i. Collet the target database server SCAN name.

   Login to target database server using private IP and ssh key file.

   Display the contents of  **/etc/hosts** file as shown below.

   ![Image showing target database server etc-hosts file contents ](./images/lab5-t1-s9-b-i.png)

   Copy the highlighted line related to target database SCAN FQDN and IP.

   ii. Modify source database server **/etc/hosts** file.

   Login to source database server using private IP and ssh key file.

   Open **/etc/hosts** file for editing by running below command as **opc** user.
   
   **sudo vi /etc/hosts**

   Insert the Target database SCAN FQDN and IP copied in previous step to /etc/hosts and save the file.

   Below is sample contents of **/etc/hosts** file after modification.

   ![Image showing source database server etc-hosts file contents ](./images/lab5-t1-s9-b-ii.png)

   iii. Verify connectivity from source database to target database server.

   Execute the below steps on source database server.

   Switch user to **oracle** using below command.

   **sudo su - oracle**

   Execute below command to check the connectivity on port 1521 (or whichever listener port )

   tnsping **target\_db\_scan\_name**:1521

   if the command output shows **OK(x msec)** as shown below , it means SQL*Net connectivity is successful from source database to target database server.

   ![Image showing output of tnsping from source to target database server ](./images/lab5-t1-s9-b-iii.png)
   
   c. Configure connectivity from target database server to source database server.

   i. Remove retriction on firewall.

   Login to source database server.

   Execute below command as **opc** user to remove restriction on firewall after replacing the **CIDR of target database subnet** which is same as private subnet CIDR for this Lab.

     ```text
     <copy>
      sudo firewall-cmd --permanent --zone=public \
      --add-rich-rule='rule family="ipv4" source address="<CIDR of target database subnet>" port port="1521" protocol="tcp" accept'
      sudo firewall-cmd --reload
     </copy>
     ```
         
   Sample output is shown below.
      
   ![Image showing ouput of command used for modifying firewall ](./images/lab5-t1-s9-c-i.png)

   ii. Modify target database server **/etc/hosts** file.

   Login to target database server.

   Open **/etc/hosts** file for editing by running below command as **opc** user.
   
   **sudo vi /etc/hosts**

   Insert source database private IP and FQDN (collected in Lab 2 task 2 step 14) to /etc/hosts file and save it.

   Below is sample contents of **/etc/hosts** file after modification.

   ![Image showing contents of target database etc/hosts file ](./images/lab5-t1-s9-c-ii.png)

   iii. Verify connectivity from target database to source database server.

   Login to target database server using private IP and ssh key file.

   Switch user to **oracle** using below command.

   **sudo su - oracle**

   Execute below command to check the SQL*Net connectivity on port 1521(listner port) to source database server.

   tnsping **source\_host\_name**:1521

   If the command output shows **OK(x msec)** as shown below , it means SQL*Net connectivity is successful from target database server to source database server.

   ![Image showing output of tnsping from target to source database server ](./images/lab5-t1-s9-c-iii.png)
 
10. Configure RMAN to automatically backup control file.

   Ignore this step for the source database you have configured in this lab since it has automatic controlfile backup already configured.

   Follow the below steps for the source database configured using steps mentioned not in this lab.

   Connect to source database using RMAN and execute below query to check the controlfile AUTOBACKUP configuration.

     ```text
     <copy>
     RMAN> show CONTROLFILE AUTOBACKUP;
     </copy>
     ```
   Below is the sample output which shows AUTOBACKUP is ON.

   ![Image showing controlfile autobackup status](./images/lab5-t1-s10-1.png)

   Proceed with below action is RMAN is not configured for AUTOBACKUP.
    
   Connect to source database using RMAN and execute below query to enable controlfile autobackup.

     ```text
     <copy>
     RMAN> CONFIGURE CONTROLFILE AUTOBACKUP ON;
     </copy>
     ```
   Below is sample output.
   
   ![Image showing output of command used to enable AUTOBACKUP](./images/lab5-t1-s10-2.png)

11. Register database with srvctl.

   Ignore this step for the source database provisioned in this lab since it is not using Grid Infrastructure.

   If the source database is deployed using Oracle Grid Infrastructure and the database is not registered using SRVCTL, then you must register the database before the migration.
   
12. Maintain RMAN backup strategy.

   This step can be ignored for this lab since there is no existing RMAN backup strategy for the source database used in this lab.

   Follow the below procedure if your source database has an existing RMAN backup strategy.

   To preserve the source database Recovery Time Objective (RTO) and Recovery Point Objective (RPO) during the migration, the existing RMAN backup strategy should be maintained.

   During the migration a dual backup strategy will be in place; the existing backup strategy and the strategy used by Zero Downtime Migration.

   Avoid having two RMAN backup jobs running simultaneously (the existing one and the one initiated by Zero Downtime Migration).

   If archive logs were to be deleted on the source database, and these archive logs are needed by Zero Downtime Migration to synchronize the target cloud database, then these files should be restored so that Zero Downtime Migration can continue the migration process.

   
13. Ensure system time of the ZDM service host and source database server  are in sync with your Oracle Cloud Infrastructure target.

    If the time on any of these systems varies beyond 6 minutes from the time on OCI, it should be adjusted. You can use ntp time check to synchronize the time if NTP is configured. If NTP is not configured, then it is recommended that you configure it. If configuring NTP is not an option, then you need to correct the time manually to ensure it is in sync with OCI time.

    Steps to check and compare time is mentioned in next Lab.

You may now **proceed to the next lab**.

## Acknowledgements
* **Author** - Amalraj Puthenchira, Cloud Data Management Modernise Specialist, EMEA Technology Cloud Engineering
* **Last Updated By/Date** - Amalraj Puthenchira, November 2026


