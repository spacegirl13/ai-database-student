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
The AI Learning Platform teaches computer science fundamentals through three progressive activities that integrate AI prompting skills with hands-on coding practice. Students learn to write better AI prompts while strengthening their programming abilities through increasingly complex challenges.

### Core Functionality
- **Activity 1 (Fundamentals)**: Fill-in-blank exercises teaching basic syntax with dropdown selections
- **Activity 2 (Building)**: Write complete code solutions, execute them, and validate output
- **Activity 3 (Analysis)**: Craft coding prompts, analyze quality, auto-improve them, generate code with Gemini API, and execute

### Inputs and Outputs

**Inputs:**
- Dropdown selections for code blanks (Activity 1)
- User-written code submissions (Activity 2)
- User-crafted coding prompts (Activity 3)
- Code for execution via code runner (Activities 2 & 3)

**Outputs:**
- Real-time validation feedback with color-coded indicators (Activity 1)
- Code execution results comparing expected vs. actual output (Activity 2)
- Prompt quality scores with 4-point checklist (Activity 3)
- Auto-improved prompt suggestions (Activity 3)
- AI-generated code from Gemini API (Activity 3)
- Execution output with error handling (Activities 2 & 3)

---

## Data Abstraction: List Usage

### List Implementation

The `analysis.checklist` list manages prompt quality metrics in Activity 3:

```python
def perform_prompt_analysis(prompt):
    """Analyze coding prompt quality"""
    checklist = []  # List initialization
    score = 0

    # Check for programming language
    languages = ['python', 'javascript', 'java', 'c++', 'c#', 'ruby']
    has_language = any(lang in prompt.lower() for lang in languages)
    checklist.append({
        'item': 'Specifies programming language',
        'passed': has_language
    })
    if has_language:
        score += 25

    # Check for specific action verb
    actions = ['explain', 'debug', 'create', 'write', 'help', 'show', 'fix']
    has_action = any(action in prompt.lower() for action in actions)
    checklist.append({
        'item': 'Uses clear action verb',
        'passed': has_action
    })
    if has_action:
        score += 25

    # Check for sufficient detail
    has_details = len(prompt) > 20
    checklist.append({
        'item': 'Includes sufficient detail',
        'passed': has_details
    })
    if has_details:
        score += 25

    # Check for context
    context_words = ['beginner', 'simple', 'step-by-step', 'example', 'comments']
    has_context = any(word in prompt.lower() for word in context_words)
    checklist.append({
        'item': 'Provides context or level',
        'passed': has_context
    })
    if has_context:
        score += 25

    return {
        'checklist': checklist,
        'score': score,
        'total': 100
    }
```

### How the List Manages Complexity

**Benefits:**
- **Structured Storage**: Each criterion stored as dictionary with item name and pass/fail status
- **Scalable Validation**: Add new quality checks by appending to list
- **Frontend Rendering**: Frontend iterates through checklist to display visual indicators
- **Score Calculation**: Each item contributes to total score calculation
- **Flexibility**: Can reorder, add, or remove criteria without rewriting logic

### Without the List

**Would require:**
```python
# Individual variables for each check (rigid)
language_check = { 'item': '...', 'passed': True }
action_check = { 'item': '...', 'passed': False }
detail_check = { 'item': '...', 'passed': True }
context_check = { 'item': '...', 'passed': True }

# Manual response building
return {
    'check1': language_check,
    'check2': action_check,
    'check3': detail_check,
    'check4': context_check
}

# Frontend cannot iterate - must hardcode each check
// Would need:
if (data.check1.passed) { /* show green */ }
if (data.check2.passed) { /* show green */ }
// ... repeat for each check

// Cannot add new checks without updating frontend code
```

---

## Procedure and Algorithm

### Core Procedure: `runWriteCode()` (Activity 2)

This procedure executes student code and validates output:

```javascript
async function runWriteCode() {
    // SEQUENCE: Validation
    if (!currentWriteCode) {
        alert('Please select a question first!');
        return;
    }

    const code = document.getElementById('write-code-editor').value;
    const language = currentWriteCode.language;

    const outputContainer = document.getElementById('write-code-output-container');
    const outputEl = document.getElementById('write-code-output');
    const expectedEl = document.getElementById('write-code-expected');

    outputContainer.classList.remove('hidden');
    outputEl.textContent = 'Running code...';

    try {
        // SELECTION: Choose endpoint based on language
        const endpoint = language === 'python'
            ? pythonURI + '/run/python'
            : pythonURI + '/run/javascript';

        // SEQUENCE: Make API request
        const response = await fetch(endpoint, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ code: code })
        });

        const data = await response.json();
        const output = data.output || 'No output';
        
        // SEQUENCE: Display results
        outputEl.textContent = output;
        expectedEl.textContent = 'Expected output: ' + currentWriteCode.expected_output;

        // SELECTION: Validate correctness
        if (output.trim() === currentWriteCode.expected_output.trim()) {
            outputEl.classList.add('text-green-400');
            outputEl.textContent = output + '\n\n✅ Correct! Your output matches!';
        } else {
            outputEl.classList.add('text-red-400');
        }

    } catch (error) {
        console.error('Error running code:', error);
        outputEl.textContent = 'Error: Could not run code.\n' + error.message;
        outputEl.classList.add('text-red-400');
    }
}
```

### Algorithm Breakdown

**SEQUENCE Steps:**
1. Validate question is selected
2. Extract code and language from UI
3. Display loading message
4. Determine correct API endpoint
5. Send code execution request
6. Receive execution results
7. Display output to student
8. Compare with expected output
9. Show success or error feedback

**SELECTION (Endpoint Choice):**
```javascript
const endpoint = language === 'python'
    ? pythonURI + '/run/python'
    : pythonURI + '/run/javascript';
```
- **Boolean Expression**: `language === 'python'`
- **True Path**: Use Python execution endpoint
- **False Path**: Use JavaScript execution endpoint

**SELECTION (Output Validation):**
```javascript
if (output.trim() === currentWriteCode.expected_output.trim()) {
    // Mark correct with green styling
} else {
    // Mark incorrect with red styling
}
```
- **Boolean Expression**: `output.trim() === currentWriteCode.expected_output.trim()`
- **True Path**: Add green styling, show success message
- **False Path**: Add red styling, student must revise code

**ITERATION (in `perform_prompt_analysis`):**
```python
# Backend procedure iterates through word lists
languages = ['python', 'javascript', 'java', 'c++']
has_language = any(lang in prompt.lower() for lang in languages)
```
- Loops through language keywords
- Checks if prompt contains programming language
- Returns True on first match found

### Contribution to Functionality

**Activity 2 Procedure:**
- **Code Execution**: Runs student solutions in safe environment
- **Instant Feedback**: Shows output immediately
- **Validation**: Compares actual vs. expected results
- **Error Handling**: Catches runtime errors gracefully

**Activity 3 Procedures:**
- **Quality Analysis**: Scores prompts on 4 criteria
- **Auto-Improvement**: Generates better prompts
- **Code Generation**: Calls Gemini API with improved prompts
- **Execution**: Tests AI-generated code

---

## Testing and Debugging

### Error: Gemini API Code Generation Failure

**Problem Identified:**
Activity 3's code generation endpoint would crash when Gemini API was not configured, preventing students from testing AI-generated code.

**Error Manifestation:**
```python
# Original problematic code
def generate_simulated_response(prompt, prompt_type):
    api_key = app.config.get('GEMINI_API_KEY')
    server = app.config.get('GEMINI_SERVER')
    
    # Would crash here if None
    endpoint = f"{server}?key={api_key}"  
    response = requests.post(endpoint, ...)
```

### Debugging Process

**Step 1: Error Detection**
- Students submitted prompts in Activity 3
- Clicked "Generate Code" button
- Received `TypeError: unsupported operand type(s)` error
- Backend logs showed string concatenation with `None`

**Step 2: Root Cause Analysis**
```python
# Problem: config.get() returns None if key missing
api_key = app.config.get('GEMINI_API_KEY')  # Could be None
server = app.config.get('GEMINI_SERVER')     # Could be None

# Crash occurs here:
endpoint = f"{server}?key={api_key}"  # TypeError if None
```
- Configuration not validated before use
- No fallback or error message
- Application crashed entirely

