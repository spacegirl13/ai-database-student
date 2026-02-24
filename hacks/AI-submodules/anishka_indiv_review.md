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
The AI Learning Platform teaches computer science fundamentals through three progressive activities that integrate AI prompting skills with hands-on coding practice. The Computer Science section walks students through a structured learning path — from filling in code blanks, to writing full solutions, to generating and running AI-produced code — building confidence and skill at each step.

### Core Functionality
- **Activity 1 (Fundamentals)**: Fill-in-blank exercises with dropdown selections
- **Activity 2 (Building)**: Write complete code solutions and execute them against expected output
- **Activity 3 (Analysis)**: Analyze prompts, generate code with Gemini API, and run it

### Inputs and Outputs

**Inputs:**
- Dropdown selections for code blanks (Activity 1)
- User-written code submissions (Activity 2)
- User-crafted coding prompts (Activity 3)
- Code for execution via code runner (Activities 2 & 3)

**Outputs:**
- Real-time feedback on blank selections with color-coded indicators (Activity 1)
- Code execution results comparing expected vs. actual output (Activity 2)
- Prompt quality scores with checklist analysis (Activity 3)
- Auto-improved prompt suggestions (Activity 3)
- AI-generated code from Gemini API (Activity 3)

### Activity 2: Input and Output In Depth

**Specific Input:**
A student selects the challenge "PYTHON: Print Even Numbers" and writes the following code in the editor:

```python
for i in range(1, 11):
    if i % 2 == 0:
        print(i)
```

This code is extracted from the textarea element and sent as a JSON payload to the backend:

```javascript
// Frontend sends code to backend
const code = document.getElementById('write-code-editor').value;

const response = await fetch(pythonURI + '/run/python', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ code: code })  // Input: student's code string
});
```

**Corresponding Output:**
The backend executes the code, returns the printed values, and the frontend compares against the expected output:

```javascript
const data = await response.json();
const output = data.output || 'No output';

// Display actual output
outputEl.textContent = output;
// Display expected output below
expectedEl.textContent = 'Expected output: ' + currentWriteCode.expected_output;

// Compare and give feedback
if (output.trim() === currentWriteCode.expected_output.trim()) {
    outputEl.classList.add('text-green-400');
    outputEl.textContent = output + '\n\n✅ Correct! Your output matches!';
} else {
    outputEl.classList.add('text-red-400');
}
```

**Result displayed to student:**
```
2
4
6
8
10

✅ Correct! Your output matches!
Expected output: 2\n4\n6\n8\n10
```

This single input-output cycle teaches students to write correct code, run it, and verify their solution — all within one seamless interface.

---

## Data Abstraction: List Usage

### List Implementation

The `writeCodeQuestions` list stores all available coding challenges loaded from the backend:

```javascript
let writeCodeQuestions = [];  // List initialization
let currentWriteCode = null;

async function loadWriteCodeQuestions() {
    const response = await fetch(pythonURI + '/api/coding/write-code');
    const data = await response.json();

    if (data.success && data.questions) {
        writeCodeQuestions = data.questions;  // Populate list from API

        const selector = document.getElementById('write-code-selector');

        // Traverse list to build dropdown
        data.questions.forEach(question => {
            const option = document.createElement('option');
            option.value = question.id;
            option.textContent = `${question.language.toUpperCase()}: ${question.title}`;
            selector.appendChild(option);
        });
    }
}
```

Each item in the list contains:
```javascript
// Structure of each question object in the list
{
    id: 1,
    title: "Print Even Numbers",
    description: "Write a loop that prints all even numbers from 1 to 10",
    language: "python",
    hint: "Use the modulo operator %",
    solution: "for i in range(1,11):\n    if i%2==0:\n        print(i)",
    expected_output: "2\n4\n6\n8\n10"
}
```

### How the List Manages Complexity

**Benefits:**
- **Dynamic Loading**: Challenges fetched from database, not hardcoded in frontend
- **Single Source of Truth**: One list holds all challenge data used across the activity
- **Traversable**: Loop through to populate dropdown with no repeated code
- **Searchable**: Find a specific challenge instantly using `.find()`
- **Scalable**: New challenges added to database appear automatically

### Without the List

