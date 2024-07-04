# Kafka Project - Consumer with Spring Boot

This project demonstrates how to create a Kafka consumer using Spring Boot. It listens to messages from the Kafka topic named `AppConstants.LOCATION_UPDATE_TOPIC`.

## Dependencies

- Kafka
- Web (Spring Web)

## Configuration

In your `application.properties` or `application.yml`, configure the Kafka consumer properties:

```properties
server.port=8081
spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=group-1
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

## Kafka Listener

Create a configuration class with a Kafka listener method:

```java
@Configuration
public class KafkaConfig {

    @KafkaListener(topics = AppConstants.LOCATION_UPDATE_TOPIC, groupId = AppConstants.GROUP_ID)
    public void updateLocation(String value) {
        System.out.println("Received message: " + value);
        // Add your custom logic here
    }
}
```