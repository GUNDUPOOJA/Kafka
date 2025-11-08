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
----------------------------------------------------
-  
- 

































