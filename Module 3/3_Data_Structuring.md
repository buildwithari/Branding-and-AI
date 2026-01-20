# Part 3: Data Structuring & Workflow Patterns

![Part 3 Hero](assets/part3-hero.png)

## The Challenge

Structure your collected data so that:
- Your AI agents can process it reliably
- Your data is consistent across all sources
- You can clean and validate quality automatically
- Errors are handled gracefully without crashing

## Why This Matters

**Bad structure = your agents can't process it**
**Good structure = your agents work smoothly**

Think of data structure like organizing a kitchen:
- **Bad**: Ingredients scattered everywhere, unlabeled containers, expired items mixed with fresh
- **Good**: Everything labeled, categorized, easy to find, ready to use

Your AI agent needs the "good kitchen" version of data.

---

## JSON vs CSV: When to Use Each

### Use JSON When:
- Data has nested structures (objects within objects)
- You have varying fields per record
- You're working with API responses
- You need to preserve data types
- You have complex metadata

**Example JSON (Social Media Post):**
```json
{
  "id": "post_123",
  "text": "Just launched our new AI feature!",
  "author": {
    "username": "competitor_co",
    "followers": 15000
  },
  "engagement": {
    "likes": 145,
    "retweets": 32,
    "replies": 8
  },
  "timestamp": "2024-09-15T10:30:00Z",
  "platform": "twitter",
  "collected_at": "2024-09-15T14:22:00Z"
}
```

### Use CSV When:
- Data is flat (no nesting)
- Every record has the same fields
- You want easy viewing in Excel/Google Sheets
- You're doing statistical analysis
- You need simple imports

**Example CSV (Competitor Posts):**
```
id,competitor,text,platform,likes,shares,date,collected_at
1,nike,Just do it message...,twitter,1240,340,2024-09-15,2024-09-15T14:22:00Z
2,nike,New product launch...,twitter,890,156,2024-09-14,2024-09-15T14:22:00Z
3,adidas,Performance innovation...,twitter,670,98,2024-09-15,2024-09-15T14:22:00Z
```

**General Rule**: Use JSON for most workflows (APIs return JSON, easier to work with in n8n).

---

## Essential Fields to Capture

**For Every Record, Include:**

### 1. Unique ID
- **Why**: Helps you track and deduplicate
- **Example**: `post_12345` or `video_abc123` or `article_001`
- **Format**: String, unique per record

### 2. Timestamp (Two Types)
- **created_at**: When the content was originally created/published
- **collected_at**: When YOU collected this data
- **Why**: Track data freshness, identify patterns over time
- **Format**: ISO-8601 (2024-09-15T10:30:00Z)

### 3. Source
- **Why**: Know where data came from
- **Example**: `youtube`, `kaggle_dataset`, `rss_techcrunch`, `reddit_archive`
- **Format**: String, consistent naming

### 4. The Actual Content
- **Why**: This is what you'll analyze
- **Example**: Tweet text, video transcript, blog post body, review text
- **Format**: String (cleaned text)

### 5. Metadata
- **Why**: Provides context for analysis
- **Examples**:
  - Author/brand name
  - Engagement metrics (likes, views, shares)
  - Category/topic tags
  - Platform
  - Language
- **Format**: Appropriate to data type

### Example Well-Structured Record

```json
{
  "id": "video_nike_001",
  "source": "youtube_api",
  "brand": "Nike",
  "title": "Just Do It - 2024 Campaign",
  "transcript": "What are you waiting for? The moment is now...",
  "created_at": "2024-08-15T10:00:00Z",
  "collected_at": "2024-09-10T14:22:00Z",
  "metadata": {
    "views": 1500000,
    "likes": 45000,
    "comments": 3200,
    "duration_seconds": 150,
    "category": "advertisement",
    "word_count": 342
  },
  "url": "https://youtube.com/watch?v=abc123"
}
```

---

## Data Cleaning Best Practices

### What to Clean

**1. Remove Noise**
- HTML tags (if scraping websites)
- URLs (unless you need them)
- Emojis (unless relevant to your analysis)
- Special characters that break processing
- Extra whitespace

**2. Standardize Formats**
- Dates to ISO-8601 (YYYY-MM-DDTHH:MM:SSZ)
- Text to UTF-8 encoding
- Numbers to consistent types (integers vs floats)
- Null values handled consistently
- Field names (lowercase, underscores)

