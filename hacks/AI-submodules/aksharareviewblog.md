---
layout: post
title: "Akshara - Individual Review Blog"
permalink: /digital-famine/aksharareview/blog
author: Akshara Shankar
microblog: true
date: 2026-2-2
---
## Process for Badges Front end:
- Started by creating a Flask Blueprint (survey_api) to isolate survey logic from the rest of the app
- Designed relational tables (SurveyResponse and AIToolPreference) to store survey submissions and per-subject AI tool choices
- Implemented server-side aggregation using SQLAlchemy (GROUP BY, COUNT) so the frontend only receives processed data
- Used optional authentication (@optional_token) to support both logged-in and anonymous users seamlessly
- - Generated anonymous usernames dynamically when no session exists, avoiding blocked submissions
Added badge-awarding logic directly in the backend to ensure rewards can’t be spoofed from the frontend
- Returned updated aggregated results + badge status in the same response to enable instant frontend updates
- Wrapped all database writes in commit/rollback logic to keep data consistent and safe

<img width="329" height="372" alt="Image" src="https://github.com/user-attachments/assets/3d4ae174-8e32-4001-8841-ce81d85073a3" />
## What I Worked On/Implemented:
- Badges front end:
 - I made sure the front end creates a fixed badge panel in the top right corner. 
    - badgeList is dynamically filled with badges using JavaScript
- I made the badge notification pop up      
    - This shows a model popup when a badge is earned
- Async function to call the backend to get previously earned badges
<img width="800" height="567" alt="Image" src="https://github.com/user-attachments/assets/a17774e5-d6ff-4943-a3e1-9257240cb47d" />
- Leaderboard Front end: I made the front end so that the leaderboard shows up at the corner of the page, and fetches score from the backend
<img width="366" height="616" alt="Image" src="https://github.com/user-attachments/assets/2d51531c-96a2-48b7-9266-7ed4364c62c0" />


Login: 
    - I used the checkAuth () command so if loggen in the user card is shown if not it will show login/signup. 
    - Its made using a cookie-based session authentication with automatic guest account creation to keep login friction low and persist user state across page reloads.
    <img width="500" height="700" alt="Image" src="https://github.com/user-attachments/assets/c20f30aa-fd10-4f66-8adf-1e5aec9d0abf" />
- Foreign Key - 
    - I made the foreign key which is a cascade relationship. This means when one thing is deleted from the table, the database deltes all the posts belonging to that user. 

## Future Goals:
- Add badges for the completetion of the Math and CS portion - Work with Ruchika
- Add more commits for analytics review
## How I WANT to improve:
I will work with my team mates and work hard on finishing the badge for the completed of Math and Computer science. I also want to work on adding more staged data for the survey. I will do this by using class time efficiently and putting in the needed effort at home as well.






