# UN-OFFICIAL OPENAI API Service Documentation

## Introduction

Welcome to the **UN-OFFICIAL OPENAI API Service** documentation. This service provides an OpenAI-compatible API interface for interacting with various GPT models, including text completion, audio generation, and image generation capabilities. The API is designed to be compatible with OpenAI's API endpoints, ensuring seamless integration with existing OpenAI API clients and applications. 

**Important Note:** While the endpoints and payload structures are compatible with OpenAI's API, the service is **currently not optimized for use with the OpenAI Python SDK**. Users should interact with the API using standard HTTP requests.

This API service was originally reverse-engineered by **Mr Leader** and is further developed and maintained by **DevsDoCode (Sreejan)**. This collaboration combines reverse engineering expertise with robust development practices to deliver a reliable and efficient API for your AI model interaction needs.

---

## Base URL

### Primary API Endpoint

All API requests should be made to the following base URL:

```
https://devsdocode-openai.hf.space
```

This endpoint is hosted on Hugging Face Spaces and offers continuous free-tier availability, making it the recommended endpoint for long-term usage due to its stability and scalability.

### Secondary API Endpoint (Optional)

As an alternative, you may also use the following endpoint:

```
https://openai-devsdocode.up.railway.app
```

Please note that this endpoint is hosted on Railway's free tier, which may have limitations and service interruptions after the free tier expires. We recommend using the Hugging Face endpoint for uninterrupted long-term service.

---

## Authentication

**Note:** Currently, the API does not require any authentication tokens or API keys. Users are expected to use the service responsibly and within reasonable limits.

---

## Available Endpoints

1. **List Available Models**
   - **Endpoint:** `GET /models`
   - **Description:** Retrieve a list of all available models.
   - **Response Format:** JSON compatible with OpenAI's model list format.

2. **API Information**
   - **Endpoint:** `GET /about`
   - **Description:** Get detailed information about the API service, including version, description, developer info, and more.
   - **Response Format:** JSON

3. **Chat Completions**
   - **Endpoint:** `POST /chat/completions`
   - **Description:** Generate chat-based text completions using GPT models.
   - **Supports Streaming:** Yes
   - **Compatible Models:** GPT-3.5 series, GPT-4 series, and others.
   - **Response Format:** JSON or Streaming Response (as per OpenAI API)

4. **Audio Speech Generation**
   - **Endpoint:** `POST /audio/speech`
   - **Description:** Generate speech audio from text input using Text-to-Speech models.
   - **Response Format:** Streaming audio (`audio/mpeg`), compatible with OpenAI's audio response format.

5. **Image Generation**
   - **Endpoint:** `POST /images/generations`
   - **Description:** Generate images based on text prompts.
   - **Compatible Models:** `dall-e-2`, `dall-e-3`
   - **Response Format:** JSON compatible with OpenAI's image generation response.

---

## Endpoint Details

### 1. Get List of Available Models

**Endpoint:**

```
GET /models
```

**Description:**

Retrieves a list of all available models that can be used with this API. The response structure is compatible with OpenAI's model listing.

**Response Example:**

```json
{
  "object": "list",
  "data": [
    {
      "id": "gpt-4-turbo-2024-04-09",
      "object": "model",
      "created": 1712601677,
      "owned_by": "DevsDoCode"
    },
    {
      "id": "tts-1-1106",
      "object": "model",
      "created": 1699053241,
      "owned_by": "DevsDoCode"
    },
    // ... other models
  ]
}
```

---

### 2. Get API Information

**Endpoint:**

```
GET /about
```

**Description:**

Provides detailed information about the API service, including version, description, developer info, and social links.

**Response Example:**

