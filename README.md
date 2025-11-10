#### Why and How to Use Kafka
-----------------------------
- Kafka is an open source distributed(works on cluster of machines) event streaming platform (deals with streaming data)
- Kafka offers 2 main things:
  1. It offers data integration/collection capabilities (data integration means you can collect data which is coming with increasing pace - 1000 events/per sec quickly collect it and store it)
  2. Data processing (kafka streams help to process the data)

- Socket isn't something to be used in production because its not a replayable source, if we have a fast producer and slow consumer, can't hold the data
- socket = IP +port
- Lets say: producer is producing at 1000 records/sec, consumer is consuming 100 records/sec  - socket doesn't have a buffering capability to hold the data - so we have a data loss - we can't have multiple consumers (if 1 consumer takes data its gone, its not replayable)
  
- By default, the data in Kafka can be retained for **7 days** (we can increase or decrease it ) 
- producer and consumer can be decoupled = That means producer doesn't worry about how fast the consumer is able to consume nor consumer should worry about how fast producer is producing the data, both producer and consumer work at own pace, there can be 1 producer and multiple consumers.
- Ex : IoT devices, tractor(collect seeds, crops) - has sensor - collect data - 1000 devices
- you need a streaming source which can collect streaming data coming at increasing pace

- Kafka to use only for buffering/collection purpose and process it using spark structured streaming
-<img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/4c1a4533-dcad-49d4-91ae-dae4e4c2b401" />
- Kafka can be use it as a source/sink
- **Kafka acts like a pub-sub model (publisher-subscriber messenging systems)**
- one publish the data, then you subscribe it, in Kafka we have **topic** - you publish the data to the topic and there would be consumer who subscribe to that topic and consume the data - more like a messaging system.
  
- **PRODUCER -> KAFKA CLUSTER -> CONSUMER**
- **producer**: producers data and publish to kafka topic
        1. Twitter application - produces new tweets every sec
- **Kafka cluster** : data is kept in kafka cluster
- **consumer** : consumes the data (Ex: spark streaming)
- **Topic** : is like a table in a db (Ex: A unique name which holds particular kind of data) - Tweets data, banking, employees
            1. If a single mchne holds tweets data, as new tweets coming in, one machine is not capable enough to handle data, so we have concept of partitions
           2. A topic can be divided into many partitions, each partition can be stored on different machines,this makes the system distributed and scalable
           3. Number of partitions is a design time decision, here is no block size like 128 mb in hadoop
           4. whenever we are creating a topic, we need to define the number of partitions
           5. As a architect, we have to decide partitions based on size of data
- **Broker** : A node running kafka software - if we say 10 brokers are running means 10 node cluster is running.
- **Cluster** : made up of various brokers ()
- **Partition** : A topic is divided into number of partitions, we have to decide on the number of partitions when we are creating a topic.
- **Partition offset** : Inside partition, the messages are stored in sequence, we have sequence id.
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/a2580d47-b4b4-4cc4-9bd9-3ebcebd1654e" />
- **Consumer group** : we can have more than 1 consumer to share the load (role is to take the data and put it to a file system)
      1. Ex: Lets assume you have walmart store there are 1000's of billing counters  across the world, your role is to get all the generated invoices and records and dump them to a file system (datacenter)
      2. producers - billing counters producing the invoices
      3. These all will go to kafka cluster (assume 100 brokers)
      4. If we just have 1 consumer to consume it and put it to a file system - its not scalable, single customer will be overloaded
      5. In such cases - go with multiple consumers which share the load
  
- **One partition cannot be handled by multiple consumers**
- **A consumer can take multiple partitions**
- If we have 4 partitions and 2 consumers - each consumer works on 2 partitions which is perfect.
  
To read a particular message in kafka cluster what details do we need
-----------------------------------------------------------------------------
- Topic name
- offset
- partition number

As a DE - know Working with producer APIs and Consumer APIs
------------------------------------------------------------
- As a developer you should learn how to produce the data and how to consume data
- someone said, data is residing in kafka topic now process it using spark structured streaming, then you should know how to consume data using spark structured streaming
- kafka topic is the source of your data - after processing also you should also keep the data in kafka topic - you would produce data to the kafka - here kafka acts like a sink
- Kafka cluster configuration takes care by admins, as a DE its ok to know few things about kafka cluster, internals of kafka, producer API and consumer API

