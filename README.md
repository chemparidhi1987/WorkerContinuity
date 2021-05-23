# WorkerContinuity
For use case with IndeedFlex

# Open questions
1. I have adopted pandas dataframe to derive the required continuity for the workers data , the same can be achived 
			a) by reading file as a record and with the help of previous records we could derive the same logic (Old fashioned , more processing time) 
			b) we could achieve the same by adopting SQL query functions on Redshift, Athena, Hive which is a simple approach comparitively
2. Review - I would check 
            a) Logic and output 
            b) data reconcilliation  - # of unique workers from source and  # of workers in final file
			c) proper logging mechanism
			d) exception handling without interupting the code flow
			e) reusablity of the functions
			f) driving the entire program with arguments rather than hard coding 
			g) proper comments added to give a clear understanding for each modules
			f) adequate unit testing covering all possible scenarios, integration test bewtween each component along with document for evidence
			g) 
3. For this use case, since we use pandas data frame, csv could be as effective any other columnar format file system like parquet or ORC, but still i could suggest the following
			a) Snappy Compressed Parquet or ORC file system -  Less storage and high efficient
			b) gz compressed file with some encryption(like PGP) when the file is received from other accounts
			c) Instead of file system the data could be retrived directly from SQL database rather downloading onto file, procesing and then again uploading into databases. 
4. Since the file is not very huge, we could adopt many techiques as stated above, but when the data exponentially increases to TeraBytes or PetaBytes, we could switch the solution to a much distributed fashion like hadoop and we can employ distributed framework like spark on top of the data that would execute the tasks in parallel at the same given point of time accross the data nodes.

# Bonus Points
Deployment in a stream oriented Fashion
1. Assuming the files arebeing pushed into an S3 Object, an AWS Lambda can be invoked for every S3 Put Event Trigger
2. This AWS Lambda, will be a communicator to get the file meta details from S3 and can invoke an AWS Glue Job (Python Shell)
3. The code for python submitted (Worker Continuity task) could be setup in AWS Glue and the transformation could be saved in either S3 or directly to a Data Warehouse tools ( Redshift on Cloud or Hive on Hadoop or by creating data lake on S3 to access via Athena/Redshift External tables based on preference)

1. Assuming the data are received as a data Events, rather than file from a streaming source (Kinesis, SQS, NiFi, Kafka) 
2. We could setup a subcription for the polling data as a listeners to the Streaming sources (AWS Lambda, Spark Streaming, AWS Glue Streaming)
3. The transformation could happen accoring to the subcription tools based on preference and could be saved in either S3 or directly to a Data Warehouse tools ( Redshift on Cloud or Hive on Hadoop)

# code deploy and excution
1. copy the tar.gz file into a server
2. untar the file - 
   tar xvzf syft-dataeng-chemparidhi-mullaiveniselvathambi.tar.gz
3. cd continuity
4. sh run-app.sh
5. The output file will be seen in the same folder

# Sample Log
sh run-app.sh
23-05-2021 09:38:48 [INFO] : JOB SUBMIT STARTS HERE FOR
23-05-2021 09:38:48 [INFO] : python3 /home/hadoop/test/tosubmit/continuity/IndeedFlex/main.py --input_file_name /home/hadoop/test/tosubmit/continuity/worker_activity.csv --target_file_name /home/hadoop/test/tosubmit/continuity/results.csv
2021-05-23 09:38:48,859: INFO - Continuity Of Work Python Program Starts Here
2021-05-23 09:38:48,859: INFO - Read CSV File into DataFrame /home/hadoop/test/tosubmit/continuity/worker_activity.csv
2021-05-23 09:38:48,902: INFO - Derive Columsn for Calulations
2021-05-23 09:38:49,104: INFO - Calculate Worker's Continuity
2021-05-23 09:38:49,109: INFO - Write the resultant Dataframe into CSV File /home/hadoop/test/tosubmit/continuity/results.csv
2021-05-23 09:38:49,112: INFO - End of Python Program
23-05-2021 09:38:49 [INFO] : JOB COMPLETED

