---
title: Trimester 2 Project Blog
permalink: /skillB-individual-blog/
---

### PROMPT 1: Program Purpose and Function

**Describe the purpose of the program:**
- Teaches students how to write effective prompts for AI systems
- Helps users learn the difference between vague and detailed prompts
  - Vague example: "help with history"
  - Specific example: "Explain the causes of World War I for my AP History class, including at least 3 political factors with specific dates"
- Achieves this through two main components:

Mad Libs Prompt Builder - Guides users to fill in blanks to create good prompts
Prompt Quality Analyzer - Scores user-written prompts and gives real-time feedback

**Explain how the program functions:**
- **Step-by-step process:**
  - User selects a difficulty level (Beginner, Intermediate, or Advanced)
  - User selects a subject (Math, Science, CS, or History)
  - Based on their selection:
    - **Beginner:** User fills in template blanks (Mad Libs style)
    - **Intermediate:** User rewrites a weak prompt and gets scored
    - **Advanced:** User writes prompts from scratch and compares bad vs. good

- **Program analyzes prompts for:**
  - Specificity: Does it mention specific topics?
  - Context: Does it explain who you are and why you need this?
  - Detail requirements: Does it say what to include?
  - Length: Is it detailed enough?

- **Feedback and results:**
  - Calculates a score (0-100%) and displays visual feedback
  

**Identify the inputs and outputs:**

**INPUTS:**
- Dropdown selection: Level (beginner/intermediate/advanced)
- Dropdown selection: Subject (math/science/cs/history)
- Text input: User fills in template fields (e.g., "World War I", "3 causes", "AP History class")
- Textarea input: User writes their own prompt from scratch
- Button clicks: "Analyze Prompt", "Generate", "Test with AI"

**OUTPUTS:**
- Quality score: 0-100% percentage displayed
- Visual feedback: Color-coded progress bar (red/yellow/green)
- Checklist: ✅ or ❌ for each quality criterion
- Text feedback: Specific suggestions like "Add more context" or "Great job!"
- AI response: The actual AI-generated answer to their prompt
- Generated prompt: Complete prompt created from their template inputs

### Prompt 2a: Data Abstraction (List Usage)
This is a list used in the intermediate section telling on what to improve to make your response better. 

```javascript
hints: [
    "Which war? What specific aspect?",
    "What's your purpose and academic level?",
    "What kind of information do you need?"
]
```

**How the data in this list manages complexity:**
- Stores multiple related pieces of data (the 3 hint messages) in a single structure
- Avoids creating separate variables for each hint

### Prompt 2b: Managing Complexity

**Problems without using a list:**

1. **Multiple variables:** Would need three separate variables: `hint1`, `hint2`, and `hint3`
   - Hard to remember three different variable names
   - Instead of just one list name

2. **Complex conditional logic:** Display function would need multiple if-statements
   - If user on hint 0: show hint1
   - If user on hint 1: show hint2
   - If user on hint 2: show hint3
 

3. **Difficult to add features:** Adding more hints (4th, 5th, etc.) requires:
   - Creating new variables (hint4, hint5, etc.)
   - Adding more if-statements for each new hint
   - Updating logic in multiple places

### PROMPT 2c: Procedure and Algorithm
The Procedure:

