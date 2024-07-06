---
Start Date: 2021-07-03
End Date: 2021-08-01
Status: Published
Assigned: MManikantha Nekkalapudi
---
Hello! Hope you're doing well. In my last [post](https://maninekkalapudi.com/hdfs-hadoop-distributed-filesystem) I explained about the internals of HDFS in detail with hands-on examples. In this post we will discuss about MapReduce, a big data processing framework. It is not a mere compute framework or a tool. It is a completely new programming paradigm that simplifies the big data processing in parallel with key-value pairs. We'll discuss everything in detail with examples in this post. Let's dive in!

### Topics covered in this post

1. What is MapReduce?
2. Traditional Programming vs MapReduce
3. Higher Order Functions
4. MapReduce Framework Components
5. MapReduce on Hadoop Cluster
6. MapReduce with Combiner
7. MapReduce with Partitioner
8. Wordcount example in MapReduce

### 1. What is MapReduce?

MapReduce is a distributed parallel compute framework, and it was developed by engineers at Google around 2004. This new framework addressed the challenges Google was facing at time to process large volumes of data for indexing websites for its search engine.

Suppose when a user search for "shopping" on Google we will receive all the shopping websites or businesses most relevant to the term. To produce such relevant search results Google must crawl through every website on the internet and understand what a user might be looking for in each website and group similar websites.

Of course, this is an oversimplification of the how the search works. But our focus is to understand how Google engineers came up with a solution to understand every website (search engine indexing) at planetary scale through big data processing.

This is probably the first time where a group of inexpensive computers connected over a network (in the form of a cluster) to perform data processing in parallel. Distribution among the nodes alone was not a sufficient answer [2]. This distribution of work must be performed in parallel for the following three reasons:

1. The processing must be able to expand and contract automatically
2. The processing must be able to proceed regardless of failures in the network or the individual systems
3. Developers leveraging this approach must be able to create services that are easy to leverage by other developers. Therefore, this approach must be independent of where the data and computations have executed

MapReduce solved all the above problems by abstracting the job orchestration by providing APIs out of the box to the end user to overlook all the steps it performed.

### 2. Traditional Programming vs MapReduce.

In traditional programming style when we write a program in any programing language of our choice, the program runs on the machine and the data is present on the same machine. This is a very efficient way to process the small-scale data and it can scale up to few GBs easily.

However, In MapReduce style, the data is present on a group of machines and the program is moved to all the machines where the relevant data is present, and the data is processed locally on those machines. This avoids the data transfer over the network (which is a precious resource in datacenters) to the machine which has the program. This is especially true when the data size is massive.

