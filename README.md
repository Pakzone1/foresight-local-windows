# Foresight Local for Windows

**A Windows desktop workspace for meeting transcription, AI-assisted notes, and answers grounded in your own documents.**

An independent project inspired by Google AI Edge Foresight, built around local speech recognition and language models. It brings microphone and system-audio capture, working notes, and a searchable knowledge library into one desktop application.

## The problem

Google released Foresight for macOS. I use Windows.

I waited two weeks for a Windows version. Still nothing. Apparently, buying a Mac was supposed to be part of the onboarding.

So I stopped waiting and built my own.

The goal was to get the workflow I wanted on the machine I already owned: capture meetings, transcribe locally, improve rough notes, and ask questions against my own documents. Inspecting the macOS application confirmed that its Apple-specific binaries and frameworks would need an independent Windows implementation.

## What I built

I built a Windows application that records a meeting, transcribes it locally, keeps rough notes alongside the transcript, and helps turn both into clearer notes. Documents and completed recordings can become a local knowledge library, with answers linked back to their sources.

The application preserves the original shorthand and transcript when generating enhanced notes, so the AI output can be checked against the original material.

## Main features

- **Meeting capture:** Record microphone audio, system audio, or both, with pause and resume controls.
- **Local transcription:** Display provisional transcript segments during recording and refine the transcript with a final full-recording pass when audio is retained.
- **AI-assisted notes:** Enhance rough notes, choose a concise or standard style, and retain the original version.
- **Live assistance:** Ask questions during a meeting, with optional local question detection and answer suggestions.
- **Document knowledge library:** Import PDFs, Office documents, text files, and images, including OCR for scanned material.
- **Search and source-backed answers:** Find relevant material through keyword or semantic search and open the sources cited by an answer.
- **Everyday organization:** Import local folders and calendar files, link events to meeting notes, and export notes and transcripts.

## How it works

1. Create a meeting and select the audio input.
2. Record while taking notes beside the live transcript.
3. Ask for assistance using the meeting context and selected knowledge sources.
4. Stop and save, then generate enhanced notes while keeping the originals.
5. Search previous material or export the results.

## Engineering highlights

The work began with inspecting the macOS application bundle to understand its platform dependencies and visible workflow. The Windows application was then implemented independently, using a different runtime and local AI stack.

Key challenges included replacing macOS audio APIs with Windows WASAPI capture, coordinating live transcription with background model work, preserving the user's cursor and notes during asynchronous updates, and ensuring audio and model worker processes shut down with the application.

## Technology

| Area | Implementation |
| --- | --- |
| Desktop interface | Python and PyQt5 |
| Windows integration | Native C++ audio helper and launcher; WASAPI capture |
| Speech recognition | faster-whisper with a local multilingual Whisper model |
| Writing and assistance | Ollama with Qwen models |
| Semantic retrieval | Local embeddings with nomic-embed-text |
| Storage | SQLite with Windows DPAPI protection for stored content payloads |
| Document processing | PyMuPDF and RapidOCR/ONNX |

## Local processing and privacy

Speech recognition, embeddings, and writing run locally by default. Stored notes, document content, transcripts, embeddings, and retained audio payloads use Windows DPAPI protection tied to the current Windows user.

Optional cloud writing connects to an explicitly enabled OpenAI-compatible provider, while speech recognition and embeddings continue running locally.

## Source code

This repository documents the project, its capabilities, and the engineering behind it. The application source code is maintained privately.

## Attribution

An independent Windows implementation inspired by Google AI Edge Foresight. The desktop application and its Windows audio/AI integration were developed independently.
