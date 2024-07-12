# Flask Chatbot with Gemini API

This is a simple chatbot built using the Flask framework and Gemini API. It handles user inputs and responds with appropriate answers using the Gemini API.

## Features

- Handles user inputs via HTTP requests.
- Integrates with Gemini API to fetch responses.
- Easy to deploy and extend.

## Requirements

- Python 3.6 or higher
- Flask
- Requests

## Installation

1. Clone the repository:

```bash
git clone https://github.com/vemulavarichandana/flask-chatbot-gemini.git
cd flask-chatbot-gemini

python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`

pip install -r requirements.txt

export GEMINI_API_KEY='your_gemini_api_key'  # On Windows use `set GEMINI_API_KEY=your_gemini_api_key`


## app.py

```python
from flask import Flask, request, jsonify
import requests
import os

app = Flask(__name__)
GEMINI_API_KEY = os.getenv('GEMINI_API_KEY')

@app.route('/')
def index():
    return "Hello! This is a chatbot powered by Flask and Gemini API."

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message')
    if not user_message:
        return jsonify({'error': 'No message provided'}), 400

    gemini_response = get_gemini_response(user_message)
    return jsonify({'response': gemini_response})

def get_gemini_response(message):
    headers = {
        'Authorization': f'Bearer {GEMINI_API_KEY}',
        'Content-Type': 'application/json'
    }
    data = {
        'query': message
    }
    response = requests.post('https://api.gemini.com/v1/chat', headers=headers, json=data)
    if response.status_code == 200:
        return response.json().get('response')
    else:
        return 'Error communicating with Gemini API'

if __name__ == '__main__':
    app.run(debug=True)

Flask==2.0.1
requests==2.25.1
