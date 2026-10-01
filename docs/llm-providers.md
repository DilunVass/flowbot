# AI models

Flowbot uses two kinds of model. Flows-only chatbots need just the first, and it runs on your own server.

- **Routing model** (local): reads a typed question and picks the flow node that answers it. It never writes answers itself.
- **Knowledge model** (OpenAI, optional): writes answers from your documents when no flow node fits.

## Routing model

### In-process (default)

Flowbot runs a GGUF model file itself, so nothing else needs to be installed. Put the file at `models/model.gguf` (see [models/README.md](../models/README.md)) and keep:

```bash
LLM_MODE=in_process
LLM_MODEL_PATH=llm_models/model.gguf
```

The model loads on the first message after each start, which takes a few seconds. It answers one message at a time. If you get enough traffic that replies start queueing, switch to Ollama.

### Ollama

[Ollama](https://ollama.com) handles several messages at once, and it can use a GPU. With Ollama installed on the same server:

```bash
ollama pull qwen3:0.6b
```

Ollama only listens on `127.0.0.1` by default, which the Flowbot container can't reach. Set `OLLAMA_HOST=0.0.0.0` for the Ollama service (see Ollama's FAQ), and firewall port 11434 from the internet. Then set in `.env`:

```bash
LLM_MODE=http
LLM_BASE_URL=http://host.docker.internal:11434/v1
LLM_MODEL=qwen3:0.6b
```

and run `docker compose up -d`. Any OpenAI-compatible server works the same way, for example `llama-server` from llama.cpp.

### Choosing a model

Qwen3 0.6B is small and quick on a CPU, and it's what Flowbot is tested with. A larger model routes unusual phrasings more accurately, but it replies more slowly and uses more memory. Check the license of any model you use; Qwen3 is Apache 2.0.

## Knowledge tier (OpenAI)

The knowledge tier is off by default because it's the only tier that costs money per message. To turn it on, add an [OpenAI API key](https://platform.openai.com/api-keys) to `.env`:

```bash
RAG_ENABLED=true
OPENAI_API_KEY=sk-...
```

and run `docker compose up -d`. Then set a chatbot's mode to **hybrid** or **documents only**, and add documents in its knowledge base.

> **OpenAI only, for now.** The knowledge tier currently supports OpenAI as its only provider. Support for other providers is planned for a later release. This doesn't affect the routing model above, which runs locally or on any OpenAI-compatible server.

### What leaves your server

- When a document is processed, its text goes to OpenAI's embedding model.
- When a question reaches the knowledge tier, the question goes to the embedding model. The question and the best-matching passages then go to the chat model.

Nothing is sent when a menu, rule or flow node answers the question.

### Tuning

`RAG_SCORE_THRESHOLD` controls how closely a passage must match before Flowbot answers from it. If the bot hands off questions your documents do cover, lower it slightly (for example to `0.7`). If it gives weak answers, raise it. To see match scores for real questions, use **Test retrieval** in the chatbot's knowledge base.
