---
description: 201007 - Tools for Reliability
---
# Tools for Reliability

Here is just to generalize my understanding of being an infrastructure Architect.

First let me clarify the difference between an infrastructure Architect and an application Architect. 

In my mind as an application Architect, we need to care more about the application coding layout for extensibility, the testability,  the design patterns and the integration with other applications, etc. For example, for Ruby on Rails application, we need to understand the convention of the layout and know which folder should be code be placed in; for backend applications, we need to check if the Dependency Injection, AOP, or IOC is used to decouple different parts for easier Unit Tests; for mobile Applications, we need to think about which design pattern to use, MVVM, VIP or MVC; and for microservice applications, we need to care about the client side resilience using circuit breaker pattern, bulkhead pattern, client-side load balancer, etc. In a word it's more about how to provide the scaffolding against which the developers can follow and build their code so that all pieces of the application can fit together.

On the other hand as an infrastructure Architect, the main responsibility is to take care the Reliability of the whole system. There is already a role named as Site Reliability Engineer, which I thought is more about maintain the reliability of the system rather than providing the design.

So the next question is, what is system reliability.

I think the system reliability include 5 parts, 

- Security
- Performance
- Availability
- Scalability
- Extensibility

So what can be the tools for designing a reliable system which the above 5 characteristics?

## Security

- encryption
  - hash code
  - symmetric encryption
  - asymmetric encryption
  - key management
  
- SSO
  - AD
  - OAuth
  
- anti-spam & information filtering
  - Text Match
  - Classification
  - Blacklist
  
- Risk Control
  - Decision Tree
  - machine learning / Deep Learning / etc  

- CyberSecurity
  - TLS 1.2
  - cert pinning
  - CORS
  - XSS

## Performance

- cpu
  - queue
  - Event Loop
    - Select
    - Poll
    - Epoll

- memory
  - memory leak

- IO
  - SSD
  - RAID
  - B+ Tree vs LSM Tres
  - Async IO
  - RAID vs HDFS

- network
  - CDN
  - reverse proxy
  - Cache
  - compression
  - protocol - http/ protobuf
  - Kernel Bypass Network

- Java Frameworks
  - Chronicle Queue
  - Disruptor
  - Vert.x

## Availability

- KPI
  - SLA
    - Change Management
    - Backup and Restore
    - Incident Management
    - PIR
    - maintenance, patching and upgrade: zero downtime upgrade
  - Concurrency
- load balancer
- message queue
- session management
  - stateless
  - session copy
  - memcache
  - redis cluster
- data
  - CAP
- CI/CD
  - canary release
  - traffic mirror
  - green-blue release
  - A/B test  
- distributed cache

## Scalability

- spike
- horizontal separation - business
- vertical separation - three layer architecture
- DNS load balancer
- TCP load balancer
- HTTP load balancer
- virtual IP
- algorithm
  - round robin
  - weight round robin
  - random
  - least connection
  - source hashing 
- consistent hash algorithm  
- relational database federation, master-slave replication, sharding
- nosql sharding and replication

## Extensibility  

- Three layer
  - Application Layer
  - Service Layer 
  - Data layer
- Microservice
- distributed message queue -> event driven
- interface driven design
- Operation
  - Evidence for business to understand the change
    - Unit Test Evidence
    - Integration Evidence
    - End-to-End Test Evidence