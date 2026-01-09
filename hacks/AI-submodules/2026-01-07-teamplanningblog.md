---
layout: post
title: "Team Planning Blog 1/9/25"
permalink: /digital-famine/ai/teamplanningblog
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Team Crashers
microblog: true
---
# Team Planning Blog 1/9/25

# Badges System
**Team Members:** Akshara and Ruchika

---

## 🏆 Available Badges

### Achievement Badges

| Badge Name | Requirement | Description |
|------------|-------------|-------------|
| **Delightful Data Scientist** | Complete Submodule 1 | Mastered foundational AI concepts and data literacy |
| **Perfect Prompt Engineer** | Complete Submodule 2 | Demonstrated expertise in crafting effective AI prompts |
| **Prodigy Problem Solver** | Complete Submodule 3 | Applied AI knowledge to solve real-world challenges |
| **Responsible AI Master** | Complete Entire Quest | Achieved comprehensive understanding of ethical AI practices |


<img width="1254" height="1260" alt="Image" src="https://github.com/user-attachments/assets/de5b55a2-4f66-4ea5-aaa4-f162e870fab9" />

### Special Badges

| Badge Name | Requirement | Description |
|------------|-------------|-------------|
| **Super Smart Genius** | Make the Leaderboard | Ranked among top performers in the platform |
| **Intelligent Instructor** | Create a "Good" Prompt | Crafted a high-quality, effective AI prompt |
| **Sensational Surveyor** | Submit the Survey | Provided valuable feedback to improve the platform |

---

##  Database Integration

### The Fundamental Problem Badges Solve

**Without a database, your site has no memory.** Every time a student refreshes the page or logs out, their progress vanishes. Badges are meaningless if they disappear.

### Why Badges Require a Database: The Evidence

#### 1️⃣ **Persistence of State**

The system must permanently remember:

- ✅ Has Student A completed Module 2? *(Boolean status)*
- 📅 When did they complete it? *(Timestamp)*
- 🏅 Which badge did they earn? *(Badge identifier)*
- 🚫 Have they already been awarded this badge? *(Prevent duplicates)*

**Without a database:**
- Information exists only in browser memory (localStorage)
- Lost when cookies are cleared
- Not accessible across devices
- Vulnerable to manipulation

**With a database:**
- Information persists permanently
- Accessible from any device
- Auditable by educators
- Protected from tampering

#### 2️⃣ **Relational Data Management**

Badges create three-way relationships that databases are designed to handle:
```
USERS ←→ BADGES ←→ MODULES
```

**Example queries that require database integration:**

| Query | Why Database is Required |
|-------|-------------------------|
| "Show me all badges Alice earned" | Join `users` table with `user_badges` table |
| "Which students completed the Privacy module?" | Join `modules`, `badges`, and `users` tables |
| "What's the completion rate for Module 2?" | Aggregate data across all users |
| "Display badge on profile" | Retrieve badge icon, name, description from `badges` table |

#### 3️⃣ **Conditional Logic**

**Example:** Award "Delightful Data Scientist" badge when:
- Submodule 1 is 100% complete **AND**
- User hasn't already earned this badge **AND**
- All lessons in Submodule 1 are marked complete

This requires:
- Querying across multiple tables
- Conditional insertion logic
- Atomic transactions for data consistency

#### 4️⃣ **Temporal Data Tracking**

Badges carry time-series data:
- **When** was the badge earned? *(earned_date)*
- **How long** did it take? *(time between first lesson and completion)*
- **Streaks**: Did user earn multiple badges consecutively?

#### 5️⃣ **Scalability**

| Without Database | With Database |
|-----------------|---------------|
| 500 separate copies of badge data (one per student) | One centralized source of truth |
| No platform-wide statistics | `SELECT COUNT(*) FROM user_badges WHERE badge_id = X` |
| Cannot generate leaderboards | SQL queries with JOINs and aggregations |
| No automated notifications | Trigger events when badges are awarded |

