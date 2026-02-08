---
title: Trimester 2 Project Blog
permalink: /skillB-individual-blog/
---

### PROMPT 1: Program Purpose and Function
Describe the purpose of the program
The purpose of my program is to teach students how to write effective prompts for AI systems. Specifically, it helps users learn the difference between vague prompts (like "help with history") and detailed, specific prompts (like "Explain the causes of World War I for my AP History class, including at least 3 political factors with specific dates").
The program does this through two main components:

Mad Libs Prompt Builder - Guides users to fill in blanks to create good prompts
Prompt Quality Analyzer - Scores user-written prompts and gives real-time feedback

Explain how the program functions
Step-by-step:

User selects a difficulty level (Beginner, Intermediate, or Advanced)
User selects a subject (Math, Science, CS, or History)
Based on their selection:

Beginner: User fills in template blanks (Mad Libs style)
Intermediate: User rewrites a weak prompt and gets scored
Advanced: User writes prompts from scratch and compares bad vs. good


The program analyzes their prompt for:

Specificity (does it mention specific topics?)
Context (does it explain who you are and why you need this?)
Detail requirements (does it say what to include?)
Length (is it detailed enough?)


Program calculates a score (0-100%) and displays visual feedback
User can test their prompt with a real AI API to see the response

Identify the inputs and outputs
INPUTS:

Dropdown selection: Level (beginner/intermediate/advanced)
Dropdown selection: Subject (math/science/cs/history)
Text input: User fills in template fields (e.g., "World War I", "3 causes", "AP History class")
Textarea input: User writes their own prompt from scratch
Button clicks: "Analyze Prompt", "Generate", "Test with AI"

OUTPUTS:

Quality score: 0-100% percentage displayed
Visual feedback: Color-coded progress bar (red/yellow/green)
Checklist: ✅ or ❌ for each quality criterion
Text feedback: Specific suggestions like "Add more context" or "Great job!"
AI response: The actual AI-generated answer to their prompt
Generated prompt: Complete prompt created from their template inputs

### Prompt 2a: Data Abstraction (List Usage)
This is a list used in the intermediate section telling on what to improve to make your response better. 

hints: [
    "Which war? What specific aspect?",
    "What's your purpose and academic level?",
    "What kind of information do you need?"
]

How the data in this list manages complexity:
The hints list manages complexity by storing multiple related pieces of data (the 3 hint messages) in a single structure instead of creating separate variables for each hint. 

### Prompt 2b: Managing Complexity
First, instead of having one hints list, I would need three separate variables: hint1, hint2, and hint3. This means I have to remember three different variable names instead of just one list name.
Second, the function that displays hints would need multiple if-statements to check which hint number the user is on. If they're on hint 0, show hint1. If they're on hint 1, show hint2. If they're on hint 2, show hint3. That's three separate checks I have to write out.
Third, if I wanted to add more hints (like a 4th or 5th hint), I would have to:

Create a new variable (hint4)
Add another if-statement to check for it
Update the logic in multiple places

### PROMPT 2c: Procedure and Algorithm
The Procedure:

function analyzeStudentPrompt() {
    const studentPrompt = document.getElementById('student-improved-prompt').value.trim();
    
    if (!studentPrompt) {
        resetAnalysis();
        return;
    }
    
    const challenge = interChallenges[currentInterChallenge];
    const lowerPrompt = studentPrompt.toLowerCase();
    
    let score = 0;
    let checks = {
        specificity: false,
        context: false,
        detail: false,
        length: false
    };
    
    if (challenge.keywords.specific.some(kw => lowerPrompt.includes(kw))) {
        checks.specificity = true;
        score += 25;
    }
    
    if (challenge.keywords.context.some(kw => lowerPrompt.includes(kw))) {
        checks.context = true;
        score += 25;
    }
    
    if (challenge.keywords.detail.some(kw => lowerPrompt.includes(kw))) {
        checks.detail = true;
        score += 25;
    }
    
    if (studentPrompt.length > 50) {
        checks.length = true;
        score += 25;
    }
    
    updateCheck('check-specificity', checks.specificity);
    updateCheck('check-context', checks.context);
    updateCheck('check-detail', checks.detail);
    updateCheck('check-length', checks.length);
    
    document.getElementById('quality-score').textContent = score + '%';
    document.getElementById('quality-bar').style.width = score + '%';
    
    if (score >= 75) {
        document.getElementById('quality-bar').className = 'bg-green-500';
    } else if (score >= 50) {
        document.getElementById('quality-bar').className = 'bg-yellow-500';
    } else {
        document.getElementById('quality-bar').className = 'bg-red-500';
    }
    
    let feedback = [];
    if (!checks.specificity) feedback.push("❌ Add more specific details");
    if (!checks.context) feedback.push("❌ Provide context about yourself");
    if (!checks.detail) feedback.push("❌ Specify what details you want");
    if (!checks.length) feedback.push("❌ Your prompt is too short");
    
    if (score === 100) {
        feedback = ["✅ Excellent! Your prompt looks great!"];
    }
    
    document.getElementById('feedback-display').innerHTML = feedback.map(f => `<p>${f}</p>`).join('');
}

