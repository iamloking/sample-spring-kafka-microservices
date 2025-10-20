# Key Learnings

## Table of contents

| No | Content    |
|----|------------|
| 1  | JSON SerDe |


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
