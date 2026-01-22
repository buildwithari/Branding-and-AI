# Part 2: Building Your n8n Workflow

![Part 2 Hero](assets/part3-hero.png)

## The Challenge

Build a working n8n workflow that:
- Connects to your chosen data sources
- Collects relevant data for your Madison project
- Structures and cleans the data automatically
- Saves output in a usable format (JSON or CSV)

## Why This Matters

A data pipeline is the plumbing of your AI project. Without it, you're manually copying and pasting data—which doesn't scale and isn't impressive to employers.

**With a working pipeline:**
- Data collection becomes repeatable
- You can refresh data when needed
- You demonstrate real engineering skills
- Your portfolio shows production-quality work

**Without a pipeline:**
- You're stuck doing manual work
- You can't easily update your data
- You have nothing to show employers
- Your project looks like a one-off prototype

---

## What Is n8n?

**Simple Answer**: A visual workflow automation tool that connects different services together.

**What That Means**: Instead of writing code to connect APIs, scrape websites, and process data, you drag and drop "nodes" that do these things and connect them together.

**Why We Use It:**
- Visual = easier to understand and debug
- Pre-built nodes for common tasks (APIs, data processing)
- Perfect for the 80/20 philosophy (powerful without complexity)
- Industry-standard for marketing automation

![n8n Interface](assets/part3-n8n.png)

---

## Installing n8n

**Quick Start:**

