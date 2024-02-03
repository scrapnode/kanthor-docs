---
sidebar_position: 5
---

# Overview

This section provides an overview of the main concepts of Kanthor and how to use them.

## Overall

The fundamental concept of the Kanthor system can be described in a single sentence: a client sends a message to an application, which then generates a request to the corresponding endpoints based on that message. The following flowchart visualizes this idea to help you understand it clearly.

```mermaid
flowchart LR

Client([Client]) -->|message| Application((Application))
Application -->|request| Endpoint_1[https://one.com/wh]
Application -->|request| Endpoint_2[https://two.com/wh]
Application --x|request| Endpoint_3[https://three.com/wh]
```

## Main Concepts

There are _seven_ main concepts that we need to introduce to you

- **Client**: The sender or producer responsible for generating and dispatching `message`s to our **Application**. This includes SDKs in various programming languages, cURL, or the OpenAPI interface.
- **Application**: The primary entry point for the Kanthor system, where `message`s are received and actions are initiated. It handles tasks such as generating `request`s and scheduling them.
- **Endpoint**: Configured destinations (typically URLs, but extendable to other [Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) formats) for generated `request`s. Each endpoint requires at least one **Endpoint Rule** to begin processing requests.
- **Endpoint Rule**: A set of rules determining whether a `message` will be scheduled for sending to an **Endpoint** or ignored.
- **Message**: An entity sent by a client, containing relevant information.
- **Request**: Generated based on a `message` and an **Endpoint**
- **Response**: The outcome returned by an **Endpoint**, indicating the success or failure of processing a `message` and `request`.

All of these concepts will be encapsulated within an entity called **Workspace**. One common use case for **Workspace** is having separate environments, such as UAT and PROD, to prevent interfering with your PROD environment with dummy data.

```mermaid
flowchart TB

Workspace_1[Workspace X] --> Application_UAT((Application - UAT))
Application_UAT --> Endpoint_mot[https://mot.com/wh]
Application_UAT --> Endpoint_hai[https://hai.com/wh]
Application_UAT --> Endpoint_ba[https://ba.com/wh]

Rule_Passthorugh{{Passthorugh}} --> Endpoint_mot
Rule_Filter{{Accept testing.playground}} --> Endpoint_hai
Rule_Ignore{{Ignore}} --> Endpoint_ba


Workspace_1[Workspace X] --> Application_PROD((Application - PROD))
Application_PROD --> Endpoint_one[https://one.com/wh]
Application_PROD --> Endpoint_two[https://two.com/wh]
Application_PROD --> Endpoint_three[https://three.com/wh]

Rule_Users{{Accept user.*}} --> Endpoint_one
Rule_Payment{{Accept payment.*}} --> Endpoint_two
Rule_Orders{{Accept order.*}} --> Endpoint_three

```