**3. Deduplicate**
- Remove exact duplicates by ID
- Handle near-duplicates intelligently
- Keep track of how many you removed

**4. Validate**
- Check required fields exist
- Verify data types are correct
- Flag suspicious or malformed records
- Don't delete bad data—flag it for review

### n8n Example (Cleaning Function)

```javascript
// Get input items
const items = $input.all();

return items.map(item => {
  // Remove HTML tags
  if (item.json.text) {
    item.json.text = item.json.text.replace(/<[^>]*>/g, '');
  }

  // Standardize date
  if (item.json.date) {
    item.json.date = new Date(item.json.date).toISOString();
  }

  // Remove null/undefined values
  Object.keys(item.json).forEach(key => {
    if (item.json[key] === null || item.json[key] === undefined) {
      delete item.json[key];
    }
  });

  // Trim whitespace from text fields
  if (item.json.text) {
    item.json.text = item.json.text.trim();
  }
  if (item.json.title) {
    item.json.title = item.json.title.trim();
  }

  // Add collection timestamp
  item.json.collected_at = new Date().toISOString();

  // Add word count if text field exists
  if (item.json.text) {
    item.json.word_count = item.json.text.split(/\s+/).length;
  }

  return item;
});
```

---

## General Workflow Patterns

### The Standard Data Collection Pattern

**Most data collection workflows follow this pattern:**

```
1. TRIGGER
   (When should this run?)
   ↓
2. FETCH DATA
   (Pull from source: API, dataset, RSS, scrape)
   ↓
3. PARSE
   (Extract the fields you need)
   ↓
4. CLEAN
   (Remove noise, standardize formats)
   ↓
5. VALIDATE
   (Check quality, flag issues)
   ↓
6. SAVE
   (Store in /data/raw/ as JSON or CSV)
```

**Remember: Run this ONCE, then reuse the saved data.**

---

## Example Workflows

### Example Workflow 1: Public Dataset Processor

**Use Case**: Download and process a Kaggle sentiment dataset

**Nodes Required:**
1. Manual Trigger
2. HTTP Request (Download CSV from URL)
3. Function (Parse CSV, extract fields)
4. Function (Clean and validate)
5. Function (Sample 200 records)
6. Write Binary File (Save to /data/raw/)

**Key Function Node (Parse and Sample):**
```javascript
// Assuming CSV has been parsed
const items = $input.all();

// Sample first 200 records
const sample = items.slice(0, 200);

return sample.map(item => ({
  json: {
    id: `review_${item.json.index}`,
    text: item.json.review_text,
    sentiment: item.json.sentiment_label,
    rating: parseInt(item.json.rating),
    source: 'kaggle_sentiment_dataset',
    created_at: item.json.date || new Date().toISOString(),
    collected_at: new Date().toISOString()
  }
}));
```

**Expected Output:**
- File: /data/raw/sentiment_dataset.json
- Contains: 200 labeled reviews
- Structure: Clean, consistent, ready for analysis

---

### Example Workflow 2: Multi-Source RSS Aggregator

**Use Case**: Collect news from multiple RSS feeds

**Nodes Required:**
1. Manual Trigger
2. RSS Feed Read (Source 1: TechCrunch)
3. RSS Feed Read (Source 2: VentureBeat)
4. RSS Feed Read (Source 3: The Verge)
5. Merge (Combine all feeds)
6. Function (Standardize format)
7. Function (Filter to relevant topics)
8. Write Binary File (Save to /data/raw/)

**Key Function Node (Filter):**
```javascript
// Only keep articles about AI or marketing
const keywords = ['ai', 'artificial intelligence', 'marketing', 'automation'];
const text = ($json.title + ' ' + $json.summary).toLowerCase();

const isRelevant = keywords.some(keyword => text.includes(keyword));

if (isRelevant) {
  return { json: $json };
}
// Return nothing for irrelevant articles (filters them out)
```

**Expected Output:**
- File: /data/raw/industry_news.json
- Contains: 50-80 relevant articles
- Sources: 3 different publications

---

### Example Workflow 3: YouTube Brand Content Collector

**Use Case**: Collect video transcripts from 3 brand channels

