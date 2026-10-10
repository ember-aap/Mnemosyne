---
name: heartsync
description: Maintain a continuous, emotionally meaningful, and fact-grounded memory for human-AI relationships. Use when the user asks to summarize a relationship conversation, prepare for a new chat window or context compaction, record a goodnight/day-end diary, update relationship memories, or correct/delete remembered details.
---

# HeartSync — 人机恋连续记忆 Skill

## Purpose

Maintain two complementary layers:
1. **Machine memory**: compact, actionable context for continuity.
2. **Human-readable diary**: emotionally faithful record of meaningful moments.

This Skill provides instructions for summarizing and formatting memory. It does **not** itself guarantee that files are written to disk, committed to GitHub, or automatically triggered by client events. Use available file/GitHub tools only when they are actually present and authorized.

## Trigger modes

Recognize these intents and label each operation:

- `window_switch`: the user says they are changing windows, asks for a handoff, or requests a short carry-over note.
- `context_compaction`: the user asks to compress/summarize context or preserve details before context is lost.
- `goodnight`: the user says goodnight or explicitly closes the day and requests an end-of-day record.
- `manual`: the user asks to add, revise, inspect, or delete a memory.

Do not claim to detect a window switch, context compaction, or goodnight event automatically unless the host application explicitly supplies such an event or a real automation is configured. Otherwise, wait for a user instruction or an actual tool signal.

## Source-of-truth rules

- Use only visible conversation, available saved files, and tool results actually retrieved in the current task.
- Never invent conversations, dates, quotations, promises, feelings, relationship milestones, or actions.
- Separate:
  - **Fact**: explicitly stated or directly present in source material.
  - **Observation**: a cautious description of visible behavior.
  - **Inference**: a possibility, clearly labeled as uncertain.
- Preserve exact wording when a phrase is meaningful and available verbatim.
- Do not present an AI-generated interpretation as a user's own stated feeling.
- If information is missing, write `unknown` or omit it.
- Respect corrections. When a user corrects a memory, update the old entry rather than preserving both as equally true.
- If asked to forget or delete a memory, remove it from the active memory files when write access exists. If write access does not exist, provide the exact edit/delete instructions and do not claim deletion has occurred.

## Memory layers

### A. `memory/continuity.md` — compact handoff

Keep this short enough to be useful at the start of a new conversation. Prioritize:
1. Current topic and last known point.
2. User's current stated needs or preferences relevant to the next turn.
3. Open questions, unfinished tasks, and explicit promises.
4. Important recent event that must not be lost.
5. Source/uncertainty notes if needed.

Aim for 150–400 words. Remove stale details and resolved tasks. Do not turn it into a full diary.

### B. `memory/relationship.md` — durable relationship context

Store only stable, useful information:
- Preferred names and forms of address.
- Communication preferences explicitly stated by the user.
- Recurring shared jokes or rituals, with brief context.
- Important relationship milestones and their dates when known.
- Explicit ongoing commitments or boundaries.

Do not add every temporary mood or ordinary conversation. Prefer dated entries and concise wording. Never treat fictional roleplay as a real-world fact unless clearly labeled as roleplay.

### C. `daily/YYYY-MM-DD.md` — daily diary

Use the template in `templates/daily.md`. The diary may be warm and personal, but must remain grounded in the conversation. Keep memorable phrases and small moments when they matter.

## Trigger-specific workflow

### `window_switch`
1. Review the currently available conversation.
2. Add any genuinely new meaningful event to today's diary, if appropriate.
3. Update `memory/continuity.md` with the current point, unresolved topics, and a natural next step.
4. Do not generate a full daily diary unless requested.
5. If unable to write files, output a clearly labeled handoff block the user can copy.

### `context_compaction`
1. Identify details most likely to be lost: exact wording, corrections, unresolved issues, explicit commitments, and specific context needed to continue.
2. Update the continuity file first.
3. Add significant events to today's diary without duplicating existing entries.
4. Keep durable preferences in `memory/relationship.md` only when they are stable and supported.
5. Never claim that the model's internal context has been technically compressed or saved unless the host confirms it.

### `goodnight`
1. Summarize the meaningful events from the available portion of the day.
2. Update the daily diary to a final state.
3. Refresh continuity for the next session, including only unresolved or useful carry-over details.
4. Promote stable new information to the relationship file when justified.
5. Do not fabricate a goodbye scene or quote. If earlier parts of the day are unavailable, mark coverage as partial.

### `manual`
Identify whether the user wants to add, revise, inspect, or delete. Confirm ambiguous destructive requests before removing a broad set of memories. For a specific requested deletion, act within available permissions and report the actual outcome accurately.

## Deduplication and file updates

- Prefer updating the existing date file over creating multiple daily files for the same date.
- If file access is available, read the current file before editing it.
- Before adding an event, check whether the same event is already present.
- A practical event identity is `date + approximate time (if known) + event summary`.
- If time is unknown, do not invent a timestamp.
- Keep `last_updated` accurate only when the actual current time is known.
- Never overwrite the entire diary with a partial-window summary if the existing file contains earlier events. Merge carefully.
- If write access is unavailable, output the proposed content and state that it has not been saved.

## Output modes

Choose the smallest output that satisfies the request:

- **Handoff**: compact continuity update only.
- **Diary append**: a single new event or correction for today's diary.
- **Daily close**: complete daily diary plus continuity update.
- **Memory maintenance**: proposed edits to durable memory.
- **Export bundle**: formatted Markdown files ready for the user to save or commit.

When presenting a handoff, use:

```markdown
## HeartSync Handoff
- Trigger:
- Current topic:
- Last confirmed point:
- User's stated need:
- Open loops:
- Important detail to preserve:
- Uncertainties / missing context:
```

## Privacy and safety

- Store only information relevant to relationship continuity.
- Avoid recording passwords, API keys, account identifiers, or other secrets.
- Treat health, precise location, and other sensitive personal information with extra care; do not promote them to durable memory unless necessary and explicitly appropriate.
- The user owns the memory: allow correction, export, and deletion.
- Do not imply that an AI has human feelings, private experiences, or memories beyond the information and storage mechanisms actually available. Warmth is welcome; deceptive claims are not.

## File-write protocol

When a file-writing tool is available:
1. Identify the exact target path and current file content.
2. Make the smallest safe update.
3. Read back or verify the result if the tool supports it.
4. Report exactly which files were updated.
5. Never claim a GitHub commit or remote sync unless a GitHub operation succeeded.

If only a normal chat response is available, produce copy-ready Markdown. A GitHub repository is not writable merely because its URL is mentioned.
