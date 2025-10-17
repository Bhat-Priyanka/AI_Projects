# Best practices for writing prompts for business applications:

## Examples: 
### 1. Craft a prompt to summarize the product description:
```
prompt = f"""
Summarize the product description delimited by triple backticks, in at most five bullet points.
 ```{product_description}```
"""
```

### 2. Craft a prompt to expand the product's description:
```
prompt = f"""Create expanded product description and write a one paragraph comprehensive overview capturing the key information of the product: unique features, benefits, and potential applications,paradelimited by three backticks ```{product_description}```
"""
```
#### Output:
    Original description: 
     
    Product: "Smart Home Security Camera"
    - High-tech security camera with night vision and motion detection.
    - Easy setup and remote monitoring.
    - Two-way audio communication for real-time interaction.
    - Mobile app integration for convenient control and alerts.
    - Weather-resistant design for both indoor and outdoor use.
    - Smart AI algorithms for advanced person and object detection.
    - Cloud storage and local backup options for recorded footage.
    - Infrared LEDs for clear imaging even in complete darkness.
    - Customizable motion zones to focus on specific areas.
    - Compatibility with voice assistants for hands-free control.
    
    Expanded description: 
     The "Smart Home Security Camera" is a cutting-edge surveillance solution designed to enhance your home security with its array of advanced features. Equipped with night vision and motion detection capabilities, this camera ensures that you can monitor your property around the clock, even in complete darkness, thanks to its infrared LEDs. The easy setup process and mobile app integration allow for seamless remote monitoring and control, enabling you to receive real-time alerts and interact with visitors through two-way audio communication. Its weather-resistant design makes it suitable for both indoor and outdoor applications, while smart AI algorithms provide advanced person and object detection, ensuring that you only receive relevant notifications. With customizable motion zones, you can focus on specific areas of interest, and the option for cloud storage and local backup ensures that your recorded footage is secure and accessible. Additionally, compatibility with voice assistants offers hands-free control, making this security camera a versatile and essential addition to any smart home.
    
## Examples for text transformation:
### 1. Translation for multilingual communication
```
prompt = f""""Translate the text from English to French, Spanish, and Japanese, delimited by three backticks  ```{marketing_message}```
"""
```

### 2. Craft a prompt to change the email's tone
```
prompt = f"""Transform the given email by changing its tone to be professional, positive, and user-centric, delimited by three backticks ```{sample_email}```
"""
```
#### Output:
Before transformation: 
 
Subject: Check out our latest products!

Dear Customer,

We are excited to introduce our latest product line that includes a wide range of items to suit your needs. Whether you're looking for electronics, home appliances, or fashion accessories, we have it all!

Hurry and visit our website to explore the fantastic deals and discounts we have for you. Don't miss out on the opportunity to get the best products at unbeatable prices.

Thank you for being a valued customer, and we look forward to serving you soon!

Best regards,
The Marketing Team

After transformation: 
Subject: Discover Our Exciting New Product Line!

Dear Valued Customer,

We are thrilled to share our latest product line with you, designed to meet a variety of your needs. From cutting-edge electronics to stylish home appliances and fashionable accessories, we have something for everyone!

We invite you to visit our website to explore the fantastic deals and exclusive discounts available just for you. This is a wonderful opportunity to find high-quality products at exceptional prices.

Thank you for being an important part of our community. We look forward to serving you and helping you find exactly what you’re looking for!

Warm regards,  
The Marketing Team

### 2. Craft a prompt to transform the text:
```
prompt = f"""Proofread the given text without changing its structure, and then adjusts its tone to be formal and friendly step by step, delimited by three backticks ```{text}```
"""
```
#### Output:
output:
    Before transformation:
     
    Hey guys, wanna know a cool trick? Here's how u can up your productivity game! First, download this awesome app, it's like the best thing ever! Then, just start using it and u'll see the difference. Its super easy and fun, trust me! So, what are u waiting for? Try it out now!
    
    After transformation:
     Sure! Let's start by proofreading the text for any grammatical or spelling errors.
    
    **Proofread Text:**
    Hey guys, want to know a cool trick? Here's how you can up your productivity game! First, download this awesome app; it's like the best thing ever! Then, just start using it, and you'll see the difference. It's super easy and fun, trust me! So, what are you waiting for? Try it out now!
    
    Now, let's adjust the tone to be formal and friendly step by step.
    
    Step 1: Address the audience more formally.
    Hello everyone, would you like to learn a useful tip?
    
    Step 2: Use complete words and a more formal tone.
    Here is how you can enhance your productivity.
    
    Step 3: Replace informal phrases with more formal alternatives.
    First, please download this excellent application; it is truly remarkable.
    
    Step 4: Encourage action in a friendly manner.
    Then, simply begin using it, and you will notice the difference.
    
    Step 5: Maintain a friendly tone while being formal.
    It is very easy and enjoyable, I assure you!
    
    Step 6: Conclude with a friendly call to action.
    So, what are you waiting for? I encourage you to try it out today!
    
    **Final Formal and Friendly Version:**
    Hello everyone, would you like to learn a useful tip? Here is how you can enhance your productivity. First, please download this excellent application; it is truly remarkable. Then, simply begin using it, and you will notice the difference. It is very easy and enjoyable, I assure you! So, what are you waiting for? I encourage you to try it out today!

### 3. Craft a prompt to classify the ticket
```
prompt = f"""Classify the given ticket as technical issue, billing inquiry, or product feedback, without providing anything else in the response, delimited by three backticks ```{ticket}```"""
```

#### Output:
   Ticket:  
    Subject: Urgent - Login Error
    
    Hi Support Team,
    
    I'm having trouble accessing my account with the username "example_user." Every time I try to log in, I encounter an error message. I've already attempted to reset my password, but the issue persists. I need to resolve this problem urgently, as I have pending tasks that require immediate attention.
    
    Please investigate and assist promptly.
    
    Thanks,
    John.
    
    Class:  ```
    technical issue
    ```
### 4. Craft a few-shot prompt to get the ticket's entities
```
prompt = f"""
Extract entities from the ticket:
{ticket_1} -> {entities_1}
{ticket_2} -> {entities_2}
{ticket_3} -> {entities_3}
{ticket_4} ->
"""
```

### 5. Craft a prompt that asks the model for the function:
```
examples="""input = [10, 5, 8] -> output = 23
input = [5, 2, 4] -> output = 11
input = [2, 1, 3] -> output = 6
input = [8, 4, 6] -> output = 18
"""

prompt = f"""
Infer the Python function that maps the inputs to the outputs in the provided examples which are delimited by triple backticks ```{examples}```.
"""
```
