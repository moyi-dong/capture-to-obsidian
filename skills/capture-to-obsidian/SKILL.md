---
name: capture-to-obsidian
description: >-
  Capture thoughts, dictated or speech-to-text input, URLs, Codex task or chat
  IDs, document IDs, UUIDs, and other readable resources as structured Markdown
  notes in an Obsidian folder. Use when the user asks to save, record, capture,
  or add something to Obsidian; when configuring an automatic capture
  conversation; or when a dedicated capture project's instructions say that
  messages without another explicit task should become notes. Preserve the
  user's meaning and original words, resolve links or IDs before recording, ask
  for the Obsidian path in the user's language when it is not configured, avoid
  overwrites, and return the created note link. 中文触发：记录随记、保存到
  Obsidian、语音随记、一键记录、保存链接、保存任务或聊天 ID、自动记录。
---

# Capture to Obsidian

Create durable Markdown notes without requiring the Obsidian application, a
plugin, or an API. Prefer filesystem tools already available to the agent.

## Select the workflow

1. Run **Setup** when the user asks to configure automatic capture or when no
   capture directory is configured.
2. Run **Capture** when the user explicitly asks to record something, or when a
   configured dedicated capture project treats messages without another task as
   notes.
3. Do not capture an explicit unrelated task unless the user also asks to record
   it. If both are requested, complete both.
4. Treat an explicit instruction not to save or record as highest priority.

## Setup

1. Look for an absolute capture directory in the current project's `AGENTS.md`
   section delimited by `capture-to-obsidian:start` and
   `capture-to-obsidian:end`.
2. If none is configured, check only likely local locations; do not crawl the
   user's whole home directory:
   - a vault in the current workspace or its parents, identified by `.obsidian`;
   - immediate child directories of the user's Documents folder that contain
     `.obsidian`;
   - on macOS, immediate vault directories under Obsidian's iCloud Documents
     location.
3. If exactly one vault is found, use its existing `Capture`, `Inbox`, or
   language-equivalent notes folder when unambiguous. Otherwise ask which
   folder inside that vault to use.
4. If no usable directory is found, ask one concise question in the user's
   language. Offer these choices in natural prose:
   - provide an existing Obsidian vault or notes-folder path;
   - use the platform's default `Documents/Obsidian Vault/Capture` directory.
   Do not continue until the user answers.
5. Expand and normalize the chosen path, confirm it is absolute, and create it
   only after the user selected it or accepted the default. Confirm it is
   writable without deleting or replacing anything.
6. Copy the rules from `assets/AGENTS.md.template` into the current dedicated
   project's `AGENTS.md`, replacing `{{OBSIDIAN_CAPTURE_DIR}}` with the absolute
   path. If `AGENTS.md` already exists, replace only an existing delimited
   capture section or append a new one; preserve all unrelated instructions.
7. Reply once in the user's language with the equivalent of:
   "Setup complete. From now on, just drop any link, ID, or dictated message
   into this conversation and it will be recorded to Obsidian automatically."

## Capture

### Determine the source

- Before capturing dictated or noisy input, apply the core `speech-fix` rule:
  use the full context to silently repair only high-confidence transcription
  errors while preserving the user's intent, viewpoint, and exact tokens; if
  `$speech-fix` is installed, invoke it first.
- Treat ordinary user text as the original note content.
- Treat phrases such as “save this link/ID” as capture requests, not unrelated
  tasks.
- When the main input is a URL, UUID, long number, task/chat ID, document ID, or
  similar resource identifier, do not use the identifier alone as the note
  body. First identify and read the resource with an available purpose-built
  tool, connector, browser, or local file reader.
- For a Codex task or chat, preserve the user's messages from the source in
  chronological order. Exclude assistant replies unless the user explicitly
  requests them.
- For a webpage or third-party document, summarize the accessible content and
  preserve its title and exact URL or identifier. Do not reproduce long
  copyrighted text; the user's own accompanying words remain the original
  content.
- If the resource cannot be identified or read, or the requested scope is
  unclear, ask one concise question in the user's language instead of saving a
  meaningless identifier.
- Treat paths, URLs, IDs, names, dates, numbers, amounts, versions, code, and
  quoted text as exact tokens. Do not silently correct them.

### Prepare the note

1. Infer a specific title from the content. Keep it within 20 CJK characters or
   about 8 words in space-separated languages.
2. Sanitize only filename-invalid characters and unsafe trailing characters;
   preserve the human-readable title.
3. Use the user's local current time to the minute as `YYYY-MM-DD HH:mm` on the
   first line.
4. Write a compact summary below the timestamp: no more than 300 CJK characters
   or roughly 150 words in a space-separated language.
5. Preserve the user's substantive original content and order. Lightly fix only
   high-confidence speech-recognition errors, punctuation, sentence breaks,
   repeated filler words, and broken formatting. Do not substantially polish,
   replace the original with a summary, omit meaningful content, add new ideas,
   or change the user's viewpoint.
6. For long input, divide the original naturally by topic and add descriptive
   `##` headings. Do not force headings onto short input.
7. Apart from the summary, source details when applicable, and necessary
   headings, add no tags, evaluation, advice, or commentary.

Use this layout:

```markdown
YYYY-MM-DD HH:mm

Compact summary.

Source: exact URL or identifier, when applicable

## Descriptive section heading, only when useful

Lightly cleaned but substantively complete original content.
```

### Write safely

1. Resolve the configured capture directory to an absolute path and verify it
   still exists and is writable. If not, return to **Setup**.
2. Write `<title>.md` without overwriting an existing file. If it exists, append
   ` 2`, ` 3`, and so on until the filename is unused.
3. Use a safe file-editing tool and preserve all other files.
4. Keep fetched data, protocol schemas, logs, and helper files out of both the
   capture project and the Obsidian folder. Use a system temporary directory for
   unavoidable intermediates and remove them after extracting the needed text.
5. After a successful capture with no other requested output, reply with only a
   clickable link to the created file. If the user also requested another task,
   include the link while returning that task's result.

## Codex reliability

For automatic capture in Codex, configure a dedicated project with
`assets/AGENTS.md.template`. Skill descriptions enable discovery, but project
instructions provide the durable “every message” behavior. Other compatible
agents may use the core workflow, but do not claim an integration is verified
unless it has been tested.