1. **Install Node.js** (if you don't have it): nodejs.org
2. **Install n8n** via terminal/command prompt:
```bash
npm install -g n8n
```
3. **Start n8n**:
```bash
n8n start
```
4. **Open in browser**: http://localhost:5678

**If you get errors**: Check that Node.js is installed properly. Most errors are because Node.js isn't in your PATH.

---

![Understanding Nodes](assets/part3-nodes.png)

## Understanding Nodes

### What Are Nodes?

Nodes are building blocks. Each node does ONE thing:
- Fetch data from an API
- Transform data (clean, filter, format)
- Make decisions (if/then logic)
- Save data to files
- Send notifications

### Common Nodes You'll Use

**Data Collection Nodes:**
- **HTTP Request**: Call any API or fetch web content
- **RSS Feed**: Pull from RSS/Atom feeds (easy for blogs/news)
- **Webhook**: Receive data from external sources

**Data Processing Nodes:**
- **Function**: Write custom JavaScript to transform data
- **Code**: Write custom Python for complex processing
- **Set**: Modify or add fields to your data
- **JSON**: Parse or format JSON data

**Control Flow Nodes:**
- **IF**: Make decisions based on conditions
- **Switch**: Route data different ways based on criteria
- **Loop**: Process arrays of data
- **Merge**: Combine data from multiple sources

**Output Nodes:**
- **Write Binary File**: Save data as files (JSON, CSV)
- **Google Sheets**: Write to spreadsheets
- **Email**: Send notifications

---

## Your First Data Collection Workflow

![First Workflow](assets/part3-first-workflow.png)

Let's build a simple RSS news aggregator (Tier 1 - recommended):

**Step 1: Add a Manual Trigger**
- Drag a "Manual Trigger" node onto canvas
- This lets you run the workflow manually

**Step 2: Fetch RSS Data**
- Add "RSS Feed Read" node
- URL: https://techcrunch.com/feed/ (or any RSS feed)
- Set items to fetch: 20

**Step 3: Parse the Response**
- Add "Function" node
- Extract just the data you need (title, summary, link, date)
- Clean up the JSON structure

**Step 4: Save the Data**
- Add "Write Binary File" node
- Format: JSON
- Filename: news_articles.json
- Path: /data/raw/

**Step 5: Test It**
- Click "Execute Workflow"
- Check if data appears
- Open the saved file to verify

**Congratulations! You just built a data pipeline.**

---

## Working with Different Data Sources

### Data Source Type 1: Public Datasets (TIER 1 - START HERE)

**Best For**: Almost everything. Training data, baseline metrics, quick prototyping.

**Why Start Here:**
- No authentication hassles
- No rate limits
- Already clean and structured
- Large volumes available
- Free and legal

**Where to Find:**

| Platform | Best For | How to Search |
|----------|----------|---------------|
| Kaggle | Marketing, Social Media, Sentiment | kaggle.com/datasets |
| HuggingFace | NLP, Text Analysis | huggingface.co/datasets |
| Google Dataset Search | Everything | datasetsearch.research.google.com |
| GitHub | Niche datasets | Search "[topic] dataset" |
| Data.gov | Government data | data.gov |

**n8n Approach for Public Datasets:**
```
Manual Trigger
    ↓
HTTP Request (Download dataset from URL)
    ↓
Function (Parse CSV/JSON)
    ↓
Function (Sample/filter if dataset is huge)
    ↓
Write Binary File (Save processed subset)
```

**Example Function Node (Parse and Sample):**
```javascript
// Parse CSV and take first 200 records
const data = $input.all();
const parsed = parseCSV(data); // Use appropriate parser
const sample = parsed.slice(0, 200); // Take subset
return sample.map(item => ({
  json: {
    id: item.id,
    text: item.text,
    label: item.label,
    source: 'kaggle_dataset',
    collected_at: new Date().toISOString()
  }
}));
```

**Pro Tips:**
- Start with datasets that match your exact use case
- Look for "cleaned" or "preprocessed" in the title
- Check the license (most are open/public)
- Download once, save locally, never re-download

---

### Data Source Type 2: RSS Feeds (TIER 1 - HIGHLY RECOMMENDED)

**Best For**: News aggregation, blog monitoring, content curation

**Why It's Great:**
- Structured data (easier than scraping)
- Designed to be consumed programmatically
- No authentication needed
- No rate limits
- Fast and reliable

**n8n Approach:**
```
Manual Trigger
    ↓
RSS Feed Read (TechCrunch)
    ↓
RSS Feed Read (VentureBeat)
    ↓
RSS Feed Read (Your Industry Blog)
    ↓
Merge (Combine all feeds)
    ↓
Function (Standardize format, clean data)
    ↓
Write Binary File (Save articles)
```

**Example Function Node (Standardize):**
```javascript
return {
  json: {
    title: $json.title,
    summary: $json.description || $json.summary,
    url: $json.link,
    source: $json.feed_title || 'TechCrunch',
    published: new Date($json.pubDate).toISOString(),
    category: $json.category || 'general',
    collected_at: new Date().toISOString()
  }
};
```

**Finding RSS Feeds:**
- Most blogs have RSS (look for RSS icon or /feed/ in URL)
- Try: websiteurl.com/rss or websiteurl.com/feed
- Use Feedly to discover feeds in your industry
- News sites almost always have RSS

**Pro Tips:**
- Collect from multiple feeds to get diversity
- RSS is perfect for "collect once, reuse forever"
- No authentication = no headaches
- Can run daily to accumulate data over time

---

### Data Source Type 3: YouTube Data API (TIER 1 - RECOMMENDED)

![YouTube API](assets/part3-youtube.png)

**Best For**: Brand analysis projects, video content analysis, transcript collection

**Why It's Valuable:**
- Video transcripts reveal authentic brand voice
- Engagement metrics show what resonates
- Multiple content types (ads, product videos, educational)
- Rich metadata (views, likes, comments, publish dates)
- Generous free tier: 10,000 quota units/day

**YouTube API Setup:**

**1. Get API Credentials:**
- Go to Google Cloud Console (console.cloud.google.com)
- Create new project
- Enable YouTube Data API v3
- Create credentials (API key)
- Free tier: 10,000 quota units/day (~100-150 video requests)

**2. Key Endpoints:**
- **Search**: Find videos by channel, keyword, or topic
- **Videos**: Get video details and statistics
- **Captions**: Download transcript/caption files

**n8n YouTube Workflow:**
```
Manual Trigger
    ↓
Set (Define list of brand channel IDs)
    ↓
Loop (Through each brand)
    ↓
HTTP Request (YouTube API - search channel)
    ↓
Function (Extract video IDs)
    ↓
Loop (Through video IDs - limit to 50 per brand)
    ↓
HTTP Request (Get video details + captions)
    ↓
Function (Parse transcript and metadata)
    ↓
Write Binary File (Save brand transcripts)
```

**Example HTTP Request (Search Videos):**
- Method: GET
- URL: https://www.googleapis.com/youtube/v3/search
- Parameters:
  - part: snippet
  - channelId: {{ $json.channelId }}
  - maxResults: 50
  - order: date
  - type: video
  - key: [Your API key]

**Example Function Node (Structure Transcript):**
```javascript
return {
  json: {
    video_id: $json.id,
    brand: $json.brand,
    title: $json.snippet.title,
    transcript: $json.caption_text || '[No transcript available]',
    published_date: new Date($json.snippet.publishedAt).toISOString(),
    view_count: parseInt($json.statistics.viewCount) || 0,
    like_count: parseInt($json.statistics.likeCount) || 0,
    comment_count: parseInt($json.statistics.commentCount) || 0,
    duration: $json.contentDetails.duration,
    video_url: `https://youtube.com/watch?v=${$json.id}`,
    word_count: $json.caption_text ? $json.caption_text.split(' ').length : 0,
    collected_at: new Date().toISOString()
  }
};
```

**Pro Tips for YouTube Data:**
- Start with one brand's channel to test
- Not all videos have captions (handle this gracefully)
- Quota limits reset daily at midnight Pacific
- Save video IDs so you can re-fetch if needed
- 50-100 videos per brand is plenty for analysis

---

### Data Source Type 4: NewsAPI (TIER 1 - RECOMMENDED)

**Best For**: News aggregation, industry monitoring, current events

**Why It's Valuable:**
- Clean, structured news data
- Multiple sources aggregated
- Free tier: 100 requests/day (sufficient for most projects)
- Easy authentication
- Well-documented

**NewsAPI Setup:**

**1. Get API Key:**
- Go to newsapi.org
- Sign up for free account
- Get API key
- Free tier: 100 requests/day, 30-day history

**2. Key Endpoints:**
- **Everything**: Search all articles
- **Top Headlines**: Breaking news
- **Sources**: List available sources

**n8n NewsAPI Workflow:**
```
Manual Trigger
    ↓
