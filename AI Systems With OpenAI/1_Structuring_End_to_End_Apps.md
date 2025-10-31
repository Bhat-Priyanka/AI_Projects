## Structuring End-to-End real world applications with OpenAI API:

### Structuring API calls:
#### Example: Formatting model response as json
```
from openai import OpenAI
# Create the OpenAI client
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create the request
response = client.chat.completions.create(
  model="gpt-4o-mini",
  messages=[
   {"role": "user", "content": "I have these notes with book titles and authors: New releases this week! The Beholders by Hester Musson, The Mystery Guest by Nita Prose. Please organize the titles and authors in a json file."}
  ],
  # Specify the response format
  response_format={"type": "json_object"}
)

# Print the response
print(response.choices[0].message.content)
```
#### Output:
{"new_releases": [
    {
        "title": "The Beholders",
        "author": "Hester Musson"
    },
    {
        "title": "The Mystery Guest",
        "author": "Nita Prose"
    }
]}
####
