# GenAI Assignment:

**Evaluation Criteria**

We will score your submission on:

* Clarity and practicality of architecture
* Robust JSON schema design
* Prompt quality (zero-shot, reliable, minimal hallucination risk)
* Handling of ambiguity + user review flow
* Bulk generation thinking (errors, naming, report)

## Problem 1: **Proposal for “Video-to-Notes”**

We have a local folder of long videos (3–4 hours each, 200MB+). Watching them fully is slow. We need an automated way to generate a “summary package” per video: **Summary.md** + highlight clips + screenshots, all organized per video. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

### **Task**

Prepare a **pre-processed solution proposal** comparing  **three approaches** **:**

1. **Online/Cloud-Based (Already Available Solutions)**
2. **Build Our Own Using LLM APIs (Hybrid: local media processing + cloud LLM)**
3. **Build Fully Offline Using Open-Source Models (Local transcription + local LLM + pipeline)**

No code required. We want a **clear, practical proposal** with architecture and tradeoffs.

### Your Solution for problem 1:

Solution Overview:
To solve the Video-to-Notes problem, we design an automated batch-processing pipeline that takes long videos from a folder and generates a structured summary package including Summary.md, highlight clips, and screenshots.

The system will work in fully automated batch mode and process all videos inside the input directory without manual intervention.

### Proposed Processing Pipeline

The system will process each video in the following steps:

1. Detect all video files inside the input folder.
2. Extract audio from each video using FFmpeg.
3. Convert extracted audio into text using a speech-to-text model (e.g., Whisper).
4. Split the transcript into manageable chunks for large videos (3–4 hours).
5. Send transcript chunks to an LLM to generate:
   - High-level summary
   - Key highlights with timestamps
   - Action items / takeaways
6. Use returned timestamps to:
   - Cut highlight clips from the original video
   - Capture screenshots at key moments
7. Generate a structured `Summary.md` file.
8. Save all outputs in a per-video folder structure.

### Approach 1: Fully Cloud-Based Solution

All processing (transcription + summarization) is done using external APIs.

Pros:
- High accuracy
- Easy to implement
- Scalable

Cons:
- High cost for long videos
- Data privacy concerns
- API rate limits


### Approach 2: Hybrid Solution (Local + Cloud LLM)

Media processing is done locally. Transcription and summarization use cloud APIs.

Pros:
- Balanced cost
- Better control
- Scalable
- Practical for production

Cons:
- Some API dependency
- Slightly more complex setup


### Approach 3: Fully Offline (Open-Source Models)

Everything runs locally using open-source models.

Pros:
- No API cost
- Full data privacy
- No external dependency

Cons:
- Requires GPU
- Lower summarization quality
- Complex setup

### Recommended Approach

The Hybrid approach is recommended because it balances cost, scalability, accuracy, and privacy. 

Local media processing ensures efficient handling of large video files, while cloud-based LLMs provide high-quality summarization and structured outputs.

This approach is practical for production use and supports batch processing of long videos.


## Problem 2: **Zero-Shot Prompt to generate 3 LinkedIn Post**

Design a **single zero-shot prompt** that takes a user’s persona configuration + a topic and generates **3 LinkedIn post drafts** in **3 distinct styles**, each aligned to the user’s voice and constraints. The output must be structured so the app can: show 3 drafts to the user. Assume we are consuming **OpenAI API / Gemini API** with **one prompt call** (no fine-tuning). Your prompt must reliably produce valid, structured output. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**TASK:** Write a prompt that can work.

### Your Solution for problem 2:

You need to put your solution here.

## Problem 3: **Smart DOCX Template → Bulk DOCX/PDF Generator (Proposal + Prompt)**

Users have many Word documents that act like templates (offer letters, certificates, invoices, contracts). They repeatedly change only a few fields (name, date, amount, address, role, etc.). Doing this manually is slow and error-prone, especially in bulk. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

We want a system that:

1. Converts an uploaded **DOCX** into a reusable **template** by identifying editable fields.
2. Supports **single generation** (form-fill → DOCX/PDF download).
3. Supports **bulk generation** via **Excel/Google Sheet** rows.

### **Task (No coding)**

Submit a **proposal** for building this system using GenAI (OpenAI/Gemini) for “template field detection” and “field schema generation”. We want a practical design, not code.

### Your Solution for problem 3:

You need to put your solution here.

## Problem 4: Architecture Proposal for 5-Min Character Video Series Generator

We want to build a system that helps a user create a short video series (around **5 minutes per episode**) using predefined characters. The user defines characters (image + personality) and relationships once, then provides a short story/situation for an episode. The system outputs a complete episode package and optionally a final video. [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

### **Task**

Create a **small, clear architecture proposal** (no code, no prompts) describing how you would design and build this system.

### Your Solution for problem 4:

You need to put your solution here.
