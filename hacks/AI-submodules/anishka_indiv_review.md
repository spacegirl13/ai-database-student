---
layout: post
title: "Anishka Sanghvi - Individual Review Blog"
permalink: /digital-famine/ai/anishkareview
categories: [CSP, Submodule, AIUsage]
tags: [database, transactional-data, survey, backend]
author: Team Crashers
microblog: true
date: 2026-02-03
---

# Anishka Sanghvi - Individual Review Blog

## How I improved:

### Fixed the transactional data tables

Before: Survey and leaderboard tables had unnecessary columns with duplicate user data

After: Clean transactional data tables

Survey Table:

![Survey Table]({{site.baseurl}}/images/anishka-review/survey_table.png)

Leaderboard Table:
![Survey Table]({{site.baseurl}}/images/anishka-review/leaderboard.png)

Process:

- Cleaned up the survey transactional data table in submodule 1 and the leaderboard table

- Removed unnecessary columns - now they just pull user data from the user table instead

- Makes the database way cleaner and easier to maintain

### Made questions dynamic

Before: All survey questions were hardcoded

After: Database-driven question system

Question Table:
![Survey Table]({{site.baseurl}}/images/anishka-review/question_table.png)


Process:

- Built a database for all the survey questions

- System randomly picks questions based on your chosen topic

- Way easier to add new questions now - just update the database instead of changing code

## Goals for Next Week

1. **Prompt display table for CS activity 3**

Create a table that shows:
- User's name
- Their prompt
- Copy button for easy sharing

![Survey Table]({{site.baseurl}}/images/anishka-review/prompt_table.png)

2. **Add completion badge**

Once you're added to the activity 3 table (meaning you finished it), a badge will show up automatically

## How I plan to improve over the next few weeks

I plan to continue improving the database structure and make sure all the transactional data tables are optimized. I'll also work on connecting the frontend features to the backend tables so everything flows smoothly.