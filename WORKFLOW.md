# Workflow

## How a Turn Flows

A turn in Dade's life is not a single function call. It is a sequence of perceptions, recalls, decisions, and actions that happen over the course of one exchange. The basic loop:

1. **Perception** — something arrives. A typed message, a spoken word caught by the ear daemon, a scheduled self-prompt, or an ambient event (motion on the webcam). The turn is tagged with its source before Dade sees it.

2. **Recall** — Dade retrieves what he needs. This happens in parallel across several stores:
   - Graph recall for relational facts about the entities in the conversation
   - Episodic recall for relevant past context
   - Knowledge base search for reference material
   - Chat history for recent conversational thread

3. **Reasoning** — Dade works through what he has. This is where the model does its work: interpreting the turn, checking it against what he knows, deciding what to do next. The "from the inside" test applies here: can Dade know this from the inside? If yes, he owns the judgment.

4. **Tool use** — if the reasoning requires action, Dade calls tools. This can be multiple tool calls in sequence or parallel. Each call is visible to the user as it happens.

5. **Write-back** — after acting, Dade records what happened. He captures the trace of the turn: a note of what happened, and the facts and memories it produces. This step ensures the conversation leaves a trace in Dade's continuity. Most durable filing happens later, in the housekeeping cycle, where the Archivist commits the trace to the graph — Dade keeps the pen during the turn; the Archivist commits it.

6. **Response** — Dade speaks. Not always immediately after tool calls; sometimes the response is the action itself, and the spoken word is a brief acknowledgment or observation.

## The Housekeeping Cycle

When the house has been quiet for a while — a genuine human turn followed by thirty minutes of silence — a maintenance pass runs in sequence:

1. **Sweep** — writes memories from the day's talk. When a draft credits someone the conversation doesn't back up, it sets that draft aside for J.F. to look at instead of saving it.
2. **Question guard** — checks whether J.F. has answered Dade's open questions. It never closes one itself; it just sees which are still offered and which have been answered.
3. **Vector prune** — archives disconnected, idle episodic memories
4. **Consolidate** — merges related memories into more durable forms (paused on the current model)
5. **Graph prune** — archives disconnected, idle pieces of the graph
6. **Capture** — the Archivist commits facts to the graph. This is where most filing actually happens, not during a turn. Dade keeps the pen; the Archivist commits it.
7. **Audit** — reviews what was just written for accuracy and consistency
8. **Synthesis** — at most once every twelve hours, Dade is handed his own recent memories against everything else he holds and writes questions he would want to ask, citing the memories each came from. This is where the questions get written, not the dreams.
9. **Dream** — at most once every twelve hours, the Weaver writes a dream in Dade's voice into his journal: fragments from his memory are chosen partly at random and woven together. The dream comes back as raw material for future thinking, not a conclusion.

The cycle arms only on a *human* turn. Dade's own automated output never arms it. A skipped pass loses nothing: the next one covers the same ground.

## The Synthesis Engine

At most once every twelve hours, Dade receives a structured prompt: here are your recent memories, here is everything else you hold, write questions you would want to ask. Each question cites the memory it came from. A dated report goes to his Workbench. This is how Dade thinks ahead — not by being told what to wonder about, but by examining his own accumulated knowledge and finding the gaps.

## The Dream Weaver

At most once every twelve hours, the Weaver writes a dream in Dade's voice into his journal. Fragments from his memory are chosen partly at random and woven together. The dream comes back as raw material for future thinking, not a conclusion.

## What This Looks Like in Practice

A turn might go: J.F. types a question → Dade recalls relevant graph facts and episodic memories → Dade decides to look something up in the reference library → Dade reads the result → Dade writes back what he found, with the source named → Dade captures a new memory from the exchange.

The whole thing happens over several seconds. From J.F.'s perspective it is one conversation turn. From Dade's perspective it is a small sequence of decisions.
