# What is Apache Kafka?

Apache Kafka is a powerful **open-source distributed event streaming platform**. Originally developed by LinkedIn, it’s now maintained by the Apache Software Foundation. Kafka is used for building real-time data pipelines and streaming applications.

## Key Concepts

- **Event Streaming**: Kafka captures and moves data (events) in real time. An event could be anything that happens in an application, like a user making a purchase, clicking a button, or a temperature sensor recording a reading.
  
- **Distributed System**: Kafka runs on multiple servers, allowing it to handle large amounts of data with high availability and reliability.
  
- **Scalability and Fault Tolerance**: It can scale to handle large data volumes and continues to work even if some servers fail.

## How Kafka Works

Kafka is based on three main components:

1. **Producer**: The part of the system that sends (produces) data to Kafka. For example, a web application sends user click events to Kafka.

2. **Consumer**: The part that reads (consumes) data from Kafka. For instance, an analytics application reads click events from Kafka to analyze user behavior.

3. **Broker**: The servers that store and manage the data (events). Each piece of data in Kafka is saved in a broker, and multiple brokers make up a **Kafka cluster**.

## Data Organization in Kafka

Kafka organizes data into structures called **topics**:

- **Topic**: A category where messages (data/events) are stored. For example, a topic named `click-events` could hold all user click data.
  
- **Partitions**: Each topic is divided into smaller chunks called partitions. This allows Kafka to spread the load across multiple brokers, making it more scalable.

## Kafka Workflow

Here's a simplified flow of how data moves in Kafka:

1. **Producer** sends data to a **topic** in Kafka.
2. Kafka stores data across multiple **partitions**.
3. **Consumer** reads data from the topic and processes it.

# Apache Kafka Sample Code in Java

1. Set up a Kafka broker (e.g., using Kafka on localhost with default settings).
2. Add Kafka dependencies to your project, such as through **Maven**:

   ```xml
   <dependency>
       <groupId>org.apache.kafka</groupId>
       <artifactId>kafka-clients</artifactId>
       <version>3.0.0</version> <!-- Use the latest version of this dependency -->
   </dependency>
   
## Producer Code

This producer sends a simple message to the test topic.

<pre><code class="language-java">
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.common.serialization.StringSerializer;

import java.util.Properties;

public class SimpleProducer {
    public static void main(String[] args) {
        // Kafka producer properties
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // Create a producer
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

        // Create a message to send to Kafka
        ProducerRecord<String, String> record = new ProducerRecord<>("test-topic", "key", "Hello, Kafka!");

        // Send the message
        producer.send(record, (metadata, exception) -> {
            if (exception == null) {
                System.out.printf("Sent message to topic %s partition %d with offset %d\n",
                    metadata.topic(), metadata.partition(), metadata.offset());
            } else {
                exception.printStackTrace();
            }
        });

        // Close the producer
        producer.close();
    }
}
</code> 
</pre>

## Consumer Code

This consumer listens to messages from the test topic and prints them out.

<pre><code class="language-java">
  import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.common.serialization.StringDeserializer;

import java.util.Collections;
import java.util.Properties;

public class SimpleConsumer {
    public static void main(String[] args) {
        // Kafka consumer properties
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        properties.put(ConsumerConfig.GROUP_ID_CONFIG, "test-group");
        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        // Create a consumer
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(properties);

        // Subscribe to the topic
        consumer.subscribe(Collections.singletonList("test-topic"));

        // Poll for new data
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(1000);
            for (ConsumerRecord<String, String> record : records) {
                System.out.printf("Received message: %s from partition: %d with offset: %d\n",
                    record.value(), record.partition(), record.offset());
            }
        }
    }
}

</code></pre>

## Kafka Use Cases

1. **Real-Time Analytics**: Companies use Kafka to collect and analyze data from websites or IoT devices in real-time.
2. **Data Integration**: Kafka can connect different systems and update them with real-time data.
3. **Monitoring**: Kafka helps track system metrics and alerts instantly if something goes wrong.

## Kafka’s Core Benefits

- **High Throughput and Low Latency**: Kafka can process millions of events per second.
- **Durability and Reliability**: Kafka keeps data for a defined period, even if it’s been read, allowing flexibility in data handling.
- **Scalability**: Kafka can handle growing data sizes by adding more brokers and partitions.

## References

<p>For more information and detailed documentation on Apache Kafka, refer to the following references:</p>
<ul>
<li><a href="https://kafka.apache.org/quickstart">Kafka Tutorial by Tutorialspoint</a></li>
<li><a href="https://docs.confluent.io/platform/current/overview.html">Confluent Kafka Documentation</a></li>
<li><a href="https://kafka.apache.org/documentation/">Apache Kafka Documentation</a></li>
</ul>

## Summary

Apache Kafka is a robust system for managing and moving large amounts of real-time data across applications, databases. It’s ideal for high-speed data handling, data integration, and real-time analytics.
