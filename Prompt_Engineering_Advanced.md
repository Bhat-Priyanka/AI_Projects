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

### Examples for multi-step:
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
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

code = '''
def calculate_rectangle_area(length, width):
    area = length * width
    return area
'''

# Create a prompt that analyzes correctness of the code
prompt = f"""
     Analyze the correctness of the function delimited by triple backticks according to the following criteria:
      1- It should have correct syntax
      2- The function should receive only 2 inputs
      3- The function should return only one output
      ```{code}```
    """

response = get_response(prompt)
print(response)
```
### Chain-of-thought prompting:
- Requires LLMs to provide reasoning steps (thoughts) before giving answers.
- Used for complex reasoning tasks.
- Help reduce model errors.
  
#### Example:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create the chain-of-thought prompt
prompt = """Q: Determine my friend's father's age in 10 years, given that he is currently twice my friend's age, and y friend is 20.
A: Lets think step by step."""

response = get_response(prompt)
print(response)
```
##### Output:
```
    Sure! Let's break it down step by step.
    
    1. **Current Age of Your Friend**: Your friend is currently 20 years old.
    
    2. **Current Age of Your Friend's Father**: Since your friend's father is currently twice your friend's age, we can calculate his age as follows:
       \[
       \text{Father's current age} = 2 \times \text{Friend's current age} = 2 \times 20 = 40 \text{ years old}
       \]
    
    3. **Father's Age in 10 Years**: To find out how old your friend's father will be in 10 years, we add 10 years to his current age:
       \[
       \text{Father's age in 10 years} = \text{Father's current age} + 10 = 40 + 10 = 50 \text{ years old}
       \]
    
    So, your friend's father will be 50 years old in 10 years.
```
#### One-shot chain-of-thought prompts:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the example 
example = """Q: Sum the even numbers in the following set: {9, 10, 13, 4, 2}.
             A: Even numbers: 10, 4 ,2. Adding them: 10+4+2=16"""

# Define the question
question = """Q:Sum the even numbers in the following set: {15, 13, 82, 7, 14}
              A:"""

# Create the final prompt
prompt = example + question
response = get_response(prompt)
print(response)
```
### Chain-of-thought vs Multi-step prompting:
- Multi-step prompts: Incorporate steps inside prompt
- Chain-of-thought: Ask model to generate intermediate steps
       - One unsuccessful thought results in unsuccessful outcome.
       - So self-consistency prompts were introduced.

### Self-consistency prompting:
- Generate multiple chain-of-thoughts by prompting the model several times.
- Majority vote to obtain final output
- Can be done by defining multiple prompts or a prompt generating multiple responses.
