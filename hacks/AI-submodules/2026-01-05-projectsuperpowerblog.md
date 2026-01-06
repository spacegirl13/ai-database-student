---
layout: post
title: "Our Team Superpower - AI"
permalink: /digital-famine/ai/superpowerblog
categories: [CSP, Submodule, AIUsage]
tags: [ai, prompts, prompt-engineering, zero-shot, few-shot]
author: Team Crashers
microblog: true
---

## Document 1: AI Student Insights Poll
Example 1: Gemini API Integration for Survey Auto-fill
```javascript
Autofill Question 7 with AI-generated sample response
function autofillQuestion7() {
    const sampleResponse = "I believe AI policies need to strike a balance between preventing academic dishonesty and allowing students to learn how to use AI as a tool. Rather than banning AI completely, I think we should learn how to use it ethically and responsibly. I would want to use AI as a learning aid—for example, to help me understand difficult concepts, brainstorm ideas, or check my work for errors—but I would still write my own original responses and do my own critical thinking. AI should enhance my learning, not replace it. I think it's important to cite when I've used AI and be transparent about how I've incorporated it into my work.";
    document.getElementById('frq-textarea').value = sampleResponse;
}
```
Example 2: Real-time Survey Data Visualization
```javascript
 Display survey data with live updates
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
## Document 2: Prompt Engineering Module
Example 3: Gemini API for Math Problem Solving
```javascript
 Autofill math prompt and call Gemini API for step-by-step solutions
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
Example 4: AI-Powered Prompt Analyzer for Computer Science
```javascript Analyze student prompts and provide feedback using AI
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
## Document 3: Minigame
Example 6: Community Prompt Feed with AI Analysis
```javascript
// Load and display community-submitted prompts with AI evaluation
async function loadPrompts(type) {
    const feedId = type === 'bad' ? 'bad-prompts-feed' : 'good-prompts-feed';
    const feedEl = document.getElementById(feedId);
    const color = type === 'bad' ? 'red' : 'green';

    try {
        const response = await fetch(`${API_BASE_URL}/${type}`);
        const data = await response.json();

        if (data.success && data.prompts.length > 0) {
            feedEl.innerHTML = data.prompts.map((p, index) => `
                <div class="prompt-card bg-slate-700/50 rounded-lg p-4 border border-slate-600/50">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-${color}-300 font-medium text-sm">@${p.user_id || 'anonymous'}</span>
                        <button onclick="copyPromptToTextarea('${type}-prompt', '${p.prompt.replace(/'/g, "\\'")}')"
                            class="px-3 py-1 bg-${color}-600 hover:bg-${color}-700 rounded text-xs text-white transition-colors">
                            Try this prompt
                        </button>
                    </div>
                    <p class="text-gray-300 text-sm">${p.prompt}</p>
                </div>
            `).join('');
        }
    } catch (error) {
        console.error('Error loading prompts:', error);
    }
}
```
## Document 4: Prompt Challenge Game
Example 7: AI-Powered Code Generation from Improved Prompts
``` javascript
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
## Document 4: Prompt Challenge Game
Example 7: AI-Powered Code Generation from Improved Prompts
```javascript 
//Generate executable code from improved prompts using Gemini API
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