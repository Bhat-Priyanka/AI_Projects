# Chatbot project with OpenAI:

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
