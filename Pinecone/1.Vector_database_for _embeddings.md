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

### Example: Create pinecone index:
- Create a serverless index called "my-first-index" to hold vectors with 256 dimensions, and configure the index for the 'aws' cloud platform in the 'us-east-1' region.
```
# Import ServerlessSpec
from pinecone import Pinecone, ServerlessSpec

# Initialize the Pinecone client with your API key
pc = Pinecone(api_key="api_key")

# If index already exists
pc.delete_index('my-first-index')

# Create your Pinecone index
pc.create_index(
    name="my-first-index",
    dimension=256,
    spec=ServerlessSpec(
        cloud='aws',
        region='us-east-1'
    )
)

# Connect to your index
index = pc.Index("my-first-index")

# Print the index statistics
print(index.describe_index_stats())

# List your indexes
print(pc.list_indexes())
```

### Managing indexes:
- Namespaces:
  - Containers for partitionaling indexes
    - Separate datasets
    - Data versioning
    - Separate groups
- Organizations:
  - Owner: Permissions across entire org, manage billing, users, all projects
  - User: Restricted org-level permissions, invited to specific projects, become owner to those projects

### Vector Ingestion:
- .upsert(): Update or insert
- Example: vectors = [
    {
        "id": "0",
        "values": [0.025525547564029694, ..., 0.0188823901116848]
        "metadata": {"genre": "action", "year": 2024}
    },
        ...,

]
```
# Check that each vector has a dimensionality of 1536
vector_dims = [len(vector['values']) == 1536 for vector in vectors]
print(all(vector_dims))

# Connect to your index
index = pc.Index("datacamp-index")

# Ingest the vectors and metadata
index.upsert(vectors)

# Print the index statistics
print(index.describe_index_stats())
```
#### Output:
```
    {'dimension': 1536,
     'index_fullness': 0.0,
     'namespaces': {'': {'vector_count': 100}},
     'total_vector_count': 100}
```
