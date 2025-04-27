# Weekly-task3
# virtual_assistant.py

import requests
import time
import logging

# Setup logging
logging.basicConfig(filename='assistant.log', level=logging.INFO, format='%(asctime)s - %(message)s')

# OpenWeatherMap API (You must replace 'YOUR_API_KEY' with your actual API key)
WEATHER_API_KEY = 'YOUR_API_KEY'
WEATHER_API_URL = 'https://api.openweathermap.org/data/2.5/weather'

# Simple reminder list
reminders = []

def get_weather(city_name):
    params = {'q': city_name, 'appid': WEATHER_API_KEY, 'units': 'metric'}
    try:
        response = requests.get(WEATHER_API_URL, params=params)
        data = response.json()
        if data.get('cod') != 200:
            return "City not found."
        temp = data['main']['temp']
        description = data['weather'][0]['description']
        return f"The weather in {city_name} is {temp}°C with {description}."
    except Exception as e:
        return f"Error fetching weather: {e}"

def add_reminder(reminder_text):
    reminders.append(reminder_text)
    logging.info(f"Reminder set: {reminder_text}")
    return f"Reminder added: {reminder_text}"

def show_reminders():
    if not reminders:
        return "No reminders set."
    return "\n".join(f"{idx+1}. {reminder}" for idx, reminder in enumerate(reminders))

def calculator():
    print("\n--- Calculator ---")
    try:
        num1 = float(input("Enter first number: "))
        op = input("Enter operator (+, -, *, /): ")
        num2 = float(input("Enter second number: "))
        
        if op == '+':
            result = num1 + num2
        elif op == '-':
            result = num1 - num2
        elif op == '*':
            result = num1 * num2
        elif op == '/':
            if num2 == 0:
                print("Error: Cannot divide by zero.")
                return
            result = num1 / num2
        else:
            print("Invalid operator.")
            return
        print(f"Result: {result}")
        logging.info(f"Calculated: {num1} {op} {num2} = {result}")
    except ValueError:
        print("Invalid input. Please enter numbers.")

def main():
    print("Welcome to the Simple Virtual Assistant!")
    print("Available commands: weather, reminder, show reminders, calculator, exit")
    
    while True:
        command = input("\nEnter command: ").lower()
        
        if command == "weather":
            city = input("Enter city name: ")
            weather_info = get_weather(city)
            print(weather_info)
        
        elif command == "reminder":
            reminder_text = input("Enter reminder: ")
            message = add_reminder(reminder_text)
            print(message)
        
        elif command == "show reminders":
            print(show_reminders())
        
        elif command == "calculator":
            calculator()
        
        elif command == "exit":
            print("Goodbye!")
            break
        
        else:
            print("Unknown command. Try again.")

if __name__ == "__main__":
    main()
