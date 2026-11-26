# EP001 — LTX-2.5 22B vs MiniMax H3 on 8 GB VRAM (15 Standardized Runs)

## Included ComfyUI Workflows (`.json`)

| File | Model | Mode | Best Use on 8 GB VRAM |
|---|---|---|---|
| `LTX25_22B_8GB_480p_to_2x_Latent_Upscale.json` | **LTX-2.5 22B** (`INT8_ConvRot`) | T2V + 2× Latent Spatial Upscaler (`1664×960`) | **Winning 8 GB Recipe (`1m 49s`)** |
| `LTX25_22B_8GB_T2V_Base.json` | **LTX-2.5 22B** (`INT8_ConvRot`) | T2V (`480p` / `720p` / `10s`) | Daily Driver (`42s` @ 480p, `1m 37s` @ 720p) |
| `LTX25_22B_8GB_I2V_Portrait.json` | **LTX-2.5 22B** (`INT8_ConvRot`) | I2V (`832×480`) | Fast identity lock (`46s`, `75°C`) |
| `MiniMax_H3_8GB_T2V_LowVRAM.json` | **MiniMax H3** (`INT8 + Turbo V4`) | T2V (`480p` / `720p`) | Overnight Batch macro physics & depth |
| `MiniMax_H3_8GB_I2V_LowVRAM.json` | **MiniMax H3** (`INT8 + Turbo V4`) | I2V (`832×480`) | Requires structured audio prompt below |

---

## Full 15-Run Measured Telemetry (RTX 3070 Laptop · 8 GB VRAM · 32 GB RAM)

All runs measured at **24 FPS · 8 distilled steps · Native Audio ON** (Warm Render with `seed+1` after `POST /free` + `≤ 60°C` cooldown).

| Test | Resolution / Length | LTX-2.5 Warm | MiniMax H3 Warm | Speed Ratio | Peak Commit | Notes |
|---|---|---:|---:|---:|---:|---|
| **CTRL-01 Rain** | `832×480` · 5s | **42s** (75°C) | 2m 11s (81°C) | **LTX 3.1×** | 42.8 GB | Ground puddles look synthetic on both |
| **CTRL-01 Rain** | `1280×704` · 5s | **1m 37s** (77°C) | 5m 22s (84°C) | **LTX 3.3×** | 43.8 GB | H3 runner faces away + piano hallucination |
| **CTRL-01 Upscale** | `480p → 1664×960` · 5s | **1m 49s** (77°C) | — | **2.7× vs 1080p** | 43.6 GB | **Best 8 GB Recipe** (no letterbox, clean rain) |
| **CTRL-02 Portrait I2V** | `832×480` · 5s | **46s** (75°C) | 2m 17s (82°C*) | **LTX 3.0×** | 43.1 GB | *H3 thermal throttled + hallucinated male voice |
| **WILD-01 Coffee** | `1280×704` · 5s | **1m 36s** (76°C) | 5m 21s (84°C) | **LTX 3.3×** | 43.9 GB | **H3 wins** liquid physics & pouring foley |
| **WILD-02 Night Alley** | `1280×704` · 5s | **1m 37s** (76°C) | 5m 19s (84°C) | **LTX 3.3×** | 43.7 GB | **H3 wins** depth; LTX cleaner rain audio |
| **CEILING 1 (10s)** | `1280×704` · 10s (241f) | **4m 00s** (77°C) | 13m 08s (85°C) | **LTX 3.3×** | 44.3 GB | LTX treadmill gait; H3 covers real distance |
| **CEILING 2 (1080p)** | `1920×1088` · 5s | **4m 57s** (78°C) | **71m 15s** (84°C) | **LTX 14.4×** | **44.6 GB** | **H3 NO-GO ON 8 GB** (PageFile thrashing) |

---

## MiniMax H3 Structured Audio Prompt Fix (Stops Speech/Piano Hallucinations)

When running MiniMax H3, avoid plain English prompts if you want clean natural foley without invented dialogue or background music:

```text
visual: Macro close-up of hands pouring steaming dark coffee from a glass carafe into a ceramic cup, condensation droplets, realistic liquid stream.
diegetic_sfx: steady hot liquid pouring into ceramic mug, subtle acoustic pitch rising as cup fills.
dialogue: N/A
non_diegetic_music: N/A
```

## License Notes
- **LTX-2.5 22B:** Lightricks Community License (free for commercial use under $10M annual revenue).
- **MiniMax H3:** Carries regional self-hosting restrictions in the US, EU, UK, and South Korea — benchmarked here strictly for independent technical review.
