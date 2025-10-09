# Prompt Engineering:

It is the best way to get desired response from LLM by giving specific and precise instructions. High quality prompts can lead to high quality responses.

## Key principles:
* Use precise and detailed instructions.
  *  Mention context, output length (limitation), format (example: f-strings) and style and audience  
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
from openai import OpenAI
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

story = "In a distant galaxy, there was a brave space explorer named Alex. Alex had spent years traveling through the cosmos, discovering new planets and meeting alien species. One fateful day, while exploring an uncharted asteroid belt, Alex stumbled upon a peculiar object that would change the course of their interstellar journey forever..."

# Create a prompt that completes the story
prompt = f"""Complete the story delimited by triple backticks. 
 ```{story}```"""

# Get the generated response 
response = client.chat.completions.create(
 model="gpt-4o-mini",
messages=[
    {"role": "system", "content": "You are a helpful assitant for story telling."},
    {"role": "user", "content": prompt}
    ]
)

print("\n Original story: \n", story)
print("\n Generated story: \n", response)
```
### Structured output:
* Generate structured outputs such as table, list, structured paragraphs, custom format.

#### Conditional prompts:
* If-else style prompt
  
#### Example: Generate a table
```
from openai import OpenAI
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create a prompt that generates the table
prompt = "Generate a table of 10 books with columns title, author, year for a science fiction lover."

# Get the response
response = client.chat.completions.create(
 model="gpt-4o-mini",
messages=[
    {"role": "system", "content": "You are a helpful assitant for book recommendation."},
    {"role": "user", "content": prompt}
    ]
)
print(response)
```

##### Example output:
Here’s a table of 10 science fiction books that any fan of the genre would appreciate:
    
    | Title                          | Author                | Year  |
    |--------------------------------|----------------------|-------|
    | Dune                           | Frank Herbert         | 1965  |
    | Neuromancer                    | William Gibson        | 1984  |
    | The Left Hand of Darkness      | Ursula K. Le Guin    | 1969  |
    | Foundation                     | Isaac Asimov         | 1951  |
    | Snow Crash                     | Neal Stephenson      | 1992  |
    | The Dispossessed               | Ursula K. Le Guin    | 1974  |
    | Hyperion                       | Dan Simmons          | 1989  |
    | The Martian                   | Andy Weir            | 2011  |
    | Ready Player One               | Ernest Cline         | 2011  |
    | The Three-Body Problem         | Liu Cixin            | 2008  |
    
    These titles represent a mix of classic and contemporary works in the science fiction genre.
