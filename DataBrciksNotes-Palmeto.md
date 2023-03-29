
1. List out some Non-Functional Requirements that you have come accross. Story.
   Performance - Software System - CPU, Memory, Network, IOPS
   100 Concurrent Requests -  REST API Method - Single Thread - 1 Sec to execute
   1 -> Locks
   2-100 are waiting
   Response Time
   100 Request -> REST API - 100 Threads
   1 Sec
   Better Response Time - Cost of Memory - 1 Thread - 1MB of Memory
   Throughput
   In-Memory Processing (No Disk IO)
   Scalability - Horizontal (Scale Out) - Vertical (Scale up)
   High Availability - DR, Replication (Replicaset),
   UX -
   Data Latency/Query Latency
   Graceful Degradation-
   Maintainnability, Servicea bility
   Diagnostics, Fault Tolerance, Resiliance _ Reliability Engg
   Safety
   Security - Authentication, Authorization, Injection Attaches, XSRF, CORS, EncDecrypt, Data at REST, DDOS

2. OLTP & OLAP. Some differences
   OLTP - Highly Transactions, ACID Compliance, ER Modelling, Structured, Relational, Normalized, SQL 92 Variant, TABLE, Designed for Insert, Update and Delete
   OLAP - Dimensional Modeling, Aggregate Data, Latent Data, Optimized for Reading, MLOAP, ROLAP, HOLAP, CACHE, CUBE, MDX/DMX, XMLA

3. RDBMS vs NoSQL. Some differences
   RDBMS - Structured, Distributed Transactions, Schemaful, Scalability Bolted On, Data written to Disk, Cache, SQL Join Surface
   NoSQL - Key Value Pair, Document Based- Composite data, No Distributed Transactions, No Joins, In Memory, Flexi Schema, Replicaset built in (HA), Distributed/Partitioned/Sharded, Data Compression - Columnar Store - Compressions, Faster Scanning of data, BSON


4. List out some demands of modern data workloads
   Data Volume, Real Time Processing(i/of Batch), Dynamic Analytics, On Demand Processing, ML, HA, Data Security (RBAC),


5. List the data exchange format options in distributed computing
   CSV, JSON, XML, AVRO, Parquet, Protobuf - Strong Types

6. When would you choose an Async Messaging over Sync Comms?
   Event Based Triggering
   REliable Messaging
   Guaranteed Delivery
   Overcome Scalability
   Sync Comm
   Client -> Server
   Async Messaging
   Publisher/Sender -> Queue/Topic -> Receiver/Subscriber
   Pub Sub
   SMS,SNS, MSMQ, JMS, MQ Services, Rabbit MQ, Kafka
   Event Grid, Event Hub, Service Bus

7. Why would an Org move to the cloud? Any Risks? Mitigation Approach.
   Move CapEx to OpEx
   less inventory/licensing easier
   Sclability (Auto Scaler/Rule engine)
   High Availability (3 Nodes)
   Engineer an Agile Model
   Security - Single Control Plane,
   Information Security - GDPR, Compliance, Governance

8. What are the types of Services offerred on the cloud

   Infra as a Svc - Starters
   PaaS - Databricks, ACS, AKS, App Service, Functions (Lambdas), Confluent Kafka Platform
   SaaS - Microsoft 365, SQL Database, CosmosDB, Storage Account

"ABC Is a Private Indian bANK. It proposed to Analyse the hourly ATM CAsh Withdrawal for the past 3 years based on ATM, KIOSK, Area, District & State"

List out the Business Rationale/Need?
1. For Efficient replenishment of Cash  - Cash Management
2. Lesser Insurance Costs
3. Reduce OpEx
4. Comission new ATM or Decomission /Move ATMs
5. No ATMs -> ATMs -> Grown in ATM Transactions -> Digital Payments -> Lessening ATM Cash Transactions


FACTS - Amouth W/D
Dimensions -
Time -  Hour, Day, Week, Month, Year,
Geography - ATM Machine, Kiosk, Area, Dist, State


Trasnsactional Data Bank  - Oracle Database
ATM Data - Vendor - Weekly basis as FTP upload - CSV

Oyo


Hybrid RT and Analytical DB

https://www.singlestore.com/ (formerly memSQL)


*************Azure Cloud Platform****************************

Security Hierarchy

Level1 - Organization - optum

Level 2 - Tenants - optum.in, optum.fr, optum.ru

Level 3 - Directory (Accounts/Principals, Groups/Memberships)
User Principal - Human User
Service Principal  - Managed Identities for use by Apps


Resources Hierarchy

Level1 - Tenant

Level 2 - Subscriptions - Commercial Container(Billing)

Level 3 - Resource Groups - To organize your resources (services)

Location

