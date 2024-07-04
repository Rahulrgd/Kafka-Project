# Kafka Project with Spring Boot

This project demonstrates how to create a Kafka-based system with two modules: `delivery-boy` (producer) and `end-user` (consumer).

## Modules

### 1. `delivery-boy` (Producer)

The `delivery-boy` module produces location updates and pushes them to a Kafka topic. It simulates a delivery person updating their location.

#### Dependencies

- Kafka
- Web (Spring Web)

#### Configuration

In your `application.properties` or `application.yml`, configure the Kafka producer properties:

```properties
spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

#### Kafka Producer

Create a service class to send location updates:

```java
@Service
public class DeliveryBoyService {

    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    public void updateLocation(String location) {
        this.kafkaTemplate.send("delivery-boy-location-topic", location);
        // Add your custom logic here
    }
}
```

### 2. `end-user` (Consumer)

The `end-user` module reads messages from the Kafka topic and processes them. It represents an end user tracking delivery updates.

#### Dependencies

- Kafka
- Web (Spring Web)

#### Configuration

In your `application.properties` or `application.yml`, configure the Kafka consumer properties:

```properties
spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=end-user-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

#### Kafka Consumer

Create a service class to consume location updates:

```java
@Service
public class EndUserService {

    @KafkaListener(topics = "delivery-boy-location-topic", groupId = "end-user-group")
    public void processLocationUpdate(String location) {
        System.out.println("Received location update: " + location);
        // Add your custom logic here
    }
}
```