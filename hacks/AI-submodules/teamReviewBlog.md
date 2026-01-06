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

## Varada: Submodule 1

## List Implementation in Backend

![Line 25 - Empty List]({{site.baseurl}}/images/teamreviewblog/line25.png)

This is an empty list that stores all student responses to the FRQ. 

<img src="{{ '/images/teamreviewblog/line63.png' | prepend: site.baseurl }}" alt="Original CS Prompt" width="300">

### Dictionary Creation and Insertion

When someone completes the survey, a dictionary with their answer and time stamp is created. The latest response becomes the 1st one on the list, and the website displays the 3 most recent responses.

## Ruchika: Submodule 2
### Topic filtering procedure for the Science Module
The user selects a button to choose which science topic they want to study. 

![Topic selection screenshot]({{site.baseurl}}/hacks/AI-submodules/topic.png)

This input goes to the backend and displays the questions according to the science topic of the user's choice. Flexible dataset allows the question to be altered with immediate updates to the frontend.

![Science prompts screenshot]({{site.baseurl}}/hacks/AI-submodules/scitopic.png)

Backend:
![Backend flow diagram]({{site.baseurl}}/hacks/AI-submodules/backend.png)

### Iteration: 
This is shown through "forEach" loop when going through and filtering the data array of questions to show the relevant subject.
![forEach example]({{site.baseurl}}/hacks/AI-submodules/forEach.png)

When someone completes the survey, a dictionary with their answer and time stamp is created. The latest response becomes the 1st one on the list, and the website displays the 3 more recent responses. 

## Computer Science Section: Prompt Analyzer - Anishka
---
## Overview of Improvements

We transformed our Computer Science learning module from a static reference page into an **interactive coding environment** tailored for student engagement. Our implementation includes three interactive activities:

1. **Fill-in-the-Blank Coding** - Complete code snippets with dropdown menus
2. **Building Challenges** - Write and execute code from scratch
3. **CS Prompt Analyzer** - Get real-time feedback on prompt quality + AI code generation

<img src="{{ '/images/ai/improved_cs_prompt.png' | prepend: site.baseurl }}" alt="Improved CS Prompt" width="500">

## Key Feature: Prompt Quality Analyzer

The most innovative addition is our **AI-powered prompt analysis system** that evaluates coding prompts and provides quality feedback with detailed scoring.

---

## Procedure: `perform_prompt_analysis`

