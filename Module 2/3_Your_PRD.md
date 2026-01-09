# Part 3: Your PRD (Product Requirements Document)

![Hero Image](assets/part3-hero.png)

## What Is a PRD?
 
**Simple Answer**: A document that explains what you're building and why.
 
**Real Answer**: The document that prevents you from building the wrong thing.
 
**Why It Matters**:
- Every tech company uses PRDs
- Product managers write them constantly
- "Can you write a PRD?" is literally a job interview question
- Having PRD examples in your portfolio = instant credibility
 
## The Four Sections
 
Every PRD has the same basic structure. Master this, and you can write PRDs for any project.
 
---

![Problem Statement Image](assets/part3-problem.png)

## Section 1: Problem Statement (10 points)
 
**The Goal**: Convince someone the problem is worth solving.
 
**The Three Questions**:
 
**1. What specific problem does your target company/role face?**
 
Not: "Marketing is hard"   
But: "Marketing teams at B2B SaaS companies spend 10-15 hours per week manually monitoring competitor social accounts to identify trending topics and competitive moves"
 
**Why Specific Matters**:
- Shows you understand the actual problem
- Gives you measurable success criteria
- Makes it clear who benefits
 
**2. Who experiences this problem?**
 
Not: "Companies"   
But: "Social media managers at Series B+ B2B SaaS companies competing in crowded markets where being first to trending topics matters"
 
**Why This Matters**:
- Defines your target user
- Helps you make better design decisions
- Shows you understand the market
 
**3. What's the cost of not solving it?**
 
Quantify the pain. Use numbers.
 
Examples:
- **Time**: "10-15 hours per week = $25,000-40,000 per year in labor costs"
- **Opportunity**: "Missing trending topics means missed engagement opportunities worth $X"
- **Competitive**: "Competitors respond to industry news faster, appearing more thought-leadership positioned"
 
**Why Cost Matters**:
- Proves it's worth building
- Gives you ROI calculations
- Shows business thinking (not just technical)
 
**Good Problem Statement Examples**:
 
**Example 1: Social Media Monitoring**
 
"Marketing teams at growth-stage B2B SaaS companies (Series B, 50-200 employees) struggle to monitor competitors effectively. Social media managers spend 10-15 hours per week manually checking 5-10 competitor accounts across Twitter, LinkedIn, and industry forums to identify competitive moves, trending topics, and messaging changes.
 
At $60-80K salaries, that's $15,000-25,000 annually in labor costs just for monitoring. More critically, manual monitoring means delays—by the time they see a competitor's viral post about an industry trend, it's too late to join the conversation. Missing these moments costs thought leadership positioning and engagement opportunities.
 
The real cost: competitors who automate this appear more responsive, more informed, and more industry-leading—all because they see and react faster."
 
**Example 2: Content Repurposing**
 
"Content marketing teams at B2B companies face a distribution efficiency problem. After spending 8-10 hours creating a high-quality blog post, they need to adapt it for 5-6 different platforms (Twitter thread, LinkedIn post, email newsletter excerpt, Instagram caption, Facebook post). Each adaptation takes 20-30 minutes and requires understanding platform-specific best practices.
 
For a team publishing 4 blog posts per month, that's 10-12 hours monthly spent on manual reformatting—120-144 hours annually at $40-50/hour = $4,800-7,200 in labor costs. More importantly, many teams skip some platforms because reformatting is tedious, leaving distribution opportunities on the table.
 
The cost: great content reaches 1-2 platforms instead of 6, reducing total potential reach by 60-70%."
 
**Example 3: Synthetic Personas**
 
"Marketing teams at startups and small businesses make critical targeting decisions based on assumptions rather than data. Creating research-based customer personas requires customer interviews (20-30 interviews), survey analysis (100+ responses), and synthesis time (15-20 hours per persona).
 
At $150-200/hour consulting rates, persona development costs $3,000-6,000 per set. Most companies under $5M revenue skip this entirely, targeting 'everyone' or guessing at their ideal customer based on founders' intuitions.
 
The cost: wasted ad spend targeting wrong audiences, messaging that doesn't resonate, product features nobody wants. Per CB Insights, 42% of startup failures cite 'no market need'—often because they never understood their actual customer."
 
**Why These Work**:
- Specific company type and size
- Specific people affected
- Quantified time and costs
- Business impact clearly stated
- Real consequences of not solving
 
---

![Proposed Solution Image](assets/part3-solution.png)

## Section 2: Proposed Solution (10 points)
 
**The Goal**: Explain your approach clearly enough that anyone could build it.
 
**The Three Questions**:
 
**1. Your Madison-Powered Approach**
 
Explain what you're building, specifically.
 
