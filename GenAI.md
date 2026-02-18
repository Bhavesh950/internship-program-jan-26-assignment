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

### Solution Overview

### High-Level Architecture Flow

Input Folder → Video Loader → Audio Extraction → Transcription → 
Transcript Chunking → LLM Summarization → 
Timestamp Mapping → Clip Generator + Screenshot Extractor → 
Summary.md Builder → Output Folder

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

### Handling Large Videos & Batch Mode

- The system processes videos sequentially to avoid memory overload.
- Large transcripts are chunked before sending to the LLM to avoid token limits.
- Failed processing steps are logged and retried.
- Processing metadata (duration, processed date, status) is stored for tracking.

### Human Review & Quality Control

- The system first generates a draft Summary.md.
- Highlights and timestamps are shown to the user for review.
- The user can edit or approve timestamps before clip generation.
- Only approved highlights trigger clip and screenshot extraction.


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

### Zero-Shot Prompt Design

The following prompt is designed to generate 3 distinct LinkedIn post drafts in a single API call. 
It takes user persona configuration and a topic as input and returns structured JSON output 
containing three stylistically different LinkedIn posts.

### Final Zero-Shot Prompt

You are a professional LinkedIn content strategist.

Your task is to generate 3 distinct LinkedIn post drafts in different writing styles 
based on the provided user persona and topic.

INPUT:
- User Persona:
    - Name:
    - Role:
    - Industry:
    - Experience Level:
    - Tone Preference (e.g., professional, storytelling, bold, educational):
    - Target Audience:
    - Content Goal (e.g., engagement, authority building, hiring, lead generation):

- Topic:
    - Main topic of the post

INSTRUCTIONS:

1. Generate exactly 3 LinkedIn post drafts.
2. Each draft must follow a different style:
   - Post 1: Educational / Value-driven
   - Post 2: Storytelling / Personal insight
   - Post 3: Bold / Opinion-based / Contrarian
3. Each post must:
   - Be 150–250 words
   - Include a strong hook in the first 2 lines
   - Use short readable paragraphs
   - End with a soft call-to-action
4. Do NOT include explanations outside the JSON.
5. Output must be strictly valid JSON.
6. Do not add extra commentary.
7. Ensure the posts are realistic, avoid generic clichés, and align with the user's role and industry.
8. If any required persona field is missing or empty, return a valid JSON error object instead of generating posts.
9. Ensure the three posts do not repeat hooks, examples, or phrasing.

OUTPUT FORMAT (STRICT JSON):

{
  "post_1": {
    "style": "Educational",
    "hook": "string",
    "body": "string",
    "cta": "string"
  },
  "post_2": {
    "style": "Storytelling",
    "hook": "string",
    "body": "string",
    "cta": "string"
  },
  "post_3": {
    "style": "Bold",
    "hook": "string",
    "body": "string",
    "cta": "string"
  }
}

This prompt ensures structured output, stylistic variation, and reliability in a single API call without fine-tuning.


## Problem 3: **Smart DOCX Template → Bulk DOCX/PDF Generator (Proposal + Prompt)**

Users have many Word documents that act like templates (offer letters, certificates, invoices, contracts). They repeatedly change only a few fields (name, date, amount, address, role, etc.). Doing this manually is slow and error-prone, especially in bulk. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

We want a system that:

1. Converts an uploaded **DOCX** into a reusable **template** by identifying editable fields.
2. Supports **single generation** (form-fill → DOCX/PDF download).
3. Supports **bulk generation** via **Excel/Google Sheet** rows.

### **Task (No coding)**

Submit a **proposal** for building this system using GenAI (OpenAI/Gemini) for “template field detection” and “field schema generation”. We want a practical design, not code.

### Your Solution for problem 3:

### Solution Overview

We propose a Smart Template Processing System that converts uploaded DOCX files into reusable structured templates.

The system automatically detects editable fields using GenAI, generates a structured schema, and supports both single and bulk document generation in DOCX and PDF formats.

This solution reduces manual effort, minimizes errors, and enables scalable document automation.

### Proposed System Architecture

The system will work in the following stages:

1. Template Upload:
   - User uploads a DOCX template.
   - The file is parsed to extract raw text and structure.

2. Field Detection using GenAI:
   - Extracted text is sent to an LLM.
   - The LLM identifies editable fields (e.g., name, date, amount, address, role).
   - The model returns a structured JSON schema defining:
       - Field name
       - Field type (text, date, number, currency)
       - Required / optional
       - Example value

3. Schema Generation:
   - The detected fields are stored as a reusable template schema.
   - A dynamic form is generated based on the schema.

### Schema Review & Confirmation Step

- The detected fields are displayed to the user.
- The user can:
  - Rename fields
  - Change required/optional status
  - Modify field types
- The template is finalized only after user approval.

4. Single Document Generation:
   - User fills the form.
   - System replaces placeholders in DOCX.
   - Output: Generated DOCX + optional PDF version.

5. Bulk Generation:
   - User uploads Excel/Google Sheet.
   - Each row maps to the template schema.
   - System generates multiple DOCX/PDF files automatically.
  
### Bulk Processing Report

After bulk generation, the system provides:

- Total rows processed
- Successful documents generated
- Failed rows
- Downloadable error report (CSV)



 ### Example Template Field Schema (Generated by LLM)