```json
{
  "name": "Unofficial OpenAI API",
  "version": "1.0.1",
  "description": "An unofficial OpenAI API service compatible with OpenAI API endpoints",
  "developer": "Sreejan (DevsDoCode)",
  "owner": "Sreejan",
  "last_updated": "2024-11-06",
  "originator": "Mr Leader",
  "originator_info": {
    "name": "Mr Leader",
    "expertise": ["Python Development", "Reverse Engineering"],
    "youtube": "https://www.youtube.com/@mr_leaderyt",
    "contribution": "Original founder of this API & has reverse-engineered it"
  },
  "social_links": {
    "telegram": "https://t.me/DevsDoCode",
    "youtube": "https://www.youtube.com/@DevsDoCode",
    "instagram": "https://www.instagram.com/sree.shades_/",
    "github": "https://github.com/SreejanPesonal"
  },
  "credits": "Special thanks to Mr Leader for the foundational work and expertise in finding this API",
  "report": "To report any bugs, please contact the owner of this API on either Telegram or Instagram",
  "validity": "This API is currently open and free to use until further announcement. The service is hosted on both Railway (1-month free tier) and Hugging Face (continuous free tier). Please note that the Railway API endpoint may be discontinued after the free tier expires in one month. However, the Hugging Face endpoint will remain continuously available as it is hosted on their free tier Spaces using Docker. For uninterrupted service, we recommend using the Hugging Face endpoint for long-term usage.",
  "maintained_by": "DevsDoCode"
}
```

---

### 3. Chat Completions

**Endpoint:**

```
POST /chat/completions
```

**Description:**

Generates chat-based text completions using GPT models. Supports both streaming and non-streaming responses. Fully compatible with OpenAI's `/v1/chat/completions` endpoint.

**Important Note:** While the API endpoints and response formats are compatible with OpenAI's API, it is currently **not optimized for use with the OpenAI Python SDK**. Users should interact with the API using standard HTTP requests.

**Request Parameters:**

- **model** (string): ID of the model to use (e.g., `gpt-4o-mini-2024-07-18`).
- **messages** (array): A list of message objects in the conversation.
- **temperature** (float, optional): Sampling temperature between 0 and 2.
- **top_p** (float, optional): Nucleus sampling probability.
- **stream** (boolean, optional): If true, the response will be streamed.
- **presence_penalty** (float, optional): Penalty for new topics.
- **frequency_penalty** (float, optional): Penalty for repetition.

#### Non-Streaming Request Example

**Request:**

```json
{
  "model": "gpt-4o-mini-2024-07-18",
  "messages": [
    {"role": "user", "content": "How many 'r's are there in Strawberry?"}
  ],
  "temperature": 0.5,
  "top_p": 1,
  "stream": false
}
```

**Response Example:**

```json
{
  "id": "chatcmpl-xxxxxxxxxxxxxx",
  "object": "chat.completion",
  "created": 1664139435,
  "model": "gpt-4o-mini-2024-07-18",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "There are three 'r's in 'Strawberry'."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 15,
    "completion_tokens": 9,
    "total_tokens": 24
  }
}
```

#### Streaming Request Example

**Request:**

```json
{
  "model": "gpt-4o-mini-2024-07-18",
  "messages": [
    {"role": "user", "content": "Write 10 lines on India"}
  ],
  "temperature": 0.5,
  "top_p": 1,
  "stream": true
}
```

**Response Example:**

Streaming responses are sent as a series of data chunks. Each chunk contains a JSON structure.

Example chunk:

```
data: {"choices": [{"delta": {"content": "India, "}, "index": 0, "finish_reason": null}]}
data: {"choices": [{"delta": {"content": "a "}, "index": 0, "finish_reason": null}]}
data: {"choices": [{"delta": {"content": "land "}, "index": 0, "finish_reason": null}]}
data: {"choices": [{"delta": {"content": "of "}, "index": 0, "finish_reason": null}]}
data: {"choices": [{"delta": {"content": "diversity,"}, "index": 0, "finish_reason": null}]}
...
```

---

### 4. Audio Speech Generation

**Endpoint:**

```
POST /audio/speech
```

**Description:**

Generates speech audio from text input using Text-to-Speech (TTS) models. The response is streamed as audio data, compatible with OpenAI's audio response format.

**Request Parameters:**

- **model** (string): ID of the TTS model to use (e.g., `tts-1-hd-1106`).
- **input** (string): The text input to be converted to speech.
- **voice** (string, optional): The voice style to use. Available options:
  - `nova`
  - `echo`
  - `fable`
  - `onyx`
  - `shimmer`
  - `alloy`

