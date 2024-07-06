Hello! In my [last post](https://maninekkalapudi.com/apache-spark-introduction), I've written a brief introduction to Apache Spark. In this post, we'll explore [Databricks](https://databricks.com/). Formed by the creators of Spark, Databricks is one unified platform for data and AI. It is by far the easiest and requires very minimal setup to begin with. It offers a free [community version](https://community.cloud.databricks.com/) which is a great fit for learners.

Topics covered in this post are:

1. Setting up Databricks Community Edition Account
2. Brief Walkthrough of the tool
3. Creating a Notebook
4. Setting up the Cluster
5. Adding a data file as a table
6. Reading the table in the Notebook

  

## 1. Setting up Databricks Community Edition Account

To set up the Community Edition, We need

1. A Community Edition. We can register an account at Databricks Community Edition [signup page](https://databricks.com/try-databricks). Provide all the necessary details in the signup page as seen below:

![[Databricks-CommuntiyEditionAccountSignUp.png]]

Note: "Work Email" is mandatory and it'll be the primary point of email communications

2. Select "Get Started" under Community Edition

![[Databricks-TryDatabricks.png]]

3. ...and it's time to check your work email and confirm your account.

![[DataBricks-EmailVerification.png]]

Note: We'll be redirected to Reset Password page and the password chosen is the account password.

4. Later, Go to [Community Edition login page](https://community.cloud.databricks.com/login.html) and login to the website.

![[DatabricksLogin.png]]

5. Now you should in Databricks home page

![[Databricks-Homepage.png]]

## Brief Walkthrough of the tool

The Databricks homepage is arranged in the following sections in the left pane under the logo:

1. Home/Workspace - Gives the folder structure where the files are arranged
2. Recents - Recent files
3. Data - Data from different sources like File uploads, AWS S3, DBFS and other sources.
4. Clusters - Compute units that are provisioned to run the notebooks
5. Jobs - It is a way of running a notebook and currently allowed to paid customers only
6. Search - File search

All of the functionality that Databricks are neatly arranged in the left pane.

### Common Tasks

1. New Notebook - [Jupter Notebook](https://jupyter.org/index.html) style workspace which provides interactive development environment
2. [Create Table](https://community.cloud.databricks.com/?o=5499493274185934#tables/new) - Create tables directly from imported data.
3. New Cluster - A Databricks cluster is a set of computation resources and configurations on which you run different workloads
4. New Job (For Enterprise costumers only)
5. New MLflow Experiment - End-to-end machine learning lifecycle for ML experiments
6. [Import Library](https://community.cloud.databricks.com/?o=5499493274185934#libraries/create/1874934627739449) - Import a third-part library
7. [Read Documentation](https://docs.databricks.com/)

## Creating a Notebook

If you're familiar with the [Jupyter Notebook](https://jupyter.org/try), Databricks provides similar form and functionality to the Notebooks. Databricks Notebooks offers support to **Python, Scala, SQL** and **R** languages to develop verity of Spark applications. The following steps will show how to create a notebook:

1. Click on "New Notebook" under Common Tasks section. Enter the name of the notebook.
    
    ![[Databricks-CreateNotebook.png]]
    
2. Select the language of your choice from the Default Language dropdown and click on "Create". Example, Python.
3. We can attach an already existing cluster in the Cluster dropdown. If no cluster is available, Cluster creation is explained in the next section.
4. We can edit the notebook at this point but it is necessary to attach a cluster to execute and obtain the results.

  

## Setting up the Cluster

Databricks cluster can be created in 3 methods.

1. Click on the "Detached" dropdown under Notebook's name and "Create a Cluster" option will lead us to "Create Cluster" page
2. Click on the "Cluster" tab on the left pane. Click on "Create Cluster" button in the Cluster page
3. When running the notebook or a cell, an alert is displayed as below. Click "Launch and Run" the cluster is created and attached to the notebook. A cluster named "My Cluster" is attached and displayed under Notebook's title

![[Databricks-CreateClusterAlert.png]]

Let's discuss the first and second steps further below. Third method is pretty straight forward.

**Note:** Cluster creation will take around a minute or two to complete.

### Method 1 and Method 2

In both the methods 1 and 2, we'll be navigated to the "Create Cluster" page as shown below.

![[DataBricks-CreateClusterPage.png]]

1. No. of Driver and Worker nodes. For Community Edition, only single cluster and one driver node above configuration is permitted
2. Provide the cluster name in the filed. Ex: "My Cluster"
3. Databricks Runtime Version includes the latest stable version of Spark runtime. This also includes beta versions as well and it'll be handy to test new versions
4. Since the backend compute instance are of AWS, we can select the Availability Zone between us-east-2a, us-east-2b and us-east-2c
5. After the name is provided, we can select the "Create Cluster"
6. Similar configuration can be provided in JSON as mentioned below(not available in Community Edition)

![[DataBricks-CreateCluster-JSON.png]]

Cluster page after the cluster up and running

![[Databricks-MyClusterPage.png]]

_A Databricks Unit (“DBU”) is a unit of processing capability per hour, billed on per-second usage. Databricks supports many AWS EC2 instance types. The larger the instance is, the more DBUs you will be consuming on an hourly basis. For example, 1 DBU is the equivalent of Databricks running on a c4.2xlarge machine for an hour._

  

## Adding a Data File

In the Data tab on the left pane, we can quickly check the data present in the workspace for a particular cluster. An example dataset in our case is the [San Francisco Fire Incidents dataset](https://oreil.ly/iDzQK). The following are the steps to add a CSV file to the workspace:

1. Click on Data tab on the left pane and Click on "Add Data" on the navigation pane.

![[Databricks-AddData.png]]

2. After Clicking on the Add Data you'll see the following page

![[Databricks-DataFileUpload.png]]

3. Once the file is uploaded, the file path is mentioned in the UI

![[Databricks-DataFileUploadSuccess.png]]

4. After uploading the file successfully, we can create the table from the table and use it to read in our notebooks as a table. To create the table, we need to select "Create Table with UI" and select the cluster. Later, select the "Preview Table" option to display the table.

![[Databricks-PreviewTable.png]]

5. To save the table in the "default" database, select "Create Table". The properties can be modified are

1. Table Name - Name of the table to be created
2. Database - Database to store the table
3. File Type - CSV, Avro or JSON
4. Column Delimiter - Character to separate the fields in the file
5. First row is header - Use first row as header
6. Infer schema - Infer the DataType of each column in the table and assign datatype. Ex: INT, STRING

6. After saving the table the sample schema and data are displayed as below:

![[DataBricks-SampleData-JSON.png]]

## Reading the Data file in the Notebook

After opening the "TestPySpark" notebook from the Workspace(Workspace→users→<your-email-id>), attach the cluster if not attached. The following controls are present besides the Cluster dropdown(left to right order).

![[Databricks-NotebookControls.png]]

1. Notebook actions - New, Clone, Delete and etc.
2. Cell actions - Undo, Cut cells, Copy cells and etc.
3. View Mode
4. Run all cells
5. Clear dropdown

We are now all set to run the code in the notebooks and every cell output will be displayed below it. So, let's start with simple "Spark" command.

![[Databricks-SparkCommand.png]]

Since the Spark is running in interactive mode, we will get the the SparkSession readily available and hence the above result is displayed for the command. Now, Let's read the data from the table we created earlier, ie., fire_incidents_2_csv.

```Python
# Reading data from the table 'fire_incidents_2_csv'
fire_df = spark.table('fire_incidents_2_csv')
display(fire_df.select('*'))
```

The output for the above code is as follows:

![[Databricks-DsiaplayTableData.png]]

This concludes the exercise of setting up the Databricks Community Edition account, Knowing different components of the Databricks UI and functionality, Loading the data and creating a table from it and finally reading the data from the table and displaying the read data.

## Conclusion

The Account setup process is one time setup. The steps later are very minimal to start and build the Spark applications in an interactive way. This is great for learners and for conducting fun experiments too! There are a number of features missing in the Community Edition and are available for Enterprise customers.

  

Thank you!