* Run npm run dev on in agni-eye-sentinel\\
* Run python app.py in backend(LLM)\\backend
* Run python -m streamlit run st.py in agni-eye-sentinel\\

* This project relies on Gemini's vision model capabilities to analyze aerial imagery and classify as threat / non-threat






API Key Usage (Gemini Integration)



This module relies on the Gemini API key for sending EO/IR frames or detections for multimodal reasoning.



1\. Set up your Gemini API Key



Create an environment variable:



export GEMINI\_API\_KEY="your\_api\_key\_here"





On Windows (PowerShell):



setx GEMINI\_API\_KEY "your\_api\_key\_here"



2\. Usage in Code



The key is securely loaded from environment variables to prevent hard-coding sensitive credentials:



import os

from google import genai



GEMINI\_API\_KEY = os.getenv("GEMINI\_API\_KEY")



if not GEMINI\_API\_KEY:

&nbsp;   raise EnvironmentError("Missing Gemini API key. Please set GEMINI\_API\_KEY.")



client = genai.Client(api\_key=GEMINI\_API\_KEY)



3\. Vision Reasoning Example

response = client.models.generate\_content(

&nbsp;   model="gemini-1.5-pro-vision",

&nbsp;   contents=\[

&nbsp;       "Analyze the EO/IR image and determine if it indicates a potential threat. "

&nbsp;       "Consider objects, formations, and context.",

&nbsp;       {"mime\_type": "image/jpeg", "data": image\_bytes},

&nbsp;   ],

)



threat\_assessment = response.text

print("Gemini Threat Analysis:", threat\_assessment)



⚙ Installation \& Setup

Clone the Repository

git clone https://github.com/<your-username>/q-sentinel-eo-ir.git

cd q-sentinel-eo-ir



Install Dependencies

pip install -r requirements.txt



Environment Variables



Create a .env file:



GEMINI\_API\_KEY=your\_api\_key\_here

MODEL\_PATH=weights/yolov8\_eoir.pt

