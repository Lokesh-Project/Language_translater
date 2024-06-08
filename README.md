# Language_translater
Here's a README file for your GitHub project that details the setup and usage of IBM Watson APIs for speech translation:

---

# Speech Translation using IBM Watson APIs

This project demonstrates how to use IBM Watson's Speech to Text, Language Translator, and Text to Speech services to create a simple speech translation pipeline. The pipeline converts spoken English into Spanish speech.

## Prerequisites

- Python 3.x
- IBM Cloud account
- IBM Watson API keys and URLs for Speech to Text, Language Translator, and Text to Speech services

## Setup

### Step 1: Create an IBM Cloud Account

1. Go to the [IBM Cloud](https://cloud.ibm.com/registration) and sign up for a free account.
2. After logging in, go to the [IBM Watson Services Catalog](https://cloud.ibm.com/catalog) and create instances for the following services:
   - Speech to Text
   - Language Translator
   - Text to Speech

### Step 2: Get API Keys and URLs

1. Navigate to each service instance you created and click on "Service credentials".
2. Click on "New credential" to create new credentials if not already created.
3. Copy the `API Key` and `URL` from the credentials.

### Step 3: Install Required Python Packages

Run the following command to install the necessary packages:
```bash
pip install ibm_watson
```

## Usage

Replace the placeholder API keys and URLs in the script with your actual credentials from IBM Cloud.

```python
# Import necessary libraries
from ibm_watson import SpeechToTextV1, LanguageTranslatorV3, TextToSpeechV1
from ibm_watson.websocket import RecognizeCallback, AudioSource
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# IBM Watson Speech to Text credentials
sttapikey = 'your_speech_to_text_api_key'
stturl = 'your_speech_to_text_url'

sttauthenticator = IAMAuthenticator(sttapikey)
stt = SpeechToTextV1(authenticator=sttauthenticator)
stt.set_service_url(stturl)

# Convert Speech to Text
with open('original_audio.mp3', 'rb') as f:
    ress = stt.recognize(audio=f, content_type='audio/mp3', model='en-US_BroadbandModel').get_result()

text = ress['results'][0]['alternatives'][0]['transcript']
print(f"Transcribed Text: {text}")

# IBM Watson Language Translator credentials
apikey = 'your_language_translator_api_key'
url = 'your_language_translator_url'

# Setup service
authenticator = IAMAuthenticator(apikey)
lt = LanguageTranslatorV3(version='2024-06-08', authenticator=authenticator)
lt.set_service_url(url)

# Translate text
translation = lt.translate(text, model_id='en-es').get_result()
translated_text = translation['translations'][0]['translation']
print(f"Translated Text: {translated_text}")

# IBM Watson Text to Speech credentials
ttsapikey = 'your_text_to_speech_api_key'
ttsurl = 'your_text_to_speech_url'

# Authenticate
ttsauthenticator = IAMAuthenticator(ttsapikey)
tts = TextToSpeechV1(authenticator=ttsauthenticator)
tts.set_service_url(ttsurl)

# Convert translated text to speech
with open('./translated_audio.mp3', 'wb') as audio_file:
    res = tts.synthesize(translated_text, accept='audio/mp3', voice='es-LA_DanielaExpressive').get_result()
    audio_file.write(res.content)
print("Audio content written to 'translated_audio.mp3'")
```

### Running the Script

1. Save the script to a file, e.g., `speech_translation.py`.
2. Place an audio file named `original_audio.mp3` in the same directory as the script.
3. Run the script:
```bash
python speech_translation.py
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

By following the steps above, you should be able to set up and run your speech translation project using IBM Watson APIs. Make sure to replace the placeholder API keys and URLs with your actual service credentials from IBM Cloud.
