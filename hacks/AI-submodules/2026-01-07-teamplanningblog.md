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

## Code Runner (Michelle and Varada)

### Guided Prompt Builder: Teaching Responsible AI Through Scaffolded Learning

Our Code Runner feature helps students learn how to write good AI prompts by giving them sentence starters and fill in the blank templates. Instead of staring at a blank box wondering what to write, students get a structured framework that guides them through creating effective and responsible prompts. They fill in the blanks based on their needs, and then the AI generates a response using their completed prompt. This way they're learning what good prompts look like while actually using them.

**Key Features:**

- Students see partially completed prompts with blanks to fill in, like "I need help understanding [TOPIC]. Please explain it in a way that is [TONE/STYLE] and includes [SPECIFIC REQUIREMENTS]. Make sure to [ETHICAL CONSIDERATION]."

- The templates consistently include elements like bias consideration, clarity, specificity, and ethical boundaries so students practice responsible AI usage every single time

- Students get immediate feedback because they see how their choices affect the AI's response in real time, creating a direct learning loop

- The fill in the blank format naturally gets students thinking about context, audience, and ethical implications without feeling like homework

**Educational Benefits:**

- Students learn by doing instead of just reading theory, with guardrails that prevent common mistakes

- After completing multiple guided prompts, students start to internalize what makes a prompt effective and responsible

- Builds confidence because students have a framework to work within rather than facing a blank screen

- By the time they finish all modules, they've practiced responsible prompt engineering dozens of times and developed real world skills

**Gamification:**

- Students earn the "Intelligent Instructor" badge when they create particularly well structured prompts

- The system evaluates based on specificity, ethical considerations, clear context, and appropriate scope

- Rewards students for actually internalizing lessons rather than just completing modules for completion's sake

