Distributed Load Testing System Using Kafka

Goal
The objective of this project is to design and implement a distributed load-testing system that coordinates between multiple driver nodes to run a highly concurrent, high-throughput load test on a web server. This system leverages Kafka as a communication backbone.

Architecture
The system consists of two main components:
Orchestrator Node
Driver Nodes
The Orchestrator Node and Driver Nodes communicate via Kafka topics to synchronize load tests, exchange metrics, and monitor health.
Each node registers itself with the Orchestrator Node and communicates using Kafka.
Nodes can be added or removed as needed for scalability.

Architecture Diagram

