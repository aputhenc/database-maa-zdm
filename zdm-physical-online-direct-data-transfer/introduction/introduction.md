# Introduction

## About this workshop

This workshop consists of step-by-step instrcutions for physical online migration using RMAN to backup/restore the source/target database and Oracle Data Guard for Database Synchronization.

Estimated Time: 3 hours 30 minutes


### About Zero Downtime Migration

Zero Downtime Migration provides a robust, flexible, and resumable migration process. Zero Downtime Migration integrates Oracle Maximum Availability Architecture (MAA) and supports Oracle Database 11g Release 2 (11.2.0.4) and later database releases.

You can perform and manage a database migration of an individual database or perform database migrations at a fleet level. Leveraging technologies such as Oracle Data Guard, Oracle Recovery Manager (RMAN), Oracle GoldenGate, and Oracle Data Pump, Transportable Tablespaces and Pluggable Database (PDB) Cloning, you can migrate databases online or offline.

Using Zero Downtime Migration, you can migrate Oracle databases from a variety of on premises and cloud sources to Oracle Databases into any Oracle-owned infrastructure, including Exadata Database Machine on-premises, Oracle Database Appliance, Exadata Cloud@Customer, Base Database Service, Exadata Database Service on Dedicated Infrastructure and on Exascale. Including Oracle Cloud Database Services at Oracle AI Database@Azure, Oracle AI Database@Google Cloud, and Oracle AI Database@AWS.

The Zero Downtime Migration software is a service with a command line interface that you install and run on a host that you provision. The server where the Zero Downtime Migration software is installed is called the Zero Downtime Migration service host. You can run one or more database migration jobs from the Zero Downtime Migration service host simultaneously.

Starting in release ZDM 26, a new feature, Instant Deploy, offers a no-install option, where ZDM can be deployed directly on the source or the target database server.

### Zero Downtime Migration Concepts

**Online** migration methods incur zero or minimal downtime (typically less than 15 minutes) and can leverage either physical or logical migration methods.

**Offline** migration methods will incur downtime on the source database as part of the migration process. It can leverage either physical, logical, or hybrid migration methods.

**Physical** migration methods:

Use Oracle Data Guard and RMAN, or Pluggable Database (PDB) cloning to perform migrations

Allow you to convert a non-multitenant (non-CDB) source database to a multitenant (CDB) target database

Allow you to migrate from Oracle Database 11g Release 2 (11.2.0.4) source database to a Oracle Database 12c Release 1 (12.1.0.2) or Oracle Database 19c target database, or from an Oracle Database 19c source database to an Oracle AI Database 26ai target database. This is accomplished leveraging a temporary auxiliary database.

**Logical** migration methods:

Use Oracle Data Pump and, for online migrations, Oracle GoldenGate Microservices

Allow you to perform cross-platform migration where the source Oracle Database is running on Oracle Solaris or IBM AIX operating system, and the target is an Oracle Autonomous AI Database or Co-managed Oracle Database on Oracle Linux.

Include integration with the Cloud Premigration Advisor Tool (CPAT), which a) warns you about any features used by your database that aren't supported in the target cloud environment, and b) makes suggestions for remedial changes and/or parameters to use for the Data Pump export and import operations

**Hybrid** migration methods:
Use Oracle RMAN Transportable Tablespaces for Data and Data Pump for Metadata for offline migrations.
Allows you to perform cross-platform migration where the source Oracle Database is running on Oracle Solaris or IBM AIX operating system.


For more information on Oracle Zero Downtime Migration please visit ZDM's [product website](www.oracle.com/goto/zdm).


### Objectives

In this lab, you will:
* Learn about Oracle Zero Downtime Migration Fundamentals.
* Create SSH keys.
* Provision source database.
* Provision target database.
* Provision and configure ZDM service host.
* Prepare source database.
* Prepare target database.
* Migrate database.

### Prerequisites

This workshop requires an Oracle Cloud account.


Please *proceed to the next lab*.


## Learn More

* [Oracle Zero Downtime Migration - Product Page](http://www.oracle.com/goto/zdm)
* [Oracle Zero Downtime Migration - Product Documentation](https://docs.oracle.com/en/database/oracle/zero-downtime-migration/)



## Acknowledgements
* **Author** - Ricardo Gonzalez, Senior Principal Product Manager, Oracle Cloud Database Migration
* **Contributors** - LiveLabs Team, ZDM Development Team & Amalraj Puthenchira
* **Last Updated By/Date** - Amalraj Puthenchira, Data Modernisation Black Belt , November 2026