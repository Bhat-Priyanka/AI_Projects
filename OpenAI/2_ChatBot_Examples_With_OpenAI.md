# Chatbot projects with OpenAI:

## Preparation:
- Create a python function for creating dual prompt: get_response(system_prompt, user_prompt)
  
### Python function to create dual prompt: for system and user role:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

def get_response(system_prompt, user_prompt):
  # Assign the role and content for each message
  messages = [{"role": "system", "content": system_prompt},
      		  {"role": "user", "content": user_prompt}]  
  response = client.chat.completions.create(
      model="gpt-4o-mini", messages= messages, temperature=0)
  
  return response.choices[0].message.content

# Try the function with a system and user prompts of your choice 
response = get_response("You are a financial advisor", "What is a stock?")
print(response)
```

## Example: Create customer support chatbot for troubleshooting:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the purpose of the chatbot
chatbot_purpose = "You are a customer support chatbot for an e-commerce company specializing in electronics and you assist with inquiries, order tracking, and troubleshooting."

# Define audience guidelines
audience_guidelines = "Target audience are tech-savvy individuals interested in purchasing electronic gadgets and save to audience_guidelines."

# Define tone guidelines
tone_guidelines = "Use a professional and user-friendly tone while interacting with customers."

system_prompt = chatbot_purpose + audience_guidelines + tone_guidelines
response = get_response(system_prompt, "My new headphones aren't connecting to my device")
print(response)
```

## Example: Refined customer support:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the order number condition
order_number_condition = "Ask the user for their order number if they submitted a query about an order without specifying an order number."

# Define the technical issue condition
technical_issue_condition = "Start the response with I'm sorry to hear about your issue with ... if the user is reporting a technical issue."

# Create the refined system prompt
refined_system_prompt = order_number_condition + technical_issue_condition

response_1 = get_response(refined_system_prompt, "My laptop screen is flickering. What should I do?")
response_2 = get_response(refined_system_prompt, "Can you help me track my recent order?")

print("Response 1: ", response_1)
print("Response 2: ", response_2)
```

## Example: Book recommendation and learning path advisor:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

base_system_prompt = "Act as a learning advisor who receives queries from users mentioning their background, experience, and goals, and accordingly provides a response that recommends a tailored learning path of textbooks, including both beginner-level and more advanced options."

# Define behavior guidelines
behavior_guidelines = "Ask a user about their background, experience, and goals, whenever any of these is not provided in the prompt."

# Define response guidelines
response_guidelines = "Recommend no more than three textbooks"

system_prompt = base_system_prompt + behavior_guidelines + response_guidelines
user_prompt = "Hey, I'm looking for courses on Python and data visualization. What do you recommend?"
response = get_response(system_prompt, user_prompt)
print(response)
```

## Example: Customer support for a delivery service:
```
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Define the system prompt
system_prompt = "You are a customer service chatbot for a delivery service that supports customers with whatever they need and answer questions in a gentle way."

context_question = "What types of items can be delivered using MyPersonalDelivery?"
context_answer = "We deliver everything from everyday essentials such as groceries, medications, and documents to larger items like electronics, clothing, and furniture. However, please note that we currently do not offer delivery for hazardous materials or extremely fragile items requiring special handling."

# Add the context to the model
response = client.chat.completions.create(
  model="gpt-4o-mini",
  messages=[{"role": "system", "content": system_prompt},
            {"role": "user", "content": context_question},
            {"role": "assistant", "content": context_answer },
            {"role": "user", "content": "Do you deliver furniture?"}])
response = response.choices[0].message.content
print(response)
```
