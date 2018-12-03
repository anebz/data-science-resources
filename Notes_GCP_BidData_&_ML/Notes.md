# Google Cloud Platform Big Data and Machine Learning Fundamentals

Data and machine learning path with google cloud platform (GCP).

[TOC]

## Introduction to Google Cloud Platform and its Big Data Products

### What is the google cloud platform

Cloud computing: available resources when you need them but you don't need to care about anything else. 

Google buys 1/5 of CPU's made each year in the world. 

No more MapReduce in google, the cloud is based on (Dremel == Big Query) and (Flume == Data Flow). The infrastructure of google:

![alt text][GCP_products]

The cloud is key when you talk about doing totally scalable system. 

### GCP Big data products 

It's like building blocks. Depending on the roles they have this is the distribution of the different google cloud platform products. 

![alt text][GCP_products_functions]

## Foundations of GCP Compute and Storage

Computing, networking and storage is the basis of any infrastructure. Here *computing and storage* will be dealt with. 

### CPU's on demand

You need CPU's and to store persistent data. They are connected by and internal network. You normally don't work at this low level. 

The idea is to stop thinking is specific physical features, and ask for more abstract tone s(ram, cpu cores,..). All the machines have google services on them if you want to. If you use more (like permanently a machine), you can get discounts. You can change easily of machines to get the characteristic you need at each time. 

pre-entible machines: you agree not to have it if someone comes and pays the full price of the machine in exchange of a discount. If you are running fault tolerant processes as hadoop, if some nodes fail nothing will happen, it'll work slower but that's it.  

#### Lab

Objectives: 

* Create a compute engine
* SSH in into the instance

1. Open google cloud platform
2. menu> Compute Engine> Create VM
3. You can Name it, Choose specs (zones, cores, ram, memory...) 
4. You can change boot (to change the OS)
5. You need to ALLOW GOOGE CLOUD API's
6. You can allow Https, http
7. Finally there are disk configuration bellow (not used this time)
8. Click Create >> Runs >> It should be a green checkmark

SSH