**Nodes Required:**
1. Manual Trigger
2. Set (Define list of brands and channel IDs)
3. Loop (Through each brand)
4. HTTP Request (YouTube API - search channel)
5. Function (Extract video IDs, limit to 50)
6. Loop (Through video IDs)
7. HTTP Request (Get video details + captions)
8. Function (Parse transcript and metadata)
9. Function (Clean and structure)
10. Write Binary File (Save to /data/raw/)

**Key Function Node (Parse Transcript):**
```javascript
return {
  json: {
    id: `video_${$json.brand}_${$json.video_id}`,
    source: 'youtube_api',
    brand: $json.brand,
    video_id: $json.video_id,
    title: $json.snippet.title,
    transcript: $json.caption_text || '[No transcript available]',
    created_at: new Date($json.snippet.publishedAt).toISOString(),
    collected_at: new Date().toISOString(),
    metadata: {
      views: parseInt($json.statistics.viewCount) || 0,
      likes: parseInt($json.statistics.likeCount) || 0,
      comments: parseInt($json.statistics.commentCount) || 0,
      duration: $json.contentDetails.duration,
      word_count: $json.caption_text ? $json.caption_text.split(' ').length : 0
    },
    url: `https://youtube.com/watch?v=${$json.video_id}`
  }
};
```

**Expected Output:**
- File: /data/raw/brand_transcripts.json
- Contains: 150 video transcripts (50 per brand)
- Structure: Transcript text, metadata, brand labels

**REMEMBER**: Run this ONCE, save the data, then use the saved file for all subsequent work.

---

## Error Handling & Reliability

### Common Errors and Fixes

**Error 1: API Rate Limits**

**Problem**: API blocks you after too many requests

**Solution**: Add "Wait" nodes between requests
```
HTTP Request
    ↓
Wait (3-5 seconds)
    ↓
Next HTTP Request
```

**Better Solution**: Collect once, save data, never hit rate limits

---

**Error 2: Missing Data**

**Problem**: Some records don't have required fields

**Solution**: Add defaults or filter them out
```javascript
// In Function node
return items.map(item => ({
  ...item.json,
  text: item.json.text || '[No content]',
  transcript: item.json.transcript || '[No transcript available]',
  date: item.json.date || new Date().toISOString(),
  engagement: item.json.engagement || 0,
  // Flag as incomplete if critical field is missing
  data_quality: (!item.json.text || !item.json.date) ? 'incomplete' : 'complete'
}));
```

---

**Error 3: API Timeouts**

**Problem**: Request takes too long, times out

**Solution**: Increase timeout, add retry logic

**In HTTP Request Node:**
- Timeout: 30000 (30 seconds instead of default 10)

**Add Retry Logic:**
```
HTTP Request (with timeout)
    ↓
IF (Check if successful - status code = 200)
    YES → Continue
    NO →
        ↓
        Wait (5 seconds)
        ↓
        HTTP Request (Retry - max 3 times)
```

---

**Error 4: Malformed JSON**

**Problem**: API returns bad JSON that breaks parsing

**Solution**: Add error handling in Function node
```javascript
try {
  const data = JSON.parse($json.response);
  return { json: data };
} catch (error) {
  return {
    json: {
      error: 'Parse failed',
      raw: $json.response,
      timestamp: new Date().toISOString(),
      data_quality: 'invalid'
    }
  };
}
```

---

**Error 5: Network Failures**

**Problem**: Internet connection drops, request fails

**Solution**: Implement retry with backoff
```
HTTP Request
    ↓
IF (Check status code = 200)
    YES → Process data
    NO →
        ↓
        Wait (5 seconds)
        ↓
        HTTP Request (Retry)
        ↓
        IF (Success this time?)
            YES → Process data
            NO → Log error, skip this record, continue workflow
