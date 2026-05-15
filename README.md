# iClinic Voice AI - Name Capture Agent

## Architecture
Caller → Retell AI → n8n Webhook → Mistral AI → Correction → Confirmation
↓
Spelling Fallback (2 attempts)

## Components

| Component | Purpose |
|-----------|---------|
| Retell AI | Voice agent runtime |
| n8n | Workflow orchestration (Render) |
| Mistral AI | LLM name correction |
| UptimeRobot | Keep-alive service |

## Flow

1. Ask first name → Extract → Call n8n webhook → Confirm
2. If rejected → Spell (2 attempts max) → Extract spelled name
3. Same for last name → End call

## n8n Webhook

**Endpoint:** `https://n8n-cnlz.onrender.com/webhook/retell-name-rescoring`

**Test:**
Response: {"action":"confirm","suggested_name":"Patel"}

Features
LLM correction for ASR errors (Fourteen → Patel)

Spelling fallback with special character handling (O'Brien)

Keep-alive via UptimeRobot (no cold starts)

Files
retell-agent-config.json - Retell workflow export

n8n-workflow.json - n8n workflow (add YOUR_MISTRAL_API_KEY)
```markdown
# iClinic Voice AI Agent

Retell AI + n8n + Mistral AI for name capture with ASR correction and spelling fallback.

## Files
- `retell-agent-config.json` - Retell workflow
- `n8n-workflow.json` - n8n webhook (add API key)

## Live Test
```bash
curl -X POST "https://n8n-cnlz.onrender.com/webhook/retell-name-rescoring" -H "Content-Type: application/json" -d "{\"asr_text\":\"Fourteen\",\"name_type\":\"last\"}"