How it contributes to overall functionality:
This is the main scoring engine of my program. Every time a student types in the Intermediate Challenge section, this procedure runs automatically and analyzes their prompt in real-time. It checks four things: whether the prompt mentions specific topics, provides personal context, requests detailed information, and is long enough. Based on what it finds, it calculates a score from 0-100% and shows visual feedback with checkmarks and a color-coded progress bar (red for bad, yellow for okay, green for good). This instant feedback teaches students what makes a good prompt and motivates them to improve their writing.


The Algorithm - Sequence, Selection, and Iteration:
The algorithm follows a specific order. First, it grabs whatever the student typed from the textarea and checks if it's empty - if so, it resets everything and stops. If there's actual text, it converts the prompt to lowercase so the keyword matching isn't case-sensitive. Then it initializes the score at zero and sets up four checks (specificity, context, detail, length) all marked as false initially.

Next comes the checking phase using selection. The algorithm checks if the prompt contains any keywords from the "specific" list like "world war", "wwi", or "cause" - if it finds even one match, it marks specificity as passed and adds 25 points. It does the same thing for context keywords like "essay", "class", or "AP", and then detail keywords like "explain", "example", or "date". For the length check, it simply counts characters - if the prompt is longer than 50 characters, that's another 25 points.
After all four checks are done, it updates the visual display. The checkmarks change from gray circles to green checkmarks for each passed criteria. The score gets displayed as a percentage, and the progress bar's width adjusts to match. Here's where another selection happens - the algorithm decides what color to make the progress bar based on the score. If it's 75% or higher, the bar turns green. If it's between 50-74%, yellow.

Anything below 50% stays red.
Finally, the algorithm builds feedback messages. For each check that failed, it adds a specific error message to a feedback array. So if the student didn't provide context, it adds "❌ Provide context about yourself". If their prompt was too short, it adds "❌ Your prompt is too short". But if they got a perfect 100%, it replaces all those messages with just "✅ Excellent! Your prompt looks great!". Then it takes all those feedback messages and converts them into HTML paragraph tags using a loop, which gets displayed on the page.
The iteration happens in a few places. When checking for keywords, the .some() method loops through each keyword in the list and checks if it appears in the student's prompt - it stops as soon as it finds one match. At the end, the .map() method loops through each feedback message and wraps it in <p> tags so it displays properly on the page.

### PROMPT 2d: Testing and Debugging
Testing Process
I tested my program by typing different prompts to check the scoring. I tried an empty prompt (worked fine - got 0%), a perfect prompt with all elements (worked fine - got 100%), and then I tested edge cases.
The Bug
I typed a prompt that was exactly 50 characters long: "Explain causes of WWI for my history class essay" and expected it to pass the length check, but it failed. I checked my code and found:
javascriptif (studentPrompt.length > 50)
The problem was I used > (greater than) instead of >= (greater than or equal to). So prompts with exactly 50 characters were being rejected even though 50 characters is a reasonable length.
The Fix
I changed it to:
javascriptif (studentPrompt.length >= 50)
Now prompts with 50 or more characters pass the length check. I tested again with my 50-character prompt and it correctly gave me the 25 points for length

### 
Procedure & Selection
First Conditional Statement: 

The first if-statement in my analyzeStudentPrompt() procedure is:
javascriptif (!studentPrompt)


Boolean Expression: !studentPrompt


This expression checks whether the studentPrompt variable is empty, null, undefined, or contains only whitespace after trimming. The exclamation mark ! means "NOT", so it's asking "Is the prompt NOT filled in?" or in other words "Is the prompt empty?"
What happens if it evaluates to false:
If this expression is false, that means the prompt is NOT empty - the user actually typed something. When this happens, the function skips the reset code and continues to the next line where it starts analyzing the prompt. It grabs the challenge data, converts the prompt to lowercase, initializes the score at zero, and then runs through all four quality checks (specificity, context, detail, and length). Basically, if the expression is false, the program does its main job of scoring the user's prompt instead of just resetting everything.
If I didn't have this check and the user cleared the textarea, the program would try to analyze an empty string and might give weird results or even crash when trying to check for keywords in nothing.

