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

### Product recommendation system: 
- Recommendation system for an online retailer that sells a variety of products. This system recommends three similar products to users who visit a product page but don't purchase, based on the last product they visited.

- How to?
  - Combine the text features in last_product, and for each product in products, using create_product_text().
  - Embed the last_product_text and product_texts using create_embeddings(), ensuring that       last_product_embeddings is a single list.
  - Find the three smallest cosine distances and their indexes using find_n_closest().

```
# Combine the features for last_product and each product in products
last_product_text = create_product_text(last_product)
product_texts = [create_product_text(product) for product in products]

# Embed last_product_text and product_texts
last_product_embeddings = create_embeddings(last_product_text)[0]
product_embeddings = create_embeddings(product_texts)

# Find the three smallest cosine distances and their indexes
hits = find_n_closest(last_product_embeddings, product_embeddings)

for hit in hits:
  product = products[hit['index']]
  print(product['title'])
```

### Adding user history to the recommendation engine:
- Combine the text features for each product in user_history, embed the resulting strings, and calculate the mean embeddings using numpy.
- Filter products to remove any products that are present in user_history.
- Combine the features for each product in products_filtered and embed the resulting strings.

```
# Prepare and embed the user_history, and calculate the mean embeddings
history_texts = [create_product_text(article) for article in user_history]
history_embeddings = create_embeddings(history_texts)
mean_history_embeddings = np.mean(history_embeddings, axis=0)

# Filter products to remove any in user_history
products_filtered = [article for article in products if article not in user_history]

# Combine product features and embed the resulting texts
product_texts = [create_product_text(article) for article in products_filtered]
product_embeddings = create_embeddings(product_texts)

hits = find_n_closest(mean_history_embeddings, product_embeddings)

for hit in hits:
  product = products_filtered[hit['index']]
  print(product['title'])
```

### Classification with embeddings:
- Zero-shot classification:
  - Not using labeled data
- Process:
  - Embed class descriptions
  - Embed the item to classify
  - Compute cosine distances
  - Assign the most similar label

### Embedding restaurant reviews:
```
sentiments = [{'label': 'Positive'},
              {'label': 'Neutral'},
              {'label': 'Negative'}]

reviews = ["The food was delicious!",
           "The service was a bit slow but the food was good",
           "The food was cold, really disappointing!"]
```
```
# Create a list of class descriptions from the sentiment labels
class_descriptions = [review['label'] for review in sentiments]

# Embed the class_descriptions and reviews
class_embeddings = create_embeddings(class_descriptions)
review_embeddings = create_embeddings(reviews)
```

### Classifying review sentiment:
```
# Define a function to return the minimum distance and its index
def find_closest(query_vector, embeddings):
  distances = []
  for index, embedding in enumerate(embeddings):
    dist = distance.cosine(query_vector, embedding)
    distances.append({"distance": dist, "index": index})
  return min(distances, key=lambda x: x["distance"])

for index, review in enumerate(reviews):
  # Find the closest distance and its index using find_closest()
  closest = find_closest(review_embeddings[index], class_embeddings)
  # Subset sentiments using the index from closest
  label = sentiments[closest['index']]['label']
  print(f'"{review}" was classified as {label}')
```
