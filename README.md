Voice Assistant :information_desk_person::sound:
===
*Project created as a part of the subject 'Programming in Python' in AGH University of Science and Technology.*

## Team :punch:

+ [Jakub Myśliwiec](https://github.com/jmysliv):airplane::bomb:
+ [Sebastian Kuśnierz](https://github.com/skusnierz) :fire:

## About Project :question:

Our idea was to create voice assistant similar to Google assistant but for desktop devices and in polish language. We decided that assistant will have the following features:

+ [telling jokes](#joke) :joy:
+ [finding curiosities](#curiosities) :mortar_board:
+ [playing YouTube videos](#yt) :tv:
+ [searching phrase in both Wikipedia and Google](#wiki) :mag_right:
+ [checking current COVID-19 data](#covid) :skull:
+ [planning events](#events) :date:
+ [sending and receiving messages with other users](#message) :incoming_envelope:
+ [planning tasks](#tasks) :calendar:
+ [changing volume](#volume) :mute:
+ [checking current weather](#weather) :cloud:
+ [changing brightness of the screen](#brightness) :high_brightness:

During implementation we used many libraries and frameworks, for instance:

+ Django REST Framework to implement dedicated server
+ Selenium for automating web browsers
+ Google speech recognition
+ Tkinter for creating graphical user interface

While working on the project we learned how to properly configure python virtual environment, so we could use it easily with operating systems both Ubuntu and Windows. New experience for us was to create REST API server using Django framework. We've had experience with REST API architecture before, but in different programming languages, so it was our first time with Django. We have also learned a lot about group cooperation, and sharing responsibilities.

## Get Started :rocket:

First thing you have to do is to clone this repository, type the following commands in your terminal:
```bash
    git clone git@github.com:jmysliv/VoiceAssistant.git
    cd VoiceAssistant
```


### Install requirements :mega:
All necessary libraries and packages are in file [requirements_windows.txt](VoiceAssistant/requirements_windows.txt) for windows operating system and in  [requirements_linux.txt](VoiceAssistant/requirements_linux.txt) for Linux.  
Before running command below make sure that you have installed Python 3.7+. :snake: 

#### Windows :poop:
```bash
    cd VoiceAssistant
    python -m venv venv
    source venv/Scripts/activate
    pip install -r requirements_windows.txt
    pipwin install pyaudio
    cd UI
    ../venv/Scripts/python main.py
```
Sometimes when you try to install pyaudio using pipwin, some errors may occur. If that happens you have to install pyaudio using .whl file which you can download it from:
https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyaudio

Find the appropriate .whl file that matches your Python version (you can check your python version using ```python --version```) and put it in the directory where VoiceAssistant is located. Then you can install pyaudio using pip and run our assistant:
```bash
    pip install <name of your .whl file>.whl
    cd UI
    ../venv/Scripts/python main.py
```

#### Linux :ok_hand:
```bash
    cd VoiceAssistant
    python3 -m venv venv
    source venv/bin/activate
    pip install wheel 
    sudo apt-get install libasound-dev portaudio19-dev libportaudio2 libportaudiocpp0
    sudo apt-get install ffmpeg libav-tools
    sudo apt-get update & sudo apt-get install espeak & sudo apt-get install python3-tk
    pip install -r requirements_linux.txt
    cd UI
    ../venv/bin/python3 main.py
```

#### Install Chrome webdriver 
This webdriver must be installed for the correct work of the voice assistant.  
Choose the webdriver specific for your Chrome browser version from this [link](https://chromedriver.chromium.org/downloads).    
If you have some problem with find your Chrome Browser version number this [link](https://help.zenplanner.com/hc/en-us/articles/204253654-How-to-Find-Your-Internet-Browser-Version-Number-Google-Chrome) will help you.    
Next extract downloaded webdriver to [driver folder](VoiceAssistant/driver) and make sure that filename is "webdriver"


## Adding new features
*If you enjoy idea of our project and you want to develop it, here is simple guide how to add new feature to our assistant.*

You have to create new Service class object, init function takes two arguments:

1. ```wake_words``` is the array of phrases that you want the assistant to recognize, that will trigger execution of what you want your service to do. Note that all phrases should be lowercase. For example if you writing service for food ordering it could be something like that:
```python
    wake_words = ["zamów jedzenie", "chce coś zjeść"]
```

2. ```wake_function``` is the function that will execute after assistant recognize one of your ```wake_words```. It takes three arguments:

    * ```frame``` that is responsible for what you see on the screen and what the assistant says. You can use the following functions:

        * ```frame.assistant_speaks(message)``` as message you pass what you want the assistant to say.
        * ```frame.user_speaks(message)``` as message you pass what you want to be displayed as what you said.
        * ```frame.assistant_listening()``` if you want the assistant to mark that he listening. He will say "Słucham...". 
        * ```frame.assistant_doesnt_understand()``` if you want the assistant to mark that he doesn't understand what user said. He will say "Nie rozumiem, możesz powtórzyć?". 

    * ```text``` it stores the phrase that user said.

    * ```token``` it stores token for server access.

    For example if you writing service for food ordering your wake function could look like that:
    ```python
        def wake_function(frame, text, token):
            frame.assistant_says("Co chcesz zjeść?")
            food = get_audio(5)
            while food == "":
                frame.assistant_doesnt_understand()
                food = get_audio(5)
            frame.user_speaks(food)
            order_food(food) #function that orders food
            frame.assistant_says("Zamówione")
    ```
Once you have ```wake_words``` and ```wake_function``` defined, you can create Service object and append it to services list in ```create_services``` function inside [service.py](VoiceAssistant/service.py) file:
```python
    services.append(Service(wake_words, wake_function))
```

*Note that if you need to recognize some additional speech in your service you need to import ```get_audio```, that takes one argument which is timeout in seconds*
```python
    from speech.speech_recognizer import get_audio
```

If you finished adding your service don't hesitate to make a pull request. Also if you notice something we missed, or discover some bug, feel free to open new issue, or simply fix it and make pull request.

## User Guide :heavy_exclamation_mark:

*Here you can find information how to proper interact with our assistant, and what command is responsible for what feature.*  
*At the beginning you have to wake your assistant, you can do it by saying **"Janusz"**,* 
*this word mobilizes the assistant to act, and the assistant is waiting for your command to execute from now on.*
### Telling jokes <a id="joke"></a> :joy:
```
    wake_words = ["suchar", "suchar", "żart", "dowcip"]
```
### Finding curiosities <a id="curiosities"></a> :mortar_board:
```
    wake_words = ["ciekawostki", "ciekawego", "ciekawostki", "ciekawostka", "ciekawostkę"]
```
### Playing YouTube videos <a id="yt"></a>  :tv:
```
    wake_words = ["uruchom", "włącz", "puść"]
```
### Checking current COVID-19 data <a id="covid"></a> :skull:
```
    wake_words = ["koronawirus", "koronawirusie", "korona wirusie", "korona wirus", "koronawirusa"]
```
### Planning events  <a id="events"></a> :date:
```
    wake_words = ["dodaj wydarzenie", "dodaj nowe wydarzenie", "zaplanuj wydarzenie", "pokaż wydarzenia", "wyświetl wydarzenia",  
     "jakie mam wydarzenia", "co mam zaplanowane", "co mam w planach"]
```
### Sending and receiving messages with other users  <a id="message"></a> :incoming_envelope:
```
    wake_words = ["wyślij wiadomość", "napisz wiadomość", "pokaż wysłane wiadomości", "pokaż wiadomości które wysłałem", 
    "pokaż przeczytane wiadomości", "pokaż stare wiadomości" "pokaż skrzynkę odbiorczą", "mam jakieś nowę wiadomości",
    "pokaż nowe wiadomości", "oznacz wiadomość jako przeczytaną", "przeczytałem wiadomość"]
```
### Planning tasks  <a id="tasks"></a> :calendar:
```
    wake_words = ["pokaż zadania do wykonania", "pokaż niezrobione zadania", "pokaż co mam do zrobienia", "co mam zrobić", 
    "co jest do zrobienia", "pokaż co zrobiłem", "pokaż zrobione zadania", "co zrobiłem", "co już zrobiłem",  
    "dodaj zadanie do zrobionych", "oznacz zadanie jako zrobione", "przenieś zadanie do zrobionych", 
    "zrobiłem zadanie", "wykonałem zadanie"]
```
### Changing volume  <a id="volume"></a> :mute:
```
    wake_words = ["głośność", "przycisz", "podgłośni", "dźwięk"]
```
### Checking current weather  <a id="weather"></a> :cloud:
```
    wake_words = ["pogoda"]
```
### Changing brightness of the screen <a id="brightness"></a> :high_brightness:
```
    wake_words = ["jasność", "kontrast"]
```
### Searching phrase in both Wikipedia and Google <a id="wiki"></a> :mag_right:
```
    wake_words = []
```
*If the given command doesn't match any of the above wake words, the given phrase will be searched in Wikipedia and the search results will be read by assistant,
if it still gives unsuccessful result the command will be searched in Google and results will be displayed in the browser.*