**Request Example:**

```json
{
  "model": "tts-1-hd-1106",
  "voice": "nova",
  "input": "I love you & I can't wait to spend time together!"
}
```

**Response:**

The response is a streaming audio file in `audio/mpeg` format.

**Usage Example:**

```python
import requests

def download_audio_speech():
    url = "https://devsdocode-openai.hf.space/audio/speech"
    payload = {
        "model": "tts-1-hd-1106",
        "voice": "nova",
        "input": "I love you & I can't wait to spend time together!"
    }
    response = requests.post(url, json=payload, stream=True)
    with open("speech.mp3", "wb") as f:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:
                f.write(chunk)
    print("Audio saved as speech.mp3")
```

---

### 5. Image Generation

**Endpoint:**

```
POST /images/generations
```

**Description:**

Generates images based on text prompts using DALL·E models. Fully compatible with OpenAI's `/v1/images/generations` endpoint.

**Request Parameters:**

- **model** (string): ID of the image generation model to use (e.g., `dall-e-3`).
- **prompt** (string): The text prompt describing the desired image.
- **n** (integer, optional): Number of images to generate (default is 1).
- **size** (string, optional): Size of the generated images (`256x256`, `512x512`, `1024x1024`).
- **response_format** (string, optional): Format of the response (`url` or `b64_json`).
- **quality** (string, optional): Quality of the image (`hd`).

**Request Example:**

```json
{
  "model": "dall-e-3",
  "prompt": "A futuristic cityscape with flying cars and tall skyscrapers.",
  "n": 1,
  "size": "1024x1024",
  "response_format": "url",
  "quality": "hd"
}
```

**Response Example:**

```json
{
  "created": 1664139435,
  "data": [
    {
      "url": "https://devsdocode-openai.hf.space/generated_images/image1.png"
    }
  ]
}
```

---

### 6. GPT-4O Audio Preview Models

We are excited to introduce the **gpt-4o-audio-preview** family of models. These models allow for an enhanced interaction experience by providing audio responses along with text. **This feature is rare and not available for free elsewhere**, and we are pleased to offer it to our users.

**Available Models:**

- `gpt-4o-audio-preview-2024-10-01`
- `gpt-4o-audio-preview`

**Description:**

The `gpt-4o-audio-preview` models generate audio responses in addition to text completions. When you use these models, the API returns a streaming response that includes both text and audio data.

**Note:** This functionality is currently in preview, and we welcome any feedback or contributions to improve it.

**Usage Example:**

```python
import requests
import base64

def get_audio_text_completion():
    url = "https://devsdocode-openai.hf.space/chat/completions"
    payload = {
        "messages": [{"role": "user", "content": "You know i follow Devs Do Code on YT for Coding. He is A legend"}],
        "model": "gpt-4o-audio-preview-2024-10-01",
        "modalities": ["text", "audio"],
        "audio": {"voice": "fable", "format": "wav"},
        "temperature": 0.9,
        "presence_penalty": 0,
        "frequency_penalty": 0,
        "top_p": 1
    }
    
    response = requests.post(url, json=payload)
    print("Status Code:", response.status_code)
    
    if response.ok:
        data = response.json()
        try:
            if "choices" in data and data["choices"]:
                message = data["choices"][0].get("message", {})
                
                if "content" in message:
                    print("Text response:", message["content"])
                
                if "audio" in message and "data" in message["audio"]:
                    wav_bytes = base64.b64decode(message["audio"]["data"])
                    print("Text Response:", message["audio"]["transcript"])
                    
                    output_file = "chat.wav"
                    with open(output_file, "wb") as f:
                        f.write(wav_bytes)
                    print(f"\nAudio saved to {output_file}")
                else:
                    print("No audio data found in the response.")
        except Exception as e:
            print(f"An error occurred: {e}")
    else:
        print("Request failed with status code:", response.status_code)
        print("Response content:", response.text)
```

**Note:** The exact handling of the audio and text data may vary depending on the API format. We recommend experimenting and adjusting the code as needed.

---

### 7. GPT-4O Real-Time Models

