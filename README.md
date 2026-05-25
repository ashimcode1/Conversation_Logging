# ConversationBot — with Logging & Validation 🤖

A production-grade class-based conversation logger built in Python. Stores, loads, and displays timestamped chat history — with full logging, input validation, and error handling.

---

## What's new in this version

- Five-level logging (DEBUG → CRITICAL) saved to `bot.log`
- Input validation — invalid roles and empty messages rejected immediately
- Graceful error handling — no silent failures
- Defensive programming — bad data never reaches conversation history

---

## How to run

```bash
python conversation_bot_logger.py
```

**Example output:**
```
2026-05-25 13:03:32 | WARNING | history.json not found — starting fresh
2026-05-25 13:03:32 | DEBUG   | Message added — role: user, chars: 11
2026-05-25 13:03:32 | DEBUG   | Message added — role: assistant, chars: 30
USER: What is AI? | 2026-05-25 13:03:32
ASSISTANT: AI is artificial intelligence. | 2026-05-25 13:03:32
2026-05-25 13:03:32 | INFO    | Saved 2 messages to history.json
2026-05-25 13:03:32 | INFO    | Summary — model: claude-sonnet-4-6, messages: 2, chars: 41
Model       : claude-sonnet-4-6
Messages    : 2
Total chars : 41
```

---

## How it works

```python
bot = ConversationBot()
bot.load()                                          # load history from disk
bot.add("user", "What is AI?")                     # validated message
bot.add("assistant", "AI is artificial intelligence.")
bot.show()                                          # print conversation
bot.save()                                          # save to disk
bot.summary()                                       # print stats
```

---

## Input validation

Invalid inputs are rejected immediately with ERROR logs:

```python
bot.add("accountant", "hello")   # ❌ ValueError: Invalid role 'accountant'
bot.add("user", "   ")           # ❌ ValueError: Message content cannot be empty
```

```
2026-05-25 13:08:47 | ERROR | Invalid role 'accountant' — must be user or assistant
2026-05-25 13:08:47 | ERROR | Empty message content rejected
```

---

## Logging levels used

| Level | When it fires |
|---|---|
| `DEBUG` | Message successfully added (chars count) |
| `INFO` | Bot initialized, file loaded, file saved, summary |
| `WARNING` | History file missing or empty — not a crash |
| `ERROR` | Invalid role, empty content, failed to save |

---

## Log file

Every run appends to `bot.log` permanently:

```
2026-05-25 13:03:32 | INFO    | ConversationBot initialized
2026-05-25 13:03:32 | WARNING | history.json not found — starting fresh
2026-05-25 13:03:32 | DEBUG   | Message added — role: user, chars: 11
2026-05-25 13:03:32 | INFO    | Saved 2 messages to history.json
```

---

## Methods

| Method | What it does |
|---|---|
| `load()` | Reads `history.json`. Logs WARNING if missing or empty |
| `add(role, content)` | Validates input, appends message with timestamp |
| `show()` | Prints each message with role and timestamp |
| `save()` | Saves to disk. Catches and logs any write errors |
| `summary()` | Prints model, message count, total characters |

---

## Concepts demonstrated

- `try / except` with specific error types
- `raise ValueError` — defensive programming
- Five-level logging with `logging` module
- Dual handlers — terminal output + file output
- `logger.getLogger(__name__)` pattern
- Input validation before data storage

---

## Requirements

Python 3.11+ · No external libraries needed.

---

## Author

Built as part of an AI Engineering learning journey — Module 01, Lesson 5.
