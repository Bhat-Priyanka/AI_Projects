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
 ```
Subject: Discover Our Exciting New Product Line!

Dear Valued Customer,

We are thrilled to share our latest product line with you, designed to meet a variety of your needs. From cutting-edge electronics to stylish home appliances and fashionable accessories, we have something for everyone!

We invite you to visit our website to explore the fantastic deals and exclusive discounts available just for you. This is a wonderful opportunity to find high-quality products at exceptional prices.

Thank you for being an important part of our community. We look forward to serving you and helping you find exactly what you’re looking for!

Warm regards,  
The Marketing Team
```
