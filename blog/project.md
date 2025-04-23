---
id: 3
title: "🎥 Contextual Video Question Answering System"
date: "April 23, 2025"
excerpt: "Enabling Natural Language Q&A over Videos using Multimodal AI. Built with BLIP, Whisper, FAISS, GPT-4, Docker, and AWS."
image: "https://via.placeholder.com/800x400/ddeeff/0056b3?text=Video+QA+System"
tags: ["AI", "Multimodal", "Video QA", "LLM", "AWS", "Docker", "Python"]
---

# 🎥 Contextual Video Question Answering System

> Enabling Natural Language Q&A over Videos using Multimodal AI

## 🚀 Project Overview

In an era where videos dominate digital content, querying and understanding what's inside them remains a challenge. This project builds a **Contextual Video QA Assistant** that allows users to upload a video and ask natural language questions like:

> *“When did the presenter mention the release date?”*  
> *“What is happening when the dog starts barking?”*

The system uses advanced **AI pipelines for video frame captioning, audio transcription, semantic indexing, and language generation**, all packed in a scalable backend and deployed via **Docker on AWS EC2**.

---

## 🎯 Objective

To build an end-to-end pipeline that allows:
- **Semantic search** over video content
- **Multimodal fusion** of frame captions and audio transcripts
- **Natural language answering** using LLMs (GPT-4)
- **Real-time querying** for educational, entertainment, and surveillance use cases

---

## 🧠 Technical Architecture

### 1. 🧩 Model Selection and Problem Framing

Key AI components:
- **BLIP (Bootstrapped Language-Image Pretraining)** for frame captioning
- **Whisper (OpenAI)** for audio transcription with timestamps
- **Sentence Transformers** for embedding video context and user queries
- **FAISS** for vector similarity search
- **GPT-4** for answer generation from retrieved context

---

### 2. 🛠 Core Logic Implementation – `video_processor.py`

#### ✅ Frame Selection and Captioning
- Used `cv2` to extract frames.
- Scene change detection via **SSIM** to avoid redundancy.
- Selected frames passed to **BLIP**, generating textual captions with timestamps.

#### ✅ Audio Extraction and Transcription
- Used `moviepy` to extract audio from video.
- Transcribed audio with Whisper via OpenAI API, with sentence-level timestamps.

#### ✅ Data Fusion and Indexing
- Merged caption and transcript segments based on timestamp overlaps.
- Combined text passed through Sentence Transformers to generate embeddings.
- Indexed via **FAISS** for fast semantic search.

#### ✅ Semantic QA Pipeline
- User query → embedded using same Sentence Transformer.
- Top-k semantically closest segments retrieved from FAISS.
- Constructed a contextual prompt with retrieved captions + transcripts.
- Sent to **GPT-4** to generate final answer.

---

### 3. ⚙️ Optimization & Resource Management

- Used `torch.no_grad()` to prevent unnecessary gradient tracking.
- Handled GPU acceleration (`CUDA`, `MPS`, or CPU fallback).
- Models loaded globally on startup for latency reduction.
- SSIM & FPS-based frame sampling to avoid processing every frame.

---

### 4. 🌐 Backend Integration – `app.py`

- Flask app with routes to:
  - Upload and process video
  - Ask questions against specific job/session IDs
- Global dictionary `jobs` stores processed segments, FAISS indices, and metadata
- Clean error handling for:
  - GPU availability
  - Memory constraints
  - API failures

---

### 5. 📦 Dockerization and Cloud Deployment

#### 🐳 Dockerfile
- Based on `python:3.9-slim`
- Installed dependencies: `ffmpeg`, `torch`, `transformers`, etc.
- Used `gunicorn` to run the Flask app

#### ☁️ AWS EC2 Deployment
- Deployed on **GPU-enabled EC2 (g4dn)** instance
- Configured environment variables like `OPENAI_API_KEY` using `.env`
- Ensured compatibility with CUDA drivers and optimized startup

---

## 📊 Example Use Case

**Video Uploaded:** A product demo  
**Question:** "When was the feature ‘Smart Sync’ introduced?"  
**Answer:**  
> [00:43 - 00:50]  
> Caption: Presenter showing UI.  
> Transcript: “This is where we added Smart Sync to let you collaborate seamlessly…”

---


---

## 🌟 Highlights

- ✅ Combines vision, audio, language, and retrieval in one cohesive pipeline
- ✅ Uses SOTA models (BLIP, Whisper, GPT-4, FAISS)
- ✅ Cloud-deployable via Docker on EC2
- ✅ Extensible to streaming, surveillance, education, or UX research tools

---

## 🧠 Future Work

- Add support for **live video streams**
- Enable **multilingual transcription and captioning**
- Build a **frontend** for more intuitive QA experience
- Explore **local inference** using quantized open-weight models

---

## 👨‍💻 Author

**Devansh Kumar**  
AI Research Engineer | NLP & Multimodal Systems  
[LinkedIn](#) • [GitHub](#) • [devansh.kumar.kd@gmail.com](mailto:devansh.kumar.kd@gmail.com)

---

*Built with ❤️ to make video content searchable and useful through AI.*
