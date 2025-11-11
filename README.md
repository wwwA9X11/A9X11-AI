# 🚀 A9X11 API

💎 Official API for the **A9X11 AI Platform**. Access 4 AI models with streaming support, web search, code execution, image analysis, TTS, and more. Designed for integration with websites, applications, or backend systems. Flexible for single queries, interactive sessions, and large-scale tasks.  


☕ Support the Project:  
[Buy Me a Coffee](https://www.buymeacoffee.com/playauraaiw)

---

## 📋 Table of Contents

- Overview  
- Authentication  
- Quick Start  
- API Endpoints  
- Features  
- SSE & Streaming Events  
- Advanced Usage  
- Configuration  
- Response Format  
- Troubleshooting  
- Rate Limits  
- License  
- Support  
- Roadmap  

---

## 🌟 Overview

A9X11 provides 4 versatile AI models, optimized for different workloads and costs:

Model: spinner-mini | Cost: $0.005 | Best For: Fast responses, lightweight queries  
Model: a9x11-mini | Cost: $0.0005 | Best For: Budget-friendly content generation  
Model: a9x11-3.0 | Cost: $0.0125 | Best For: Professional work, complex reasoning  
Model: a9x11-4.0 | Cost: $0.019 | Best For: Enterprise-scale content and large projects  

Supports single queries, streaming responses, batch processing, interactive sessions, and custom workflows.

---

## 🔑 Authentication

All requests require an API key. Include in headers:

    Authorization: Bearer YOUR_API_KEY

Optional environment variables:

    export A9X11_API_KEY="your-api-key"
    export A9X11_API_URL="https://api.a9x11.com"

---

## 🚀 Quick Start

### Single Query Example

POST https://api.a9x11.com/v1/completions  
Content-Type: application/json  
Authorization: Bearer YOUR_API_KEY

    {
      "model": "a9x11-mini",
      "messages": [{"role": "user", "content": "Explain quantum computing"}],
      "temperature": 0.7,
      "max_tokens": 4000,
      "stream": false,
      "depth": "normal"
    }

---

## 📡 API Endpoints

POST /v1/completions

Generates text, images, code execution results, web search results, TTS, and more.

**Parameters:**

- model (required) – "spinner-mini" | "a9x11-mini" | "a9x11-3.0" | "a9x11-4.0"  
- messages (required) – array of message objects  
- temperature (optional) – 0–2, default 0.7, controls creativity  
- max_tokens (optional) – maximum output tokens  
- stream (optional) – enable real-time output streaming  
- depth (optional) – "quick" | "normal" | "deep"  

**Response Example:**

    {
      "success": true,
      "model": "a9x11-mini",
      "response": "AI has revolutionized multiple industries...",
      "usage": {
        "input_tokens": 15,
        "output_tokens": 342,
        "total_tokens": 357
      },
      "execution_time": 2341
    }

---

## 🎨 Features

- Multi-model access: choose the best AI model for speed, complexity, or cost  
- Streaming responses: real-time output for interactive sessions or large content  
- Web search integration: supports Google, Bing, DuckDuckGo, Brave  
- Code execution: securely run code in multiple languages  
- Image analysis: OCR, object detection, scene understanding  
- Text-to-Speech / Speech-to-Text  
- File referencing: include files in queries via identifiers or URLs  
- Session management: stateful multi-step interactions  
- Export capabilities: save session data or conversation history  
- Custom prompts: fine-tune AI behavior and response style  
- Cost tracking: monitor token usage and spending  

---


🔄 Session Management & Reattachment

A9X11 supports stateful sessions using sessionId. This allows users to:

1️⃣ Start a Session

```
POST https://api.a9x11.com/v1/completions
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY

{
  "model": "a9x11-mini",
  "messages": [
    {"role":"user","content":"Create a Python script that communicates with an Arduino to control RGB lights via serial. Include clear README and example usage. Return files: arduino_sketch.ino, controller.py, README.md"}
  ],
  "stream": false,
  "sessionId": "session_abc123",
  "includeCodeExecution": false
}
```
Response should include generated source files (strings or file references). Store session_abc123 — it ties together subsequent uploads/edits/downloads.

2️⃣ Start session and request a safe, high-level Windows driver scaffold (non-actionable)

```
POST https://api.a9x11.com/v1/completions
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY
{
  "model": "a9x11-3.0",
  "messages": [
    {"role":"user","content":"Provide a high-level template and developer checklist for writing a Windows kernel-mode driver that exposes a simple device interface (no actual kernel code). Include build steps, signing guidance, and safety best practices. Output files: DRIVER_README.md, DESIGN.md"}
  ],
  "stream": false,
  "sessionId": "session_abc123",
  "includeCodeExecution": false
}
```
This asks for design & checklist only (safe). If you later need actual driver code, require explicit security review and legal validation.

✅Keep multi-step conversations or code execution context.

✅Reattach to an existing session after disconnect or when performing file/code operations.

3️⃣ Upload multiple files into the session (original DLL, patch instructions, source files)

```
POST https://api.a9x11.com/v1/files
Authorization: Bearer YOUR_API_KEY
(multipart/form-data)
Form fields:
  - file: original.dll
  - file: helper_patch_script.txt
  - file: design_notes.md
  - sessionId: session_abc123
```

Pass the session ID to attach files, run code, or resume SSE streaming.

4️⃣ Ask AI to patch a file (request safe, high-level modifications or apply provided patch script) — run code execution in sandbox

```
POST https://api.a9x11.com/v1/completions
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY
{
  "model": "a9x11-mini",
  "messages": [
    {"role":"user","content":"Using files attached in this session, apply the patch described in helper_patch_script.txt to original.dll and produce a new file named updated.dll. Validate structure and produce a report describing changes. Do not produce executable patching instructions in plain text; run the patch inside a sandbox and return the updated file reference."}
  ],
  "stream": true,
  "sessionId": "session_abc123",
  "files": ["original.dll","helper_patch_script.txt"],
  "includeCodeExecution": true
}
```
The API will run the orchestrated code execution in your configured sandbox/orchestrator. Response (or SSE) should include a mention/reference to updated.dll if created. Do not run this against untrusted binaries without sandboxing.
```
5️⃣ Reattach later to continue work (reuse same sessionId)
POST https://api.a9x11.com/v1/completions
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY

{
  "model": "a9x11-mini",
  "messages": [
    {"role":"user","content":"Continue the previous session: run additional tests on updated.dll, attach test results to session, and produce diagnostics.txt summarizing results."}
  ],
  "stream": true,
  "sessionId": "session_abc123",
  "includeCodeExecution": true
}
```
Because session_abc123 is reused, the orchestrator can access previously attached files and state.

⚠️ Note: Sessions expire after 1 day (24 hours). After that, a new sessionId is required.

6️⃣ Download updated file(s) from the session
GET https://api.a9x11.com/v1/files/download?name=updated.dll&sessionId=session_abc123
Authorization: Bearer YOUR_API_KEY
This returns binary data for the updated file. In browser, convert to Blob and trigger download (see earlier example with URL.createObjectURL and creating an <a> element).
## 🔔 SSE & Streaming Events

Enable `stream: true` to receive **Server-Sent Events** (SSE) for live processing and token updates.

Event types:
```
- connected – connection established  
  Payload example: { "type": "connected", "timestamp": 1699999999999 }
```
```
- stage – current processing stage (analyzing, thinking, web_search, generation, tools, TTS)  
  Payload example: { "type": "stage", "stage": "thinking", "tokens_processed": 1234 }
```
```
- search_start – web search initiated  
  Payload example: { "type": "search_start", "query": "latest AI news" }
```
```
- search_results – partial web search results returned  
  Payload example: { "type": "search_results", "results": [{"title":"AI breakthrough","url":"https://example.com"}], "tokens_processed": 432 }
```
```
- tools_start – tool execution starting  
  Payload example: { "type": "tools_start", "tool": "code_execution", "tokens_processed": 100 }
```
```
- tools_complete – tool execution finished  
  Payload example: { "type": "tools_complete", "tool": "code_execution", "output": "...", "tokens_processed": 210 }
```
```
- chunk – partial text/content generated in real-time  
  Payload example: { "type": "chunk", "text": "Hello, world", "tokens_processed": 10 }
```
```
- image_generated – new AI-generated image available  
  Payload example: { "type": "image_generated", "url": "https://example.com/image.png" }
```
```
- transcription – audio transcription returned  
  Payload example: { "type": "transcription", "text": "Hello world", "tokens_processed": 12 }
```
```
- tts_start – text-to-speech synthesis started  
  Payload example: { "type": "tts_start", "text": "Hello world" }
```
```
- tts_complete – TTS finished  
  Payload example: { "type": "tts_complete", "audio_url": "https://example.com/audio.mp3" }
```
```
- done – AI generation fully completed  
  Payload example: { "type": "done", "tokens_total": 500 }
```
```
- error – an error occurred during processing  
  Payload example: { "type": "error", "message": "Something went wrong" }
```
---

## 🔧 Advanced Usage

- Batch queries: submit multiple messages in one request  
- Depth control: "quick" for lightweight, "deep" for detailed reasoning  
- Custom prompts: define system instructions for AI behavior  
- Session management: use conversationId to maintain context  
- Cost optimization: choose lower-cost models for high-volume tasks  
- Submit tasks: POST /v1/completions with sessionId + files
- Download session files: GET /v1/files/download?sessionId=YOUR_SESSION_ID
---

## 🔍 Response Format

    {
      "success": true,
      "model": "a9x11-mini",
      "response": "Generated text or analysis...",
      "usage": {
        "input_tokens": 15,
        "output_tokens": 342,
        "total_tokens": 357
      },
      "execution_time": 2341
    }

---

## ⚠️ Troubleshooting

- Check API key is correct and in headers  
- Slow responses: reduce depth or choose faster models  
- Connection issues: verify https://api.a9x11.com is accessible  
- Balance errors: check account credits  

---

## 📊 Rate Limits

- Tracked per API key  
- No hard rate limits; fair usage policies apply  

---

## 📝 License

Proprietary – A9X11.COM  

---

## 🤝 Support

Website: https://a9x11.com  
API Status: https://status.a9x11.com  

---

## 🎯 Roadmap

- GraphQL API support  
- WebSocket integration  
- Additional AI models  
- Multi-modal AI capabilities  
- Fine-tuning options  
- Enterprise features  

---

**Enjoying A9X11?**  
☕Support us with a coffee: [Buy Me a Coffee](https://www.buymeacoffee.com/playauraaiw)  

**Built with ❤️ by A9X11 Team**
