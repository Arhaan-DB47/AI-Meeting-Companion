# 🎧 AI Meeting Companion

> An AI-powered meeting assistant that transcribes audio recordings using **OpenAI Whisper** and extracts key points using **Meta's LLaMA** via IBM watsonx — progressing from simple scripts to a full Gradio web app.

Built as part of **Course 8 – Building Generative AI-Powered Applications with Python** in the [IBM AI Developer Professional Certificate](https://www.coursera.org/professional-certificates/applied-artifical-intelligence-ibm-watson-ai).

---

## 📌 Overview

This project builds an AI meeting companion step by step — starting with standalone speech-to-text and LLM scripts, then combining them into a single application that can:

1. **Transcribe** an uploaded audio file (speech → text)
2. **Analyze** the transcript and **extract key points** using a large language model

| # | File | What It Does |
|---|------|-------------|
| 1 | `simple_speech2text.py` | Transcribes a local audio file (`downloaded_audio.mp3`) using OpenAI Whisper |
| 2 | `simple_llm.py` | Tests the IBM watsonx LLaMA model with a sample prompt |
| 3 | `speech2text_app.py` | Gradio web app — upload audio and get a transcription |
| 4 | `speech_analyzer.py` | **Full pipeline** — upload audio → Whisper transcription → LLaMA key-point extraction via LangChain |

---

## 📂 Project Structure

```
AI-Meeting-Companion/
├── simple_speech2text.py     # Step 1 — Whisper STT on a local audio file
├── simple_llm.py             # Step 2 — Test IBM watsonx LLaMA model
├── speech2text_app.py        # Step 3 — Gradio app for audio transcription
├── speech_analyzer.py        # Step 4 — Full pipeline: STT + LLM summarization
├── downloaded_audio.mp3      # Sample audio file for testing
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Access to **IBM watsonx.ai** (for LLaMA model in steps 2 & 4)

### Installation

```bash
# Clone the repository
git clone https://github.com/Arhaan-DB47/AI-Meeting-Companion.git
cd AI-Meeting-Companion

# Install dependencies
pip install torch transformers gradio langchain ibm_watson_machine_learning
```

---

## 📖 Scripts — Step by Step

### Step 1: `simple_speech2text.py` — Whisper Transcription

Loads OpenAI's **Whisper Tiny (English)** model via Hugging Face and transcribes a local audio file.

```bash
python simple_speech2text.py
```

**How it works:**
- Uses the `automatic-speech-recognition` pipeline with `openai/whisper-tiny.en`
- Processes audio in 30-second chunks with a batch size of 8
- Transcribes `downloaded_audio.mp3` and prints the result

---

### Step 2: `simple_llm.py` — IBM watsonx LLaMA Test

Tests the **Meta LLaMA** model hosted on IBM watsonx.ai to verify the LLM connection works.

```bash
python simple_llm.py
```

**How it works:**
- Connects to IBM watsonx.ai (`us-south.ml.cloud.ibm.com`)
- Loads `meta-llama/llama-4-maverick-17b-128e-instruct-fp8` with controlled generation parameters (`temperature=0.1`, `max_tokens=700`)
- Wraps the model in a LangChain-compatible `WatsonxLLM` interface
- Sends a test prompt: *"How to read a book effectively?"*

---

### Step 3: `speech2text_app.py` — Gradio Transcription App

A Gradio web interface that lets users **upload any audio file** and receive a text transcription.

```bash
python speech2text_app.py
```

The app launches at `http://0.0.0.0:7860`.

**Features:**
- Drag-and-drop audio upload
- Real-time transcription using Whisper
- Clean UI with title and description

---

### Step 4: `speech_analyzer.py` — Full Meeting Companion ⭐

The **complete pipeline** — combines Whisper transcription with LLaMA-powered analysis. Upload a meeting recording, and the app will transcribe it and extract the key points.

```bash
python speech_analyzer.py
```

The app launches at `http://0.0.0.0:7860`.

**How it works:**
1. User uploads an audio file via Gradio
2. **Whisper** transcribes the audio to text
3. The transcript is passed to a **LangChain PromptTemplate**:
   ```
   List the key points with details from the context:
   The context: {transcribed text}
   ```
4. **LLaMA** (via IBM watsonx) analyzes the transcript and returns structured key points
5. Results are displayed in the Gradio text output

**Key implementation details:**
- Uses **LangChain** `PromptTemplate` + `LLMChain` to structure the LLM interaction
- LLaMA instruction format with `<s><<sys>>` / `<</sys>>` delimiters
- `temperature=0.1` for deterministic, factual summarization
- `max_new_tokens=800` to accommodate detailed key-point extraction

---

## 🧠 Models & Services Used

| Model / Service | Provider | Purpose |
|----------------|----------|---------|
| [openai/whisper-tiny.en](https://huggingface.co/openai/whisper-tiny.en) | OpenAI (via Hugging Face) | Speech-to-text transcription |
| [LLaMA 4 Maverick 17B Instruct](https://huggingface.co/meta-llama) | Meta (via IBM watsonx.ai) | Key-point extraction and text analysis |
| [IBM watsonx.ai](https://www.ibm.com/products/watsonx-ai) | IBM | LLM hosting and inference platform |

---

## 🛠️ Technologies & Libraries

| Library | Purpose |
|---------|---------|
| [Hugging Face Transformers](https://huggingface.co/docs/transformers) | Whisper model loading and ASR pipeline |
| [LangChain](https://python.langchain.com/) | Prompt templates and LLM chains |
| [IBM watsonx ML SDK](https://ibm.github.io/watson-machine-learning-sdk/) | Connection to IBM watsonx.ai LLaMA model |
| [Gradio](https://gradio.app/) | Web UI for audio upload and result display |
| [PyTorch](https://pytorch.org/) | Deep learning backend |

---

## 🔗 Related Projects

This project is part of a series built during the IBM AI Developer Professional Certificate (Course 8):

| Project | Description |
|---------|-------------|
| [Image Captioning with BLIP & Gradio](https://github.com/Arhaan-DB47/Img_captioning) | AI-powered image captioning — from local inference to automated web scraping |
| [Simple Chatbot Using LLM](https://github.com/Arhaan-DB47/Simple-chatbot-using-LLM) | Terminal-based chatbots using BlenderBot and SmolLM2 |
| [Integrate Chatbot into Web App](https://github.com/Arhaan-DB47/Integrate-chatbot-into-web-application) | Full-stack web chatbot with Flask + custom UI + Docker |
| [Voice Assistant](https://github.com/Arhaan-DB47/Voice-Assistant) | Voice-enabled assistant with OpenAI GPT + IBM Watson STT/TTS |
| **AI Meeting Companion** *(this repo)* | Audio transcription + LLM key-point extraction with Whisper + LLaMA |

---

## 📜 License

This project was created for educational purposes as part of the IBM AI Developer Professional Certificate on Coursera.

---

## 👤 Author

**Arhaan Khan** — [@Arhaan-DB47](https://github.com/Arhaan-DB47)
