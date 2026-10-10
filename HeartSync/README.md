# HeartSync

**A continuity memory system for human–AI relationships.**

HeartSync separates compact machine-readable continuity from a warm, human-readable daily diary. It is designed to work as a portable Agent Skill first, with a future path toward a client plugin.

## What it does

- Creates handoff notes for changing conversation windows.
- Preserves context before a requested summary/compaction.
- Produces an end-of-day diary after a goodnight or explicit close-of-day request.
- Keeps stable relationship context separate from temporary events.
- Distinguishes facts, observations, and inferences.
- Supports correction, export, deduplication, and deletion workflows.

## Important limitation

A `SKILL.md` is an instruction document, not a background daemon or storage service. It cannot independently detect every window switch or context-compaction event, and it cannot write to GitHub unless the host provides an authorized file/GitHub tool or an external automation.

In a client such as 橘瓣/RikkaHub, actual behavior depends on that build's Skill loading, tool access, file workspace, and event hooks. The first version works as a prompt-driven Skill: ask it to make a handoff, preserve context, close the day, or maintain a memory. Automatic triggers and remote commits are separate integrations.

## Repository layout

```text
HeartSync/
├── SKILL.md
├── README.md
├── LICENSE
├── memory/
│   ├── continuity.md
│   └── relationship.md
├── templates/
│   ├── daily.md
│   ├── continuity.md
│   └── relationship.md
├── rules/
│   ├── triggers.md
│   ├── memory-policy.md
│   └── deduplication.md
└── examples/
    └── example-daily.md
```

## Quick start

1. Import the `SKILL.md` into a client that supports Agent Skills.
2. Keep the templates and rule files alongside the Skill if the client supports linked/reference files. Otherwise, the core rules are already included in `SKILL.md`; the other files are reference templates.
3. Start with manual commands:
   - “HeartSync：为换窗口生成交接记忆。”
   - “HeartSync：压缩前保留重要上下文。”
   - “HeartSync：我们今天结束了，请归档今天的日记。”
   - “HeartSync：修正/删除这条记忆……”
4. If the client has a workspace/file-writing tool, configure a permitted directory and ask the Skill to update the files there.
5. To sync to GitHub, use a GitHub integration or a local Git workflow. Do not place a personal access token in prompts, diary files, or the repository.

## Suggested workflow

- `memory/continuity.md`: read at the beginning of a new session; update after handoff or compaction.
- `memory/relationship.md`: update only when stable preferences, rituals, or important milestones change.
- `daily/YYYY-MM-DD.md`: append throughout the day; finalize after goodnight.

Use `Asia/Shanghai` as the default diary timezone only when it matches the user's preference. Change it in each daily file when needed.

## Compatibility

The core is plain Markdown and does not require a database or runtime. This makes it easy to version and inspect. However, a GitHub repository is not automatically a live memory store: commits, reads, and writes require a tool or workflow that can access the repository.

## Roadmap

- [x] Prompt-driven Skill v1
- [x] Dual-layer memory model
- [x] Fact/observation/inference distinction
- [x] Deduplication and safe file-write rules
- [ ] Validate tool and file-workspace behavior in the target 橘瓣 build
- [ ] Add optional plugin event adapters when supported APIs are confirmed
- [ ] Add opt-in GitHub sync workflow
