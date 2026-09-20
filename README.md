# ✈️ Smart Airport Passenger Assistance Multimodal Chatbot

> MSc in Artificial Intelligence — BSBI  
> Student: Geethu Satheesh | Year: 2025–2027

---

## Project Overview

This project implements a **Smart Airport Passenger Assistance Chatbot** capable of processing four different types of passenger input — text, image, voice, and combined image+text — to help passengers navigate airport facilities. The system retrieves relevant location information from a structured airport knowledge base and returns natural language responses.

The chatbot was built entirely in **Google Colab** using free, open-source tools and deployed as a live web interface using **Gradio**.

---

## Features

| Modality | Model Used | Description |
|---|---|---|
| 💬 Text Query | DistilBERT (fine-tuned) | Classifies passenger intent from typed questions |
| 🖼️ Image Query | CLIP + FAISS | Identifies airport facility from an uploaded photo |
| 🎤 Voice Query | Whisper (base) | Transcribes spoken audio then classifies intent |
| 🔀 Combined Query | CLIP + DistilBERT | Processes image and text simultaneously |

---

## Models and Libraries

- **DistilBERT** (`distilbert-base-uncased`) — Fine-tuned for 8-class intent classification
- **CLIP** (`openai/clip-vit-base-patch32`) — Zero-shot image embedding (512 dimensions)
- **FAISS** (`IndexFlatIP`) — Cosine similarity search over 16 airport image embeddings
- **Whisper** (`openai/whisper-base`) — Zero-shot automatic speech recognition
- **gTTS** — Generated 24 synthetic MP3 audio query files
- **Gradio** — 4-tab web interface deployed via Google Colab public URL
- **HuggingFace Transformers 5.x** — Model loading and inference
- **PyTorch 2.x** — GPU/CPU tensor operations

---

## Dataset

| Dataset | Size | Description |
|---|---|---|
| Text queries | 200 queries (25 per class) | Synthetic passenger questions in CSV format |
| Image dataset | 16 images (2 per class) | Airport facility photos across 8 categories |
| Audio dataset | 24 MP3 files (3 per class) | Synthetic voice queries generated with gTTS |

**Intent categories:** gate, baggage\_claim, check\_in, security, transportation, lounge, restaurant, restroom

---

## Results

| Component | Metric | Result |
|---|---|---|
| DistilBERT Text Classifier | Validation Accuracy | ~95% (200-query dataset) |
| Whisper ASR | Word Error Rate | ~4% (23/24 correct transcriptions) |
| CLIP + FAISS Image Search | Top-1 Accuracy | 100% on 16-image index |
| Combined Query (Text + Image) | Confidence | 82.2% on restroom test |

---

## Project Structure

```
smart-airport-chatbot/
│
├── MMC_3RDSEM.ipynb          # Main Colab notebook (all 7 tasks)
├── airport_kb.json           # Airport knowledge base (locations, terminals, floors)
├── text_query_dataset.csv    # 200 labelled passenger text queries
├── requirements.txt          # Python dependencies
└── README.md                 # This file
```

> **Note:** The `airport_images/` folder (16 images) and `audio_queries/` folder (24 MP3 files) are generated/downloaded within the notebook. Large media files are not tracked in this repository.

---

## How to Run

1. Open the notebook in **Google Colab**: click the badge below or upload `MMC_3RDSEM.ipynb` manually.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

2. Set the runtime to **T4 GPU**: Runtime → Change runtime type → T4 GPU

3. Run all cells in order: Runtime → Run all

4. The Gradio interface will launch automatically at the end and provide a **public shareable URL** valid for 72 hours.

---

## Architecture

```
Passenger Input
       │
  ┌────┴─────────────────────────────────┐
  │  Text  │  Image  │  Voice  │Combined │
  └──┬─────┴────┬────┴────┬────┴───┬─────┘
     │          │         │        │
DistilBERT   CLIP+FAISS Whisper→ Both pipelines
Classifier   Search     DistilBERT simultaneously
     │          │         │        │
  └────────────┴──────────┴────────┘
                    │
          Airport KB Lookup
          (airport_kb.json)
                    │
          Natural Language Response
```

---

## Ethical Considerations

- No audio or personal data is stored beyond an active session (GDPR compliant)
- Confidence scores are shown with every prediction (transparency)
- Four input modalities support accessibility for diverse passengers
- Bias audit recommended before production deployment

---

## References

- Vaswani et al. (2017) — Attention Is All You Need
- Devlin et al. (2019) — BERT
- Sanh et al. (2019) — DistilBERT
- Radford et al. (2021) — CLIP
- Douze et al. (2024) — FAISS
- Radford et al. (2023) — Whisper
- Abid et al. (2019) — Gradio

---

## License

This project was created for academic purposes as part of the MSc Artificial Intelligence programme at BSBI. Not for commercial use.
