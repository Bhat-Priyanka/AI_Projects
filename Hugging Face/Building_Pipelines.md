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

### Summarizing long texts:
Two types:
1. Extractive: selects key sentences from original text
2. Abstractive: generates new sentences summarizing main ideas

### Example for abstractive summarization pipeline using cnicu/t5-small-booksum model:
```
# Create the summarization pipeline
summarizer = pipeline(task="summarization", model="cnicu/t5-small-booksum", min_new_tokens=1, max_new_tokens=10)

# Summarize the text
summary_text = summarizer(original_text)

# Compare the length
print(f"Original text length: {len(original_text)}")
print(f"Summary: {summary_text}")
print(f"Summary length: {len(summary_text[0]['summary_text'])}")
```

### Auto Classes:
- Flexible access to models and tokenizers, provides more control
- Ideal for advanced tasks
- Pipelines - quick, auto classes - flexible

### AutoModels:
- Choose autoModel class to directly download model

### AutoTokenizers:
- Prepare text input data, use tokenizer paired with model for best results

### Example: tokenizing text with AutoTokenizer:
```
# Import necessary library for tokenization
from transformers import AutoTokenizer

# Load the tokenizer
tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

# Split input text into tokens
tokens = tokenizer.tokenize("AI: Making robots smarter and humans lazier!")

# Display the tokenized output
print(f"Tokenized output: {tokens}")
```
#### Output:
    Tokenized output: ['ai', ':', 'making', 'robots', 'smarter', 'and', 'humans', 'la', '##zier', '!']

### AutoClasses example:
```
from transformers import AutoModelForSequenceClassification, AutoTokenizer, pipeline

# Download the model and tokenizer
my_model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
my_tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

# Create the pipeline
my_pipeline = pipeline(task="sentiment-analysis", model=my_model, tokenizer=my_tokenizer)

# Predict the sentiment
output = my_pipeline("This course is pretty good, I guess.")
print(f"Sentiment using AutoClasses: {output[0]['label']}")
```
#### Output:
Sentiment using AutoClasses: POSITIVE

### Extracting text with PyPDF:
```
from pypdf import PdfReader

# Extract text from the PDF
reader = PdfReader("example.pdf")

# Extract text from all pages
document_text = ""
for page in reader.pages: 
    document_text += page.extract_text()

print(document_text)
```

### Q&A pipeline:
```
# Load the question-answering pipeline
qa_pipeline = pipeline(task="question-answering", model="distilbert-base-cased-distilled-squad")

question = "What is the notice period for resignation?"

# Get the answer from the QA pipeline
result = qa_pipeline(question=question, context=document_text)

# Print the answer
print(f"Answer: {result['answer']}")
```
