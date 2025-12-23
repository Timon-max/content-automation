cat > docs/setup.md << 'EOF'
# Setup Instructions

## Prerequisites

- n8n instance (cloud or self-hosted)
- Fathom account with API access
- Notion workspace with API integration
- OpenAI API key
- Perplexity API key

## Notion Database Structure

### Content Library
- Title (title)
- Content (text)
- Status (select)
- Input/Brief (text)
- FIRE AUTOMATION (checkbox)
- Related Content Project (relation)

### Call Insights & Ideas
- Title (title)
- Content Angles (text)
- Pain Points (text)
- Client Language (text)
- Aha Moment (text)

### Brand Guidelines
- Body (rich text with brand voice rules)

### Generated Content
- Title (title)
- Content (text)
- Link (URL)
- Status (select)

## Workflow Import

1. Import `fathom-webhook-workflow.json` first
2. Configure Fathom webhook URL in workflow settings
3. Import `content-generation-workflow.json`
4. Update all credentials (Notion, OpenAI, Perplexity)
5. Test with a sample call insight entry

## Configuration

Each workflow requires:
- Notion API credentials
- OpenAI API key (GPT-4 access required)
- Perplexity API key
- Fathom webhook secret (for webhook workflow)
EOF
