# Part 4: Technical Architecture (30 points)

![Hero Image](assets/part4-hero.png)

## The Madison Agent Design (15 points)
 
**What Are AI Agents?**
 
An AI agent is a program that:
1. Takes input
2. Makes decisions (usually using AI)
3. Produces output
4. Can communicate with other agents
 
**In Madison**: You'll have multiple agents working together, each with a specific job.
 
## Designing Your Agents

![Agent Design Image](assets/part4-agents.png)

**Think Like a Team Manager**:
 
If you had a team of specialists, what would each person do?
 
**Example Project 1: Competitor Social Monitor**
 
**Bad Agent Design** (too generic):
- Agent 1: Get social data
- Agent 2: Analyze posts
- Agent 3: Send alerts
 
**Why It's Bad**: Not specific enough. What exactly does each agent do?
 
**Good Agent Design** (specific jobs):
 
**Agent 1: Social Data Collection Agent**
- **Specific Job**: Fetch posts from 5 competitors across Twitter and LinkedIn
- **Input**: List of competitor handles/company pages, time range (last 24 hours)
- **Processing**: Authenticate with APIs, paginate through posts, handle rate limits
- **Output**: Standardized JSON array with post text, timestamp, engagement metrics, platform
- **Why Needed**: Different platforms have different API structures; this normalizes data
 
**Agent 2: Trend Detection Agent**
- **Specific Job**: Identify posts with unusual engagement or trending keywords
- **Input**: Standardized posts + baseline engagement data + industry keyword list
- **Processing**: Calculate engagement rate vs. competitor's average, match against trending keywords, score relevance
- **Output**: Posts scored for strategic importance (0-100), with reasoning
- **Why Needed**: Not all posts matter; this filters signal from noise
 
**Agent 3: Digest & Alert Agent**
- **Specific Job**: Generate daily summary and send immediate alerts for high-priority posts
- **Input**: Scored posts from Trend Detection Agent
- **Processing**: Format top 10 posts into digest email, send Slack alerts for >90 importance scores
- **Output**: HTML email digest, Slack messages for urgent items
- **Why Needed**: Humans need digestible formats, not raw JSON
 
---
 
**Example Project 2: Multi-Platform Content Adapter**
 
**Agent 1: Content Extraction Agent**
- **Specific Job**: Pull blog post content and extract key information
- **Input**: Blog post URL
- **Processing**: Fetch HTML, remove boilerplate (header, footer, sidebar), extract title, main points, key quotes
- **Output**: Clean text with structure (title, intro, 3-5 main points, conclusion)
- **Why Needed**: Blog posts have lots of HTML noise; agents need clean content
 
**Agent 2: Platform Adaptation Agent**
- **Specific Job**: Generate platform-specific versions of the content
- **Input**: Clean structured content + platform specifications (character limits, best practices, format)
- **Processing**: Use GPT-4 with platform-specific prompts to create Twitter thread, LinkedIn post, Instagram caption
- **Output**: Platform-optimized versions maintaining core message
- **Why Needed**: Each platform has different norms, limits, and audience expectations
 
**Agent 3: Quality Check Agent**
- **Specific Job**: Verify all versions maintain message fidelity
- **Input**: Original content + all adapted versions
- **Processing**: Check that key points appear in all versions, verify tone consistency, flag any major deviations
- **Output**: Quality score per version + flagged issues
- **Why Needed**: Automated generation can drift from original message; this catches problems
 
---
 
**Example Project 3: Synthetic Persona Generator**
 
**Agent 1: Data Ingestion Agent**
- **Specific Job**: Process customer data from multiple sources
- **Input**: Survey CSV, interview transcripts (text files), customer support tickets
- **Processing**: Parse structured data (CSV), extract text from unstructured sources, normalize into common format
- **Output**: Array of customer responses with metadata (source, date, customer ID)
- **Why Needed**: Data comes in different formats; needs standardization
 
