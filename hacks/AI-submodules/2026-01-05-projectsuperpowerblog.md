---
layout: post
title: "Our Team Superpower - AI"
permalink: /digital-famine/ai/superpowerblog
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Team Crashers
microblog: true
---

## The Superpower: AI-Enhanced Interactive Learning

Our team's superpower is using AI to make learning actually interactive instead of just reading text. We connected the Gemini API to our website so students can type something in, get instant feedback, and see real code get generated right in front of them. Instead of just learning about coding or prompt engineering from a textbook, you actually DO it and see what works and what doesn't. The AI responds to what you type, analyzes your work, and helps you get better in real-time.

The coolest part is that you're not just learning the subject - you're also learning how to work with AI tools, which is a skill you'll actually use in the real world.

---

## Example 1: Gemini API Integration for Survey Auto-fill

### What this code does:
This code shows how we use AI to help students who get stuck on survey questions. The auto-fill button generates a sample answer so students can see what a good response looks like.
```javascript
// Autofill Question 7 with AI-generated sample response
function autofillQuestion7() {
    const sampleResponse = "I believe AI policies need to strike a balance between preventing academic dishonesty and allowing students to learn how to use AI as a tool. Rather than banning AI completely, I think we should learn how to use it ethically and responsibly. I would want to use AI as a learning aid—for example, to help me understand difficult concepts, brainstorm ideas, or check my work for errors—but I would still write my own original responses and do my own critical thinking. AI should enhance my learning, not replace it. I think it's important to cite when I've used AI and be transparent about how I've incorporated it into my work.";
    document.getElementById('frq-textarea').value = sampleResponse;
}
```

### How it works:
When you click the auto-fill button, **this function puts a pre-written sample response into the text box**. This helps students who aren't sure how to start or **what a good answer looks like**. It's basically **showing you an example** before you write your own.

---

## Example 2: Real-time Survey Data Visualization

### What this code does:
We visualize all the survey data in real-time with charts so you can see what other students are thinking.
```javascript
// Display survey data with live updates
async function loadSurveyData() {
    try {
        const response = await fetch(`${API_URL}/survey`);
        const data = await response.json();
        displayRecentFRQs(data.frqs.slice(0, 3), 'recentFRQs');
    } catch (error) {
        console.error('Error loading survey data:', error);
    }
}

// Create interactive charts using Chart.js
function createChart(canvasId, label, data, color = '#60a5fa') {
    const ctx = document.getElementById(canvasId);
    const gradient = ctx.getContext('2d').createLinearGradient(0, 0, 0, 300);
    
    charts[canvasId] = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: Object.keys(data),
            datasets: [{
                label: 'Votes',
                data: Object.values(data),
                backgroundColor: gradient,
                borderColor: color,
                borderWidth: 1
            }]
        }
    });
}
```

### How it works:
The first function **fetches survey data from our backend and displays the three most recent responses**. The second function **creates bar charts that show voting results**. This **makes survey data visual and easy to understand** instead of just showing numbers.

---

## Example 3: Gemini API for Math Problem Solving

### What this code does:
This sends math problems to the Gemini API and gets back step-by-step solutions with proper math formatting.
```javascript
// Autofill math prompt and call Gemini API for step-by-step solutions
async function autofillPrompt(questionId) {
    const promptText = document.getElementById(`prompt-text-${questionId}`).textContent;
    const outputArea = document.getElementById(`gemini-output-${questionId}`);
    const resultArea = document.getElementById(`gemini-result-${questionId}`);

    outputArea.classList.remove('hidden');
    resultArea.textContent = 'Loading AI response...';

    try {
        const response = await fetch('http://localhost:8001/api/gemini', {
            method: 'POST',
            credentials: 'include',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                text: promptText,
                prompt: ''
            })
        });

        const data = await response.json();
        if (data.success && data.text) {
            resultArea.textContent = data.text;
            
            // Trigger MathJax to render LaTeX equations
            if (window.MathJax && window.MathJax.typesetPromise) {
                window.MathJax.typesetPromise([resultArea]).catch((err) => {
                    console.error('MathJax rendering error:', err);
                });
            }
        }
    } catch (error) {
        console.error('Gemini API error:', error);
        resultArea.textContent = 'Error: Failed to get AI response. Please make sure you are logged in.';
    }
}
```

### How it works:
This function **grabs the math problem from the page, sends it to the Gemini API, and displays the AI's step-by-step solution**. The MathJax part **makes sure equations look properly formatted** with actual math symbols instead of plain text. So you **see nice-looking fractions and exponents** instead of messy text.

---

## Example 4: AI-Powered Prompt Analyzer for Computer Science