We are also introducing the **gpt-4o-realtime** family of models, which support real-time interaction capabilities similar to OpenAI's official real-time API.

**Available Models:**

- `gpt-4o-realtime-preview-2024-10-01`
- `gpt-4o-realtime-preview`

**Description:**

The `gpt-4o-realtime` models are designed for applications requiring immediate response generation, enabling a more interactive and dynamic experience.

**Usage Notes:**

- Currently, the code examples and documentation for these models are in development and will be updated soon.
- If you are familiar with using OpenAI's official real-time API, you can attempt to use these models similarly.
- **We encourage users who successfully utilize these models to contribute code examples and improvements to our GitHub repository.**

**Contribution Invitation:**

If you discover a successful method for utilizing the `gpt-4o-realtime` models, please consider contributing to our GitHub repository. Your contributions will help enhance the documentation and assist other users.

---

## Available Models

### GPT Models

- **GPT-4 Series:**
  - `gpt-4o-realtime-preview-2024-10-01`
  - `gpt-4o-audio-preview-2024-10-01`
  - `gpt-4o-mini-2024-07-18`
  - `gpt-4o-mini`
  - `gpt-4o`
  - `gpt-4-turbo-2024-04-09`
  - `gpt-4-turbo`

- **GPT-3.5 Series:**
  - `gpt-3.5-turbo-1106`
  - `gpt-3.5-turbo-0613`
  - `gpt-3.5-turbo`
  - `gpt-3.5-turbo-0125`
  - `gpt-3.5-turbo-instruct-0914`
  - `gpt-3.5-turbo-instruct`

### Text-to-Speech Models

- `tts-1-hd-1106`
- `tts-1-hd`
- `tts-1-1106`
- `tts-1`

### Image Generation Models

- `dall-e-3`
- `dall-e-2`

### Audio-Capable Models

- `gpt-4o-audio-preview-2024-10-01`
- `gpt-4o-audio-preview`

### Real-Time Models

- `gpt-4o-realtime-preview-2024-10-01`
- `gpt-4o-realtime-preview`

### Embedding Models

- `text-embedding-3-large`
- `text-embedding-3-small`
- `text-embedding-ada-002`

---

## Parameters

### Common Parameters

| Parameter             | Type    | Description                                                                                  |
|-----------------------|---------|----------------------------------------------------------------------------------------------|
| **model**             | string  | ID of the model to use.                                                                      |
| **messages**          | array   | Array of message objects, each with `role` and `content`.                                    |
| **temperature**       | float   | Sampling temperature between 0 and 2. Higher values make the output more random.             |
| **top_p**             | float   | Nucleus sampling probability. An alternative to using temperature.                           |
| **stream**            | boolean | If true, responses are streamed back as they are generated.                                  |
| **presence_penalty**  | float   | Penalizes new tokens based on whether they appear in the text so far. Range: -2.0 to 2.0.    |
| **frequency_penalty** | float   | Penalizes new tokens based on their frequency in the text so far. Range: -2.0 to 2.0.        |

### Audio Speech Parameters

| Parameter   | Type   | Description                                             |
|-------------|--------|---------------------------------------------------------|
| **input**   | string | The text input to be converted to speech.               |
| **voice**   | string | The voice style to use for speech synthesis.            |
| **format**  | string | The audio file format (`mp3`, `wav`). Default is `mp3`. |

### Image Generation Parameters

| Parameter          | Type    | Description                                                 |
|--------------------|---------|-------------------------------------------------------------|
| **prompt**         | string  | The text prompt to generate the image from.                 |
| **n**              | integer | Number of images to generate. Default is 1.                 |
| **size**           | string  | Image dimensions (`256x256`, `512x512`, `1024x1024`).       |
| **response_format**| string  | Format of the response (`url`, `b64_json`).                 |
| **quality**        | string  | Quality of the generated images (`hd`).                     |

---

## Usage Examples

### 1. Chat Completion (Non-Streaming)