Kafka Cluster
------------------
- **kafka cluster is made of brokers**
- Lets say I want a 4 node cluster - taking 4 machines and installing broker software on 4 machines
- A machine running broker software is part of kafka cluster
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/0e1d5a52-de14-4332-af0c-c016de1a8834" />
- producer is producing the data to a kafka cluster that has 4 brokers and consumer will be consuming the data
- best place to practice kafka is confluent.io -> confluent is a company developed by developers of kafka
- **confluent is managed kafka on cloud**

Kafka as a Data Integration platform and basic terminologies
----------------------------------------------------------------
- Leverage kafka as a data integration platform
- Like Kafka, there are traditional messaging systems - like RabbitMQ, ActiveMQ kafka is slightly different from those
- In traditional messaging systems - if there is 1 producer which produces data, if consumer consumes data, data is gone, no else can consume it
- But in kafka we can have multiple consumers, data will still be kept intact even if 1 consumer consume data (1 producer - multiple consumers)
- Data is retained for a period of 7 days

Practice Kafka
--------------------
- Kafka practice in confluent website
- Create a environment (there is default environment) , dev,stage, prod
- Inside environment you create clusters
- default -> cluster1, cluster2
- Inside cluster - you create a topic
- **environment ->  cluster -> topic**
  
- **Every message needs 2 things : key and value (Timestamp takes automatically)**
- In orders.json take first order_id and format it properly **using Json formatter website**
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/405d9c1c-987b-4a0e-9815-3362928a78b1" />
- Make customer_id as the key so same customer_ids fall in same partitions
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/5c906fbb-48f5-4fac-9553-491f8cbbf278" />
- In key field give 11599, value (copy all the json)
- Here partition is 0 and offset is 0, this is calculated based on a hash function
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/0b7ea606-da9c-4672-975c-83ce2185ae7f" />
- we can also do the above same process using cli commands (check confluent website)
- we have seen Graphical way of working on kafka, cli way how to connect to environment, create env, connect to cluster, create topic, reading topic, starting a consumer, starting a producer, create diff number of partitions

Programmatic Approach of Implementing Kafka Retail Use-Case
--------------------------------------------------------------------
- Industries use programmatic approach
- consider there is a retail chain: walmart store
- A lot of transactions are happening over the walmart store - if you make a order there will be multiple line items because one order contains multiple items
- when someone is making a transaction, this retail organization sends this transaction to a kafka topic
- since we don't have machines which generate transactions and all 
- we are simulating the same approach, we have a file, I'll read it line by line, produce it to my kafka topic.
- we will write a python code (python producer) that takes the data in the file line by line and produce it
- someone asked to do analysis on customer_id, we want output like
- customer_id    total_orders    total_line_items    total_amount
- 11599             5                         18        2000

- Open Pycharm - create a new project (kafka project) - create a new file(myproducer.py)
- Fill the config values
- bootstrap server value -> get it from your kafka cluster settings
- username, pw -> get it from API Keys section
- **Asynchronous - if we are producing 200 messages, it will not wait for first message to get produced and wait for acknowledgement, it will keep producing messages.**
- ``` producer.produce(topic: "retail-data-new",key=customer_id,value=customer_details, callback = acked) ```
- In case if we want to do something - we can call callback method and write something say if a message is successful, we can show a message
-**callbacks** - is to check and post the acknowledgements
- when we put something to kafka topic it will serialize it, that means it will convert to bytes and put it across, it goes in a serialized manner, we have to convert it to str(msg)
- whenever we use a callback, we should be using a poll
- ```producer.poll(1) 1 is timeout```
- whenever we call the produce(), it puts the message in the producers pending buffer, it will asynchronously dispatch those messages, what it will then not do by itself is called callback functions
- when you call poll the producer checks for asynchronous events that needed to be processed. These events include acknowledgement from brokers for message sent, error
  
