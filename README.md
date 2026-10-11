# Qwen Image for ComfyUI by Ace Data Cloud

Use the current [QwenImageDify](https://github.com/AceDataCloud/QwenImageDify) tool contracts as independent ComfyUI nodes. This package exposes all **3 Dify tool definitions** and **2 fixed model IDs** from source snapshot `7e3866e`. It calls the hosted Ace Data Cloud API; no local model or GPU is needed. Paid actions consume Credits at the [current service price](https://platform.acedata.cloud/models).

[简体中文](docs/README.zh-CN.md) · [Ace Data Cloud](https://platform.acedata.cloud) · [Issues](https://github.com/AceDataCloud/QwenImageComfyUI/issues)

## Install

When the Registry version is active, search **Qwen Image** in ComfyUI-Manager, confirm publisher **acedatacloud** and package **Qwen Image by Ace Data Cloud**, install, and restart. Registry ID: `qwen-image`. Source repository: `AceDataCloud/QwenImageComfyUI`. Source code, CI, or Registry upload alone does not prove a market install.

For local source testing, clone this repository into `ComfyUI/custom_nodes/QwenImageComfyUI` and run `python -m pip install -r requirements.txt` in ComfyUI's environment, then restart. Native media loaders require ComfyUI 0.39.0 or newer. This package uses ComfyUI's existing image/audio/video libraries for decoding.

## Key and tool inputs

Set `ACEDATACLOUD_API_KEY` (or `ACEDATACLOUD_API_TOKEN`) in the **ComfyUI process environment** and restart. Keep the key out of workflow JSON, screenshots, issues, and the repository.

Each Dify tool has its own node. Its `parameters_json` input accepts the same JSON fields as the Dify API contract. The model and action selectors expose every fixed choice in the source YAML and override those fields in JSON when selected. `(from JSON/default)` keeps a value supplied by JSON or the API default. Task lookup nodes also offer `task_id` and `wait_seconds`; batch lookup nodes offer `ids_json`. Contract validation happens before a request. Paid submissions are never retried after a connection failure; preserve an accepted task ID and inspect request history before paying again.

| Dify tool | Required fields in JSON | Model choices | Action choices |
| --- | --- | --- | --- |
| `qwen_image` | `prompt` | `qwen-image-3.0`, `qwen-image-3.0-pro` | — |
| `qwen_image_task_retrieve` | `task_id` | — | — |
| `qwen_image_tasks_retrieve_batch` | `ids` | — | — |

Import [01-configure-and-run.json](examples/01-configure-and-run.json) with **File → Open**, fill its placeholder task ID or JSON parameters, then run. The example is a configuration template; it does not submit a paid request by itself. Query the same task ID until `status` is `succeeded`. The node returns status, task ID, all media URLs as JSON, first media URL, normalized result JSON, and trace ID. Connect a completed Ace CDN URL to the provided `IMAGE` loader where applicable.

Check current service pricing before a paid run. Read-only task queries and media downloads should not generate a new paid task. Destructive Dify actions still require an explicit `confirm: true` in JSON. See [privacy notes](docs/PRIVACY.md).

## Development

```bash
python -m unittest discover -s tests -v
python -m py_compile __init__.py client.py api_contracts.py nodes.py
```

`pyproject.toml` declares short Registry ID `qwen-image`, publisher `acedatacloud`, and a 400×400 proportional resize of the [canonical Ace logo](https://cdn.acedata.cloud/assets/logo.png).
