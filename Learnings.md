# Key Learnings

## Table of contents

| No | Content        |
|----|----------------|
| 1  | JSON SerDe     |
| 2  | StreamsBuilder |
| 3  | KStream        |


### JSON SerDe (Translator b/w Java Objects and Json Text)
1. It stands for JSON Serialization and DeSerialization
2. Every software component has its own way of communicating.
3. Java communicates with objects, whereas Kafka, APIs communicates in JSON (text format).
4. To bridge this gap we make use of JSON SerDe. It’s a mechanism that converts Java objects ↔ JSON strings — i.e., how data moves between in-memory objects and text-based JSON messages
5. Serialization - Converts objects into json strings.
6. DeSerialization -  Converts json strings into objects.
7. SerDe - it bundles serialization & deserialization logic for a specific data type — ensures consistency and type-safety.
8. `JsonSerde<Order> orderSerde = new JsonSerde<>(Order.class);`
9. | Concept          | Meaning                                          | Example Class      |
   | ---------------- | ------------------------------------------------ | ------------------ |
   | **Serializer**   | Java → JSON                                      | `JsonSerializer`   |
   | **Deserializer** | JSON → Java                                      | `JsonDeserializer` |
   | **SerDe**        | Bundle of both                                   | `JsonSerde<T>`     |
   | **Purpose**      | Translation between Java objects and JSON format | Kafka, APIs, etc.  |


### StreamsBuilder (Blueprint for Kafka Streams Topology)

1. It is the entry point for building a Kafka Streams topology (the logical flow of data processing).
Think of it as a builder/factory class that lets you define how data moves between Kafka topics and how it’s processed.

2. It doesn’t execute anything itself — it describes what should happen (like a pipeline definition). Execution happens when the KafkaStreams object starts.

3. You use it to create streams (KStream) and tables (KTable) from Kafka topics.

4. Once you define all transformations and joins using StreamsBuilder, you call builder.build() to create a Topology.

5. This topology is then passed to a KafkaStreams instance, which actually runs the processing logic.

6. Example:
    ```
    StreamsBuilder builder = new StreamsBuilder();
    KStream<Long, Order> stream = builder.stream("orders");
    ```


### KStream (Continuous Flow of Real-Time Records)

1. KStream represents a continuous, never-ending flow of key-value records — think of it as a real-time event pipeline.
2. Conceptually, it’s similar to a Java Stream, but instead of processing data in memory, it processes Kafka topic messages as they arrive.

3. Created using StreamsBuilder:
`KStream<Long, Order> stream = builder.stream("payment-orders", Consumed.with(Serdes.Long(), orderSerde));
`
- "payment-orders" = Kafka topic name
- Serdes.Long() = key serializer/deserializer
- orderSerde = value serializer/deserializer

4. Once a KStream is created, you can apply various transformations to it — map, filter, branch, join, etc.
It’s immutable — each transformation returns a new stream. The original KStream remains unchanged.
Common operations:
   - join() → join with another stream or table
   - peek() → inspect data (useful for logging/debugging)
   - to() → send output to another Kafka topic

5. Example (from your code):

```aiignore
stream.join(
builder.stream("stock-orders"),
orderManageService::confirm,
JoinWindows.of(Duration.ofSeconds(10)),
StreamJoined.with(Serdes.Long(), orderSerde, orderSerde))
.peek((k, o) -> LOG.info("Output: {}", o))
.to("orders");
```


Joins two streams within a 10-second window.
Applies a custom logic (orderManageService::confirm) to combine events.
Logs output using .peek().
Sends joined results to the orders topic.