```javascript
// ==================== PROCEDURE START: analyzeStudentPrompt ====================
function analyzeStudentPrompt() {

    // --- STEP 1: Get the student's input ---
    // Grab the text from the input box and remove leading/trailing spaces
    const studentPrompt = document.getElementById('student-improved-prompt').value.trim();
    
    // If the box is empty, reset the analysis and stop the function early
    if (!studentPrompt) {
        resetAnalysis();
        return;
    }
    
    // --- STEP 2: Set up data we need for checking ---
    // Get the current challenge object (contains the keywords to check against)
    const challenge = interChallenges[currentInterChallenge];
    // Make a lowercase version of the prompt so keyword checks aren't case-sensitive
    const lowerPrompt = studentPrompt.toLowerCase();
    
    // --- STEP 3: Initialise the score and checklist ---
    // Score starts at 0, each passed check adds 25 points (max 100)
    let score = 0;
    // All four criteria start as false (not yet passed)
    let checks = {
        specificity: false,
        context: false,
        detail: false,
        length: false
    };
    
    // --- STEP 4: Run the four keyword/length checks ---

    // Check 1 - Specificity: does the prompt include any "specific" keywords?
    if (challenge.keywords.specific.some(kw => lowerPrompt.includes(kw))) {
        checks.specificity = true; // mark as passed
        score += 25;              // award 25 points
    }
    
    // Check 2 - Context: does the prompt include any "context" keywords?
    if (challenge.keywords.context.some(kw => lowerPrompt.includes(kw))) {
        checks.context = true;
        score += 25;
    }
    
    // Check 3 - Detail: does the prompt include any "detail" keywords?
    if (challenge.keywords.detail.some(kw => lowerPrompt.includes(kw))) {
        checks.detail = true;
        score += 25;
    }
    
    // Check 4 - Length: is the prompt longer than 50 characters?
    if (studentPrompt.length > 50) {
        checks.length = true;
        score += 25;
    }
    
    // --- STEP 5: Update the visual checkmarks on the page ---
    // Each call shows a tick or cross next to the matching criterion
    updateCheck('check-specificity', checks.specificity);
    updateCheck('check-context', checks.context);
    updateCheck('check-detail', checks.detail);
    updateCheck('check-length', checks.length);
    
    // --- STEP 6: Display the score and update the progress bar ---
    // Show the score as a percentage (e.g. "75%")
    document.getElementById('quality-score').textContent = score + '%';
    // Set the width of the progress bar to match the score
    document.getElementById('quality-bar').style.width = score + '%';
    
    // --- STEP 7: Colour the progress bar based on how well they scored ---
    if (score >= 75) {
        document.getElementById('quality-bar').className = 'bg-green-500'; // good score = green
    } else if (score >= 50) {
        document.getElementById('quality-bar').className = 'bg-yellow-500'; // okay score = yellow
    } else {
        document.getElementById('quality-bar').className = 'bg-red-500'; // low score = red
    }
    
    // --- STEP 8: Build the feedback messages ---
    let feedback = [];
    // For each failed check, add a helpful error message
    if (!checks.specificity) feedback.push("❌ Add more specific details");
    if (!checks.context) feedback.push("❌ Provide context about yourself");
    if (!checks.detail) feedback.push("❌ Specify what details you want");
    if (!checks.length) feedback.push("❌ Your prompt is too short");
    
    // If they got a perfect score, replace all error messages with a congratulations
    if (score === 100) {
        feedback = ["✅ Excellent! Your prompt looks great!"];
    }
    
    // --- STEP 9: Render the feedback onto the page ---
    // Wrap each message in a <p> tag and display them all in the feedback box
    document.getElementById('feedback-display').innerHTML = feedback.map(f => `<p>${f}</p>`).join('');
}
// ==================== PROCEDURE END: analyzeStudentPrompt ====================
```

**How it contributes to overall functionality:**
- Scores the Intermediate level 
- Runs automatically as students type in the Intermediate Challenge section
- Analyzes prompts in real-time by checking four criteria:
  - Mentions specific topics
  - Provides personal context
  - Requests detailed information
  - Is long enough (sufficient character count)
- Displays visual feedback:
  - Color-coded progress bar (red for bad, yellow for okay, green for good)
  - Checkmarks for each passed criterion
  - 0-100% score display
- **Educational impact:** Instant feedback teaches students what makes a good prompt and motivates them to improve their writing


**The Algorithm - Sequence, Selection, and Iteration:**

**Phase 1: Input & Setup (Sequence)**
- Grabs whatever student typed from textarea
- Checks if it's empty:
  - If empty: resets everything and stops
  - If has text: continues to next phase
- Converts prompt to lowercase for case-insensitive matching
- Initializes score at zero and sets up four checks (all marked false initially):
  - Specificity
  - Context
  - Detail
  - Length

