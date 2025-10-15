# Uninstall / Cleanup

Stop and remove the persistent container:

```bash
docker stop nemotron_vllm || true
docker rm nemotron_vllm || true
```

Optionally delete the image (re-downloads next install):

```bash
docker rmi nvcr.io/nvidia/vllm:25.09
```

Optional cache cleanup (removes downloaded weights):

```bash
rm -rf ~/.cache/huggingface/hub/*
```
