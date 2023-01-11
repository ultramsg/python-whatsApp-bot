# [Ultramsg.com](https://ultramsg.com/?utm_source=github&utm_medium=python&utm_campaign=chatbot) WhatsApp Bot using WhatsApp API and ultramsg
Demo WhatsApp API ChatBot using [Ultramsg API](https://ultramsg.com/?utm_source=github&utm_medium=python&utm_campaign=chatbot) with python.
# Opportunities and tasks:
- The output of the command list .
- The output of the server time of the bot running on .
- Sending image to phone number or group .
- Sending audio file .
- Sending ppt audio recording .
- Sending Video File.
- Sending contact .

# Getting Started
- Ultramsg account is required to run examples. Log in or Create Account if you don't have one [ultramsg.com](https://ultramsg.com/?utm_source=github&utm_medium=python&utm_campaign=chatbot).
- go to your instance or Create one if you haven't already.
- Scan Qr and make sure that instance Auth Status : authenticated

## install flask
the WebHook URL must be provided for the server to trigger our script for incoming messages. we will deployed the server using the FLASK microframework. The FLASK server allows us to conveniently process incoming requests.

> pip install flask

Then clone the repository for yourself.
Then go to the **ultrabot.py** file and replace the ultraAPIUrl and instance token.

## install ngrok
for local development purposes, a tunneling service is required. This example uses ngrok , You can download ngrok from here : [ngrok](https://ngrok.com/download) .


Class constructor, the default one that will accept JSON, which will contain information about incoming messages (it will be received by WebHook and forwarded to the class). To see how the received JSON will look this [video](https://www.youtube.com/watch?v=kipBHDOsFKI) .


# Run a chatbot

## Run FLASK server  
> flask run

## Run ngrok

### Run ngrok For Windows :
> ngrok http 5000

### Run ngrok For Mac :
> ./ngrok http 5000

# Functions
## send_request 
Used to send requests to the Ultramsg API
```python
def send_requests(self, type, data):
    url = f"{self.ultraAPIUrl}{type}?token={self.token}"
    headers = {'Content-type': 'application/json'}
    answer = requests.post(url, data=json.dumps(data), headers=headers)
    return answer.json()
```
- **type** determines the type message .
- **data** contains the data required for sending requests.

## send_message
Used to send WhatsApp text messages
```python
def send_message(self, chatID, text):
    data = {"to" : chatID,
        "body" : text}  
    answer = self.send_requests('messages/chat', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us . 
- Text – Text of the message .


## time
Sends the current server time .
```python
def time(self, chatID):
    t = datetime.datetime.now()
    time = t.strftime('%d:%m:%Y')
    return self.send_message(chatID, time)
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .

## send_image
Send a image to phone number or group
```python
def send_image(self, chatID):
    data = {"to" : chatID,
        "image" : "https://file-example.s3-accelerate.amazonaws.com/images/test.jpeg"}  
    answer = self.send_requests('messages/image', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .

## send_video
Send a Video to phone number or group
```python
def send_video(self, chatID):
    data = {"to" : chatID,
        "video" : "https://file-example.s3-accelerate.amazonaws.com/video/test.mp4"}  
    answer = self.send_requests('messages/video', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .

## send_audio
Send a audio file to phone number or group
```python
def send_audio(self, chatID):
    data = {"to" : chatID,
        "audio" : "https://file-example.s3-accelerate.amazonaws.com/audio/2.mp3"}  
    answer = self.send_requests('messages/audio', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .


## send_voice
Send a ppt audio recording to phone number or group
```python
def send_voice(self, chatID):
    data = {"to" : chatID,
        "audio" : "https://file-example.s3-accelerate.amazonaws.com/voice/oog_example.ogg"}  
    answer = self.send_requests('messages/voice', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .

## send_contact
Sending one contact or contact list to phone number or group
```python
def send_contact(self, chatID):
    data = {"to" : chatID,
        "contact" : "14000000001@c.us"}  
    answer = self.send_requests('messages/contact', data)
    return answer
```
- ChatID – ID of the chat where the message should be sent for him, e.g 14155552671@c.us .

# Incoming message processing
```python
def Processingـincomingـmessages(self):
    if self.dict_messages != []:
        message =self.dict_messages
        text = message['body'].split()
        if not message['fromMe']:
        chatID  = message['from'] 
        if text[0].lower() == 'hi':
            return self.welcome(chatID)
        elif text[0].lower() == 'time':
            return self.time(chatID)
        elif text[0].lower() == 'image':
            return self.send_image(chatID)
        elif text[0].lower() == 'video':
            return self.send_video(chatID)
        elif text[0].lower() == 'audio':
            return self.send_audio(chatID)
        elif text[0].lower() == 'voice':
            return self.send_voice(chatID)
        elif text[0].lower() == 'contact':
            return self.send_contact(chatID)
        else:
            return self.welcome(chatID, True)
        else: return 'NoCommand'
```

# Flask 
To process incoming messages to our server 

```python
from flask import Flask, request, jsonify
from ultrabot import ultraChatBot
import json

app = Flask(__name__)

@app.route('/', methods=['POST'])
def home():
    if request.method == 'POST':
    bot = ultraChatBot(request.json)
    return bot.Processingـincomingـmessages()

if(__name__) == '__main__':
    app.run()

```

We will write the path app.route('/', methods = ['POST']) for it. This decorator means that our home function will be called every time our FLASK server .

