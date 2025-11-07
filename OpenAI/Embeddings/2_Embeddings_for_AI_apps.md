## Semantic search:
- To return most similar results to a search query
- How to?
  - Embed search query and other texts
  - Compute cosine distance
  - Extract texts with smallest cosine distance

 ### Enriching embeddings:
 - Example to embed in the following structure:
```
  Title: <product title>
  Description: <product description>
  Category: <product category>
  Features: <feature 1>; <feature 2>; <feature 3>; ...
```

```
# Define a function to combine the relevant features into a single string
def create_product_text(product):
  return f"""Title: {product['title']}
Description: {product['short_description']}
Category: {product['category']}
Features: {product['features']}"""

# Combine the features for each product
product_texts = [', '.join(product['features']) for product in products]
```

### Sorting by similarity:
Steps:
- Calculate the cosine distance between the query_vector and embedding.
- Append a dictionary containing dist and its index to the distances list.
- Sort the distances list by the 'distance' key of each dictionary.
- Return the first n elements in distances_sorted.

```
def find_n_closest(query_vector, embeddings, n=3):
  distances = []
  for index, embedding in enumerate(embeddings):
    # Calculate the cosine distance between the query vector and embedding
    dist = distance.cosine(query_vector, embedding)
    # Append the distance and index to distances
    distances.append({"distance": dist, "index": index})
  # Sort distances by the distance key
  distances_sorted = sorted(distances, key=lambda x: x['distance'])
  # Return the first n elements in distances_sorted
  return distances_sorted[0:n]
```

### Semantic search for products:
- Find five most semantically similar products - for computer

```
# Create the query vector from query_text
query_text = "computer"
query_vector = create_embeddings(query_text)[0]

# Find the five closest distances
hits = find_n_closest(query_vector, product_embeddings, 5)

print(f'Search results for "{query_text}"')
for hit in hits:
  # Extract the product at each index in hits
  product = products[hit['index']]
  print(product["title"])
# Create the embeddings from product_texts
product_embeddings = create_embeddings(product_texts)
```