#### Complete Data Flow Example
Student Earns "Delightful Data Scientist" Badge:
Student completes final lesson in Submodule 1
Frontend: POST /api/progress/complete-lesson
Database: INSERT INTO lesson_progress (marks complete)
Backend: Counts completed lessons = total lessons?
Backend: INSERT INTO user_badges (awards badge)
Backend: Returns {badgeEarned: true, badge: {...}}
Frontend: Shows celebration modal 🎉
Badge appears on student profile permanently

---

##  Purpose: Why Badges Work for Responsible AI Education

### Teaching Benefits

#### ** Progress Visualization**
Students see concrete evidence of their learning journey, making abstract AI ethics concepts feel more tangible.

#### ** Intrinsic Motivation**
Earning badges triggers dopamine responses, making responsible AI practices feel rewarding rather than restrictive.

#### ** Competency Signaling**
Badges represent mastery of specific responsible AI principles:
- Fairness & Bias Detection
- Transparency & Explainability
- Privacy & Data Protection
- Accountability & Governance

#### ** Social Proof**
Students can share achievements, spreading awareness about AI responsibility throughout their networks.

### Specific Benefits for Responsible AI Education

✅ **Reframes ethical AI as a skill set** to be developed, not just rules to follow

✅ **Each badge becomes a mini-certification** in concepts like bias detection, data privacy, or algorithmic transparency

✅ **Creates positive associations** with responsible practices instead of framing them as limitations or restrictions

✅ **Encourages continuous learning** by visualizing the journey from novice to "Responsible AI Master"

✅ **Reinforces key concepts** through memorable, achievement-based learning milestones

---

## Expected Outcomes

By integrating badges with database persistence, the platform will:

1. **Increase engagement** through gamification and visible progress
2. **Improve knowledge retention** by celebrating concept mastery
3. **Enable data-driven insights** into which modules students find most challenging
4. **Foster a community** of learners committed to responsible AI practices
5. **Provide verifiable credentials** that students can reference in portfolios or resumes

---

*Database integration transforms badges from temporary browser decorations into permanent, meaningful credentials that validate student learning and drive engagement with responsible AI principles.*

## Survey Staged Data (Anishka)

### Purpose: Understanding Student AI Tool Usage Patterns

The Submodule 1 survey collects critical data about how students actually use AI tools in their academic work. This data helps identify usage patterns, preferred platforms, and student perspectives on AI policies in education—insights that are essential for teaching responsible AI practices that align with real-world student behavior.

### Survey Data Structure

#### **1. AI Tool Preference Matrix (Multiple Choice Table)**
Captures which AI platform students prefer for different subjects:

**Data Collected:**
- Subject areas: English, Math, Science, Computer Science, History
- AI platforms: ChatGPT, Claude, Gemini, Copilot
- One selection per subject (radio button constraint)

**Database Schema Requirements:**
```
survey_responses table:
- response_id (primary key)
- user_id (foreign key)
- subject (enum: english, math, science, cs, history)
- preferred_ai_tool (enum: ChatGPT, Claude, Gemini, Copilot)
- timestamp
```

**Why This Data Matters:**
- Reveals subject-specific AI tool preferences (e.g., "Do students prefer Copilot for CS?")
- Identifies platform dominance across different academic domains
- Helps educators understand which tools students are most familiar with
- Enables targeted lessons: "Since 80% use ChatGPT for English, let's address its specific limitations"

#### **2. AI Usage Behavior (Yes/No Question)**
"Do you use AI to help you with schoolwork in essays and assignments?"

**Data Collected:**
- Binary response (Yes/No)
- Links to user profile for demographic analysis

**Database Schema Requirements:**
```
survey_responses table:
- uses_ai_for_schoolwork (boolean)
- user_id (foreign key to link with other data)
```

**Why This Data Matters:**
- Establishes baseline: What percentage of students actively use AI for academics?
- Identifies potential gap between policy and practice
- Segments users for personalized learning paths (AI users vs. non-users need different approaches)
- Correlates with badge completion rates (Do AI users engage more with responsible AI content?)

