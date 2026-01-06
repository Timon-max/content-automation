# Content Automation Workflow - Complete Setup Guide

## What This Automation Does

This automation generates high-quality LinkedIn posts automatically based on your brief. It:

- **Pulls from multiple data sources** - Your client testimonials, brand guidelines, high-performing posts, and Fathom call insights
- **Researches industry trends** - Uses Perplexity AI to find current statistics and data
- **Generates authentic content** - Uses Claude AI to write posts in your unique voice
- **Saves everything to Notion** - Automatically writes generated posts back to your database

**Trigger:** Check a box in Notion called "FIRE AUTOMATION"
**Result:** Multiple LinkedIn post variations appear in your Notion page within 1-2 minutes

---

## Prerequisites

Before you begin, you'll need accounts for:

- [ ] **Notion** (Free plan works)
- [ ] **n8n** (Cloud or self-hosted)
- [ ] **Claude API** (Anthropic)
- [ ] **Perplexity API**
- [ ] **Fathom** (optional, for call insights)

**Estimated setup time:** 45-60 minutes

---

## Part 1: Notion Database Setup

### Step 1: Create Your Main Content Database

1. Open Notion and navigate to your workspace
2. Create a new database (full-page database, not inline)
3. Name it **"Content Brief & Outputs"** (or your preferred name)

### Step 2: Add Required Properties

Your database needs these **exact** properties (names matter!):

| Property Name | Type | Purpose |
|--------------|------|---------|
| `Name` | Title | Default title property |
| `Input/Brief` | Text (long text) | Your content brief/topic |
| `FIRE AUTOMATION` | Checkbox | Triggers the workflow |
| `Status` | Select | Tracks automation status |
| `Created time` | Created time | Auto-generated |

**For the Status property, add these options:**
- Pending
- Processing
- Complete
- Error

### Step 3: Create Supporting Databases

You'll need 4 additional databases:

#### A. High Performers Database
**Purpose:** Store your best-performing LinkedIn posts as examples

**Properties:**
- `Name` (Title)
- `Post Content` (Text - long text)
- `Engagement` (Number - optional)
- `Date Posted` (Date - optional)

**Action:** Add 3-5 of your best LinkedIn posts to this database

#### B. Branding Guidelines Database
**Purpose:** Store your brand voice and writing guidelines

**Properties:**
- `Name` (Title)
- `Guidelines` (Text - long text)

**Action:** Create one page with your brand voice description. Example:
```
Name: Brand Voice Guidelines
Guidelines:
- Direct, conversational tone
- No corporate jargon or buzzwords
- Short sentences, punchy paragraphs
- Use specific numbers and results
- Avoid: "game-changer", "leverage", "synergy", "ecosystem"
- Use storytelling and personal examples
```

#### C. Client Testimonials Database
**Purpose:** Store real client results (prevents AI hallucination)

**Properties (must match exactly):**
- `Name` (Title)
- `Client Name` (Text)
- `Project Type` (Text)
- `Result/Metric` (Text)
- `Testimonial` (Text - long text)

**Action:** Add your client success stories. Example:
```
Name: Sarah - Coaching Business Scale
Client Name: Sarah Johnson
Project Type: 1:1 Coaching Program
Result/Metric: Scaled from $5K/month to $35K/month in 4 months
Testimonial: "The framework completely transformed how I approach client acquisition..."
```

#### D. Call Insights & Ideas Database
**Purpose:** Store content ideas from Fathom call analysis

**Properties:**
- `Name` (Title)
- `Content Angles` (Text - long text)
- `Key Insights` (Text - long text)
- `Fathom Call ID` (Number)
- `Call Date` (Date)

**Action:** This will be populated automatically by the Fathom workflow (optional)

---

## Part 2: Setting Up n8n

> **Note:** The user will provide n8n setup instructions here

---

## Part 3: API Credentials Setup

### Setting Up Claude API (Anthropic)

**Step 1: Create Anthropic Account**

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Click "Sign Up" or "Get Started"
3. Complete the registration process
4. Verify your email address

**Step 2: Get API Key**

