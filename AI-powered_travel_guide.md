# Start your code here!
import os
from openai import OpenAI

# Define the model to use
model = "gpt-4o-mini"

# Define the client
client = OpenAI()

conversation = [{"role": "system", "content": "You are a helpful tourist guide who speaks concisely about various locations and distances."}]
user_msgs = ["How far away is the Louvre from the Eiffel Tower (in miles) if you are driving?", "Where is the Arc de Triomphe?", "What are the must-see artworks at the Louvre Museum?"]

# Loop over the user questions
for q in user_msgs:

    # Create a dictionary for the user message from q and append to conversation
    user_dict = {"role": "user", "content": q}
    conversation.append(user_dict)
    
    # Create the API request
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=conversation,
        max_completion_tokens=100
    )
    
    # Append the assistant's message to messages
    assistant_dict = {"role": "assistant", "content": response.choices[0].message.content}
    conversation.append(assistant_dict)

Assistant:  The distance from the Louvre to the Eiffel Tower is approximately 3.5 miles (5.6 kilometers) when driving. 

Assistant:  The Arc de Triomphe is located at the western end of the Champs-Élysées in Paris, France. It stands at the Place Charles de Gaulle and honors those who fought and died for France in the French Revolutionary and Napoleonic Wars. 

Assistant:  At the Louvre Museum, must-see artworks include:

1. **Mona Lisa** by Leonardo da Vinci - Famous for its enigmatic smile.
2. **Venus de Milo** - A renowned ancient Greek statue representing the goddess Aphrodite.
3. **Winged Victory of Samothrace** - A stunning Hellenistic sculpture of the goddess Nike.
4. **The Coronation of Napoleon** by Jacques-Louis David - A grand neoclassical painting depicting Napoleon's coronation.
5. 
    print("Assistant: ", response.choices[0].message.content, "\n")
