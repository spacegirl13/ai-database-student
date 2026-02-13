---
layout: post
title: "Skill C: AI Study Buddy Team Review"
description: "Individual contribution and goals for next week"
permalink: /skill-c-teamreview/
categories: [Review, Progress]
tags: [reflection, goals, development]
author: "Crashers"
date: 2026-02-13
--- 

## Overview

## Ruchika

## Akshara

## Anishka

## Varada

## Michelle

**My Features:** Authentication, Quiz, Feedback, Check for Understanding  
---

## What I Built

I created **Submodule 3** - an interactive quiz teaching students how to write effective AI prompts and use AI ethically.

---

## Feature 1: Authentication

**Users must log in before playing.**

```javascript
// Frontend checks if logged in
const response = await fetch('/api/id', { credentials: 'include' });
if (response.ok) {
    showGame();
} else {
    showLoginScreen();
}
```

```python
# Backend validates JWT token
@token_required()
def get(self):
    return jsonify(g.current_user.read())
```

---

## Feature 2: Quiz System

**10 questions about prompt engineering and AI ethics.**

```python
# Backend serves questions
@game_api.route('/questions')
def get_questions():
    return jsonify(questions_data)
```

**Example Question:**
> "You need help solving: 2x² + 5x - 3 = 0. Which prompt is best?"
> - ❌ "Help with math homework" (too vague)
> - ✅ "Solve 2x² + 5x - 3 = 0 step-by-step using quadratic formula"

---

## Feature 3: Feedback Survey

**Collects star ratings and comments after quiz.**

```python
# Backend saves feedback
@game_api.route('/feedback', methods=['POST'])
def save_feedback():
    rating = request.json.get('rating')
    category = request.json.get('category')
    feedback.create()
```

---

## Feature 4: Check for Understanding

**Tracks which AI concepts students understand.**

```python
# Backend increments "Got It" counter
def addJokeHaHa(id):
    for joke in jokes_data:
        if joke['id'] == id:
            joke['haha'] += 1
```

```javascript
// Frontend prevents duplicate clicks
if (_pendingRequests.has(elemID)) return;
_pendingRequests.add(elemID);
fetch(postURL, { method: 'PUT' });
```

---

## Debugging

### Bug #1: Duplicate Button Clicks
**Problem:** Spam-clicking sent 10+ requests, counter went from 1 → 11.

**Fix:** Added `_pendingRequests` Set to block duplicates.

```javascript
if (el.disabled || _pendingRequests.has(elemID)) {
    return; // Block duplicate
}
```

### Bug #2: Playing Without Login
**Problem:** Users could access quiz without logging in.

**Fix:** Added authentication check on page load.

```javascript
document.addEventListener('DOMContentLoaded', () => {
    checkAuthenticationAndSetup();
});
```

---

## Bulk Data

**`db_migrate.py` - Resets database with test data**

```python
db.drop_all()
db.create_all()
initUsers()         # 3 test users
initQuestions()     # 10 quiz questions
initSurveyResults() # 100 survey responses
```

---

## Peer Feedback

**Changes I made based on teammate feedback:**
- Changed bright colors → softer blue gradients
- Reduced scrolling by 40%
- Added 30-second timer to increase difficulty
- Fixed button text contrast

---

## My Superpower

**Teaching students AI is a tutor, not a cheat sheet.**

**Example ethics question:**
> "Should you use AI to write your entire essay?"
> - ❌ Yes (Plagiarism!)
> - ✅ Use AI to brainstorm, then write your own argument

**Happy moment:** Students debated "Is using AI to brainstorm plagiarism?" in class. They got it! 😊

---

## Tech Stack

- **Frontend:** HTML/CSS/JavaScript
- **Backend:** Python Flask, JWT authentication
- **Database:** SQLite
- **APIs:** RESTful endpoints

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/id` | Check login |
| GET | `/api/prompt-game/questions` | Get quiz questions |
| POST | `/api/submodule3/feedback` | Submit feedback |
| PUT | `/api/jokes/like/:id` | Increment "Got It" |

---