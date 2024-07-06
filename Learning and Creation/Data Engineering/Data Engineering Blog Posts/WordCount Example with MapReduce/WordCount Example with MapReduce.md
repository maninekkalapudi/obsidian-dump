---
Start Date: 2021-08-01
End Date: 2021-08-15
Status: Published
Assigned: MManikantha Nekkalapudi
---
Hello! Hope you're doing well. In my last [post](https://maninekkalapudi.com/hadoop-mapreduce) I've explained about internals of Hadoop MapReduce. As promised in the last post, in this post will introduce you to MapReduce programming with Java using a simple wordcount example. Let's dive in.

### Topics covered in this post

1. Pre-requisites
2. Hadoop cluster setup on local machine and on Cloud
3. Writing a MapReduce program on Eclipse
4. Create a JAR file for the MapReduce Program and Uploading to HDFS
5. Executing the MapReduce Program on the Hadoop Cluster
6. Results

### 1. Pre-requisites

1. Admin access to the machine (local preferably)
2. Hadoop Cluster (Single/Multi node cluster) on local machine or on cloud
3. Install [JDK 1.8 or later](https://www.oracle.com/in/java/technologies/javase/javase-jdk8-downloads.html) on the local machine
4. [Eclipse IDE](https://www.eclipse.org/downloads/) or any Java IDE installed on the local machine

### 2. Hadoop cluster setup on local machine and on Cloud

1. **Single Node cluster setup**
    
    As we already discussed, the DataNodes store and process the data. We need at least a single node Hadoop cluster to run the MapReduce program and process the data.
    
    Setting up a single node Hadoop cluster on a local machine is a bit lengthy process and often could lead us to errors. I'm sharing the guides that I've used to setup the cluster on my local for testing below for both and Windows and Linux.
    
    - Windows- [https://towardsdatascience.com/installing-hadoop-3-2-1-single-node-cluster-on-windows-10-ac258dd48aef](https://towardsdatascience.com/installing-hadoop-3-2-1-single-node-cluster-on-windows-10-ac258dd48aef)
    - Linux (Ubuntu)- [https://phoenixnap.com/kb/install-hadoop-ubuntu](https://phoenixnap.com/kb/install-hadoop-ubuntu)

**ii. Multi node Cluster setup**

Alternatively, we can use a cloud-based Hadoop cluster like [DataProc](https://cloud.google.com/dataproc) on Google Cloud platform (GCP) which doesn't require any setup other than selecting the configuration of the NameNode and the DataNodes. The GCP account setup can be referred [here](https://www.youtube.com/watch?v=W5mPX1-015o). We'll see the setup in the following steps.

Before going any further you should consider two important steps while operating in any cloud environment.

1. Setting up the [billing alerts](https://www.youtube.com/watch?v=F4omjjMZ54k) to avoid any unexpected bills.
2. Turn off/delete the resources soon after the work is done

1. Sign up to the [Google Cloud](https://cloud.google.com/) and login to your account

![[Untitled 27.png|Untitled 27.png]]

1. Search for "**DataProc**" and select the option with the same name in the results

![[Untitled 1 13.png|Untitled 1 13.png]]

1. Select the "**Create Cluster**" option

![[Untitled 2 13.png|Untitled 2 13.png]]

1. Provide the following details in the create cluster page under "setup a cluster page"
    
    1. Cluster name - **test-cluster**
    2. Cluster region and Zone - **us-central1**, **us-central1-a**
    3. Cluster Type - **Standard (1 master, N workers)**
    
    ![[Untitled 3 13.png|Untitled 3 13.png]]
    
    1. Autoscaling Policy - **None**
    2. Image type and version - **2.0-debian10 (default)**
    3. **Select Enable Component gateway**
    
    ![[Untitled 4 11.png|Untitled 4 11.png]]
    
    ![[Untitled 5 10.png|Untitled 5 10.png]]
    
2. Under "**Configure nodes**" select the following for Master node
    1. Machine family - **General-Purpose (default)**
    2. Series - **N1 (default)**
    3. Machine type - **n1-standard-2 (2 vCPU, 7.5 GB memory)**
    4. Primary disk size (min 15 GB) - **100GB**
    5. Primary disk type - **Standard Persistent Disk**
    6. Number of local SSDs - **0**

![[Untitled 6 10.png|Untitled 6 10.png]]

1. Select the following for "Worker Nodes"
    1. Machine family - **General-Purpose (default)**
    2. Series - **N1 (default)**
    3. Machine type - **n1-standard-2 (2 vCPU, 7.5 GB memory)**
    4. Number of worker nodes - **2**
    5. Primary disk size (min 15 GB) - **100GB**
    6. Primary disk type - **Standard Persistent Disk**
    7. Number of local SSDs **- 0**

![[Untitled 7 9.png|Untitled 7 9.png]]

1. Leave the rest of the config as is and select on "CREATE"

![[Untitled 8 7.png|Untitled 8 7.png]]

7. Click on the cluster name and select the "VM Instances" tab in the page

![[Untitled 9 6.png|Untitled 9 6.png]]

1. Click on "SSH" for the master node and you'll be presented with a new browser window connected to the master node of our HDFS cluster. I've used local terminal to connect to the master node for the rest of the post.

![[Untitled 10 5.png|Untitled 10 5.png]]

**Note:** In real world scenarios, we would connect to the Hadoop cluster via a gateway node or edge node. We'll not use the NameNode for connecting to the cluster since it'll be very busy in handling the cluster.

### 3. Writing a MapReduce program on Eclipse

1. Create a new Java project called "wordcountmapreduce" in Eclipse IDE on your local machine. Here, I'm using a Linux (ubuntu) machine to create the project. The rest of the steps should stay same for Windows machine as well.

![[Untitled 11 5.png|Untitled 11 5.png]]

b. Create a new Class for Map by right clicking on the project and select "Class". Once you select it, enter the name of the Map class as "WordCountMapper" and hit Finish.

![[Untitled 12 5.png|Untitled 12 5.png]]

c. Once WordCountMapper class is created, use the following link for the mapper, reducer, partitioner implementation for the wordcount example. Refer the [GitHub link](https://github.com/maninekkalapudi/dataengineeringbyexamples/blob/main/Hadoop/MapReduce/eclipseprojects/wordcountmapreduce/src/wordcountpackage/WordCountMapper.java) for the code.

![[Untitled 13 3.png|Untitled 13 3.png]]

d. To remove the errors in the IDE, we must mention the Hadoop libraries in project build path. The following are the libraries (only jar files) that should be added to the project:

- <hadoop_dir>/share/hadoop/mapreduce (<hadoop_dir> is the path where you saved the hadoop distribution. Ex: /home/<username>/hadoop-3.3.1)
- <hadoop_dir>/share/hadoop/hdfs
- <hadoop_dir>/share/hadoop/client
- <hadoop_dir>/share/hadoop/common
- <hadoop_dir>/share/hadoop/yarn

![[Untitled 14 3.png|Untitled 14 3.png]]

Click on "Add External JARs" and navigate to the paths mentioned in the above list. After all the required JARs, click "Apply and Close"

![[Untitled 15 3.png|Untitled 15 3.png]]

e. After adding the jars to the project build path, we can see the errors disappeared in the IDE in the below image. Use the code for the reducer (WordCountReducer.java), partitioner (WordCountPartitioner.java) and the driver (WordCount.java) classes from the [GitHub link](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/eclipseprojects/wordcountmapreduce/src/wordcountpackage)

![[Untitled 16 3.png|Untitled 16 3.png]]

f. Once the project setup is done, we will have a look at the "WordCount.java" class. This is a driver class which executes the Map, Reduce, Combiner and the Partitioner classes on the cluster. This class includes config like

1. Job Name - setJobName
2. Driver class - setJarByClass
3. Mapper class - setMapperClass
4. Combiner class - setCombinerClass. This will be same as Reducer class for the wordcount example
5. Reducer class - setReducerClass
6. Number of Reducers - setNumReduceTasks
7. Output data types from each class - setOutputKeyClass, setOutputValueClass
8. Input and Output paths - addInputPath, setOutputPath respectively

![[Untitled 17 3.png|Untitled 17 3.png]]

This is basically the end of the project and code setup required for the wordcount problem in MapReduce.

### 4. Create a JAR file for the MapReduce Program and Uploading to HDFS

Once the project and the Mapreduce code setup is done, there are two ways we could execute the MapReduce Java program:

1. Run the Java program within the eclipse. You can find the guide for the same [here](https://projectgurukul.org/create-hadoop-mapreduce-project-eclipse/).
2. Package the Java program as a JAR file with all the dependencies and execute on the Hadoop cluster. We'll follow this method for this guide

Steps to package the wordcount MapReduce Java program as a JAR file:

1. Right click on the project and select "Export" option

![[Untitled 18 3.png|Untitled 18 3.png]]

b. Under Java, select "JAR" option and click Next.

![[Untitled 19 3.png|Untitled 19 3.png]]

c. Select the path for saving the JAR file. Click Next until the final step

![[Untitled 20 2.png|Untitled 20 2.png]]

d. Select the Main class as "WordCount" using Browse window.

![[Untitled 21 2.png|Untitled 21 2.png]]

e. Select Finish to create the jar file

![[Untitled 22 2.png|Untitled 22 2.png]]

f. The jar file will be created as shown below. Once the jar file is created, we'll upload it to the GCP Hadoop cluster and run it.

![[Untitled 23 2.png|Untitled 23 2.png]]

g. Now, we'll upload this to the master node in the HDFS cluster using SCP. You can configure SSH to connect to HDFS cluster instance on GCP using this [link](https://www.youtube.com/watch?v=2ibBF9YqveY). I've used Windows + Windows Terminal and the same steps mentioned below are followed. To copy the jar file(s) to master node on the cluster, we use the following command:

```Shell
SCP -i "<Path/to/SSH/key/ssh-key>" Path/to/jar/file/wordcountmapperonly.jar  username@<master-ip>:/path/on/server
```

![[Untitled 24 2.png|Untitled 24 2.png]]

h. Once the jar file is available on the master node instance, we can use the following commands to copy the jar file to the HDFS cluster. Please note master node instance and the HDFS cluster are different.

```Shell
SSH -i "<Path/to/SSH/key/>" username@<master-ip>
hadoop fs -put -f Path/to/jar/file/wordcountmapperonly.jar <hdfs_path>
hadoop fs -ls <hdfs_path>
```

![[Untitled 25 2.png|Untitled 25 2.png]]

Here, we are copying the jar files `wordcountmapperonly.jar`, `wordcountmapreduce.jar` and `wordcountmapreducepartitioner.jar` and the input data folder `HadoopInputFiles`. The input folder contains 3 text files

### 5. Executing the MapReduce Program on the Hadoop Cluster

As we've seen already, the MapReduce driver class (WordCount.java) will be configured to execute Mapper, Combiner, Reducer and Partitioner. We'll run the MapReduce program with different configurations using the driver class

1. Only Mapper
2. Mapper and Reducer
3. Mapper, Reducer and Partitioner

We will evaluate the results after each run.

1. **Only Mapper**

To run Mapper only, we need to comment out the Combiner, Reducer and Partitioner classes configured in the driver class and package the jar file as shown in the above step. The driver class should look like the below picture. The code for the same is [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/blob/main/Hadoop/MapReduce/eclipseprojects/wordcountmapperonly/src/wordcountpackage/WordCount.java).

![[Untitled 26 2.png|Untitled 26 2.png]]

The input files are in "/HadoopInputFiles" and has data as in three files as mentioned below. You can find the input files [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/HadoopInputFiles).

![[Untitled 27 2.png|Untitled 27 2.png]]

Now, run the jar file "wordcountmapperonly.jar" on the Hadoop cluster with the following command and above input files. The steps to copy the jar file to HDFS location is shown above section.

```Shell
hadoop jar <hdfs_path>/wordcountmapperonly.jar <input_file_or_dir_path> <output_path>
```

The following image show how to run the mapreduce jars on Hadoop cluster. The full output log of the run is [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/blob/main/Hadoop/MapReduce/logs/mapperonly-log.log).

![[Untitled 28.png]]

The output of the mapper only phase contains all the words with count 1 as shown below

![[Untitled 29.png]]

Once we run the MapReduce job, we can see the application is tracked under YARN which is a resource manager for the cluster. Every run gets an entry here. The default YARN URL is **<cluster-hostname>:8088**. For DataProc cluster though, we need to go to cluster details in the GCP console, select "**Web Interfaces**" tab under cluster details and select "**YARN ResourceManager**" to get the YARN web interface.

![[Untitled 30.png]]

![[Untitled 31.png]]

In case where the output path in the `hadoop jar` command already exists, the MapReduce framework throws "**Output directory already exists**" error as shown below. This is to avoid the overwriting of any output data.

![[Untitled 32.png]]

**Note:** `-D mapred.reduce.tasks` is set to 3 by default and we need only map phase to run. We can force the reducer count to zero using this property.

In the output path, we can see four different files

1. **_SUCCESS** - Indicates the job status
2. **part-m-00000 to part-m-00002** - output file corresponding each input files. here 'm' in the output filename indicates 'mapper' phase. Since we don't have a reduce phase configured for this run, we'll get an output file for an input file

![[Untitled 33.png]]

As we already know, each mapper produces the key-value pairs <word,1> for all the words in the input sentence as output shown below

![[Untitled 34.png]]

b. **Mapper and Reducer**

Now, Let's run the '[wordcountmapreduce.jar](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/jarfiles)' with the same input files and a different output path. This has both map and reduce phase configured in the driver class. Logs for the run are [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/logs) and code for the same is [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/eclipseprojects/wordcountmapreduce)

![[Untitled 35.png]]

The output is generated after the reduce phase into a single output file. Since we have only one reducer by default in the cluster

![[Untitled 36.png]]

![[Untitled 37.png]]

c. **Mapper, Reducer and Partitioner**

Now, Let's run the '[wordcountmapreducepartitioner.jar](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/jarfiles)' with the same input files and a different output path. This has map, partition and reduce phases configured in the driver class. Logs for the run are [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/logs) and code for the same is [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/tree/main/Hadoop/MapReduce/eclipseprojects/wordcountmapreducepartitioner)

![[Untitled 38.png]]

The output for the MapReduce with partitioner is as follows. As per the partitioner logic [here](https://github.com/maninekkalapudi/dataengineeringbyexamples/blob/main/Hadoop/MapReduce/eclipseprojects/wordcountmapreducepartitioner/src/wordcountpackage/WordCountPartitioner.java), for each letter at the starting of the word, there will be a different output file created. This means we are creating 26 partitions which will create same number of reducers to process the records Example: all the words starting with letter 'a' will end up in 'part-r-00001' file

![[Untitled 39.png]]

![[Untitled 40.png]]

  

### Conclusion

We have seen a practical example of wordcount with MapReduce as promised in my last [post](https://maninekkalapudi.com/hadoop-mapreduce). This is an exhaustive guide to capture most known ways to create and execute the MapReduce programs in Java.

MapReduce as a compute has lot it edge to new compute framework like Spark. But do you know that we can use the MapReduce to ingest the data into HDFS from an RDBMS source? or write SQL like queries to execute MapReduce job? We will discuss about those in detail in my next blog posts. Stay tuned!

For now though, I'll delete the cloud resource that I've spun up for the tutorial. If you did the same, please delete the resource you have created else you'll end up with something like [this](https://twitter.com/forrestbrazeal/status/1389622850567421952?lang=en)

  

[https://twitter.com/forrestbrazeal/status/1389622850567421952?s=20](https://twitter.com/forrestbrazeal/status/1389622850567421952?s=20)

  

### Resources

1. [Big Data course](https://trendytech.in/) by [Sumit M](https://www.linkedin.com/in/bigdatabysumit/)

  

Thanks,

Mani