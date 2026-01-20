# Chapter 3: Data Pipeline

## Shopping for Ingredients Before You Cook

![Hero Image](assets/hero.png)

## What You'll Learn

By the end of this chapter, you will:
- Build a working n8n workflow that collects real data
- Connect to 2-3 different data sources (prioritizing reliable Tier 1 sources)
- Structure and save data in formats ready for analysis
- Handle common errors and edge cases
- Document your work so others can reproduce it
- Understand data quality and why it matters

**Time Investment**: 10-12 hours
**Deliverable**: A working data pipeline + comprehensive documentation

---

## Why This Matters

You've planned your Madison agent. You know exactly what problem you're solving. Now it's time to collect the data your agent will need.

**Here's the reality:**
- Your AI agent is only as good as the data it learns from
- Garbage in = garbage out
- Clean, relevant data = intelligent agents
- This chapter is about getting good ingredients before you cook

Most students jump straight into building AI features without solid data foundations. They:
- Build analysis on bad data (results are meaningless)
- Realize halfway through they need different data
- Can't test their agents properly
- Have nothing meaningful to demonstrate

**Those who prioritize quality data collection?** They build faster later because they have solid foundations.

![Planning Image](assets/planning.png)

## The Core Philosophy: Quality Over Quantity

**You don't need:**
- Perfect data
- Millions of records
- Every possible data source
- Complex ETL pipelines

**You DO need:**
- Relevant data that addresses your problem
- Enough data for meaningful patterns (50-100 clean records is sufficient)
- Clean structure that your agents can process
- Documentation so you can reproduce it

**Remember the 80/20 rule**: 80% of your value comes from getting good, relevant data. The other 20% is optimization and polish.

**NEW PHILOSOPHY: Quality beats quantity every time.**

50-100 clean, relevant, well-structured records > 1,000 messy, inconsistent records

## Table of Contents

1. [Part 1: Data Source Selection](1_Data_Source_Selection.md)
2. [Part 2: Building Your n8n Workflow](2_Building_your_n8n_workflow.md)
3. [Part 3: Data Structuring & Workflow Patterns](3_Data_Structuring.md)
4. [Part 4: Documentation & Submission](4_Documentation_and_Submission.md)

## Your Deliverable

### What You're Creating

A working data pipeline with comprehensive documentation that includes:

**1. Working n8n Workflow**
- Collects data from 2-3 Tier 1 sources
- Cleans and structures data consistently
- Saves output in JSON or CSV format
- Runs without major errors

**2. Data Inventory**
- What data you collected
- Where it came from (sources)
- How much (volume and quality metrics)
- Why it's relevant to your project

**3. Setup Guide**
- Step-by-step instructions to run your workflow
- Required credentials/API keys
- Common errors and fixes
- Expected output

**4. Demo Documentation**
- Video walkthrough (3 minutes) OR
- Screenshot walkthrough (5-8 annotated screenshots)

### Data Volume Standards

**Minimum Viable (Required):**
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

**IMPORTANT:** Notice that "excellence" is 150-300 records, NOT 1,000+. We prioritize quality over fighting with APIs.

![Figma Board Image](assets/figma-board.png)

## Success Criteria

You'll know you've succeeded when:

✅ Your workflow runs without major errors
✅ You have 50-200 clean, relevant records
✅ Data is structured consistently (JSON or CSV)
✅ Someone else could follow your documentation and reproduce your results
✅ Quality metrics are documented (completeness, validity rates)
✅ Your data is ready for the next phase (adding AI intelligence)

![Completion Image](assets/completion.png)

## Common Pitfalls & How to Avoid Them

### Pitfall #1: Using Problematic APIs
**Symptom**: Spending hours fighting rate limits, getting IP banned, complex authentication
**Fix**: Start with Tier 1 sources (public datasets, RSS feeds, YouTube API, NewsAPI). Avoid LinkedIn API, Twitter free tier, and live Reddit scraping entirely.

### Pitfall #2: Prioritizing Quantity Over Quality
**Symptom**: "I need 1000+ records to be impressive"
**Fix**: 100 clean, relevant records is better than 1000 messy ones. Quality beats quantity.

### Pitfall #3: Re-fetching Data During Development
**Symptom**: Running your data collection workflow every time you test
**Fix**: Collect once, save locally, then reuse that data for all testing. This prevents rate limits and bans.

