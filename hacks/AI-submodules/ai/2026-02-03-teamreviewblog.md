---
layout: post
title: "Weekly Review: AI Study Buddy Progress"
description: "Individual contribution and goals for next week"
permalink: /weekly-review-feb-03/
categories: [Review, Progress]
tags: [reflection, goals, development]
author: "Crashers"
date: 2026-02-03
--- 

### Pieces that I have been working on - Varada Vichare:

#### Frontend Improvements

I worked on improving the frontend for both Submodule 1 and Submodule 2 based on user feedback. The main goal was to reduce unnecessary scrolling and make everything more concise and accessible.

**Submodule 1 - Before:**
- The "Show Previous Student Responses" section displayed a large amount of text all at once, taking up significant screen space.
- The two FRQ questions were stacked vertically one by one, requiring users to scroll extensively to view both questions.
- The layout felt cluttered and disorganized due to the amount of vertical space consumed.

**Submodule 1 - Improvements:**
- Minimized the "Show Previous Student Responses" section so it doesn't display all the text by default. Users can now click to expand it when they want to view past responses, keeping the interface clean.
- Repositioned the two FRQ questions side by side instead of stacking them vertically. This layout consumes less space and looks significantly more organized and neat.
- Overall reduction in scrolling required, making the user experience much smoother.

<img src="{{site.baseurl}}/images/varadareview/submod1.png" alt="Submodule 1 improvements" width="600">


**Submodule 2 - Before:**
- Selecting subjects for Math and Science would redirect users to an entirely different screen just to pick what they wanted to work on.
- The four questions were displayed in a single vertical line, forcing users to scroll down continuously.
- Activity selection and question displays in the Computer Science section were large and took up excessive space.
- All text and instructions were visible at once, creating a visually overwhelming and distracting experience.

**Submodule 2 - Improvements:**
- Changed the theme and layout for every section to be more compact and user-friendly.
- For Math and Science, replaced the redirect screen with a simple dropdown menu where users can select their desired subjects directly without leaving the page.
- Reorganized the four questions into a square grid layout instead of a vertical stack, which takes up less space and appears more organized.
- For the Computer Science section, created smaller selective buttons for different activities and added dropdowns for the questions themselves, making everything more organized and less distracting.
- Added a question mark button to each section that users can click to expand additional information. This keeps unnecessary text hidden unless specifically needed.
- For the Challenge section, replaced large buttons with a dropdown for selecting difficulty levels and added another dropdown for selecting subjects, so users only see the specific Mad Lib outline or writing prompt relevant to their choice.

<img src="{{site.baseurl}}/images/varadareview/math.png" alt="Math and Science section improvements" width="500">


<img src="{{site.baseurl}}/images/varadareview/compsci.png" alt="Computer Science section improvements" width="500">


<img src="{{site.baseurl}}/images/varadareview/challenge.png" alt="Challenge section improvements" width="500">

### Backend Development

For Submodule 1, I worked on the backend to handle staged data stored in the survey_responses table. The table includes:
- ID
- Whether the user uses AI or not
- FRQ response content
- Timestamp of submission
- Whether the user was rewarded a badge

There is a file called survey_results.py which generates the staged data by taking the ID and all associated information to create a structured table. This is visible on the frontend when you navigate to Submodule 1, where you can see over 100 responses reflected in the scaling of the bar graph, demonstrating real data visualization.

### Future Improvements

Moving forward, I plan to focus on several key enhancements:
- Making buttons clearer and more intuitive for users by improving labels and visual feedback.
- Implementing an overall less bright color scheme to reduce eye strain and create a more professional aesthetic.
- Adding more staged data to the survey responses table to provide a larger dataset for analysis and testing.

---
## How I improved - Anishka Sanghvi:

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

---

## Process for Badges Front end - Akshara Shankar
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

---

## How I improved - Ruchika Kench:

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

## What I've Accomplished - Michelle Ji

### Frontend
- Built interactive quiz with 10 prompt engineering questions
- Implemented drag-and-drop ordering exercises
- Created timer system with visual warnings
- Added badge notification popups with animations
- Integrated user authentication flow

### Backend
- Developed Flask REST API (`/questions`, `/scores`, `/feedback`)
- Created SQLAlchemy models for data persistence
- Built "Check for Understanding" tracking system with concept reactions
- Integrated badge awards with user authentication

### Technical Skills
- Used lists for question banks and game state
- Implemented async/await for API calls
- Created reusable procedures like `loadQuestionsFromBackend()` and `conceptReaction()`
- Built algorithms with sequence, selection, and iteration

---

## Goals for Next Week

1. **Authentication Integration**: Remove manual name entry—automatically use logged-in user's info. Redirect to login page if not authenticated.

2. **Fix Concept Tracking**: Debug the joke/concept increment system (`haha`/`boohoo` counters not updating correctly after clicks)

3. **Clean Up Results Screen**: Remove leaderboard display after quiz completion to focus on personal performance metrics

4. **Badge Display Fix**: Resolve timing issue where badges don't appear immediately after being awarded

5. **Error Handling**: Add loading spinners during API calls and improve error messages

---

## How I'll Improve

### Authentication Check
```javascript
// ✅ New: Verify user is logged in before starting
async function startGame() {
    const user = await checkAuthentication();
    
    if (!user) {
        window.location.href = '/login';
        return;
    }
    
    playerName = user.username;  // Auto-populate from session
    gameQuestions = shuffleArray(allQuestions);
    loadQuestion();
}
```

### Fix Concept Reaction Updates
```javascript
// Current issue: Counters don't refresh properly
function conceptReaction(type, postURL, elemID) {
    const options = {
        ...fetchOptions,
        method: 'PUT',
    };

    fetch(postURL, options)
        .then(response => response.json())
        .then(data => {
            // ✅ Add immediate visual feedback
            const button = document.getElementById(elemID);
            button.innerHTML = data[type];  // Update count immediately
            button.style.animation = 'pulse 0.3s';  // Visual confirmation
        })
        .catch(err => console.error('Update failed:', err));
}
```

### Simplified Results Display
```javascript
// ✅ Remove leaderboard section from results screen
async function endGame() {
    showScreen('resultsScreen');
    const results = calculateFinalResults();
    await saveGameData(results);
    updateResultsDisplay(results);
    // Removed: loadLeaderboard() call
}
```

### Better Error Messages
```javascript
// ❌ Before
catch (error) {
    alert('Error!');
}

// ✅ After
catch (error) {
    if (error.message.includes('network')) {
        showErrorMessage('Unable to connect. Check your internet.');
    } else if (error.status === 401) {
        showErrorMessage('Please log in to continue.');
        setTimeout(() => window.location.href = '/login', 2000);
    }
}
```

---

## Reflection

**Wins**: Successfully built full-stack app with authentication, real-time concept tracking, and badge system 

**Challenges**: Async timing for badge awards, concept counter refresh, authentication flow 

**Key Lesson**: Always check user authentication state before allowing access to features 

**Next Priority**: Integrate auth check, fix concept tracking, streamline results display 

---