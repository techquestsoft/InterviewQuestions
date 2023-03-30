
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


# Day 4 ================================

**********Lib from Maven Central for Accessing CosmosDB

com.azure.cosmos.spark:azure-cosmos-spark_3-3_2-12:4


cosmosEndPoint = "https://cosmossrinimar2023.documents.azure.com:443/"
cosmosKey = "W4RbXpH8fVHEKZRDUxCLoAWOKNanSOxDvZrrRrwr4RS8kfcHy9Xi623A9jvVve1qagC7sO3bQqZJACDbMLarng=="
cosmosDBName = "Covid"
cosmosCatalogName = "SouthKoreaCovid"

# Spark Configuration

spark.conf.set("spark.sql.catalog.cosmosCatalog","com.azure.cosmos.spark.CosmosCatalog")

spark.conf.set("spark.sql.catalog.cosmosCatalog.spark.cosmos.accountEndpoint",cosmosEndPoint)

spark.conf.set("spark.sql.catalog.cosmosCatalog.spark.cosmos.accountKey",cosmosKey)



dbfs:/databricks-datasets/COVID/coronavirusdataset/SeoulFloating.csv


from pyspark.sql.types import *

covidSchema = StructType([
StructField('date', DateType(), True), StructField('hour', IntegerType(), True), StructField('birth_year', IntegerType(), True), StructField('sex', StringType(), True), StructField('province', StringType(), True), StructField('city', StringType(), True), StructField('fp_num', StringType(), True)])

#dfSeoulCovd.schema

dfSeoulCovid = spark.read.format("csv").option("header","true").schema(covidSchema).load("dbfs:/databricks-datasets/COVID/coronavirusdataset/SeoulFloating.csv")

display(dfSeoulCovid.limit(5))

display(dfSeoulCovid.count())



cfg = {

    "spark.cosmos.accountEndpoint":cosmosEndPoint,
    "spark.cosmos.accountKey":cosmosKey,
    "spark.cosmos.database":cosmosDBName,
    "spark.cosmos.container":cosmosCatalogName,
    "spark.cosmos.write.strategy":"ItemOverwrite"
}


#display(dfSeoulCovid.toDF("date","hour","birth_year","sex","province","city","id").limit(10))

#Write to the cosmos db

# it will take a long time

dfSeoulCovid.toDF("date","hour","birth_year","sex","province","city","id").write.format("cosmos.oltp").options(**cfg).mode("APPEND").save()


dfRead = spark.read.format("cosmos.oltp").options(**cfg).load()
display(dfRead.count())


#reading complex json with explode

from pyspark.sql.functions import explode

#dfOwners = dfComplexJson.select("_id",explode('owners').alias("myowners"))
dfOwners = dfComplexJson.select("_id",explode('owners').alias("myowners")).select("_id","myowners.name","myowners.phone")

display(dfOwners.limit(5))
display(dfOwners.count())


#constucting json

from pyspark.sql.functions import to_json,col,struct

dfOwnersJson = dfOwners.withColumn("jsoncol",to_json(struct([dfOwners[x] for x in dfOwners.columns]))).select("jsoncol")

display(dfOwnersJson)



*****************************************************************************************

***********************Seting up Docker Desktop on Azure Win 11 VM************

1. Download Docker Desktop for
   https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
   Windows

https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe

2. Download WSL update 64 bit msi



3. Install the Docker Dekstop

4. Restart Windows after installnote

Reconnect to VM after 15 secs
5. Docker Desktop would start with a Startup Screen

Click on Close (DO NOT Click on ACCEPT)
Wait for 30 secs
Run WSLUpate msi installer
Start Docker Desktop from (Desktop Shortcut)
Now click Accept in the Startup Screen


7. Create a folder in C: drive by name kafka

Open notepad and save the following file as "docker-compose.yml" (with quotes)

--------------------------------"docker-compose.yml"-----------------
version: '2'
services:
zookeeper:
image: confluentinc/cp-zookeeper:6.0.1
container_name: zookeeper
environment:
ZOOKEEPER_CLIENT_PORT: 2181
ZOOKEEPER_TICK_TIME: 2000