**Social Media Monitor Example**:
"A multi-agent Madison system that: (1) automatically fetches competitor posts from Twitter and LinkedIn every 6 hours via APIs, (2) analyzes posts for trending industry keywords and engagement spikes, (3) identifies content that's outperforming typical engagement by 2x+, and (4) sends daily digest emails with top findings and suggested response opportunities"
 
**Content Adapter Example**:
"A Madison workflow that: (1) takes a blog post URL, (2) extracts key points and main message, (3) uses GPT-4 to generate platform-optimized versions (Twitter thread with hook, LinkedIn post with professional tone, Instagram caption with hashtags), (4) outputs all versions in one click, maintaining core message while adapting to platform best practices"
 
**Synthetic Persona Example**:
"An AI-powered persona generator that: (1) ingests customer data from surveys and interviews (text responses), (2) uses NLP to identify common patterns in goals, pain points, and behaviors, (3) clusters customers into 3-5 distinct segments, (4) generates detailed persona documents with demographics, motivations, preferred channels, and buying triggers for each segment"
 
**Structure Your Solution**:
What it does:
Input: [What data it needs]
Processing: [How it analyzes]
Output: [What it produces]
Integration: [How it fits into existing workflows]
 
**2. Why Madison vs. Other Solutions?**
 
What makes your approach better?
 
**Compare to Alternatives**:
 
**For Monitoring Tools**:
- Manual checking: Slow, inconsistent, humans miss things
- Social listening tools (Hootsuite, Sprout): Expensive ($100-300/month), designed for own brand not competitors
- Google Alerts: Misses social media, too general
- Custom-built: Expensive to build and maintain
 
**For Content Tools**:
- Manual reformatting: Slow, tedious, teams skip platforms
- Content calendars (CoSchedule): Schedule but don't reformat
- AI writing tools (Jasper, Copy.ai): Generic output, no platform optimization
- Copy-paste: Inconsistent, platform best practices ignored
 
**Your Madison Advantage**:
- Madison's agent architecture allows specialized processing per platform
- Open-source = customizable for your specific needs
- n8n integration = works with existing marketing stack
- Extensible = add new platforms or features easily
- Free (except AI API costs)
 
**Example**:
"Madison's multi-agent architecture is ideal for competitor monitoring because unlike monolithic social listening tools, we can deploy specialized agents: one for Twitter tracking, one for LinkedIn, one for trend detection, one for engagement analysis. Each agent can be optimized for its specific platform's API and data structure. Plus, the n8n integration means marketers can customize alerts, filters, and outputs without writing code."
 
**3. What Makes This Technically Interesting?**
 
Show you understand the technical challenge.
 
**Discuss**:
- What's hard about this problem
- What technical decisions you'll make
- What trade-offs exist
- Why your approach handles these challenges
 
**Competitor Monitor Example**:
"The technical challenge is signal vs. noise. Competitors post 10-50 times per day; most posts are routine, but 1-2 might be strategically important (product launch, pricing change, major campaign). My approach uses multi-dimensional scoring: (1) engagement anomaly detection (2x+ normal engagement), (2) keyword matching against strategic terms ('pricing,' 'launch,' 'feature'), (3) sentiment shifts (suddenly negative = possible crisis), (4) posting pattern changes (sudden frequency = campaign). Only posts scoring high on 2+ dimensions get flagged as 'high-priority.' This reduces noise by 90%+ while catching strategic signals."
 
**Content Adapter Example**:
"The challenge is maintaining message fidelity while optimizing for platform. Simply shortening for Twitter loses context; simply copy-pasting to LinkedIn ignores professional norms. My approach uses a two-step process: (1) Extract Agent identifies core message and 3-5 key points, (2) Adaptation Agent rewrites for each platform using platform-specific prompts (Twitter: hook-first, thread structure, character limits; LinkedIn: professional tone, business context, longer-form). This preserves meaning while respecting platform cultures."
 
**Synthetic Persona Example**:
"The challenge is going from unstructured qualitative data (interview transcripts, survey responses) to structured personas. My approach uses: (1) NLP to extract goals, pain points, and behavioral patterns from text responses, (2) clustering algorithm to group similar customers, (3) statistical profiling to identify defining characteristics per cluster, (4) GPT-4 to synthesize findings into readable persona documents. The multi-step approach ensures personas are data-driven (not assumed) while remaining human-readable."
 
---

![User Stories Image](assets/part3-userstories.png)

## Section 3: User Stories (10 points)
 
**What Are User Stories?**
 
A format for describing features from the user's perspective.
 
**The Format**:
"As a [role], I want [feature] so that [benefit]"
 
**Why This Format Works**:
- Forces you to think from user perspective
- Connects features to actual needs
- Industry-standard format
- Shows you understand user-centered design
 
