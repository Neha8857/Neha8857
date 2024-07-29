
import speech_recognition as sr
import pyttsx3
import pywhatkit
import wikipedia
import webbrowser
import smtplib
import os
import datetime
import requests

# Initialize speech recognition and text-to-speech engines
r = sr.Recognizer()
engine = pyttsx3.init()

# Define a function to process voice commands
def process_command(command):
    # Natural Language Processing (NLP) goes here
    if "what is" in command:
        # Search Wikipedia
        result = wikipedia.summary(command.replace("what is", ""), 2)
        engine.say(result)
    elif "open" in command:
        # Open a website or application
        webbrowser.open(command.replace("open", ""))
    elif "play" in command:
        # Play a song or video
        pywhatkit.playonyt(command.replace("play", ""))
    elif "time" in command:
        # Tell the current time
        engine.say(datetime.datetime.now().strftime("%H:%M:%S"))
    elif "email" in command:
        # Send an email
        send_email(command.replace("email", ""))
    elif "weather" in command:
        # Fetch weather data
        get_weather(command.replace("weather", ""))
    else:
        # Handle unknown commands
        engine.say("Sorry, I didn't understand that.")

# Define a function to send emails
def send_email(command):
    # Extract recipient and message from command
    recipient = command.split("to")[1].split("say")[0].strip()
    message = command.split("say")[1].strip()
    # Send email using SMTP
    server = smtplib.SMTP("(link unavailable)", 587)
    server.starttls()
    server.login("your-email@gmail.com", "your-password")
    server.sendmail("your-email@gmail.com", recipient, message)
    server.quit()

# Define a function to fetch weather data
def get_weather(command):
    # Extract location from command
    location = command.split("in")[1].strip()
    # Fetch weather data using API (e.g., OpenWeatherMap)
    api_key = "your-api-key"
    base_url = f"(link unavailable)"
    response = requests.get(base_url)
    weather_data = response.json()
    # Extract and read weather information
    weather_description = weather_data["weather"][0]["description"]
    temperature = weather_data["main"]["temp"]
    engine.say(f"Weather in {location}: {weather_description}, Temperature: {temperature}°C")

# Define a function to handle user interaction
def user_interaction():
    with sr.Microphone() as source:
        # Listen for voice commands
        audio = r.listen(source)
        try:
            # Recognize speech
            command = r.recognize_google(audio, language="en-US")
            process_command(command)
        except sr.UnknownValueError:
            # Handle speech recognition errors
            engine.say("Sorry, I didn't understand that.")
        except sr.RequestError:
            # Handle network request errors
            engine.say("Sorry, I'm experiencing technical difficulties.")

# Run the voice assistant
while True:
    user_interaction()
```