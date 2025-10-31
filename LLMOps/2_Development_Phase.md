## Development phase:

### Prompt Engineering:
- To improve performance
- Control over output

### Chains and  Agents:
- Need for chains:
  - For sophesticated applications
  - Modular design - enhancing scalability and operational efficiency
  - Customization
- Agents:
  - Consists of multiple actions (tools), LLM deciding which action to take
  - Useful when there are many actions, optimum sequence of steps is unknown, uncertain inputs

### Chains vs agents:
Chains: deterministic, less complexity, flexibility, and risk
Agents: adaptive, high complexity, flexibility, and risk

### RAG vs fine-tuning:
### RAG (Retrieval Augmented Generation):
- Combine LLM's reasoning capability with external knowledge
- Three steps in a chain:
  - Retrieve related documents
  - Augment prompt with examples
  - Generate output
- Often implementaed with vector databases

#### RAG-chain with vector databses:
1. Retrieve:
  - Convert input to embedding
  - Search vector databses
  - Retrieve most similar documents
2. Augment:
  - Combine input with top-k documents and create augmented prompt
3. Generate:
  - Use prompt to create output

### Fine-tuning:
- Adjusts LLM's weights
- Expand to specific tasks and domains
  - Different langauges
  - Specialized fields

#### Types of fine-tuning:
  1. Supervised (transfer learning):
     - Type of data needed : demonstration data - inputs with desired outputs
     - Approach: Re-trin (parts of) model
  2. Reinforcement learning from human feedback (RLHF):
     - Type of data: ranking or quality scores - obtained from likes and dislikes
     - Approach: train an extra reward model, optimize LLM to maximize this
    
### RAG or fine tuning?
RAG: 
  - when including factual knowledge, keeps capabilities of LLM, easy to implement
  - adds extra components - requires careful engineering
Fine tuning:
  - when specializing in new domain
  - full control and no extra components
  - needs labeled data and specialized knowledge, bias amplification, catastrophic forgetting

### Testing:
  - LLMs can make mistakes
  - Ready for deployment


