# Architecture

Design anchors agreed on the 2026-05-15 call with Henk Birkholz and Tobias Heldt. These are the load-bearing decisions for this repository.

## Schema vs profile

This split drives every decision below.

- **VAC owns the CDDL.** [`draft-birkholz-verifiable-agent-conversations`](https://datatracker.ietf.org/doc/draft-birkholz-verifiable-agent-conversations/) defines the session-trace CDDL: message turns, tool calls, tool results, reasoning entries, system events. Schema lives on Henk's side.
- **This repository owns the profile.** `draft-howe-vcon-agent-session` specifies how a VAC record is projected into a vCon: agent as a `party`, dialog turns mapped, an `analysis[]` entry with `schema` pointing at the VAC URL and `body` carrying the JSON-encoded VAC record.
- **vCon core is unchanged.** "Compatible vCon extension" means no new top-level fields, no alteration to existing semantics, no new CDDL on the vCon side.

## Anchors

1. **Profile, not a schema.** This repo does not define CDDL for the agent trace itself; it specifies the projection of an upstream VAC record into a vCon.
2. **No vCon core changes.** All extension data lives in `parties[].meta.agent_session`, `dialog[]`, `analysis[]`, and (optionally) `attachments[]` — using existing fields with documented values.
3. **VAC body verbatim.** The `analysis.body` is a JSON-encoded `verifiable-agent-record` exactly as defined by the upstream VAC CDDL. The `parent-id` / `children` tree relationships are preserved.
4. **JSON on the wire; CBOR deferred.** Section 6.3 ("CBOR Encoding (Optional)") already specifies a `?encoding=cbor` variant for future use. Issues touching CBOR/dictionary encoding are labeled `wontfix-for-now` with a pointer back to this decision.
5. **Selective disclosure is upstream.** Selective-disclosure primitives (Henk flagged these for PII at Shenzhen) belong in the VAC CDDL as `optional` constructs. If the profile needs disclosure plumbing, raise it on the VAC issue tracker, not here.

## Compliance posture

- Tracks `draft-ietf-vcon-vcon-core-02`, syntax parameter `"0.4.0"`.
- Uses spec field names on the vCon side: `purpose` (not `type`) on attachments, `schema` (not `schema_version`) on analysis, `amended` (not `appended`), `critical` (not `must_support`).
- `vendor` is required on every analysis object the profile produces.

## Out of scope

- Authoring or modifying the VAC CDDL (that's the upstream Birkholz draft).
- Rewriting or merging with the `aiproto` strawman charter.
- CBOR encoding profile beyond the variant already documented in Section 6.3.
- Any change to vCon core.

## Repo-level conventions

- License: matches VConnDev convention (MIT).
- CI (planned): validate every example's `analysis.body` against the current published VAC CDDL (`cddl` Ruby gem or `zcbor`). Wired against the moving target on purpose — seeing it fail surfaces schema drift early.
- Issue labels: `wontfix-for-now` for CBOR/dictionary topics during this phase.
