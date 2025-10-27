# Hugging Face:

## What is Hugging Face:
It is an open source AI community where people share their models. You can build with many such models and libraries. 
It is just like github for machine learning and AI applications.

## Tranformers Library:
- Simplifies working with pre-trained models

### Building a text generation pipeline using gpt2 model:
```
from transformers import pipeline 

gpt2_pipeline = pipeline(task="text-generation", model="openai-community/gpt2")

# Generate three text outputs with a maximum length of 10 tokens
results = gpt2_pipeline("What if AI", max_new_tokens=10, num_return_sequences=2)

for result in results:
    print(result['generated_text'])
```

#### Output:
    What if AI can be used to build new technologies?
    
    What if AI is just the beginning?
