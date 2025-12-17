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

# Computer Science Section Progress: Building Interactive Coding Tools - Anishka

## Overview of Improvements

We transformed our Computer Science learning module from a static reference page into an **interactive coding environment** tailored for student engagement. The original design (screenshot 1) featured basic tips, but lacked hands-on practice. 

<img src="{{ '/images/ai/original_cs_prompt.png' | prepend: site.baseurl }}" alt="Original CS Prompt" width="300">

Our new implementation includes three interactive activities:

1. **Fill-in-the-Blank Coding** - Complete code snippets with dropdown menus
2. **Building Challenges** - Write and execute code from scratch
3. **CS Prompt Analyzer** - Get real-time feedback on prompt quality + AI code generation

<img src="{{ '/images/ai/improved_cs_prompt.png' | prepend: site.baseurl }}" alt="Improved CS Prompt" width="500">

## Key Feature: Prompt Analyzer with Code Generation

The most innovative addition is our **AI-powered prompt improvement system** that helps students learn effective prompting while generating executable code.

### Frontend Implementation
```javascript
// Generate code from improved prompt using Gemini API
async function generateCodeFromPrompt() {
    const improvedPrompt = document.getElementById('cs-improved-text').textContent.trim();
    
    // Create a more specific prompt for code generation
    const codeGenPrompt = improvedPrompt + 
        '\n\nIMPORTANT: Provide ONLY the complete, executable code...';
    
    // Call Gemini API
    const response = await fetch('http://localhost:8001/api/gemini', {
        method: 'POST',
        credentials: 'include',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            text: codeGenPrompt,
            prompt: ''
        })
    });
    
    const data = await response.json();
    if (data.success && data.text) {
        // Extract code from markdown code blocks
        let code = data.text;
        const codeBlockMatch = code.match(/```(?:python|javascript)?\n([\s\S]*?)```/);
        if (codeBlockMatch) {
            code = codeBlockMatch[1];
        }
        document.getElementById('cs-generated-code-editor').value = code.trim();
    }
}
```

**What this does:** When you click "Generate Code", this function grabs your improved prompt, sends it to the Gemini AI, and then pulls out just the code part (no extra explanations) to display in the editor. It's basically stripping away the markdown formatting to give you clean, runnable code.

### Backend: Prompt Improvement (Sequencing + Selection)
```python
def generate_improved_prompt(prompt):
    """Generate improved version using sequencing and selection"""
    improved = prompt
    
    # SEQUENCING: Step 1 - Add language if missing
    languages = ['python', 'javascript', 'java', 'c++']
    has_language = any(lang in improved.lower() for lang in languages)
    if not has_language:  # SELECTION
        improved = f"In Python, {improved.lower()}"
    
    # SEQUENCING: Step 2 - Add detail if too short
    if len(improved) < 30:  # SELECTION
        improved += " with step-by-step explanation and examples"
    
    # SEQUENCING: Step 3 - Add context for beginners
    context_words = ['beginner', 'simple', 'example']
    has_context = any(word in improved.lower() for word in context_words)
    if not has_context:  # SELECTION
        improved += ". Explain it in simple terms for beginners."
    
    # SEQUENCING: Step 4 - Ensure proper formatting
    improved = improved[0].upper() + improved[1:] if improved else improved
    if improved and not improved.endswith('.'):
        improved += '.'
    
    return improved
```

**What this does:** This takes your vague prompt and makes it better step-by-step. First it adds a programming language if you forgot one, then adds more detail if it's too short, then makes sure it's beginner-friendly, and finally cleans up the formatting. It goes through these steps in order (sequencing) and only makes changes when needed (selection).

ruchika- science module- topic filters through to show only questions for that topic (show array!) talk more about iteration (for each loop)
anishka- computer science module- prompt goes to smth smth....
michelle- questions + jokes- procedure for jokes

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


## ✅ Requirements Met

- ✅ **Input:** Button clicks (Got It / Need Help)
- ✅ **List:** `data` array from API
- ✅ **Procedure:** `loadConceptsDataForModal` with algorithm
- ✅ **Algorithm:** Sequencing + Selection + Iteration
- ✅ **Calls:** Called when modal opens
- ✅ **Output:** Displays concept table with vote counts

git 
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