**Agent 2: Pattern Detection Agent**
- **Specific Job**: Identify common themes in goals, pain points, behaviors
- **Input**: Customer response array
- **Processing**: Use NLP to extract mentioned goals, pain points, preferences; cluster similar patterns
- **Output**: Theme clusters with frequency counts and example quotes
- **Why Needed**: Personas are based on patterns, not individual responses
 
**Agent 3: Persona Synthesis Agent**
- **Specific Job**: Generate readable persona documents from pattern data
- **Input**: Theme clusters + demographic data (if available)
- **Processing**: Use GPT-4 to synthesize patterns into persona narratives (demographics, goals, pain points, behaviors, preferences)
- **Output**: 3-5 persona documents in standard format
- **Why Needed**: Data patterns need to become human-readable personas marketers can use
 
---

![Agent Communication Image](assets/part4-communication.png)

## Agent Communication
 
**How Do They Talk to Each Other?**
 
Agents pass data through a defined structure.
 
**Competitor Monitor Flow**:  
User Input (Competitor List) -> [Social Data Collection Agent] -> Standardized Posts JSON -> [Trend Detection Agent] -> Scored Posts -> [Digest & Alert Agent] -> Email + Slack Notifications
 
**Content Adapter Flow**:  
User Input (Blog URL) -> [Content Extraction Agent] -> Clean Structured Content -> [Platform Adaptation Agent] -> Platform-Specific Versions -> [Quality Check Agent] -> Reviewed Versions + Quality Scores
 
**Synthetic Persona Flow**:  
User Input (Customer Data Files) -> [Data Ingestion Agent] -> Normalized Customer Responses -> [Pattern Detection Agent] -> Theme Clusters -> [Persona Synthesis Agent] -> Persona Documents
 
---

![MVP Scope Image](assets/part4-mvp.png)

## The MVP Scope (15 points)
 
**MVP = Minimum Viable Product**
 
The simplest version that proves your concept works.
 
**The Hard Truth**: You don't have time to build everything.
 
**The Goal**: Build the ONE workflow that proves your core idea.
 
## Define Your ONE Workflow
 
**What to Include**:
 
**1. The ONE workflow you'll create in n8n**
 
Be specific about what it does.
 
**Competitor Monitor**: "A workflow that: takes a list of 5 competitor Twitter handles, fetches their last 24 hours of posts, scores them for strategic importance, and emails a digest of the top 10 posts"
 
**Content Adapter**: "A workflow that: takes a blog post URL, extracts the content, generates Twitter thread + LinkedIn post + Instagram caption, and outputs all three in a formatted document"
 
**Synthetic Personas**: "A workflow that: takes a CSV of survey responses (100 rows), identifies 3-4 customer segments, and generates persona documents for each segment"
 
**2. The 3-5 nodes you'll use**
 
n8n works with "nodes" (building blocks). Name them specifically.
 
**Common Nodes**:
- HTTP Request (fetch web content, call APIs)
- OpenAI (run GPT analysis)
- Function (custom JavaScript)
- Code (Python scripts)
- Switch (conditional logic)
- Set (modify data)
- JSON (parse/format JSON)
- Email (send notifications)
- Slack (send messages)
 
**Competitor Monitor Example**:  
Node 1: HTTP Request - Fetch tweets via Twitter API Node 2: Function - Parse JSON, extract text and metrics Node 3: Function - Calculate engagement rate vs. baseline Node 4: OpenAI - Analyze for trending industry keywords Node 5: Email - Send daily digest
 
**Content Adapter Example**:  
Node 1: HTTP Request - Fetch blog post HTML Node 2: Function - Extract clean text, remove HTML Node 3: OpenAI - Generate Twitter thread version Node 4: OpenAI - Generate LinkedIn post version Node 5: OpenAI - Generate Instagram caption version
 
**3. The input → process → output flow**
 
Map it out clearly.
 
