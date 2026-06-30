# 1. Data-Intensive vs Compute-Intensive Systems

There's a difference between systems where **CPU is the bottleneck** vs systems where **data is the bottleneck**.

- **Compute-intensive** systems struggle with raw processing power — the hard part is parallelizing a huge computation across machines (think: training an ML model, rendering video).
    
- **Data-intensive** systems have a different set of problems. The CPU is rarely the issue. What keeps you up at night is:
    
    - Storing and processing large volumes of data
        
    - Handling changes to data over time
        
    - Keeping things consistent when failures happen or multiple requests hit at the same time
        
    - Making sure your services stay available even when stuff goes wrong
        

Most of the systems we build day-to-day — APIs, backends, analytics pipelines — are data-intensive.

# 2. What Most Data-Intensive Systems Are Built From

At some level, almost every data-intensive system is just a combination of these building blocks:

|Component|What it does|
|---|---|
|**Database**|Stores data so your app (or other apps) can read it later|
|**Cache**|Remembers results of expensive operations so you don't recompute them on every read|
|**Search Index**|Lets users search or filter data by keywords — databases alone aren't great at this|
|**Stream Processor**|Reacts to events and data changes as they happen, in real time|
|**Batch Processor**|Crunches through a large backlog of accumulated data on a schedule|

Most production systems use several of these together. Your job as the engineer is to figure out which combination makes sense for your use case.

> **Note to self:** Application code is the business logic layer — it's the code you write that sits between the user and the database. Think of it as the "brain" that decides what to do with a request: validate input, call the right services, read/write to the DB, and send back a response. It's usually **stateless**, meaning it doesn't hold onto any data between requests. Every request comes in fresh—no memory of the previous one. All the actual state (user data, session info, etc.) lives in the database or cache. This is intentional: it makes it easy to scale horizontally by spinning up more instances, since any instance can handle any request.

# 3. Operational Systems vs Analytical Systems

These two types of systems have very different jobs.

**Operational systems** are your live backend—the databases and services that power the product users actually interact with. The app code is constantly reading and writing data based on what users are doing (clicking, submitting forms, making purchases, etc.).

**Analytical systems** serve data scientists and business analysts. They hold a **read-only copy** of data from operational systems and are optimized for running complex queries over large datasets—not for handling live user traffic.

# 4. Transaction Processing (OLTP) vs Analytics (OLAP)

The word **"transaction"** gets thrown around loosely, but in this context it means a set of low-latency reads and writes that form one logical unit of work (like "place an order" or "update a user's profile").

## OLTP vs OLAP — Quick Comparison

|Property|OLTP (Online Transaction Processing)|OLAP (Online Analytical Processing)|
|---|---|---|
|**Main read pattern**|Fetch a small number of rows by key/id|Aggregate over millions of rows|
|**Main write pattern**|Low-latency inserts/updates from user actions|Bulk loads or event streams|
|**Human user example**|A customer checking out on an e-commerce site|An analyst running a revenue report|
|**Machine use example**|An API handling 10k requests/sec|A nightly ETL pipeline|
|**Type of queries**|Simple, pre-defined, short-running|Complex, ad-hoc, long-running|
|**Query volume**|Very high (thousands/sec)|Low to medium (scheduled or on-demand)|
|**Data represents**|Current state of the world|History of events over time|
|**Data used by**|Core product / end users|Business intelligence / data science|

## How SQL is Used Differently

In OLTP, SQL queries are usually simple and targeted—for example, `SELECT * FROM orders WHERE id = 123`. They're fast because they touch a small slice of the data. Most OLTP systems also restrict what kinds of queries are allowed (no arbitrary full-table scans), because a badly written query could tank performance for all users.

In OLAP, you're doing the opposite—scanning millions of rows, joining huge tables, and computing aggregates. The queries are often written by analysts on the fly, and the system is optimized to handle that.

## Real-Time Analytical Workloads — A Different Category

Traditional OLAP (think: data warehouses like Redshift or BigQuery) is built for batch analytics—you load data in bulk and run reports. But there's a newer category built for **product analysts and real-time analytics**, where you need query results in milliseconds even over billions of rows.

**ClickHouse** is a great example of this. It's a columnar database built specifically for high-throughput analytical queries on fresh data.

The difference from traditional OLAP:

- **Traditional OLAP:** Data is usually hours or days old (batch ingestion), and queries take seconds to minutes.
    
- **ClickHouse-style:** Data can be seconds old (real-time ingestion), and queries return in milliseconds.
    

This is the kind of system you'd use if you wanted to let a product manager filter live user events by country, device, and feature flag—and get results instantly.