Level 4 - Resource
Storage Account - Databricks, App Services, Functions, SQL Server, Cosmos, ......
Location Choice
Proximity -FitBit - East US  -> REST API - East US
Quality of Service - East US, West US
Law of Land - Indian customers data must be in India Locations
Disaster Recovery - Biz Cont Planning - India & Norway East

Sponsorship Pass - Ensure you use only East US - Azure has provided addition/spare infra for Sponsorship

Note that you are using Subscription pass, You will be last in the Priority list. Expect resource creation to take some time


3 Resource Groups
a)  rgBricks - that we created -
Azure Databricks Service - This is a Web App. UI for managing, running notebooks, cluster management etc.
b) Resource Group - Infra for DataBricks Connected Storage Account
Storage Account
VNET
Network Security Group
Managed Identity


The Storage Account store the data - configuration, metadata, Notebooks, Cluster(s) info, logs for the connected Databricks Service
c) NetworkWatcherRG. This is required by Azure to monitor changes to any network identities


Who created the Databricks Service?

User Principal ------@hotmail.com

Your hotmail/outlook a/c is an external user
not part of this domain

Compute Resources - Azure Databricks Service - Clusters(s)

1 Databricks Service - Multiple Clusters (Work horses)

Streaming Analytics job - 24/7 - Permanent Cluster (low workload)

Intensive ETL - EOD Job - Dynamically provision a cluster (20 Nodes)

Create Cluster
Using Databricks UI
Cluster using Cluster CLI
Using Azure PowerShell

Limitation in Sponsorship - 10 VCPUS

2 Node Cluster - 1 Driver (Master) - 1 Worker Node (min 4 vCPU)

Balance only 2


Driver is the End Point for all clients (Acts as Aggregator and LoadBalancer)
Worker Nodes do the data processing. Store inmemory data. Workers can be scaled out.



All nodes are Linux Virtual Machines


\\192.168.10.36

\\192.168.10.36


Day 2 
===========================

Download PowerBI

https://www.microsoft.com/en-us/download/details.aspx?id=58494

ToolSet

1. Azure CLI

https://aka.ms/installazurecliwindows


2. Visual Studio Code



******************8# Variables


$tenandId = "85910361-6578-4823-a066-8cd42f5bbfa9"
$appId = "de4edf02-4c45-4c37-967a-b8f2e8abf429"
$appSecret = "V.q8Q~pi8rZ_uS68E72258OgAifFD~M3q-EDHdz9"
# 
$resourceGroup = "rgDatabricksAutomate"

# Login using the Service Principla
az login --service-principal --username $appId --password $appSecret --tenant $tenandId
# Set the subsciption to your subscription
$subscriptionId = "a53be5f3-8ea6-4554-b950-a94c3eea5142"
az account set --subscript $subscriptionId
# Create the Resource Group
az group create --name $resourceGroup --location "East US"
# Create databricks Workspace
az databricks workspace create --resource-group $resourceGroup --name sriniBigDataWS --location "East US"


************To resolve error - execution of scripts disabled****

1. Kill All Terminals in VS Code
2. Close VS Code
3. Open PowerShell -> RtClick-> Run As Administrator
4. Set Execution Policy
   Set-ExecutionPolicy -ExecutionPolicy UnRestricted
   5. Open VS Code -> Run As Administrator
      New Terminal



.\1.3-AutomateClusterCreation.ps1

Install-Module -Name azure.databricks.cicd.tools -RequiredVersion 2.2.5727
Import-Module -Name azure.databricks.cicd.tools
Install-Module Az.Databricks
Import-Module Az.Databricks
#Variables
$tenantId = "0cfd6307-5f4d-4276-87ce-3783a334cf08"
$appId = "f32aeee4-0457-45b0-931a-40506d8d6843"
$appSecret = "x.w8Q~kFUB~O5fF9D3P4BrBWm-Hzh75vYmgaFbSd"


