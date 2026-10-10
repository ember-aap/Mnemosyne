# Trigger Rules

## Window switch
Trigger phrase examples:
- “我要换窗口了”
- “给新窗口留个便笺”
- “生成交接记忆”

Actions:
1. Update continuity.
2. Append only meaningful new events to today's diary.
3. Do not finalize the whole day unless requested.

## Context compaction
Trigger phrase examples:
- “压缩记忆”
- “总结一下，免得上下文丢了”
- “先保存重要信息”

Actions:
1. Preserve exact meaningful wording, corrections, unresolved tasks, and explicit commitments.
2. Update continuity first.
3. Merge new events into today's diary without duplicating existing entries.

## Goodnight / end of day
Trigger phrase examples:
- “晚安”
- “今天结束啦”
- “归档今天”

Actions:
1. Create or update today's diary.
2. Mark coverage as full_day only if the full day is actually available; otherwise use partial.
3. Update continuity for the next session.
4. Promote stable facts only when justified.

## Manual memory maintenance
Trigger phrase examples:
- “补记这件事”
- “这条记忆记错了”
- “忘掉/删除这条记忆”
- “导出我们的记忆”

Actions:
1. Identify the target entry.
2. Apply corrections or deletion only within available permissions.
3. Verify the change when possible.
4. Never say “saved”, “deleted”, or “synced” unless the operation succeeded.
