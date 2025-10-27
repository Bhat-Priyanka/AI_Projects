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

### Inference providers:
In cases where, the hardware to run Hugging Face models locally are not available, inference providers are used.. Large-parameter LLMs, and image and video generation models in particular often require Graphics Processing Units (GPUs) to parallelize the computations. Hugging Face providers inference providers to outsource this hardware to third-party partners.

### Loading Datasets:
Here "validation" split of the "TIGER-Lab/MMLU-Pro" dataset, which is a benchmark evaluation dataset is loaded.
```
from datasets import load_dataset

# Load the "validation" split of the TIGER-Lab/MMLU-Pro dataset
my_dataset = load_dataset("TIGER-Lab/MMLU-Pro", split="validation")

# Display dataset details
print(my_dataset)
```
#### Output:
Dataset({
        features: ['question_id', 'question', 'options', 'answer', 'answer_index', 'cot_content', 'category', 'src'],
        num_rows: 70
    })
