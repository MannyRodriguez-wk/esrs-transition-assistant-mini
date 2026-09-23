# Knowledge base

esrs-transition-agent calls **`revised_esrs_knowledge_base`** at most **once per batch**. That is the only knowledge base. There is no 2023 / `esg_esrs_knowledge_base` tool on this agent.

The **trigger matrix** on columns H, J, L, and NMIG still makes the decision. The KB is consulted, not a second matrix.

## When it is used

1. **Verify trigger inputs.** Column H is annotated (`WK POV:`, `WK Note:`) or self-contradictory. The KB confirms what EFRAG actually did. Change Type is a trigger input — correcting it can change which trigger fires. Text after `WK POV:` / `WK Note:` does not override the EFRAG classification before the annotation unless the KB says that classification is wrong.

2. **Choose among candidates.** Two triggers both fire, or **no** trigger fires and a closest-fit Column S string must be chosen. Typical gaps:
   - Amended or Moved with L = No
   - Deleted + NMIG (voluntary vs delete)
   - no-trigger row: Maintain Disclosure vs Adjust IDs

Query the batch’s requirements together in that one call (IDs / E text), not one call per row.

## When it is not used

A **clean single match** — exactly one trigger fires on unambiguous H, J, and L — is the decision. The KB must not overrule it.

Examples that should not wait on the KB: Deleted + L = Yes + no NMIG → Delete from Program; New + L = No → Add to Program.

## What customers see

Nothing from the KB. No citations, bracket numbers, source snippets, or the tool name. Chat after start-row is the `Row | Decision` table, then `Write this batch?`.

## Helm / spec

The tool must stay attached:

```yaml
- tool: revised_esrs_knowledge_base
```

Prompt section: **Knowledge Base — silent** in `system-prompt.md`.