1. Once logged in, click on your profile/account settings (top right)
2. Navigate to **"API Keys"** section
3. Click **"Create Key"** or **"New API Key"**
4. Give it a descriptive name: `n8n-content-automation`
5. Copy the API key immediately (you won't see it again!)
6. Store it somewhere safe (password manager recommended)

**Step 3: Add Credits**

1. Go to **"Billing"** section
2. Click **"Add Credits"**
3. Add at least $10 to start (Claude 3.5 Sonnet costs ~$3 per million tokens)
4. Set up auto-reload if desired

**Step 4: Configure in n8n**

1. In n8n, go to **Credentials** (left sidebar)
2. Click **"Add Credential"**
3. Search for **"Anthropic"** or **"Claude"**
4. Enter your API key
5. Click **"Save"**
6. Name it: `Claude API - Content Automation`

**Expected costs:** ~$0.10-0.30 per content generation run

---

### Setting Up Perplexity API

**Step 1: Create Perplexity Account**

1. Go to [perplexity.ai](https://perplexity.ai)
2. Sign up for an account
3. Verify your email

**Step 2: Access API Dashboard**

1. Navigate to [perplexity.ai/settings/api](https://perplexity.ai/settings/api)
2. Or click your profile → Settings → API

**Step 3: Get API Key**

1. In the API section, click **"Generate API Key"**
2. Give it a name: `n8n-research`
3. Copy the API key
4. Store it securely

**Step 4: Add Credits**

1. In the API settings, go to billing
2. Add credits (minimum $10 recommended)
3. Perplexity API costs ~$1-5 per 1000 requests depending on model

**Step 5: Configure in n8n**

Since Perplexity doesn't have a native n8n node, we'll use HTTP Request:

1. In the workflow, find the **"HTTP Request - Perplexity Research"** node
2. In the **Authentication** section:
   - Select **"Generic Credential Type"**
   - Choose **"Header Auth"**
3. Create new Header Auth credential:
   - Name: `Authorization`
   - Value: `Bearer YOUR_PERPLEXITY_API_KEY`
   - Replace `YOUR_PERPLEXITY_API_KEY` with your actual key
4. Save the credential

**Expected costs:** ~$0.05-0.15 per research request

---

### Setting Up Notion Integration

**Step 1: Create Integration**

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **"+ New integration"**
3. Fill in the details:
   - Name: `n8n Content Automation`
   - Associated workspace: Select your workspace
   - Type: Internal integration
4. Click **"Submit"**
5. Copy the **"Internal Integration Secret"** (starts with `secret_`)

**Step 2: Configure Capabilities**

Make sure these are enabled:
- [x] Read content
- [x] Update content
- [x] Insert content

**Step 3: Share Databases with Integration**

This is critical - the integration can only access databases you explicitly share!

1. Open each of your 5 databases in Notion
2. Click the **"Share"** button (top right)
3. Click **"Invite"**
4. Search for your integration name: `n8n Content Automation`
5. Click to add it
6. Repeat for all databases:
   - Content Brief & Outputs
   - High Performers
   - Branding Guidelines
   - Client Testimonials
   - Call Insights & Ideas

**Step 4: Get Database IDs**

You'll need the database ID for each database:

1. Open a database in Notion
2. Look at the URL: `https://notion.so/workspace/DATABASE_ID?v=...`
3. The DATABASE_ID is the 32-character code before the `?v=`
4. Example: `https://notion.so/myworkspace/a1b2c3d4e5f6...` → `a1b2c3d4e5f6...` is the ID

Copy and save each database ID:
- Content Brief & Outputs ID: `_______________`
- High Performers ID: `_______________`
- Branding Guidelines ID: `_______________`
- Client Testimonials ID: `_______________`
- Call Insights & Ideas ID: `_______________`

**Step 5: Configure in n8n**

1. In n8n, go to **Credentials**
2. Click **"Add Credential"**
3. Search for **"Notion API"**
4. Paste your integration secret
5. Click **"Save"**
6. Name it: `Notion - Content Automation`

---

## Part 4: Importing the Workflow

### Step 1: Get the Workflow JSON

The workflow JSON file should be in your project files:
- Look for `workflows/content-automation.json` or similar

### Step 2: Import to n8n

1. Open n8n
2. Click **"Workflows"** in the left sidebar
3. Click **"Import from File"** or use the menu (⋮) → Import from File
4. Select the workflow JSON file
5. The workflow will open in the editor

### Step 3: Configure Each Node

Now you need to connect your credentials and database IDs to each node:

#### Node 1: Notion Trigger
1. Click on the **"Notion Trigger"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database
4. **Database ID:** Paste your **Content Brief & Outputs** database ID
5. **Trigger On:** Page Updated
6. **Polling Interval:** 1 minute (or adjust to your preference)
7. Click **"Execute Node"** to test

**Expected output:** Should return pages from your database

#### Node 2: IF
1. Click on the **"IF"** node
2. **Condition:** Boolean
3. **Value 1:** `{{ $json.property_fire_automation }}` (mapped from Notion Trigger)
4. **Operation:** Is True

No changes needed if already configured.

#### Node 3: High Performers Search
1. Click on the **"High Performers Search"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database Page
4. **Operation:** Get Many
5. **Database ID:** Paste your **High Performers** database ID
6. **Return All:** Toggle ON
7. Click **"Execute Node"** to test

**Expected output:** Your high-performing posts

#### Node 4: Fathom Call Ideas
1. Click on the **"Fathom Call Ideas"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database Page
4. **Operation:** Get Many
5. **Database ID:** Paste your **Call Insights & Ideas** database ID
6. **Options → Sort:** Created Time, Descending
7. **Limit:** 3
8. Click **"Execute Node"** to test

**Expected output:** Recent Fathom call insights (or empty if you don't use Fathom)

#### Node 5: Branding Guidelines
1. Click on the **"Branding Guidelines"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database Page
4. **Operation:** Get Many
5. **Database ID:** Paste your **Branding Guidelines** database ID
6. Click **"Execute Node"** to test

**Expected output:** Your brand guidelines

#### Node 6: Client Testimonials
1. Click on the **"Client Testimonials"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database Page
4. **Operation:** Get Many
5. **Database ID:** Paste your **Client Testimonials** database ID
6. **Return All:** Toggle ON
7. Click **"Execute Node"** to test

**Expected output:** All your client testimonials

#### Node 7: HTTP Request - Perplexity Research
1. Click on the **"HTTP Request - Perplexity Research"** node
2. **Authentication:** Generic Credential Type → Header Auth
3. **Credential:** Select your Perplexity Header Auth credential
4. **Method:** POST
5. **URL:** `https://api.perplexity.ai/chat/completions`

The body should already be configured with:
```json
{
  "model": "sonar",
  "messages": [
    {
      "role": "system",
      "content": "You are a research assistant providing accurate, recent statistics and industry data for business content creation. Focus on delivering factual, verifiable information with specific numbers, percentages, and credible sources."
    },
    {
      "role": "user",
      "content": "={{ \"CONTENT BRIEF:\\n\" + $('Notion Trigger').item.json['Input/Brief'] + \"\\n\\nResearch and provide:\\n1. RECENT STATISTICS & METRICS related to this topic (with specific numbers and percentages)\\n2. CURRENT INDUSTRY TRENDS and market data\\n3. COMMON CHALLENGES or debates in this space\\n4. CASE STUDIES or data points from reputable sources\\n\\nFormat your response clearly with headers and bullet points.\" }}"
    }
  ],
  "temperature": 0.2,
  "max_tokens": 1500
}
```

Click **"Execute Node"** to test.

**Expected output:** Research data from Perplexity

#### Node 8: Context Aggregator (Code)
1. Click on the **"Context Aggregator"** node
2. This is a JavaScript code node - should already be configured
3. Click **"Execute Node"** to test

**Expected output:** Formatted prompt combining all data sources

#### Node 9: Create Posts (Claude)
1. Click on the **"Create Posts"** node
2. **Credential:** Select your `Claude API - Content Automation` credential
3. **Model:** claude-3-5-sonnet-20241022 (or latest version)
4. **Prompt:** Should already contain the full prompt (very long)
5. **Temperature:** 0.7
6. **Max Tokens:** 4000

Click **"Execute Node"** to test.

**Expected output:** JSON with generated posts

#### Node 10: Code in JavaScript
1. Click on the **"Code in JavaScript"** node
2. This parses Claude's response and formats for Notion
3. Should already be fully configured
4. Click **"Execute Node"** to test

**Expected output:** Notion blocks formatted correctly

#### Node 11: Put Content Into Database (HTTP Request)
1. Click on the **"Put Content Into Database"** node
2. **Authentication:** Predefined Credential Type → Notion API
3. **Credential:** Select your `Notion - Content Automation` credential
4. **Method:** PATCH
5. **URL:** `={{ "https://api.notion.com/v1/blocks/" + $('Notion Trigger').item.json.id + "/children" }}`

Headers should be:
```
Notion-Version: 2022-06-28
Content-Type: application/json
```

Body should reference the previous node's output.

Click **"Execute Node"** to test.

**Expected output:** Success message from Notion API

#### Node 12: Update a database page
1. Click on the **"Update a database page"** node
2. **Credential:** Select your `Notion - Content Automation` credential
3. **Resource:** Database Page
4. **Operation:** Update
5. **Page ID:** `={{ $('Notion Trigger').item.json.id }}`
6. **Properties → FIRE AUTOMATION:** Uncheck (false)
7. **Properties → Status:** Complete

Click **"Execute Node"** to test.

**Expected output:** Updated page in Notion

### Step 4: Save and Activate

1. Click **"Save"** (top right)
2. Name your workflow: **"Content Automation - Main"**
3. Toggle **"Active"** (top right) to turn it ON

The workflow is now running and will check Notion every minute!

---

## Part 5: Testing Your Workflow

### First Test Run

1. Open your **Content Brief & Outputs** database in Notion
2. Create a new page with:
   - **Name:** "Test - LinkedIn Post About Time Management"
   - **Input/Brief:**
     ```
     Create a LinkedIn post about how I help coaches manage their time better.

     Target audience: Online coaches who feel overwhelmed
     Key message: You don't need more time, you need better systems
     Desired action: Comment with their biggest time management challenge
     ```
   - **Status:** Pending
   - **FIRE AUTOMATION:** ☐ (leave unchecked for now)

3. Make sure you have:
   - At least 1 page in **High Performers** database
   - At least 1 page in **Branding Guidelines** database
   - At least 2-3 pages in **Client Testimonials** database

4. Check the **FIRE AUTOMATION** checkbox ✅

5. Wait 1-2 minutes (the polling interval)

6. Refresh your Notion page

**What should happen:**
- ✅ Page content fills with generated LinkedIn posts (3-5 variations)
- ✅ **FIRE AUTOMATION** checkbox unchecks automatically
- ✅ **Status** changes to "Complete"

**If nothing happens:**
- Check that the workflow is Active in n8n
- Check the n8n executions tab for errors
- Verify all credentials are connected
- Verify all database IDs are correct
- See Troubleshooting section below

---

## Part 6: Troubleshooting

### Error: "Cannot read properties of undefined"

**Cause:** Data not flowing between nodes correctly

**Fix:**
1. Click on the failing node
2. Check that input references are correct
3. Example: If referencing testimonials, make sure the node name matches: `$('Client Testimonials')`
4. Use the "Execute Previous Nodes" button to ensure data is available

---

### Error: "body.children[X].paragraph.rich_text[0].text.content should be ≤ 2000"

**Cause:** Notion has a 2000 character limit per text block

**Fix:** The workflow already includes a `splitIntoChunks()` function. If you still see this:
1. Open the **"Code in JavaScript"** node
2. Check that `maxLength = 1950` (not 2000)
3. Verify the function is being called: `splitIntoChunks(post.content)`

---

### Error: "Unexpected token 'I', "I'll creat"... is not valid JSON"

**Cause:** Claude is adding explanatory text instead of pure JSON

**Fix:**
1. Open the **"Create Posts"** node
2. Verify the prompt starts with: "ABSOLUTE REQUIREMENT: You MUST return ONLY the JSON object..."
3. The **"Code in JavaScript"** node should have aggressive JSON extraction code (already included)

---

### Error: Client testimonials showing as "Anonymous"

**Cause:** Property names don't match Notion's API format

**Fix:** In the **"Context Aggregator"** node, ensure property names use both formats:
```javascript
const clientName = t.property_client_name || t['Client Name'] || 'Anonymous';
```

Notion API returns properties as `property_client_name` (lowercase with underscores)

---

### Workflow not triggering

**Checklist:**
- [ ] Workflow is Active (toggle in top right)
- [ ] Notion integration has access to all 5 databases
- [ ] Database ID in Notion Trigger is correct
- [ ] "FIRE AUTOMATION" property name matches exactly (case-sensitive)
- [ ] Polling interval is set (default: 1 minute)

---

### Claude API errors

**"Invalid API key"**
- Regenerate API key in Anthropic console
- Update credential in n8n
- Make sure there are no extra spaces when pasting

**"Insufficient credits"**
- Add more credits in Anthropic billing section
- Set up auto-reload

**"Rate limit exceeded"**
- Wait a few minutes and try again
- Claude API has rate limits for new accounts

---

### Perplexity API errors

**"401 Unauthorized"**
- Check that Header Auth credential is formatted correctly
- Should be: `Authorization: Bearer YOUR_KEY`
- Regenerate API key if needed

**"Insufficient credits"**
- Add credits in Perplexity API billing

---

### Generated content quality issues

**Posts sound too generic/corporate:**
1. Add more high-performing examples to **High Performers** database
2. Make **Branding Guidelines** more specific with examples of what to avoid
3. Add more specific instructions in the Claude prompt

**Posts include fake statistics:**
1. Ensure **Client Testimonials** database has real, specific results
2. The prompt already includes anti-hallucination rules
3. Verify Perplexity node is running successfully (provides real industry data)

**Posts too long/short:**
1. Open **"Create Posts"** node
2. Modify the prompt to specify desired length
3. Example: "Each post should be 150-200 words (roughly 1200-1600 characters)"

---

## Part 7: Daily Usage Guide

### How to Use This Automation

**Every time you want to create LinkedIn content:**

1. Open your **Content Brief & Outputs** database in Notion
2. Create a new page (or duplicate a previous one)
3. Fill in the **Input/Brief** with:
   - Topic or theme
   - Target audience
   - Key message
   - Desired action (comment, DM, link click, etc.)
   - Any specific requirements

**Example brief:**
```
Topic: How I helped a client 3x their revenue

Target audience: Consultants stuck at $10-15K/month
Key message: It wasn't about working harder, it was about changing the offer structure
Call to action: DM me "OFFER" to learn the framework
Tone: Story-driven, specific, no hype

Requirements:
- Use Sarah's testimonial (the $5K to $35K case study)
- Reference the industry stat about pricing from recent Perplexity research
- Hook should be contrarian/surprising
```

4. Set **Status** to "Pending"
5. Check the **FIRE AUTOMATION** ✅ checkbox
6. Wait 1-2 minutes
7. Refresh the page
8. Review the 3-5 generated post variations
9. Pick your favorite, make minor edits if needed, and post!

### Best Practices

**Keep your databases updated:**
- Add new high-performers monthly
- Update client testimonials when you get results
- Refresh branding guidelines quarterly

**Optimize your briefs:**
- Be specific about the audience
- Reference specific client results by name
- Include the desired emotional response
- Specify the call-to-action

**Review before posting:**
- Always read generated content before posting
- Check that statistics are attributed correctly
- Ensure client results are accurate
- Add your personal touch

**Track what works:**
- Note which angles get the most engagement
- Add successful posts to High Performers database
- Refine your briefs based on results

---

## Part 8: Understanding the Workflow

### What Happens Behind the Scenes

Here's the step-by-step process when you check that box:

1. **Notion Trigger** detects the checkbox change (polls every minute)
2. **IF Node** confirms checkbox is checked (true)
3. **Data Collection Phase:**
   - Fetches your high-performing posts (examples)
   - Fetches recent Fathom call insights (optional)
   - Fetches your brand guidelines
   - Fetches all client testimonials (real results)
4. **Research Phase:**
   - Perplexity searches for current industry statistics
   - Finds relevant trends, data, challenges
5. **Context Aggregation:**
   - Combines all data into a structured prompt
   - Separates "YOUR RESULTS" from "INDUSTRY DATA"
   - Formats for Claude AI
6. **Content Generation:**
   - Claude generates 3-5 post variations
   - Each with different angles/hooks
   - Follows your brand voice
   - Uses real client results
   - Includes industry data
7. **Formatting:**
   - Parses Claude's JSON response
   - Splits long content into chunks (Notion 2000 char limit)
   - Formats as Notion blocks (headings, paragraphs)
8. **Writing to Notion:**
   - Appends formatted posts to your page content
   - Updates Status to "Complete"
   - Unchecks FIRE AUTOMATION box

**Total time:** 60-120 seconds

---

## Part 9: Cost Breakdown

### Estimated Monthly Costs

If you generate content 3x per week (12x per month):

**Claude API (Anthropic):**
- Per request: ~$0.15-0.30
- Monthly (12 runs): ~$1.80-3.60

**Perplexity API:**
- Per request: ~$0.05-0.15
- Monthly (12 runs): ~$0.60-1.80

**Notion API:**
- Free (no API costs)

**n8n:**
- Cloud: $20/month (starter plan)
- Self-hosted: Free (server costs vary)

**Total monthly cost:** ~$22-26 (cloud n8n) or ~$2-6 (self-hosted)

**Cost per generated content piece:** ~$1.80-2.00

This is significantly cheaper than hiring a content writer ($50-200 per post) or agency ($500-2000/month retainer).

---

## Part 10: Advanced Customization

### Changing the Number of Post Variations

1. Open **"Create Posts"** node
2. Find this section in the prompt:
   ```
   DELIVERABLE: Generate 5 distinct LinkedIn post variations
   ```
3. Change `5` to your desired number (3-7 recommended)

### Adjusting Content Length

1. Open **"Create Posts"** node
2. Add length specification to the prompt:
   ```
   Each post should be approximately 150-200 words (1200-1600 characters).
   ```

### Adding New Data Sources

Want to pull from another Notion database?

1. Create the database in Notion
2. Share it with your n8n integration
3. Add a new Notion node in the workflow
4. Reference it in the **Context Aggregator** code node

### Changing the Trigger

Want to use a different trigger method?

**Option A: Manual Button (Webhook)**
- Replace Notion Trigger with Webhook node
- Create a button in Notion that calls the webhook
- Faster but requires Notion API setup

**Option B: Schedule-Based**
- Replace with Schedule Trigger node
- Runs automatically on schedule
- Less control over when it fires

---

## Next Steps

Once your workflow is running smoothly:

1. **Build the Fathom Integration** (optional)
   - Automatically analyze sales calls
   - Extract content ideas
   - Feed into main workflow

2. **Create a Content Calendar**
   - Set up Notion database for scheduling
   - Plan content themes weekly
   - Batch generate content

3. **Add Quality Filters**
   - Create approval workflow
   - Add human review step
   - Track engagement metrics

4. **Scale to Other Platforms**
   - Duplicate workflow for Twitter/X
   - Adapt for Instagram captions
   - Modify for email newsletters

---

## Support & Resources

**Official Documentation:**
- n8n: [docs.n8n.io](https://docs.n8n.io)
- Notion API: [developers.notion.com](https://developers.notion.com)
- Claude API: [docs.anthropic.com](https://docs.anthropic.com)
- Perplexity API: [docs.perplexity.ai](https://docs.perplexity.ai)

**Community:**
- n8n Community: [community.n8n.io](https://community.n8n.io)
- n8n Discord: Join for real-time help

**Need Help?**
If you get stuck, check:
1. The Troubleshooting section above
2. n8n execution logs (shows exactly where it failed)
3. Notion API error messages (very descriptive)
4. This documentation's specific sections

---

## Changelog

**Version 1.0** (Current)
- Initial complete workflow
- Claude 3.5 Sonnet integration
- Perplexity research integration
- Anti-hallucination safeguards
- Character limit handling
- Automatic checkbox reset

**Improvements from earlier versions:**
- Switched from ChatGPT to Claude for better voice matching
- Added client testimonials to prevent fake statistics
- Implemented text chunking for Notion limits
- Added industry research via Perplexity
- Separated client results from industry data
- Fixed property name mapping issues

---

**You're all set!** Follow the steps above, take your time with each section, and you'll have your content automation running smoothly. The first setup takes about an hour, but then you'll have unlimited content generation at your fingertips.
