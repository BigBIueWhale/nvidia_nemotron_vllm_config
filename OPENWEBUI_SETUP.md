# OpenWebUI Setup (vLLM backend)

These steps match my deployment style (loopback bind + Docker bridge access).

## 1) Run OpenWebUI (if not already)
I run OpenWebUI isolated and let it reach the host via the Docker host‑gateway mapping:

```bash
docker run -d   -p 127.0.0.1:3000:8080   --add-host=host.docker.internal:host-gateway   -v open-webui:/app/backend/data   --name open-webui --restart always   ghcr.io/open-webui/open-webui:v0.6.25
```

## 2) Point OpenWebUI to vLLM

- Provider: **OpenAI Compatible**
- Base URL: `http://host.docker.internal:8000`
- API Key: any non-empty string (e.g., `test`)
- Model name: `nvidia/NVIDIA-Nemotron-Nano-12B-v2`

This keeps both containers on loopback while still allowing OpenWebUI to reach vLLM via the Docker host gateway.

### Alternative (Docker bridge bind)
If you prefer the Docker bridge (useful when OpenWebUI is on another host), change the vLLM publish in `run_vllm.sh`:

```
BIND="172.17.0.1"
```

Then use Base URL: `http://172.17.0.1:8000` inside OpenWebUI.

## 3) Suggested app settings (optional)

- **LLM Providers → Default**: set to OpenAI Compatible provider above.
- **Task model**: a small non‑reasoning model is fine, but Nemotron works too.
- **Disable “Code Interpreter”** (I keep it off) and keep **“Enable Code Execution”** on `pyodide` if you like running code blocks inline.