- To initiate sending message to kafka, call produce method passing in a message value its mandatory (topic,key, value, callback method, in which partition we want to put)
- **flush should be called before shutting down the producer to ensure all outstanding/queued/in-flight messages in transit are delivered**
-
```
from confluent_kafka import Producer
import json
conf = {
    'bootstrap.servers': 'pkc-abcd85.us-west-2.aws.confluent',
    'security.protocol': 'SASL_SSL',
    'sasl.mechanism': 'PLAIN',
    'sasl.username': '<CLUSTER_API_KEY>',
    'sasl.password': '<CLUSTER_API_SECRET>',
    'client.id': 'pooja windows'
}

producer  = Producer(conf)

customer_id = "11599"
customer_details = '{"{"order_id":1,"customer_id":11599,"customer_fname":"Mary","customer_lname":"Malone","city":"Hickory","state":"NC","pincode":28601,"line_items":[{"order_item_id":1,"order_item_product_id":957,"order_item_quantity":1,"order_item_product_price":299.98,"order_item_subtotal":299.98}]}"}'

def acked(err,msg):
    if err is not None:
        print(f"Failed to deliver message: {err}")
    else:
        msg_key = msg.key().decode('utf-8')
        msg_value = msg.value().decode('utf-8') #to remove b in the output added decode
        print("Message produced:" key is {msg_key} and value is {msg_value}")

with open('/user/desktop/trendytech/orders_input.json', 'r') as file:
for line in file:
     order = json.loads(line) #mapping the each line to a json to extract each field out of it
    customer_id = str(order["customer_id"])
    producer.produce(topic: "retail-data-new",key=customer_id,value=line)

producer.poll(1)

producer.flush()
```

what if we want to read a file and iterate it line by line and produce each thing as a message
--------------------------------------------------------------------------------------------------
- check the above modified producer code

- so far we have seen writing a producer code, that takes data line by line from a file and write to kafka topic

- **Bootstrap servers**: if we have 100 node cluster, no need to give all broker list, give 3-4 list and atleast if one fails, it can route back to 1st one, as soon as the client connects to one it gets all metadata - all brokers available, topics, location of brokers

Connecting to Kafka topic - Read and persist the data 
---------------------------------------------------------
- so far we have seen how do we produce data to a kafka topic, we have seen plain python producer and we did it from a local system
- we set various configurations and try to write it using producer API
- In this producer, we have this file - 57431 lines and take each line at a time, write it as a key value pair
- kafka topic name is : retail-data-new
- write a consumer program to read the data from topic, put it to a disk or table so the data will be persistent, so we can use it later
- open databricks cluster - we have to talk to confluent from here - click on libraries - install new - select PyPI - in package mention this
- ```confluent-kafka[avro,json,protobuf]>=1.4.2```
- Now open a new notebook - refer kafka consumer notebook
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/f8cebdff-a582-4ad6-8786-6dd73be2a5cb" />
- There are 2 timestamps
  1. **event timestamp** - time at which actual event is generated
  2. **timestamp type** - when the event comes to kafka broker
- key and value fields are in binary format, so we can't read it

How to convert the above code to streaming (Logic to persist the data in Delta table from kafka source)
-----------------------------------------------------------------------------------------------------------
- we are going to write a streaming job that connects to kafka source and we write it back to a delta table
- Source -> Kafka
- Target -> Delta table
- Refer kafka consumer streaming notebook
- If you see the table the value field has multiple line items, while reading from the kafka source itself we could have clean and flatten the data, so that clean data would be stored in delta table
- <img width="300" height="300" alt="image" src="https://github.com/user-attachments/assets/95f5dd5b-1bfe-40af-b02b-29974685dcae" />

Persist cleaned and formatted data to delta table
--------------------------------------------------
- Refer order streaming2 notebook

Kafka producer using pyspark
---------------------------------
- Here will see how to write producer using pyspark
- someone might tell you just process this data using kafka sync - we will take it from topic
- Ideally the flow can be - you read data from kafka topic (write own pyspark consumer) - write the results to bronze layer(persistent storage - dbfs, hdfs, delta format)
- you should take this persistent storage - do processing filtering, aggregations and then you can write to a kafka topic
- earlier we thought terminals produce data -> wrote python producer (produces data to a topic) -> read data from a topic( then we use pyspark kafka consumer) -> put it to a persistent storage -> now we are doing processing -> to put the data back (pyspark kafka rather than plain python)
- we can read from persistent storage do filtering on the go and write it back
- But here we are reading from consumer - do filtering and then write it back
- Filtering criteria is where city = 'chicago'
- Refer kafka producer streaming notebook
- 

































































