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

#### Example: Handling Exceptions
```
from openai import OpenAI
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Use the try statement
try: 
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[message]
    )
    # Print the response
    print(response.choices[0].message.content)
# Use the except statement
except openai.AuthenticationError as e:
    print("Please double check your authentication key and try again, the one provided is not valid.")
```

#### Example: Avoiding rate limits with retry
- To avoid fails due to rate limits, @retry decorator from the tenacity library is used.
- Start retrying at an interval of 5 seconds, up to 40 seconds, and to stop after 4 attempts.
```
from tenacity import retry, wait_random_exponential, stop_after_attempt

client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Add the appropriate parameters to the decorator
@retry(wait=wait_random_exponential(min=5, max=40), stop=stop_after_attempt(4))
def get_response(model, message):
    response = client.chat.completions.create(
      model=model,
      messages=[message]
    )
    return response.choices[0].message.content
print(get_response("gpt-4o-mini", {"role": "user", "content": "List ten holiday destinations."}))
```

#### Example: Batch messages
- To avoid using a for loop that would send too many requests - send the requests in batches
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

messages = []
# Provide a system message and user messages to send the batch
messages.append({
            "role": "system",
            "content": "Convert each measurement, given in kilometers, into miles, and reply with a table of all measurements."
        })
# Append measurements to the message
[messages.append({"role": "user", "content": str(i) }) for i in measurements]

response = get_response(messages)
```
print(response)

### Example: setting a limit of n tokens - token limits
- using tiktonen library
```
import tiktoken
client = OpenAI(api_key="<OPENAI_API_TOKEN>")
input_message = {"role": "user", "content": "I'd like to buy a shirt and a jacket. Can you suggest two color pairings for these items?"}

# Use tiktoken to create the encoding for your model
encoding = tiktoken.encoding_for_model("gpt-4o-mini")
# Check for the number of tokens
num_tokens = len(encoding.encode(input_message['content']))

# Run the chat completions function and print the response
if num_tokens <= 100:
    response = client.chat.completions.create(model="gpt-4o-mini", messages=[input_message])
    print(response.choices[0].message.content)
else:
    print("Message exceeds token limit")
```

## OpenAI's tools:
- Can be used to return more specific information
- Can define a more prescise staructure
- Enhances capabilities of API call
- Function calling: Used to enhance responses by integrating with extrenal APIs, such as for a weather chatbot calling an API to return current temperatures at specific locations.

### Extracting information:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the function parameter type
function_definition[0]['function']['parameters']['type'] = 'object'

# Define the function properties
function_definition[0]['function']['parameters']['properties'] = {'title': {'type': 'string',
      'description': 'Title of the research paper'},
     'year': {'type': 'string', 'description': 'Year of publication of the research paper'}}

response = get_response(messages, function_definition)
print(response)
```

### Extracting response:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

response = get_response(messages, function_definition)

# Define the function to extract the data dictionary
def extract_dictionary(response):
  return response.choices[0].message.tool_calls[0].function.arguments

# Print the data dictionary
print(extract_dictionary(response))
```
#### Output:
{"product": "TechCorp ProMax", "sentiment": "positive", "features": ["powerful processor"], "suggestions": ["offer more color options"]}

### Example: Setting specific function:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

response= client.chat.completions.create(
    model=model,
    messages=messages,
    # Add the function definition
    tools=function_definition,
    # Specify the function to be called for the response
    tool_choice={'type': 'function', 'function': {'name': 'extract_review_info'}}
)

# Print the response
print(response.choices[0].message.tool_calls[0].function.arguments)
```

### Example: Defining a function with external APIs:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the function to pass to tools
function_definition = [{"type": "function",
                        "function" : {"name": "get_airport_info",
                                      "description": "This function calls the Aviation API to return the airport code corresponding to the airport in the request",
                                      "parameters": {"type": "object",
                                                     "properties": {"airport_code": {"type": "string","description": "The code to be passed to the get_airport_info function."}} }, 
                                      "result": {"type": "string"} } } ]

response = get_response(function_definition)
print(response)
```
