# Serialization

## Introduction

Serialization is the process of translating an in-memory data structure into a format that can be stored or transmitted across a network. It is akin to a group of people speaking different languages at a gathering and needing to communicate in a common language for efficient interaction.

In the context of computing, data serialization ensures that data can be efficiently shared between systems and applications. This article explores two popular data serialization formats: Protocol Buffers (Protobuf) and JSON.

- **Why Serialization?**: Serialization is essential for data interchange between different systems and programming languages. It enables data to be transformed into a standardized format that can be easily reconstructed at the receiving end, facilitating communication and data storage.

## Protobuf Serialization

### What is Protocol Buffers?

Protocol Buffers, or Protobuf, is a data serialization format that includes a set of tools for exchanging data efficiently. It separates data from metadata and serializes data into a binary format. Protobuf messages can be transmitted over various network protocols like REST and RPC and are supported by multiple programming languages, including Python, Java, Go, and C++.

- **Binary Format Advantage**: Protobuf's binary format offers significant advantages in terms of efficiency and reduced network bandwidth compared to textual formats like JSON.

### Protobuf Serialization Workflow

1. **Create a proto file**: Define the payload schema, specifying data fields and types.
2. **Compile proto file to language-specific source files**: Use the Protobuf compiler to generate language-specific source files, one for serializing data (client-side) and one for deserializing data (server-side).
3. **Create an executable package**: Compile the generated Protobuf source files with your project code.
4. **Serialize or deserialize data**: Use the generated code to serialize or deserialize data at runtime.

- **Strong Typing**: Protobuf enforces strong typing, ensuring data consistency and reducing the risk of runtime errors.

### Limitations of Protobuf

- **Hard to debug**: Protobuf's binary format is not human-readable, making it challenging to inspect data manually.
- **Extra effort to update the proto file**: Any changes to the data schema require updating the proto file and regenerating code, which can be cumbersome.
- **Limited language support compared to JSON**: While Protobuf supports various programming languages, it may not cover all the languages used in a diverse tech stack.

### When to Use Protobuf

Use Protobuf when:

- Dealing with large payloads to reduce bandwidth requirements.
- Communicating between non-JavaScript environments.
- Expecting frequent changes to the payload schema.

- **Performance Benefits**: Protobuf can significantly reduce latency and improve throughput, especially for large payloads, making it ideal for high-performance applications.

## JSON Serialization

### What is JSON Serialization?

JSON (JavaScript Object Notation) is another data serialization format widely used in computing. It is human-readable, easy to use, and supports various programming languages. JSON offers flexibility in defining data structures but may not perform as efficiently as Protobuf for large payloads.

- **Human-Readable**: JSON's human-readable format makes it accessible for developers and easy to debug.

### JSON Serialization Workflow

1. **Create a JSON object**: Define data structures using JSON's flexible schema, which can easily adapt to changes.
2. **Serialize or deserialize data**: Use built-in functions or libraries in your chosen programming language to convert JSON objects to strings (serialization) or parse strings into JSON objects (deserialization).

- **Simplicity**: JSON's simplicity and wide adoption make it an excellent choice for scenarios where readability and ease of use are crucial.

### Limitations of JSON

- **No support for schema validation**: JSON lacks built-in schema validation, making it challenging to ensure data consistency and correctness.
- **Poor performance for big payloads**: JSON's textual format requires more network bandwidth and computing resources for compression, leading to higher latency and reduced throughput.
- **Backward compatibility problems**: Changing the structure of JSON data can break compatibility with existing clients.

- **Performance Trade-off**: While JSON is easy to work with, it may not be the best choice for applications requiring high-performance data transmission.

### When to Use JSON

Use JSON when:

- Simplicity is a priority.
- High performance is not a critical requirement.
- You need to communicate between JavaScript and Node.js or other environments that naturally support JSON.

- **Versatility**: JSON's versatility and ease of integration with web technologies make it suitable for web-based applications and APIs.

In summary, the choice between Protobuf and JSON for data serialization depends on your specific project needs. Protobuf excels in scenarios with large payloads and non-JavaScript environments, while JSON offers simplicity and versatility, making it suitable for certain use cases. Make an informed decision based on your project's requirements to avoid over-engineering.

## References
* Chatgpt
* https://newsletter.systemdesign.one/p/protocol-buffers-vs-json