# Connect to Databricks
Connect-Databricks -Region "East US" -ApplicationId $appId -Secret $appSecret `
-DatabricksOrgId 6ade9deb-fc53-4dd8-80b8-c0294c18638a `
-TenantId $tenantId
# Get the list of clusters # Expect an empty
$bearerToken = "dapi640a92295424509c85046fe8cc89eb0f-3"
Get-DataBricksClusters -BearerToken $bearerToken -Region "East US"
# Create a new Single Node Cluster
New-DatabricksCluster -BearerToken $bearerToken -Region "East US" `
-ClusterName "amitDevCluster" `
-SparkVersion "12.2.x-scala2.12" `
-NodeType "Standard_D3_v2" `
-MinNumberOfWorkers 1 `
-MaxNumberOfWorkers 1 `
-AutoTerminationMinutes 60 `
-PythonVersion 3


************Creating a cluster with AZure CLI**************

1. Start Azure Shell
# Wait for the Shell to Create Storage Account
2. Create a Virtual Environment
   $ virtualenv -p /usr/bin/python3.9 mydatabrickscli
3. Move to mydatabricks
   $ source mydatabrickscli/bin/activate
4. Install the databricks cli
   $ pip install databricks-cli
5. Connect to Databricks
   $ databricks configure --token
   --provide the ADB URL
   --provide the token (from the earlier PS1)
6. Create a json file "create-singlenode-cluster.json"
   $ code create-singlenode-cluster.json
   --paste the below json contents
   {
   "cluster_name": "yrr-dev-cluster",
   "spark_version" : "12.2.x-scala2.12",
   "node_type_id" : "Standard_D3_v2",
   "spark_conf":{
   "spark.speculation":true
   },
   "num_workers" :1
   }
   Save  Ctrl+S
   Close Ctrl:Q
7. create the cluster
   $ databricks clusters create --json-file create-singlenode-cluster.json



*************************Mounting and accessing Blob Container **********

# Mount the rawdata container as a drive onto dbfs

#variables
storageAccount = "blobstorageyrr2023mar"
storageKey = "OmoaGwiMFxBzp5TmDgEtd3Vzw6H2+cOS+QUIeduyXrsUmOVj6Rp76H09P37l6Z6tBO9WdDQRg1jY+AStdQipBA=="
#dbfs:/mnt/Blob
mountPoint = "/mnt/Blob"
storageEndPoint = "wasbs://rawdata@{}.blob.core.windows.net".format(storageAccount)
storageConnectionString = "fs.azure.account.key.{}.blob.core.windows.net".format(storageAccount)

#Try mounting using the variables
try:
dbutils.fs.mount(
source = storageEndPoint,
mount_point = mountPoint,
extra_configs = {storageConnectionString:storageKey}
)
except ex:
print("Something Went Wrong! Check Storage Account or Access Key")
print(ex)



# Day 3======================================

***********Connect to ADLS Gen 2 Storage*************

# Protocol is abfss
# Authentication is to an OAuth Endpoint

storageAccount = "gen2storsrini666"
mountPoint = "/mnt/Gen2Lake"
storeageEndPoint = "abfss://rawdata@{}.dfs.core.windows.net".format(storageAccount)

tenantId = "85910361-6578-4823-a066-8cd42f5bbfa9"
appId = "3f6df942-c2ba-480b-961a-88282169ad96"
appSecret = "T3R8Q~eocK0eeIfc16q7oQft9rRhkgMAlEdhVaQ."

oAuth2EndPoint = "https://login.microsoftonline.com/{}/oauth2/token".format(tenantId)


configs = {
"fs.azure.account.auth.type":"OAuth",
"fs.azure.account.oauth.provider.type":"org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
"fs.azure.account.oauth2.client.id": appId,
"fs.azure.account.oauth2.client.secret": appSecret,
"fs.azure.account.oauth2.client.endpoint": oAuth2EndPoint
}

try:
dbutils.fs.mount(
source = storeageEndPoint,
mount_point = mountPoint,
extra_configs = configs
)

except ex:
print("Something went wrong!")
*******************************************************

-------------Database Table---------------------

CREATE TABLE [dbo].[CUSTOMER](
[C_CUSTKEY] [int] NULL,
[C_NAME] [varchar](25) NULL,
[C_ADDRESS] [varchar](40) NULL,
[C_NATIONKEY] [smallint] NULL,
[C_PHONE] [char](15) NULL,
[C_ACCTBAL] [decimal](18, 0) NULL,
[C_MKTSEGMENT] [char](10) NULL,
[C_COMMENT] [varchar](117) NULL
) ON [PRIMARY]
GO

#connection variables
logicalServerName = "srinisqlserver2023"
databaseName = "salesdbsrini"
tableName = "dbo.CUSTOMER"
userName = "sriniadmin"
password = "Password123#"

jdbcUrl = "jdbc:sqlserver://srinisqlserver2023.database.windows.net:1433;database=salesdbsrini;user=sriniadmin@srinisqlserver2023;password=Password123#;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"

connectionProps = {
"user": userName,
"password":password,
"driver" : "com.microsoft.sqlserver.jdbc.SQLServerDriver"
}

pushQuery = "(select * from dbo.Customer where C_MKTSEGMENT = 'BUILDING') as buildingQuery"

dfBuildingCustomers = (spark.read
.format("jdbc")
.option("url",jdbcUrl)
.option("dbtable",pushQuery)
.option("user",userName)
.option("password",password)
.load()
)

display(dfBuildingCustomers.count())


https://blobstorsrini2023mar.blob.core.windows.net/courseware/Optum-Azure-Databricks-27-31-Mar-2023-Day03.zip