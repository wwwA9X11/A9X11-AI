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

## 🔔 SSE & Streaming Events

Enable `stream: true` to receive **Server-Sent Events** (SSE) for live processing and token updates.

Event types:

- connected – connection established  
  Payload example: { "type": "connected", "timestamp": 1699999999999 }

- stage – current processing stage (analyzing, thinking, web_search, generation, tools, TTS)  
  Payload example: { "type": "stage", "stage": "thinking", "tokens_processed": 1234 }

- search_start – web search initiated  
  Payload example: { "type": "search_start", "query": "latest AI news" }

- search_results – partial web search results returned  
  Payload example: { "type": "search_results", "results": [{"title":"AI breakthrough","url":"https://example.com"}], "tokens_processed": 432 }

- tools_start – tool execution starting  
  Payload example: { "type": "tools_start", "tool": "code_execution", "tokens_processed": 100 }

- tools_complete – tool execution finished  
  Payload example: { "type": "tools_complete", "tool": "code_execution", "output": "...", "tokens_processed": 210 }

- chunk – partial text/content generated in real-time  
  Payload example: { "type": "chunk", "text": "Hello, world", "tokens_processed": 10 }

- image_generated – new AI-generated image available  
  Payload example: { "type": "image_generated", "url": "https://example.com/image.png" }

- transcription – audio transcription returned  
  Payload example: { "type": "transcription", "text": "Hello world", "tokens_processed": 12 }

- tts_start – text-to-speech synthesis started  
  Payload example: { "type": "tts_start", "text": "Hello world" }

- tts_complete – TTS finished  
  Payload example: { "type": "tts_complete", "audio_url": "https://example.com/audio.mp3" }

- done – AI generation fully completed  
  Payload example: { "type": "done", "tokens_total": 500 }

- error – an error occurred during processing  
  Payload example: { "type": "error", "message": "Something went wrong" }

---

## 🔧 Advanced Usage

- Batch queries: submit multiple messages in one request  
- Depth control: "quick" for lightweight, "deep" for detailed reasoning  
- Custom prompts: define system instructions for AI behavior  
- Session management: use conversationId to maintain context  
- Cost optimization: choose lower-cost models for high-volume tasks  

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
Support us with a coffee: [Buy Me a Coffee](https://www.buymeacoffee.com/playauraaiw)
**Built with ❤️ by A9X11 Team**