### What this code does:
This analyzes the prompts you write and tells you what's good and what needs improvement. Basically, it grades your prompt and shows you exactly what to fix.
```javascript
// Analyze student prompts and provide feedback using AI
async function analyzeCSPrompt() {
    const prompt = document.getElementById('cs-prompt-input').value.trim();
    
    if (!prompt) {
        alert('Please enter a prompt first!');
        return;
    }

    try {
        const response = await fetch(`${API_BASE_URL}/analyze`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ prompt: prompt })
        });

        const data = await response.json();
        const { checklist, score } = data.analysis;

        // Display AI-generated analysis
        const checklistDiv = document.getElementById('cs-checklist');
        checklistDiv.innerHTML = checklist.map(item =>
            `<div class="flex items-center gap-2">
                <span class="text-${item.passed ? 'green' : 'red'}-400">${item.passed ? '✅' : '❌'}</span>
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
        alert('Error: Could not connect to server.');
    }
}
```

### How it works:
You type in a prompt and **this function sends it to our backend for analysis**. The backend **uses AI to check if your prompt has the important elements** like being specific, including the programming language, and giving enough context. It **returns a checklist showing what you did right and wrong, plus a score out of 100**. Green checkmarks mean you did that part well, red X's mean you need to improve it.

---

## Example 5: Improved Prompt Interface (Prompt Engineering Submodule)

### What this code does:
This creates the visual interface that shows your improved prompt and gives you buttons to either copy it or generate code from it. It's part of the prompt engineering learning module.
```html
<!-- Improved Version -->
<div id="cs-improved" class="mt-4 hidden">
    <div class="bg-green-900/30 border border-green-500/50 rounded-lg p-4">
        <p class="font-semibold text-green-300 mb-2">✅ Improved Version:</p>
        <p id="cs-improved-text" class="text-white p-3 bg-slate-800/50 rounded"></p>
        <div class="grid md:grid-cols-2 gap-3 mt-3">
            <button onclick="copyToClipboard('cs-improved-text')"
                class="px-4 py-2 bg-green-700 hover:bg-green-600 rounded text-sm">
                📋 Copy Improved Prompt
            </button>
            <button onclick="generateCodeFromPrompt()"
                class="px-4 py-2 bg-purple-700 hover:bg-purple-600 rounded text-sm">
                🤖 Generate Code
            </button>
        </div>
    </div>
</div>
```

### How it works:
This HTML **creates a green box that appears after the AI improves your prompt**. Inside the box, **you see your improved prompt text**. Below that are two buttons - one **copies the improved prompt to your clipboard** so you can use it elsewhere, and the other **button triggers the code generation function**. The section starts hidden and **only appears when there's an improved prompt to show**.

---

## Example 6: Generated Code Display and Execution Interface (Prompt Engineering Submodule)

### What this code does:
This creates the code editor where AI-generated code appears and gives you a button to run it. It's also part of the prompt engineering module and works together with the improved prompt interface.
```html
<!-- Generated Code Section -->
<div id="cs-generated-code" class="mt-4 hidden">
    <div class="bg-purple-900/30 border border-purple-500/50 rounded-lg p-4">
        <p class="font-semibold text-purple-300 mb-2">💻 Generated Code:</p>
        <textarea id="cs-generated-code-editor"
            rows="12"
            class="w-full p-3 rounded bg-slate-900 border border-purple-500/30 text-white font-mono text-sm"
            spellcheck="false"></textarea>
        <button onclick="runGeneratedCode()"
            class="mt-3 px-4 py-2 bg-green-700 hover:bg-green-600 rounded text-sm">
            ▶️ Run Code
        </button>

        <!-- Output Display -->
        <div id="cs-generated-output-container" class="hidden mt-4">
            <label class="block text-sm font-semibold mb-2">Output:</label>
            <pre id="cs-generated-output"
                class="bg-slate-900 border border-green-500/30 rounded-lg p-4 text-green-400 font-mono text-sm whitespace-pre-wrap min-h-[100px]"></pre>
        </div>
    </div>
</div>
```

### How it works:
This **creates a purple box with a code editor where the AI-generated code shows up**. **You can edit the code directly in the text area**. The "Run Code" button **executes whatever code is in the editor**. When you run it, **a third section appears below showing the output in green text**. This **completes the learning loop** - you improve your prompt, generate code from it, edit the code if needed, and see it run.

---

## Example 7: AI-Powered Code Generation from Improved Prompts

### What this code does:
This is the most important function because it ties everything together. It takes your improved prompt and uses it to generate actual working code. The code appears in an editor where you can change it and run it.
```javascript
// Generate executable code from improved prompts using Gemini API
async function generateCodeFromPrompt() {
    const improvedPrompt = document.getElementById('cs-improved-text').textContent.trim();

    if (!improvedPrompt) {
        alert('Please improve the prompt first!');
        return;
    }

    try {
        const codeEditor = document.getElementById('cs-generated-code-editor');
        const codeSection = document.getElementById('cs-generated-code');
        codeSection.classList.remove('hidden');
        codeEditor.value = 'Generating code...';

        // Create a specific prompt for code generation
        const codeGenPrompt = improvedPrompt + '\n\nIMPORTANT: Provide ONLY the complete, executable code that will produce output when run. Do not include explanations, pseudocode, or comments about what to fill in. Write actual working code with example values that demonstrates the solution.';

        // Call Gemini API for code generation
        const response = await fetch('http://localhost:8001/api/gemini', {
            method: 'POST',
            credentials: 'include',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                text: codeGenPrompt,
                prompt: ''
            })
        });

        const data = await response.json();

        if (data.success && data.text) {
            // Extract code from markdown code blocks if present
            let code = data.text;
            const codeBlockMatch = code.match(/```(?:python|javascript)?\n([\s\S]*?)```/);
            if (codeBlockMatch) {
                code = codeBlockMatch[1];
            }
            codeEditor.value = code.trim();
        }
    } catch (error) {
        console.error('Gemini API error:', error);
        document.getElementById('cs-generated-code-editor').value = '// Error: Failed to generate code.';
    }
}
```

### How it works:
1. **Grabs your improved prompt from the green box**
2. **Adds extra instructions telling the AI to give you actual working code**, not just explanations
3. **Sends it to the Gemini API**
4. When the response comes back, **it extracts just the code part** (removing markdown formatting)
5. **Puts the clean code in the editor where you can see it, edit it, and run it**

This **shows you the whole point of learning prompt engineering** - **better prompts get you better results**. You **literally see your improved prompt turn into working code**.

---

## Conclusion

Our superpower is connecting AI to the actual learning process instead of just talking about AI. Students type something in, get feedback, see code get generated, and learn by doing. Every feature uses the Gemini API to give real-time responses that help you learn both the subject and how to work with AI tools. This isn't just for class - knowing how to write good prompts and use AI is a real skill you'll need for jobs and projects in the future.