### Procedural Abstraction
Parameter in My Procedure

In the Intermediate Challenge section, I use this procedure:

javascriptfunction selectInterChallenge(subject)
The Parameter: subject - tells the function which subject challenge to load (like "history", "math", "science", or "cs")


How it manages complexity:
This parameter lets one function handle all four subjects instead of writing four separate functions. When a user picks history, the program calls selectInterChallenge('history') and the function uses that parameter to grab the history challenge data with interChallenges[subject]. If they pick math, it calls selectInterChallenge('math') and gets the math data.
Without this parameter, I'd need four functions: selectHistoryChallenge(), selectMathChallenge(), selectScienceChallenge(), and selectCSChallenge() - all doing the exact same thing but with different hard-coded data. The parameter reduces 4 functions into 1, making the code shorter and easier to maintain. Adding a fifth subject would just mean passing in a new subject name, not writing a whole new function.

### Procedure Calls & Testing
The function we're using:
javascriptfunction selectInterChallenge(subject)
What this function does: When a student picks a subject in the Intermediate Challenge, this function loads all the data for that subject (the weak prompt they need to fix, the hints, and the keywords to check for).
New Call 1:
javascriptselectInterChallenge('cs')
What happens:

Shows the weak prompt "code help"
Loads computer science keywords like "python", "function", "loop"
When the student types their improved prompt, the program checks if it contains CS words like "python" or "loop"

New Call 2:
javascriptselectInterChallenge('science')
What happens:

Shows the weak prompt "explain photosynthesis"
Loads science keywords like "photosynthesis", "chloroplast", "biology"
When the student types, the program checks if it contains science words like "photosynthesis" or "chemical"

### Logic Errors

Modification That Would Cause a Logic Error
In the History Section, there's a function selectHistoryType(type) that determines which prompt template to load.
Current (Correct) Code:
javascriptfunction selectHistoryType(type) {
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
Modified (Buggy) Code:
javascriptfunction selectHistoryType(type) {
    selectedHistoryType = 'cause';  // BUG: Always sets to 'cause' instead of using parameter
    
    if (type === 'cause') {
        // load cause & effect template
    } else if (type === 'compare') {
        // load compare events template
    }
}
How this changes the output:
If the user clicks "Compare Events", the function receives type = 'compare', but then immediately overwrites it with selectedHistoryType = 'cause'. Later when they try to generate their prompt, it uses the cause & effect template instead of the comparison template they wanted. The user thinks they're making a comparison prompt but gets a completely different format, confusing them about what they're supposed to fill in.


### List Utilization
How My Code Uses a List
In the History Section, I have this list:
javascripthints: [
    "Which war? What specific aspect?",
    "What's your purpose and academic level?",
    "What kind of information do you need?"
]
Adding data: The three hint messages are stored in the array when I initialize it.
Accessing elements: When the user clicks "Get Hint", the code uses hints[hintsUsed] to grab the specific hint. If hintsUsed = 0, it gets hints[0] which is "Which war? What specific aspect?". If hintsUsed = 1, it gets hints[1] which is "What's your purpose and academic level?".

This manages complexity because I don't need separate variables like hint1, hint2, hint3 and multiple if-statements. The list automatically keeps track of how many hints exist, and I can access any hint using just the index number.

### Algorithm Analysis
Algorithm Within Iteration Statement
In the Beginner Level, when a student selects "History" for Mad Libs, the program needs to create input boxes on the screen. The buildMadLibForm() function uses a loop to do this:
javascriptObject.keys(currentMadLibTemplate.fields).forEach(fieldKey => {
    const field = currentMadLibTemplate.fields[fieldKey];
    // creates HTML for this field
    inputsContainer.innerHTML += fieldHTML;
});
What the loop does:
The history Mad Libs template has 5 fields: EVENT, ASPECT, DETAILS, FORMAT, and REQUIREMENT. The loop goes through each one and creates the input boxes the student sees on the screen.
Step-by-step what happens:

Loop starts at field 1 (EVENT)
Creates a label "Historical Event/Topic"
Creates an input box where student can type
Creates example buttons like "American Revolution", "World War II"
Puts all that on the page
Loop moves to field 2 (ASPECT)
Creates label "Focus Aspect"
Creates input box and example buttons
Puts that on the page
Repeats for fields 3, 4, and 5

Why use a loop? Without the loop, I'd have to manually write the code to create the EVENT input box, then manually write code for the ASPECT input box, then DETAILS, then FORMAT, then REQUIREMENT - all separately. The loop does it automatically for however many fields exist. If I add a 6th field later, the loop handles it without me changing any code.