broker:
image: confluentinc/cp-kafka:6.0.1
container_name: broker
depends_on:
- zookeeper
ports:
# "`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-
# An important note about accessing Kafka from clients on other machines:
# -----------------------------------------------------------------------
#
# The config used here exposes port 9092 for _external_ connections to the broker
# i.e. those from _outside_ the docker network. This could be from the host machine
# running docker, or maybe further afield if you've got a more complicated setup.
# If the latter is true, you will need to change the value 'localhost' in
# KAFKA_ADVERTISED_LISTENERS to one that is resolvable to the docker host from those
# remote clients
#
# For connections _internal_ to the docker network, such as from other services
# and components, use broker:29092.
#
# See https://rmoff.net/2018/08/02/kafka-listeners-explained/ for details
# "`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-'"`-._,-
#
- 9092:9092
environment:
KAFKA_BROKER_ID: 1
KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://broker:29092,PLAINTEXT_HOST://zzzzzzzzzzz.eastus.cloudapp.azure.com:9092
KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
KAFKA_TRANSACTION_STATE_LOG_MIN_ISR: 1
KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR: 1
KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS: 100

schema-registry:
image: confluentinc/cp-schema-registry:6.0.1
container_name: schema-registry
ports:
- "8081:8081"
depends_on:
- broker
environment:
SCHEMA_REGISTRY_HOST_NAME: schema-registry
SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: broker:29092

kafka-connect:
image: confluentinc/cp-kafka-connect:5.4.0
container_name: kafka-connect
depends_on:
- zookeeper
- broker
- schema-registry
ports:
- 8083:8083
environment:
CONNECT_LOG4J_APPENDER_STDOUT_LAYOUT_CONVERSIONPATTERN: "[%d] %p %X{connector.context}%m (%c:%L)%n"
CONNECT_BOOTSTRAP_SERVERS: "broker:29092"
CONNECT_REST_PORT: 8083
CONNECT_REST_ADVERTISED_HOST_NAME: "kafka-connect"
CONNECT_GROUP_ID: kafka-connect
CONNECT_CONFIG_STORAGE_TOPIC: docker-connect-configs
CONNECT_OFFSET_STORAGE_TOPIC: docker-connect-offsets
CONNECT_STATUS_STORAGE_TOPIC: docker-connect-status
CONNECT_KEY_CONVERTER: io.confluent.connect.avro.AvroConverter
CONNECT_KEY_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema-registry:8081'
CONNECT_VALUE_CONVERTER: io.confluent.connect.avro.AvroConverter
CONNECT_VALUE_CONVERTER_SCHEMA_REGISTRY_URL: 'http://schema-registry:8081'
CONNECT_LOG4J_ROOT_LOGLEVEL: "INFO"
CONNECT_LOG4J_LOGGERS: "org.apache.kafka.connect.runtime.rest=WARN,org.reflections=ERROR"
CONNECT_CONFIG_STORAGE_REPLICATION_FACTOR: "1"
CONNECT_OFFSET_STORAGE_REPLICATION_FACTOR: "1"
CONNECT_STATUS_STORAGE_REPLICATION_FACTOR: "1"
CONNECT_PLUGIN_PATH: '/usr/share/java'
volumes:
- $PWD/data:/data
#   - /my/local/folder/with/jdbc-driver.jar:/usr/share/java/kafka-connect-jdbc/jars/
command:
- /bin/bash
- -c
- |
# JDBC Drivers
# ------------
# MySQL
cd /usr/share/java/kafka-connect-jdbc/
# See https://dev.mysql.com/downloads/connector/j/
curl https://cdn.mysql.com/Downloads/Connector-J/mysql-connector-java-8.0.23.tar.gz | tar xz
# MS SQL
cd /usr/share/java/kafka-connect-jdbc/
# See https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc/7.0.0.jre8
curl https://repo1.maven.org/maven2/com/microsoft/sqlserver/mssql-jdbc/7.0.0.jre8/mssql-jdbc-7.0.0.jre8.jar --output mssql-jdbc-7.0.0.jre8.jar
# # Oracle
#cp /db-leach/jdbc/lib/ojdbc8.jar /usr/share/java/kafka-connect-jdbc
# Now launch Kafka Connect
sleep infinity &
/etc/confluent/docker/run