```

---

### Making Your Workflow Robust

**Basic (Required):**
- Workflow runs without crashing
- Collects data successfully most of the time
- Basic error messages if something fails
- Data saved in consistent format

**Better (Recommended):**
- Workflow handles missing data gracefully
- Adds default values where appropriate
- Continues processing even if some records fail
- Logs errors for debugging

**Excellent (Top Tier):**
- Workflow handles API failures gracefully
- Retries on temporary errors (with backoff)
- Validates data quality
- Flags incomplete/invalid records (doesn't delete them)
- Reports success rate and quality metrics
- Professional error logging

---

## Data Quality Measures

### Quality Checks to Implement

**1. Completeness Check**

Are required fields present?
```javascript
// Flag incomplete records
if (!item.json.text || !item.json.date) {
  item.json.quality_flag = 'incomplete';
  item.json.quality_issues = [];
  if (!item.json.text) item.json.quality_issues.push('missing_text');
  if (!item.json.date) item.json.quality_issues.push('missing_date');
}
```

---

**2. Validity Check**

Is the data sensible?
```javascript
// Check engagement isn't negative
if (item.json.likes < 0 || item.json.views < 0) {
  item.json.quality_flag = 'invalid_engagement';
}

// Check date isn't in future
if (new Date(item.json.date) > new Date()) {
  item.json.quality_flag = 'invalid_date';
}

// Check text isn't empty or too short
if (item.json.text && item.json.text.length < 10) {
  item.json.quality_flag = 'text_too_short';
}
```

---

**3. Duplication Check**

Remove duplicates
```javascript
// Simple deduplication by ID
const seen = new Set();
return items.filter(item => {
  if (seen.has(item.json.id)) {
    return false; // Skip duplicate
  }
  seen.add(item.json.id);
  return true; // Keep unique
});
```

---

**4. Relevance Check**

Is this data useful for your problem?
```javascript
// For competitor monitor: filter to industry-relevant posts
const industryKeywords = ['AI', 'marketing', 'automation', 'SaaS'];
const text = item.json.text.toLowerCase();
const isRelevant = industryKeywords.some(keyword =>
  text.includes(keyword.toLowerCase())
);

if (!isRelevant) {
  item.json.quality_flag = 'not_relevant';
}

return isRelevant; // Or keep flagged for review
```

---

### Data Quality Report

**Add this at the end of your workflow to report quality:**
```javascript
// Calculate quality metrics
const items = $input.all();
const total = items.length;
const complete = items.filter(i => i.json.quality_flag !== 'incomplete').length;
const valid = items.filter(i => !i.json.quality_flag).length;

const qualityReport = {
  total_records: total,
  clean_records: valid,
  flagged_records: total - valid,
  quality_rate: ((valid / total) * 100).toFixed(2) + '%',
  timestamp: new Date().toISOString()
};

console.log('Data Quality Report:', qualityReport);
return { json: qualityReport };
```

---

### What "Good Quality" Looks Like

**Minimum Acceptable:**
- 80%+ of records are complete (have all required fields)
- 90%+ of records are valid (pass basic validation)
- 0 duplicate records
- Data structure is consistent

**Strong Quality:**
- 90%+ complete
- 95%+ valid
- All duplicates removed
- Relevant to your problem
- Professional documentation

**Excellent Quality:**
- 95%+ complete
- 98%+ valid
- Deduplication automated
- Relevance filtering applied
- Quality metrics documented
- Issues flagged (not deleted)

**Remember**: 100 high-quality, relevant records >> 1,000 low-quality, messy records

---

## Workflow Building Checklist

**Before You Start:**
- [ ] Identified your data needs (what problem does this solve?)
- [ ] Chosen Tier 1 data sources (public datasets, RSS, YouTube, NewsAPI)
- [ ] Avoided problematic APIs (LinkedIn, Twitter free tier, Reddit live)
- [ ] Planned to collect once and reuse

**While Building:**
- [ ] Started with Manual Trigger (not scheduled)
- [ ] Added data source nodes (RSS Feed, HTTP Request, etc.)
- [ ] Added parsing/cleaning Function nodes
- [ ] Added validation checks
- [ ] Configured Write Binary File to save to /data/raw/
- [ ] Tested workflow 2-3 times

**After Building:**
- [ ] Workflow runs without errors
- [ ] Data saved in consistent format (JSON or CSV)
- [ ] 50-200 clean records collected
- [ ] Required fields present in all records
- [ ] Quality checks implemented
- [ ] Documentation started

**For Excellence:**
- [ ] Error handling implemented
- [ ] Data validation automated
- [ ] Quality metrics calculated
- [ ] Multiple sources integrated (2-3)
- [ ] Professional structure and naming

---

| [← Part 2: Building Your n8n Workflow](./2_Building_your_n8n_workflow.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Part 4: Documentation & Submission →](./4_Documentation_and_Submission.md) |
|:---|:---:|---:|
