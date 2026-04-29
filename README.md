<div align="center">

# 🍂 St6.Audio Splitter

### After Effects AI Stem Splitter

**Split the audio of any layer into vocals, drums, bass, and music — directly inside After Effects, locally on your GPU.**

[![Version](https://img.shields.io/badge/version-1.0.0-orange?style=for-the-badge)](https://github.com/st6autumn/St6.Audio-Splitter/releases/latest)
[![Platform](https://img.shields.io/badge/platform-Windows-blue?style=for-the-badge&logo=windows)](https://github.com/st6autumn/St6.Audio-Splitter/releases/latest)
[![After Effects](https://img.shields.io/badge/After%20Effects-2020%2B-9999FF?style=for-the-badge&logo=adobeaftereffects)](https://www.adobe.com/products/aftereffects.html)
[![License](https://img.shields.io/badge/license-Proprietary-red?style=for-the-badge)](LICENSE)

**[⬇ Download Latest Release](https://github.com/st6autumn/St6.Audio-Splitter/releases/latest)**

---

*Made by [Autumn](https://linktr.ee/st6.autumn)*

</div>

---

## 📖 About

**St6.Audio Splitter** is a self-contained CEP extension for Adobe After Effects. Select any layer with audio, click **SPLIT**, and walk away — sixty seconds later your comp has four new layers — `St6.Splitter (Vocals)`, `St6.Splitter (Drums)`, `St6.Splitter (Bass)`, `St6.Splitter (Music)` — sitting right above the source.

It's the local-AE equivalent of [vocalremover.org/splitter-ai](https://vocalremover.org/splitter-ai), but you never leave the timeline. Powered by Meta's open-source [Demucs](https://github.com/facebookresearch/demucs) running locally on your GPU. No accounts, no cloud, no upload limits, no per-song fees.

The first time you launch the panel it auto-installs a Python venv, PyTorch, Demucs, and ffmpeg — one click in the panel, ~2.5 GB download. After that every future split is just SPLIT → wait → done.

---

## ✨ Features

### Core flow
- **One-click split** — select an audio (or AV) layer, hit SPLIT
- **WAV → MP3 → AVI render fallback** — uses AE's WAV template when available (AE 2025+), MP3 next, AVI lossless last; whichever AE accepts
- **GPU acceleration** — auto-detects CUDA capability, picks the right device. Falls back to CPU automatically if your torch build doesn't support your GPU's compute capability (e.g. RTX 50-series sm_120)
- **Auto-import** — the four stems land above the source layer at the same in-point, ready to be muted/soloed/mixed

### Stem actions
- **MERGE** — combine 2-4 stems into one with a click. Pop-up picker, ffmpeg `amix` does the math, original stems get cleaned up. Stack merges to make composites like `St6.Splitter (Bass+Drums+Vocals)`
- **DELETE** — remove a stem from the comp AND the file from disk in one click
- **SOLO (click the colored dot)** — toggle AE's solo flag on that stem. Active dot pulses with an amber ring

### Modes
- **4 STEMS** — full Demucs `htdemucs` separation: Vocals / Drums / Bass / Music
- **VOCALS ONLY** — runs `--two-stems=vocals` for ~3× faster splits when you only need to isolate vocals from instrumental

### Quality of life
- **Auto-update banner** — checks GitHub on launch, lights up if a newer release is published
- **Open Folder button** — jumps straight to `<projectDir>/St6.Splitter_temp/` in Explorer
- **Live Demucs progress** — model download %, separation %, all streamed into the panel's LOGS
- **Copy Logs** — pipes the entire log buffer to your clipboard via Windows `clip.exe` for easy bug reports
- **Solo / merge state synced with AE** — chip rings stay in sync with whatever you do directly in the timeline

---

## 🔄 Auto-Updates

St6.Audio Splitter checks GitHub for new releases on startup. When a new version is available:
1. An animated **"⬆ Update vX.Y.Z"** pill appears in the panel header
2. Click it → opens the release page in your default browser
3. Download the new `.zxp` and re-install via [ZXP Installer](https://aescripts.com/learn/zxp-installer/)

The check runs once per session and caches for 30 minutes to stay under GitHub's 60/hr unauthenticated rate limit.

---

## 📥 Install

1. Download the latest `St6.Audio.Splitter-vX.Y.Z.zxp` from the [Releases page](https://github.com/st6autumn/St6.Audio-Splitter/releases/latest)
2. Install [ZXP Installer](https://aescripts.com/learn/zxp-installer/) (free) if you don't already have it
3. Drag the `.zxp` onto ZXP Installer
4. Restart After Effects → **Window ▸ Extensions ▸ St6.Audio Splitter**
5. **First launch:** a red "Backend missing" banner appears at the bottom of the panel. Click **Run Setup** — a console window opens and installs Python 3.12 (if needed), creates a venv, installs Demucs + PyTorch with the right CUDA build for your GPU (cu128 for RTX 50-series, cu126 for 40-series, cu124 for older, CPU fallback if no CUDA), and downloads ffmpeg. ~2.5 GB total, one-time.
6. Once setup finishes, the banner goes away and the status flips to "Idle." You're ready.

---

## 🎯 Usage

```
Save your project (File ▸ Save) ─┐
                                  ↓
Select a layer with audio ───────→ SPLIT ─→ ~30-60s on GPU
                                              ↓
                            4 new layers added above source:
                              ├ St6.Splitter (Vocals)
                              ├ St6.Splitter (Drums)
                              ├ St6.Splitter (Bass)
                              └ St6.Splitter (Music)

Click the colored dot   →  Solo this stem in AE
Click MERGE on a stem   →  Pop-up to combine with others
Click DELETE on a stem  →  Remove layer + file
```

The four `.wav` files live permanently in `<your-project-folder>/St6.Splitter_temp/`. Move them, back them up, drag them into a DAW — they're regular 16-bit 48 kHz stereo PCM WAV files.

---

## 🛠️ Tech Stack

| Component | Purpose |
|-----------|---------|
| [CEP 9+](https://github.com/Adobe-CEP/CEP-Resources) | Adobe panel framework |
| [Demucs (htdemucs)](https://github.com/facebookresearch/demucs) | Source separation model — by Meta AI |
| [PyTorch](https://pytorch.org/) | GPU inference (cu128 / cu126 / cu124 / CPU auto-pick) |
| [FFmpeg](https://ffmpeg.org/) | Audio decode + `amix` for merging |
| [soundfile](https://github.com/bastibe/python-soundfile) (with stdlib `wave` fallback) | WAV writer (bypasses torchaudio's torchcodec dep) |
| Vanilla HTML / CSS / JS | Panel UI — burnt-amber theme matching St6.Lapidary |
| ExtendScript | AE host integration (render, import, solo, undo groups) |

---

## 📊 Stats

| Metric | Value |
|--------|-------|
| Frontend (panel UI) | ~750 lines |
| Backend (Python driver) | ~325 lines |
| Host (ExtendScript) | ~750 lines |
| Total panel size | < 50 KB (excluding venv) |
| Demucs model | htdemucs (~80 MB, downloaded on first split) |
| Output format | 16-bit 48 kHz stereo PCM WAV |
| Typical split time | 30 s on RTX 4070 · 60 s on RTX 3060 · 4-6 min CPU |

---

## 🐛 Troubleshooting

**"CUDA error: no kernel image is available"** — your installed PyTorch doesn't have kernels for your GPU's compute capability. The driver auto-falls-back to CPU, but for GPU speed re-run `setup_deps.bat` (it'll pick the right cu* wheel for your card).

**"Render finished but no media file found"** — AE rejected every render template. Open AE's render queue manually and check that at least the "Lossless" template is available; the panel will fall through to it.

**Setup hangs** — close the panel, kill the `python.exe` process if needed, delete `%LOCALAPPDATA%\st6.audio.splitter\venv`, re-run `setup_deps.bat` from a Terminal.

**Files won't delete after merge** — the panel retries 4× with 350 ms gaps. If it still fails, check the LOGS pane — Windows is probably holding the file (e.g. open in another app).

---

## 📜 License

© 2025-2026 Autumn. All rights reserved.

This software is proprietary. You may not copy, modify, distribute, or reverse-engineer any part of this application without explicit written permission from the author.

---

## 🙏 Acknowledgments

St6.Audio Splitter is built on top of incredible open-source projects:

- **[Demucs](https://github.com/facebookresearch/demucs)** — the source separation model that does all the heavy lifting. By Meta AI Research (Alexandre Défossez et al.). MIT licensed.
- **[PyTorch](https://pytorch.org/)** — the inference engine that runs Demucs on your GPU. By the PyTorch Foundation.
- **[FFmpeg](https://ffmpeg.org/)** — used for audio decoding and the `amix` merge filter. LGPL/GPL licensed.
- **[soundfile](https://github.com/bastibe/python-soundfile)** — clean Python WAV writer that lets us bypass torchaudio's bleeding-edge torchcodec dependency.
- **[Adobe CEP](https://github.com/Adobe-CEP/CEP-Resources)** — the panel framework that lets a CEP extension live inside After Effects.
- **[vocalremover.org](https://vocalremover.org/splitter-ai)** — the inspiration for this panel's UX. Theirs is online; this one runs in your AE.

These projects are maintained by their respective communities and are not affiliated with St6.Audio Splitter.

---

<div align="center">

**[⬇ Download Latest Release](https://github.com/st6autumn/St6.Audio-Splitter/releases/latest)**

*St6.Audio Splitter — by [Autumn](https://linktr.ee/st6.autumn) 🍂*

</div>
