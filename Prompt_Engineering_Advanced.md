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

## Multi-step prompting:
- Break down end goal into series of steps. Model goes through each step to give final output.
- Used for Sequential tasks, Cognitive tasks. Example: Analyse code step by step
  
### Difference between few-shot and multi-step:
- Few-shot : Questions and answers model learns from
- Multi-step: Explicitly telling model what to do

### Example for single-step:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create a single-step prompt to get help planning the vacation
prompt = "Help me plan a mountain vacation."

response = get_response(prompt)
print(response)
```

### Example for multi-step:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create a prompt detailing steps to plan the trip
prompt = """
     Help me plan a mountain vacation.
     Step 1 - Specify four potential locations for beach vacations
     Step 2 - State some accommodation options in each
     Step 3 - State activities that could be done in each
     Step 4 - Evaluate the pros and cons for each destination
    """

response = get_response(prompt)
print(response)
```
