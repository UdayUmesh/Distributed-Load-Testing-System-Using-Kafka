# Distributed Load Testing System Using Kafka

## Goal
The objective of this project is to design and implement a distributed load-testing system that coordinates between multiple driver nodes to run a highly concurrent, high-throughput load test on a web server. This system leverages Kafka as a communication backbone.

## Architecture
- The system consists of two main components:
  - **Orchestrator Node**
  - **Driver Nodes**
- The **Orchestrator Node** and **Driver Nodes** communicate via Kafka topics to synchronize load tests, exchange metrics, and monitor health.
- Each node registers itself with the **Orchestrator Node** and communicates using Kafka.
- Nodes can be added or removed as needed for scalability.

### Architecture Diagram
![image](https://github.com/user-attachments/assets/191101ef-080b-4687-a083-c5192c7424e8)

## Features
### 1. Load Testing Types
- **Tsunami Testing**: Each request is sent with a defined delay interval between them.
- **Avalanche Testing**: All requests are sent as soon as possible in a first-come, first-served manner.
  
### 2. Target Throughput
- The user can specify a target throughput per driver node (X requests per second).
- The system respects this limit and executes the load test accordingly.

### 3. Test Configuration
- **Test Type**: Avalanche or Tsunami.
- **Delay**: Optional, only for Tsunami Testing.
- **Request Count**: Number of requests per driver node.

### 4. Observability and Metrics
- The system aggregates metrics such as **mean**, **median**, **min**, **max** response latency across all driver nodes.
- The **Orchestrator Node** tracks the number of requests sent by each node and displays the metrics on a dashboard.
- Metrics are updated every second.
  
### 5. Scalability
- Supports a minimum of 1 orchestrator and 2 driver nodes, with a maximum of 1 orchestrator and 8 driver nodes.
- Nodes can be added or removed between tests to scale the system as needed.

## Components

### 1. Orchestrator Node
- Exposes a REST API for controlling and monitoring tests.
- Coordinates load tests by sending configuration messages to driver nodes.
- Collects and displays metrics from driver nodes.
- Stores metrics in a local metrics store.

### 2. Driver Node
- Receives load test configuration from the orchestrator node.
- Sends requests to the target web server based on the load test type (Tsunami or Avalanche).
- Collects response metrics and sends them to the orchestrator node via Kafka.

### 3. Target Server
- Implements a basic HTTP server for testing purposes.
  - **/ping** endpoint to simulate requests.
  - **/metrics** endpoint to view request statistics.

Sample target server implementation: [GitHub Link](https://github.com/anirudhRowjee/bd2023-load-testing-server)

## JSON Message Format
### 1. Node Registration
```json
{
  "node_id": "<NODE ID>",
  "node_IP": "<NODE IP ADDRESS>",
  "message_type": "DRIVER_NODE_REGISTER"
}
```
### 2. Test Configuration
```json
{
  "test_id": "<RANDOM TEST ID>",
  "test_type": "<AVALANCHE|TSUNAMI>",
  "test_message_delay": "<CUSTOM_DELAY>",
  "message_count_per_driver": "<NUMBER OF REQUESTS>"
}
```
### 3. Trigger Load Test
```json
{
  "test_id": "<TEST ID>",
  "trigger": "YES"
}
```
### 4. Metrics Report
```json
{
  "node_id": "<NODE ID>",
  "test_id": "<TEST ID>",
  "report_id": "<REPORT ID>",
  "metrics": {
    "mean_latency": "",
    "median_latency": "",
    "min_latency": "",
    "max_latency": ""
  }
}

```
### 5. Heartbeat Message
```json
{
  "node_id": "<NODE ID>",
  "heartbeat": "YES",
  "timestamp": "<TIMESTAMP>"
}
```