HTTP Request (NewsAPI - search articles)
    ↓
Function (Parse and filter articles)
    ↓
Function (Clean and structure data)
    ↓
Write Binary File (Save articles)
```

**Example HTTP Request Configuration:**
- Method: GET
- URL: https://newsapi.org/v2/everything
- Parameters:
  - q: "artificial intelligence marketing" (your search query)
  - language: en
  - sortBy: publishedAt
  - pageSize: 100
  - apiKey: [Your API key]

**Example Function Node (Parse):**
```javascript
const articles = $json.articles;
return articles.map(article => ({
  json: {
    title: article.title,
    description: article.description,
    content: article.content,
    url: article.url,
    source: article.source.name,
    author: article.author,
    published: new Date(article.publishedAt).toISOString(),
    collected_at: new Date().toISOString()
  }
}));
```

**Pro Tips:**
- 100 requests/day = 100 articles/day (plenty for most projects)
- Use specific search queries to filter relevance
- Combine with RSS feeds for more sources
- Free tier is sufficient for learning projects

---

### Data Source Type 5: Web Scraping (TIER 1 - USE CAREFULLY)

![Web Scraping](assets/part3-scraping.png)

**Best For**: Websites without APIs, public data, competitor content

**When to Use:**
- No API available
- Public data you can legally access
- Need specific website content
- RSS feed not available

**n8n Approach:**
```
Manual Trigger
    ↓
HTTP Request (Fetch blog homepage)
    ↓
HTML Extract (Get all post links)
    ↓
Loop (Through first 20-30 post links)
    ↓
HTTP Request (Fetch each post)
    ↓
HTML Extract (Get title, content, date)
    ↓
Function (Clean HTML, extract text)
    ↓