| Component | Value |
|-----------|-------|
| **Name** | `perform_prompt_analysis` |
| **Parameter** | `prompt` (string - the user's coding prompt text) |
| **Return** | dictionary containing `checklist`, `score`, and `total` |
| **Purpose** | Analyzes coding prompt quality and returns detailed feedback |

---

## Algorithm Components

| Component | Evidence |
|-----------|----------|
| **SEQUENCING** | Step 1: Check language → Step 2: Check action → Step 3: Check details → Step 4: Check context |
| **SELECTION** | Multiple `if` statements checking conditions (`if has_language`, `if has_action`, etc.) |
| **ITERATION** | Explicit `for` loops iterating through `languages` list and `actions` list |

---

## Lists Used in Procedure

1. **`languages` list** - Contains programming languages to check: `['python', 'javascript', 'java', 'c++', 'c#', 'ruby', 'go', 'rust', 'html', 'css']`
2. **`actions` list** - Contains action verbs to check: `['explain', 'debug', 'create', 'write', 'help', 'show', 'fix', 'build', 'implement']`
3. **`context_words` list** - Contains context indicators: `['beginner', 'simple', 'step-by-step', 'example', 'comments', 'for', 'with']`
4. **`checklist` list** - Stores analysis results as dictionaries with `item` and `passed` keys

---

## Backend Implementation: Complete Algorithm
```python
def perform_prompt_analysis(prompt):
    """Analyze coding prompt quality using sequencing, selection, and iteration"""
    checklist = []  # List to store analysis results
    score = 0

    # SEQUENCING: Step 1 - Check for programming language
    # LIST: languages array
    languages = ['python', 'javascript', 'java', 'c++', 'c#', 'ruby', 'go', 'rust', 'html', 'css']
    has_language = False
    
    # ITERATION: Loop through languages list
    for lang in languages:
        if lang in prompt.lower():  # SELECTION: Check if language exists
            has_language = True
            break
    
    checklist.append({
        'item': 'Specifies programming language',
        'passed': has_language
    })
    if has_language:  # SELECTION: Conditionally add points
        score += 25

    # SEQUENCING: Step 2 - Check for specific action verb
    # LIST: actions array
    actions = ['explain', 'debug', 'create', 'write', 'help', 'show', 'fix', 'build', 'implement']
    has_action = False
    
    # ITERATION: Loop through actions list
    for action in actions:
        if action in prompt.lower():  # SELECTION: Check if action exists
            has_action = True
            break
    
    checklist.append({
        'item': 'Uses clear action verb',
        'passed': has_action
    })
    if has_action:  # SELECTION: Conditionally add points
        score += 25

    # SEQUENCING: Step 3 - Check for sufficient detail
    has_details = len(prompt) > 20  # SELECTION: Check length
    checklist.append({
        'item': 'Includes sufficient detail',
        'passed': has_details
    })
    if has_details:  # SELECTION: Conditionally add points
        score += 25

    # SEQUENCING: Step 4 - Check for context
    # LIST: context_words array
    context_words = ['beginner', 'simple', 'step-by-step', 'example', 'comments', 'for', 'with']
    has_context = False
    
    # ITERATION: Loop through context words
    for word in context_words:
        if word in prompt.lower():  # SELECTION: Check if context word exists
            has_context = True
            break
    
    checklist.append({
        'item': 'Provides context or level',
        'passed': has_context
    })
    if has_context:  # SELECTION: Conditionally add points
        score += 25

    return {
        'checklist': checklist,
        'score': score,
        'total': 100
    }
```

---

## Frontend: Where the Procedure is Called

The procedure is called from the frontend when a user clicks the "Analyze Prompt" button:
```javascript
// User clicks "Analyze Prompt" button (INPUT)
async function analyzeCSPrompt() {
    const prompt = document.getElementById('cs-prompt-input').value.trim();

    if (!prompt) {
        alert('Please enter a prompt first!');
        return;
    }

    try {
        // Call the backend procedure
        const response = await fetch('http://localhost:8001/api/prompts/analyze', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ prompt: prompt })  // Send prompt parameter
        });

        const data = await response.json();
        const { checklist, score } = data.analysis;

        // Display results (OUTPUT)
        const checklistDiv = document.getElementById('cs-checklist');
        checklistDiv.innerHTML = checklist.map(item =>
            `<div class="flex items-center gap-2">
                <span class="text-${item.passed ? 'green' : 'red'}-400">
                    ${item.passed ? '✅' : '❌'}
                </span>
                <span>${item.item}</span>
            </div>`
        ).join('');

        const scoreDiv = document.getElementById('cs-score');
        let scoreColor = score >= 75 ? 'green' : score >= 50 ? 'yellow' : 'red';
        scoreDiv.textContent = `Score: ${score}/100`;
        scoreDiv.className = `text-center p-3 rounded-lg font-bold text-xl bg-${scoreColor}-900/30 text-${scoreColor}-300`;

        document.getElementById('cs-analysis').classList.remove('hidden');

    } catch (error) {
        console.error('Error:', error);
        alert('Error: Could not connect to server. Make sure the backend is running.');
    }
}
```

---

## ✅ AP Requirements Met for Anishka's Section

- ✅ **Input:** User types coding prompt into text area
- ✅ **List:** Multiple lists used (`languages`, `actions`, `context_words`, `checklist`)
- ✅ **Procedure:** `perform_prompt_analysis(prompt)` with name and parameter
- ✅ **Algorithm:** Sequencing (4 steps) + Selection (if statements) + Iteration (for loops)
- ✅ **Calls:** Called from frontend `analyzeCSPrompt()` function when button clicked
- ✅ **Output:** Displays checklist and score in UI with visual feedback

---

**What this does:** When a student types a coding prompt and clicks "Analyze Prompt", the text is sent to the backend `perform_prompt_analysis` procedure. The procedure systematically checks the prompt by iterating through lists of programming languages, action verbs, and context words. For each category, it uses selection (if statements) to determine if the requirement is met, then adds the result to a checklist. The procedure returns a detailed analysis with a score out of 100, which is displayed to the student with color-coded feedback showing exactly what makes a good coding prompt.

## Michelle: Procedure for Jokes
# Check for Understanding - AP Requirements

## Procedure: `loadConceptsDataForModal`

| Component | Value |
|-----------|-------|
| **Name** | `loadConceptsDataForModal` |
| **Parameters** | None (accesses global variables) |
| **Return** | void |
| **Purpose** | Loads AI concepts from backend and displays in table |

---

## Algorithm Components

| Component | Evidence |
|-----------|----------|
| **SEQUENCING** | Fetch → Parse → Clear → Loop → Display (7 sequential steps) |
| **SELECTION** | `if (response.status !== 200)` checks for errors |
| **ITERATION** | `for (const row of data)` loops through concept array |

---

## Code with AP Requirements
```javascript
// STUDENT-DEVELOPED PROCEDURE
function loadConceptsDataForModal() {
    // SEQUENCING - Step 1: Fetch data
    fetch(getConceptsURLModal, CONCEPTS_API_CONFIG_MODAL.fetchOptions)
        .then(response => {
            // SELECTION - Check if successful
            if (response.status !== 200) {  // ← SELECTION
                conceptsErrorModal('GET API response failure');
                return;
            }
            
            // SEQUENCING - Step 2: Parse JSON
            response.json().then(data => {
                // SEQUENCING - Step 3: Clear table
                conceptsResultContainerModal.innerHTML = '';

                // ITERATION - Loop through list
                for (const row of data) {  // ← ITERATION
                    // SEQUENCING - Step 4: Create row
                    const tr = document.createElement("tr");

                    const concept = document.createElement("td");
                    concept.innerHTML = row.id + ". " + row.joke;

                    const gotIt = document.createElement("td");
                    const gotItBtn = document.createElement('button');
                    gotItBtn.innerHTML = row.haha;
                    gotItBtn.onclick = function () {
                        conceptReactionModal(HAHA_KEY_MODAL, 
                            likeConceptsURLModal + row.id, gotItBtn.id);
                    };
                    gotIt.appendChild(gotItBtn);

                    const needHelp = document.createElement("td");
                    const needHelpBtn = document.createElement('button');
                    needHelpBtn.innerHTML = row.boohoo;
                    needHelp.appendChild(needHelpBtn);

                    // SEQUENCING - Step 5: Append to table
                    tr.appendChild(concept);
                    tr.appendChild(gotIt);
                    tr.appendChild(needHelp);
                    conceptsResultContainerModal.appendChild(tr);
                }
            })
        })
}
```

---


## Akshara: How the Frontend and Backend of the Feedback Survey are Connected:
Frontend sends POST request with JSON data
<img width="1198" height="454" alt="Image" src="https://github.com/user-attachments/assets/845bfcd8-fa1f-4b08-9f0f-db3c76776f5e" />
Backend receives the POST request at matching route:
<img width="1122" height="188" alt="Image" src="https://github.com/user-attachments/assets/1f6d81b9-85ef-45ec-b335-b4ad596f56bd" />
Backend validates the exact fields frontend sent:
<img width="1128" height="210" alt="Image" src="https://github.com/user-attachments/assets/32954fad-f09e-4e65-b66b-678853c8c75f" />
Backend returns JSON response:
<img width="1014" height="196" alt="Image" src="https://github.com/user-attachments/assets/b3bceda1-5751-43fd-b921-73ecb4eadb01" />
Frontend receives and processes backend's response:
<img width="910" height="240" alt="Image" src="https://github.com/user-attachments/assets/e3402983-f783-4e42-b1de-e9c6681f8b4a" />

---

# 🎯 Team Summary: AP CSP Component A Requirements

## How Our Project Collectively Meets ALL Requirements

Our AI Usage Quest educational platform demonstrates all required AP CSP Component A elements across our team's implementations:

### ✅ **Requirement 1: Input**
- **Varada:** Survey form submission (user text responses)
- **Ruchika:** Topic selection buttons (biology/chemistry/physics)
- **Anishka:** Coding prompt text input via text area
- **Michelle:** Button clicks for joke reactions (Got It / Need Help)
- **Akshara:** Feedback form with rating, category selection, and comments

### ✅ **Requirement 2: List/Collection Type**
- **Varada:** `prompt_history` list stores all student FRQ responses as dictionaries
- **Ruchika:** `data` array containing science questions filtered by topic
- **Anishka:** Multiple lists including:
  - `languages` list: `['python', 'javascript', 'java', 'c++', 'c#', 'ruby', 'go', 'rust', 'html', 'css']`
  - `actions` list: `['explain', 'debug', 'create', 'write', 'help', 'show', 'fix', 'build', 'implement']`
  - `context_words` list: `['beginner', 'simple', 'step-by-step', 'example', 'comments', 'for', 'with']`
  - `checklist` list: stores analysis results as dictionaries
- **Michelle:** `data` array from API containing jokes with vote counts
- **Akshara:** Survey responses stored as list of dictionaries

### ✅ **Requirement 3: Student-Developed Procedure with Parameters**
**Primary Example - Anishka's `perform_prompt_analysis(prompt)`:**
- **Name:** `perform_prompt_analysis`
- **Parameter:** `prompt` (string containing user's coding prompt)
- **Return type:** dictionary with keys `checklist`, `score`, `total`
- **Purpose:** Analyzes coding prompt quality and returns detailed feedback with scoring

### ✅ **Requirement 4: Algorithm with Sequencing, Selection, AND Iteration**
**Demonstrated in Anishka's `perform_prompt_analysis(prompt)`:**

**SEQUENCING (4 ordered steps):**
1. Check for programming language → 
2. Check for action verb → 
3. Check for sufficient detail → 
4. Check for context/level

**SELECTION (conditional logic):**
- Multiple `if` statements: `if has_language:`, `if has_action:`, `if has_details:`, `if has_context:`
- Each condition determines whether to add 25 points to the score

**ITERATION (explicit loops):**
- `for lang in languages:` - loops through 10 programming languages
- `for action in actions:` - loops through 9 action verbs  
- `for word in context_words:` - loops through 7 context words
- Each loop searches the prompt for matching terms

### ✅ **Requirement 5: Calls to Student-Developed Procedure**
**Anishka's procedure is called from frontend JavaScript:**
```javascript
async function analyzeCSPrompt() {
    const prompt = document.getElementById('cs-prompt-input').value.trim();
    
    // Calls the backend procedure via API
    const response = await fetch('http://localhost:8001/api/prompts/analyze', {
        method: 'POST',
        body: JSON.stringify({ prompt: prompt })
    });
    
    const data = await response.json();
    // Uses returned checklist and score
}
```
**When called:** User clicks "Analyze Prompt" button after typing their coding prompt

### ✅ **Requirement 6: Output**
- **Varada:** Visual display of 3 most recent survey responses with timestamps
- **Ruchika:** Filtered science questions displayed based on selected topic
- **Anishka:** Visual output showing:
  - Color-coded checklist (✅/❌ for each quality criterion)
  - Numerical score out of 100
  - Color-coded score display (green/yellow/red based on quality)
- **Michelle:** Interactive joke table with vote counts that update on button click
- **Akshara:** Success/error messages and form validation feedback