**Would require:**
```javascript
// Hardcoded variables — completely unscalable
let challenge1 = { id: 1, title: "Print Even Numbers", solution: "...", expected: "..." };
let challenge2 = { id: 2, title: "Reverse a String", solution: "...", expected: "..." };
// Dozens more...

// Manually match selection to variable
if (selectedId === 1) {
    currentWriteCode = challenge1;
} else if (selectedId === 2) {
    currentWriteCode = challenge2;
}
// Cannot build dropdown dynamically
// Cannot add challenges without changing code
// Cannot store expected output and solution together cleanly
```

---

## Procedure and Algorithm

### Core Procedure: `runWriteCode()`

This procedure takes student-written code, executes it, and validates the output:

```javascript
async function runWriteCode() {
    // SEQUENCE Step 1: Validate a question is loaded
    if (!currentWriteCode) {
        alert('Please select a question first!');
        return;
    }

    const code = document.getElementById('write-code-editor').value;
    const language = currentWriteCode.language;

    const outputEl = document.getElementById('write-code-output');
    const expectedEl = document.getElementById('write-code-expected');

    document.getElementById('write-code-output-container').classList.remove('hidden');
    outputEl.textContent = 'Running code...';

    try {
        // SELECTION Step 2: Pick endpoint based on language
        const endpoint = language === 'python'
            ? pythonURI + '/run/python'
            : pythonURI + '/run/javascript';

        // SEQUENCE Step 3: Send code to backend for execution
        const response = await fetch(endpoint, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ code: code })
        });

        const data = await response.json();
        const output = data.output || 'No output';

        // SEQUENCE Step 4: Display output and expected
        outputEl.textContent = output;
        expectedEl.textContent = 'Expected output: ' + currentWriteCode.expected_output;

        // SELECTION Step 5: Compare output to expected
        if (output.trim() === currentWriteCode.expected_output.trim()) {
            outputEl.classList.add('text-green-400');
            outputEl.textContent = output + '\n\n✅ Correct! Your output matches!';
        } else {
            outputEl.classList.add('text-red-400');
        }

    } catch (error) {
        outputEl.textContent = 'Error: Could not run code.\n' + error.message;
        outputEl.classList.add('text-red-400');
    }
}
```

### Algorithm Breakdown

**SEQUENCE:**
1. Check that a question is selected
2. Extract student code from the editor
3. Determine the correct execution endpoint
4. POST code to the backend
5. Receive and display execution output
6. Show expected output for comparison
7. Apply styling based on correctness

**SELECTION (Endpoint Choice):**
```javascript
const endpoint = language === 'python'
    ? pythonURI + '/run/python'
    : pythonURI + '/run/javascript';
```
- **Boolean Expression**: `language === 'python'`
- **True Path**: Routes to Python execution endpoint
- **False Path**: Routes to JavaScript execution endpoint

**SELECTION (Output Validation):**
```javascript
if (output.trim() === currentWriteCode.expected_output.trim()) {
    // Show green success message
} else {
    // Show red, student must revise
}
```
- **Boolean Expression**: `output.trim() === currentWriteCode.expected_output.trim()`
- **True Path**: Green styling, success message shown
- **False Path**: Red styling, student revises and resubmits

**ITERATION (in `loadWriteCodeQuestions`):**
```javascript
data.questions.forEach(question => {
    const option = document.createElement('option');
    option.value = question.id;
    option.textContent = `${question.language.toUpperCase()}: ${question.title}`;
    selector.appendChild(option);
});
```
- Loops through every question in `writeCodeQuestions`
- Creates a dropdown option element for each
- Builds the entire selector UI in one pass

### Contribution to Functionality

- **Code Execution**: Safely runs student code on the backend
- **Instant Validation**: Compares actual vs. expected output immediately
- **Multi-language Support**: Handles Python and JavaScript with one function
- **Error Resilience**: Catches runtime errors and displays them cleanly

---

## Testing and Debugging

### Error: Code Execution Endpoint Failure

**Problem Identified:**
Activity 2's code runner returned 500 errors when students submitted code, preventing any execution from working.

**Error Manifestation:**
```
POST /run/python
Status: 500 Internal Server Error
Response: "KeyError: 'code'"
```

### Debugging Process

**Step 1: Reproduce the Error**
- Opened Activity 2, selected a Python challenge
- Wrote valid code and clicked "Run Code"
- Backend crashed and returned 500 with no useful message

