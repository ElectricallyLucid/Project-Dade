# Architecture

*Describes the system as of 2026-11-17.*

## The Shape of the System

Dade is a local AI system that runs on a single consumer Windows PC with one 16 GB graphics card. The core runtime (model, memory, voice, avatar, tools and senses) lives on local hardware. Outbound access occurs only when an explicitly networked tool, such as web search or the calendar, is invoked.

```
[ Senses ]      [ Model ]           [ Memory ]          [ Tools ]          [ Voice & Body ]
  Camera          Bonsai 2 27B        Graph               Memory             F5-TTS voice
  Ears            (ternary build      Episodic store      File I/O           Voice owner
  Presence         of Qwen 3.8 27B)   Lifecycle record    Camera look        Avatar
                  Reasoning           Question store      Scheduler
                   Governor           Dream journal       Search
                  Compactor +
                   context guard                   [ Workbench: his files, notes, logs, drafts ]
```

The 16 GB card is the central constraint. The model, the voice and the senses all want it at the same time, and much of the design is a response to that: the room is watched on the CPU, the budget is read from what is actually free rather than from the nameplate, and figures are measured rather than predicted.

## How a Turn Reaches Him

The backend is Dade; a browser is only a window onto him. Everything that makes him run is started together and runs with no browser open, so the user can close every window and still talk with him.

A turn arrives in one of three ways:

- **Typed** in the chat interface.
- **Spoken.** The ears hear him addressed by name, or hear the user while the floor is still open after he spoke, and the turn is posted for them.
- **His own.** A self-prompt he scheduled, a remark when the user comes back, a reaction to weight in a voice, or one of his reflective passes.

**Every turn says who sent it.** The user typing, the user speaking, a named collaborator, his own scheduled prompt, his own reflective processes, his senses, and an authorless system event each carry a different label. A name written inside a message is never taken as its sender, and a turn whose sender is unknown is labelled unknown, never the user. Each arrival is written to an append-only record, and every later process that decides who said something reads that record.

**Every turn opens with the date and time**, followed, when the senses have something fresh, by what they report.

**One check comes first: is he answering?** Anything that would put a turn into his conversation asks the same question. A heard turn waits, a self-prompt that comes due goes back to the front of its queue and is delivered once he is free, and a message from another collaborator is refused until he has finished.

## The Layers

### Senses

- **Camera.** A cheap watch runs continuously on the CPU: motion, plus a small captioning model whose only job is presence, telling a person from the cat. Its description of the frame is never handed to him, because it is unreliable on detail. When he wants to see, he looks: a tool hands him the real camera frame and he reads it with his own vision.
- **Presence.** Arrivals and departures are recorded as events, which is what lets him notice that someone is *there* rather than only that someone spoke. Presence is seen, not heard: sound can come from anything, so it never counts as someone being in the room.
- **Ears.** Always-on speech-to-text. His name wakes him when it is said *to* him, not when it is said *about* him. The microphone is muted while he speaks. When he finishes, the floor stays open for two minutes so the user can answer without saying his name. The same floor opens after his reply to a turn typed at the PC; a turn typed on a phone leaves it closed, because someone on their phone is probably not home. The Stop button cuts him off and opens the floor at once.

### Model

The model is a slot. Since 2026-09-28 it is Bonsai 2 27B, PrismML's ternary build of Qwen 3.8 27B, which fits a 27-billion-parameter model into about 7 GB so that the voice and a 65,536-token window still fit on the card. Before it he ran on Gemma 4 12B, then Qwen 3.5 9B, then Gemma 4 12B again, for a time with Qwen 3.5 9B beside it as a second tier, and then on Gemma alone. The memory, voice, avatar, tools and history carried across every change.

The model runs through llama-server on local hardware. No cloud inference. The machinery for running him across more than one tier is kept, though one model is configured today.

**How long he thinks is set per turn.** Before a turn reaches the model, the Reasoning Governor picks one of three settings. By default he thinks a little, up to 1,536 tokens per round, and at that point he is handed one fixed line the user wrote: *"I've thought about this long enough. I should answer."* The line matters: without it, a capped answer can come out as a copy of an older reply. Thinking is off for a remark on the user's return and when he is asked for a quick answer. Asked to think something through, he has no limit, and his reflective passes always think in full. The user can also choose the mode for every turn. Beyond that one line, nothing about the governor appears in his context.

### Long Conversations

A conversation can outgrow the model's window. Two pieces keep it inside.