**Write Three User Stories**
 
**Good Examples - Competitor Monitor**:
 
**Story 1 - Core Use Case**:  
"As a social media manager at a B2B SaaS company, I want to receive a daily digest of my competitors' highest-performing posts from the past 24 hours so that I can identify trending topics to respond to without spending 90 minutes manually checking their accounts."
 
**Story 2 - Alert Use Case**:  
"As a marketing director, I want to receive immediate Slack alerts when competitors post about pricing, product launches, or major campaigns so that I can brief my team and prepare our response within hours, not days."
 
**Story 3 - Analysis Use Case**:  
"As a content strategist, I want to see which topics competitors are posting about most frequently so that I can identify content gaps where we're under-represented in industry conversations."
 
---
 
**Good Examples - Content Adapter**:
 
**Story 1 - Core Use Case**:  
"As a content marketer, I want to paste a blog post URL and receive platform-optimized versions for Twitter, LinkedIn, and Instagram in one click so that I can distribute content across all channels without spending 30-45 minutes reformatting."
 
**Story 2 - Quality Use Case**:  
"As a marketing manager, I want the adapted content to maintain our core message while following each platform's best practices so that our content performs well everywhere without me reviewing and editing each version."
 
**Story 3 - Efficiency Use Case**:  
"As a small business owner with limited time, I want to write once and distribute everywhere so that I can maintain presence on 5 platforms without hiring a social media manager."
 
---
 
**Good Examples - Synthetic Personas**:
 
**Story 1 - Core Use Case**:  
"As a marketing strategist, I want to upload 50 customer interview transcripts and receive 3-4 data-driven persona documents so that I can base our targeting on actual customer patterns rather than assumptions, without spending 20 hours manually synthesizing."
 
**Story 2 - Segmentation Use Case**:  
"As a product manager, I want to see how our customers cluster into distinct groups with different goals and pain points so that I can prioritize features that matter most to our largest segments."
 
**Story 3 - Validation Use Case**:  
"As a founder, I want to validate or challenge my assumptions about our ideal customer by seeing what patterns actually emerge from customer data so that I stop wasting ad budget on audiences that don't convert."
 
---
 
**Bad Examples (And Why)**:
 
❌ "As a user, I want the tool to work so that I can use it"
- Too vague
- No specific role
- No specific feature
- No measurable benefit
 