control-center:
image: confluentinc/cp-enterprise-control-center:6.0.1
container_name: control-center
depends_on:
- broker
- schema-registry
ports:
- "9021:9021"
environment:
CONTROL_CENTER_BOOTSTRAP_SERVERS: 'broker:29092'
CONTROL_CENTER_CONNECT_CONNECT_CLUSTER: 'kafka-connect:8083'
CONTROL_CENTER_SCHEMA_REGISTRY_URL: "http://schema-registry:8081"
CONTROL_CENTER_KSQL_KSQLDB_URL: "http://ksqldb:8088"
# The advertised URL needs to be the URL on which the browser
#  can access the KSQL server (e.g. http://localhost:8088/info)
CONTROL_CENTER_KSQL_KSQLDB_ADVERTISED_URL: "http://localhost:8088"
# -v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v-v
# Useful settings for development/laptop use - modify as needed for Prod
CONFLUENT_METRICS_TOPIC_REPLICATION: 1
CONTROL_CENTER_REPLICATION_FACTOR: 1
CONTROL_CENTER_COMMAND_TOPIC_REPLICATION: 1
CONTROL_CENTER_MONITORING_INTERCEPTOR_TOPIC_REPLICATION: 1
CONTROL_CENTER_INTERNAL_TOPICS_PARTITIONS: 1
CONTROL_CENTER_INTERNAL_TOPICS_REPLICATION: 1
CONTROL_CENTER_MONITORING_INTERCEPTOR_TOPIC_PARTITIONS: 1
CONTROL_CENTER_STREAMS_NUM_STREAM_THREADS: 1
CONTROL_CENTER_STREAMS_CACHE_MAX_BYTES_BUFFERING: 104857600
command:
- bash
- -c
- |
echo "Waiting two minutes for Kafka brokers to start and
necessary topics to be available"
sleep 120  
/etc/confluent/docker/run

----------------------------------cmfcmd-------------------------------------
Change the name of the url in "zzzzzzzz.westus.cloudapp.azure.com" to your VM Url and save the file as "docker-compose.yml" in c:\kafka

Open a Command Prompt
Cd c:\kafka
docker-compose up -d



Download and install python with all options checked
https://www.python.org/ftp/python/3.8.10/python-3.8.10-amd64.exe


.01PublishKafkaMessages.py

# pip install kafka-python
# imports
import time
import os
import datetime
import random
import json
from kafka import KafkaProducer
from json import dumps
from time import sleep

# create a list of 5 car models
cars= []
cars.append("Ambassador Mark IV")
cars.append("Contessa Classis")
cars.append("Standard 2000")
cars.append("Premier Padmini")
cars.append("Matiz")
# for each car generate 40 messages and push to kafka topic
producer = KafkaProducer(bootstrap_servers=['vmwin11srinikafkamar2023.eastus.cloudapp.azure.com:9092'],
value_serializer = lambda x:
dumps(x).encode("UTF-8"))
for y in range (0,40):
for car in cars:
# simulate vehicle position
reading = {'id':car,'timestamp':str(datetime.datetime.utcnow()),'rpm':random.randrange(100),'speed':random.randint(70,100),'kms':random.randint(100,1000)}
producer.send('vehicleposition',reading)
sleep(.1)
print(reading)
producer.flush



02SubscribeKafkaMessages.py

from kafka import KafkaConsumer
consumer = KafkaConsumer(bootstrap_servers=['vmwin11srinikafkamar2023.eastus.cloudapp.azure.com:9092'],
auto_offset_reset='earliest',api_version=(0,10,1))

#subscribe to the topic
consumer.subscribe('vehicleposition')
#when message arrives print
for msg in consumer:
print(msg)

