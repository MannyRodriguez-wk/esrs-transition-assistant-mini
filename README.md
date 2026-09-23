# ESRS Transition Assistant Mini

Workiva Assistant **single-agent** config for staging ESRS Action Sheet **Column S** only. Customers name their own metrics in T/U. No subagents.

- **Agent name:** ESRS Transition Agent (AI-panel)
- **Repo / product name:** `esrs-transition-assistant-mini`
- **Prompt version:** 11 (`VERSION`)
- **Model:** `claude_46_opus` (temperature 1.0, top_p 1.0, `thinking: true`)

## Files

| File | Use |
|------|-----|
| `system-prompt.md` | Paste-ready system prompt |
| `spec.yaml` | Spec-only AgentConfig |
| `helm/agentconfig.yaml` | Full Helm `AgentConfig` CR |
| `docs/knowledge-base.md` | How `revised_esrs_knowledge_base` is used |
| `docs/qa-and-objections.md` | In-batch Review Qs, metric-name refusals, general ESRS Q |
| `docs/screenshots/` | Run captures |
| `docs/alan-v8-system-prompt.md` | Alan v8 prompt (baseline for copy delta) |
| `docs/alan-v8-vs-v9.diff` | Unified diff: Alan v8 → v9 locked copy only |

## Walkthrough

Customer opens the Action Sheet side panel. Mini checks write access and outline. Wrong file → Marketplace lock string. On the Action Sheet it states then asks (verbatim):

The Action Sheet has [Y] rows of data, starting with row 4.

I'll review the Decision column of the Action Sheet, 50 rows at a time. Which row should I start with?

Approval question (verbatim): **Update the Action Sheet with these suggested decisions?** Buttons: **Yes** | **No** | **Review again**.

![Start row](docs/screenshots/v2-start-row.png)

After they pick a row (here: 54), Mini reads H/S then D–R, applies the trigger matrix, and may call **`revised_esrs_knowledge_base` once** if H is annotated or no trigger is clean. See [Knowledge base](docs/knowledge-base.md). It should not narrate that work; the next customer-facing step is approval.

**Approve** writes **Column S only** in one `docplat_write_cell_data` span (`S{first}:S{last}`). T and U stay customer-owned (`N/A`, `Assign Metric Name`, etc.).

![Sheet after S write — T/U untouched](docs/screenshots/v2-sheet-s-written-t-untouched.png)

The assistant should stop at `Update the Action Sheet with these suggested decisions?` (Yes / No / Review again).

![Approve then write array chatter](docs/screenshots/v2-approve-write-array.png)

Then one platform **confirmed** write. Cursor advances; next cycle starts silently from Step 4.

## Questions and objections

Customers can push back on a row at `Write this batch?` (e.g. “explain the discrete?” on row 108) or ask Mini to draft metric names. Mini should explain S, refuse T/U naming, and not write until Approve. Full captures: [Questions and objections](docs/qa-and-objections.md).

**General ESRS questions work at the start of a new session.** The same question **mid row-scan hangs** (infinite loading). Use a fresh chat for team/briefing Qs; keep in-batch chat to S decisions only.

## Workflow (short)

1. Start row  
2. Silent read / decide (KB at most once if needed)  
3. One `Row | Decision` table  
4. `Write this batch?`  
5. One write to Column S  

## Tools

`authorization_evaluation`, `xml_get_table_outline`, `request_human_input`, `docplat_query_range`, `read-spreadsheet-row`, `docplat_write_cell_data`, `revised_esrs_knowledge_base`