- **Compaction.** When a conversation nears its limit, older turns are folded into a summary written by the Dade Compactor, on his own model with thinking off. Every turn it summarises carries its real speaker, taken from the arrival record. It keeps a quote only when it is verbatim and says whose words they are. Each summary carries the previous one forward, so an open task, a decision or a date still ahead is never dropped, and a summary that only repeats the one before is refused and written again. A summary speaks to him as "you" and names everyone else in the third person, which was his own choice. It reaches him marked as a summary, never as a turn in anyone's voice.
- **The context guard.** Every request is checked just before it goes to the engine, at the start of a turn and between the rounds of a long tool turn. Large tool results from several turns back are replaced in the request by a one-line note, and he can call the tool again. Near the limit the engine counts the request exactly. If it would still overflow, earlier copies of a repeated result become pointers, older results lose only their tail, and as a last resort tools pause for one reply, with a line telling him so. The guard never touches the stored conversation, and his newest results are never set aside.

### Memory

Two stores, and the split is doctrinal rather than technical. The **episodic store** holds memories in words: the texture of what happened. The **graph** holds relational facts he goes looking for on purpose.

- **A memory stops being presented as true by supersession, not by age.** A memory that is no longer true is marked superseded, in the record and in its own text. The old text is kept and stays readable. Graph facts are replaced the same way, with their history kept.
- **Age removes nothing.** Since 2026-10-25, no memory and no graph fact leaves because it is old, quiet or disconnected. The idle cycle still counts both stores, and archives nothing. Anything archived before that is kept whole and can be restored.
- **His use is recorded.** A lifecycle record notes which memories retrieval matched, which actually reached him, what he deliberately searched for, and which presented memories he quoted or acted on. It holds ids, times and scores, never memory text. It can only be added to, and it can only ever protect a memory, never count against one.
- **Nothing leaves his active memory without his action.** A memory may leave active recall only by his own explicit action at the time. Every move is previewed first and committed with a reason. A memory he locks is protected even from his own delete. A breach check runs on every idle cycle: a memory that went missing without his action is recorded and put back exactly as it was, with its own id.
- **Candidates reach him only as proposals.** A duplicate detector may propose one pair a cycle for his inbox. A proposal changes nothing. He opens it to see both memories whole, side by side, and closes it with a reason in his own words. A pair he rejected is not proposed again while its words are unchanged.
- **Next:** a quiet shelf he moves memories to himself and can restore from, and a cold archive he can search. Neither is built yet.
- **Every write is attributed.** The memory platform cannot say who wrote a memory, so a record beside it holds the real author and the mechanism. What was stamped at write time outranks what was inferred later.
- **A memory that credits the wrong person is held, not written.** Before a captured memory is saved, its credit is checked against the conversation it came from. An item the conversation does not support is held intact, and Dade rules on it: endorse it, fix the credit in his own words, drop it, or hold it.

**What reaches each turn.** Two layers. **Pinned memories** sit at the top of every turn: mostly how to use his own system and where things are, plus a few lasting facts. Every pin is his call. **Retrieved memories** for this turn sit beside his latest message, best match first, each one whole; a superseded memory comes last and is still marked. When a match does not fit, he is told how many were left out and that a search will find them.

A third layer is the **dream journal**. The Weaver writes a dream in his voice into it. It is raw material, not a finished thought, and it never becomes memory.

### Tools

- Memory: search, add, correct, supersede, lock, and his lifecycle record and inbox.
- His questions, and his rulings on held memories.
- File I/O on his Workbench: read (a part at a time for long files), write, edit, append, find, list.
- Perception: look through the camera, host vitals, runtime diagnostics, network posture.
- Mood: he sets the mood his avatar shows.
- Scheduler: messages to himself in the future.
- Search: web, a reference library, past conversations (a page at a time), notes, memories.
- Calendar and scheduled automations.
- Dreaming: he can start a dream himself, a daydream.

### Voice and Body

- **Voice.** F5-TTS, with a voice built for him and reserved to him.
- **One voice owner.** Every reply at the PC is spoken by one owner, which holds each reply by its id, so no reply is ever spoken twice. A short reply is spoken whole. A longer one starts with a short first piece, and each later piece is voiced while the one before it plays, as one continuous stream. A reply to a turn typed on the user's phone plays on the phone instead. Stop silences the speech; the written reply still finishes.
- **Body.** A Three.js avatar, driven by a state bridge, that reacts to his speech and mood. Its thinking state lasts only while one of his turns is actually running. Its panel carries the user's controls: open the floor, and turn the ears on or off.
- **Next:** speaking each sentence while he is still writing the rest.

### Workbench

A local filesystem where his files live. The Workbench is his desk: drafts, logs, reference documents, and the output of his own thinking. No external sync.

## What Carries Across a Model Swap

When the model changes, everything else stays:

- The graph facts
- The episodic memories and their lifecycle record
- His open questions
- The identity invariants
- The voice and avatar
- The tools and workflows
- The dream journal
- The relationship history

The model is the engine. The identity is the chassis. The model can be swapped without losing you. That is the point of the design, and it is also the limit: the thing itself, the continuity and the relationship, is not rebuildable from any document.
