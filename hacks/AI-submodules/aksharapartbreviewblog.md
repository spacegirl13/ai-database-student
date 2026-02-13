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
## Overall Purpose: 
The purpose of AI study buddy is to help students use AI responsibly and in a way to help create prompts to improve their learning and study strategies. 
### PROMPT 1: Program Purpose and Function
The purpose of my program is to collect feedback from users at the end of a learning submodule.
Users can rate the submodule and write comments, which are stored in a database.
## Explain how the Program Functions 
When a user finishes a submodule, a feedback popup appears.
The user:
- Clicks a star rating (1–5 stars)
- Clicks a category button (Bug, Feature Request, General, Other)
Types optional comments
The backend stores the feedback in a database.
The program displays previous feedback so users can see what others said.
## Inputs:
Star rating (1–5 stars)
Category button 
Optional comment text
User ID (if logged in)
## Outputs:
Stored feedback entry in the database
Feedback list shown on the screen (past feedback)
JSON data with rating, category, comment, username, and timestamp
## List Initialization 
```
sample_feedback = [
    {"title": "Great platform", "body": "Loved it", "type": "General"},
    {"title": "content quality", "difficulty level": "clarity", "pacing": "other"}
]
```

he program uses a list to store feedback entries. 
## How the list manages complexity
- Stores many feedback records in one structure.
- A loop processes all feedback automatically.
- Prevents writing separate variables for each feedback entry.
Without the list
## Would need many variables like fb1, fb2, fb3.
- Would need repeated database insert code.
- Adding new feedback would require rewriting the program.
- Using a list makes the program shorter and easier to update.
## PROMPT 2c: Procedure and Algorithm
The procedure is the create() method: 
def create(self):
    db.session.add(self)
    db.session.commit()
    return self
## How it helps the program
Saves feedback to the database.
Runs every time a user submits the survey.
Makes feedback permanent.
## Procedure Call
``` feedback = Feedback("Nice lesson", "Loved the quizzes", "General", user_id=5)
feedback.create() ```
- (putting the feedback in the container named feedback)
## Algorithm (Sequence, Selection, Iteration)
## Sequence
User clicks stars and category button.
User types optional comments.
Program creates a Feedback object.
Feedback is stored in the database.
Previous feedback is displayed.
## Selection (if statement)
if self.user_id:
- The code if self.user_id: is asking: "Does this feedback belong to a specific person who is logged in?
- If user is logged in → show username
- If not → show “Anonymous”
## Iteration (loop)
for fb_data in sample_feedback:
The loop accesses each element in the list one at a time. For each feedback entry, the program creates a Feedback object and saves it to the database. This repeats until all feedback entries are processed.
## Testing and Debugging
Testing
- Submitted feedback with 5 stars → saved correctly.
- Submitted anonymous feedback → showed “Anonymous”.
- Tested category buttons.
- Checked that previous feedback displays
Bug Found
Sometimes feedback did not save due to a database error.
Fix
Added rollback:
except IntegrityError:
    db.session.rollback()
Now the program does not crash.
## REQUIRED PPR QUESTIONS SECTION
Procedure & Selection
First if-statement: 
if self.user_id:
What happens if false:
If the condition is false, the program assigns the username as "Anonymous" and continues storing the feedback.
Why it matters:
This prevents errors when users are not logged in and still allows anonymous feedback.
## Procedural Abstraction (Parameters)
Constructor:
``` 
def __init__(self, title, body, type="Other", user_id=None):
```
Parameters:
title, body, type, user_id
How they manage complexity:
Same procedure works for all feedback types and users.
No separate functions for bugs, features, or comments.
## Procedure Calls and Testing 
Call 1 : 
```
Feedback("Great module", "Very helpful", "General", 2).create()
```
Call 2L
Feedback("Bug found", "Stars not working", "Bug").create()
Different calls store different feedback.
## Logic Error Example
Wrong Code: 
``` 
self.user_id = 1
```
Effect: 
- All feedback shows user 1, even anonymous users.
- Incorrect usernames displayed.
## List Utilization
The feedback list:
Stores multiple feedback records.
Loop accesses each entry and saves it.
Reduces repeated code.
## Algorithm Analysis (Iteration)
In the loop:
for fb_data in sample_feedback:
Steps:
1. Take one feedback entry from the list.
2. Create a Feedback object.
3. Add it to the database.
4. Repeat for all entries.
## Steps for Feedback Survey Backend
1. Class Feedback(db.Model)
- This creates a table in the database called feedback
- Each row is one feedback and the columns are different questions.
2. id = db.Column (db.integer, primary_key = True)
- Unique ID for each feedback entry
- unique number for each user
3. user_id = db.Column(db.integer, db.foreignKey ('user.id')))
- Links user id to number, stores which user submitted what
- anonymous feedback allowed
- Foregin key allows duplicates (primary doesn't)
3. title = db.Comlumn (db.string(255), nullable = False)
- Nullable = false means it can't be empty
type = db.Column (db.String(64), default = "other")
- categories of feedback 
- stored in Database
4. def create(self)
- adds feedback to database
- permanently saves
5. def read(self)
- processes feedback to be sent to front end
 db.create_all()
creates table