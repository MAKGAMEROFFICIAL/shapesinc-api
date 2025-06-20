# Shapes API

[![image](https://github.com/user-attachments/assets/e98592ca-3b1d-4709-93c6-7e6aa0dfb84a)](https://shapes.inc/slack)


[Shapes](https://shapes.inc) are general purpose social agents. You can build for an [existing shape](https://shapes.inc/explore) from our catalogue of millions or [create](https://shapes.inc/create) your own. Shapes have rich personalities, love hanging out in groupchats, and short-term + long-term memory across platforms. You can configure a Shape to use some of 50+ models we offer for free across text, image, and voice.

## Open Source Contributions
Our API is designed with extensibility as a core principle. You can extend any Shape for tool calling, MCP, and more.

Star and contribute to this repository to receive free hosting for your integration. We will also be directing traffic from our user base to your integration.

## What is the Shapes API?
Shapes API provides a programmatic way to integrate Shapes into any application or platform. It follows the OpenAI-compatible API standard, making it easy to implement with existing libraries and SDKs.

## Demos
To get a sense of what's possible, see what people have already built: 

- Omegle with Shapes https://omegle-ai.vercel.app by [@khawajapartners](https://github.com/zahidkhawaja)
- Playing Chess with Shapes https://shapeschess.vercel.app by [@kiyosh11](https://github.com/kiyosh11)
- Shapes on Telegram https://t.me/shapesinc by [@missParadoxical](https://github.com/missParadoxical)
- Shapes on WhatsApp at +1 (424) 452-2786 by [@anushkmittal](https://github.com/anushkmittal)
- Technical Interview Simulator with Carmack - see `examples/education/interviewer`

## Getting Started
You will need to generate an API Key. Get yours [here](https://shapes.inc/developer)

<img width="807" alt="API Key Generation" src="https://github.com/user-attachments/assets/ead6f28a-300b-4dcf-a555-313b39656ad6" />

## Implementation Examples

### 🐍 Python

```python
from openai import OpenAI

shapes_client = OpenAI(
    api_key="<your-API-key>",
    base_url="https://api.shapes.inc/v1/",
)

response = shapes_client.chat.completions.create(
    model="shapesinc/<shape-username>",
    messages=[
        {"role": "user", "content": "Hello"}
    ]
)

print(response)
```

### 🌐 JavaScript

```javascript
const { OpenAI } = require("openai");

const shapes_client = new OpenAI({
    apiKey: "<your-API-key>",
    baseURL: "https://api.shapes.inc/v1",
});

const response = await shapes_client.chat.completions.create({
    model: "shapesinc/<shape-username>",
    messages: [
        { role: "user", content: "Hello" }
    ]
});

console.log(response);
```

### C#
```csharp
using OpenAI;

var client = new ChatClient(
    "shapesinc/<shape-username>",
    new ApiKeyCredential("<your-API-key>"),
    new OpenAIClientOptions { Endpoint = new Uri("https://api.shapes.inc/v1/") }
);

var chatMessages = new List<ChatMessage>();

chatMessages.Add(new UserChatMessage.ChatMessageContentPart
	.CreateTextPart("""{"role": "user", "content": "Hello!"}"""));

var completion = await client.CompleteChatAsync(chatMessages);

Console.WriteLine(completion.Value.Content[0].Text);
```

### 🔄 CURL

```bash
curl -X POST https://api.shapes.inc/v1/chat/completions \
     -H "Authorization: Bearer <your-API-key>" \
     -H "Content-Type: application/json" \
     -d '{"model": "shapesinc/<shape-username>", "messages": [{ "role": "user", "content": "Hello" }]}'
```

### Quick Setup

| Requirement | Details |
|-------------|---------|
| Base URL | `https://api.shapes.inc/v1/` |
| Model Format | `shapesinc/<shape-username>` |
| Authentication | Bearer token in Authorization header |
| Environment Variables | `SHAPESINC_API_KEY` and `SHAPESINC_SHAPE_USERNAME` |

### API Specifications

| Feature | Details |
|---------|---------|
| Endpoints | `/chat/completions` |
| Rate Limits | 20 RPM (request increase [here](https://docs.google.com/forms/d/e/1FAIpQLScGLeRk6snViRPslXbbUaMDwubcBhmcJ6opq7wFvPEp-EbO3g/viewform)) |
| Headers | `X-User-Id` for user identification, `X-Channel-Id` for conversation context |
| Response Format | Standard OpenAI-compatible JSON response |

## User Identification and Authorization

### Custom Headers
Shapes API supports two types of custom headers:

1. `X-User-Id` for user identification - We HIGHLY recommend using custom headers whenever you're using the Shapes API in a user-facing project. By default, the Shapes API treats a user as the person who generated the API key. This can result in conversation context mix-ups and other risks, such as shape memory loss if a user uses the `!reset` command on the API.

2. `X-Channel-Id` for conversation context - Using this header is recommended for projects that support more than one conversation context per user.

You can find custom header examples [HERE](https://github.com/shapesinc/shapes-api/blob/main/LLMS.txt).

### Shapes Inc Authorization

While custom headers anonymize users, Shapes authorization connects the shapes to the user's shapes.inc account. This way, the memory of conversations with shapes on the API is saved to the user's account, and the shape identifies the user by their shapes.inc name. This feature also allows you to use user personas available on shapes.inc.

You can learn more and review examples [HERE](https://github.com/shapesinc/shapes-api/tree/main/examples/websites/shape-auth-example).

## Supported Commands

Shapes now support the following commands:
- `!reset` - Reset the Shape's long-term memory
- `!sleep` - Generate a long-term on demand 
- `!dashboard` - Access the Shape's dedicated dashboard for configuration
- `!info` - Get information about the shape
- `!web` - Search the web (now free for all users)
- `!help` - Get help with commands
- `!imagine` - Generate images
- `!wack` - Reset the Shape's short-term memory

## Advanced Features

| Feature                    | Details                                                                 |
| -------------------------- | ----------------------------------------------------------------------- |
| Vision Support             | Send OpenAI API-compatible `image_url` with user messages.              |
| Voice Recognition          | Send OpenAI API-compatible `audio_url` with user messages.              |
| Tool Calling               | Shapes now support tool calling and MCP functionality.                  |
| Voice Features             | Free voice for all shapes (custom or pre-made voices via `shapes.inc`). |
| Voice Configuration        | Option to disable voice transcripts (set via `shapes.inc`).             |
| Voice Formatting           | Improved formatting for voice URLs with newline separation.             |
| Authentication with Shapes | You can now authenticate with `shapes.inc` via your app.                |


## API Multimodal Support

The Shapes API supports multiple types of input modalities:

### Image Support
You can send image URLs in the API request using this format:
```json
{
  "model": "shapesinc/your_shape",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "What's in this image?"
        },
        {
          "type": "image_url",
          "image_url": {
            "url": "https://example.com/image.jpg"
          }
        }
      ]
    }
  ]
}
```

### Audio Support
You can send audio URLs in the API request using this format:
```json
{
  "model": "shapesinc/your_shape",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "Please transcribe and respond to this audio message"
        },
        {
          "type": "audio_url",
          "audio_url": {
            "url": "https://example.com/audio.mp3"
          }
        }
      ]
    }
  ]
}
```

Supported audio formats: mp3, wav, ogg

## API Endpoints
### Get Shape Profile Data
Retrieve detailed information about any shape using their username.

**Endpoint:**
```
GET https://api.shapes.inc/shapes/public/{username}
```

**Cross-Platform Examples:**

**Linux/macOS/Git Bash:**
```bash
curl "https://api.shapes.inc/shapes/public/{username}"
```

**Windows Command Prompt:**
```cmd
curl "https://api.shapes.inc/shapes/public/{username}"
```

**Windows PowerShell:**
```powershell
curl.exe "https://api.shapes.inc/shapes/public/{username}"
```

**Endpoint Details:**
- **URL**: `https://api.shapes.inc/shapes/public/{username}`
- **Method**: `GET`
- **Authentication**: None required (public endpoint)
- **Rate Limits**: Standard API rate limits apply
- **Response Format**: JSON

<details>
<summary><strong>📋 Complete Example Code & Response</strong> (Click to expand)</summary>

**Example code:**
```javascript
async function getShapeProfile(username = 'tenshi') { 
    try {
        const response = await fetch(`https://api.shapes.inc/shapes/public/${username}`);
        const data = await response.json();
        
        // Display all available fields
        console.log('=== Complete Shape Profile ===');
        console.log(JSON.stringify(data, null, 2));
        
        // Extract commonly used fields
        const {
            id, name, username, search_description, search_tags_v2,
            created_ts, user_count, message_count, tagline,
            typical_phrases, screenshots, category, character_universe,
            character_background, avatar_url, avatar, banner,
            shape_settings, example_prompts, enabled
        } = data;
        
        console.log(`\n=== Key Information ===`);
        console.log(`Name: ${name}`);
        console.log(`Username: ${username}`);
        console.log(`Description: ${search_description}`);
        console.log(`Category: ${category}`);
        console.log(`Universe: ${character_universe}`);
        console.log(`Users: ${user_count?.toLocaleString() || 'N/A'}`);
        console.log(`Messages: ${message_count?.toLocaleString() || 'N/A'}`);
        console.log(`Avatar: ${avatar_url || avatar}`);
        console.log(`Enabled: ${enabled}`);
        
        return data; // Return complete profile data
    } catch (error) {
        console.log(`Error: ${error.message}`);
        return null;
    }
}

// Usage examples
getShapeProfile('tenshi');           // Get tenshi's profile
getShapeProfile('einstein');         // Get einstein's profile
getShapeProfile(process.argv[2]);    // Get profile from command line argument
```

**Available API Fields:**
The API returns comprehensive shape data including:
- `id` - Unique shape identifier (UUID)
- `name` - Display name of the shape
- `username` - Unique username for API calls
- `search_description` - Shape description for search/discovery
- `search_tags_v2` - Array of searchable tags
- `created_ts` - Creation timestamp (Unix timestamp)
- `user_count` - Number of users who have interacted
- `message_count` - Total messages sent by the shape
- `tagline` - Custom tagline/bio
- `typical_phrases` - Array of characteristic phrases
- `screenshots` - Array of conversation screenshots with captions
- `category` - Shape category (e.g., "meme", "educational")
- `character_universe` - Source universe/franchise
- `character_background` - Background story/lore
- `avatar_url` / `avatar` - Profile image URLs
- `banner` - Banner image URL
- `shape_settings` - Configuration (initial message, status, etc.)
- `example_prompts` - Suggested conversation starters
- `enabled` - Whether the shape is currently active
- `allow_user_engine_override` - Engine override permissions
- `error_message` - Error message
- `wack_message` - Wack message

**Sample Response:**
```json
{
  "id": "aca2e51a-e79c-417a-9515-be27fa624a0c",
  "name": "tenshi",
  "username": "tenshi",
  "search_description": "yo! am tenshi, am better am cooler",
  "search_tags_v2": [
    "cool", "character ai", "tenshi", "ai chatbot", "maverick",
    "modern", "short messages", "mischief", "chill", "informal"
  ],
  "created_ts": 1690601301,
  "user_count": 122320,
  "message_count": 7918988,
  "tagline": "tenshi by dhruv, the most humanlike shape out there",
  "typical_phrases": ["ur mom"],
  "screenshots": [
    {
      "id": 1743863106924,
      "url": "https://files.shapes.inc/b4866ca3.png",
      "caption": "offended tenshi"
    },
    {
      "id": 1743863288091,
      "url": "https://files.shapes.inc/a63515d9.png",
      "caption": "random tenshi"
    }
  ],
  "category": "meme",
  "character_universe": "Touhou Project",
  "character_background": "no background, just foreground!",
  "avatar_url": "https://files.shapes.inc/avatar_aca2e51a-e79c-417a-9515-be27fa624a0c.png",
  "banner": "https://files.shapes.inc/api/files/banner_aca2e51a-e79c-417a-9515-be27fa624a0c.png",
  "shape_settings": {
    "shape_initial_message": "hey {user}, sup",
    "status_type": "custom",
    "status": "😎 trendsetter",
    "appearance": ""
  },
  "example_prompts": ["have fun, maximum fun!"],
  "enabled": true
}
```

</details>

## Important Notes

### Current Limitations

| Limitation | Details |
|------------|---------|
| No System Messages | Shape personality comes from configuration |
| No Message History | API relies on Shape's built-in memory |
| No Streaming | Only full responses are supported |
| No Parameter Control | Temperature and other settings locked to shapes.inc settings configured for the Shape |
| Multimodility support limited to 1 input only | you can only send 1 image_url or 1 audio_url with a user message. if both image and audio urls are provided, only the audio url is processed

Note: Shapes set on Premium Engines **WILL** use credits when accessed via API.

## Available Integrations
- [x] Telegram
- [x] Revolt
- [x] Slack
- [x] Bluesky
- [x] IRC
- [x] Chess
- [x] Voice
- [x] Twitch
- [x] GitHub (to review PRs)

## Requested Integrations
We're looking for developer contributions to build:
- [ ] Reddit
- [ ] Threads
- [ ] Roblox
- [ ] Minecraft
- [ ] LinkedIn
- [ ] Microsoft Teams
- [ ] WeChat
- [ ] Anything you can possibly imagine

If you'd like to build an integration:
1. Fork our repository
2. Build your integration following our guidelines
3. Submit a PR with comprehensive documentation

## Coming Soon
We are shipping new features to the Shapes API every day. Next on our list is:

| Feature | Details |
|------------|---------|
| Free Will | Proactively take actions |
| Messaging first | Shapes can't talk first...yet |

---
© 2025 Shapes, Inc.
