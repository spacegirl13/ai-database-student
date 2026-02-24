---
layout: post
title: "Akshara Shankar - PPR"
permalink: /digital-famine/ai/aksharappr
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Akshara Shankar
microblog: true
---
<img width="800" height="696" alt="Image" src="https://github.com/user-attachments/assets/749c23e4-4c21-4fe3-b0c3-d1014b36b763" />
# Akshara Shankar – Feedback Survey Backend PPR
 
## PROGRAM PURPOSE AND FUNCTION
 
**Purpose** 
Collect and store student feedback from submodules, allowing logged-in and anonymous users to submit ratings, categories, and optional comments.
 
**Function** 
- Display a feedback popup after completing a submodule. 
- User clicks stars (1–5), selects category (Bug, Feature Request, General, Other), types optional comments. 
- Feedback saved in the database via the `Feedback` class. 
- Previous feedback displayed on frontend. 
 
**Inputs** 
- `title` (string) 
- `body` (string) 
- `type` (Bug, Feature Request, General, Other) 
- `user_id` (optional, if logged in) 
 
**Outputs** 
- Stored feedback object in the database 
- JSON from `Feedback.read()` with id, user_id, username, title, body, type, timestamp, GitHub issue URL 
- Display list of previous feedback 
 
---
 
## LIST INITIALIZATION & USAGE
 
**List Initialization (`initFeedback()` function)** 
 
```python
sample_feedback = [
    {
        "title": "Great learning platform!",
        "body": "I really enjoyed using AI Study Buddy. The prompts and challenges helped me understand AI concepts better.",
        "type": "Content Quality",
    },
    {
        "title": "Feature request: Dark mode",
        "body": "It would be great to have a dark mode option for studying at night.",
        "type": "Other",
    },
    {
        "title": "Bug: Survey not loading",
        "body": "Sometimes the survey page takes a long time to load. Could you optimize it?",
        "type": "Other",
    },
    {
        "title": "Love the badge system",
        "body": "The badges motivate me to complete all the submodules. Great gamification!",
        "type": "Engagement",
    },
    {
        "title": "Suggestion for more subjects",
        "body": "Could you add more subjects like economics or social studies to the question bank?",
        "type": "Other",
    }
]
```
## **Purpose of List**
- Stores multiple feedback records in one structure.
- Allows iteration to create Feedback objects and save them to the database.
- Reduces repeated code for each feedback entry.
- Makes adding new feedback easy without rewriting program logic.
 
## Using the List in initFeedback()
```python
for i, fb_data in enumerate(sample_feedback):
    user_id = users[i].id if i < len(users) else None
    feedback = Feedback(
        title=fb_data["title"],
        body=fb_data["body"],
        type=fb_data["type"],
        user_id=user_id
    )
    db.session.add(feedback)
db.session.commit()
```
## Usage Explanation
- Loops through each dictionary in sample_feedback.
- Creates a Feedback object from each entry.
- Adds the object to the database session.
- Commits all entries at once.
## Managing Complexity
With List:
- One structure + loop to handle multiple feedback entries
- Easy to add new feedback
- Minimal repeated code
 
Without List:
- Would require multiple variables (fb1, fb2, fb3…)
- Repeated db.session.add() calls
- Hard to scale or update
## PROCEDURE & ALGORITHM
## Feedback Class Constructor (__init__)
```
def __init__(self, title, body, type="Other", user_id=None):
    self.title = title
    self.body = body
    self.type = type
    self.user_id = user_id
```
## Create Procedure (Feedback.create())
```
def create(self):
    try:
        db.session.add(self)
        db.session.commit()
        return self
    except IntegrityError:
        db.session.rollback()
        return None
```
## Read Procedure (Feedback.read())
```
def read(self):
    return {
        "id": self.id,
        "user_id": self.user_id,
        "username": self.username,
        "title": self.title,
        "body": self.body,
        "type": self.type,
        "created_at": self.created_at.isoformat() if self.created_at else None,
        "github_issue_url": self.github_issue_url
    }
    ```
## Delete Procedure (Feedback.delete())
```
def delete(self):
    db.session.delete(self)
    db.session.commit()
    return None
```
## Username Selection (Feedback.username property)
```
@property
def username(self):
    from model.user import User
    if self.user_id:
        user = User.query.get(self.user_id)
        if user:
            return user.uid
    return "Anonymous"
```
## Sequence, Selection, Iteration in Feedback Submission
```
Sequence:
1. User enters title, body, type, optionally user_id.
2. Feedback object is created.
3. create() stores object in database.
4. read() retrieves feedback for display.
 
Selection (if statement):
- If self.user_id exists, fetch username from User table.
- If not, return "Anonymous".
 
Iteration:
- for fb_data in sample_feedback loop creates and saves all sample feedback entries.
```
## TESTING & DEBUGGING
```
Testing:
- 5-star feedback stored correctly
- Anonymous feedback shows "Anonymous"
- Category buttons recorded
- Previous feedback displayed
 
Bug Found:
- Database IntegrityError prevented saving feedback
 
Fix:
- Added db.session.rollback() in except block to prevent crash
```
## PPR QUESTIONS
## Q1: Selection Statement
Procedure: Feedback.username
- Conditional: if self.user_id
- False path: return "Anonymous"
- Importance: Handles anonymous users safely
## Q2: Parameters / Procedural Abstraction
Procedure: Feedback.__init__(title, body, type="Other", user_id=None)
- Parameters: title, body, type, user_id
- Manages complexity: Single constructor handles all types of feedback
## Q3: Different Calls
```
Feedback("Great module", "Very helpful", "General", 2).create()
Feedback("Bug found", "Stars not working", "Bug").create()
```
## Q4: Logic Error Example
Wrong: self.user_id = 1  → all feedback shows user 1
Effect: Anonymous feedback incorrectly attributed to user 1
## Q5: List Usage
- Accessing: Loop through sample_feedback → create Feedback objects
- Updating: db.session.add() + db.session.commit() once → efficient
## Q6: Algorithm (Iteration Example)
1. Take entry from sample_feedback
2. Create Feedback object
3. Add to database
4. Repeat for all entries
## PPR Meeting Requirements: 
This PPR meets AP CSP requirements because it defines a clear purpose, uses procedures and abstraction, demonstrates sequence, selection, and iteration, manages complexity with lists and loops, includes testing/debugging, and documents all program logic with examples.