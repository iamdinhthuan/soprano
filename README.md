<div align="center">
  
  # Soprano: Instant, Ultra‑Realistic Text‑to‑Speech

  [![Alt Text](https://img.shields.io/badge/HuggingFace-Model-orange?logo=huggingface)](https://huggingface.co/ekwek/Soprano-80M)
  [![Alt Text](https://img.shields.io/badge/HuggingFace-Space-yellow?logo=huggingface)](https://huggingface.co/spaces/ekwek/Soprano-TTS)
</div>


<!-- Embedded demo video (placeholder) -->

<p align="center">
</p>

---

## Overview

**Soprano** is an ultra‑lightweight, open‑source text‑to‑speech (TTS) model designed for real‑time, high‑fidelity speech synthesis at unprecedented speed, all while remaining compact and easy to deploy.

With only **80M parameters**, Soprano achieves a real‑time factor (RTF) of **~2000×**, capable of generating **10 hours of audio in under 20 seconds**. Soprano uses a **seamless streaming** technique that enables true real‑time synthesis in **<15 ms**, multiple orders of magnitude faster than existing TTS pipelines.

---

## Installation

**Requirements**: Linux or Windows, CUDA‑enabled GPU required (CPU support coming soon).

### One‑line install

```bash
pip install soprano-tts
```

### Install from source

```bash
git clone https://github.com/ekwek1/soprano.git
cd soprano
pip install -e .
```

> **Note**: Soprano uses **LMDeploy** to accelerate inference by default. If LMDeploy cannot be installed in your environment, Soprano can fall back to the HuggingFace **transformers** backend (with slower performance). To enable this, pass `backend='transformers'` when creating the TTS model.

---

## Usage

```python
from soprano import SopranoTTS

model = SopranoTTS()
```

### Basic inference

```python
out = model.infer("Hello world!")
```

### Save output to a file

```python
out = model.infer("Hello world!", "out.wav")
```

### Custom sampling parameters

```python
out = model.infer(
    "Hello world!",
    temperature=0.3,
    top_p=0.95,
    repetition_penalty=1.2,
)
```

### Batched inference

```python
out = model.infer_batch(["Hello world!"] * 10)
```

#### Save batch outputs to a directory

```python
out = model.infer_batch(["Hello world!"] * 10, "/dir")
```

### Streaming inference

```python
import torch

stream = model.infer_stream("Hello world!", chunk_size=1)

# Audio chunks can be accessed via an iterator
chunks = []
for chunk in stream:
    chunks.append(chunk)

out = torch.cat(chunks)
```

---

## Key Features

### 1. High‑fidelity 32 kHz audio

Soprano synthesizes speech at **32 kHz**, delivering clarity that is perceptually indistinguishable from 44.1 kHz audio and significantly higher quality than the 24 kHz output used by many existing TTS models.

### 2. Vocos‑based neural decoder

Instead of slow diffusion decoders, Soprano uses a **Vocos‑based decoder**, enabling **orders‑of‑magnitude faster** waveform generation while maintaining comparable perceptual quality.

### 3. Seamless real‑time streaming

A novel streaming strategy leverages the decoder’s receptive field to generate audio with **ultra‑low latency**. The streamed output is acoustically identical to offline synthesis, enabling interactive applications with sub‑frame delays.

### 4. State‑of‑the‑art neural audio codec

Speech is represented using a **neural codec** that compresses audio to **~15 tokens/sec** at just **0.2 kbps**, allowing extremely fast generation and efficient memory usage without sacrificing quality.

### 5. Sentence‑level streaming for infinite context

Each sentence is generated independently, enabling **effectively infinite generation length** while maintaining stability and real‑time performance for long‑form generation.

---

## Limitations

* Trained on only 1,000 hours of data. The quality is already very good, but I expect it to get much better with even more training.
* No voice cloning support yet
* English‑only model

---

## Roadmap

* [x] Add model and inference code
* [x] Seamless streaming
* [x] Batched inference
* [ ] Command-line interface (CLI)
* [ ] Server / API inference
* [ ] Additional LLM backends
* [ ] CPU support
* [ ] Voice cloning
* [ ] Multilingual support

---

## Acknowledgements

Soprano builds on and is inspired by the following projects:

* [Vocos](https://github.com/gemelo-ai/vocos)
* [XTTS](https://github.com/coqui-ai/TTS)
* [LMDeploy](https://github.com/InternLM/lmdeploy)

---

## License

This project is licensed under the **Apache-2.0** license. See `LICENSE` for details.