**Step 3: Implement Solution**
```python
def generate_simulated_response(prompt, prompt_type):
    from __init__ import app

    api_key = app.config.get('GEMINI_API_KEY')
    server = app.config.get('GEMINI_SERVER')

    # VALIDATION: Check configuration before proceeding
    if not api_key or not server:
        return "Error: Gemini API not configured. Please contact your administrator."

    try:
        endpoint = f"{server}?key={api_key}"
        payload = {
            "contents": [{
                "parts": [{"text": prompt}]
            }]
        }

        response = requests.post(
            endpoint,
            headers={'Content-Type': 'application/json'},
            json=payload,
            timeout=30
        )

        if response.status_code == 200:
            result = response.json()
            generated_text = result['candidates'][0]['content']['parts'][0]['text']
            return generated_text
        else:
            error_details = response.text
            current_app.logger.error(f"Gemini API error {response.status_code}")
            return f"Error: Gemini API returned status {response.status_code}"

    except Exception as e:
        current_app.logger.error(f"Error calling Gemini API: {e}")
        return f"Error: Could not generate response. {str(e)}"
```

**Step 4: Validation Enhancement**
- Added configuration validation check
- Implemented try-except for API calls
- Provided user-friendly error messages
- Added logging for debugging
- Set timeout to prevent hanging

### Testing Results

**Before Fix:**
- Application crashed on code generation attempts
- No feedback to students
- Required server restart
- Lost student progress

**After Fix:**
- Graceful error message displayed
- Application remains stable
- Clear guidance for administrators
- Students can continue other activities

**Test Cases:**
```python
# Test 1: Valid configuration
api_key = "valid_key", server = "https://api.com"
# Result: Successfully generates code

# Test 2: Missing API key
api_key = None, server = "https://api.com"
# Result: Returns error message, no crash

# Test 3: Missing server
api_key = "valid_key", server = None
# Result: Returns error message, no crash

# Test 4: API timeout
# Result: Exception caught, error message returned
```

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

**Boolean Expression:** `!currentWriteCode` checks if a question is loaded

**If False:** The procedure continues to extract code, determine the endpoint, execute the code, and validate output. The check prevents executing code when no challenge is selected, which would cause undefined reference errors.

---

### 2. Procedural Abstraction: Parameters

**Parameter in `perform_prompt_analysis(prompt)`:**

```python
def perform_prompt_analysis(prompt):
    checklist = []
    score = 0
    
    languages = ['python', 'javascript', 'java']
    has_language = any(lang in prompt.lower() for lang in languages)
    # Uses prompt parameter throughout
```

**How It Manages Complexity:**
- **Single Function**: One procedure analyzes any prompt regardless of content
- **Reusable**: Works for all Activity 3 prompt submissions
- **Testable**: Can pass different prompts to verify scoring logic
- **Flexible**: Same algorithm evaluates beginner and advanced prompts

**Without the parameter:**
```python
# Would need separate functions for each prompt
def analyze_python_prompt():
    # Hardcoded checks for Python prompts only
    
def analyze_javascript_prompt():
    # Duplicate logic for JavaScript prompts
    
# Cannot handle user-submitted prompts dynamically
```

---

### 3. Procedure Calls & Testing

**Call 1: Python Code Execution**
```javascript
// Student writes Python code in Activity 2
currentWriteCode = { language: 'python', expected_output: '10' }
runWriteCode()
// Executes: endpoint = pythonURI + '/run/python'
// Returns: Validates output against expected_output
```

**Call 2: JavaScript Code Execution**
```javascript
// Student writes JavaScript code in Activity 2
currentWriteCode = { language: 'javascript', expected_output: 'Hello World' }
runWriteCode()
// Executes: endpoint = pythonURI + '/run/javascript'
// Returns: Validates output against different expected_output
```

**Different Execution Paths:**
- Different API endpoints selected based on language
- Different expected outputs compared
- Different syntax highlighting applied
- Same validation algorithm for both languages

---

### 4. Logic Errors: Procedure Modification

**Problematic Modification to `runWriteCode()`:**
```javascript
async function runWriteCode() {
    if (!currentWriteCode) {
        alert('Please select a question first!');
        return;
    }

    const code = document.getElementById('write-code-editor').value;
    const language = currentWriteCode.language;

    // WRONG: Always use Python endpoint regardless of language
    const endpoint = pythonURI + '/run/python';  // Logic error here

    const response = await fetch(endpoint, {
        method: 'POST',
        body: JSON.stringify({ code: code })
    });

    // Rest of code unchanged...
}
```

