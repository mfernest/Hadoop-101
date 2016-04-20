# Why Hadoop?

### Let's cut right to the chase!

* Hadoop offers a low-cost approach to storing and processing large amounts of data. 
* Perhaps you're already doing something big with a lot of data
  * The cost of storing and processing that data may limit the amount you keep and use
  * The various systems you use may be difficult to integrate and coordinate
  * The types of systems you use may impose constraints without much benefit

--

### But your legacy systems aren't just going away:

* You understand them well
* In many case they have just the features you need
* Moving data to new systems isn't necessarily easy, fast, or protected
* Exploring new technology and lower costs can't bring in unacceptable risks

--

### So What About Hadoop?

* A distributed, write-once storage system (HDFS)
* Highly-generalized data processing frameworks (MapReduce, Spark)
* Tools for ingesting data (Flume, Sqoop, Kafka)
  * We mention these for reference only
* Tools for writing jobs in familiar forms
  * Hive: turns relational queries into MapReduce jobs
  * Pig: turns script-like queries into MapReduce jobs
  
--

### Sixty-second HDFS Review

* Stores files over any number of worker nodes in a cluster
  * Large files are broken into blocks and distributed among the nodes
  * Blocks are replicated for redundancy and locality
  * Processing an HDFS file may occur in parallel among all its blocks
  * In general, more nodes means greater parallelization for large files
  
--

### Sixty-second MapReduce Review

* Distributes processing code to each node
  * Best case, every compute node has data on the same machine (locality)
  * This initial phase is called mapping
  * All mapped results are keyed in some way
* If a job has one or more reducers, the key determines which reducer processes a mapped result
  * Each reducer pulls and sorts its mapped data, at a minimum
  * The reducer code may then operate on its sorted data
* By design, this is a very general framework with many possible applications 
  
--

### One MapReduce Example: WordCount

* Let's say we store the all the works of Shakespeare on HDFS
* We want to know how many times Shakespeare has used each word
* We can map each play or poem as a file
  * Break every line into words (`And you all know, security / Is mortals' chiefest enemy`).
  * Use each word as a key, and give it a value of 1 (e.g., `(chiefest, 1)`)
* We can reduce all the data by sorting it and counting each key
  * Once reducer would give us the whole list
  * Several reducers would divide the keys and each count its sorted subset
* The process is always the same: the amount of data doesn't matter
  * Speed can be improved, for large amounts of data, by adding worker nodes

--

## So How Would We Do This?

* Load text files into HDFS
* Find a MapReduce program for counting words (or write one)
* Submit the program, parameters, job properties, and the file(s) to count
  * Specify a new location for output
  * Sit back and wait!
* You can do this with many tools
  * Unless you want to process tens of TBs or more at a time 
  * And you can't wait days or weeks for the result
  * And you want to keep costs down
  * And you want to be able to scale your system for growth
  
--

## Other Jobs That Can Be Made Into MapReduce

* Downloading tables from a database: Sqoop does this
* Queries: Hive helps you with this
* ETL workflows: Oozie can orchestrate MapReduce and other actions together
  * Spark is becoming a popular tool for this as well
* Data Analytics: Hadoop provides tools, or sometimes common storage for this

--

## What Can We Try Right Now?

* We'll have you each log into a HUE portal
  * We have very small clusters available, so we'll need to spead you out
  * We'll review the HUE interface together
  * You'll explore operations with the File Browser
  * You'll run a short-running stub job and observe it in the Job Browser
  * You'll tour the other facilities available in HUE

--

## First, Some Review Questions for the Audience

* What is the default replication level for an HDFS file?
  * How many nodes are required to support it?
* What is the default block size for HDFS data?
  * (From now on, let's call it a block limit)
* So, how many times would a 20 MB file get replicated?
* How many blocks would it take to hold a 100 MB file?
* How many reducers are required for a MapReduce job to run?
* 

--

## Lab Work - HDFS Browsing 

* Login to HUE with your assigned username
  * They will be of the form `bcbs_##`
  * The password will be `cloudera`
* Upload five or six files to HDFS
  * Nothing too large -- we have limited bandwidth to these clusters
  * You might like to try different types: text, PDF, a Word doc, etc.
* Review the folders in the `/user` directory
  * What is the folder name underneath `/user/hive`?
* Click the Home link in the File Browser

--

## Lab Work - HDFS Operations 1

* Click on any uploaded file to view its contents
  * Your browser may invoke a viewer for the content (such as a PDF doc)
  * You can navigate to HUE with a back arrow if necessary
* Try browsing other folders in HDFS -- observe where you can or can't go  
* After exploring a bit, go to your Home folder and click the `History` button
* Try creating a new file or folder through the HUE interface

--

## Lab Work - HDFS Operations 2

* Select one of your uploaded files and Open the `Actions` dropdown box
  * Change the permissions of the file so only you can read it
* Select another of your uploaded files and Click the button `Move to Trash`
  * Confirm the delete operation
  * Notice the folder `.Trash` appears in your Home folder
  * Click this folder and subfolders until you see your file
  * What is the full path to your file from the Home folder?
  * Select this file and click the `Restore` button
* Select another of your uploaded files (a small one)
  * This time, click the drop-down arrow on the right of the `Move to Trash` button
  * Choose the option to `Delete Forever`
  * Confirm the file has not been relocaed to `.Trash` 
* Select another of your uploaded files (a large one)
  * Under the `Actions` button, select `Summary`
  
--
  
## Lab Work - Understanding a Job

* Choose the Job Browser web app from HUE's control bar (to the right)
  * If this is your first visit, it will be empty
* Mouse over the Home icon in HUE's control bar (to the left)
  * The tool tip should read `My Documents`
  * Click the icon
  * You should see a long list of objects available to use
* In the search bar near the top of the page, enter `MapReduce`
  * This search will limit the page contents to two items
* Choose the item called just `MapReduce`
  * Clicking through takes you to a `Job Designer page`
  * You're just here to look around for now
  * Notice the JAR path property, indicating where the MapReduce code lives
    * Make note of the full path to the JAR
  * There are also many MapReduce Job Properties listed
    * These parameters provide the execution constraints for the job

--

## Lab Work - Setting Up a Job

* Go back to `My Documents` and enter `Sleep jobs` in the search bar
  * A single item should appear -- click on it
* You are now in an Oozie Workflow editor
  * Oozie sees a MapReduce job as one of several possible 'actions'
  * You can hook together actions to form a workflow
  * You can then schedule a workflow to run at a certain time
* This Workflow editor displays one action, a MapReduce Sleep job, that can run on its own
  * You can explore the features in this tool, but there are no lab steps at present to assist you.
  * Right now we just want to execute and observe a job in progress
  
--

## Lab Work - Executing a Job
  
* In the MapReduce job box, click the small "play" button in the upper-right corner
  * In the dialog box that pops, choose a sleep time (in seconds) for your job
* Once the job is launched, you'll be taken a console that includes tabs for:
  * Graph
  * Actions
  * Details
  * Configuration
  * Log
  * Definition
* Take some time to browse these tabs
* You can run these jobs several times with different reducer wait times
  * Notice the common features and artifacts of any correctly-running job
* When we resume, we'll discuss some of these features together and take questions.
