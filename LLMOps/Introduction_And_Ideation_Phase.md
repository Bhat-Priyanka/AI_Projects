# LLMOps - Large Language Model Operations:
- Helps to effectively manage, deploy, and maintain LLM applications.
- Seamless integration of LLMs into organizations.

## LLM: What is it?
- Pre-trained on large set of text data
- Can understand and generate hum-like texts
  
## LLMOps vs MLOps:
- LLMOps:
  - Model size is large
  - Process only text data
  - Pre-trained models
  - Includes prompt engineering and fine tuning
  - Usually general purposes
  - More unpredicted output - hallucinations
- MLOps:
  - Model size is small
  - Can process any type of data
  - Typlically not pre-trained models
  - Includes feature engineering and model selection
  - Fixed purpose
  - Predictable output

## Lifecycle of LLMs:
1. Ideation phase:
  - Data sourcing
  - Base model selection
2. Development phase:
  - Prompt engineering
  - Chains ans agents
  - RAG versus fine-tuning
  - Testing
3. Operational phase:
  - Deployment
  - Monitoring and observability
  - Cost management
  - Governance and security

## Ideation phase:
- Data sourcing:
  - Is the data relevant?
  - Is the data available?
    - Transformation of data, set up additional databases, evaluate costs for extenal data
    - consider other access limitations
  - Does the data meet standards?
    - Concerns quality and governance

- Selecting base models:
  - Pre-tarined models
  - Proprietary (privately owned) or open-source models
  - Consider performance, quality and speed
  - Consider license, cost and environmental impact 
