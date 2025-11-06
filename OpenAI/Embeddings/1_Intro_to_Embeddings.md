## Embeddings:

### What are Embeddings?
- Numerical representaion of text
- Text is mapped onto a multi-dimensional vector space.
- Numbers outputted by the model are text's location in the space.
- Similar words are mapped together
- Allow semantic (context and intent behind text) to be captured
- Used in recommendation systems - semantic recommendation systems
- Used in classifications: sentiment, observations, categorization

### Example: Creating text embeddings:
```
from openai import OpenAI
# Create an OpenAI client
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create a request to obtain embeddings
response = client.embeddings.create(model="text-embedding-3-small",
input="this is a long text.")

# Convert the response into a dictionary
response_dict = response.model_dump()
print(response_dict)
print(response_dict['usage']['total_tokens'])
```

### Dimensionality reduction and t-SNE:
- t-SNE: t-distributed Stochastic Neighbor Embedding

### Example: Embedding product description and visualize:
- Embed the 'short_description' for each product to enable semantic search for the retailer's website.

```
from sklearn.manifold import TSNE
import numpy as np
import matplotlib.pyplot as plt
# Extract a list of product short descriptions from products
product_descriptions = [product['short_description'] for product in products]

# Create embeddings for each product description
response = client.embeddings.create(
  model="text-embedding-3-small",
  input=product_descriptions
)
response_dict = response.model_dump()

# Extract the embeddings from response_dict and store in products
for i, product in enumerate(products):
    product['embedding'] = response_dict['data'][i]['embedding']
    
print(products[0].items())

# Create categories and embeddings lists using list comprehensions
categories = [product['category'] for product in products]
embeddings = [product['embedding'] for product in products]

# Reduce the number of embeddings dimensions to two using t-SNE
tsne = TSNE(n_components=2, perplexity=5)
embeddings_2d = tsne.fit_transform(np.array(embeddings))

# Create a scatter plot from embeddings_2d
plt.scatter(embeddings_2d[:,0], embeddings_2d[:,1])

for i, category in enumerate(categories):
    plt.annotate(category, (embeddings_2d[i, 0], embeddings_2d[i, 1]))

plt.show()
```
