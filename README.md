Streaming Machine Translation on Apache Flink pipelines leveraging Apache OpenNLP

The NMT model has been trained offline using a parallel WMT corpus for German and English, 
uses Byte-Pair Encoding (BPE) to account for unseen vocabulary.
The model has been dockered into Sagemaker as a Flask app - honestly the performance is 
infinitely better if just deployed as an RPC server (gRPC), but... why not try something new.

Given the > 6 mins time to create a Sagemaker endpoint, the fact that all Sagemaker model
 training is batch only, and also that Python is the primary language of Sagemaker - its best to use 
Sagemaker for model inference when invoked from modern streaming pipelines like Kafka Streams and Flink.
For streaming model training - consider looking at --> Oryx2, PredictionIO
or Flink's Queryable State.

Even better and more recent options for Streaming Model Training would be to leverage something like 
Pravega (http://pravega.io) or Flink's Stateful stream processing for realtime model updates.

## Building the Project

1. Fill twitter.properties with Twitter Developer OAuth Creds
2. Deploy the model into Sagemaker as a Docker container - 
    get that here  --> https://amazon.awsapps.com/workdocs/index.html#/folder/e811a3ed006b7bb0b88d46b6010d1d232c21f8f69030dfa56133ee7918bb18fa
   See the Readme file there for instructions on Sagemaker deployment
3. Specify the AWS Sagemaker endpoint and region in aws.properties 
    make sure you have your AWS creds locally
4. Run mvn package to generate a project jar
5. Start a Flink cluster - see https://ci.apache.org/projects/flink/flink-docs-release-1.6/quickstart/setup_quickstart.html
6. From the Flink Dashboard UI - upload the jar generated in Step 2 and submit the same
    Main class = de.dws.berlin.StreamingNmt
    