1. Click SSH and a console will open
2. Check if it's empty `ls`
3. check instance `cat /proc/cpuinfo`
4. `git --version (no git yet)`
5. `sudo apt-get update`
6. `sudo apt-get -y -qq install git`
7. you can type sudo as su (to be a super user yo you don't need to type sudo all the time)

### A global file system (Cloud Storage)

Cloud Storage. If you want to stage the data into GCP first you need cloud storage. Store raw data in any format. You need to ingest the data first and in you need to do processing you need a compute engine. 

You can't attach the compute engine because if the engine goes away data does too. So you persist somewhere else. As cloud storage. 

#### How to get your data into cloud storage

The simplest way is by using a cmd tool called gsutil. Comes with G cloud SDK. So once you have it installed (in the machine you'll use to upload the files) you'll have gsutil command line. `gsutil cp yourFiles*.csv` you need to git a where `gs://whatever/data/`. You create a bucket like that. Their uniqueness works as the domains uniqueness. `mb` make a bucket, `rb` remove a bucket and `rsync` to make a mirror in local.  

All the command line tool invokes a REST API. So you can use any language and interact with cloud storage.  

![alt text][GCP_products_ingest]

You can set a transfer service, one time or recurring, and the source can be local, s3 or whatever. The access of writing and reading from buckets is linked to the permissions of the project the package belongs to. It's an easy way to build a contact delivery flow. 

#### Zones regions and latencies

You want to choose the closest one to where your clients are so you can reduce the latency. The problem is that if you have everything in one zone, if the zone goes down...That's why you have multiple zones to reduce service disruption. 

If you have a global app, you need to distribute your data across regions. 

#### Lab 

Objectives: 

* Create the compute engine
* Transform data
* Store data
* Publish on the web

![alt text][GCP_Cloud_Storage_Lab]

1. Create a google cloud engine and go to the SSH shell
2. `sudo apt-get update`
3. `sudo apt-get -y -qq install git` so we can clone repositories
4. `git clone (link of the repository)` it will be copied
5.  Now you search for the folder you need.
6.  `less ingest.sh` see what is inside (q to quit)
7.  `bash ingest.sh` to run the ingest code
8.  To install all the dependencies you make an .sh file with all the apt instances you need to make the .py work
9.  Now you can execute `python transform.py`

Create a bucket

1. Navigation menu > Storage -- Create Bucket
2. Chose name (needs to be unique) and region
3. Now you go to SHH Shell
4. You need to copy the data on that engine to the bucket: `cp <Folder you want to copy>*.csv gs://<Bucket Name>/<Name a Folder>` In the .csv folder can be changed by file.
5. Make it public > By clicking the option public in the file name (because right now it can only be seen in your bucket) 

Note-1: The is a calculator for cloud fees to know how much will you pay. 

## Data analysis on the cloud 

Big data tool and SQL databases. Manage services for common use cases. 

### Data analysis on the cloud: Managed services for common use cases

#### Products to do migration

* App engine: Let's you scale your code depending on the number of users. Run in many environment and frameworks.
* App engine flex: The same idea than app engine with a flexibility of environments and frameworks.
* Container engine: To run apps as you have them. You containerize them with docker and container engine orchestrates the containers. 
* Computer engine: You just take your server as it is and upload it to the cloud.

With this tool they allow you to do the migration at your own pace. 

#### SQL databases on the cloud

This are the different storage solution based on the access pattern. 

![alt text][GCP_Storage_based_on_access_pattern]

Cloud SQL is google manged mySQL. Advantage.

* Pricing / Time flexibility (good for unit test databases)
* MySQL (is extended)
* Google manages the backups
* Connectible anywhere
* Fast connection (specially to google cloud apps)
* Google provides security

#### Lab

Objectives

* Create a cloud SQL
* Fill the database

1. Activate google cloud shell (it's a VM with the basic packages loaded & 5GB of persistence).
2. `gcloud auth list` To see if you are authenticated
3. `gcloud config list project` see that you are in the correct project
4. Clone the code from github and navigate to the place you have the scripts
5. tableCreation.sql is a file that creates the .sql structure and the .csv have the content

Create the bucket -- If you want this data in GCP you need to create one

1. Storage > Create Bucket
2. `pwd` to check which location you are in (GCP shell)
3. Copy the files to a new sql folder (Done in the prewies lab)

Create SQL database

1. (Navigation menu) SQL > Create instance > (select mySQL)
2. Name an instaceID name a password
3. Configuration > Authorized network > Add network (We add our IP -- There is a .sh script that prints it)
4. Click create instance
5. Inside rentals > Import (Choose format)  and Browse (select from bucket)
6. First you need to import the .sql and then the .csv naming the tables as they where name in the .sql file

Open Cloud Shell

1. `mysql --host-<IP adress> --user=root --password`
2. Enter your password (will be asked)
3. `use recomendation_spark;` specific for this hadoop based context
4. `show tables`, `select * from Rating` and other SQL sentences

#### Managed Hadoop in the cloud

Hadoop is the first MapReduce framework. You can write code in java and execute it in MapRduce. Also it's popular because of it's distributed file system; HDFS. There are many programs in top of Hadoop those are 2 examples:

Pig is a scripting language much higher level and it's used as an ETL language. 

Spark is very fast and may libraries in top of it. 

##### Dataproc

It's the google managed Hadoop, Pig, Hive and Spark programs. Dataproc is a cluster. You add/remove cluster nodes and keep the cluster up. You can store the data in HDFS, but also you can store your data in google cloud storage. If you use HDFS your cluster needs to be up and running because if not you lost your data. The pre-entiable machines are specially useful in this failure prove systems.

#### Lab

Objectives:

* Train and apply ML to data loaded in the previews lab

1. Google Cloud Shell `git clone`
2. Create again the SQL tables
3. When you create a SQL database make sure you authorize the IP of the Cloud Shell
4. Create a bucket 
5. Copy all the repo you've downloaded from github to my bucket `gsutil cp cloudsql/* gs://<My Bucket>/folder`

Back to the SQL

6. Need to the IP address of the SQL
7. Now you create the table with.sql and fill it with .csv(choose recommendation spark for this case)

Dataproc

1. Menu > Dataproc > Create Cluster (change zone and region to be close)
2. You configure the characteristics of your cluster
3. You need to authorize the cluster `bash authorize_dataproc.sh <Cluster Name> <Zone> <Working Nodes>`
4. If you open train_and_apply.py, you need to change few things: Cloud_Instace_IP (you need to put the public IP of your SQL) and CloudSQL_PWD (put the password of your SQL)
5. `cntrl + O` save and `cntrl + X` to exit the editor
6. Copy this file to bucket 
7. Dataproc > Jobs Configure job type to pyspark and get the .py from the bucket
8. If you click the job yo can see what is going on the job
9. Now the recommendation table is filled with the prediction

Check the results

1. Authorize again the Cloud Shell to use the SQL
2. Now enter yor SQL instance by my SQL (as done in the lab above)

Note: Google cloud has some solutions for different fields (a good starting point)

## Scaling data analysis and machine learning

### Fast random access

Most of the languages are OOP so there is a hierarchy. If you want to persist it in a relational database there are data integrity problems. 

#### Datastore

It's transactional store for hierarchical data. 

A way to solve it is to save the object directly (Datastore lets you do that). You store a hash map, a key object database, you can search from the key or by the object properties. Is NOT an easy migration because you change the way you interact with data.

#### Bigtable

If you have sensors get a lot of messages per minute, its and append only operation. There is no need for transactional support. And then we read from the last, backwards. It's for flattened data. Only search based on the key, so it's very fast. There should be hot spots on the key. The tables should have the shape of tall and narrow. You can group together related columns. Work with it through Hbase API.

### Warehouse and interactivity: Big Query

Server less data warehouse operates in a massive scale. You use it through an API in a browser. Data tables are organized into datasets and those are linked to you project. 

#### Ingest data

![alt text][GCP_Big_Query_Ingest]

In local or in google cloud store you can handle it from the command line. 

Stream data with cloud data flow, and you can run queries even if there is a streaming.

If you leave your data in a raw form. You could have data in a google sheet an query it. You could JOIN this data with the massive data of Big Query.

### Development and demo: Cloud datalab

It's a jupyter notebook that handles GCP authentication. It's completely flexible and scalable. The notebook can be persisted in git and the computation it's run in the cloud engine. 

### Combine both

By using a module of big query you can pass the query as a string and get a pandas dataframe. Big Query can make fastly data aggregation functions. 

### Lab

Objective:

* Use Big Query and Datalab 

Big Query and Jupyter notebook (Dataset has bilions of registers)

1. Google cloud shell `datalab create bdlvm -- zone <Name of the zone>` It will take some minutes
2. Above you can open web preview > Change port
3. Top right > Ungit 
4. go to the place you want to clone and add link to clone repository and click clone
5. There in datalab a new folder will appear

The notebook

1. Works like a normal jupyter notebook
2. Big Query has a SQL like language to make queries

### Machine learning with tensorflow

Tensorflow is a google's open source library written in C++, this way it can handle the resources (CPU, GPU). API is in python. It's a numerical library but with specific features to develop NN. (playground Tensorflow -- cool to see).

Train the NN and keep changing parameters to get the best result you can. 

Gather data > Create a model > Train the model based on input data > Use it in new data

1. Gather data: Go trough data and check which ones are numeric. The NN will only add and subtract data. Categorical variables: You do a hot encoding (change it to columns and a 1 or a 5). You need at least five examples, if any parameter is too specific, you need to discard it until you have more data or the NN will memorize it. 

![alt text][GCP_NN_1]

2. Create a model: You try with several parameters to see witch one gives better results. 

![alt text][GCP_NN_2]

3. Train the model: Use the fit function with the predictors, targets and steps)

![alt text][GCP_NN_3]

4. You basically create a code that looks like that.

![alt text][GCP_NN_4]

### Lab

Objectives:

* Use the dataset to do ML with a NN
(continue the previous notebook)

### Pre-bulit machine learning models

CloudML engine, machine learning at large scale. Machine learning API. Taking pre-trained models and apply it to your data, as as the amount of data goes up, you're results will be better, and google will take care of the computation that is required (very specific problems related to images or voice-notes)

![alt text][GCP_ML_API]

vision API: 

* Label detection
* Face detection
* Key facial features
* Detect words and languages
* Explicit content adult/violent
* Places
* Logo

translation API:

* Auto translate

NPL API:

* Sentiment analysis
* Text analysis

Speech detect API:

* Detects text when you talk

Video Intelligence API

* Image detect in videos

### Lab

Objective:

* Try the APIs

1. Open Datalab
2. Clone the repository
3. Menu > API's & Services > Click the API you want and by clicking should be enabled
4. API's & Services > Credentials > Create Credentials 
5. Install the python client
6. Use the API as the notebooks says

## Data Processing Architectures: Scalable Ingest, Transform and Load

### Message-oriented architectures

Prevents applications from crashing when they get much more requests than expected. People get responses but a bit delayed. You have the receiving code and the processing code separated. New request comes, go to a messaging queue and then consumers go getting the jobs from the queue. This way you build highly available systems. You guarantee that any request sent to the system will be processed. 

![alt text][GCP_Message_Queue]

The request and the response can be separate serves, but in the same installation so you can reduce latency. 

You can build those architectures using Cloud Pub/Sub. The idea is server less so every client asks a GET or POST by *http* whenever there is a new message for it, and when there is, it processes again. 

![alt text][GCP_Message_Architecture]

### Serverless Data pipelines

Cloud Dataflow is the tool to build pipelines. Each of the lines is been running in parallel and auto scaled by the framework. Manages the auto provisioning of the machines. You get a no ops data pipeline.

![alt text][GCP_Dataflow_Pipeline]

![alt text][GCP_Connection_Other_Products]

Dataflow can be considered using instead of spark.

![alt text][GCP_dataflow_Spark]

[GCP_products]: ./doc/GCP_products.png
[GCP_products_functions]: ./doc/GCP_products_by_functions.png
[GCP_products_ingest]: ./doc/GCP_data_ingestion_pipeline.png
[GCP_Cloud_Storage_Lab]: ./doc/GCP_Cloud_Storage_Lab.png
[GCP_Storage_based_on_access_pattern]: ./doc/GCP_storage_based_on_access_pattern.png
[GCP_Big_Query_Ingest]: ./doc/GCP_Big_Query_Ingest.png
[GCP_NN_1]: ./doc/GCP_NN_1.png
[GCP_NN_2]: ./doc/GCP_NN_2.png
[GCP_NN_3]: ./doc/GCP_NN_3.png
[GCP_NN_4]: ./doc/GCP_NN_4.png
[GCP_ML_API]: ./doc/GCP_ML_API.png
[GCP_Message_Queue]: ./doc/GCP_Message_Queue.png
[GCP_Message_Architecture]: ./doc/GCP_Message_Architecture.png
[GCP_Dataflow_Pipeline]: ./doc/GCP_Dataflow_Pipeline.png
[GCP_Connection_Other_Products]: ./doc/GCP_Connection_Other_Products.png
[GCP_dataflow_Spark]: ./doc/GCP_dataflow_Spark.png