In my last [post](https://maninekkalapudi.com/hdfs-hadoop-distributed-filesystem) we understood how a distributed filesystem works. A Hadoop cluster can also perform the big data processing using MapReduce framework on the same node which store the data.

### 3. Higher Order Functions

Before we understand how MapReduce we need to understand a programming concept called Higher Order Functions. All the modern programming languages support higher order functions. A [higher order function](https://en.wikipedia.org/wiki/Higher-order_function) is a function that at least does one of the following:

1. **takes one or more functions as arguments**
2. **returns a function as its result**

- `map` is a higher order function. It takes two parameters:
    1. A function which performs a task (Ex: multiply number by 2)
    2. List of values (Ex: List of numbers)

The `map` function takes a list of values and return same number of values in the result after applying a specified function. Here is an example of a map function in python

```Python
# Map function - Python example:
# Define a function which takes a parameter(a) and returns 2*a
def double(a):
	return a*2

# Create a list with some numbers
lst = [1,3,5,7,9]

# map the double function to the list of values in list 'lst'
double_lst = map(double, lst)

# Printing map object double_lst will give you the address of the map
print(double_lst)
<map obect at 0x000001756B5FACF8>

# Convert the map object to list and print the results
print(list(double_lst))

#[2, 6, 10, 14, 18] -> result of map function. Every number in the list is doubled
```

  

- `reduce` is also a higher order function takes a list of values along with the function as parameters and returns a single value. Here is an example of reduce function in python

```Python
import functools # This is a python library which has reduce function
# create a list of numbers
lst = [100, 353, 565, 976, 128, 232]

# Define a function which takes two numbers and gives the higher number as the output
def greater(a,b):
	if a > b:
		return a
	else:
		return b

print(functools.reduce(greater, lst))

# 976 -> Output of reduce function. It returns the highest numbers in the list
```

With the above example we get an understanding of how a map and a reduce function work independently. This idea can be generalized to any type of tasks, and we'll see this for wordcount problem.

### 4. MapReduce Framework Components

The three important components of MapReduce framework are:

1. Mapper
2. Reducer
3. Combiner

MapReduce library is written in Java and above components are Java classes. Every component in the MapReduce library works only with <key-value> pairs.

**1. Mapper**

A Mapper/Map are individual tasks which maps the input <key-value> pairs to an intermediate <key-value> pairs. The transformed intermediate records need to be of the same type as the input records. **The output of the Mapper is not the final output, and the output will be passed to a Reducer**.

We will understand this with a word count problem. For the Mapper, the input will be a <rk (randomkey), line> and the output of the Mapper will be <word, 1>. Every line in the input key-value will be split into words and each word will have the count of 1, even if the word is repeating. the input key (randomkey) will be ignored.

![[Untitled 42.png|Untitled 42.png]]

Our input to the Mapper or MapReduce program will be raw text for a wordcount problem. But how did we get the <rk (randomkey), line> as input to the Mapper? We'll see that in the next section.

**2. Reducer**

**A Reducer works on the intermediate output from the Mappers and aggregates the results**. **The output of the Reducer is the final output**.

In the below example, the output from the Mapper contains every word with count 1. The Reducer will take this input and aggregates (sum up) the count for each distinct keyword.

![[Untitled 1 15.png|Untitled 1 15.png]]

**3. Combiner**

A combiner will have the same aggregation logic as the Reducer (in most cases), and it runs along with the Mapper on the same machine. It performs the local aggregations at Mapper level before sending the data to a Reducer for final aggregation. Thus, decreasing the amount of data transfer from Mapper to Reducer by a huge degree.

Combiners work fine for the aggregations like count and sum, but one must be careful when implementing aggregation like average. A combiner and a Reducer should not perform average at once.

### 5. MapReduce on Hadoop Cluster

Typically, the compute nodes and the storage nodes are the same in a HDFS cluster, that is, the MapReduce framework and the HDFS are running on the same set of nodes.

As shown below, A client will send the program (jar file containing Mapper, Reducer and Combiner classes along with all necessary libraries) to each node where the relevant data is present in [serialized format](https://en.wikipedia.org/wiki/Serialization), The computations will take place on the same machine. Now, we need to focus on how computations (map and reduce) are done.

![[Untitled 2 15.png|Untitled 2 15.png]]

- When the MapReduce program (Mapper and Reducer classes) is sent to the nodes, the framework will split the data into logical `InputSplit`s and assign them to the Mapper using `InputFormat`class. So, each block (128MB max) will be the input size to each Mapper. The number of Mappers that runs is equal to number of blocks. Sometimes the logical `InputSplit`s are not enough, we can use `RecordReader` class for the splitting as per the special case.
- `RecordReader` class typically, converts the byte-oriented view of the input, provided by the InputSplit, and presents a record-oriented view for the Mapper & Reducer tasks for processing.
- What is byte-oriented view and record-oriented view? HDFS splits files as blocks (byte-oriented view). So, it is not following a logical split, meaning a part of last record may reside in one block and rest of it is in another block. This seems correct for storage but while processing, the partial records in a block cannot be processed as it is. So, the record-oriented view comes into place. This will ensure to get the remaining part of the last record in the other block to make it a block of complete records. This is called input-split (record-oriented view).[5]
- When the `RecordReader` reads each line in the file, it converts them into key-value pairs. The keys assigned to each line will be generated randomly. This will be the input (<rk (randomkey), line>) to the Mapper class in our MapReduce program. The above steps are taken care by the MapReduce framework itself. We can also customize the behavior of `RecordReader` as per our requirement. More on that [here](https://data-flair.training/blogs/hadoop-recordreader/).

![[Untitled 3 14.png|Untitled 3 14.png]]

- Once the Mapper receives the output from the `RecordReader`, the random generated keys in the input will be ignored and only the values will be considered by the Mapper for processing. In our wordcount example, we will consider only the lines and ignore random keys
- The output of each Mapper will be intermediate output and it is stored on the disk once it finishes the processing. Since Mappers runs on each node on HDFS cluster, the mapper stage will be executed parallelly and it is monitored by `JobTracker` in the framework. The output of the Map stage for a wordcount example is as mentioned below:

![[Untitled 42.png|Untitled 42.png]]

- After all the Mappers are done with the processing, the data is sorted on a disk and sent to another node within the cluster or one of the nodes which performed Map stage. This operation is called **Sorting and Shuffle,** and the node to which the data is called **Reducer**. Without a Reducer, there's no **Sorting and Shuffle** shuffling phase. Every mapper will create an output file.

![[Untitled 4 12.png|Untitled 4 12.png]]

- The Reducer will aggregate the results that it receives from all the mappers in key-value pairs. The final output will be stored in the location provided by the user. For the wordcount example, the output will be <word, count of all the occurrences>
- All the work should be done at the Mappers because it runs in parallel. Only the final aggregation should take place at the Reducer. The output of the reduce phase will be stored in a single output file.

### 6. MapReduce with Combiner

A Combiner is an optional class that operates by accepting the inputs from the Map and thereafter passing the output key-value pairs to the Reducer. The Combiner has the exact same logic (in most cases) as the Reducer, and it performs the local aggregations at each Map level.

Since it runs along with the Mapper, the Combiner also runs in parallel. Having a Combiner with the Mappers will reduce the amount of data shuffled between Mappers and Reducer.

![[Untitled 5 11.png|Untitled 5 11.png]]

The output of the MapReduce with combiner looks as follows:

![[Untitled 6 11.png|Untitled 6 11.png]]

Source: [https://tutorials.freshersnow.com/map-reduce-tutorial/combiner-in-hadoop-mapreduce/](https://tutorials.freshersnow.com/map-reduce-tutorial/combiner-in-hadoop-mapreduce/)

### 7. MapReduce with Partitioner

A partitioner will partition the intermediate output from the Mappers into different groups. The partitions will be created based on the user-defined function which work as a hash function.

In normal MapReduce application, we will have only one Reducer that aggregates all the data in the final stage but with addition of partitioners each partition will be aggregated by a separate reducer. So, the number of partitioners is equal to number of reducers. Now, Let's discuss a bit about user-defined function for a partition.

In our wordcount example, let suppose we want to create groups of words based on alphabetical order, i.e., all the words starting with letter 'a' should be present in a group and likewise for all the letters. The partitioner will require 26 conditions (each for a letter) to achieve this, and we'll have 26 output files from same num reducers.

MapReduce with Partitioner will look as mentioned below:

![[Untitled 7 10.png|Untitled 7 10.png]]

### 8. Wordcount example in MapReduce

Well, that's a lot of content for a single blog post. I will cover the Wordcount example with great detail in my next post.

### Conclusion

MapReduce is a pioneer big data processing framework. Though it does most of the processing in parallel with Mappers, it is certainly slow due to its frequent disk writes and reads at each stage. Newer and better compute engines like Spark does improve a lot with in-memory computing but the understanding of the core functionality comes from MapReduce.

I've not covered `jobtracker` in this post since it would be incomplete to talk about it without mentioning YARN (An operating system for data applications). We'll discuss about YARN in a separate post.

### Sources

1. [Big Data course](https://trendytech.in/) by [Sumit M](https://www.linkedin.com/in/bigdatabysumit/). All drawn pictures are courtesy of the course
2. [MapReduce Paper by Google](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)
3. [https://www.dummies.com/programming/big-data/engineering/big-data-and-the-origins-of-mapreduce/](https://www.dummies.com/programming/big-data/engineering/big-data-and-the-origins-of-mapreduce/)
4. [https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html)
5. [https://data-flair.training/blogs/hadoop-recordreader/](https://data-flair.training/blogs/hadoop-recordreader/)
6. [https://stackoverflow.com/questions/34871351/hadoop-input-splits-and-record-reader](https://stackoverflow.com/questions/34871351/hadoop-input-splits-and-record-reader)
7. [https://www.tutorialspoint.com/map_reduce/map_reduce_combiners.htm](https://www.tutorialspoint.com/map_reduce/map_reduce_combiners.htm)
8. [https://www.tutorialspoint.com/map_reduce/map_reduce_partitioner.htm](https://www.tutorialspoint.com/map_reduce/map_reduce_partitioner.htm)