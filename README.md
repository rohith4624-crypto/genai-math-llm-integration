## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
### Problem Statement

Develop a Python-based application that calculates the volume of a cylinder using its radius and height. Integrate the Python calculation function with a Large Language Model (LLM) using OpenAI's function-calling feature. The system should allow the LLM to identify when the cylinder volume calculation is required, call the appropriate Python function with the required parameters, obtain the calculated result, and generate a clear natural-language response for the user. The API key must be securely stored and should not be exposed or committed to the project repository.

### DESIGN STEPS:

### step 1: Create a Python Function: Define a function to calculate the volume of a cylinder using the formula V = π × r² × h.
### step 2: Integrate with LLM Function Calling: Connect the Python function to the LLM so it can identify the user's request and call the function with the required radius and height.
### step 3: Generate the Final Response: Send the calculated result back to the LLM and generate a clear, natural-language response containing the cylinder's volume.
```
import os
import json
import math
import openai
from dotenv import load_dotenv

load_dotenv()

openai.api_key = os.getenv("OPENAI_API_KEY")

print("OpenAI API configured successfully")
def calculate_cylinder_volume(radius, height):
    volume = math.pi * (radius ** 2) * height
    return round(volume, 2)
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder using its radius and height.",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "The radius of the cylinder in centimeters."
                },
                "height": {
                    "type": "number",
                    "description": "The height of the cylinder in centimeters."
                }
            },
            "required": ["radius", "height"]
        }
    }
]

messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a cylinder with radius 5 cm and height 10 cm."
    }
]
response_message = response["choices"][0]["message"]

messages.append(response_message)

if "function_call" in response_message:

    function_name = response_message["function_call"]["name"]

    args = json.loads(
        response_message["function_call"]["arguments"]
    )

    if function_name == "calculate_cylinder_volume":

        radius = args["radius"]
        height = args["height"]

        result = calculate_cylinder_volume(
            radius,
            height
        )

        print("Function result:", result)
messages.append(
    {
        "role": "function",
        "name": "calculate_cylinder_volume",
        "content": json.dumps(
            {
                "radius": radius,
                "height": height,
                "volume": result
            }
        )
    }
)
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages
)
print("Function called: calculate_cylinder_volume")
print("Radius:", radius)
print("Height:", height)
print("Calculated volume:", result)

### OUTPUT:
<img width="556" height="137" alt="image" src="https://github.com/user-attachments/assets/a13c246f-2b26-466f-9d3b-06efe9ed7f12" />


### RESULT:
The Python function successfully calculated the volume of the cylinder using the radius and height provided by the user. The LLM successfully identified and called the required function, received the calculated result, and generated a clear natural-language response. For a cylinder with a radius of 5 cm and height of 10 cm, the calculated volume is 785.4 cubic centimeters (cm³).
