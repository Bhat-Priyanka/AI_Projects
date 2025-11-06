## Organizing large anonymized datasets with OpenAI:

### Task:
- Use OpenAI API to automatically extract medical information from these transcripts and automate the matching with the appropriate ICD-10 codes.
- Assume datasets are provided in csv file.
- Extract "age", "medical_specialty", and a new data field to store the recommended treatment extracted from each transcription.
- Match each recommended treatment with the corresponding ICD code, and save your answers in a pandas DataFrame named df_structured.

#### Column	Description
"medical_specialty"	- The medical specialty associated with each transcription.
"transcription"	- Detailed medical transcription texts, with insights into the medical case.

#### Concepts used:
- function calling to extract necessary information in predefined format
#### Code:
```
# Initialize the OpenAI client
import pandas as pd
from openai import OpenAI
import json

client = OpenAI()

# Load the data
df = pd.read_csv("data/transcriptions.csv")

def get_medical_data(transcription):
    """Extracts age and recommended treatment from a transcription using OpenAI."""

    # Define the function definitions for the tools
    function_definition = [
        {
            'type': 'function',
            'function': {
                'name': 'extract_info',
                'description': 'Get the age and recommended treatment from the input text. Always return both age and recommended treatment..',
                'parameters': {
                    'type': 'object',
                    'properties': {
                        'age': {'type': 'integer', 'description': 'age of the patient'},
                        'treatment': {'type': 'string', 'description': 'recommended treatment or procedure'}
                    }
                }
            }
        }
    ]
    messages = [
    {
            "role": "system",
            "content": "You are a healthcare professional extracting patient data. Always return both the age and recommended treatment. If the information is missing, still create the field and specify 'Unknown'.",
            "role": "user",
            "content": f"Please extract and return both the patient's age and recommended treatment from the following transcription. Return only exact treatment. Do not incluide additional information.Transcription: {transcription}."
        }
    ]
    # Call the model with both function definitions
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=messages,
        tools=function_definition
    )
    return json.loads(response.choices[0].message.tool_calls[0].function.arguments)

def get_icd_codes(treatment):
    if treatment != 'Unknown':
        """Retrieves ICD codes for a given treatment using OpenAI."""
        messages = [
        {
            "role": "user",
            "content": f"Provide the ICD codes for the following treatment or procedure: {treatment}. Return the answer as a list of codes. Please only include the codes and no other information."
        }
        ]
        response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=messages
        )
        output = response.choices[0].message.content
    else:
        output="Unknown"
    return output

# Start an empty list to store processed data
processed_data = []

for index, row in df.iterrows():
    medical_speciality = row['medical_specialty']
    info = get_medical_data(row['transcription'])
    info["Medical Specialty"] = medical_speciality
    icd_code = get_icd_codes(info['treatment'])
    info["ICD Code"] = icd_code
    processed_data.append(info)

# Convert the list to a DataFrame
df_structured = pd.DataFrame(processed_data)
```
