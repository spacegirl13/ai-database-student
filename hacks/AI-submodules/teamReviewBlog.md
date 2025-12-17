---
layout: post
title: "AP CSP AP requirements - Team Review Blog"
permalink: /digital-famine/ai/blog
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Team Crashers
microblog: true
date: 2025-10-30
---
# AP CSP Component A Requirements - Code Analysis

---
layout: post
title: "Team Review Blog - Code Analysis"
categories: [CSP, Team Review]
---

# AP CSP Component A Requirements - Code Analysis

## Varada: Submodule 1

### List Implementation in Backend

![Line 25 - Empty List]({{site.baseurl}}/images/teamreviewblog/line25.png)

This is an empty list that stores all student responses to the FRQ. 

### Dictionary Creation and Insertion

![Line 63 - Dictionary Creation]({{site.baseurl}}/images/teamreviewblog/line63.png)

When someone completes the survey, a dictionary with their answer and time stamp is created. The latest response becomes the 1st one on the list, and the website displays the 3 most recent responses.

## Ruchika: Submodule 2
### Topic filtering procedure for the Science Module
The user selects a button to choose which science topic they want to study. 

![Topic selection screenshot]({{site.baseurl}}/hacks/AI-submodules/topic.png)

This input goes to the backend and displays the questions according to the science topic of the user's choice. Flexible dataset allows the question to be altered with immediate updates to the frontend.


Backend:
![Backend flow diagram]({{site.baseurl}}/hacks/AI-submodules/backend.png)

### Iteration: 
This is shown through "forEach" loop when going through and filtering the data array of questions to show the relevant subject.
![forEach example]({{site.baseurl}}/hacks/AI-submodules/forEach.png)

anishka- computer science module- prompt goes to smth smth....
michelle- questions + jokes- procedure for jokes
akshara- feedback survey FE + BE how they are connected