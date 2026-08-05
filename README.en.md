# Capture to Obsidian

Capture dictated thoughts, text, links, and readable IDs as structured Markdown
notes in Obsidian. It writes directly to the filesystem, so it needs no Obsidian
plugin, API, or external service.

The first release is tested for Codex. The skill follows the open Agent Skills
format, but other agent integrations are not yet claimed as supported. If you
want support for another agent, please open an issue with the agent name,
version, install location, and observed behavior.

[中文说明](README.md)

## Install for Codex

```bash
npx skills add moyi-dong/capture-to-obsidian --skill capture-to-obsidian -g -a codex -y
```

For stronger voice-dictation recovery, install the companion skill as well:

```bash
npx skills add moyi-dong/speech-fix --skill speech-fix -g -a codex -y
```

Open a dedicated Codex project and run:

```text
$capture-to-obsidian Set up this conversation for automatic Obsidian capture.
```

The skill looks for a local Obsidian vault. If it cannot find one, it asks for
the path in your language and offers a default under your Documents folder. It
then installs a small, project-scoped `AGENTS.md` rule so future messages are
captured automatically.

After setup, simply send:

- a dictated or typed thought;
- a webpage link;
- a Codex task or chat ID;
- a document ID, UUID, or another readable resource identifier.

No “save this” phrase is required inside the dedicated capture conversation.

## Built-in speech-fix behavior

To prevent homophones, typos, missing words, and mixed-language ASR errors from
being recorded literally, this skill includes the core `speech-fix` rule: infer
intent from full context and silently repair only high-confidence transcription
errors while preserving the user's viewpoint and exact tokens. If the separate
[`speech-fix`](https://github.com/moyi-dong/speech-fix) skill is installed,
Codex invokes it first for a stronger interpretation pass and then writes the
result with `capture-to-obsidian`.

## Note format

Each note contains:

1. local date and time to the minute;
2. a compact summary;
3. the user's substantively complete original words, lightly cleaned only when
   speech recognition or formatting is clearly broken;
4. natural section headings for long notes;
5. the exact source link or ID when applicable.

Filenames are generated from the content and never overwrite existing notes.
After capture, Codex returns only the created note link.

## Safety and boundaries

- Explicit “do not record” instructions always win.
- Unreadable or ambiguous IDs are clarified instead of being saved as
  meaningless text.
- Assistant messages from referenced Codex conversations are excluded unless
  requested.
- Existing `AGENTS.md` content and existing notes are preserved.
- Web content is summarized and linked; long third-party text is not copied.

## Compatibility

- **Codex:** supported and tested.
- **Other Agent Skills clients:** the core format may work, but setup and
  automatic-trigger behavior have not yet been verified. Please use the
  [compatibility issue template](https://github.com/moyi-dong/capture-to-obsidian/issues/new?template=agent-compatibility.yml).

## License

MIT
