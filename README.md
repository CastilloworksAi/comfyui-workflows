# ComfyUI Workflows

A collection of ComfyUI workflows covering the full image generation and LoRA training pipeline — from basic generation to Flux portraits to training your own LoRA from scratch.

Built and tested on a local GPU setup. All workflows run offline, no cloud required.

---

## Workflows

### Generation

| File | What it does | Model |
|---|---|---|
| `SDXL_Default.json` | Clean starting point for image generation | JuggernautXL (SDXL) |
| `X1.json` | Same as SDXL_Default with educational notes on the canvas explaining prompting | JuggernautXL (SDXL) |
| `ThinkDiffusion_LoRA.json` | SDXL generation with a LoRA slot — swap in any trained LoRA | JuggernautXL (SDXL) |
| `flux_creative_v1.json` | High-quality Flux generation, no LoRAs, clean slate | Flux Dev (GGUF) |
| `flux_portrait_v1.json` | Flux portraits with add_details + AntiBlur LoRAs pre-loaded | Flux Dev (GGUF) |
| `image_z_image_turbo.json` | Fast generation using z-image-turbo + Qwen text encoder | z-image-turbo |

### Dataset Preparation

| File | What it does |
|---|---|
| `face_dataset_builder.json` | Takes one reference face photo, generates 30-50 synthetic variations for training |
| `caption_single.json` | Captions a single image using WD14 Tagger + vision LLM, outputs a .txt file |
| `caption_batch.json` | Same as caption_single but processes an entire folder automatically |

### Training

| File | What it does |
|---|---|
| `flux_lora_train.json` | Full Flux LoRA training workflow — reads dataset, trains, outputs .safetensors |

---

## Models needed

These workflows reference the following model files. You'll need to download them separately and place them in the correct ComfyUI model folders.

**Flux workflows** (`diffusion_models/`):
- `flux1-dev-Q5_K_S.gguf` — main Flux model (~9.5GB, fits 16GB VRAM with LoRAs)

**SDXL workflows** (`checkpoints/`):
- `juggernautXL_v9.safetensors` — or any SDXL checkpoint you prefer
- `Realistic_Vision_V5.1_fp16.safetensors` — used by face_dataset_builder

**VAE** (`vae/`):
- `ae.safetensors` — required for Flux workflows

**Text encoders** (`text_encoders/`):
- `clip_l.safetensors`
- `t5xxl_fp8_e4m3fn.safetensors`

**Flux LoRAs** (`loras/flux/`) — optional, used by flux_portrait_v1:
- `FLUX-dev-lora-add_details.safetensors`
- `FLUX-dev-lora-AntiBlur.safetensors`

---

## Quick start

1. Install [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
2. Download the model files listed above into the appropriate folders
3. Open ComfyUI at `http://localhost:8188`
4. Load any workflow: `Ctrl+O` → select the `.json` file
5. Replace the example prompt with your own and hit `Queue` (`Ctrl+Enter`)

---

## Tips

**Flux prompting** — Flux responds to natural language, not tag lists. Write sentences.
```
# Works well
"A orange tabby cat sitting on a sun-warmed windowsill, golden afternoon light,
 dust particles in the air, shallow depth of field, photorealistic"

# Works less well  
"cat, orange, window, sunlight, photorealistic, 8k, detailed"
```

**Seed control** — Set seed to `-1` for random results every time. Lock a specific number to reproduce a result you liked.

**Red lines on canvas** — A model file is missing. Click the loader node dropdown and pick a model file you actually have.

**LoRA strength** — Start at `0.7`. Go higher for stronger likeness, lower to blend more naturally with the base model. Above `1.0` usually introduces artifacts.

---

## LoRA training

For a full guide on using the `flux_lora_train.json` workflow — including dataset prep, captioning, training settings, and using your LoRA after training — see the companion repo: [lora-training-guide](https://github.com/CastilloworksAi/lora-training-guide)

---

MIT License — castilloworks.ai
