---
layout: post
title: "Team Crashers - AI Project Storyboard"
permalink: /digital-famine/ai/storyboard
categories: [CSP, Visualization, TeamWork]
tags: [storyboard, flowchart, team-project, ai-modules]
author: Team Crashers
date: 2025-12-16
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Project Storyboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: #1e1e1e;
            color: #d4d4d4;
            font-family: 'Consolas', monospace;
            padding: 20px;
        }
        
        .storyboard {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        .header {
            text-align: center;
            padding: 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
            margin-bottom: 30px;
        }
        
        .header h1 {
            color: white;
            font-size: 32px;
            margin-bottom: 10px;
        }
        
        .header p {
            color: rgba(255,255,255,0.9);
            font-size: 14px;
        }
        
        .module {
            background: #252526;
            border-left: 4px solid;
            border-radius: 8px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.3);
        }
        
        .module.varada { border-color: #4fc3f7; }
        .module.ruchika { border-color: #81c784; }
        .module.anishka { border-color: #ffb74d; }
        .module.michelle { border-color: #f06292; }
        .module.akshara { border-color: #ba68c8; }
        
        .module-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #3e3e42;
        }
        
        .icon {
            font-size: 36px;
            margin-right: 15px;
        }
        
        .module-title h2 {
            font-size: 20px;
            margin-bottom: 5px;
        }
        
        .module-title span {
            font-size: 12px;
            opacity: 0.7;
        }
        
        .flow {
            display: flex;
            align-items: center;
            gap: 15px;
            flex-wrap: wrap;
        }
        
        .box {
            background: #1e1e1e;
            padding: 15px 20px;
            border-radius: 6px;
            border: 1px solid #3e3e42;
            flex: 1;
            min-width: 150px;
        }
        
        .box-title {
            font-size: 11px;
            text-transform: uppercase;
            opacity: 0.6;
            margin-bottom: 8px;
            letter-spacing: 1px;
        }
        
        .box-content {
            font-size: 14px;
            line-height: 1.6;
        }
        
        .arrow {
            font-size: 24px;
            color: #569cd6;
            flex-shrink: 0;
        }
        
        .code-block {
            background: #1e1e1e;
            border: 1px solid #3e3e42;
            border-radius: 6px;
            padding: 15px;
            margin-top: 15px;
            font-family: 'Consolas', monospace;
            font-size: 12px;
            line-height: 1.6;
        }
        
        .keyword { color: #569cd6; }
        .string { color: #ce9178; }
        .function { color: #dcdcaa; }
        .comment { color: #6a9955; }
        
        .requirements {
            display: flex;
            gap: 10px;
            margin-top: 15px;
            flex-wrap: wrap;
        }
        
        .req-badge {
            background: #264f78;
            padding: 6px 12px;
            border-radius: 4px;
            font-size: 11px;
            border: 1px solid #3e3e42;
        }
    </style>
</head>
<body>
    <div class="storyboard">
        <div class="header">
            <h1>🎯 Team Crashers - AI Learning Platform</h1>
        </div>
        
        <!-- VARADA -->
        <div class="module varada">
            <div class="module-header">
                <div class="icon">📋</div>
                <div class="module-title">
                    <h2>Varada: FRQ Response Storage</h2>
                    <span>Submodule 1 - List & Dictionary</span>
                </div>
            </div>
            
            <div class="flow">
                <div class="box">
                    <div class="box-title">📝 Input</div>
                    <div class="box-content">User submits FRQ answer</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🗂️ Process</div>
                    <div class="box-content">Create dictionary {answer, timestamp}</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">💾 Storage</div>
                    <div class="box-content">Insert at index 0 in list</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">📺 Display</div>
                    <div class="box-content">Show 3 most recent</div>
                </div>
            </div>
            
            <div class="code-block">
<span class="comment">// Line 25</span>
responses = []

<span class="comment">// Line 63</span>
<span class="keyword">new_response</span> = {answer: data, time: now}
responses.<span class="function">insert</span>(0, new_response)
            </div>
            
            <div class="requirements">
                <div class="req-badge">✓ List</div>
                <div class="req-badge">✓ Dictionary</div>
                <div class="req-badge">✓ Insert</div>
            </div>
        </div>
        
        <!-- RUCHIKA -->
        <div class="module ruchika">
            <div class="module-header">
                <div class="icon">🔬</div>
                <div class="module-title">
                    <h2>Ruchika: Science Topic Filter</h2>
                    <span>Submodule 2 - Selection & Iteration</span>
                </div>
            </div>
            
            <div class="flow">
                <div class="box">
                    <div class="box-title">🎯 Input</div>
                    <div class="box-content">User clicks topic button</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🔄 Backend</div>
                    <div class="box-content">Filter questions array</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">📊 Process</div>
                    <div class="box-content">forEach loop iteration</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">✨ Display</div>
                    <div class="box-content">Show filtered questions</div>
                </div>
            </div>
            
            <div class="code-block">
questions.<span class="function">forEach</span>(<span class="keyword">question</span> => {
  <span class="keyword">if</span> (question.topic === selectedTopic) {
    <span class="function">displayQuestion</span>(question)
  }
})
            </div>
            
            <div class="requirements">
                <div class="req-badge">✓ Selection (if)</div>
                <div class="req-badge">✓ Iteration (forEach)</div>
                <div class="req-badge">✓ Array filtering</div>
            </div>
        </div>
        
        <!-- ANISHKA -->
        <div class="module anishka">
            <div class="module-header">
                <div class="icon">💻</div>
                <div class="module-title">
                    <h2>Anishka: AI Prompt Analyzer</h2>
                    <span>CS Module - Sequencing & Selection</span>
                </div>
            </div>
            
            <div class="flow">
                <div class="box">
                    <div class="box-title">✍️ Input</div>
                    <div class="box-content">User enters vague prompt</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🔧 Step 1-4</div>
                    <div class="box-content">Sequential improvements</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🤖 Gemini API</div>
                    <div class="box-content">Generate code</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">📝 Output</div>
                    <div class="box-content">Clean executable code</div>
                </div>
            </div>
            
            <div class="code-block">
<span class="comment">// SEQUENCING</span>
<span class="keyword">if</span> (!has_language) improved = <span class="string">"In Python, "</span> + prompt
<span class="keyword">if</span> (len < 30) improved += <span class="string">" with examples"</span>
<span class="keyword">if</span> (!has_context) improved += <span class="string">" for beginners"</span>
improved = <span class="function">capitalize</span>(improved)

<span class="comment">// Extract code</span>
code = response.<span class="function">match</span>(<span class="string">/```python(.*?)```/</span>)
            </div>
            
            <div class="requirements">
                <div class="req-badge">✓ Sequencing (4 steps)</div>
                <div class="req-badge">✓ Selection (if checks)</div>
                <div class="req-badge">✓ API integration</div>
            </div>
        </div>
        
        <!-- MICHELLE -->
        <div class="module michelle">
            <div class="module-header">
                <div class="icon">😄</div>
                <div class="module-title">
                    <h2>Michelle: Jokes & Concepts</h2>
                    <span>Check for Understanding - Algorithm</span>
                </div>
            </div>
            
            <div class="flow">
                <div class="box">
                    <div class="box-title">🌐 Fetch</div>
                    <div class="box-content">GET concepts from API</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">✔️ Selection</div>
                    <div class="box-content">if (status !== 200) error</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🔄 Iteration</div>
                    <div class="box-content">for (row of data)</div>
                </div>
                <div class="arrow">→</div>
                <div class="box">
                    <div class="box-title">🎨 Display</div>
                    <div class="box-content">Render table with votes</div>
                </div>
            </div>
            
            <div class="code-block">
<span class="keyword">function</span> <span class="function">loadConceptsDataForModal</span>() {
  <span class="function">fetch</span>(url)
    .<span class="function">then</span>(response => {
      <span class="keyword">if</span> (response.status !== 200) <span class="keyword">return</span> <span class="function">error</span>()
      
      <span class="keyword">for</span> (<span class="keyword">const</span> row <span class="keyword">of</span> data) {
        <span class="function">createTableRow</span>(row.joke, row.haha, row.boohoo)
      }
    })
}
            </div>
            
            <div class="requirements">
                <div class="req-badge">✓ Procedure</div>
                <div class="req-badge">✓ Sequencing</div>
                <div class="req-badge">✓ Selection</div>
                <div class="req-badge">✓ Iteration</div>
            </div>
        </div>
        
        <!-- AKSHARA -->
        <div class="module akshara">
            <div class="module-header">
                <div class="icon">📨</div>
                <div class="module-title">
                    <h2>Akshara: Feedback Survey Connection</h2>
                    <span>Frontend ↔ Backend Integration</span>
                </div>
            </div>
            
            <div class="flow">
                <div class="box">
                    <div class="box-title">🎨 Frontend</div>
                    <div class="box-content">POST /feedback with JSON</div>
                </div>
                <div class="arrow">⇄</div>
                <div class="box">
                    <div class="box-title">🔌 Route</div>
                    <div class="box-content">Backend receives at /feedback</div>
                </div>
                <div class="arrow">⇄</div>
                <div class="box">
                    <div class="box-title">✅ Validate</div>
                    <div class="box-content">Check required fields</div>
                </div>
                <div class="arrow">⇄</div>
                <div class="box">
                    <div class="box-title">📤 Response</div>
                    <div class="box-content">Return JSON success/error</div>
                </div>
            </div>
            
            <div class="code-block">
<span class="comment">// Frontend</span>
<span class="function">fetch</span>(<span class="string">'/feedback'</span>, {
  method: <span class="string">'POST'</span>,
  body: <span class="function">JSON.stringify</span>(feedbackData)
})

<span class="comment">// Backend</span>
<span class="keyword">@app.route</span>(<span class="string">'/feedback'</span>, methods=[<span class="string">'POST'</span>])
<span class="keyword">def</span> <span class="function">handle_feedback</span>():
  <span class="keyword">return</span> <span class="function">jsonify</span>({<span class="string">'success'</span>: <span class="keyword">True</span>})
            </div>
            
            <div class="requirements">
                <div class="req-badge">✓ API Integration</div>
                <div class="req-badge">✓ JSON Communication</div>
                <div class="req-badge">✓ Validation</div>
            </div>
        </div>
    </div>
</body>
</html>