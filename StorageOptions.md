# Storage Options

The following Table provides a good overview of how to decide about the storage options.
## How to decide while designing a system 
When designing a new system:

- Identify what type of data you need to store (binary, structured, config, logs).
- Map it to one or more storage types above.
- Decide based on
   - Access pattern (frequent, archival)  
   - Performance needs (latency, throughput)
   - Consistency vs flexibility
   - Cost vs scalability


| **Storage Type**                | **Use Case**                                                    | **Concept**                                 | **Examples / Tools**                                           | **Notes / Design Considerations**                                          |
| ------------------------------- | --------------------------------------------------------------- | ------------------------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Object Storage**              | Storing static files (images, docs, reports), large blobs       | Key-value flat storage, immutable, scalable | AWS S3, Azure Blob, Google Cloud Storage, MinIO, FluentStorage | Best for unstructured data, supports pre-signed URLs, folder-like prefixes |
| **Block Storage**               | Virtual hard drives for VMs/DBs, fast I/O                       | Raw storage volumes, attached to compute    | EBS (AWS), Azure Disks                                         | Not ideal for sharing files; used for persistence for VMs, DBs             |
| **File Storage (NAS)**          | Shared file systems across multiple servers                     | Hierarchical file systems, POSIX-compliant  | NFS, Azure Files, AWS EFS                                      | Good for legacy apps needing mounted drives                                |
| **Relational DB (SQL)**         | Structured data with relationships, transactional use           | Tables, rows, SQL schema                    | SQL Server, PostgreSQL, MySQL, Azure SQL                       | Strong consistency, ACID compliance                                        |
| **NoSQL (Document DB)**         | JSON-like semi-structured data, high scalability                | Key-document or column-value stores         | MongoDB, Cosmos DB, DynamoDB                                   | Schemaless, flexible, good for hierarchical data                           |
| **Time Series DB**              | Logging, sensor data, metrics over time                         | Optimized for time-indexed data             | InfluxDB, TimescaleDB, Azure Data Explorer                     | Efficient for aggregations and queries over time ranges                    |
| **Graph DB**                    | Relationship-heavy queries (e.g., social networks, permissions) | Nodes + edges                               | Neo4j, Amazon Neptune                                          | Fast for relationship traversal, but niche use                             |
| **In-Memory Cache**             | Fast access to hot data                                         | Key-value cache in memory                   | Redis, Memcached, Azure Cache for Redis                        | Volatile, best for reads, rate limiting, sessions                          |
| **Search Index Storage**        | Full-text search, filters, analytics                            | Inverted index, distributed shards          | Elasticsearch, OpenSearch                                      | Not a DB, optimized for read + text queries                                |
| **Cold/Archive Storage**        | Cost-effective long-term storage                                | Write-once-read-rarely                      | Amazon Glacier, Azure Archive                                  | Cheap but slow access; for backups/logs                                    |
| **Data Lake**                   | Analytics on large, raw datasets                                | Flat file + schema-on-read                  | Azure Data Lake, S3 + Athena, Delta Lake                       | Used in ML/BI workloads, supports batch & streaming                        |
| **CDN (Edge Cache)**            | Static asset delivery near users                                | Cache + edge replication                    | Cloudflare, Azure CDN, Amazon CloudFront                       | Not origin storage, acts as global cache layer                             |
| **Content-Addressable Storage** | Versioned, deduplicated storage for immutable blobs             | Data identified by hash                     | Git, IPFS, OCI registries (Docker images)                      | Great for version control, immutability                                    |


## Tools That Abstract Across Storage Types

Imagine:
You want to store a file, but you don’t care whether it’s going into:

- AWS S3 (object storage),
- Azure Blob,
- Google Cloud Storage,
- A local file system.

A tool that abstracts storage types lets you write the same code or use the same commands regardless of where the file is going. You delegate the differences to the tool.

### Why Is This Useful?
- Portability: Switch between providers or environments without rewriting code.
- Consistency: One API or interface to learn.
- Cloud Agnostic: Great for hybrid or multi-cloud strategies.

| Tool/Library                   | What It Does                                                  | Abstracts Over                     |
| ------------------------------ | ------------------------------------------------------------- | ---------------------------------- |
| **FluentStorage (.NET)**       | Unified API for S3, Azure Blob, file system, etc.             | Object/File storage                |
| **RClone**                     | Sync/copy files across cloud storage                          | Many: S3, Azure, GDrive, FTP, etc. |
| **MinIO Gateway Mode**         | Provides S3 interface over Azure Blob, NAS, etc.              | Storage backends                   |
| **Apache Libcloud** (Python)   | Unified interface to many cloud storage APIs                  | File/object storage                |
| **HashiCorp Vault + Plugins**  | Store secrets or files in various backends                    | Storage for secrets/files          |
| **CSI Drivers (Kubernetes)**   | Mount different storage types into pods                       | Block/File/Object storage          |
| **AbstractRepository Pattern** | In your own code, abstracting between DBs, blob storage, etc. | Manual abstraction                 |