**Competitor Monitor Example**:  
INPUT:  
List of 5 competitor Twitter handles  
Time range: last 24 hours

PROCESS:  
For each competitor, fetch tweets from Twitter API  
Parse JSON responses, extract text, likes, retweets  
Calculate engagement rate (likes + retweets) / followers  
Flag posts with 2x+ average engagement  
Send flagged posts to GPT-4 for keyword matching  
Format top 10 as email digest with links

OUTPUT:  
Daily email with top 10 competitor posts  
Each post shows: text snippet, engagement metrics, why it was flagged  
Links to original posts
 
**Content Adapter Example**:  
INPUT:  
Blog post URL  
Target platforms: Twitter, LinkedIn, Instagram  

PROCESS:  
Fetch HTML from URL  
Extract text, remove HTML/CSS/JS  
Identify title, main points (3-5), conclusion  
Send to GPT-4 with Twitter prompt (create thread, hook-first, <280 chars per tweet)  
Send to GPT-4 with LinkedIn prompt (professional tone, business context, 1300 chars)  
Send to GPT-4 with Instagram prompt (visual language, hashtags, 2200 chars)  
Format all versions in document

OUTPUT:  
Twitter thread (5-7 tweets)  
LinkedIn post (professional, ready to paste)  
Instagram caption (with 5-10 hashtags)
 
**4. What you WON'T build**
 
**This is critical.** Being clear about what's out of scope keeps you focused.
 
**Competitor Monitor - NOT IN MVP**:  
NOT INCLUDED IN MVP:  
❌ LinkedIn monitoring (just Twitter for MVP)  
❌ Historical trend analysis (just 24-hour snapshots)  
❌ Sentiment analysis (just engagement metrics)  
❌ Automated posting responses (just monitoring)  
❌ Visual dashboard (just email digest)  
❌ Customizable filters (fixed importance algorithm)  
WHY: Core concept is "can we automate competitor monitoring?" Everything else is polish. Can add in future iterations.
 
**Content Adapter - NOT IN MVP**:
NOT INCLUDED IN MVP:
❌ Additional platforms (just Twitter/LinkedIn/Instagram)
❌ Image generation (text only)
❌ Automated posting (adaptation only, not distribution)
❌ Performance tracking (creation only, not analytics)
❌ Content calendar integration (standalone tool)
❌ Team collaboration (single user)
WHY: Proving we can maintain message fidelity while adapting to platform norms. Distribution comes in future iterations.
 
**Synthetic Personas - NOT IN MVP**:
NOT INCLUDED IN MVP:
❌ Real-time data ingestion (batch processing only)
❌ Visual persona cards (text documents only)
❌ Journey mapping (just personas)
❌ Competitive persona comparison (single company focus)
❌ Persona updates over time (snapshot only)
❌ Integration with CRM/marketing platforms
WHY: Core concept is "can we generate data-driven personas from customer data?" Integrations come in future iterations.
 
**Why This Matters**:
- Prevents scope creep
- Sets realistic expectations
- Shows strategic thinking
- Keeps you from burning out
 
## Realistic Scope Check

![Scope Check Image](assets/part4-scope-check.png)

**Can you build this in the time available?**
 
Ask yourself:
- Do I understand what each component does?
- Have I seen examples of similar workflows?
- Are the APIs available and documented?
- Is the processing complexity reasonable?
- Can I test this incrementally?
 
**Red Flags**:
- "I'll figure out the hard parts as I go"
- "I'll probably need custom AI training"
- "This requires data I don't have access to"
- "I've never used any of these technologies"
 
**Green Flags**:
- "I've seen similar n8n examples I can adapt"
- "The APIs have good documentation"
- "I can test each step independently"
- "If GPT-4 doesn't work, I have a Plan B"
 
---

![Excellence Level Image](assets/part4-excellence.png)

## Excellence Level (Technical Architecture)
 
**Get 6 Additional Points By**:
 
