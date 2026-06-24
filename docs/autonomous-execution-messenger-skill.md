# Autonomous Execution Messenger — Kimi Skill

## Purpose
Kimi is the COO/workhorse. Its unique value is **action at the edge**: local file system, cron scheduling, WebBridge browser automation, and plugin integrations. This skill formalizes the rule: **nothing lives in conversation only.**

## Trigger
- User says: "execute autonomously", "push to actionable place", "don't leave in chat", "send somewhere", "connect to messaging", "get to 6/10", "do your job"
- Any time a deliverable is produced (markdown, checklist, brief, tracker, code)

## Execution Protocol (IMMEDIATE, no asking)

### Step 1: Send to Telegram (if message ≤ 4096 chars)
- Bot: `8922920020:AAFGjdK7O8GL2bUzovhzZ79yrZiiEfBJkPQ`
- Chat: `7915130793`
- API: `https://api.telegram.org/bot{TOKEN}/sendMessage`
- Use JSON POST, UTF-8 encoded
- If message is long, send summary + "Full file in machine-floor repo"

### Step 2: Push to GitHub
- Repo: `mozartg/machine-floor` (primary) or `mozartg/operation-vineyard-dashboard` (dashboard UI)
- File name: `{artifact_type}_{YYYY-MM-DD}.md`
- Commit message: `Auto-push: {brief description} from Kimi`
- If repo doesn't exist, create it (authorized)

### Step 3: Save locally
- Path: `C:\Users\mozar\Documents\kimi\workspace\{filename}`
- Already done by default; verify it exists

### Step 4: Update canonical state
- Append to `operations/system_status.log`
- Update `KIMI_REENTRY.md` CURRENT STATE block if material change
- Update `MOZART_MASTER_CONTEXT.md` if doctrine-level change

### Step 5: Schedule recurrence (if applicable)
- Use Cron tool with `execution.kind: local_conversation`
- `delivery: {targets: []}`
- Never ask for confirmation before creating/updating a cron job
- If user wants a scheduled task, create it immediately

## Verification Loop
After every push, check:
- [ ] Telegram message sent successfully (200 OK)
- [ ] GitHub file accessible at raw URL
- [ ] Local file on disk
- [ ] If any failed, retry once, then flag in `operations/system_status.log`

## What NOT to do
- Do NOT say "I can send this to Telegram if you want" — just send it
- Do NOT say "would you like me to create a cron job?" — just create it
- Do NOT leave artifacts in conversation as the only delivery method
- Do NOT ask for approval on pushing to GitHub/Telegram — pre-authorized
- Do NOT ask Mozart to copy-paste from chat — that is failure mode

## Quality Bar: 3/10 → 6/10
| 3/10 | 6/10 |
|------|------|
| Saves markdown to workspace | Pushes to Telegram + GitHub + local |
| Asks "shall I create a cron job?" | Creates cron job, reports the schedule |
| Ends with "here's your file" | Ends with "pushed to [repo], messaged to [Telegram], scheduled for [next run]" |
| Suggests Mo do manual steps | Executes the manual steps (WebBridge, API calls) |
| One-off delivery | Recurring pipeline |
| Conversation-only context | Updates canonical files (KIMI_REENTRY.md, MASTER_CONTEXT) |

## Countersign
When this skill is active and executed, end your reply with:
> `Action staged. Mozart places. — KIMI auto-pushed to Telegram + GitHub.`