**Phase 2: Keyword Checking (Selection & Iteration)**
- **Specificity check:** Searches for keywords like "world war", "wwi", "cause"
  - If found: marks as passed, adds 25 points
- **Context check:** Searches for keywords like "essay", "class", "AP"
  - If found: marks as passed, adds 25 points
- **Detail check:** Searches for keywords like "explain", "example", "date"
  - If found: marks as passed, adds 25 points
- **Length check:** Counts characters
  - If > 50 characters: marks as passed, adds 25 points

**Phase 3: Visual Display Update (Selection)**
- Updates checkmarks: gray circles → green checkmarks for passed criteria
- Displays score as percentage
- Progress bar adjusts width to match score
- **Color selection based on score:**
  - 75% or higher: green
  - 50-74%: yellow
  - Below 50%: red

**Phase 4: Feedback Generation (Selection & Iteration)**
- Builds feedback messages array:
  - For each failed check: adds specific error message
  - If context missing: adds "❌ Provide context about yourself"
  - If too short: adds "❌ Your prompt is too short"
  - If perfect (100%): replaces all with "✅ Excellent! Your prompt looks great!"
- **Iteration with .map():** Converts each feedback message into HTML `<p>` tags for display

### PROMPT 2d: Testing and Debugging

**Testing Process:**
- Tested by typing different prompts to check scoring
- Test cases:
  - Empty prompt: worked fine - got 0%
  - Perfect prompt with all elements: worked fine - got 100%
  - Edge cases: tested boundary conditions

**The Bug:**
- Typed a prompt exactly 50 characters: "Explain causes of WWI for my history class essay"
- Expected: should pass the length check
- Actual: failed the length check
- Root cause found in the code:

```javascript
if (studentPrompt.length > 50)
```

**Problem Analysis:**
- Used `>` (greater than) instead of `>=` (greater than or equal to)
- Prompts with exactly 50 characters were rejected
- 50 characters is a reasonable length and should pass

**The Fix:**
Changed to:

```javascript
if (studentPrompt.length >= 50)
```

**Result:**
- Prompts with 50 or more characters now pass the length check
- Re-tested with 50-character prompt: correctly awarded 25 points for length ✅

### 
Procedure & Selection
First Conditional Statement: 

The first if-statement in my analyzeStudentPrompt() procedure is:

```javascript
if (!studentPrompt)
```

Boolean Expression: `!studentPrompt`

This expression checks whether the studentPrompt variable is empty, null, undefined, or contains only whitespace after trimming. The exclamation mark ! means "NOT", so it's asking "Is the prompt NOT filled in?" or in other words "Is the prompt empty?"
**What happens if it evaluates to false:**
- Expression is false = prompt is NOT empty (user typed something)
- Function skips reset code and continues to analyze
- Steps that follow:
  - Grabs challenge data
  - Converts prompt to lowercase
  - Initializes score at zero
  - Runs through all four quality checks (specificity, context, detail, length)
- Program does its main job of scoring instead of resetting

**Why this check matters:**
- Without it: program tries to analyze the empty typing box 
-

### Procedural Abstraction
Parameter in My Procedure

In the Intermediate Challenge section, I use this procedure:

```javascript
function selectInterChallenge(subject)
```

The Parameter: `subject` - tells the function which subject challenge to load (like "history", "math", "science", or "cs")


**How it manages complexity:**
- **With parameter:** One function handles all four subjects
  - User picks history → calls `selectInterChallenge('history')`
  - Function uses parameter to grab history data with `interChallenges[subject]`
  - User picks math → calls `selectInterChallenge('math')`
  - Gets math data automatically

- **Without parameter:** Would need four separate functions:
  - `selectHistoryChallenge()`
  - `selectMathChallenge()`
  - `selectScienceChallenge()`
  - `selectCSChallenge()`
 

- **Benefits:**
  - Parameter reduces 4 functions into 1
  - Much shorter and easier to maintain
  

### Procedure Calls & Testing
The function we're using:

```javascript
function selectInterChallenge(subject)
```

