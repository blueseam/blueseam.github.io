---
layout: post
title:  "System Design"
categories: [ blueseam, tutorial ]
image: assets/images/h1.png
---

# System Design Notes

**I found these notes sometime ago, i am not the author.**

1. Try to break th problem into simple modules (Top Down approach)

2. Talk about the trade-off (No soltion is perfect)
   Calculate the impact on system based on all the constraints and the end test cases

3. Architectural pieces/resources available

4. How these resources work together

5. Utilization and Tradeoffs
   - Consistent Hashing
   - CAP Theorem
   - Load balancing
     - Type of distribution
       1. Random
       2. Round-robin
       3. Weight for CPU & Memory cycle
     - To utilize full scalabillity & redundancy(Add 3 LB[3-Tier Architecutre])
       1. User << LB1 >> Web Server
       2. Web Server << LB2 >> App Server / Cache Server
       3. App Server / Cache Server << LB3 >> DB
     - Smart Clients
       - Takes a pool of service hosts & balances load
         - detects hosts that are not responsive
         - recovered hosts
         - addition of new hosts
       - Load balancing functionality to DB(Cache service)
       - Attractive solution for developers
       - As system grows > LBs (Standalone Servers)
     - Hardware Load Balancers
       - Expensive but high preformance
       - Not trivial to configure
       - Large companies tond to avoid this config or use it as 1st point of contact to their system to serve user requests
       - Intra network uses smart clients / hybrid solution for load balancing traffic
     - Software Load Balancers
       - No pain of creation of smart client
       - No cost of purchasing dedicated hardware
       - hybrid approach : HAProxy(OSS Load balancer)
         1. Running on client machine(Client managed by HAProxy)
         2. Running on intermediate server(Server Side  Components)
         3. HAProxy
            - manages health checks
            - removal & addtion of machines
            - balances requests a/c pools
   - Queues
     -  effectively manages requests in large-scal distubuted system
     -  In small systems > write are fast
     -  In complex systems > high incomming load & individual write take mass time
     -  To achieve high performance & availaility (System need to the asynchronous > Queues)
     -  difficult for fair & balanced distribution
   - Caching
   - Replication
   - SQL vs No-SQL
     - SQL(Relational Database)
       1. Structured
       2. Predefined schema
       3. Data in rows & columns
       4. MySQL, Oracle, MS SQL Server, SQLite, Postgre, MariaDB 
     - NO SQL(Non Relational Dataase)
       1. Unstructured
       2. distributed
       3. dynamic schema
       4. DB Method: Key-Value Stores, Document DB, Wide-column DB, Graph DB
          ![NoSQL Description](/assets/images/h2.png)
       5. High Level differences between SQL & NoSQL
         | Property | SQL | NoSQL |
         |---|---|---|
         |A|B|C|
   - Indexes
   - Proxies
   - Data Partitioning
