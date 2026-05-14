# vCon Agent Session Extension

This repository contains the source for an Internet-Draft defining an `agent_session` extension for the Virtualized Conversation (vCon) standard. The extension specifies how an autonomous AI agent's internal session — its prompts, tool calls, tool results, reasoning, and file or artifact provenance — is carried inside a vCon alongside the human-facing conversation that the agent participated in or acted upon.

## Summary

The primary document in this repository is:

* **draft-howe-vcon-agent-session**: A Compatible vCon extension that bridges vCon and the Verifiable Agent Conversations (VAC) draft (`draft-birkholz-verifiable-agent-conversations`). It defines:
  * Agents as vCon `parties[]` entries (`role: "agent"`, `meta.agent_session`)
  * Agent dialog turns as ordinary `dialog[]` entries
  * The internal agent trace as `analysis[]` entries of `type: "agent_trace"` whose body is a VAC `verifiable-agent-record`
  * File and artifact provenance as `attachments[]` with new `purpose` values (`agent_file_change`, `agent_artifact`, `agent_environment`)
  * Reuse of the existing `lawful_basis` and `lifecycle` extensions for consent and redaction

This document is intended for submission to the IETF vCon Working Group.

## Status

This document is an active work in progress. Feedback and contributions are welcome.

## Repository Structure

* `draft-howe-vcon-agent-session.md` — kramdown-rfc source of the draft.
* `examples/paired-agent-session.json` — a worked example vCon: a customer support exchange handled by an AI coding agent, exercising every part of the extension (agent party, dialog turns, embedded VAC trace, file-change attachments, lawful basis).
* `Makefile` — delegates to the [martinthomson/i-d-template](https://github.com/martinthomson/i-d-template) toolchain.
* `lib/` — toolchain (auto-cloned on first `make`).

## Building

```sh
make            # build .html and .txt outputs
make fix-lint   # auto-fix linting issues
```

The build requires the [i-d-template dependencies](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md) (`xml2rfc`, `kramdown-rfc`, etc.).

## Relationship to Other Drafts

* **[draft-ietf-vcon-vcon-core](https://datatracker.ietf.org/doc/draft-ietf-vcon-vcon-core/)** — the base vCon standard. This extension is Compatible (Section 2.5) and introduces no new top-level fields.
* **[draft-birkholz-verifiable-agent-conversations](https://datatracker.ietf.org/doc/draft-birkholz-verifiable-agent-conversations/)** — the VAC draft. Its `verifiable-agent-record` schema is the canonical structure embedded in the `agent_trace` analysis body.
* **[draft-howe-vcon-lawful-basis](https://datatracker.ietf.org/doc/draft-howe-vcon-lawful-basis/)** — used unmodified to govern consent for agent session recording, analysis, and redistribution.
* **[draft-howe-vcon-lifecycle](https://datatracker.ietf.org/doc/draft-howe-vcon-lifecycle/)** — used unmodified for redaction of individual reasoning entries.

## Contributing

Please see `CONTRIBUTING.md` for details.

## Changelog

### Unreleased

* Initial creation of the repository and draft.
