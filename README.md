<!-- # Voice-Assistant-Xavier- -->
import audioop
from os import set_inheritable
import pyttsx3  #to convert text to speech
import datetime #to get date and time
import speech_recognition as sr #to recognize voice
import wikipedia #to search on wikipedia
import webbrowser #to open websites directly
import os


engine = pyttsx3.init('sapi5') 
voices = engine.getProperty('voices')
engine.setProperty('rate',180)
# print(voices)
# print(voices[1].id)
engine.setProperty('voice', voices[1].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)

    if hour >= 0 and hour < 12:
        speak("HOLA AMIGOS !")

    elif hour >= 12 and hour < 18:
        speak("HOLA AMIGOS !")

    else:
        speak("Good Evening!")

    speak("Hi!I am Xavier , what can i do for you ? ") 


def takeCommand():
    # It takes mic input from user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1

        r.energy_threshold=10000
        r.adjust_for_ambient_noise(source,1.2)
        audio = r.listen(source)

    try:
        query = r.recognize_google(audio, language='en-in')
        print(f"user said: {query}\n")

    except Exception as e:
        # print(e) commented this out to hide errors on the console or terminal.
        print("Say that again please...")
        return "None"

    return query


if __name__ == "__main__":
    wishMe()

    while True:
        query = takeCommand().lower()


        if 'information' in query:
            speak('Yes mam... ,  Information about which topic...? ')
            query = takeCommand().lower()
            results = wikipedia.summary(query ,sentences =1)  
            speak("According to wikipedia") 
            try:                    
                print(results)
                speak(results)
            except Exception as e:
                speak(results)

        if 'wikipedia' in query:
            results = wikipedia.summary(query ,sentences =2)  
            speak("According to wikipedia..") 
            try:                    
                print(results)
                speak(results)
            except Exception as e:
                speak(results)

        elif 'open youtube' in query:
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            webbrowser.open("google.com")
        
        elif 'open stack overflow' in query:
            webbrowser.open("stackoverflow.com")

        elif 'the time' in query:
            strTime=datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Mam , The time is {strTime}")

        
        elif 'how' and 'are' and 'you' in query:
            speak("I am absolutely fine mam ! , what can i do for you ??")

        elif 'quit' in query:
            speak('BYE BYE ...')
            break
        elif 'stop' in query:
            speak('Sayonara... have a great day ahead..')
            break

        else:
            speak("Say it again please...")
