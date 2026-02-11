---
layout: post
title: "Anishka Sanghvi - Individual Review Blog"
permalink: /digital-famine/ai/anishkareview
categories: [CSP, Submodule, AIUsage]
tags: [database, transactional-data, survey, backend]
author: Team Crashers
microblog: true
date: 2026-02-03
---

# AI Learning Platform: Computer Science Module
## Technical Documentation

## Program Purpose and Function

### Overview
The AI Learning Platform teaches computer science fundamentals through three progressive activities that integrate AI prompting skills with hands-on coding practice. Students learn to write better AI prompts while strengthening their programming abilities.

### Core Functionality
- **Activity 1 (Fundamentals)**: Fill-in-blank exercises with dropdown selections
- **Activity 2 (Building)**: Write complete code solutions and execute them
- **Activity 3 (Analysis)**: Analyze prompts, generate code with Gemini API, and run it

### Inputs and Outputs

**Inputs:**
- Dropdown selections for code blanks (Activity 1)
- User-written code submissions (Activity 2)
- User-crafted coding prompts (Activity 3)
- Code for execution via code runner

**Outputs:**
- Real-time feedback on blank selections (correct/incorrect indicators)
- Code execution results with expected vs. actual output comparison
- Prompt quality scores with checklist analysis
- Auto-improved prompt suggestions
- AI-generated code from prompts
- Code execution output with error handling

---

## Data Abstraction: List Usage

### List Implementation

The `questions` list manages coding challenges from the database:

```python
questions = []
for q in db_questions:
    opts, correct_idx = _make_options_for_question(q.question, topic)
    question_obj = {
        'id': q.id,
        'title': q.title,
        'code_template': q.code_template,
        'blanks': q.blanks,  # Dictionary of blank options
        'solution': q.solution
    }
    questions.append(question_obj)
```

**Frontend usage:**
```javascript
let fillBlankQuestions = [];  // Stores all fill-blank challenges

async function loadFillBlankQuestions() {
    const data = await fetch('/api/coding/fill-in-blank');
    fillBlankQuestions = data.questions;  // Populate list
    
    // Populate dropdown selector
    data.questions.forEach((question) => {
        const option = document.createElement('option');
        option.value = question.id;
        option.textContent = `${question.language}: ${question.title}`;
        selector.appendChild(option);
    });
}
```

### How the List Manages Complexity

**Benefits:**
- **Dynamic Loading**: Questions loaded from database without hardcoding
- **Easy Selection**: Users can choose from all available questions
- **Scalable**: Adding new questions requires only database updates
- **Traversable**: Iterate through questions to build UI dynamically
- **Searchable**: Find specific questions by ID using `.find()`

### Without the List

**Would require:**
```javascript
// Manual variable creation (unscalable)
let question1 = { id: 1, title: "For Loops", ... };
let question2 = { id: 2, title: "If Statements", ... };
// ... dozens more variables

// Complex selection logic
if (selectedId == 1) {
    currentQuestion = question1;
} else if (selectedId == 2) {
    currentQuestion = question2;
} // ... endless conditionals

// Cannot dynamically build dropdown options
// Cannot add new questions without code changes
// Cannot filter or sort questions programmatically
```

---

## Procedure and Algorithm

### Core Procedure: `checkFillBlank()`

This procedure validates student answers in Activity 1:

```javascript
function checkFillBlank() {
    if (!currentFillBlank) {
        alert('Please select a question first!');
        return;
    }

    let allCorrect = true;
    let incorrectCount = 0;

    // ITERATION: Loop through all blanks in the question
    Object.keys(currentFillBlank.blanks).forEach(blankKey => {
        const selectEl = document.getElementById(blankKey);
        const selectedValue = selectEl.value;
        const correctValue = currentFillBlank.blanks[blankKey].correct;

        // SELECTION: Check each blank's answer
        if (!selectedValue) {
            incorrectCount++;
            selectEl.style.borderColor = '#f59e0b';  // Orange for empty
            allCorrect = false;
        } else if (selectedValue !== correctValue) {
            incorrectCount++;
            selectEl.style.borderColor = '#ef4444';  // Red for wrong
            allCorrect = false;
        } else {
            selectEl.style.borderColor = '#10b981';  // Green for correct
        }
    });

    // SEQUENCE: Display appropriate feedback
    const feedbackEl = document.getElementById('fill-blank-feedback');
    
    if (allCorrect) {
        feedbackEl.className = 'bg-green-900/30 border border-green-500/50 rounded-lg p-4 text-green-300';
        feedbackEl.textContent = '✅ Perfect! All answers are correct!';
    } else {
        feedbackEl.className = 'bg-red-900/30 border border-red-500/50 rounded-lg p-4 text-red-300';
        feedbackEl.textContent = `❌ ${incorrectCount} incorrect. Study the correct answers.';
    }
    feedbackEl.classList.remove('hidden');
}
```

### Algorithm Breakdown

**SEQUENCE:**
1. Validate current question exists
2. Initialize tracking variables
3. Iterate through all blanks (see iteration below)
4. Determine overall result
5. Display feedback message
6. Show feedback element

**SELECTION:**
```javascript
if (!selectedValue) {
    // Empty selection - mark as orange
} else if (selectedValue !== correctValue) {
    // Wrong answer - mark as red
} else {
    // Correct answer - mark as green
}
```
- **Boolean Expression**: `selectedValue !== correctValue`
- **True Path**: Mark incorrect, increment counter
- **False Path**: Mark correct with green border

**ITERATION:**
```javascript
Object.keys(currentFillBlank.blanks).forEach(blankKey => {
    // Process each blank
})
```
- Loops through all dropdown fields in the coding question
- Retrieves student's selection and correct answer for each
- Applies visual feedback (border colors)
- Counts incorrect answers

### Contribution to Functionality

This procedure enables:
- **Immediate Feedback**: Students see results instantly
- **Visual Indicators**: Color-coded borders show status
- **Progress Tracking**: Count of errors helps gauge understanding
- **Learning Reinforcement**: Students identify specific mistakes

---

## Testing and Debugging

### Error: Code Execution Endpoint Failure

**Problem Identified:**
Activity 2's code runner crashed when students submitted code without proper error handling in the backend endpoint.

**Error Manifestation:**
```javascript
// Frontend request succeeds but backend returns 500
POST /run/python
Status: 500 Internal Server Error
Response: "KeyError: 'code'"
```

### Debugging Process

**Step 1: Reproduce the Error**
- Submitted Python code through Activity 2 interface
- Observed server crash in console logs
- Identified missing error handling in endpoint

**Step 2: Analyze Root Cause**
```python
# Original problematic code
@app.route('/run/python', methods=['POST'])
def run_python():
    data = request.json
    code = data['code']  # KeyError if 'code' missing
    # Execute code...
```
- Direct dictionary access fails if key missing
- No validation of request payload
- No error messages returned to user

**Step 3: Implement Solution**
```python
# Fixed version with defensive programming
@app.route('/run/python', methods=['POST'])
def run_python():
    try:
        data = request.json or {}
        code = data.get('code', '').strip()
        
        if not code:
            return jsonify({'error': 'Code is required'}), 400
        
        # Safe code execution with timeout
        result = execute_code_safely(code, language='python')
        return jsonify({'output': result}), 200
        
    except Exception as e:
        current_app.logger.error(f"Code execution error: {e}")
        return jsonify({'error': str(e)}), 500
```

**Step 4: Enhanced Validation**
- Added `.get()` with default values
- Implemented input validation before execution
- Added try-except for unexpected errors
- Returned meaningful error messages to frontend
- Logged errors for debugging

### Testing Results

**Before Fix:**
- Server crashed on every code submission
- Students saw generic "Server Error" message
- Required manual server restart
- No logging of what went wrong

**After Fix:**
- Graceful error handling for all edge cases
- Clear error messages guide students
- Server remains stable
- Detailed error logs for debugging

**Test Cases:**
```javascript
// Test 1: Valid code
{ code: "print('Hello')" } 
// Result: Success, output displayed

// Test 2: Empty code
{ code: "" }
// Result: 400 error with clear message

// Test 3: Missing code field
{ }
// Result: 400 error instead of crash

