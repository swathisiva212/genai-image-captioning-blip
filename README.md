## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Automated image captioning involves generating descriptive text for visual content, an essential capability for applications in accessibility, multimedia retrieval, and automated content creation. The challenge is to produce accurate and meaningful captions using pre-trained models while ensuring ease of use for end users. This project leverages the BLIP model to address these challenges, with a Gradio-powered interface for user interaction and evaluation.

## DESIGN STEPS:
## STEP 1: Model Preparation
Load the BLIP model (Salesforce/blip-image-captioning-base) from Hugging Face Transformers.
Ensure the model is configured for generating captions from images.
## STEP 2: Application Development
Develop a function that takes an image as input and generates a caption.
Set up Gradio widgets to accept user-uploaded images and display outputs.
## STEP 3: Deployment and Testing
Host the Gradio app on a suitable platform like Google Colab or Hugging Face Spaces.
Test the prototype with diverse images to validate caption accuracy.

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
# Helper functions
import requests, json

#Image-to-text endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_ITT_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
image_url = "https://free-images.com/md/7687/blue_jay_bird_nature.jpg"
display(IPython.display.Image(url=image_url))
get_completion(image_url)
import gradio as gr 

def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    return result[0]['generated_text']

gr.close_all()
demo = gr.Interface(fn=captioner,
                    inputs=[gr.Image(label="Upload image", type="pil")],
                    outputs=[gr.Textbox(label="Caption")],
                    title="Image Captioning with BLIP",
                    description="Caption any image using the BLIP model",
                    allow_flagging="never",
                    examples=["christmas_dog.jpeg", "bird_flight.jpeg", "cow.jpeg"])
demo.launch()
#demo.launch(share=True, server_port=int(os.environ['PORT1']))
gr.close_all()
### OUTPUT:
![image](https://github.com/user-attachments/assets/a2c3aee8-cc0f-4729-a0ef-fb0fd655c622)


### RESULT:
Successfully a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.
