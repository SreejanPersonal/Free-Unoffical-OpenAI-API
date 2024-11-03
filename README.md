# GPT API Service Documentation

## Introduction
The GPT API Service, originally reverse-engineered by Mr Leader and further developed by DevsDoCode (Sreejan), provides a powerful interface for interacting with various GPT models. This innovative service combines Mr Leader's foundational expertise with DevsDoCode's development capabilities to deliver a robust, OpenAI-compatible API with enhanced features.

## Base URL
```
https://great-blondell-devsdocodeorg-cc6b3578.koyeb.app
```

## Authentication
Currently, the API doesn't require authentication tokens. However, it's recommended to use the service responsibly and within reasonable limits.

## Available Endpoints

### 1. List Available Models
Retrieve information about all available models.

**Endpoint:** `GET /models`

**Response Example:**
```json
{
  "object": "list",
  "data": [
    {
      "id": "gpt-4o-mini-2024-07-18",
      "object": "model",
      "created": 1715367049,
      "owned_by": "DevsDoCode & Mr Leader"
    },
    // ... other models
  ]
}
```

### 2. Chat Completions
Generate text responses using various GPT models.

**Endpoint:** `POST /v1/chat/completions`

#### Non-Streaming Request
```python
{
    "messages": [
        {"role": "user", "content": "How many 'r's are there in Strawberry?"}
    ],
    "model": "gpt-4o-mini-2024-07-18",
    "temperature": 0.5,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "top_p": 1
}
```

#### Streaming Request
```python
{
    "messages": [
        {"role": "user", "content": "Write 10 lines on India"}
    ],
    "model": "gpt-4o-mini-2024-07-18",
    "temperature": 0.5,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "top_p": 1,
    "stream": true
}
```

### 3. Audio Preview
Generate both text and audio responses using the audio-capable model.

**Endpoint:** `POST /v1/chat/completions`

**Request:**
```python
{
    "messages": [
        {"role": "user", "content": "How are you doing?"}
    ],
    "model": "gpt-4o-audio-preview",
    "modalities": ["text", "audio"],
    "audio": {
        "voice": "fable",
        "format": "wav"
    },
    "temperature": 0.9,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "top_p": 1
}
```

#### Available Voices
- alloy
- echo
- fable
- onyx
- nova
- shimmer

For detailed audio implementation guidelines, refer to the [OpenAI Audio Documentation](https://platform.openai.com/docs/guides/audio).


### 4. About Information
Get detailed information about the API service.

**Endpoint:** `GET /about`

## Available Models

1. **GPT-4 Series**
   - `gpt-4o-mini-2024-07-18`
   - `gpt-4o-mini`
   - `gpt-4o`
   - `gpt-4`

2. **GPT-3.5 Series**
   - `gpt-3.5-turbo-1106`
   - `gpt-3.5-turbo-0613`
   - `gpt-3.5-turbo`
   - `gpt-3.5-turbo-0125`
   - `gpt-3.5-turbo-0301`

3. **Audio Models**
   - `gpt-4o-audio-preview-2024-10-01`
   - `gpt-4o-audio-preview`

4. **Embedding Models**
   - `text-embedding-3-large`
   - `text-embedding-3-small`
   - `text-embedding-ada-002`

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| messages | array | Array of message objects with role and content |
| model | string | ID of the model to use |
| temperature | float | Controls randomness (0-1) |
| presence_penalty | float | Penalty for new topics |
| frequency_penalty | float | Penalty for repetition |
| top_p | float | Nuclear sampling parameter |
| stream | boolean | Enable streaming responses |

## Code Examples

### 1. Non-Streaming Request
```python
import requests

def send_non_streaming_request():
    url = "https://great-blondell-devsdocodeorg-cc6b3578.koyeb.app/v1/chat/completions"
    payload = {
        "messages": [{"role": "user", "content": "Hello!"}],
        "model": "gpt-4o-mini-2024-07-18",
        "temperature": 0.5,
        "stream": False
    }
    response = requests.post(url, json=payload)
    return response.json()
```

### 2. Streaming Request
```python
import requests

def send_streaming_request():
    url = "https://great-blondell-devsdocodeorg-cc6b3578.koyeb.app/v1/chat/completions"
    payload = {
        "messages": [{"role": "user", "content": "Write a story"}],
        "model": "gpt-4o-mini-2024-07-18",
        "temperature": 0.5,
        "stream": True
    }
    
    with requests.post(url, json=payload, stream=True) as response:
        for line in response.iter_lines(decode_unicode=True):
            if line and line.startswith("data: "):
                try:
                    content = json.loads(line[6:])
                    if "choices" in content:
                        print(content["choices"][0]["delta"]["content"], end="", flush=True)
                except:
                    continue
```

### 3. Audio Preview Request
```python
import requests
import base64
import json

def send_audio_request():
    url = "https://great-blondell-devsdocodeorg-cc6b3578.koyeb.app/v1/chat/completions"
    payload = {
        "messages": [{"role": "user", "content": "Tell me a joke"}],
        "model": "gpt-4o-audio-preview",
        "modalities": ["text", "audio"],
        "audio": {
            "voice": "fable",
            "format": "wav"
        },
        "temperature": 0.9
    }
    
    response = requests.post(url, json=payload)
    if response.ok:
        data = response.json()
        if "choices" in data and data["choices"]:
            message = data["choices"][0].get("message", {})
            
            # Text response
            if "content" in message:
                print("Text:", message["content"])
            
            # Audio response
            if "audio" in message and "data" in message["audio"]:
                wav_bytes = base64.b64decode(message["audio"]["data"])
                with open("response.wav", "wb") as f:
                    f.write(wav_bytes)
                print("Audio saved as response.wav")
```

## Rate Limits and Usage Guidelines

- The service runs on Koyeb's free tier hosting
- Please maintain reasonable request intervals

### Technical Details
- Hosting: Koyeb Free Tier
- Version: 1.0.0
- Maintained by: DevsDoCode (Sreejan)

## Credits and Recognition

### Original Development
Special recognition goes to **Mr Leader** for his groundbreaking work in reverse engineering this API. His expertise in Python development and reverse engineering laid the foundation for this service.
- **Mr Leader** - Pioneer in reverse engineering this API and establishing its foundational architecture
  - Expertise: Python Development, Reverse Engineering
  - YouTube: [@mr_leaderyt](https://www.youtube.com/@mr_leaderyt)

### Current Maintenance and Enhancement
- **DevsDoCode (Sreejan)** - Current maintainer and developer
  - Enhanced API functionality and documentation
  - Implemented additional features and optimizations

## Support and Contact

For bug reports or support:
- Telegram: [@DevsDoCode](https://t.me/DevsDoCode)
- Instagram: [@sree.shades_](https://www.instagram.com/sree.shades_/)
- YouTube: [DevsDoCode](https://www.youtube.com/@DevsDoCode)
- GitHub: [SreejanPesonal](https://github.com/SreejanPesonal)

For original API inquiries:
- YouTube: [@mr_leaderyt](https://www.youtube.com/@mr_leaderyt)

## Legal Notice
This API is provided for testing and educational purposes only. Users are responsible for ensuring their usage complies with relevant laws and regulations.
