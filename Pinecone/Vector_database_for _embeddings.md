# Vector databases for embeddings with Pinecone:
Pinecone is vector databse solution for builing and scaling GenAI applications.
Used for search engines, recommendation engines etc.

## Indexes:
- Used to store vectors and serve queries and other vector manupulations.
- Each index contains records for each vector, including metadata.

### Pod-based index:
- Choose hardware to create index -> pods
- Pod type determines storage, query, latency and query throughout

### Serverless index:
- No resource management, scale automatically
- Run on cloud and stored in blob
- Easy and low cost
