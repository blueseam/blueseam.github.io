---
layout: post
title:  "System Design"
categories: [ blueseam, tutorial ]
image: assets/images/h1.png
---

# System Design Notes

## 1. System Design Basics
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
   - Queues
   - Caching
   - Replication
   - SQL vs No-SQL
   - Indexes
   - Proxies
   - Data Partitioning