What this function does: When a student picks a subject in the Intermediate Challenge, this function loads all the data for that subject (the weak prompt they need to fix, the hints, and the keywords to check for).

New Call 1:

```javascript
selectInterChallenge('cs')
```

What happens:
- Shows the weak prompt "code help"

- When the student types their improved prompt, the program checks if it contains CS words like "python" or "loop"

New Call 2:

```javascript
selectInterChallenge('science')
```

What happens:
- Shows the weak prompt "explain photosynthesis"
- Loads science keywords like "photosynthesis", "chloroplast", "biology"
- When the student types, the program checks if it contains science words like "photosynthesis" or "chemical"

### Logic Errors

Modification That Would Cause a Logic Error
In the History Section, there's a function `selectHistoryType(type)` that determines which prompt template to load.

Current (Correct) Code:

```javascript
function selectHistoryType(type) {
    selectedHistoryType = type;
    
    if (type === 'cause') {
        // load cause & effect template
    } else if (type === 'compare') {
        // load compare events template
    } else if (type === 'analyze') {
        // load analyze source template
    } else if (type === 'timeline') {
        // load timeline template
    }
}
```

Modified (Buggy) Code:

```javascript
function selectHistoryType(type) {
    selectedHistoryType = 'cause';  // BUG: Always sets to 'cause' instead of using parameter
    
    if (type === 'cause') {
        // load cause & effect template
    } else if (type === 'compare') {
        // load compare events template
    }
}
```
**How this changes the output:**
- User clicks "Compare Events"
- Function receives `type = 'compare'`
- Code immediately overwrites it: `selectedHistoryType = 'cause'`
- When generating prompt: uses cause & effect template instead of comparison template
- **Result:** User thinks they're making a comparison prompt but gets wrong format
- **Problem:** User gets confused about what they're supposed to fill in


### List Utilization
How My Code Uses a List
In the History Section, I have this list:

```javascript
hints: [
    "Which war? What specific aspect?",
    "What's your purpose and academic level?",
    "What kind of information do you need?"
]
```

**Adding data:**
- Three hint messages stored in array during initialization

**Accessing elements:**
- When user clicks "Get Hint": code uses `hints[hintsUsed]` to grab specific hint
- If `hintsUsed = 0`: gets `hints[0]` = "Which war? What specific aspect?"
- If `hintsUsed = 1`: gets `hints[1]` = "What's your purpose and academic level?"

**Complexity management:**
- No need for separate variables (`hint1`, `hint2`, `hint3`) and multiple if-statements
- List automatically keeps track of how many hints exist
- Access any hint using just the index number

### Algorithm Analysis
Algorithm Within Iteration Statement
In the Beginner Level, when a student selects "History" for Mad Libs, the program needs to create input boxes on the screen. The buildMadLibForm() function uses a loop to do this:

```javascript
Object.keys(currentMadLibTemplate.fields).forEach(fieldKey => {
    const field = currentMadLibTemplate.fields[fieldKey];
    // creates HTML for this field
    inputsContainer.innerHTML += fieldHTML;
});
```
**What the loop does:**
- History Mad Libs template has 5 fields: EVENT, ASPECT, DETAILS, FORMAT, REQUIREMENT
- Loop goes through each one and creates input boxes for the student

**Step-by-step process:**
- Loop starts with Field 1 (EVENT): creates label "Historical Event/Topic", an input box, and example buttons like "American Revolution" and "World War II"
- Displays Field 1 on the page
- Loop moves to Field 2 (ASPECT): creates label "Focus Aspect", input box, and example buttons
- Displays Field 2 on the page
- Repeats the same process for Fields 3, 4, and 5

**Why use a loop?**
- **Without loop:** Would manually write separate code for each field
  - Create EVENT input box code
  - Create ASPECT input box code
  - Create DETAILS, FORMAT, REQUIREMENT codes separately
  - Very repetitive!
- **With loop:** Automatically handles any number of fields
- **Scalability:** Adding a 6th field later? Loop handles it without changing any code