#### **3. Policy Perspectives (Free Response Question)**
"Most classes do not want students to use AI. What do you feel about these policies? How would you want to use AI without having it do everything for you and it being considered cheating?"

**Data Collected:**
- Long-form text response (stored as TEXT datatype)
- Qualitative insights into student reasoning and ethical frameworks

**Database Schema Requirements:**
```
survey_responses table:
- policy_perspective (TEXT, allows unlimited characters)
- response_length (integer, for analyzing engagement depth)
- sentiment_score (optional: AI-analyzed sentiment)
```

**Why This Data Matters:**
- **Qualitative insights**: Reveals student reasoning about AI ethics in their own words
- **Identifies misconceptions**: "Some students think AI detection is perfect—we need to address that"
- **Thematic analysis**: Common themes like "AI as a tutor vs. AI as a replacement"
- **Authentic voice**: Direct quotes can inform lesson examples and case studies
- **Ethical development tracking**: Responses before vs. after completing modules show learning impact

### Data Visualization: Real-Time Bar Graph Display

#### **Interactive Analytics Dashboard**

All survey responses are aggregated and displayed in dynamic bar graphs that update in real-time as students submit their surveys. This visualization serves multiple educational and analytical purposes:

**Graph Types Displayed:**

1. **AI Tool Preference by Subject**
   - Five grouped bar charts (one per subject)
   - Four bars per chart (ChatGPT, Claude, Gemini, Copilot)
   - Y-axis: Number of students who selected each tool
   - Color-coded bars for easy comparison

2. **Overall AI Usage Distribution**
   - Simple bar chart showing Yes vs. No responses
   - Percentage labels on each bar
   - Updates live as new submissions arrive

**Technical Implementation:**
```
Frontend: GET /api/survey/analytics/submodule1
Backend queries database:
  SELECT subject, ai_tool, COUNT(*) as count
  FROM ai_tool_preferences
  GROUP BY subject, ai_tool

Returns JSON:
{
  "english": {"ChatGPT": 45, "Claude": 12, "Gemini": 8, "Copilot": 3},
  "math": {"ChatGPT": 38, "Claude": 18, "Gemini": 7, "Copilot": 5},
  ...
  "usesAI": {"Yes": 52, "No": 16}
}

Frontend renders using Chart.js or similar library
Graphs update every time page refreshes or new submission occurs
```

**Educational Benefits of Visualization:**

✅ **Transparency**: Students see they're contributing to real data collection, not just filling out forms

✅ **Peer comparison**: "Wow, most students prefer ChatGPT for English—maybe I should try it"

✅ **Data literacy**: Students learn how raw survey data transforms into meaningful visualizations

✅ **Immediate feedback**: Seeing their response appear in the graph creates satisfaction and validates participation

✅ **Pattern recognition**: Visual trends are easier to grasp than raw numbers (e.g., "Copilot dominates Computer Science")

✅ **Motivates participation**: "Only 30 responses so far—let me add mine!" encourages survey completion

**Example Dashboard Layout:**
```
┌─────────────────────────────────────────────────────┐
│  📊 Submodule 1 Survey Results (Live)              │
│                                                     │
│  Total Responses: 68                               │
├─────────────────────────────────────────────────────┤
│  🔹 AI Tool Preferences by Subject                 │
│                                                     │
│  English:    [ChatGPT ████████ 45]                │
│              [Claude ███ 12]                       │
│              [Gemini ██ 8]                         │
│              [Copilot █ 3]                         │
│                                                     │
│  Math:       [ChatGPT ███████ 38]                 │
│              [Claude ████ 18]                      │
│              [Gemini ██ 7]                         │
│              [Copilot █ 5]                         │
│                                                     │
│  [Similar charts for Science, CS, History...]      │
├─────────────────────────────────────────────────────┤
│  🔹 Do Students Use AI for Schoolwork?             │
│                                                     │
│  Yes: ████████████████ 76% (52 students)          │
│  No:  █████ 24% (16 students)                     │
└─────────────────────────────────────────────────────┘
```

### Database Integration Architecture