**Step 2: Analyze Root Cause**
```python
# Original broken endpoint
@app.route('/run/python', methods=['POST'])
def run_python():
    data = request.json
    code = data['code']  # Crashes with KeyError if 'code' key is missing
```
- Used direct dictionary access (`data['code']`) instead of `.get()`
- No check for empty or missing payload
- No try-except to catch failures

**Step 3: Implement Fix**
```python
# Fixed endpoint with defensive programming
@app.route('/run/python', methods=['POST'])
def run_python():
    try:
        data = request.json or {}
        code = data.get('code', '').strip()  # Safe access with default

        if not code:
            return jsonify({'error': 'Code is required'}), 400

        result = execute_code_safely(code, language='python')
        return jsonify({'output': result}), 200

    except Exception as e:
        current_app.logger.error(f"Code execution error: {e}")
        return jsonify({'error': str(e)}), 500
```

**Step 4: Validated with Test Cases**
```python
# Test 1: Valid code submission
{ "code": "print('Hello')" }
# → Output: "Hello", status 200 ✅

# Test 2: Empty code field
{ "code": "" }
# → Error: "Code is required", status 400 ✅

# Test 3: Missing 'code' key entirely
{}
# → Error: "Code is required", status 400 (no crash) ✅

# Test 4: Runtime error in student code
{ "code": "print(1/0)" }
# → Error captured, returned cleanly, status 200 ✅
```

**Before Fix:** Server crashed, required restart, no student feedback
**After Fix:** Handles all edge cases, returns meaningful errors, remains stable

---

## PPR Questions

### 1. Procedure & Selection

**First Conditional in `runWriteCode()`:**
```javascript
if (!currentWriteCode) {
    alert('Please select a question first!');
    return;
}
```
**Boolean Expression:** `!currentWriteCode` — checks if a challenge is loaded

**If False:** The procedure continues: it reads the student's code, determines the endpoint, sends the execution request, and displays results. Without this guard, accessing `currentWriteCode.language` would throw a `TypeError` on a null object.

---

### 2. Procedural Abstraction: Parameters

**Parameter in `loadWriteCodeQuestion(select)`:**
```javascript
function loadWriteCodeQuestion(select) {
    const selector = document.getElementById('write-code-selector');

    // If select parameter is passed, set the selector value first
    if (select !== undefined) {
        selector.value = select;
    }

    const questionId = parseInt(selector.value);

    if (!questionId) {
        document.getElementById('write-code-info').classList.add('hidden');
        document.getElementById('write-code-editor-container').classList.add('hidden');
        document.getElementById('write-code-actions').classList.add('hidden');
        currentWriteCode = null;
        return null;
    }

    const writeCodeQuest = writeCodeQuestions.find(question => question.id === questionId);
    currentWriteCode = writeCodeQuest;

    if (currentWriteCode) {
        document.getElementById('write-code-title').textContent = currentWriteCode.title;
        document.getElementById('write-code-description').textContent = currentWriteCode.description;
        document.getElementById('write-code-hint').textContent = currentWriteCode.hint;
        document.getElementById('write-code-hint').classList.add('hidden');
        document.getElementById('write-code-info').classList.remove('hidden');
        document.getElementById('write-code-editor').value = '';
        document.getElementById('write-code-editor-container').classList.remove('hidden');
        document.getElementById('write-code-actions').classList.remove('hidden');
        document.getElementById('write-code-output-container').classList.add('hidden');
    }

    return writeCodeQuest;
}
```
**`select` manages complexity by:**
- Allowing the function to be called programmatically (auto-load) or manually (dropdown change) with one unified function
- When `select` is `""`, it clears/resets the current question view
- When `select` is a question ID, it directly loads that challenge without user interaction
- Eliminating the need for separate reset and load functions
- Making the function reusable for any number of database-stored challenges

---

### 3. Procedure Calls & Testing

**Call 1: Reset/clear the question view**
```javascript
// Called after questions load to start in a clean state
loadWriteCodeQuestion("");
// select = ""
// questionId = NaN → falsy
// Hides all UI panels, sets currentWriteCode = null
// Returns null
```

**Call 2: Auto-load the first question**
```javascript
// Called immediately after to pre-load the first challenge
loadWriteCodeQuestion(writeCodeQuestions[0].id);
// select = 1 (first question's ID)
// writeCodeQuest = { language: 'python', expected_output: '2\n4\n6\n8\n10', ... }
// currentWriteCode set, UI panels shown
// Returns the question object
```

