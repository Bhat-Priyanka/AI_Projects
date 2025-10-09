# Prompt Engineering:

It is the best way to get desired response from LLM by giving specific and precise instructions. High quality prompts can lead to high quality responses.

## Key principles:
* Use precise and detailed instructions.
  *  Mention:
    *  context, output length (limitation), format (example: f-strings) and style and audience  
* Use the right action verbs. Example: Write, complete, explain etc.
  * Do not use verbs like think, feel, know, undrestand, try etc. which are ambiguous.
* Use delimiters - parenthesis, braclets, backticks to specify input parts

### Creating a prompt to complete a story usinf f-string with tripple backticks (```):
#### Why use f-strings?
* f-strings are a concise and readable way to format strings in Python, introduced in Python 3.6. They allow you to embed expressions inside string literals using curly braces {}.

#### Why use backticks?
* The f""" syntax combines f-strings with triple-quoted strings in Python. Triple quotes allow you to create multi-line strings, and the f prefix enables formatted string literals.
* Backticks help distinguish code from regular text, making prompts clearer.
* Preventing Interpretation Issues: They ensure AI models treat content as literal text rather than instructions.
* Variable/Parameter Highlighting: Used to emphasize specific values or parameters

```
from OPENAI import OpenAI
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

story = "In a distant galaxy, there was a brave space explorer named Alex. Alex had spent years traveling through the cosmos, discovering new planets and meeting alien species. One fateful day, while exploring an uncharted asteroid belt, Alex stumbled upon a peculiar object that would change the course of their interstellar journey forever..."

# Create a prompt that completes the story
prompt = f"""Complete the story delimited by triple backticks. 
 ```{story}```"""

# Get the generated response 
response = get_response(prompt)

print("\n Original story: \n", story)
print("\n Generated story: \n", response)
```