**Logic Error Impact:**
- JavaScript code sent to Python interpreter
- Python endpoint cannot parse JavaScript syntax
- Execution fails with syntax errors
- Students writing JavaScript get "invalid syntax" errors
- Cannot distinguish between student mistakes and endpoint errors
- Activity 2 JavaScript challenges become unusable

**Output Comparison:**
- **Intended**: JavaScript code → JavaScript endpoint → correct execution
- **With Error**: JavaScript code → Python endpoint → syntax errors
- Students cannot complete JavaScript challenges successfully

---

### 5. List Utilization

**Adding Data to Checklist:**
```python
checklist.append({
    'item': 'Specifies programming language',
    'passed': has_language
})
```
- Adds quality criteria without manual indexing
- Order preserved for consistent display

**Accessing Elements:**
```javascript
// Frontend displays checklist results
const checklistDiv = document.getElementById('cs-checklist');
checklistDiv.innerHTML = checklist.map(item =>
    `<div class="flex items-center gap-2">
        <span class="text-${item.passed ? 'green' : 'red'}-400">
            ${item.passed ? '✅' : '❌'}
        </span>
        <span>${item.item}</span>
    </div>`
).join('');
```
- `.map()` transforms each item into HTML
- Dynamically generates UI from list

**Traversal with Calculations:**
```python
# Backend calculates total score
for item in checklist:
    if item['passed']:
        score += 25
        
return {
    'checklist': checklist,
    'score': score,
    'total': 100
}
```
- Single loop aggregates score
- Percentage calculated from list length
- Easy to add more criteria

**Complexity Managed:**
- No hardcoded HTML for each criterion
- Adding new quality checks requires only appending to list
- Frontend automatically displays new checks
- Score calculation scales with list size

---

### 6. Algorithm Analysis: Iteration Processing

**Iteration in `perform_prompt_analysis()`:**

```python
languages = ['python', 'javascript', 'java', 'c++', 'c#', 'ruby']
has_language = any(lang in prompt.lower() for lang in languages)
```

**Algorithm Steps:**

1. **Initialize**: Create list of programming language keywords
2. **Convert**: Transform user prompt to lowercase for comparison
3. **Iterate**: Loop begins with first language ('python')
4. **Check**: Evaluate if current language appears in prompt
5. **Short-circuit**: If match found, return True immediately
6. **Continue**: If no match, advance to next language
7. **Repeat**: Check 'javascript', 'java', etc. sequentially
8. **Terminate**: Stop when match found OR all languages checked
9. **Store Result**: Append pass/fail to checklist
10. **Update Score**: Add 25 points if language found

**Per-Element Processing:**
```python
# For each language keyword:
# 1. Extract keyword: lang = 'python'
# 2. Check presence: 'python' in prompt.lower()
# 3. Boolean evaluation: True/False
# 4. Early exit if True (short-circuit)
# 5. Continue to next if False
```

**Similar Iterations for Other Criteria:**
```python
actions = ['explain', 'debug', 'create', 'write']
has_action = any(action in prompt.lower() for action in actions)

context_words = ['beginner', 'simple', 'step-by-step', 'example']
has_context = any(word in prompt.lower() for word in context_words)
```

Each quality check uses the same iteration pattern but with different keyword lists, demonstrating algorithmic reusability.

---

## Conclusion

The AI Learning Platform Computer Science Module demonstrates comprehensive programming concepts across three integrated activities:

**Activity 1** teaches syntax fundamentals through interactive fill-in-blank exercises
**Activity 2** builds coding skills with write-and-execute challenges  
**Activity 3** integrates AI by teaching prompt engineering, quality analysis, and code generation

Key Technical Achievements:
- **Data Abstraction**: Checklist list manages quality metrics efficiently
- **Procedural Abstraction**: `runWriteCode()` executes any language with parameters
- **Algorithm Design**: Selection, sequence, and iteration combine for validation
- **Error Handling**: Defensive programming prevents API configuration crashes
- **Progressive Learning**: Activities scaffold from basics to AI-powered development

Students master both computer science fundamentals and AI prompting through an integrated curriculum that prepares them for modern software development practices.