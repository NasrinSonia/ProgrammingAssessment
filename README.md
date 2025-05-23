# ProgrammingAssessment Description
WeatherWise is a Python-based system that analyzes weather data using API and NLP, with visualizations and basic chatbot features.
# Code Explination 
The **WeatherWise: Intelligent Weather Analysis & Advisory System** is a Python-based application designed to fetch, analyze, and visualize weather data using real-time API integration and natural language processing (NLP). It installs and imports libraries such as `requests`, `matplotlib`, `spacy`, and `pyinputplus`, leveraging them to interact with a free weather API (`wttr.in`), handle user input, and parse text-based queries. The spaCy English model enhances the chatbot-like interaction, allowing users to input weather-related questions in natural language. The core functionality revolves around the `get_weather_data` function, which retrieves weather data based on city names and structures it for both current conditions and multi-day forecasts, including temperature, wind speed, humidity, and sunrise/sunset times. Visualizations are generated to represent weather trends and make the data more interpretable for users.

However, the system has some limitations. The free API used does not support coordinate-based queries, meaning it cannot retrieve weather data using latitude and longitude, which restricts the ability to provide precise, location-specific forecasts. Additionally, while the NLP functionality offers an intelligent interface, it lacks the ability to extract detailed query context and geographic data in real time. This shortcoming reduces the effectiveness of the chatbot in interpreting vague or complex user queries. Moreover, the API often fails to deliver data for narrowly defined or small localities, which can limit the system's usefulness for users in rural or lesser-known areas. Despite these constraints, WeatherWise remains a functional prototype that showcases the integration of data science and NLP for intelligent weather advisory services.


### **Simulated Conversation Log Between Me and ChatGPT**


**Me:**
I want to build a weather application using Python that can fetch forecasts and answer natural language weather questions. Can you guide me?

**ChatGPT:**
Absolutely! We can build this using a modular design. I'd recommend breaking it into components:

* `WeatherDataHandler` for fetching and processing weather data
* `NLProcessor` for parsing natural language questions
* `Visualizer` for plotting weather trends
* `UserInterface` for input/output interactions

Let’s start by setting up the weather data component using the `wttr.in` API.

---

**Me:**
Sounds good. Can you give me the code for fetching weather data?

**ChatGPT:**
Sure! Here's the code to get weather data for a given location using the `requests` library:

```python
import requests

def get_weather_data(location):
    url = f"https://wttr.in/{location}?format=j1"
    response = requests.get(url)
    data = response.json()
    return data
```

This function returns weather data in JSON format for the specified location.

---

**Me:**
Now I want to ask questions like “What’s the weather in London tomorrow?” How do I parse that?

**ChatGPT:**
For this, we’ll use the `spaCy` NLP library. First, install it and load the English model:

```python
!pip install spacy
!python -m spacy download en_core_web_sm

import spacy
nlp = spacy.load("en_core_web_sm")
```

Then we’ll build a function to extract location, timeframe, and question type from the input.

---

**Me:**
Okay, but my question parsing isn’t working. It's not correctly identifying the location or forecast date.

**ChatGPT:**
Let’s refine that. You need to address:

1. **Location Case Sensitivity** – Match API data with case-corrected location.
2. **Timeframe Parsing** – Use keywords like "today", "tomorrow", or explicit days.
3. **Attribute Matching** – Detect if the user wants temperature, humidity, general forecast, etc.

Here’s an updated parsing function:

```python
def parse_weather_question(question):
    doc = nlp(question.lower())
    location = None
    day_index = 0  # default: today
    if "tomorrow" in question:
        day_index = 1
    for ent in doc.ents:
        if ent.label_ == "GPE":
            location = ent.text.title()
    return {"location": location, "day_index": day_index}
```

---

**Me:**
Cool, it parses the question now. But it doesn’t always match the attribute like temperature or wind.

**ChatGPT:**
Good point. Let’s add a simple keyword check:

```python
def get_weather_attribute(question):
    if "temperature" in question:
        return "temperature"
    elif "wind" in question:
        return "wind"
    elif "humidity" in question:
        return "humidity"
    return "general"
```

Combine this with the parser to generate responses more accurately.

---

**Me:**
Now I want to structure this into a clean Colab project. How do I organize it?

**ChatGPT:**
Structure your code into logical Colab cells:

1. **Installations**
2. **Imports & Model Loading**
3. **Weather API Function**
4. **NLP Parsing**
5. **Question to Forecast Mapping**
6. **Display Output**
7. (Optional) **Error Handling & UI**

---

**Me:**
Finally, what were the biggest limitations I faced?

**ChatGPT:**
Three main limitations:

* The `wttr.in` API doesn't support GPS coordinates—only city names.
* NLP can't always extract fine-grained location or date information accurately in real time.
* Small or uncommon location names may not return data from the API.