{
  "template_name": "Offer Letter Template",
  "fields": [
    {
      "field_name": "candidate_name",
      "type": "text",
      "required": true,
      "example": "Rahul Sharma"
    },
    {
      "field_name": "joining_date",
      "type": "date",
      "required": true,
      "example": "2025-02-01"
    },
    {
      "field_name": "salary_amount",
      "type": "currency",
      "required": true,
      "example": "8,00,000 INR"
    }
  ]
}


### LLM Prompt for Template Field Detection

You are a document automation assistant.

Analyze the provided DOCX template text and identify all editable fields that are likely to change per document instance.

Instructions:
1. Identify placeholders such as names, dates, amounts, addresses, roles, IDs.
2. Do not include static content.
3. Return strictly valid JSON.
4. Infer field type (text, date, number, currency, email, etc.).
5. Mark fields as required unless clearly optional.

Output format:

{
  "template_name": "string",
  "fields": [
    {
      "field_name": "string",
      "type": "string",
      "required": true/false,
      "example": "string"
    }
  ]
}

### Error Handling & Validation

- Validate Excel columns against template schema.
- Highlight missing required fields.
- Log generation errors per row.
- Prevent incorrect data types (e.g., text in date field).
- Support retry mechanism for failed document generations.


## Problem 4: Architecture Proposal for 5-Min Character Video Series Generator

We want to build a system that helps a user create a short video series (around **5 minutes per episode**) using predefined characters. The user defines characters (image + personality) and relationships once, then provides a short story/situation for an episode. The system outputs a complete episode package and optionally a final video. [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

### **Task**

Create a **small, clear architecture proposal** (no code, no prompts) describing how you would design and build this system.

### Your Solution for problem 4:

### Proposed System Architecture

The system is divided into two major layers:

1. Series Setup Layer (Reusable Configuration)
2. Episode Generation Layer (Per-Episode Processing)

#### 1. Series Setup Layer (Series Bible)

This layer stores reusable character and world configuration.

Components:

- Character Manager:
  - Stores character reference images
  - Personality traits
  - Speaking style rules
  - Behavioral constraints
  - Visual consistency notes (clothing, age, expressions)

- Relationship Engine:
  - Defines relationships (friend, rival, parent-child, mentor)
  - Stores interaction tone rules

- World Settings Module:
  - Location / environment
  - Recurring themes
  - Series tone (comedy, drama, motivational)

All data is stored as a structured JSON-based “Series Bible”.

### Persistent Character Memory Store

- Each character is assigned:
  - Voice ID
  - Visual embedding reference
  - Style configuration
- These references ensure consistency across episodes.


#### 2. Episode Generation Layer (Per-Episode Processing)

This layer generates one complete 5-minute episode based on the Series Bible and user input.

Components:

- Episode Input Module:
  - Short story prompt (situation + conflict + outcome)
  - Selected characters (full cast or subset)
  - Tone (comedy, drama, motivational, slice-of-life)
  - Duration target (~5 minutes)
  - Language & format (9:16 / 16:9)

- Script Generator (LLM-based):
  - Expands short prompt into:
      - Scene-by-scene breakdown
      - Character dialogues
      - Narration (if required)
  - Ensures personality and relationship consistency
  - Controls pacing to fit ~5 minutes

- Scene Planner:
  - Determines number of scenes
  - Estimates time per scene
  - Generates storyboard / shot list

- Visual Asset Generator:
  - Generates prompts for:
      - Character visuals
      - Background settings
  - Maintains visual consistency using character reference data

- Audio Planner:
  - Voice lines per character
  - Narration segments
  - Background music cues

#### 3. Consistency & Control Engine

To maintain character and relationship consistency across episodes:

- Character Lock System:
  - Reuses stored character reference images.
  - Enforces personality traits and speaking style rules.
  - Prevents behavior that contradicts defined relationships.

- Relationship Validator:
  - Ensures dialogues align with relationship type.
  - Example: Rivals should not behave like close friends unless story justifies it.

- Duration Controller:
  - Controls total word count and scene pacing.
  - Estimates runtime based on dialogue length and narration speed.
  - Adjusts scene count to match ~5-minute target.
#### 4. Rendering & Output Layer

- Script + Scene Output:
  - Final episode script
  - Scene breakdown
  - Visual prompts
  - Voiceover lines

- Video Assembly Module:
  - Combine generated visuals + character voices + background music
  - Align timing with script pacing
  - Export final video (9:16 or 16:9)

- Output Package:
  - Final video file (optional)
  - Production-ready assets (script + storyboard + prompts)

#### 5. Iteration & Regeneration System

- Scene-Level Regeneration:
  - User can regenerate only a specific scene.
  - User can regenerate only dialogues or visuals.
  - Other scenes remain unchanged.

- User can:
  - Edit story prompt
  - Swap characters
  - Modify tone
  - Regenerate specific scenes only

- System preserves:
  - Character consistency
  - Relationship rules
  - Visual style

### Final Recommendation

This modular architecture ensures character consistency, scalable episode generation, and controlled pacing. 

By separating Series Setup and Episode Processing, the system enables reusable characters and flexible storytelling while maintaining high-quality production output.