```python
import requests

def get_chat_completion():
    url = "https://devsdocode-openai.hf.space/chat/completions"
    payload = {
        "model": "gpt-4o-mini-2024-07-18",
        "messages": [
            {"role": "user", "content": "Tell me a joke about programmers."}
        ],
        "temperature": 0.7,
        "top_p": 1,
        "stream": False
    }
    response = requests.post(url, json=payload)
    if response.ok:
        result = response.json()
        print(result["choices"][0]["message"]["content"])
    else:
        print("Request failed:", response.text)
```

### 2. Chat Completion (Streaming)

```python
import requests

def get_chat_completion_streaming():
    url = "https://devsdocode-openai.hf.space/chat/completions"
    payload = {
        "model": "gpt-4o-mini-2024-07-18",
        "messages": [
            {"role": "user", "content": "Write a poem about the sea."}
        ],
        "temperature": 0.7,
        "top_p": 1,
        "stream": True
    }
    with requests.post(url, json=payload, stream=True) as response:
        for chunk in response.iter_lines():
            if chunk:
                data = chunk.decode('utf-8').strip()
                if data == "[DONE]":
                    break
                else:
                    print(data)
```

### 3. Audio Speech Generation

```python
import requests

def generate_audio_speech():
    url = "https://devsdocode-openai.hf.space/audio/speech"
    payload = {
        "model": "tts-1-hd-1106",
        "voice": "echo",
        "input": "Hello, this is a test of the speech synthesis."
    }
    response = requests.post(url, json=payload, stream=True)
    with open("output.mp3", "wb") as f:
        for chunk in response.iter_content(chunk_size=8192):
            if chunk:
                f.write(chunk)
    print("Audio saved as output.mp3")
```

### 4. Image Generation

```python
import requests
import json

def generate_image():
    url = "https://devsdocode-openai.hf.space/images/generations"
    payload = {
        "model": "dall-e-3",
        "prompt": "A serene landscape of mountains during sunset.",
        "n": 1,
        "size": "1024x1024",
        "response_format": "url",
        "quality": "hd"
    }
    response = requests.post(url, json=payload)
    if response.ok:
        image_url = json.loads(response.json())["data"][0]["url"]
        print("Image URL:", image_url)
              
    else:
        print("Request failed:", response.text)
```

### 5. GPT-4O Audio Preview Models

```python
import requests
import base64

def get_audio_text_completion():
    url = "https://devsdocode-openai.hf.space/chat/completions"
    payload = {
        "messages": [{"role": "user", "content": "You know i follow Devs Do Code on YT for Coding. He is A legend"}],
        "model": "gpt-4o-audio-preview-2024-10-01",
        "modalities": ["text", "audio"],
        "audio": {"voice": "fable", "format": "wav"},
        "temperature": 0.9,
        "presence_penalty": 0,
        "frequency_penalty": 0,
        "top_p": 1
    }
    
    response = requests.post(url, json=payload)
    print("Status Code:", response.status_code)
    
    if response.ok:
        data = response.json()
        try:
            if "choices" in data and data["choices"]:
                message = data["choices"][0].get("message", {})
                
                if "content" in message:
                    print("Text response:", message["content"])
                
                if "audio" in message and "data" in message["audio"]:
                    wav_bytes = base64.b64decode(message["audio"]["data"])
                    print("Text Response:", message["audio"]["transcript"])
                    
                    output_file = "chat.wav"
                    with open(output_file, "wb") as f:
                        f.write(wav_bytes)
                    print(f"\nAudio saved to {output_file}")
                else:
                    print("No audio data found in the response.")
        except Exception as e:
            print(f"An error occurred: {e}")
    else:
        print("Request failed with status code:", response.status_code)
        print("Response content:", response.text)
```

### 6. GPT-4O Real-Time Models

**Note:** Code examples for the `gpt-4o-realtime` models will be provided soon. If you have experience using OpenAI's official real-time API, you may attempt to use these models similarly. **We encourage you to contribute working code examples to our GitHub repository.**

---

## Error Handling

The API returns errors in a format compatible with OpenAI's API. Error responses include an HTTP status code and a JSON body with details.

**Error Response Example:**

```json
{
  "error": {
    "message": "The requested model does not exist",
    "type": "invalid_request_error",
    "param": "model",
    "code": "model_not_found"
  }
}
```