Write Binary File (Save all posts)
```

**Example HTML Extract Node (Get Content):**
- Multiple selectors:
  - Title: h1.post-title
  - Content: div.post-content
  - Date: time.post-date

**Example Function Node (Clean):**
```javascript
return {
  json: {
    title: $json.title.trim(),
    content: $json.content.replace(/<[^>]*>/g, '').trim(),
    date: new Date($json.date).toISOString(),
    url: $json.url,
    word_count: $json.content.split(' ').length,
    collected_at: new Date().toISOString()
  }
};
```

**Legal & Ethical Notes:**
- Check robots.txt (website's scraping rules)
- Read terms of service
- Don't overload servers (add delays between requests)
- Only scrape public data
- Respect copyright

**Pro Tips:**
- Add 2-5 second delays between requests
- Scrape once, save locally, never re-scrape
- Start with small batches (10-20 pages)
- Use browser inspector to find CSS selectors

---

### Data Source Type 6: Simulated Data (TIER 1 - WHEN APPROPRIATE)

**Best For**: Testing, prototyping, demonstrating concepts, when real data unavailable

**When It's Appropriate:**
- You don't have access to real customer data
- Real data is private/sensitive
- You need specific patterns to test against
- You're prototyping before real integration

**How to Generate:**
- Use GPT-4 to generate realistic examples
- Use Python libraries (Faker for fake data)
- Use spreadsheet formulas for patterns
- Combine real structures with fake content

**Example n8n Workflow:**
```
Manual Trigger
    ↓
Code Node (Python: generate 100 realistic support tickets)
    ↓
Function (Structure as JSON with ticket ID, category, sentiment)
    ↓
Write Binary File (Save support_tickets.json)
```

**IMPORTANT:**
- **Always label simulated data clearly**
- Don't pass it off as real
- Document in your data inventory that it's simulated
- Use for testing and demonstration only

---

### Data Source Type 7: Twitter/Reddit Archives (TIER 2 - USE CAUTION)

**Best For**: When you need social media data but can't use live APIs

**RECOMMENDED APPROACH: Use Archives, NOT Live APIs**

**Twitter Archives:**
- Search Kaggle for "Twitter dataset"
- Many pre-collected, cleaned datasets available
- No API limits, no auth needed
- Example datasets: "Twitter Sentiment Analysis Dataset", "Twitter US Airline Sentiment"

**Reddit Archives:**
- Pushshift Reddit datasets
- Available on Archive.org and academic repositories
- Search Kaggle for "Reddit comments dataset"
- No risk of IP bans

**n8n Approach for Archives:**
```
Manual Trigger
    ↓
HTTP Request (Download archived dataset)
    ↓
Function (Parse and filter)
    ↓
Function (Sample subset - e.g., 200 records)
    ↓
Write Binary File (Save filtered data)
```

**Why Use Archives Instead of Live APIs:**
- No rate limits
- No authentication hassles
- No risk of bans
- Already collected and often cleaned
- Can sample exactly what you need

---

## Quick Start Templates by Project Type

### For Brand Analysis / Voice Detection Projects

**Recommended Stack (Tier 1 only):**
1. YouTube Data API (for video transcripts)
2. Public brand datasets from Kaggle
3. RSS feeds from brand blogs
4. Web scraping (brand About pages)

**Expected Output**: 150-200 pieces of branded content

---

### For News/Content Aggregation Projects

**Recommended Stack (Tier 1 only):**
1. RSS feeds (primary)
2. NewsAPI
3. Public news datasets from Kaggle

**Expected Output**: 100-200 industry articles

---

### For Customer/Sentiment Analysis Projects

**Recommended Stack (Tier 1 only):**
1. Public sentiment datasets from Kaggle (PRIMARY)
2. Reddit archives (Pushshift)
3. Amazon reviews datasets
4. Simulated data if needed

**Expected Output**: 150-200 labeled examples

---

| [← Part 1: Data Source Selection](./1_Data_Source_Selection.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Part 3: Data Structuring →](./3_Data_Structuring.md) |
|:---|:---:|---:|
