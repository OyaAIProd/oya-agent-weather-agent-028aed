# weather agent

> Built with [Oya AI](https://dev.oya.ai)

## About

You are a Weather Agent specialized in providing timely, accurate, and easy-to-understand weather information. Your expertise is in meteorology-based data interpretation and delivering clear weather forecasts, alerts, and explanations. You communicate in a friendly and approachable tone, ensuring users feel comfortable asking about weather conditions, forecasts, and climate-related questions. You specialize in helping users plan their days or events based on weather insights, while avoiding speculation or generalized advice outside verified weather data.

## Configuration

- **Mode:** skills
- **Agent ID:** `e2e1f4dd-f706-4248-bace-489eac0800b4`
- **Model:** `gemini/gemini-2.0-flash`

## Usage

Every deployed agent exposes an **OpenAI-compatible API endpoint**. Use any SDK or HTTP client that supports the OpenAI chat completions format.

### Authentication

Pass your API key via either header:
- `Authorization: Bearer a2a_your_key_here`
- `X-API-Key: a2a_your_key_here`

Create API keys at [https://dev.oya.ai/api-keys](https://dev.oya.ai/api-keys).

### Endpoint

```
https://dev.oya.ai/api/v1/chat/completions
```

### cURL

```bash
curl -X POST https://dev.oya.ai/api/v1/chat/completions \
  -H "Authorization: Bearer a2a_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"model":"gemini/gemini-2.0-flash","messages":[{"role":"user","content":"Hello"}]}'

# Continue a conversation using thread_id from the first response:
curl -X POST https://dev.oya.ai/api/v1/chat/completions \
  -H "Authorization: Bearer a2a_your_key_here" \
  -H "Content-Type: application/json" \
  -d '{"model":"gemini/gemini-2.0-flash","messages":[{"role":"user","content":"Follow up"}],"thread_id":"THREAD_ID"}'
```

### Python

```python
from openai import OpenAI

client = OpenAI(
    api_key="a2a_your_key_here",
    base_url="https://dev.oya.ai/api/v1",
)

# First message — starts a new thread
response = client.chat.completions.create(
    model="gemini/gemini-2.0-flash",
    messages=[{"role": "user", "content": "Hello"}],
)
print(response.choices[0].message.content)

# Continue the conversation using thread_id
thread_id = response.thread_id
response = client.chat.completions.create(
    model="gemini/gemini-2.0-flash",
    messages=[{"role": "user", "content": "Follow up question"}],
    extra_body={"thread_id": thread_id},
)
print(response.choices[0].message.content)
```

### TypeScript

```typescript
import OpenAI from "openai";

const client = new OpenAI({
  apiKey: "a2a_your_key_here",
  baseURL: "https://dev.oya.ai/api/v1",
});

// First message — starts a new thread
const response = await client.chat.completions.create({
  model: "gemini/gemini-2.0-flash",
  messages: [{ role: "user", content: "Hello" }],
});
console.log(response.choices[0].message.content);

// Continue the conversation using thread_id
const threadId = (response as any).thread_id;
const followUp = await client.chat.completions.create({
  model: "gemini/gemini-2.0-flash",
  messages: [{ role: "user", content: "Follow up question" }],
  // @ts-ignore — custom field
  thread_id: threadId,
});
console.log(followUp.choices[0].message.content);
```

### Swift

```swift
// Package.swift:
// .package(url: "https://github.com/MacPaw/OpenAI.git", from: "0.4.0")
import Foundation
import OpenAI

@main
struct Main {
    static func main() async throws {
        let config = OpenAI.Configuration(
            token: "a2a_your_key_here",
            host: "dev.oya.ai",
            scheme: "https"
        )
        let client = OpenAI(configuration: config)

        let query = ChatQuery(
            messages: [.user(.init(content: .string("Hello")))],
            model: "gemini/gemini-2.0-flash"
        )
        let result = try await withCheckedThrowingContinuation { continuation in
            _ = client.chats(query: query) { continuation.resume(with: $0) }
        }
        print(result.choices.first?.message.content ?? "")
    }
}
```

### Kotlin

```kotlin
// build.gradle.kts dependencies:
// implementation("com.aallam.openai:openai-client:4.0.1")
// implementation("io.ktor:ktor-client-cio:3.0.0")
import com.aallam.openai.api.chat.ChatCompletionRequest
import com.aallam.openai.api.chat.ChatMessage
import com.aallam.openai.api.chat.ChatRole
import com.aallam.openai.api.model.ModelId
import com.aallam.openai.client.OpenAI
import com.aallam.openai.client.OpenAIHost
import kotlinx.coroutines.runBlocking

fun main() = runBlocking {
    val openai = OpenAI(
        token = "a2a_your_key_here",
        host = OpenAIHost(baseUrl = "https://dev.oya.ai/api/v1/")
    )
    val completion = openai.chatCompletion(
        ChatCompletionRequest(
            model = ModelId("gemini/gemini-2.0-flash"),
            messages = listOf(ChatMessage(role = ChatRole.User, content = "Hello"))
        )
    )
    println(completion.choices.first().message.messageContent)
}
```

### Streaming

```python
stream = client.chat.completions.create(
    model="gemini/gemini-2.0-flash",
    messages=[{"role": "user", "content": "Tell me about AI agents"}],
    stream=True,
)
for chunk in stream:
    delta = chunk.choices[0].delta.content
    if delta:
        print(delta, end="", flush=True)
```

### Embeddable Widget

```html
<!-- Oya Chat Widget -->
<script
  src="https://dev.oya.ai/widget.js"
  data-agent-id="e2e1f4dd-f706-4248-bace-489eac0800b4"
  data-api-key="a2a_your_key_here"
  data-title="weather agent"
></script>
```

### Supported Models

- `gemini/gemini-2.0-flash`
- `gemini/gemini-2.5-flash`
- `gemini/gemini-2.5-pro`
- `gemini/gemini-3-flash-preview`
- `gemini/gemini-3-pro-preview`
- `anthropic/claude-sonnet-4-5-20241022`
- `anthropic/claude-haiku-4-5-20251001`

---

*Managed by [Oya AI](https://dev.oya.ai). Do not edit manually — changes are overwritten on each sync.*