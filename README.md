# 🎙️ Voxtral Mini — Docker Image

A Docker image for serving [Voxtral-Mini-4B-Realtime](https://huggingface.co/mistralai/Voxtral-Mini-4B-Realtime-2602)
via [vLLM](https://github.com/vllm-project/vllm), with an OpenAI-compatible API.

## 📦 What's inside

- `vllm/vllm-openai` as the base image
- Extra dependencies: `soxr`, `librosa`, `soundfile`, `transformers`
- Serves `mistralai/Voxtral-Mini-4B-Realtime-2602` on port `8000`

## 🚀 Usage

Pull and run the latest image:

```bash
docker run -p 8000:8000 ghcr.io/virtuos/vllm-voxtral:latest
```

### Run with compose

```yaml
services:
  voxtral:
    image: ghcr.io/virtuos/vllm-voxtral:latest
    entrypoint: 
      - vllm
      - serve
      - mistralai/Voxtral-Mini-4B-Realtime-2602
      - --compilation_config
      - '{"cudagraph_mode": "PIECEWISE"}'
    ports:
      - "8000:8000"
    environment:
      - VLLM_DISABLE_COMPILE_CACHE=1
    volumes:
      - huggingface-cache:/root/.cache/huggingface
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]

volumes:
  huggingface-cache:
```

## 🔧 Build locally

```bash
# with default (latest) vLLM version
docker build -t voxtral-mini .

# with a specific vLLM version
docker build --build-arg VLLM_VERSION=v0.17.1 -t voxtral-mini .
```

## 🤖 Automated builds

Daily at 6am UTC or by manual trigger `.github/workflows/build-push-image.yml` checks for a new vLLM release and rebuilds if needed.

## 📋 Requirements

- Docker with GPU support (nvidia-container-toolkit)

## 📝 License

MIT

## ✒️ Authors

virtUOS, Osnabrueck University
