---
layout: post
title: "AP CSP AP requirements - Ruchika"
permalink: /digital-famine/ai/blog/Ruchika
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Ruchika Kench
microblog: true
date: 2025-10-30
---
# AP CSP Component A Requirements - Code Analysis

## 1. ✅ Instructions for Input from User

### Science Section - Frontend
User clicks buttons to select topics (biology/chemistry/physics), triggering events that store input and load corresponding questions. This is explicit user interaction that drives program behavior.
```javascript
function selectScienceTopic(topic) {
    selectedScienceTopic = topic;
    // ... updates UI and loads questions
}
```

<img width="752" height="803" alt="Image" src="https://github.com/user-attachments/assets/2fa8d6a4-d7f4-4f82-adef-2038f32de264" />

### History Section - Frontend
Multiple text inputs (hist-event, hist-count, etc.) and button clicks collect user data. The program responds differently based on what the user selects (cause & effect, compare events, analyze source, or timeline).
```javascript
function selectHistoryType(type) {
    selectedHistoryType = type;
    // ... shows appropriate form inputs
}

<img width="770" height="782" alt="Image" src="https://github.com/user-attachments/assets/a4c2f680-92ec-427c-8c58-49d2115b869d" />

function generateHistoryPrompt() {
    const event = document.getElementById('hist-event').value;
    const count = document.getElementById('hist-count').value;
    // ... processes user input
}
```

### Backend - Survey Endpoint
Backend receives and validates user input from the survey, processing it to determine which science questions to serve.
```python
@prompt_api.route('/survey', methods=['POST'])
def submit_science_survey():
    payload = request.get_json() or {}
    topic = (payload.get('topic') or '').strip().lower()
    
    if topic not in ('biology', 'chemistry', 'physics'):
        return jsonify(success=False, message='Invalid topic'), 400
```

---

## 2. ✅ Use of at Least One List (Collection Type)

### Science Section - Backend
**Why this meets the requirement:**
- Multiple lists managing complexity: `banks` dictionary contains lists of question dictionaries for each topic
- `questions = []` accumulates processed question objects
- `opts` (inside `_make_options_for_question`) is a list of 4 shuffled prompt options
- These lists directly manage program complexity by organizing questions by topic and providing multiple-choice options
```python
def generate_science_questions(topic):
    banks = {
        'biology': [
            {
                'question': 'Describe the process of cellular respiration...'
            },
            {
                'question': 'Explain the role of DNA in heredity...'
            },
            {
                'question': 'What are the main differences between prokaryotic...'
            }
        ],
        'chemistry': [...],
        'physics': [...]
    }
    
    selected_bank = banks.get(topic, banks['biology'])[:3]
    
    questions = []
    for q in selected_bank:
        opts, correct_idx = _make_options_for_question(q['question'], topic)
        question_obj = {
            'id': random.randint(100000, 999999),
            'category': topic,
            'question': q['question'],
            'options': opts,  # LIST of 4 prompt options
            'correct_index': correct_idx
        }
        questions.append(question_obj)
    
    return questions
```

<img width="773" height="808" alt="Image" src="https://github.com/user-attachments/assets/efd6052c-9f66-469a-b09d-38b2e5c464a1" />

### History Section - Frontend
Implicit list usage: The switch statement operates on a predefined set of history types (`['cause', 'compare', 'analyze', 'timeline']`), managing different prompt templates based on user selection.
```javascript
function generateHistoryPrompt() {
    // ... collects user input ...
    
    switch (selectedHistoryType) {
        case 'cause':
            prompt = `Explain the main causes of ${event}...`;
            break;
        case 'compare':
            prompt = `Compare and contrast the ${event1} and ${event2}...`;
            break;
        // ... more cases
    }
}
```


## 3. ✅ At Least One Procedure with Defined Name, Return Type, and Parameters

### Science Section - Backend
**Why this meets the requirement:**
- **Procedure name:** `_make_options_for_question`
- **Parameters:** `question_text` (string), `topic` (string)
- **Return type:** Tuple containing `(list, int)` - the shuffled options and correct index
- **Purpose:** Generates multiple-choice prompt options for science questions, contributing to the educational goal of teaching prompt quality
```python
def _make_options_for_question(question_text, topic):
    """
    Build 4 prompt-options for a question
    Returns (options_list, correct_index)
    """
    good = f"Explain step-by-step how to answer this: {question_text}..."
    wrong1 = f"Give the direct answer to: {question_text}..."
    wrong2 = f"State the main fact that answers: {question_text}..."
    wrong3 = f"List the key result for: {question_text}..."
    
    options = [good, wrong1, wrong2, wrong3]
    combined = list(enumerate(options))
    random.shuffle(combined)
    shuffled_options = [text for (i, text) in combined]
    correct_index = shuffled_options.index(good)
    
    return shuffled_options, correct_index  # RETURN TYPE: tuple
```

