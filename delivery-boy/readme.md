# Kafka Project - Producer with Spring Boot

This project demonstrates how to create a Kafka producer using Spring Boot. It publishes messages to a Kafka topic named `AppConstants.LOCATION_TOPIC_NAME`.

## Dependencies

- Kafka
- Web (Spring Web)

## Configuration

In your `application.properties` or `application.yml`, configure the Kafka producer properties:

```properties
spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

## Creating a Kafka Topic

To create the Kafka topic, use the following method in your configuration class:

```java
@Bean
public NewTopic newTopic() {
    return TopicBuilder.name(AppConstants.LOCATION_TOPIC_NAME).build();
}
```

## Kafka Service

The `KafkaService` class sends messages to the Kafka topic:

```java
@Service
public class KafkaService {

    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    private Logger logger = LoggerFactory.getLogger(KafkaService.class);

    public boolean updateLocation(String location) {
        this.kafkaTemplate.send(AppConstants.LOCATION_TOPIC_NAME, location);
        logger.info("Message produced: " + location);
        return true;
    }
}
```