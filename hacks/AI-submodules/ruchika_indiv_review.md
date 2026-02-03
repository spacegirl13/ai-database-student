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

# Ruchika Kench - Individual Review Blog (Badges Backend and Survey Frontend)

## How I improved :

### Badges Backend


Before: Static backend with only stored values of badge name, description, requirements, and photo

After: Transactional Data!! Yay

Process:

- Was a very, long interative process but I was super glad to learn about this concept!

- Started by creating transactional **user_badges** and **badges** table (modeled after users table)

- Used different calling method in backend to match new transactional data 

- Made foreign key cascade relationship (Akshara)

- Tables are interactive (you can add or delete user's badges) and displayed on admin UI!

<img width="1849" height="813" alt="Image" src="https://github.com/user-attachments/assets/c58a8f82-d1e6-4110-a3d1-8e8451f27973" />

<img width="1856" height="766" alt="Image" src="https://github.com/user-attachments/assets/24f9addf-f104-429e-8daa-0f56685dc38f" />

#### BE - FE connection

- Frontend references BE data table to reward badges and analytics/data collection displayed on Admin UI tables

### Submodule 1 Survey Frontend

1. Clean, distraction-free survey layout with minimal CSS

<img width="1530" height="403" alt="Image" src="https://github.com/user-attachments/assets/66ec0cd1-cb5f-40ee-89d0-a173d8022569" />

Why this matters:


It forces comparison between tools → students start thinking about a comparison of different AI resources for their education topics

2. Badge earned popup!

<img width="1177" height="567" alt="Image" src="https://github.com/user-attachments/assets/71072d2c-ccc0-43c0-94a9-0c5167ac8f31" />

User gets the “Sensational Surveyor” reward after submitting the survey.

What it does:

Makes participation feel more fun with tangible rewards.

3. Results visualization with bar charts

<img width="1236" height="716" alt="Image" src="https://github.com/user-attachments/assets/dfb179f8-fefb-464d-9a7d-93c67ae582cc" />

What it does: 

Turns personal responses into community data.

- Allows students to compare the popularity of AI for subjects and maybe influence their own decision on what AI to use

## Goals for Next week

1. Get some analytics on most common badges/submodule completion with staged data (only 5 users so far)

2. Fully implement the new Math and CS completion badges (work in progress)

3. Add more staged data for the survey

## How I plan to improve over the next few weeks

I plan to improve by adding more analytics to enhance the metrics and data of the badges and survey tables through adding some staged data. I will also take into consideration ways to display the Admin UI badges tables better and make the badges boxes more theme-fitting.


