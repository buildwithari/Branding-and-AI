# Part 1: Data Source Selection

![Part 1 Hero](assets/part2-hero.png)

## The Challenge

Choose data sources that will:
- Provide relevant data for your Madison project
- Be accessible without complex authentication hurdles
- Not get you rate limited or banned
- Give you enough clean data for meaningful analysis

## Why This Matters

Your data source selection can make or break your project. Choose well, and you'll collect clean data in hours. Choose poorly, and you'll spend days fighting APIs, getting banned, and starting over.

**The students who succeed**: Start with public datasets and RSS feeds, avoid problematic APIs, collect once and reuse.

**The students who struggle**: Jump straight to Twitter/LinkedIn APIs, get rate limited, waste days debugging, end up with messy data.

**Your goal**: Be in the first group.

![Data Sources Image](assets/part2-sources.png)

---

## Critical Warning: Choose Your Data Sources Wisely

**Before you start building, read this carefully.** Many data sources will cause you frustration, wasted time, and potential account bans.

### Tier 3: Problematic APIs to AVOID

**LinkedIn API** - Requires business account, complex approval process, and can be expensive. Most learners cannot access it. **Skip this entirely.**

**Twitter/X API (Free Tier)** - Severe rate limiting that will frustrate you. The free tier sounds generous (500K tweets/month) but you'll hit limits quickly with even modest data collection. Rate limits trigger after just 50-100 requests in practice. **Avoid unless you have a paid account.**

**Reddit API (Live Scraping)** - **EXTREMELY PROBLEMATIC.** Known to ban IP addresses mid-project, even when following their rules. Works fine in development, then fails in production. Once banned, you may lose access for future accounts from that IP. **Never use live Reddit API scraping. Use Reddit archives or public datasets instead.**

**Any API requiring business verification** - If it requires company credentials or business account approval, skip it. You're here to learn, not fight bureaucracy.

---

### Tier 1: Recommended Data Sources (Start Here)

![Tier 1 Sources](assets/part2-tier1.png)

**These are reliable, accessible, and won't ban you:**

**1. Public Datasets (PRIMARY RECOMMENDATION)**
- **Kaggle Datasets** - Thousands of clean, structured datasets. Marketing, customer data, sentiment analysis datasets all available.
- **HuggingFace Datasets** - Excellent for NLP and text data. Pre-labeled, high quality.
- **Google Dataset Search** - Find datasets by topic across the web.
- **Data.gov** - Government datasets, often very clean and well-documented.
- **GitHub** - Many repos share marketing and social media datasets.

**Why start here**: Zero authentication, no rate limits, large volumes, already clean and structured.

**Example datasets to search for:**
- "Twitter sentiment analysis dataset" on Kaggle
- "Social media posts dataset" on HuggingFace
- "Customer reviews dataset" on Kaggle
- "Brand mentions dataset" on GitHub
- "Marketing campaign data" on Data.gov

**2. RSS Feeds**
- Built for programmatic access
- No authentication needed
- No rate limits
- Perfect for news aggregation and blog monitoring
- Examples: TechCrunch, VentureBeat, industry blogs

**3. YouTube Data API**
- Well-documented with excellent Google Cloud Console support
- Generous free tier: 10,000 quota units/day (~100-150 video requests)
- Stable and reliable
- Great for brand voice analysis via video transcripts

**4. NewsAPI**
- Clean news aggregation API
- Free tier: 100 requests/day (sufficient for most projects)
- No complicated auth process
- Good documentation

---

### Tier 2: Use with Extreme Caution

**Only use these if you have a specific need AND understand the limitations:**

**Twitter/X API (with heavy warnings)**
- If you must use it: Keep collections SMALL (<50 records per day)
- Add 5-10 second delays between requests
- Have a backup plan (use a public Twitter dataset instead)
- Expect to hit rate limits quickly
- **Better alternative**: Use pre-existing Twitter datasets from Kaggle

**Reddit Archives (NOT live API)**
- Use archived Reddit datasets available on Kaggle or Pushshift archives
- Never scrape Reddit live
- Reddit archives are safe and won't get you banned
- Search for "Reddit comments dataset" or "Reddit submissions dataset"

---

## The Golden Rule: Collect Once, Reuse Forever

![Collect Once](assets/part2-golden-rule.png)

**CRITICAL WORKFLOW PRINCIPLE:**

**Run your data collection ONCE, save everything locally, then reuse that data for all testing and development.**

### Why This Matters

Re-scraping or re-fetching data every time you test your workflow will:
- Trigger API rate limits within hours
- Get your IP address banned
- Waste hours of your time debugging API errors
- Potentially lose access to your account permanently

### Best Practice Workflow

**Phase 1: Collection (Do Once)**
```
1. Run your n8n workflow
2. Save output to: /data/raw/collected_data.json
3. Commit to version control
4. STOP HERE - Do not re-run
```

**Phase 2: Development (Use Saved Data)**
```
For all subsequent testing:
1. Load from: /data/raw/collected_data.json
2. Process, analyze, iterate
3. Never re-fetch from API
```

**Phase 3: Only Re-collect When:**
- You need fresh/updated data
- You're adding a new data source
- Your original data was flawed

### Implementation in n8n

**Bad (Re-fetches every time):**
```
Manual Trigger → HTTP Request (API) → Process → Save
```

**Good (Collect once, reuse):**
```
Collection workflow (run ONCE):
Manual Trigger → HTTP Request (API) → Save to /data/raw/

Development workflow (run many times):
Manual Trigger → Read from /data/raw/ → Process → Analyze
```

