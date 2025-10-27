## Working with Hugging Face

### Example for text classification pipeline:
```
from transformers import pipeline

# Create a pipeline for grammar checking
grammar_checker = pipeline(
    task="text-classification",
    model="abdulmatinomotoso/English_Grammar_Checker"
)

# Check grammar of the input text
output = grammar_checker("I will walk dog")
print(output)
```

#### Output:
    [{'label': 'LABEL_0', 'score': 0.9956323504447937}]

### Question Natural Language Inference (QNLI):
- This checks if a premise contains enough information to answer a posed question, determining whether the answer can be found in the given text.

```
- # Create the pipeline
classifier = pipeline(task="text-classification", model="cross-encoder/qnli-electra-base")

# Predict the output
output = classifier("Where is the capital of France?, Brittany is known for its stunning coastline.")
```
#### Output:
[{'label': 'LABEL_0', 'score': 0.016211988404393196}]
print(output)

### Dynamic category assignment:
Dynamic category assignment enables a model to classify text into predefined categories, even without prior training for those categories.

```
from transformers import pipeline

text = "AI-powered robots assist in complex brain surgeries with precision."

# Create the pipeline
classifier = pipeline(task="zero-shot-classification", model="facebook/bart-large-mnli")

# Create the categories list
categories = ["politics", "science", "sports"]

# Predict the output
output = classifier(text, categories)

# Print the top label and its score
print(f"Top Label: {output['labels'][0]} with score: {output['scores'][0]}")
```
#### Output:
Top Label: science with score: 0.9510332942008972
