---
layout: post
title: "Ruchika Kench - Individual Review Blog"
permalink: /digital-famine/ai/ruchikareview
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Team Crashers
microblog: true
date: 2026-1-30
---

# Ruchika Kench - Individual Review Blog (Badges Backend)

## How I improved :

### Badges Backend

**Before:**
- Static backend with only stored values of badge name, description, requirements, and photo. 

- Badges were also not showing up accurately according to an event-based award. 

- No metrics of badge data tied to user (absence of Admin UI tables)

**After:** Transactional Data!! Yay

#### Process:

- Was a very, long interative process but I was super glad to learn about this concept!

- Started by creating transactional **user_badges** and **badges** table (modeled after users table)

- Many to many relationship with create() and delete() commands

- Made foreign key cascade relationship (Akshara)

- Tables are interactive (you can add or delete user's badges) and displayed on admin UI!

<img width="1849" height="513" alt="Image" src="https://github.com/user-attachments/assets/c58a8f82-d1e6-4110-a3d1-8e8451f27973" />

<img width="1856" height="566" alt="Image" src="https://github.com/user-attachments/assets/24f9addf-f104-429e-8daa-0f56685dc38f" />

#### BE - FE connection

- Frontend references BE data table to reward badges and analytics/data collection displayed on Admin UI tables

- Tied to user login (No login, No badges!) and stays with user for entire quest

## Goals for next few weeks

- Incorporate staged data to add more data on user_badges transactional table (only 5 users so far)

- Track analytics and metrics of certain badge acheivement (frequency)

- Maybe get metrics on badge acheivement vs. leaderboard position (compare tables)

- Complete new math and cs submodule completion badge (Work in progress!)

## How I plan to improve over the next few weeks

I plan to improve by adding more analytics to enhance the metrics and data of the badges tables through adding some staged data. I will also take into consideration ways to display the Admin UI badges tables better and make the badges boxes more theme-fitting.


