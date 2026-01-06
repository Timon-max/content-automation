# Quick Import Guide - Coaches & Consultants Workflow

## 📥 Importing the Workflow

1. **Download the workflow file:**
   - File: `docs/workflows/Content-Automation-Coaches-Consultants.json`

2. **Import to n8n:**
   - Open n8n
   - Click "Workflows" → "Import from File"
   - Select the JSON file
   - Click "Import"

## 🔧 Required Configuration (10 minutes)

### Step 1: Replace Database IDs

The workflow has placeholder database IDs. Replace them with your actual IDs:

**Find these in each node:**
1. **Notion Trigger** → `YOUR_CONTENT_DATABASE_ID_HERE`
2. **High Performers Search** → `YOUR_HIGH_PERFORMERS_DATABASE_ID_HERE`
3. **Fathom Call Ideas** → `YOUR_CALL_INSIGHTS_DATABASE_ID_HERE` (optional)
4. **Branding Guidelines** → `YOUR_BRANDING_GUIDELINES_DATABASE_ID_HERE`
5. **Client Testimonials** → `YOUR_CLIENT_TESTIMONIALS_DATABASE_ID_HERE`

**How to get database IDs:**
- Open database in Notion
- Look at URL: `https://notion.so/workspace/DATABASE_ID?v=...`
- Copy the 32-character code before `?v=`

### Step 2: Add API Credentials

**Notion API:**
- Click any Notion node
- "Credential for Notion API" → "Create New"
- Paste your Notion integration secret

**Perplexity API:**
- Click "HTTP Request - Perplexity Research" node
- "Authentication" → "Generic Credential Type" → "Header Auth"
- Name: `Authorization`
- Value: `Bearer YOUR_PERPLEXITY_API_KEY`

**Claude API (Anthropic):**
- Click "Create Posts" node
- "Credential for Anthropic" → "Create New"
- Paste your Anthropic API key

### Step 3: Update Status Property Name (if different)

In the last node "Update a database page":
- If your status property is named differently than "Status"
- Update the key in the node configuration

## ✅ Test the Workflow

1. Click "Execute Workflow" in n8n
2. Or create a test page in Notion:
   - Add brief in "Input/Brief" field
   - Check "FIRE AUTOMATION" checkbox
   - Wait 1-2 minutes

## 🎯 What's Already Configured

✅ **All prompts updated for coaches/consultants**
- Generic coaching/consulting industry focus
- No AI agency specific language
- Anti-hallucination rules included
- Voice authenticity guidelines

✅ **Claude 3.5 Sonnet integration**
- Temperature: 0.7
- Max tokens: 4000
- JSON-only output format

✅ **Perplexity research**
- Coaching industry trends
- Client transformation data
- ROI and success metrics

✅ **Context Aggregator**
- Combines all 6 data sources
- Proper attribution rules
- Real vs industry data separation

✅ **Notion formatting**
- Character limit handling (2000 char chunks)
- Proper block structure
- Voice scores included

✅ **Automatic cleanup**
- Unchecks FIRE AUTOMATION
- Sets status to Complete

## 📋 Database Structure Needed

Make sure your Notion databases have these properties:

**Content Brief & Outputs:**
- Name (Title)
- Input/Brief (Text)
- FIRE AUTOMATION (Checkbox)
- Status (Select: Pending, Processing, Complete, Error)

**High Performers:**
- Name (Title)
- Post Content (Text)

**Branding Guidelines:**
- Name (Title)
- Guidelines (Text)

**Client Testimonials:**
- Name (Title)
- Client Name (Text)
- Project Type (Text)
- Result/Metric (Text)
- Testimonial (Text)

**Call Insights & Ideas (optional):**
- Name (Title)
- Content Angles (Text)
- Key Insights (Text)

## 🚀 You're Done!

Activate the workflow and start creating content!

For detailed setup instructions, see: `docs/content-automation-guide.md`