### History Section - Frontend
**Why this meets the requirement:**
- **Procedure name:** `generateHistoryPrompt`
- **Parameters:** Uses global `selectedHistoryType` and reads from DOM inputs (functional parameters)
- **Return type:** Implicitly returns `undefined`, but the procedure's purpose is fulfilled through DOM manipulation
- **Purpose:** Constructs customized history prompts based on user-selected template type
```javascript
function generateHistoryPrompt() {  // PROCEDURE NAME
    let prompt = '';  // Local variable
    
    switch (selectedHistoryType) {  // PARAMETER (global, but acts as input)
        case 'cause':
            const event = document.getElementById('hist-event').value;
            const count = document.getElementById('hist-count').value;
            if (!event) { alert('Please enter an event!'); return; }  // RETURN
            prompt = `Explain the main causes of ${event}...`;
            break;
        // ... more cases
    }
    
    document.getElementById('history-prompt-text').textContent = prompt;
    document.getElementById('history-output').classList.remove('hidden');
    // Implicit return (undefined) but updates DOM as side effect
}
```

<img width="729" height="790" alt="Image" src="https://github.com/user-attachments/assets/7cfe9707-943e-454e-839b-c7ed1443bbd1" />

---

## 4. ✅ Algorithm Including Sequencing, Selection, and Iteration

### Science Section - Backend
**Algorithm breakdown:**
- **Sequencing:** Steps execute in order (normalize topic → select bank → create list → iterate → build objects → append → return)
- **Selection:** `banks.get(topic, banks['biology'])` chooses appropriate question set using conditional logic
- **Iteration:** `for q in selected_bank:` loops through each question to create processed question objects
```python
def generate_science_questions(topic):
    # SELECTION: Choose question bank based on topic
    topic = topic.lower()
    banks = {
        'biology': [...],
        'chemistry': [...],
        'physics': [...]
    }
    selected_bank = banks.get(topic, banks['biology'])[:3]  # SELECTION
    
    questions = []
    # ITERATION: Process each question in the bank
    for q in selected_bank:  # ITERATION
        opts, correct_idx = _make_options_for_question(q['question'], topic)
        question_obj = {  # SEQUENCING: Build object step-by-step
            'id': random.randint(100000, 999999),
            'category': topic,
            'question': q['question'],
            'options': opts,
            'correct_index': correct_idx
        }
        questions.append(question_obj)  # SEQUENCING
    
    return questions  # SEQUENCING
```

### Science Section - Frontend (Display)
**Algorithm breakdown:**
- **Iteration:** `forEach` loops through all questions
- **Sequencing:** Builds HTML string step-by-step for each question
- **Selection:** Conditional rendering based on question properties
```javascript
questions.forEach((q, index) => {
    const questionHtml = `
            Question ${index + 1} - ${q.category.charAt(0).toUpperCase() + q.category.slice(1)}
            ${q.question}
                View Prompt
   
                    Generated Prompt:
                    ${q.prompt_template.replace('{question}', q.question)}
                        Autofill & Test
                        Copy Prompt
                    AI Response:
                    Expected: ${q.answer}
    `;
    container.innerHTML += questionHtml;
});
```

