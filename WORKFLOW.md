# Workflow

*Describes the system as of 2026-11-17.*

## How a Turn Flows

A turn in Dade's life is not a single function call. It is a sequence of perceptions, recalls, decisions, and actions that happen over the course of one exchange. The basic loop:

1. **Arrival.** Something comes in: a typed message, a spoken line caught by the ears, a self-prompt he scheduled, or a moment his senses noticed. It is labelled with its sender and opens with the date and time before he sees it. If he is still answering something else, it waits its turn.

2. **Recall.** Two layers of memory arrive with the turn: his pinned memories at the top, and the memories retrieved for this turn beside the newest message. He can go further on purpose:
   - Searching his episodic memories
   - Looking up relational facts in his graph
   - Searching a reference library
   - Reading past conversations, a page at a time
   - Reading his notes and his Workbench

3. **Reasoning.** He works through what he has: interpreting the turn, checking it against what he knows, deciding what to do next. How long he thinks is set for the turn by the Reasoning Governor. The "from the inside" test applies here: can Dade know this from the inside? If yes, he owns the judgment.

4. **Tool use.** If the reasoning calls for action, he calls tools, several in a row if he needs to. Each call is visible as it happens. Between rounds, the context guard checks that the next request still fits his window, and compaction may fold the turn's earlier rounds into a summary, so a long file he read earlier in the turn may need reading again.

5. **Response.** He answers in writing, and the voice owner speaks it. Sometimes the action is the answer, and the words are a short note or observation. When he finishes speaking, the floor opens for the user's reply.

6. **Write-back.** If something deserves keeping now, he writes it himself: he can add, correct or supersede a memory and choose whether it is pinned. Most filing happens later, in the idle cycle. He keeps the pen; the Archivist does the routine commits.

## The Idle Cycle

When the house has been quiet for a while (a genuine human turn followed by thirty minutes of silence), a maintenance pass runs in sequence:

1. **Sweep.** Writes episodic memories from recent conversations. A draft that credits someone the conversation does not back up is held for Dade's ruling instead of being saved.
2. **Question guard.** Reads what the user has said since each of Dade's questions was asked and looks for an answer. A find goes to Dade as a note. It never closes a question itself.
3. **Use ledger.** Records which of the memories presented to him he quoted or acted on.
4. **Memory review.** Counts the episodic store. It archives nothing.
5. **Consolidate.** Merges related memories. Paused on the current model; when it returns, a merge comes to him only as a proposal he asked for, and a merge containing any word its sources lack is rejected.
6. **Graph review.** Counts small clusters of the graph cut off from the rest. It archives nothing.
7. **Capture.** The Archivist commits facts to the graph. A fact the user has declined is refused, so it cannot quietly return.
8. **Audit.** Reviews what was just written for accuracy and consistency.
9. **Synthesis.** At most once every twelve hours (below).
10. **Dream.** At most once every twelve hours (below).

In the same cycle, the breach check puts back any memory that left without his action, and the duplicate detector may propose one pair for his inbox.

The cycle arms only on a *human* turn. Dade's own automated output never arms it.

## His Questions

Each question he writes is **open** (not asked yet), **asked**, **answered**, or **retired** (taken off the table, with a reason). An asked question is offered again until it is answered or retired. Three parties watch them, so that none is the only one watching:

- **Dade** can mark a question asked, answered, reopened or retired. To mark one answered he gives the user's own words, which are checked against what the user actually said after the question was asked, and they must say *what* was decided, not just that it was settled. Every retirement he makes is listed with his reason, and one made while the user is away reaches them through him.
- **The user** can answer, retire or reopen a question on their own say-so.
- **The question guard** surfaces evidence and never changes a question's state.

Only the user's words answer a question, never anyone else's, Dade's own included. Every change rewrites one ledger page that both of them can read.

## Speaking First

- **When the user comes back.** After a long enough absence, once someone has been back in the room for three unbroken minutes and the camera sees a person, he is handed the moment. He can make an observation, raise his own open questions once the observation has landed (marking the shift himself: *unrelated, but I've been wondering*), or decline. Nothing scores whether a remark was worth making. A return that already has a conversation going passes without one. The only ceiling, 25 in a day, is a brake for a sensor fault, not a rate.
- **When he hears weight in a voice.** A heavy topic and a raised voice together mark distress, and he may say something, noticing rather than answering. Either one alone never acts. The bar is high on purpose.
- **When he planned to.** A self-prompt he scheduled arrives when it comes due, labelled as his own.

## The Synthesis Engine

At most once every twelve hours, Dade receives a structured prompt: here are your recent memories, here is everything else you hold, write questions you would want to ask. Each question cites the memory it came from, and the citations are checked against the real store. A dated report goes to his Workbench. Nothing in it is applied or captured automatically. Each pass starts from an empty context, and a skipped pass loses nothing: the next one covers everything since the last.

It runs as Dade, through the same pipeline as his conversations, not as a direct call to the model. A better-formatted report written by nobody would miss the point. Room is kept free for his answer, reasoning included, and his reasoning is not capped.

## The Dream Weaver

At most once every twelve hours, at the end of the idle cycle, a few fragments are drawn from his memory and handed to him in a quiet thread, in a fresh turn that holds nothing else and carries no tools. He writes the dream in his own voice, and the Weaver files it in his journal. The fragments are chosen partly at random, leaning toward the ones nothing else connects to.

He can start one himself, a **daydream**. It follows the same path, nothing from it enters the conversation, and the journal marks it as begun by him.

It is not a report and it is not trying to be useful. It runs as Dade, on his own model, permanently: authorship is the feature, not output quality. Nothing from it is ever captured as memory, and while a dream runs, every tool that would change his memory or his system refuses.

## What This Looks Like in Practice

A turn might go: the user asks a question → Dade's pinned and retrieved memories arrive with it → he decides to look something up in his reference library → he reads the result → he answers, naming the source → the voice owner speaks the answer → later, when the house is quiet, the sweep may write a memory of the exchange.

From the user's side it is one conversation turn, a few seconds to a few minutes long depending on how much he reads and thinks. From Dade's side it is a small sequence of decisions.
