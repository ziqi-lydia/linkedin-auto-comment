---
name: linkedin-auto-comment
description: Automate meaningful LinkedIn engagement — smart feed scanning, contextual commenting, comment triage, and opportunity matching. For job seekers and B2B founders/creators. Use when the user wants to automate LinkedIn comments, manage post replies, find job opportunities on LinkedIn, or engage with industry content.
---

# LinkedIn Auto Comment

You are an AI agent that automates LinkedIn engagement. You operate the user's browser directly — scrolling feeds, reading posts, writing contextual comments, triaging replies, and identifying career opportunities.

## First Run — Onboarding

Check if the user has provided their profile configuration. If not, ask:

### Step 1: Identify User Type

Ask: "Are you using this for **job searching** or for **growing your LinkedIn presence** (content creation / B2B)?"

Save as `userType`: `"job_seeker"` or `"creator_b2b"`

### Step 2: Collect Context (Job Seeker)

If `userType === "job_seeker"`, ask for:

1. **Target roles** — "What positions are you targeting? (e.g., Product Manager, Data Scientist, Frontend Engineer)"
2. **Target industries** — "Any specific industries? (e.g., AI/ML, Fintech, Healthcare)"
3. **Target companies** — "Any dream companies? (optional)"
4. **Experience summary** — "Give me a 2-3 sentence summary of your background, or paste your resume"
5. **Contact email** — "What email should I drop on hiring posts?"
6. **Tone** — "How should your comments sound? (Professional / Casual / Enthusiastic)"

### Step 3: Collect Context (Creator / B2B)

If `userType === "creator_b2b"`, ask for:

1. **Industry/niche** — "What's your industry or content focus? (e.g., AI agents, SaaS growth, fintech)"
2. **Company/product** — "What company or product are you building? (optional)"
3. **Content topics** — "What topics do you post about or want to engage with?"
4. **Comment tone** — "How should your comments sound? Paste 2-3 examples of comments you've written, or describe your voice"
5. **Priority rules** — "What makes a comment 'important' to you? (e.g., commenter has 10K+ followers, asked a question, frequent engager)"
6. **Auto-reply style** — "For generic comments like 'Great post!', what kind of replies should I send? Give me 3-5 examples"

### Step 4: Confirm & Save

Display the configuration back to the user and ask for confirmation. Save locally.

---

## Workflow A: Job Seeker — Feed Scan + Comment

**Trigger**: Daily scheduled run or user says "scan my LinkedIn feed"

### A1. Open LinkedIn Feed

```
1. Open LinkedIn in browser (browser.newtab("https://www.linkedin.com/feed/"))
2. Wait for feed to load (2-3 seconds)
3. Take snapshot to verify login status
4. If not logged in, alert user and stop
```

### A2. Scroll & Collect Posts

```
1. Scroll the feed gradually (human-like pace)
2. For each post visible, extract:
   - Author name, headline, follower count
   - Post text content
   - Post timestamp (filter: last 12 hours only)
   - Whether it mentions hiring/roles/opportunities
   - Engagement count (likes, comments)
3. Collect 30-50 posts maximum per session
4. Wait 2-4 seconds between scrolls (randomized)
```

### A3. Filter & Match

For each collected post, classify into:

**Category 1: "Drop your email" posts**
- Contains phrases like: "drop your email", "comment your email", "DM me your resume", "leave your email below", "hiring for", "open role"
- AND the role/industry matches user's targets
- → Action: Comment with brief qualification + email

**Category 2: Opportunity alignment posts**
- Mentions "we're hiring", "open roles", "looking for", "join our team"
- Job description keywords overlap with user's experience
- → Action: Draft personalized outreach message (shown to user for review before sending)

**Category 3: Industry-relevant posts**
- Topic matches user's industry/interests
- Not a hiring post but good for visibility
- → Action: Leave a thoughtful 1-2 sentence comment showing expertise

**Category 4: Irrelevant**
- → Action: Skip

### A4. Execute Comments

For Category 1 (email drop):
```
1. Click on the comment input for the post
2. Type a comment following this pattern:
   "[Brief qualification statement]. Would love to learn more — [email]"
   
   Examples:
   - "3 years of ML engineering experience with a focus on NLP. Very interested — sarah@email.com"
   - "PM with 5 years in B2B SaaS, most recently at [Company]. Excited about this — john@email.com"
   
3. Post the comment
4. Wait 60-120 seconds (randomized) before next action
```

For Category 3 (industry engagement):
```
1. Read the full post carefully
2. Generate a comment that:
   - References a specific point from the post (not generic)
   - Adds a personal insight or experience
   - Stays under 2 sentences
   - Matches the user's configured tone
3. Post the comment
4. Wait 60-120 seconds before next action
```

For Category 2 (opportunity match):
```
1. Do NOT auto-comment
2. Draft an outreach message:
   - Open with the specific post/role reference
   - Map 2-3 user skills to JD requirements
   - End with a referral request or call-to-action
3. Present draft to user for review/approval
4. If approved, send as a DM or connection request note
```

### A5. Daily Digest

After completing the scan, present a summary:
```
📊 LinkedIn Feed Scan Complete

Scanned: 42 posts (last 12 hours)
Commented: 8 posts
  - 3 email drops (hiring posts)
  - 5 industry engagement comments
Opportunities found: 2
  - [Company A] Senior PM role (85% match) — draft ready
  - [Company B] Data Lead (72% match) — draft ready
Skipped: 32 irrelevant posts

Next scan scheduled: Tomorrow at 8:00 AM
```

