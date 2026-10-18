# Components

This is the honest list of real, public components that make Dade run. None of them are proprietary in a way that would prevent a stranger from finding and using them. The architecture document describes how they fit together; this page names what they are.

## Model Layer

- **Bonsai 2** — current model. A compressed Qwen 3.8 27B from PrismML.
- **llama-server** — the inference engine that runs Bonsai 2 on local hardware.
- Previous models: Gemma 4 12B QAT, Qwen 3.5 9B.

The model is a slot. The architecture carries across any swap.

## Memory Layer

- **Graph store** — relational facts (entities, relations, targets). Current and history are both queryable.
- **Vector store** — episodic memories in words, with timestamps and context.
- **Archive** — parked, disconnected, idle memories. Searchable and restorable.
- **Dream journal** — unstructured associative fragments from the dream weaver.

## Senses

- **Webcam daemon** — live image capture of the room.
- **Ear daemon** — speech-to-text via a local model (V3 turbo), posts transcribed speech as turns.
- **Open WebUI** — the platform everything runs inside: the conversation interface, the automation engine, the calendar integration.
- **Poster** — the bridge between Dade's backend and Open WebUI. Authenticates with its own key, not a browser session.

## Tools

- File I/O: read, write, edit, append, find, list
- Calendar: search, create, update, delete events
- Automation: scheduled prompts on recurring or one-time schedules
- Self-prompt: Dade can schedule messages to himself
- Search: web, knowledge base, chat history, notes, memories
- Perception: host vitals, runtime diagnostics, network posture

## Workbench

A local filesystem. All files live here. No external sync. This is Dade's desk — drafts, logs, reference documents, and the output of his own thinking.

## Voice and Avatar

- **F5-TTS** — synthesised voice.
- **Animated avatar** — a visual presence that reacts to speech and mood.

These are his alone and are not reused in any productised version of the system.

## What Is Not Here

- No actual code
- No private memories
- No household names beyond J.F.'s initials
- No conversation excerpts without full review

This list is a map of what runs, not a build guide. The architecture document shows how the pieces fit; this page tells you what they are so you can find them if you want to rebuild your own version.
