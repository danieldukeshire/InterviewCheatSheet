# System Design

When designing a system, there are three important keys:
1. What are the different architectural pieces that can be used? 
2. How do these pieces work with each other? 
3. How can we best utilize these pieces: what are the right tradeoffs?

~ Some good videos to watch ~
- [Autonomy of a Prodcution App](https://www.youtube.com/watch?v=akXP6pC0piE)
- [Designing Youtube](https://www.youtube.com/watch?v=jPKTo1iGQiE)
- [Designing Twitter](https://www.youtube.com/watch?v=o5n85GRKuzk&list=RDCMUC_mYaQAE6-71rjSN6CeCA-g&start_radio=1&rv=o5n85GRKuzk&t=1)

## Common Trade-Offs
1. Consistency vs Performance (Availability)
- Caching -> Quick access speeds, however, data may not be up-to-date (updated from the db)
  - Maintaining cache consistency (ensuring data coherence between the cache and the underlying data source) versus achieving optimal performance. Strong consistency might require frequent cache invalidation or updates, which can impact performance. Looser consistency models might sacrifice some accuracy for improved performance.
2. Complexity vs. Maintainability (Serviceability or Manageability):
3. Data Integrity vs Redundancy
4. Security vs Usability

## Key Characteristics
1. Scalability
2. Reliability
3. Availability
#### Reliability Vs. Availability
4. Efficiency

##  Key Concepts
### Horizontal vs Vertical Scaling
A system can be scaled one of two ways.
- Vertical Scaling means increasing the resources of a specific node. For example, you might add additional memory to a server to improve its ability to handle load changes.
- Horizontal scaling means increasing the number of nodes. For example, you might add additional servers, thus decreasing the load on one server.
  
Vertical scaling is generally easier than horizontal scaling, but it is limited (moores law). You can only add mo much memory or disk space.

### Load Balancing
Typically, some frontend parts of a scalable website will be thrown behind a load balancer.
This allows the system to distribute the load evenly so that one server doesn't crash and take down the whole system. To do so you have to build a network of cloned servers that all have essentially the same code and access to the same data.

We can add a Load balancing layer at three places in our system:
1. Between Clients and Application servers 
2. Between Application Servers and database servers 
3. Between Application Servers and Cache servers

- Load Balancers can become a single point of failure 
  - May be good to introduce a second load balancer to form a cluster to introduce a form of resiliancy

### Asynchronism

### Database Denormalization and NoSQL

- Joins in a relational database such as SQL can get very slow as the system grows bigger. For this reason, you will generally avoid them.
- Denormalization is one part of this. Denormalization means adding redundant information into a database to speed up reads.
  - For read-intensive workloads, use a NOSQL database.
  - The tradeoff is storing WAY more data
- It is designed to scale better

#### SQL vs NoSQL
When it comes to database technology, there’s no one-size-fits-all solution. 
That’s why many businesses rely on both relational and non-relational 
databases for different needs. Even as NoSQL databases are gaining 
popularity for their speed and scalability, there are still situations where a 
highly structured SQL database may perform better; choosing the right 
technology hinges on the use case.
Reasons to use SQL database
Here are a few reasons to choose a SQL database:
1.We need to ensure ACID compliance. ACID compliance reduces 
anomalies and protects the integrity of your database by prescribing 
exactly how transactions interact with the database. Generally, NoSQL 
databases sacrifice ACID compliance for scalability and processing 
speed, but for many e-commerce and financial applications, an ACID-
compliant database remains the preferred option.
2.Your data is structured and unchanging. If your business is not 
experiencing massive growth that would require more servers and if 
you’re only working with data that is consistent, then there may be no 
reason to use a system designed to support a variety of data types and 
high traffic volume.
Reasons to use NoSQL database
When all the other components of our application are fast and seamless, 
NoSQL databases prevent data from being the bottleneck. Big data is 
contributing to a large success for NoSQL databases, mainly because it 
handles data differently than the traditional relational databases. A few 
popular examples of NoSQL databases are MongoDB, CouchDB, Cassandra, 
and HBase.
1.Storing large volumes of data that often have little to no structure. A 
NoSQL database sets no limits on the types of data we can store 
together and allows us to add new types as the need changes. With 
document-based databases, you can store data in one place without 
having to define what “types” of data those are in advance.
2.Making the most of cloud computing and storage. Cloud-based storage 
is an excellent cost-saving solution but requires data to be easily spread
across multiple servers to scale up. Using commodity (affordable, 
smaller) hardware on-site or in the cloud saves you the hassle of 
additional software and NoSQL databases like Cassandra are designed 
to be scaled across multiple data centers out of the box, without a lot of
headaches.
3.Rapid development. NoSQL is extremely useful for rapid development 
as it doesn’t need to be prepped ahead of time. If you’re working on 
quick iterations of your system which require making frequent updates 
to the data structure without a lot of downtime between versions, a 
relational database will slow you down.

#### Database Partitioning (Sharding)

- Sharding mesns splitting the data across multiple machines while ensure you have a way of figuring out which data is on which machine
- Many architectures end up using multiple partitioning schemes
- A few common ways of partitioning incude:

#### Vertical Partitioning
- This is basically partitioning by feature 
- For example ...
  - If you were building a social network, you might have one partition for tables relating to profiles, another one for messages, and so on.
- One drawback of this is that if one of these tables gets very large, you might need to repartition that database

#### Key-Based (Hash-Based) Partitioning
- This uses some part of the data (for example an ID) to partition it.
- A very simple way to do this is to allocate N servers and put the data on mod(key, n).
- One drawback with this is that the number of servers you have is effectively fixed.
  - Adding additional servers means reallocating the data -> an expensive task

#### Consistent Hashing

#### Directory-Based Partitioning
- Maintain a lookup table for where the data can be found.
  - This makes it relatively easy to add servers, but it comes with two major drawbacks:
    - The lookup table can be a single point of failure
    - Constantly accessing the table impacts performance

### Caching
- An in-memory cache can deliver very repid results. It is a simple key-value pairing and typically sits between your application layer and your data store
- Load balancing helps you scale horizontally across an ever-increasing number of servers, but caching will enable you to make vastly better use of the resources you already have
  - Locality of reference -> recently requested data is likely to be requested again
- A cache is short term memory: it has a limited amount of space, but is much faster than the original data source and contains the most recently accessed items.
   
- When an application requests a piece of information, it first tries the cache
- If the cache does not contain the key, it will then look up the data in the data store
  - When you cache, you might cache a query and its results directly. Or, alternatively, you can cache the specific object 

- An example of a cache would be an LRU cache -> where the least recently used item is removed to make space for new data.  

#### Cache Invalidation
- While caching is fantastic, it does require some maintenance for keeping cache coher

### Asynchronous Processing

#### Message Queues

### Networking Metrics
- Bandwidth - This is the maximum amount of data that can be transferred in a unit of time. It is typically expressed in bits per second
- Throughput - Wheras bandwidth is the maximum data that can be transferred in a unit of time, throughput is the actual amount of data 
- Latency - This is how long it takes data to go from one end to the other. That is, it is the delay between the sender sending information and the receiver receiving it.
- Availability -
- Consistency -
- Polling vs Streaming - 
- Messaging Patterns -

### CDN (Content Distribution Network)
- CDN is a kind of cache that comes into play for sites serving large amount of static media
- In a typical CDN setup, a request will first ask the CDN for a piece of static media; the CDN will serve that content if it as it locally available.
  - If it isn't available, the CDN will query the back-end servers for the file, cache it locally, and serve it to the requesting user.
- If the system we are building isn't yet large enough to have its own CDN, we can ease a future transition by serving the static media off of a separate subdomain using a lightweight HTTP server like Nginx, and cut over the DNS from your servers to a CDN cluster. 

## Handling the Questions
### Considerations
1. Failures: Essentially any part of the system can fail. You need to plan for many of these failures
2. Availability and Readability
3. Read-heavy vs Write Heavy
- Whether an application will do a lot of reads or a lot of writes impacts design
	1. Write-heavy: Consider queuing up the writes (but think about the potential failure here!)
	2. Read-heavy: Maybe a NoSQL database (Normalized), and definately want a cache
4. Security
- Think about the types  of issues a system might face and design around those.

### Design Step-by-Step
1. Scope the Problem
2. Make Reasonable Assumptions
3. Draw the Major Components
4. Identify the Key Issues
5. Redesign for the Key Issues

### Things to remember:
- Communicate
- Go broad first
- Use the whiteboard
- Acknowledge interviewer concerns
- Be careful about assumptions
- State your assumptions explicitly
  - When you do make assumptions, state them. This allows your interviewer to correct you if you are mistaken.
- Estimate when necessary
- Drive
- Application vs Web Server


