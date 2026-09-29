# models

Put the model file for `LLM_MODE=in_process` here, named `model.gguf`.

Flowbot is tested with Qwen3 0.6B (about 640 MB, Apache 2.0 license):

```bash
curl -fL -o models/model.gguf \
  https://huggingface.co/Qwen/Qwen3-0.6B-GGUF/resolve/main/Qwen3-0.6B-Q8_0.gguf
```

Any GGUF chat model works. Larger models route messages more accurately, but they
answer more slowly and use more memory. This folder is mounted read-only into the
container, so replace the file and run `docker compose restart` to switch models.

If you use `LLM_MODE=http` instead, this folder can stay empty.