**Common Error Codes:**

- `400 Bad Request`: The request was invalid or cannot be served.
- `401 Unauthorized`: Authentication failed or user does not have permissions.
- `404 Not Found`: The requested resource does not exist.
- `429 Too Many Requests`: Rate limit exceeded.
- `500 Internal Server Error`: An error occurred on the server.

---

## Rate Limits

- **Hosting Details:** The service is hosted on Hugging Face Spaces (primary) and Railway's free tier (secondary).
- **Usage Guidelines:** Please use the API responsibly. Excessive requests may lead to rate limiting or temporary bans.
- **Recommended Use:** Implement client-side rate limiting and exponential backoff strategies to handle potential `429 Too Many Requests` responses.

---

## Support and Contact

For support, bug reports, or inquiries, please reach out through the following channels:

### Developer and Maintainer:

- **Name:** DevsDoCode (Sreejan)
- **Telegram:** [@DevsDoCode](https://t.me/DevsDoCode)
- **Instagram:** [@sree.shades_](https://www.instagram.com/sree.shades_/)
- **YouTube:** [DevsDoCode](https://www.youtube.com/@DevsDoCode)
- **GitHub:** [SreejanPesonal](https://github.com/SreejanPesonal)

### Original Founder:

- **Name:** Mr Leader
- **YouTube:** [@mr_leaderyt](https://www.youtube.com/@mr_leaderyt)

---

## Credits and Acknowledgments

- **Mr Leader**
  - **Contribution:** Original founder and reverse engineer of the API.
  - **Expertise:** Python Development, Reverse Engineering.
  - **Note:** Special thanks for the foundational work and expertise in establishing this API.

- **DevsDoCode (Sreejan)**
  - **Contribution:** Current maintainer and developer.
  - **Expertise:** Enhancements, additional features, ongoing maintenance.

---

## Changelog

### Version 1.0.1

- **Date:** 2024-11-06
- **Changes:**
  - Added `/audio/speech` endpoint for Text-to-Speech functionality.
  - Added `/images/generations` endpoint for image generation.
  - Introduced **gpt-4o-audio-preview** and **gpt-4o-realtime** model families.
  - Updated available models in `/models` endpoint.
  - Improved API compatibility with OpenAI's API.

### Version 1.0.0

- **Date:** 2023-11-03
- **Initial Release**

---

## License and Legal Notice

This API service is provided for educational and testing purposes. Users are responsible for ensuring compliance with all applicable laws and regulations when using the service.

- **Disclaimer:** The service is unofficial and not affiliated with OpenAI.
- **Usage:** By using this API, you agree to use it responsibly and not for any malicious activities.

---

## Frequently Asked Questions (FAQ)

**Q1:** *Is this API compatible with OpenAI's official API clients and libraries?*

**A:** The API endpoints and response formats are designed to be compatible with OpenAI's API. However, it is currently **not optimized for use with the OpenAI Python SDK**. Users should interact with the API using standard HTTP requests.

**Q2:** *Do I need an API key to use this service?*

**A:** Currently, no API key or authentication token is required. However, this may change in the future, and users should follow any updates.

**Q3:** *What are the limitations of this API service?*

**A:** The service is hosted on platforms with free-tier limitations. Therefore, performance and availability may vary. For critical applications, it's recommended to consider these factors.

**Q4:** *Can I use this API for commercial purposes?*

**A:** The API is provided primarily for educational and testing purposes. Commercial use is not officially supported, and users should proceed with caution and ensure compliance with any applicable terms and regulations.

---

## Final Notes

While this API aims to provide a compatible and accessible interface for AI model interactions, users should be aware of the following:

- **Stability:** Being hosted on free-tier services may affect reliability.
- **Updates:** The API may undergo changes. Users should keep an eye on the `/about` endpoint for the latest information.
- **Ethical Use:** Please ensure that your use of the API aligns with ethical guidelines and does not promote harm.

---

## Feedback

Your feedback is valuable. If you encounter any issues or have suggestions for improvements, please reach out via the contact channels provided above.

Thank you for using the UN-OFFICIAL OPENAI API Service!

---
