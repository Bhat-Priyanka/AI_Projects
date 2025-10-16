## Few-shot prompting:
- Give examples for the model in the form of question-answer pairs
- Zero - Zero-shot prompting - ideal for quick and simple tasks
- One - One-shot prompting - for consitent formatting or style
- One - Few-shot prompting - for contextual tasks - example: sentiment analysis

### Example for One-shot:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create a one-shot prompt
prompt = """
     Q: Extract the odd numbers from {1, 3, 7, 12, 19}. A: Odd numbers = {1, 3, 7, 19}
     Q: Extract the odd numbers from {3, 5, 11, 12, 16}. A:
"""

response = get_response(prompt)
print(response)
```
#### Output:
Odd numbers = {3, 5, 11}

### Sentiment analysis with few-shot prompting
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

response = client.chat.completions.create(
  model = "gpt-4o-mini",
  messages = [{"role": "user", "content": "The product quality exceeded my expectations"},
              {"role": "assistant", "content": "1"},
              {"role": "user", "content": "I had a terrible experience with this product's customer service"},
              {"role": "assistant", "content": "-1"},
              # Provide the text for the model to classify
              {"role": "user", "content": "The price of the product is really fair given its features"}
             ],
  temperature = 0
)
print(response.choices[0].message.content)
```
#### Output: 
1