#### **Normalized Data Storage**
```
users table                survey_responses table              ai_tool_preferences table
-----------               ---------------------              ------------------------
user_id                   response_id                        preference_id 
username                  user_id                            response_id 
email                     uses_ai_schoolwork                 subject 
                          policy_perspective                 ai_tool 
                          completed_at            
                          badge_awarded 
```

#### **Data Flow on Survey Submission**
```
Student completes survey → Frontend validates all required fields
↓
POST /api/survey/submodule1/submit
↓
Backend receives:
{
  userId: 123,
  toolPreferences: {
    english: "ChatGPT",
    math: "Claude",
    science: "Gemini",
    cs: "Copilot",
    history: "ChatGPT"
  },
  usesAI: true,
  policyPerspective: "I think AI should be allowed as a study tool..."
}
↓
Database transactions (atomic):
1. INSERT INTO survey_responses (user_id, uses_ai_schoolwork, policy_perspective)
2. INSERT INTO ai_tool_preferences (5 rows, one per subject)
3. UPDATE user_badges SET badge_earned = TRUE WHERE badge = "Sensational Surveyor"
4. TRIGGER analytics cache refresh (updates bar graph data)
↓
Response to frontend: {success: true, badgeAwarded: true}
↓
UI shows: Badge notification + "Thank you! View your response in the live results below ⬇️"
↓
Page redirects to analytics dashboard showing updated bar graphs
```


## Code Runner (Michelle and Varada)
---
## Guided Prompt Builder: Teaching Responsible AI Through Scaffolded Learning

Our Code Runner feature helps students learn how to write good AI prompts by giving them sentence starters and fill-in-the-blank templates. Instead of staring at a blank box wondering what to write, students get a structured framework that guides them through creating effective and responsible prompts. They fill in the blanks based on their needs, and then the AI generates a response using their completed prompt. This way they're learning what good prompts look like while actually using them.

---

## **Key Features:**

- Students see partially completed prompts with blanks to fill in, like "I need help understanding [TOPIC]. Please explain it in a way that is [TONE/STYLE] and includes [SPECIFIC REQUIREMENTS]. Make sure to [ETHICAL CONSIDERATION]."

- The templates consistently include elements like bias consideration, clarity, specificity, and ethical boundaries so students practice responsible AI usage every single time

- Students get immediate feedback because they see how their choices affect the AI's response in real time, creating a direct learning loop

- The fill-in-the-blank format naturally gets students thinking about context, audience, and ethical implications without feeling like homework

---

## **Educational Benefits:**

- Students learn by doing instead of just reading theory, with guardrails that prevent common mistakes

- After completing multiple guided prompts, students start to internalize what makes a prompt effective and responsible

- Builds confidence because students have a framework to work within rather than facing a blank screen

- By the time they finish all modules, they've practiced responsible prompt engineering dozens of times and developed real-world skills

---

## **Gamification:**

- Students earn the "Intelligent Instructor" badge when they create particularly well-structured prompts

- The system evaluates based on specificity, ethical considerations, clear context, and appropriate scope

- Rewards students for actually internalizing lessons rather than just completing modules for completion's sake

---

## 📊 **Database Integration**

### **What We Store:**

**Prompt Construction Data:**
```
prompt_constructions table:
- Template used (beginner/intermediate/advanced)
- Student's filled-in values for each blank
- Time spent creating the prompt
- Revision count
```

**Response Tracking:**
```
ai_responses table:
- AI-generated response text
- Student rating (helpful/not helpful)
- Whether they regenerated the response
```

**Progress Metrics:**
```
learning_analytics table:
- Total prompts created
- Current template difficulty level
- Badges earned and when
```

### **How Data Drives Learning:**

**For Students:**
- Track badge progress: "2 more quality prompts until Intelligent Instructor badge"
- Get personalized suggestions: "You've mastered beginner templates—try intermediate next"

**For Teachers:**
- Identifies which students need help
- Reveals which template types work best

---

**This creates a feedback loop: students practice → data tracks improvement → badges reward mastery → students are motivated to improve further.**