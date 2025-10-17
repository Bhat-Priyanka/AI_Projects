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
    
