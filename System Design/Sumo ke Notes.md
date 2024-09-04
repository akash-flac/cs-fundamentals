# Gaurav Sen's Playlist

1: vertical scaling: optimise precision and increase through put with the same resources

2: preprocessing (e.g cron job) : prepare before hand during non pick hours

3: Backups: keep backups and avoid single point of failure

4: horizontal scaling: get more resources

5: micro service architecture - different components handling different tasks / sub-applications of the system

6: distributed system (partioning) - same task assigned to different components depending on traffic, load etc

7: load balancer - routes requests to different servers  

8: Decoupling - separate workloads independent of each other

9: Logging - keeping track of metrics

10: extensible - extend the system to any application

  

# Monolith vs Microservice

Monolith can be more than 1 machine

## Monolith

All the code/application logic is written in a single project

### Advantage

- scales out when put under a small load

- less complex

- less duplication of code like setting up tests etc

- code can run faster (since the logic is in the same place)

### Disadvantage:

- more context required

- complicated deployments (needed for smallest code change)

- too much responsibility on each server (1 crash whole server crash)

## Microservice

### Advantage

- new person needs context of just the small machine

- parallel development is easy (maybe 2 systems calling the same system in a monolith so can't use them parallely)

- can have a streamlined database (not all applications will require equal resources)

### Disadvantage:

- Needs smart architect to decide if microservice needed (if it only talks to 1 service, its not needed)

- hard to design

# Database Sharding

Database sharding **splits a single dataset into partitions or shards**. Each shard contains unique rows of information that you can store separately across multiple computers, called nodes. All shards run on separate nodes but share the original database's schema or design.

---
# Introduction
# Why

- Understand architecture - break into components

- Understand system tradeoffs

- Ensure - structure, maintenance, user needs

- High level understanding

  

# What

- Designing structure of software system for desired functionality and efficiency-measuring metrics

- Design architecture, components, data to meet user needs

  

## Key Terms

1. Documentation

2. Requirement Analysis - functional and non-functional

3. High level design - overall

4. Database Design

5. API Design

6. Component Design

7. Scalability and Performance

8. Testing

9. Security

10. Reliability and Performance Breach

  

# Concepts

## Availability

- System should always be up and responsive to user's request

- Measured as % time that system is operational in a given time window (day/month/week)

- Measured in 9s

![[Pasted image 20240525223749.png]]

- Goal is to minimize downtime

  

## Throughput

- Measure performance and capacity of system to complete tasks, transactions

- Amount of data transferred through system in given time

- High Throughput

- Solution - split requests and distribute workload over different devices

  

## Latency

- Time taken by a request to receive response from server

- Lower latency = quick response time, higher performance

- Important for real-time responsiveness like online gaming, video streaming etc.

- Solution - Optimizing algorithms, Hardware

  

## Network Protocols

- Network - communication between users/servers

- Protocols - list of rules defining communication/transmission

  

## Load Balancers

- Balance load among various servers

- Can direct load/requests

- Traffic managers, help increase throughput and availability

- Prevent single point of failure

  

## Proxy

- Between client and server

- Types:

    - Forward proxy: mask for client and hides client identity from server

    - Reverse proxy: mask for server and hides server identity from response

  

## Database

- data stored in systems for later retrieval

- Database Partition - dividing DB into smaller chunks to increase performance

### Relational Database

- MySQL, PostSQL

- Can only store structured relationships and hierarchies

- ACID

#### ACID

Transaction is an interaction with the DB. ACID compliance controls execution of transactions.

- Atomicity

    - All or nothing

    - Group of operations treated as single unit of work

    - One fail, whole transaction fails

- Consistency

    - DB state changes don't corrupt data

    - Tx can move from one valid state to another

- Isolation

    - Concurrency

    - Transactions handled independently, without affecting others

- Durability

    - Data written to DB persists there even during failure

### Non - Relational Database

- Redis, Cassandra

- Flexible structure, can store both structured and non-structured

- Used in high distributed and high speed systems

- BASE

#### BASE

Basically Available Soft State Eventual Consistency.

Maintains integrity of NoSQL DBs.

Key Factor in their scalability.

- Basically Available - always available

- Soft State - provides flexibility to system, allows it to change over time

- Eventual Consistency - takes time to reach consistent state

### SQL vs NoSQL

sometimes polyglot architecture of both

- NoSQL

    - lots of data

    - distributed system

    - scalability required

- SQL

    - data structure important

    - complex queries

    - DB needs fewer updates

  

## Scaling

- Increasing capacity of system to handle more requests

- Horizontal Scaling

    - Adding more servers to handle requests

    - Handles more traffic by sending to multiple servers

- Vertical Scaling

    - Increase capacity of single machine by improving CPU, RAM, etc.

    - Handles more traffic through single machine

  

## Caching

- Reduces system latency

- Frequently used data stored in cache for quick access, instead of querying the DB

- Adds complexity because important to maintain consistency between cache data and DB data

- Limited size cache cause expensive

- Cache eviction algorithms FIFO LIFO LRU LFU to determine which data to remove from cache when space needed

  

## Consistent Hashing

- improvement over traditional hashing to offer flexibility in scaling

- Hash Ring - Users and servers connected in virtual circular ring

- Assumed infinite, can have any number of servers

- Servers given random locations based on hash functions

- So efficient distribution of requests or data among servers

- Horizontal Scaling

  

## CAP Theorem

A distributed database system can only have 2 of 3 essential properties:

1. Consistency - any READ operation gives most recent WRITE

2. Availability - despite one or more node failure

3. Partition Tolerance

    1. important, so tradeoff between first 2

    2. means system should work irrespective of node/network failures