**Different paths executed:**
- Call 1 hits the early-return branch (`!questionId`), hiding the UI and nulling state
- Call 2 passes the guard, performs the `.find()` lookup, populates the editor, and returns the question
- Same `loadWriteCodeQuestion()` function handles both reset and load behavior

---

### 4. Logic Errors: Procedure Modification

**Problematic Modification:**
```javascript
// WRONG: Remove the trim() from comparison
if (output === currentWriteCode.expected_output) {
    outputEl.textContent = output + '\n\n✅ Correct!';
} else {
    outputEl.classList.add('text-red-400');
}
```
**Logic Error Impact:**
- Python's `print()` adds a trailing newline (`\n`) to output
- Without `.trim()`, `"2\n4\n6\n8\n10\n"` never equals `"2\n4\n6\n8\n10"`
- Every correct solution is marked wrong
- Students believe their code is broken when it isn't
- They waste time debugging correct code

**Output Comparison:**
- **Intended**: `"2\n4\n6\n8\n10\n".trim()` === `"2\n4\n6\n8\n10"` → ✅ Correct
- **With Error**: `"2\n4\n6\n8\n10\n"` !== `"2\n4\n6\n8\n10"` → ❌ Always wrong

---

### 5. List Utilization

**Adding Data (backend populates, frontend receives):**
```javascript
writeCodeQuestions = data.questions;  // Entire list assigned at once
```
- All challenges stored in one structure
- No manual tracking of how many exist

**Accessing Elements:**
```javascript
const writeCodeQuest = writeCodeQuestions.find(question => question.id === questionId);
```
- Retrieves one specific challenge by ID using the descriptive `question` loop variable
- No index tracking needed

**Traversal for UI Building:**
```javascript
data.questions.forEach(question => {
    const option = document.createElement('option');
    option.value = question.id;
    option.textContent = `${question.language.toUpperCase()}: ${question.title}`;
    selector.appendChild(option);
});
```
- One loop builds the entire challenge selector dropdown
- Scales to any number of questions automatically

**Complexity Managed:**
- No hardcoded challenge variables
- Dropdown built dynamically from list
- Supports multiple languages in one unified list
- Easy to extend with new challenge types

---

### 6. Algorithm Analysis: Iteration Processing

**Iteration in `loadWriteCodeQuestions()`:**
```javascript
data.questions.forEach(question => {
    const option = document.createElement('option');
    option.value = question.id;
    option.textContent = `${question.language.toUpperCase()}: ${question.title}`;
    selector.appendChild(option);
});
```

**Algorithm Steps:**
1. **Initialize**: Start at first question in `data.questions`
2. **Access**: Retrieve current question object as `question`
3. **Create**: Build a new `<option>` HTML element
4. **Assign ID**: Set `option.value` to `question.id` for lookup later
5. **Format Label**: Combine language and title into readable string
6. **Append**: Add option to the dropdown selector
7. **Advance**: Move to next question in list
8. **Repeat**: Until all questions processed
9. **Terminate**: Dropdown fully populated, ready for student use

**Per-Element Processing:**
```javascript
// For each question object:
// 1. question.id        → option.value (used by loadWriteCodeQuestion)
// 2. question.language  → formatted label prefix (e.g., "PYTHON:")
// 3. question.title     → readable challenge name (e.g., "Print Even Numbers")
// 4. Combined           → option.textContent shown to student
```

Each question undergoes the same transformation from raw data to a UI element, demonstrating how iteration removes the need for repetitive manual code.

---

## Conclusion

Activity 2 of the AI Learning Platform CS module demonstrates core programming principles through a practical write-and-run coding experience:

- **Data Abstraction**: `writeCodeQuestions` list manages all challenges dynamically
- **Procedural Abstraction**: `runWriteCode()` handles any language and challenge with one function
- **Algorithm Design**: Sequence, selection, and iteration combine for execution and validation
- **Error Handling**: Defensive programming prevents crashes from bad code submissions
- **Real Feedback Loop**: Students write code, run it, see output, and correct mistakes immediately

The activity bridges the gap between understanding code concepts and writing working solutions, preparing students for the AI-powered code generation in Activity 3.