// Test 4: Code with runtime error
{ code: "1/0" }
// Result: Error captured and displayed
```

---

## PPR Questions

### 1. Procedure & Selection

**First Conditional in `checkFillBlank()`:**

```javascript
if (!selectedValue) {
    incorrectCount++;
    selectEl.style.borderColor = '#f59e0b';
    allCorrect = false;
}
```

**Boolean Expression:** `!selectedValue` checks if dropdown is empty

**If False:** The code skips to the next conditional (`else if`), which checks whether the selected value matches the correct answer. The empty check ensures we distinguish between "no answer" and "wrong answer" states.

---

### 2. Procedural Abstraction: Parameters

**Parameters in `loadFillBlankQuestion()`:**

```javascript
function loadFillBlankQuestion() {
    const selector = document.getElementById('fill-blank-selector');
    const questionId = parseInt(selector.value);
    
    currentFillBlank = fillBlankQuestions.find(q => q.id === questionId);
    // Uses questionId parameter implicitly
}
```

**Implicit Parameters:**
- `questionId`: Extracted from user selection
- `fillBlankQuestions`: Global list reference

**How They Manage Complexity:**
- **Single Function**: One function loads any question by ID
- **No Hardcoding**: Works with any number of questions
- **Reusable**: Same logic for all question types
- **Maintainable**: Adding questions requires no code changes

**Without parameters:**
```javascript
// Would need separate functions per question
function loadQuestion1() { /* hardcoded for q1 */ }
function loadQuestion2() { /* hardcoded for q2 */ }
// ... one function per question
```

---

### 3. Procedure Calls & Testing

**Call 1: First Question Selection**
```javascript
// User selects "PYTHON: For Loops" from dropdown
loadFillBlankQuestion()
// Executes: currentFillBlank = fillBlankQuestions.find(q => q.id === 1)
// Result: Displays for loop code template with blanks
```

**Call 2: Different Question**
```javascript
// User switches to "JAVASCRIPT: If Statements"
loadFillBlankQuestion()
// Executes: currentFillBlank = fillBlankQuestions.find(q => q.id === 5)
// Result: Displays if statement code template with blanks
```

**Different Execution Paths:**
- Different questions have different numbers of blanks
- Different code templates rendered
- Different correct answers validated
- Same algorithm handles all variations

---

### 4. Logic Errors: Procedure Modification

**Problematic Modification:**
```javascript
function checkFillBlank() {
    let allCorrect = true;
    
    Object.keys(currentFillBlank.blanks).forEach(blankKey => {
        const selectEl = document.getElementById(blankKey);
        const selectedValue = selectEl.value;
        const correctValue = currentFillBlank.blanks[blankKey].correct;

        // WRONG: Always mark as correct regardless of answer
        selectEl.style.borderColor = '#10b981';  // Always green
        // Missing: if (selectedValue !== correctValue) check
    });

    // Always shows success message
    feedbackEl.textContent = '✅ Perfect! All answers are correct!';
}
```

**Logic Error Impact:**
- All selections marked correct even when wrong
- Students receive false positive feedback
- No learning from mistakes
- Defeats educational purpose
- Breaks validation system

**Output Comparison:**
- **Intended**: Wrong answers show red borders, error count displayed
- **With Error**: All answers show green, always displays success message
- Students cannot identify mistakes or improve

---

### 5. List Utilization

**Adding Data:**
```javascript
// Backend: Building questions list
questions.append(question_obj)
```
- Adds questions without manual indexing
- Automatically maintains insertion order

**Accessing Elements:**
```javascript
// Frontend: Find specific question
currentFillBlank = fillBlankQuestions.find(q => q.id === questionId);
```
- Direct access by ID using `.find()`
- No need to track array indices

**Traversal for UI Building:**
```javascript
data.questions.forEach((question) => {
    const option = document.createElement('option');
    option.value = question.id;
    option.textContent = `${question.language}: ${question.title}`;
    selector.appendChild(option);
});
```
- Single loop builds entire dropdown menu
- Dynamically creates options from list
- Scales automatically with list size

**Complexity Managed:**
- No hardcoded question variables
- Database-driven content
- Easy to add/remove questions
- Supports filtering and sorting

---

### 6. Algorithm Analysis: Iteration Processing

**Iteration in `checkFillBlank()`:**

```javascript
Object.keys(currentFillBlank.blanks).forEach(blankKey => {
    const selectEl = document.getElementById(blankKey);
    const selectedValue = selectEl.value;
    const correctValue = currentFillBlank.blanks[blankKey].correct;
    // Validation logic...
})
```

**Algorithm Steps:**

1. **Extract Keys**: `Object.keys()` creates array of blank identifiers
2. **Initialize Loop**: Start with first blank
3. **Access DOM**: Get dropdown element by blank ID
4. **Retrieve Values**: Read student selection and correct answer
5. **Compare**: Evaluate if selection matches correct value
6. **Apply Feedback**: Set border color based on correctness
7. **Update Counter**: Increment `incorrectCount` if wrong
8. **Advance**: Move to next blank
9. **Repeat**: Process all blanks in question
10. **Terminate**: Complete when all blanks checked

**Per-Element Processing:**
```javascript
// For each blank:
// 1. Get HTML element: document.getElementById(blankKey)
// 2. Extract user choice: selectEl.value
// 3. Get answer key: currentFillBlank.blanks[blankKey].correct
// 4. Conditional check: if/else validation
// 5. Visual feedback: border color change
// 6. Count tracking: incorrectCount update
```

Each blank undergoes identical validation logic but with different values, demonstrating the power of iteration for repeated operations.

---

## Conclusion

The AI Learning Platform Computer Science Module demonstrates:

- **Data Abstraction**: Lists manage questions from database efficiently
- **Procedural Abstraction**: Functions validate answers across multiple questions
- **Algorithm Design**: Iteration, selection, and sequence combine for validation
- **Error Handling**: Defensive programming prevents code runner crashes
- **Progressive Learning**: Three activities build from fundamentals to AI-powered coding

Students learn both computer science concepts and effective AI prompting through an integrated, hands-on approach that scales with their skill development.