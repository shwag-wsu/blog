---
layout: post
title: "Enterprise Integration Design Architecture"
categories: Enterprise Architecture, Integration Architecure
---

This design outlines a flexible, scalable integration design approach that supports hybrid clouds. This design levelages a hub and spoke design and does not address point to point integrations.

![enterprise_integration](https://shwag-wsu.github.io/blog/enterprise_integration.png)

- Hub and Spoke design to centralize the integrations to a common design 
- Layered design promotes reusability and clear separation of responsibilities.
- Pub/Sub Messaging ensures decoupling between producers and consumers—scales well.
- Data Consistency layer for data quality
- Security & Governance layers show mature operational readiness.
- Support for hybrid cloud (on-prem + SaaS) integration.
- Inclusion of Boomi and Workato in orchestration offers flexibility for integration use cases.


## Message Layer
### Purpose
Facilitates asynchronous communication between systems using a publish/subscribe model.

## Pub /Sub Messaging
Event-driven, loosely coupled, multi-listener.


### Components
- **Publisher**: Publishes data events to the message bus.
- **Subscribers**: Consume relevant messages based on topic or event type.

### Patterns
There are several patterns to use in a pub / sub plaform I prefer the One-to-Many option defined below:

#### One-to-Many
A single or multiple publisher applications publish messages to a single topic. This single topic is attached to multiple subscriptions. Each subscription is connected to a single subscriber application. Each of the subscriber applications gets the same set of published messages from the topic. When a topic has multiple subscriptions, then every message has to be sent to a subscriber receiving messages on behalf of each subscription. If you need to perform different data operations on the same set of messages, fan out is a good option. You can also attach multiple subscribers to each subscription and get a load-balanced subset of messages for each subscriber.

![one_to_many](https://shwag-wsu.github.io/blog/one_to_many.png)

### Important Considerations:
Using Pub / Sub messaging there are some common pitfalls to be aware of they are listed below.
- Message Ordering
- Loop Protection

#### Message Ordering
Messages for the subscriber will need to be processed in the order in which they are recieved.  This is to makes sure the correct changes are applied in order this is accomplished by Enable message ordering in the subscription.  Here's a more detailed explanation:

- 1. Enable Message Ordering:
When creating a subscription, enable the "Message ordering" property. This tells Pub/Sub to enforce ordering for messages associated with the same key. 
- 2. Use Ordering Keys:
When publishing messages, include an ordering key. This key will be used to group messages together for ordered delivery. 
- 3. Publish in the Same Region:
Messages with the same ordering key must be published in the same region to ensure ordered delivery. 
- 4. Subscriber Behavior:
Subscribers will receive messages with the same ordering key in the order they were published, even if there are multiple subscribers to the same topic. 

##### Additional Considerations for Ordering:
- Ordering is not guaranteed across different keys:
Ordering is only enforced within the same ordering key. Messages with different keys may be delivered out of order. 
- Published in a different region:
If you are publishing messages with the same ordering key to different regions, then it's not possible to guarantee the order across those regions. 
- Exactly-once delivery:
While message ordering ensures messages with the same key are delivered in order, it's still possible for messages to be delivered more than once, especially with exactly-once delivery enabled. 

By following these steps, you can ensure that your subscribers receive messages in the order they were published, even if multiple subscribers are consuming the same topic. 

#### Loop Protection

 loop protection refers to mechanisms to prevent messages from circulating indefinitely within a distributed system, which can lead to duplicate delivery and resource exhaustion. This is particularly important in scenarios involving proxy subscriptions, where messages can propagate through a network of queue managers. Here is a recommended standard message envelope:

#### Prevention Strategy
To prevent the messages from circulating indefinitely within a distributed system there are a couple different step to implement within the pub/sub platform.

- 1. Use Event Metadata to Track Origin
  Here is a recommended standard message envelope:
  Add metadata to each message, such as:
  sourceSystem: e.g., Salesforce, BillingSystem, Workday
  correlationId: for traceability
  processedBy[]: a list or hash of systems that have touched this message

  
 {% highlight json %}
 {
  "messageId": "uuid-12345-abc",
  "source_system": "workday",
  "processed_by": ["workato", "boomi"],
  "correlationId":[{"workday":"wd-12345"},{"workato":"w-145365"},{"boomi":"b-565213"}],
  "timestamp": "2025-04-29T10:00:00Z",
  "event": {
    "type": "employee.updated",
    "payload": {...}
  }
}
{% endhighlight %}
 <b>How to use it:</b>
  When an event is received, the integration flow checks sourceSystem or processedBy and skips or logs if it already processed it.
  You can drop or flag the message if it’s circular.
  In Workato or Boomi, this is usually done with:
  Conditional steps (e.g., “Only run if source ≠ 'BillingSystem'”)
  Lookup tables or audit logs for recent event IDs

- 2. Idempotency at Destination Systems
Make sure downstream APIs (like Salesforce, Workday) are idempotent, meaning:
Repeating the same update doesn’t cause changes or duplicates.
For example, updating a customer with the same data is a no-op.
This doesn’t stop the loop, but it reduces the impact if it happens.

- 3. Filtering Logic on Subscriptions
At the Pub/Sub subscription level, apply filters:
Only subscribe to events relevant to the system (e.g., eventType = OrderStatusUpdate)
Exclude messages originated by the system itself (via metadata)
Some systems like GCP Pub/Sub, Kafka Streams, or AWS EventBridge support native filtering.

- 4. Use Message TTLs or Hop Limits
Add a TTL (time to live) or max hops count to messages:
After a certain time or number of handoffs, the message is discarded or quarantined.
This prevents zombie loops and makes debugging easier.

### Tech Stack Examples
- Kafka, <b>Google Pub/Sub</b>, AWS SNS/SQS, RabbitMQ

## Orchestration Layer

### Purpose
Coordinates business workflows and integration processes using automation platforms.

### Components
- **Workato Recipes**: No-code/low-code flows optimized for business users.
- **Boomi Flows**: Enterprise-grade, scalable workflows for complex integrations.

### Responsibilities
- Execute business logic across systems.
- Manage sequencing, conditional logic, error handling.

### Best Practices
- Design modular flows for reuse.
- Include fallback and alerting mechanisms.

## Data Consistency Layer

### Purpose
Ensures data consistency across systems via polling or event-driven techniques.

### Components
- **Change Data Capture (CDC)**: Tracks real-time data changes.
- **Polling**: Periodic checks for data changes in non-event sources.

### Best Practices
- Prefer CDC for low-latency and efficient change detection.
- Use timestamp or checkpoint-based polling to reduce load.

## Security and Governance Layer

### Purpose
Enforces policies around authentication, data protection, and compliance.

### Components
- **Authentication/Authorization**: OAuth2, SAML, OpenID
- **Encryption**: Transit (TLS/SFTP), At-rest (PGP)
- **Secrets Management**: Secure handling of API keys, tokens
- **Data Masking**: Sensitive data obfuscation

### Governance Inclusions
- Centralized audit trail and logging
- Enforcement of access controls

## Governance Layer

### Purpose
Supports transparency, traceability, and maintainability of integration flows.

### Components
- **Logging**: Centralized log management for observability.
- **Audit Trail**: Change tracking for compliance.
- **Reusable Components**: Common connectors, transformation scripts, validation routines.

### Standards
- Use correlation IDs across logs for traceability.
- Enforce logging policies for all components.

## 🤖 Boomi vs. Workato in This Design

| Feature                    | **Boomi**                                   | **Workato**                                |
|---------------------------|----------------------------------------------|--------------------------------------------|
| **Target Users**           | IT & technical teams                         | Business users & citizen developers        |
| **Use Cases**              | Complex integrations, ETL, backend workflows | Quick automations, SaaS integrations       |
| **Customization**          | High (via scripting, components)             | Moderate (drag & drop, limited code)       |
| **Governance**             | Strong enterprise governance tools           | Improving, more limited in complex orgs    |
| **Speed of Deployment**    | Medium                                        | Fast                                        |
| **Integration Templates**  | Available but requires customization         | Rich library, often plug-and-play          |
| **Best Fit In This Design**| System-to-system or backend orchestration    | SaaS-to-SaaS or user-facing process flows  |

### Summary:
- <b>Boomi</b> is better suited where deeper control, advanced error handling, or hybrid scenarios are needed.
- <b>Workato</b> excels in rapid automation and user-friendly design for business-led use cases.

