# ESRS Transition Assistant Mini

Workiva Assistant **single-agent** config for staging ESRS Action Sheet **Column S** only. Customers name their own metrics in T/U. No subagents.

- **Agent name:** esrs-transition-assistant-mini (AI-panel)
- **Repo / product name:** `esrs-transition-assistant-mini`
- **Prompt version:** 2 (`VERSION`)
- **Model:** `claude_46_sonnet` (temperature 0.1, top_p 1, `thinking: false`)

## Files

| File | Use |
|------|-----|
| `system-prompt.md` | Paste-ready system prompt |
| `spec.yaml` | Spec-only AgentConfig (`single_agent_config`) |
| `helm/agentconfig.yaml` | Full Helm `AgentConfig` CR (`ai.workiva.net/v1alpha1`) |
| `docs/screenshots/` | Opening UI captures |

## Workflow

1. Start row  
2. Silent read/decide  
3. One `Row | Decision` table  
4. `Write this batch?` — Approve / Hold / Review  
5. One `docplat_write_cell_data` call on Column S only  

## Tools

`authorization_evaluation`, `xml_get_table_outline`, `request_human_input`, `docplat_query_range`, `read-spreadsheet-row`, `docplat_write_cell_data`, `revised_esrs_knowledge_base`