---

## Data Volume: What's Actually Enough?

**Minimum Viable (Absolutely Required):**
- 50-100 clean records
- From 2 different sources
- Structured consistently
- Documented clearly

**Strong Quality (Recommended):**
- 100-200 clean records
- From 2-3 sources
- Validated and deduplicated
- Error handling implemented

**Excellence (Top Tier):**
- 150-300 clean, relevant records
- From 3+ sources
- Automated cleaning and validation
- Professional documentation
- Quality metrics documented

**IMPORTANT:** Notice that "excellence" is 150-300 records, NOT 1,000+. We've adjusted expectations to prioritize quality over fighting with APIs.

---

## Understanding Your Data Needs

### What Data Does Your Project Actually Need?

**Before you start collecting, answer these questions:**

1. **What problem are you solving?**
2. **What data would help solve it?**
3. **Where does that data live?**
4. **Can you access it legally and ethically?**

---

## Data Needs by Project Type

![Project Types](assets/part2-project-types.png)

### Project Type 1: Competitor Social Monitor

**Problem**: Marketing teams spend 10-15 hours/week manually checking competitor social accounts

**Data Needed:**
- Competitor posts (text, timestamps, engagement)
- Engagement metrics (likes, shares, comments)
- Posting patterns (frequency, timing)
- Historical baseline (to detect anomalies)

**Recommended Approach (Tier 1):**
- **Public Twitter Dataset** from Kaggle
- **YouTube Data API** for video content analysis
- **RSS feeds** from competitor blogs

**Avoid:** Live Twitter API scraping, LinkedIn API, Live Reddit API

**Minimum Viable Data:** 50-100 posts total from 3-5 competitors

---

### Project Type 2: Multi-Platform Content Adapter

**Problem**: Content teams spend 30-45 minutes reformatting blog posts for different platforms

**Data Needed:**
- Example blog posts (to test adaptation)
- Platform-specific best practices (character limits, formats)
- Successful posts per platform (to learn patterns)

**Recommended Approach (Tier 1):**
- **RSS feeds** from target blogs (easy, no auth)
- **Public social media datasets** from Kaggle
- **YouTube Data API** for video descriptions

**Minimum Viable Data:** 50-75 total examples

---

### Project Type 3: Synthetic Persona Generator

**Problem**: Companies make targeting decisions without data-driven customer understanding

**Data Needed:**
- Customer feedback (surveys, interviews)
- Behavioral data (what they do)
- Demographic data (who they are)
- Pain points and goals (what they need)

**Recommended Approach (Tier 1):**
- **Public customer survey datasets** from Kaggle
- **Reddit archives** (Pushshift datasets, NOT live scraping)
- **Simulated data** (GPT-4 generated, clearly labeled)
- **CSV exports** from survey tools if you have access

**Avoid:** Live Reddit API scraping

**Minimum Viable Data:** 100-150 total data points

---

### Project Type 4: Industry News Aggregator

**Problem**: Marketers manually check 10+ sources daily for industry news

**Data Needed:**
- Industry news articles
- Publication sources
- Article metadata (date, author, topic)
- Engagement signals (if available)

**Recommended Approach (Tier 1):**
- **RSS feeds** (easiest and best!)
- **NewsAPI** (free tier: 100 requests/day)
- **Public news datasets** from Kaggle

**Minimum Viable Data:** 100-200 articles from 5-10 sources

---

### Project Type 5: Brand Voice Analysis / Detection

**Problem**: Brands need to maintain consistent voice across content

**Data Needed:**
- Brand content samples (various formats)
- Multiple brands for comparison
- Metadata (engagement, context, platform)

**Recommended Approach (Tier 1):**
- **YouTube Data API** (video transcripts reveal authentic brand voice)
- **Public brand content datasets** from Kaggle/GitHub
- **RSS feeds** from brand blogs
- **Web scraping** (brand websites, About pages)

**Avoid:** Live Twitter scraping, LinkedIn API

**Minimum Viable Data:** 100-200 content pieces from 3-5 brands

---

### Project Type 6: Sentiment Analysis Tool

**Problem**: Brands don't know how customers feel about them in real-time

**Data Needed:**
- Customer mentions (social, reviews)
- Context around mentions
- Sentiment labels (for training/validation)
- Competitor mentions (for comparison)

**Recommended Approach (Tier 1):**
- **Public sentiment datasets** from Kaggle
- **Amazon reviews datasets** (publicly available)
- **Reddit archives** (Pushshift, NOT live scraping)
- **YouTube comments** via YouTube API

**Avoid:** Live Twitter scraping, Live Reddit scraping

**Minimum Viable Data:** 150-200 labeled examples

---

## Quick Reference: Data Source Decision Tree

**START HERE:**
1. Search Kaggle for relevant public datasets
2. Search HuggingFace for text/NLP datasets
3. Search Google Dataset Search for your topic

**If you need fresh/current data:**
4. Set up RSS feeds (no auth, no limits)
5. Use YouTube Data API (generous limits, well-documented)
6. Use NewsAPI (100 requests/day, sufficient)

**Only if absolutely necessary:**
7. Twitter datasets (Kaggle archives, NOT live API)
8. Reddit archives (Pushshift, NOT live API)

**NEVER USE:**
- LinkedIn API
- Live Twitter scraping (unless you have paid access)
- Live Reddit scraping
- Any API requiring business verification

---

| [← Module 3: Data Pipeline](./README.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Part 2: Building Your n8n Workflow →](./2_Building_your_n8n_workflow.md) |
|:---|:---:|---:|