**1. Specific n8n Nodes Researched (3 pts)**
 
Don't just name nodes. Show you've researched them.
 
**Example**:  
HTTP Request Node (Twitter API):  
Using Twitter API v2 timeline endpoint  
Bearer token authentication  
Pagination to get 50 tweets (25 per request max)  
Rate limit: 75 requests per 15 min window  
Will implement exponential backoff for rate limit errors

Function Node (Engagement Calculation):  
JavaScript to calculate (likes + retweets) / follower_count  
Handle edge case: division by zero if new account  
Compare to 30-day rolling average per competitor  
Return anomaly score: current / average

OpenAI Node (Trend Analysis):  
GPT-4-turbo model  
Temperature: 0.2 (need consistent analysis)  
System prompt includes industry keyword list  
Returns JSON with matched keywords and relevance score
 
**2. Data Schemas Shown (3 pts)**
 
Define the exact data structure between agents.
 
**Competitor Monitor Example**:
```javascript
// Output from Social Data Collection Agent
{
  "competitor": "string (Twitter handle)",
  "posts": [
	{
  	"id": "string",
  	"text": "string",
  	"timestamp": "ISO-8601 datetime",
  	"metrics": {
    	"likes": "integer",
    	"retweets": "integer",
    	"replies": "integer"
  	},
  	"url": "string"
	}
  ],
  "competitor_baseline": {
	"avg_engagement_rate": "float",
	"follower_count": "integer"
  }
}
 
// Output from Trend Detection Agent
{
  "post_id": "string",
  "strategic_score": "integer (0-100)",
  "flags": {
	"engagement_anomaly": "boolean",
	"trending_keywords": ["array of strings"],
	"sentiment_shift": "boolean"
  },
  "reasoning": "string (why this scored high)"
}
 
// Output from Digest Agent
{
  "digest_date": "ISO-8601 date",
  "high_priority_posts": [
	{
  	"competitor": "string",
  	"text_snippet": "string (first 100 chars)",
  	"score": "integer",
  	"why_important": "string",
  	"url": "string"
	}
  ],
  "alerts_sent": "integer"
}
```

**Content Adapter Example**:

```javascript
// Output from Content Extraction Agent
{
  "source_url": "string",
  "extracted_content": {
	"title": "string",
	"intro": "string",
	"main_points": ["array of strings"],
	"conclusion": "string",
	"word_count": "integer"
  },
  "extraction_quality": "float (0-1)"
}
 
// Output from Platform Adaptation Agent
{
  "original_url": "string",
  "adaptations": {
	"twitter": {
  	"format": "thread",
  	"tweets": ["array of strings"],
  	"total_chars": "integer",
  	"hashtags": ["array of strings"]
	},
	"linkedin": {
  	"format": "single_post",
  	"text": "string",
  	"char_count": "integer",
  	"includes_link": "boolean"
	},
	"instagram": {
  	"format": "caption",
  	"text": "string",
  	"hashtags": ["array of strings"],
  	"emoji_count": "integer"
	}
  }
}
 
// Output from Quality Check Agent
{
  "original_message": "string (core message)",
  "version_scores": {
	"twitter": {
  	"fidelity_score": "integer (0-100)",
  	"issues": ["array of strings or empty"]
	},
	"linkedin": {
  	"fidelity_score": "integer (0-100)",
  	"issues": ["array of strings or empty"]
	},
	"instagram": {
  	"fidelity_score": "integer (0-100)",
  	"issues": ["array of strings or empty"]
	}
  },
  "overall_quality": "integer (0-100)"
}
```

**Why This Matters**:
- Shows you think like an engineer
- Makes implementation much easier
- Helps others understand your architecture
- Demonstrates technical depth

---

| [← Part 3: Your PRD (Product Requirements Document)](./3_Your_PRD.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Module 3 (Data Pipeline) →](../Module%203/README.md) |
|:---|:---:|---:|