### Pitfall #4: Skipping Documentation
**Symptom**: "I'll document it later" (you won't)
**Fix**: Document as you go. Take screenshots while building. Write notes immediately.

### Pitfall #5: Over-Engineering the Workflow
**Symptom**: 15+ nodes, complex branching, handling every edge case
**Fix**: Simple is fine. 4-6 nodes that work reliably beats a complex workflow that breaks.

### Pitfall #6: No Error Handling
**Symptom**: Workflow crashes on first missing field or API error
**Fix**: Add basic null checks and default values. Handle common errors gracefully.

![Pitfalls Image](assets/pitfalls.png)

## Pro Tips from the Instructors

**From Nik (AI engineering)**:

> "The data pipeline is the unglamorous part of AI work, but it's where most projects fail. I've seen brilliant AI models trained on garbage data—they produce garbage results. Spend the time here, and everything downstream gets easier."

**From Nina (40 years in creative)**:

> "Documentation is storytelling for technical work. If you can't explain what you collected and why, you haven't finished the job. Clear documentation shows professionalism—it's what separates students from practitioners."

**On Data Sources**:

> "Start with public datasets. Seriously. Kaggle has thousands of clean, structured datasets that are perfect for learning. Don't waste days fighting Twitter's API when you could download a Twitter dataset in 5 minutes."

**On Quality**:

> "100 clean records that you understand completely > 1000 messy records you haven't looked at. Know your data. Open the files. Look at what you collected. Quality problems caught early are easy to fix."

**On the Golden Rule**:

> "Collect once, reuse forever. Every time you re-run your data collection during testing, you risk rate limits and bans. Save your data locally after the first successful run, then work with that saved data."

![Pro Tips Image](assets/pro-tips.png)

## Real Student Examples

### Example 1: Brand Voice Analysis (Excellent)

**Data Collected:**
- 180 records total
- Source 1: YouTube transcripts (120 videos from 3 brands via YouTube API)
- Source 2: Public brand content dataset from Kaggle (60 additional samples)
- Quality: 95% complete rate

**Why It Worked:**
- Used only Tier 1 sources (YouTube API + Kaggle)
- Quality over quantity (180 clean records, not 1000+ messy)
- Professional documentation
- Error handling for missing transcripts

### Example 2: Industry News Aggregator (Excellent)

**Data Collected:**
- 156 records total
- Source 1: RSS feeds from 5 tech publications (120 articles)
- Source 2: NewsAPI for supplemental sources (36 articles)
- Quality: 100% complete rate

**Why It Worked:**
- Tier 1 sources only (RSS + NewsAPI)
- No authentication headaches
- Relevance filtering applied
- Clean, consistent structure

### Example 3: What NOT to Do (Poor)

**Data Attempted:**
- Tried to collect 2000+ tweets via free Twitter API
- Got rate limited after 50 tweets
- Tried Reddit API, got IP banned
- Ended with 63 messy, incomplete records

**What Went Wrong:**
- Used all Tier 3 sources (problematic APIs)
- Ignored warnings about rate limits
- Prioritized quantity over quality
- No backup plan when APIs failed

![Student Examples Image](assets/student-examples.png)

## Tools & Resources

### Data Sources (Tier 1 - Recommended)
- **Kaggle Datasets**: kaggle.com/datasets
- **HuggingFace Datasets**: huggingface.co/datasets
- **Google Dataset Search**: datasetsearch.research.google.com
- **Data.gov**: Government datasets
- **RSS Feeds**: Most blogs and news sites have RSS

### APIs (Tier 1 - Recommended)
- **YouTube Data API**: developers.google.com/youtube/v3
- **NewsAPI**: newsapi.org/docs

### n8n Resources
- **Official Docs**: docs.n8n.io
- **Community Workflows**: n8n.io/workflows
- **Forum**: community.n8n.io

### Data Processing
- **JSON Formatting**: jsonformatter.org
- **CSV Tools**: Google Sheets, Excel
- **Regex Testing**: regex101.com

![Tools Image](assets/tools.png)

## Next Steps

Once you complete your data pipeline:

1. **Submit your deliverables** (workflow JSON, data files, documentation)
2. **Review feedback** from your TA
3. **Prepare for Chapter 4**: You'll add AI intelligence to process this data

Remember: This data collection work saves you hours later. The students who rush past this? They rebuild 2-3 times. The students who collect quality data? They ship on the first try.

**The students who nail this chapter?** They have clean, structured data ready for AI processing.

**The students who skip this chapter?** They spend weeks debugging why their AI outputs garbage.

Which one will you be?

![Journey Image](assets/journey.png)

## Quick Start Checklist

Use this checklist to make sure you've completed everything:

### Data Source Selection
- [ ] Reviewed Tier 1 data sources (public datasets, RSS, YouTube, NewsAPI)
- [ ] Avoided Tier 3 sources (LinkedIn, Twitter free tier, Reddit live)
- [ ] Identified 2-3 sources relevant to your project
- [ ] Planned to collect once and reuse

### n8n Workflow
- [ ] Installed n8n successfully
- [ ] Built workflow with data collection nodes
- [ ] Added data cleaning/structuring functions
- [ ] Configured output to save JSON or CSV
- [ ] Tested workflow 3+ times
- [ ] Exported workflow JSON file

### Data Quality
- [ ] Collected 50-200 clean records
- [ ] Records have consistent structure
- [ ] Required fields present (id, timestamp, source, content)
- [ ] Duplicates removed
- [ ] Quality metrics calculated

### Documentation
- [ ] Data inventory completed
- [ ] Setup guide written with step-by-step instructions
- [ ] Common errors documented with fixes
- [ ] Demo created (video OR screenshots)

### Final Checks
- [ ] Workflow runs without major errors
- [ ] Data saved in /data/raw/ directory
- [ ] All files organized in proper folder structure
- [ ] Someone else could reproduce your work

**Total Time**: 10-12 hours
**Impact**: Solid data foundation for all future chapters
**Value**: Data engineering skills that employers actually value

*Remember: Ship it clean, not complicated.*

---

| [← Chapter 2: Madison Planning](../Module%202/README.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Part 1: Data Source Selection →](./1_Data_Source_Selection.md) |
|:---|:---:|---:|
