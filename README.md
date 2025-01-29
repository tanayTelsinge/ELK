# Elastic Stack (ELK) Notes - credits to Timy Sam

## Introduction to Elastic Stack:
- **Elastic Stack** consists of:
  - **Beats**: Data shippers
  - **Logstash**: Data processing pipeline
  - **Elasticsearch**: Storage and search engine
  - **Kibana**: Visualization tool
- Common uses:
  - **Logging**: Example: Blizzard uses Elastic Stack for logging gamer and server events.
  - **Metrics**: Example: NASA JPL used Elastic Stack.
  - **Security**: Example: Slack integrates Elastic Stack for alerting based on logs.
  - **Business**: Example: Uber, Tinder use Elastic Stack for business data.

## Components of Elastic Stack:

### 1. **Beats**:
   - Collection of data shippers that collect logs and metrics from various sources.
   - Sends data to Logstash or Elasticsearch for further processing.
   - Types of Beats:
     - **Filebeat**: For log files.
     - **Metricbeat**: For system metrics.
     - **Packetbeat**: For network data.
     - **Winlogbeat**: For Windows event logs.
   
### 2. **Logstash**:
   - A data processing pipeline.
   - Collects, parses, and transforms logs into a format suitable for Elasticsearch.
   - It can filter, enrich, and format the data before sending it to Elasticsearch.

### 3. **Elasticsearch**:
   - The core of the stack: Stores and searches data.
   - Designed for fast and efficient full-text search on large volumes of data.
   - Uses **REST APIs** to communicate with other components.
   - Stores data as **Documents** (JSON objects with metadata).
   - Organizes related documents into **Indices**.

### 4. **Kibana**:
   - A web interface for visualizing data stored in Elasticsearch.
   - Provides features like pie charts, bar graphs, and dashboards.
   - Allows querying Elasticsearch through its REST APIs.

## Elasticsearch Architecture:

- **Node**: An individual instance of Elasticsearch, responsible for storing data and performing search operations.
- **Cluster**: A group of nodes working together. A cluster can contain one or more nodes.
- **Document**: A basic unit of data, stored in JSON format, with metadata like index, type, and ID.
- **Index**: A collection of documents that are logically related (like a database table). It helps to search and organize data efficiently.

## Sharding and Replication:
- **Shard**: A basic unit of storage in Elasticsearch, where data is physically stored.
- **Replication**: Creating copies (replica shards) of primary shards to ensure data availability and redundancy in case of node failure.
- **Sharding**: Dividing data into smaller parts (shards) and distributing them across nodes to optimize storage and search speed.
- **Replication** ensures high availability and improves performance by distributing search queries across replicas.
  
## Key Elasticsearch Concepts:
1. **Shards and Replica Shards**:
   - When creating an index, a primary shard is created by default, but you can specify multiple shards.
   - **Replica shards** provide backup copies for each primary shard.
   - Elasticsearch automatically creates a replica of each shard.

2. **Routing**:
   - The process of determining which shard a document should be stored in or searched from.
   - The routing mechanism uses a **hashing function** based on the document's ID to assign it to a particular shard.
   
3. **Indexing and Querying**:
   - **PUT / POST** to create an index or document.
   - **GET** to retrieve a document.
   - **POST** for partial updates to a document (using the _update endpoint).
   - **DELETE** to delete documents or entire indices.

4. **Analyzers**:
   - Elasticsearch uses analyzers to convert text into terms for efficient searching.
   - An analyzer includes:
     - **Character filtering** (e.g., remove certain characters).
     - **Tokenization** (splitting text into terms).
     - **Token filtering** (e.g., removing stop words).
   
   - Example of analysis:
     ```json
     POST _analyze
     {
       "text": "Hello, How are you? Thanks for joining this session!! High-five",
       "analyzer": "standard"
     }
     ```
   - Custom analyzers can be configured as needed.

## CRUD Operations:

### 1. **Create an Index**:
   - `PUT /index-name`
   - Example: `PUT students`

### 2. **Index a Document**:
   - **POST** for auto-generated ID or **PUT** for specific ID.
   - Example: 
     ```json
     POST students/_doc
     {
       "name": "Rohit",
       "age": 21
     }
     ```

### 3. **Retrieve a Document**:
   - `GET /index-name/_doc/id`
   - Example: `GET students/_doc/1`

### 4. **Update a Document**:
   - `POST /index-name/_update/id`
   - Example:
     ```json
     POST students/_update/3
     {
       "doc": {
         "age": 22
       }
     }
     ```

### 5. **Delete a Document**:
   - `DELETE /index-name/_doc/id`
   - Example: `DELETE students/_doc/1`

### 6. **Delete an Index**:
   - `DELETE /index-name`
   - Example: `DELETE students`

## Handling Elasticsearch Cluster:

1. **Cluster Health**:
   - `GET /_cluster/health`
   - Shows the health of the cluster (green, yellow, red).

2. **Nodes Information**:
   - `GET /_cat/nodes?v`
   - Shows the list of nodes in the cluster.

3. **Shards Information**:
   - `GET /_cat/shards?v`
   - Shows shard allocation and health status (e.g., STARTED, UNASSIGNED).

## Hands-on Example:

1. **Start Elasticsearch Node**:
   - Run `elasticsearch.bat` to start a node.

2. **Create a Node Enrollment Token**:
   - `elasticsearch-create-enrollment-token -s node`
   - Use token to connect a new node to the cluster.

3. **Start Kibana**:
   - Run `kibana.bat` to start Kibana and connect with Elasticsearch.

## Sharding and Replica Configuration:

1. **Sharding**: Divide large datasets across multiple nodes.
2. **Replication**: Ensure data availability by storing copies of shards across nodes.

## Notes on Data Storage:

- **Data is stored as Documents (JSON)**: Metadata plus the actual data.
- **Cluster**: Contains multiple nodes; each node can store data and participate in search operations.
- **Index**: Logical grouping of documents; stored physically across shards.

## Advanced Concepts:

- **Routing**: Resolves which shard a document belongs to, using a hash based on the document's ID.
- **Analysis**: Converts text into terms for building an inverted index.
