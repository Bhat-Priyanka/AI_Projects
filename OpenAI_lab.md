# Working with OpenAI API and Python:

## What is OpenAI API:
OpenAI is a company that research and develop AI systems. Their groundbreaking development ChatGPT which is an AI powered chatbot gained worldwide attention.  
OpenAI API allows access to OpenAI models using which customers can build their own applications.

## Prerequisites

### Setting up your OpenAI API access:
* Create an account at https://openai.com/
* Navigate to the https://platform.openai.com/api-keys
* Create a new secret key
* Store this key securely - do not share with anyone

### Installing OpenAI Python package:
```
pip install openai
```

### Understanding different chat roles and system messages:
* System - behaviour of the model. For example: "You are a Python tutor."
* User - the one who instructs
* Assistant - ideal response to the user instruction

### Creating a Geography assistant using OpenAI:
This example shows how we can use chat models with ideal model responses to create awesome conversational applications. 

Example Question: Give me a quick summary of India.  
Example Answer: India is a country in Asia that borders Nepal. The capital city is New Delhi.  

It uses System > User-Assistant > User prompt strategy to achieve best results.

```
from openai import OpenAI
# Add your key here.
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

response = client.chat.completions.create(
 model="gpt-4o-mini",
# Add a user and assistant message for in-context learning
messages=[
    {"role": "system", "content": "You are a helpful Geography tutor that generates concise summaries for different countries."},
    {"role": "user", "content": "Give me a quick summary of India."},
    {"role": "assistant", "content": "India is a country in Asia that borders Nepal. The capital city is New Delhi."},
    {"role": "user", "content": "Give me a quick summary of Greece."}
    ]
)

print(response.choices[0].message.content)
```

#### Example output:
Greece is located in Southeast Europe, bordered by the Aegean Sea, the Ionian Sea, and the Mediterranean Sea. Its capital is Athens, known for its rich history and ancient landmarks like the Acropolis. Greece is famous for its islands, including Crete and Santorini, and is known for its contributions to philosophy, art, and democracy.

### Creating conversation histroy:
Here how responses to user messages can be stored in a message history, which will enable full conversations is demonstrated.

```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

messages = [{"role": "system", "content": "You are a helpful math tutor that speaks concisely."}]
user_msgs = ["Explain what pi is.", "Summarize this in two bullet points."]

# Loop over the user questions
for q in user_msgs:
    print("User: ", q)
    
    # Create a dictionary for the user message from q and append to messages
    user_dict = {"role": "user", "content": q}
    messages.append(user_dict)
    
    # Create the API request
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=messages,
        max_completion_tokens=100
    )
    
    # Append the assistant's message to messages
    assistant_dict = {"role": "assistant", "content": response.choices[0].message.content}
    messages.append(assistant_dict)
    print("Assistant: ", response.choices[0].message.content, "\n")
```
#### Example output:
User:  Explain what pi is.
Assistant:  Pi (π) is a mathematical constant representing the ratio of a circle's circumference to its diameter. It is approximately equal to 3.14159. Pi is an irrational number, meaning it has an infinite number of non-repeating decimal places. It is widely used in geometry, trigonometry, and calculus. 

User:  Summarize this in two bullet points.
Assistant:  - Pi (π) is the ratio of a circle's circumference to its diameter, approximately equal to 3.14159.  
- It is an irrational number with an infinite, non-repeating decimal expansion, commonly used in various areas of mathematics.
