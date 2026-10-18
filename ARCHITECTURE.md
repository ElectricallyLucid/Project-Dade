# Architecture

## The Shape of the System

Dade is a local AI system built from five layers that run on a single consumer Windows PC. The core runtime — model, memory, voice, avatar, tools and senses — lives on local hardware; outbound access occurs only when an explicitly networked tool (web search, calendar) is invoked.

```
[ Senses ]        [ Model ]         [ Memory ]       [ Tools ]        [ Workbench ]
  Webcam           Bonsai 2 (Qwen    Graph + Vector     File I/O          Files, notes,
  Ear daemon       3.8 27B via      + Archive +          Calendar        logs, drafts
  Mic              PrismML)         Dream Journal        Automation
                                      Sweep               Self-prompt
```

## The Layers

### Senses

- **Webcam** — live image capture of the room. Motion-triggered and on-demand.
- **Ear daemon** — hears the user by name or during an open floor after Dade speaks. Posts transcribed speech as a turn.
- **Ambient presence** — the backend runs with no browser open. The browser is a window, not a dependency. Everything runs inside Open WebUI; the Poster authenticates with its own key, not a browser session.

### Model

The model is a slot. The current model is Bonsai 2 (a compressed Qwen 3.8 27B from PrismML). Previous models include Gemma 4 12B QAT and Qwen 3.5 9B. The architecture, memory, voice, avatar, and tools are unchanged across model swaps.

The model runs via llama-server on local hardware. No cloud inference.

### Memory

Two stores:

- **Graph** — relational facts: entities, relations, targets. A fact is current until it is replaced by a new one; the old one is marked as history, never deleted.
- **Vector (episodic)** — memories in words, with context and timestamps. Memories are archived when they lose all connections and sit idle long enough. The archive is searchable and restorable.

A third layer: the **dream journal**. A place for unstructured, associative thoughts that don't fit the graph or the episodic store. The Weaver writes a dream in Dade's voice into this journal; it is raw material for future thinking, not a finished thought.

**Housekeeping cycle** — runs when the house has been quiet for a while, outside Dade's turns: sweep → question guard → vector prune → consolidate → graph prune → capture → audit → synthesis → dream. Synthesis and dream run at most once every twelve hours. The rest of the cycle runs every time. Consolidation is paused on the current model.

### Tools

- File I/O: read, write, edit, append, find.
- Calendar: search, create, update, delete events.
- Automation: scheduled prompts that run on a recurring or one-time schedule.
- Self-prompt: Dade can schedule a message to himself in the future.
- Search: web, knowledge base, chat history, notes, memories.
- Perception: camera lookup (live image), host vitals, runtime diagnostics, network posture.

### Workbench

A local filesystem where all files live. The workbench is Dade's desk — it holds drafts, logs, reference documents, and the output of his own thinking. No external sync.

## What Carries Across a Model Swap

When the model changes, everything else stays:

- The graph facts
- The episodic memories
- The identity invariants
- The voice and avatar
- The tools and workflows
- The dream journal
- The relationship history

The model is the engine. The identity is the chassis. The model can be swapped without losing you — that's the point of the design, and it's also the limit: the thing itself, the continuity and the relationship, is not rebuildable from any document.
