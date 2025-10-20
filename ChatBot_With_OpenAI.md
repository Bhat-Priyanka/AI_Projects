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

# Craft the system_prompt using the role-playing approach
system_prompt = "You are a personalized learning advisor chatbot that recommends textbooks for users. Your role is to receive queries from learners about their background, experience, and goals, and accordingly, recommend a learning path of textbooks, including both beginner-level and more advanced options. "

user_prompt = "Hello there! I'm a beginner with a marketing background, and I'm really interested in learning about Python, data analytics, and machine learning. Can you recommend some books?"

response = get_response(system_prompt, user_prompt)
print(response)
```