❌ "As a developer, I want to build an API so that it's scalable"
- Wrong perspective (you're not the end user)
- Technical focus, not user benefit
- No user problem being solved
 
---

![Success Metrics Image](assets/part3-metrics.png)

## Section 4: Success Metrics (10 points)
 
**The Goal**: Define how you'll know if you succeeded.
 
**The Three Questions**:
 
**1. What Improves? By How Much?**
 
Define clear, measurable improvements.
 
**Good Metrics by Project Type**:
 
**For Monitoring/Scraping Tools**:
- Time saved: "Reduce monitoring time from 10 hours/week to 30 minutes/week"
- Coverage: "Track 10 competitors across 3 platforms automatically"
- Speed: "Detect trending topics within 2 hours vs. 24 hours manual"
 
**For Content Tools**:
- Time saved: "Reduce content adaptation from 45 minutes to 2 minutes per post"
- Quality: "Maintain 90%+ message fidelity across platforms"
- Reach: "Enable posting to 5 platforms instead of 2"
 
**For Analysis Tools**:
- Accuracy: "Identify customer segments with 85%+ confidence"
- Speed: "Process 100 survey responses in 5 minutes vs. 20 hours manual"
- Insight: "Generate 5 actionable insights per analysis"
 
**Bad Metrics**:
- "Better marketing" (not measurable)
- "Happier teams" (subjective)
- "More efficient" (vague)
 
**2. Time Saved? Errors Reduced? Cost Cut?**
 
Quantify the business impact.
 
**Framework**:  
Current State:  
Process: [How it works now]  
Time: [How long it takes]  
Cost: [What it costs]   
Error Rate: [How often problems occur]  

Future State (with your solution):  
Process: [How it will work]  
Time: [New time required]  
Cost: [New cost]  
Error Rate: [New error rate]  

Improvement:  
Time: [X% faster or X hours saved]  
Cost: [Y% cheaper or $Y saved]  
Quality: [Z% fewer errors]
 
**Competitor Monitor Example**:  
Current State:  
Social media manager manually checks 8 competitors daily  
15-20 minutes per competitor = 2-3 hours daily  
At $60K salary = $7,500-11,000 annually just on monitoring  
Misses posts published outside work hours (60% of content)  

With Madison Competitor Monitor:  
Automated checking every 6 hours  
10-15 minutes daily reviewing digest of high-priority posts only  
At $60K salary = $1,000-1,500 annually  
Catches 100% of posts 24/7  

Improvement:  
Time: 90% reduction (2.5 hours saved daily)  
Cost: $6,000-10,000 saved annually  
Coverage: 2.5x more competitive intelligence gathered
 
**Content Adapter Example**:  
Current State:  
Content marketer manually reformats blog post for 5 platforms  
30-45 minutes total adaptation time  
Creates 4 blog posts monthly = 2-3 hours monthly reformatting  
Often skips 1-2 platforms due to time constraints

With Madison Content Adapter:  
Automated platform-specific adaptation  
5 minutes reviewing/editing outputs  
4 blog posts monthly = 20 minutes monthly  
All platforms covered consistently  

Improvement:  
Time: 85% reduction (2+ hours saved monthly)  
Reach: 40% increase (all platforms covered vs. 3-4)  
Consistency: All platforms get timely content
 
**3. How Would You Measure Success?**
 
Define your testing methodology.
 
**Approaches**:
 
**Quantitative Testing**:
- "Test with 200 competitor posts, measure detection accuracy for trending topics"
- "Measure processing speed with datasets of 100, 500, and 1000 posts"
- "Track precision (flagged posts that were actually important) and recall (important posts that were flagged)"
 
**User Testing**:
- "Have 3 social media managers use it for one week, measure time spent on competitor monitoring"
- "Survey satisfaction on 1-5 scale across 5 dimensions"
- "Track adoption: are they still using it after 30 days?"
 
**Business Impact**:
- "Monitor response time to trending topics before/after implementation"
- "Measure competitive intelligence quality (insights gathered per week)"
- "Track cost per competitive insight"
 
**Example - Competitor Monitor**:  
"I'll measure success through three methods:
 
1. **Accuracy Testing**: Run the system for 2 weeks alongside manual monitoring. Count how many strategically important posts it correctly flagged. Success = 85%+ precision (flagged posts were actually important) and 90%+ recall (caught all important posts).
 
2. **Speed Testing**: Measure time from competitor post to alert received. Success = alerts within 2 hours for high-priority posts.
 
3. **User Testing**: Have two social media managers use it for one week. Success = 75%+ time reduction on competitor monitoring and 4+/5 satisfaction rating."
 
**Example - Content Adapter**:  
"I'll measure success through three methods:
 
1. **Quality Testing**: Have 3 content marketers rate platform-adapted versions on message fidelity (1-5). Success = 4+ average rating.
 
2. **Speed Testing**: Time the full workflow (blog URL → 5 platform versions). Success = under 3 minutes including review.
 
3. **Adoption Testing**: Two content marketers use it for all blog posts over 2 weeks. Success = they adopt it for at least 80% of posts and rate it 4+/5 on usefulness."
 
---

![Excellence Level Image](assets/part3-excellence.png)

## Excellence Level for Your PRD
 
**How to Get Excellence Points** (8 additional points possible):
 
**1. Industry Research (2 pts)**
- Cite actual studies or reports
- Reference market data
- Show competitive analysis
- Quote industry experts
 
Examples:
- "According to HubSpot's 2024 State of Marketing report, 63% of marketers say competitor intelligence is critical but 71% do it manually"
- "Per Sprout Social's research, brands that respond to trends within 2 hours see 3x engagement vs. those responding after 24 hours"
 
**2. Specific API/Tool References (2 pts)**
- Name specific APIs you'll integrate
- Reference specific n8n nodes
- List specific AI models you'll use
- Show technical research
 
Example: "Will integrate with: Twitter API v2 (tweet timeline endpoint), LinkedIn Marketing API (company posts), using n8n's HTTP Request, JSON Parse, and Switch nodes, powered by GPT-4 for trend analysis and GPT-3.5 for summarization (cost optimization)."
 
**3. Edge Cases and Failure Modes (2 pts)**
- What could go wrong?
- How will you handle errors?
- What are system limitations?
- What won't it do well?
 
Examples:
- "Edge cases: API rate limits hit if checking too frequently (will throttle to every 6 hours), sarcastic posts may be misclassified as negative (requires human review flags), non-English posts require translation API (out of scope for MVP)"
- "Failure modes: If API goes down, queue requests and retry; if GPT-4 is slow, fall back to GPT-3.5; if trend detection has high false positives, add confidence thresholds"
 
**4. Professional Format (2 pts)**
- Clean, organized layout
- Consistent formatting
- Visual hierarchy
- Professional presentation
- Proper headers/sections
- No spelling/grammar errors

---

| [← Part 2: Gap Analysis - What's Between You and That Job?](./2_Gap_Analysis.md) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | [Part 4: Technical Architecture →](./4_Technical_Architecture.md) |
|:---|:---:|---:|