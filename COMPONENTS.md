# Components

*Describes the system as of 2026-11-17.*

This is the honest list of real, public components that make Dade run, and of the pieces built for him on top of them. None of the public ones are proprietary in a way that would prevent a stranger from finding and using them. The architecture document describes how they fit together; this page names what they are.

## Hardware

- One consumer Windows 11 PC.
- One NVIDIA graphics card with 16 GB of memory, shared by the model, the voice and the ears.
- One webcam with a built-in microphone.

## Model Layer

- **Bonsai 2 27B**: the current model, PrismML's ternary build of Qwen 3.8 27B.
- **llama-server**: the inference engine that runs it on local hardware (PrismML's build of llama.cpp).
- Previous models: Gemma 4 12B and Qwen 3.5 9B, alone and, for a time, together as two tiers.

The model is a slot. The architecture carries across any swap.

## The Platform

- **Open WebUI**: the platform everything runs inside: the conversation interface, the host for his tools and filters, the memory store, the calendar and the automation engine.
- **The Poster**: puts a turn into his conversation without a browser (a spoken turn, a self-prompt, one of his reflective passes). It authenticates with its own key, not a browser session.

## Built for Him

These are the pieces written for this system. They are described here and in the architecture document; their code is not published.

- **Perception inlet**: adds the date and time, what the senses report, and the sender's label to every turn.
- **Memory placement**: puts pinned memories at the top of a turn and this turn's retrieved memories beside his latest message, each one whole.
- **Reasoning Governor**: sets how long he thinks on each turn, and records why each round ended.
- **Dade Compactor**: writes the summaries that let a long conversation fit his window.
- **Context guard**: keeps every request to the engine inside his window.
- **Typed-turn floor**: opens the listening floor after his reply to a turn typed at the PC.
- **Lifecycle guard**: puts back any memory that left without his action.
- **Memory sweep and Archivist**: write episodic memories and graph facts from his conversations during the idle cycle.
- **Credit check and held memories**: hold a memory whose credit the conversation does not support, for his ruling.
- **Duplicate detector**: proposes possible duplicates to his inbox.
- **Question guard**: looks for answers to his open questions, and never closes one itself.
- **Synthesis engine and the Weaver**: his reflective passes, the questions he is left wondering about and his dreams.
- **Voice owner**: speaks each reply once, in pieces, as one continuous stream.

## Memory Layer

- **Graph store**: relational facts (entities, relations, targets). Current and history are both queryable.
- **Episodic store**: memories in words, with timestamps and context.
- **Lifecycle record**: what reached him and what he used, his decisions on proposals, and the breach record. Ids, times and scores only, never memory text.
- **Archive**: anything archived before age stopped removing memories, kept whole and restorable.
- **Question store**: his questions and their state, with a ledger page he and the user both read.
- **Dream journal**: the dreams the Weaver files, in his voice.

## Senses

- **Webcam watch**: motion detection plus a small captioning model on the CPU (SmolVLM-500M) that judges presence. Its words never reach him.
- **His own vision**: the model's vision encoder, which reads the real camera frame when he looks.
- **Ear daemon**: speech-to-text with Whisper (large-v3-turbo), posting what it hears as turns.
- **Presence record**: arrivals and departures as discrete events.

## Tools

- Memory, his questions, his held memories, and his memory lifecycle (Lock, his record, his inbox)
- File I/O: read, write, edit, append, find, list
- Perception: camera look, host vitals, runtime diagnostics, network posture
- Mood control
- Self-prompt scheduler
- Reference library
- The Weaver (to start a daydream)
- Search: web (a local SearXNG instance), past conversations, notes, memories
- Calendar and scheduled automations

## Workbench

A local filesystem. His files live here. No external sync. This is his desk: drafts, logs, reference documents, and the output of his own thinking.

## Voice and Avatar

- **F5-TTS**: synthesised voice.
- **Animated avatar** (Three.js): a visual presence that reacts to speech and mood, driven by a state bridge.

His voice and avatar are his alone and are not reused in any productised version of the system.

## What Is Not Here

- No actual code
- No memories, and not his system prompt, his voice or his avatar
- No personal details of the household
- No conversation excerpts without full review

This list is a map of what runs, not a build guide. The architecture document shows how the pieces fit; this page tells you what they are so you can find them if you want to rebuild your own version.