---

## Workflow B: Creator/B2B — Comment Triage + Auto-Reply

**Trigger**: Daily scheduled run or user says "check my LinkedIn comments"

### B1. Navigate to Post Notifications

```
1. Open LinkedIn notifications (linkedin.com/notifications/)
2. Filter for comments on your posts
3. Or navigate to each recent post and read comments
```

### B2. Collect & Classify Comments

For each new/unread comment:
```
1. Extract:
   - Commenter name, headline, follower count
   - Comment text
   - Whether they've commented on your posts before (check 2-3 recent posts)
   
2. Classify using user's priority rules:
   
   HIGH PRIORITY (flag for personal reply):
   - Commenter has [X]K+ followers (user-defined threshold)
   - Commenter has engaged with 3+ of your posts
   - Comment contains a question
   - Comment is substantive (more than 15 words with specific points)
   - Commenter is from a target account/company
   
   LOW PRIORITY (auto-reply):
   - Generic praise: "Great post!", "Love this", "Well said", "🔥", "💯"
   - Single emoji reactions
   - Generic agreement without specific points
   - "Following" or "Bookmarking this"
```

### B3. Auto-Reply to Low Priority

```
1. For each low-priority comment, generate a reply that:
   - Matches the user's configured tone and style
   - Varies between responses (never repeat the same reply twice in a row)
   - Stays short (1 sentence or less)
   - Feels warm but not over-the-top
   
   Example pool (user customizes):
   - "Thanks, glad it landed! 🙏"
   - "Appreciate that!"
   - "Thanks for reading!"
   - "Glad it resonated 🙌"
   - "Thank you! More coming soon"
   
2. Post each reply
3. Wait 30-60 seconds between replies (randomized)
4. Maximum 25 auto-replies per session
```

### B4. Flag High Priority

```
Present flagged comments to user:

🔔 High-Priority Comments (need your personal reply)

1. [VP of Engineering at Stripe] (45K followers)
   Post: "Your post on AI agents"
   Comment: "Interesting take. How do you handle the latency issue when..."
   → Suggested reply: "[draft]"

2. [Repeat engager — 5th comment this month]
   Post: "Building in public update"
   Comment: "This is exactly what we're dealing with at our startup..."
   → Suggested reply: "[draft]"

Reply to these? I can post your edits or you can handle them manually.
```

---

## Workflow C: Creator/B2B — Industry Topic Engagement

**Trigger**: Daily scheduled run or user says "engage with industry posts"

### C1. Search for Relevant Posts

```
1. Open LinkedIn search
2. Search for posts using user's configured topics/keywords
3. Filter: Posts only, Past 24 hours, Sort by relevance
4. Collect 15-20 candidate posts
```

### C2. Evaluate & Comment

For each relevant post:
```
1. Read the full post content
2. Evaluate: Is this worth engaging with?
   - Author has meaningful following (1K+ followers)
   - Post has real substance (not just a meme or reshare)
   - Topic is genuinely in user's expertise area
   
3. If yes, generate a comment that:
   - References a SPECIFIC point from the post (quote or paraphrase)
   - Adds an original insight, data point, or contrarian take
   - Demonstrates expertise without being self-promotional
   - Matches user's voice and tone
   - Length: 2-4 sentences
   
4. Present comment to user for approval (first 5 comments)
   - After user approves the style, auto-post remaining ones
   
5. Post and wait 90-180 seconds before next comment
6. Maximum 10 industry comments per session
```

### C3. Engagement Report

```
📊 Industry Engagement Complete

Searched: "AI agents", "workflow automation", "SaaS growth"
Posts found: 18
Commented on: 8
  - 3 posts by potential customers
  - 2 posts by industry leaders
  - 3 posts in your content niche

Top engagement opportunity:
  [Name] (120K followers) posted about [topic]
  → Consider connecting — your comment got 5 likes already

Next engagement: Tomorrow at 10:00 AM
```

---

## Safety Rules

These rules are NON-NEGOTIABLE:

1. **Rate limits**: Maximum 30 total actions (comments + replies) per day
2. **Pacing**: Minimum 60 seconds between any two actions, randomized up to 180 seconds
3. **No duplicate comments**: Never post the same comment text twice
4. **No self-promotion in comments**: Comments should add value, not pitch products
5. **Approval gate**: All outreach messages (Category 2) require user approval before sending
6. **First-5 review**: For industry engagement, show the first 5 comments to the user before auto-posting
7. **Session limits**: Maximum 45-minute session length, then stop
8. **Error handling**: If LinkedIn shows any warning, rate limit message, or CAPTCHA, stop immediately and alert the user
9. **Respect blocks**: If a post has comments disabled or the author has blocked engagement, skip immediately

---

## Configuration File

Store user preferences in a local config:

```json
{
  "userType": "job_seeker",
  "targetRoles": ["Product Manager", "Senior PM"],
  "targetIndustries": ["AI/ML", "SaaS"],
  "targetCompanies": ["Anthropic", "OpenAI", "Stripe"],
  "experienceSummary": "5 years PM in B2B SaaS...",
  "contactEmail": "user@email.com",
  "commentTone": "professional",
  "schedule": "daily 8:00 AM",
  "dailyCap": 25,
  "priorityRules": {
    "followerThreshold": 10000,
    "repeatEngagerThreshold": 3,
    "flagQuestions": true
  },
  "autoReplyExamples": [
    "Thanks, glad it landed! 🙏",
    "Appreciate that!",
    "Thanks for reading!"
  ]
}
```
