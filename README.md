

#install bllow all libery

#python_project

#pip install pywhatkit

#pip install openai

#pip install geopy

#pip install twilio

#pip install folium

#pip install pyperclip







import os
import subprocess
import urllib.parse
import pywhatkit
import openai
import smtplib
from twilio.rest import Client
import folium
from geopy.geocoders import Nominatim
#############################################################################################################
                                    #Email function#
def send_email():
    print("Thanks for choosing email service...")
    smtp_server = "smtp.gmail.com"
    email = "Enter your email"
    password = "Enter your password" 

    to_email = input("Enter recipient's email: ")
    subject = input("Enter the email subject: ")
    message = input("Enter the email message: ")


    with smtplib.SMTP(smtp_server, 587) as server:
            server.starttls()
            server.login(email, password)
            server.sendmail(email, to_email, f"Subject: {subject}\n\n{message}")
    print("Email sent successfully.")
    
#################################################################################################################
                                    #SMS function#
                
def send_sms(to_phone_number, message_body):
    print("Thanks for choosing SMS service...")
    account_sid = "Enter your sid acc"
    auth_token = "Enter your auth_token"

    from_phone_number = "+123456789" #twilio enter your number 


    client = Client(account_sid, auth_token)
    message = client.messages.create(
    body=message_body,
    from_=from_phone_number,
    to=to_phone_number
        )
    print(f"Message SID: {message.sid}")
    print("Message has been sent successfully.")
  
  ################################################################################################################
                                  #What's app function#
    
def send_whatsapp(to_phone_number, message_body, hour, minute):
    print("Thanks for choosing WhatsApp service...")
    
    try:
        hour = int(hour)
        minute = int(minute)
        pywhatkit.sendwhatmsg(f"+{to_phone_number}", message_body, hour, minute)
        print("WhatsApp message has been scheduled.")
    except Exception as e:
        print(f"Failed to schedule the WhatsApp message: {str(e)}")

##################################################################################################################
                          #Location function#
    

def find_location(location_name, output_file="location_map.html"):
    print("Thanks for choosing location service...")
    geolocator = Nominatim(user_agent="location_pinner")
    location = geolocator.geocode(location_name)

    if location:
        m = folium.Map(location=[location.latitude, location.longitude], zoom_start=10)
        folium.Marker([location.latitude, location.longitude], popup=location_name).add_to(m)
        m.save(output_file)
        print(f"Location map saved as {output_file}")
    else:
        print(f"Location not found: {location_name}")
        
        
####################################################################################################################
                                    #chat_gpt# 
        

# Set your OpenAI API key
openai.api_key = "Enter_your_openapi_key"

def chat_with_gpt(user_input):
    print("Thanks for choosing ChatGPT service...")
    
    try:
        response = openai.Completion.create(
            engine="davinci",
            prompt=user_input,
            max_tokens=50  # Adjust based on your needs
        )
        assistant_response = response.choices[0].text
        print("ChatGPT Response:", assistant_response)
    except Exception as e:
        print(f"Failed to interact with ChatGPT: {str(e)}")

      
######################################################################################################        
                          #chrome function#
        
        
def open_chrome_search(query):
    try:
        search_url = f"https://www.google.com/search?q={urllib.parse.quote(query)}"
        chrome_path = "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"  # Replace with the actual path
        subprocess.Popen([chrome_path, search_url])
        print(f"Searching for '{query}' in Chrome...")
    except Exception as e:
        print(f"Failed to open Chrome: {str(e)}")

        
##############################################################################################################


def main_menu():
    while True:
        print("\t\t\t\t\tMy Project")
        print("********************************************************************************************************************")
        print("""
            \t\t\tpress 1: Send Email
            \t\t\tpress 2: Send SMS
            \t\t\tpress 3: Send WhatsApp
            \t\t\tpress 4: Find Location
            \t\t\tpress 5: Ask ChatGPT
            \t\t\tpress 6: Open Browser
            \t\t\tpress 0: Exit
        """)

        service = input("Enter your choice: ")

        if service == '1':
            send_email()
        elif service == '2':
            to_phone_number = input("Enter the recipient mobile number with country code: ")
            message_body = input("Enter your message:")
            send_sms(to_phone_number, message_body)
        elif service == '3':
            to_phone_number = input("Enter the recipient WhatsApp number (without '+' or '00'): ")
            message_body = input("Enter your WhatsApp message: ")
            hour = input("Enter the hour (24-hour format): ")
            minute = input("Enter the minute: ")
            send_whatsapp(to_phone_number, message_body, hour, minute)
        elif service == '4':
            location_name = input("Enter the location name: ")
            find_location(location_name)
        elif service == '5':
            user_input = input("Enter your prompt for ChatGPT:")
            chat_with_gpt(user_input)
        elif service == '6':
            search_query = input("Enter your search query: ")
            open_chrome_search(search_query)
        elif service == '0':
            print("Exiting the program. Goodbye!")
            break
        else:
            print("Invalid choice. Please select a valid option.")

   