### History Section - Frontend
**Algorithm breakdown:**
- **Sequencing:** Initialize prompt → evaluate type → construct prompt string → update DOM
- **Selection:** `switch` statement routes to different prompt construction logic; `if` statements validate input
- **Iteration:** While not explicit in this function, the broader system iterates through history types when user changes selection
```javascript
function generateHistoryPrompt() {
    let prompt = '';
    
    // SELECTION: Different logic branches based on type
    switch (selectedHistoryType) {
        case 'cause':
            const event = document.getElementById('hist-event').value;
            const count = document.getElementById('hist-count').value;
            if (!event) {  // SELECTION: Validation
                alert('Please enter an event!'); 
                return; 
            }
            prompt = `Explain the main causes of ${event}...`;  // SEQUENCING
            break;
        
        case 'compare':
            const event1 = document.getElementById('hist-event1').value;
            const event2 = document.getElementById('hist-event2').value;
            if (!event1 || !event2) {  // SELECTION: Validation
                alert('Please enter both events!'); 
                return; 
            }
            prompt = `Compare and contrast the ${event1}...`;
            break;
        
        // Additional cases for 'analyze' and 'timeline'
    }
    
    // SEQUENCING: Update DOM with generated prompt
    document.getElementById('history-prompt-text').textContent = prompt;
    document.getElementById('history-output').classList.remove('hidden');
}
```

<img width="763" height="812" alt="Image" src="https://github.com/user-attachments/assets/ff18da94-e358-4c94-bc9a-d23cc6da36e4" />

---

## 5. ✅ Calls to Student-Developed Procedure

### Science Section - Frontend
`selectScienceTopic()` calls `loadScienceQuestions()`, which then calls `displayScienceQuestions()`. These are all student-developed procedures working together.
```javascript
function selectScienceTopic(topic) {
    selectedScienceTopic = topic;
    // ...
    loadScienceQuestions();  // CALL to student-developed procedure
}

async function loadScienceQuestions() {  // STUDENT-DEVELOPED PROCEDURE
    if (!selectedScienceTopic) return;
    
    const response = await fetch('http://localhost:8001/api/science/questions');
    const data = await response.json();
    
    if (data.success && data.questions) {
        const filteredQuestions = data.questions.filter(
            q => q.category === selectedScienceTopic
        );
        displayScienceQuestions(filteredQuestions);  // CALL to another procedure
    }
}
```

### History Section - Frontend
The history workflow involves multiple procedure calls: `selectHistoryType()` → user interaction → `generateHistoryPrompt()` → `copyToClipboard()`. Each procedure has a defined purpose and they work together to fulfill the program's educational goals.
```javascript
function selectHistoryType(type) {
    selectedHistoryType = type;
    // ... setup logic ...
    document.getElementById('history-template-form').classList.remove('hidden');
}

// Later, user clicks "Generate Prompt" button
// onclick="generateHistoryPrompt()"  // CALL from HTML

function generateHistoryPrompt() {  // STUDENT-DEVELOPED PROCEDURE
    // ... prompt generation logic ...
}

// User can then copy the prompt
// onclick="copyToClipboard('history-prompt-text')"  // CALL

function copyToClipboard(elementId) {  // STUDENT-DEVELOPED PROCEDURE
    const text = document.getElementById(elementId).textContent;
    navigator.clipboard.writeText(text).then(() => {
        alert('Copied to clipboard!');
    });
}
```

---

## 6. ✅ Instructions for Output

### Science Section - Frontend
Based on user input (selected topic) and fetched data, the program generates visual output displaying science questions with interactive elements.
```javascript
function displayScienceQuestions(questions) {
    const container = document.getElementById('science-questions-container');
    container.innerHTML = '';  // TEXTUAL/VISUAL OUTPUT
    
    questions.forEach((q, index) => {
        const questionHtml = `
            
                Question ${index + 1} - ${q.category}
                ${q.question}
                View Prompt
                
            
        `;
        container.innerHTML += questionHtml;  // VISUAL OUTPUT
    });
}
```

<img width="718" height="678" alt="Image" src="https://github.com/user-attachments/assets/0b343139-2bf1-4569-b67e-e267f6cf07d4" />

### History Section - Frontend
The program outputs the customized history prompt as textual/visual content based on user inputs (event names, dates, etc.), fulfilling the requirement for output based on input and program functionality.
```javascript
function generateHistoryPrompt() {
    // ... prompt generation logic ...
    
    // OUTPUT: Display generated prompt to user
    document.getElementById('history-prompt-text').textContent = prompt;
    document.getElementById('history-output').classList.remove('hidden');
}
```

### Backend - API Response Output
Backend outputs structured JSON data containing science questions, which the frontend then renders as visual output for the user.
```python
@science_api.route('/questions', methods=['GET'])
def get_science_questions():
    topic = request.args.get('topic', '').strip().lower()
    questions = generate_science_questions(topic)
    
    return jsonify(success=True, questions=questions), 200  # TEXTUAL OUTPUT (JSON)
```