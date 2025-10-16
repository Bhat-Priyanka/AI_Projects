# Best practices for writing prompts for business applications:

## Examples: 
### 1. Craft a prompt to summarize the product description:
prompt = f"""
Summarize the product description delimited by triple backticks, in at most five bullet points.
 ```{product_description}```
"""
