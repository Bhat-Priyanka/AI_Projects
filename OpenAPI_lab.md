# Working with OpenAPI and Python:

## Prerequisites

### Setting up your OpenAI API access:
* Create an account at https://openai.com/
* Navigate to the https://platform.openai.com/api-keys
* Create a new secret key
* Store this key securely - do not share with anyone

### Installing OpenAPI Python package:
<br /> <code> 
pip install openai
</code> 

### Creating a Geography assistant using OpenAPI:
This example shows how we can use chat models with ideal model responses to create awesome conversational applications.
Example Question: Give me a quick summary of India.
Example Answer: India is a country in Asia that borders Nepal. The capital city is New Delhi.
It uses System > User-Assistant > User prompt strategy to achieve best results.

from openai import OpenAI

# Add your key here.
<br /> <code> 
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

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
</code>

Example output:
Greece is located in Southeast Europe, bordered by the Aegean Sea, the Ionian Sea, and the Mediterranean Sea. Its capital is Athens, known for its rich history and ancient landmarks like the Acropolis. Greece is famous for its islands, including Crete and Santorini, and is known for its contributions to philosophy, art, and democracy.
)

print(response.choices[0].message.content)
