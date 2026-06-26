# Day-25
Build an AI Shark Tank Simulator
[ai_shark_tank_simulator.html](https://github.com/user-attachments/files/29377696/ai_shark_tank_simulator.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Shark Tank</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  :root{
    --bg:#0a0e1a;--bg2:#0f1629;--bg3:#141d35;--bg4:#1a2540;
    --border:#1e2d4a;--border2:#2a3d5e;
    --text:#e8edf5;--text2:#8fa3c0;--text3:#4a6080;
    --accent:#3b82f6;--accent2:#1d4ed8;
    --gold:#f59e0b;--gold2:#d97706;
    --green:#10b981;--red:#ef4444;--purple:#8b5cf6;--orange:#f97316;
    --radius:12px;--radius-sm:8px;
    --shadow:0 4px 24px rgba(0,0,0,0.4);
  }
  body{font-family:'Inter',system-ui,sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden}
  h1,h2,h3{font-weight:600;line-height:1.2}
  p{line-height:1.6;color:var(--text2)}
  input,textarea,select{background:var(--bg3);border:1px solid var(--border2);border-radius:var(--radius-sm);color:var(--text);padding:10px 14px;font-size:14px;width:100%;outline:none;transition:border-color .2s;font-family:inherit}
  input:focus,textarea:focus{border-color:var(--accent)}
  textarea{resize:vertical;min-height:80px}
  button{cursor:pointer;font-family:inherit;font-size:14px;font-weight:500;border:none;border-radius:var(--radius-sm);transition:all .2s;padding:10px 20px}
  .btn-primary{background:linear-gradient(135deg,var(--accent),var(--accent2));color:#fff;box-shadow:0 4px 14px rgba(59,130,246,.3)}
  .btn-primary:hover{transform:translateY(-1px);box-shadow:0 6px 20px rgba(59,130,246,.4)}
  .btn-primary:active{transform:translateY(0)}
  .btn-gold{background:linear-gradient(135deg,var(--gold),var(--gold2));color:#000}
  .btn-ghost{background:transparent;border:1px solid var(--border2);color:var(--text2)}
  .btn-ghost:hover{border-color:var(--accent);color:var(--accent)}
  .btn-green{background:var(--green);color:#fff}
  .badge{display:inline-flex;align-items:center;gap:6px;padding:4px 10px;border-radius:20px;font-size:12px;font-weight:600}
  .card{background:var(--bg2);border:1px solid var(--border);border-radius:var(--radius);padding:24px}
  .card-sm{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius-sm);padding:16px}

  #app{max-width:900px;margin:0 auto;padding:20px}

  .header{text-align:center;padding:40px 20px 20px;position:relative}
  .header-logo{font-size:42px;margin-bottom:8px}
  .header h1{font-size:32px;background:linear-gradient(135deg,#f59e0b,#ef4444,#8b5cf6);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
  .header p{color:var(--text2);margin-top:8px;font-size:15px}

  .screen{display:none;animation:fadeIn .4s ease}
  .screen.active{display:block}
  @keyframes fadeIn{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}

  .form-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px}
  .form-group{display:flex;flex-direction:column;gap:6px}
  .form-group label{font-size:13px;font-weight:600;color:var(--text2);text-transform:uppercase;letter-spacing:.05em}
  .form-group.full{grid-column:1/-1}
  .form-actions{display:flex;justify-content:center;margin-top:28px}
  .form-actions button{padding:14px 48px;font-size:16px;border-radius:var(--radius)}

  .pitch-header{background:linear-gradient(135deg,var(--bg3),var(--bg4));border:1px solid var(--border2);border-radius:var(--radius);padding:28px;margin-bottom:24px}
  .pitch-header h2{font-size:24px;margin-bottom:4px;color:var(--gold)}
  .pitch-meta{display:flex;gap:12px;margin-top:12px;flex-wrap:wrap}
  .pitch-meta span{background:var(--bg);border:1px solid var(--border2);border-radius:20px;padding:4px 14px;font-size:13px;color:var(--text2)}

  .judges-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:16px;margin-bottom:24px}
  .judge-card{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius);padding:20px;transition:all .3s;position:relative;overflow:hidden}
  .judge-card.active{border-color:var(--accent);box-shadow:0 0 20px rgba(59,130,246,.15)}
  .judge-card.thinking::after{content:'';position:absolute;top:0;left:-100%;width:100%;height:2px;background:linear-gradient(90deg,transparent,var(--accent),transparent);animation:shimmer 1.5s infinite}
  @keyframes shimmer{to{left:100%}}
  .judge-avatar{width:48px;height:48px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:22px;margin-bottom:12px}
  .judge-name{font-size:16px;font-weight:600;margin-bottom:2px}
  .judge-role{font-size:12px;color:var(--text3);margin-bottom:10px;text-transform:uppercase;letter-spacing:.05em}
  .judge-focus{font-size:13px;color:var(--text2);background:var(--bg);border-radius:6px;padding:8px;margin-top:8px}
  .judge-chat{margin-top:12px;display:none}
  .judge-card.active .judge-chat{display:block}
  .judge-bubble{background:var(--bg);border:1px solid var(--border2);border-radius:10px;padding:12px;font-size:13px;color:var(--text2);margin-bottom:8px;line-height:1.5}
  .judge-bubble.question{border-left:3px solid var(--accent)}
  .judge-bubble.reaction{border-left:3px solid var(--green);color:var(--text)}
  .answer-input{display:flex;gap:8px;margin-top:8px}
  .answer-input input{flex:1}
  .answer-input button{padding:10px 16px;background:var(--accent);color:#fff;border-radius:var(--radius-sm);white-space:nowrap;flex-shrink:0}

  .progress-bar{height:4px;background:var(--bg3);border-radius:4px;overflow:hidden;margin-bottom:24px}
  .progress-fill{height:100%;background:linear-gradient(90deg,var(--accent),var(--purple));border-radius:4px;transition:width .5s ease}

  .pitch-status{text-align:center;padding:16px;color:var(--text2);font-size:14px}

  .scores-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin:20px 0}
  @media(max-width:600px){.scores-grid{grid-template-columns:repeat(2,1fr)}.form-grid{grid-template-columns:1fr}.judges-grid{grid-template-columns:1fr}}
  .score-card{background:var(--bg3);border:1px solid var(--border);border-radius:var(--radius-sm);padding:16px;text-align:center}
  .score-label{font-size:11px;text-transform:uppercase;letter-spacing:.08em;color:var(--text3);margin-bottom:8px}
  .score-val{font-size:32px;font-weight:700}
  .score-bar{height:6px;background:var(--bg);border-radius:4px;margin-top:8px;overflow:hidden}
  .score-fill{height:100%;border-radius:4px;transition:width 1.2s cubic-bezier(.34,1.56,.64,1)}

  .decision-card{background:var(--bg2);border:2px solid var(--border);border-radius:var(--radius);padding:32px;text-align:center;margin:24px 0;transition:all .5s}
  .decision-card.invest{border-color:var(--green);box-shadow:0 0 40px rgba(16,185,129,.2)}
  .decision-card.reject{border-color:var(--red);box-shadow:0 0 40px rgba(239,68,68,.15)}
  .decision-card.acquire{border-color:var(--purple);box-shadow:0 0 40px rgba(139,92,246,.2)}
  .decision-card.later{border-color:var(--gold);box-shadow:0 0 40px rgba(245,158,11,.15)}
  .decision-icon{font-size:56px;margin-bottom:12px}
  .decision-verdict{font-size:28px;font-weight:700;margin-bottom:8px}
  .decision-val{font-size:20px;font-weight:600;margin:16px 0 4px}
  .decision-funding{font-size:15px;color:var(--text2)}
  .decision-reasoning{font-size:14px;color:var(--text2);line-height:1.7;background:var(--bg3);border-radius:var(--radius-sm);padding:16px;text-align:left;margin-top:16px}

  .actions-row{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin-top:24px}
  .actions-row button{padding:12px 24px}

  .leaderboard{background:var(--bg2);border:1px solid var(--border);border-radius:var(--radius);padding:20px;margin-top:24px}
  .lb-title{font-size:16px;font-weight:600;margin-bottom:16px;display:flex;align-items:center;gap:8px}
  .lb-row{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border);font-size:13px}
  .lb-row:last-child{border-bottom:none}
  .lb-rank{width:28px;height:28px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:12px;flex-shrink:0}
  .lb-name{flex:1;margin:0 12px;color:var(--text)}
  .lb-score{font-weight:700;color:var(--gold)}
  .lb-verdict{font-size:11px;padding:2px 8px;border-radius:10px}

  .loading-overlay{position:fixed;inset:0;background:rgba(10,14,26,.9);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;display:none}
  .loading-overlay.visible{display:flex}
  .spinner{width:48px;height:48px;border:3px solid var(--border2);border-top-color:var(--accent);border-radius:50%;animation:spin 1s linear infinite}
  @keyframes spin{to{transform:rotate(360deg)}}
  .loading-text{margin-top:20px;color:var(--text2);font-size:15px;animation:pulse 2s ease infinite}
  @keyframes pulse{0%,100%{opacity:.6}50%{opacity:1}}

  canvas#confetti-canvas{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:9998;display:none}

  .typing-dots{display:inline-flex;gap:4px;margin-left:4px;vertical-align:middle}
  .typing-dots span{width:6px;height:6px;background:var(--accent);border-radius:50%;animation:bounce .8s ease infinite}
  .typing-dots span:nth-child(2){animation-delay:.15s}
  .typing-dots span:nth-child(3){animation-delay:.3s}
  @keyframes bounce{0%,80%,100%{transform:translateY(0)}40%{transform:translateY(-6px)}}

  .judge-status{font-size:12px;color:var(--text3);display:flex;align-items:center;gap:6px;margin-top:4px}
  .status-dot{width:6px;height:6px;border-radius:50%;background:var(--text3)}
  .status-dot.active{background:var(--green);animation:statusPulse 1.5s ease infinite}
  @keyframes statusPulse{0%,100%{opacity:1}50%{opacity:.4}}

  .total-score-ring{width:120px;height:120px;margin:0 auto 20px;position:relative}
  .total-score-ring svg{transform:rotate(-90deg)}
  .score-number{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;flex-direction:column}
  .score-number span{font-size:28px;font-weight:700}
  .score-number small{font-size:11px;color:var(--text3);text-transform:uppercase;letter-spacing:.05em}

  .tag{display:inline-block;font-size:11px;font-weight:600;padding:3px 8px;border-radius:4px;text-transform:uppercase;letter-spacing:.05em}
  .tag-invest{background:rgba(16,185,129,.15);color:var(--green)}
  .tag-reject{background:rgba(239,68,68,.15);color:var(--red)}
  .tag-acquire{background:rgba(139,92,246,.15);color:var(--purple)}
  .tag-later{background:rgba(245,158,11,.15);color:var(--gold)}

  .section-title{font-size:13px;text-transform:uppercase;letter-spacing:.08em;color:var(--text3);margin-bottom:12px;display:flex;align-items:center;gap:8px}
  .section-title::after{content:'';flex:1;height:1px;background:var(--border)}

  .share-toast{position:fixed;bottom:24px;right:24px;background:var(--bg2);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:12px 20px;font-size:14px;color:var(--text);z-index:9999;transform:translateY(60px);transition:transform .3s;display:flex;align-items:center;gap:8px}
  .share-toast.visible{transform:translateY(0)}
</style>
</head>
<body>
<canvas id="confetti-canvas"></canvas>
<div id="loading-overlay" class="loading-overlay"><div class="spinner"></div><p class="loading-text" id="loading-text">Getting the sharks ready...</p></div>
<div id="share-toast" class="share-toast"><i class="ti ti-check"></i> Copied to clipboard!</div>
<div id="app">
  <div class="header">
    <div class="header-logo">🦈</div>
    <h1>AI Shark Tank</h1>
    <p>Pitch your startup to 4 AI investors and see if you land the deal</p>
  </div>

  <!-- SCREEN 1: INPUT -->
  <div id="screen-input" class="screen active">
    <div class="card">
      <div class="section-title"><i class="ti ti-bulb"></i> Your Startup Pitch</div>
      <div class="form-grid">
        <div class="form-group">
          <label>Startup Name</label>
          <input type="text" id="startup-name" placeholder="e.g. GrowthOS" />
        </div>
        <div class="form-group">
          <label>Funding Ask</label>
          <input type="text" id="funding-ask" placeholder="e.g. $500,000 for 10%" />
        </div>
        <div class="form-group full">
          <label>Problem Statement</label>
          <textarea id="problem" placeholder="What painful problem are you solving?"></textarea>
        </div>
        <div class="form-group full">
          <label>Your Solution</label>
          <textarea id="solution" placeholder="How does your product solve this?"></textarea>
        </div>
        <div class="form-group">
          <label>Revenue Model</label>
          <input type="text" id="revenue-model" placeholder="e.g. SaaS subscription, $99/mo" />
        </div>
        <div class="form-group">
          <label>Target Audience</label>
          <input type="text" id="target-audience" placeholder="e.g. SME founders in the US" />
        </div>
      </div>
      <div class="form-actions">
        <button class="btn-primary" onclick="startPitch()"><i class="ti ti-player-play" style="margin-right:6px"></i>Enter the Tank</button>
      </div>
    </div>
  </div>

  <!-- SCREEN 2: PITCH -->
  <div id="screen-pitch" class="screen">
    <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:0%"></div></div>
    <div class="pitch-header">
      <h2 id="ph-name"></h2>
      <p id="ph-problem" style="margin-top:8px;font-size:14px"></p>
      <div class="pitch-meta">
        <span id="pm-audience"></span>
        <span id="pm-model"></span>
        <span id="pm-ask"></span>
      </div>
    </div>
    <div class="section-title"><i class="ti ti-users"></i> The Sharks</div>
    <div class="judges-grid" id="judges-grid"></div>
    <div class="pitch-status" id="pitch-status">The sharks are reviewing your pitch...</div>
  </div>

  <!-- SCREEN 3: RESULTS -->
  <div id="screen-results" class="screen">
    <div class="section-title"><i class="ti ti-chart-bar"></i> Pitch Scores</div>
    <div style="text-align:center;margin-bottom:24px">
      <div class="total-score-ring">
        <svg viewBox="0 0 120 120" width="120" height="120">
          <circle cx="60" cy="60" r="50" fill="none" stroke="#1a2540" stroke-width="10"/>
          <circle id="score-ring-fill" cx="60" cy="60" r="50" fill="none" stroke="#3b82f6" stroke-width="10" stroke-dasharray="314" stroke-dashoffset="314" stroke-linecap="round" style="transition:stroke-dashoffset 1.5s ease"/>
        </svg>
        <div class="score-number"><span id="total-score-val">0</span><small>/ 100</small></div>
      </div>
    </div>
    <div class="scores-grid" id="scores-grid"></div>
    <div class="section-title" style="margin-top:28px"><i class="ti ti-gavel"></i> Investment Decision</div>
    <div class="decision-card" id="decision-card">
      <div class="decision-icon" id="decision-icon"></div>
      <div class="decision-verdict" id="decision-verdict"></div>
      <div class="decision-val" id="decision-val"></div>
      <div class="decision-funding" id="decision-funding"></div>
      <div class="decision-reasoning" id="decision-reasoning"></div>
    </div>
    <div class="actions-row">
      <button class="btn-ghost" onclick="downloadReport()"><i class="ti ti-download" style="margin-right:6px"></i>Download Report</button>
      <button class="btn-ghost" onclick="shareResult()"><i class="ti ti-share" style="margin-right:6px"></i>Share Result</button>
      <button class="btn-primary" onclick="resetApp()"><i class="ti ti-refresh" style="margin-right:6px"></i>Pitch Again</button>
    </div>
    <div class="leaderboard" id="leaderboard">
      <div class="lb-title"><i class="ti ti-trophy"></i> Leaderboard</div>
      <div id="lb-rows"></div>
    </div>
  </div>
</div>

<script>
const JUDGES = [
  {id:'vc',name:'Alex Mercer',role:'Venture Capitalist',emoji:'💼',color:'#3b82f6',focus:'Market size, scalability, and VC-return potential. I want 10x or I walk.',questions:['What is your total addressable market size and how did you arrive at that figure?','What is your customer acquisition strategy and what are your unit economics?']},
  {id:'founder',name:'Sarah Chen',role:'Serial Founder',emoji:'🚀',color:'#8b5cf6',focus:'Execution, team strength, and speed to market. Ideas are worthless without execution.',questions:['Who is on your founding team and what relevant experience do they bring?','What\'s the biggest execution risk in your model and how are you mitigating it?']},
  {id:'customer',name:'Marcus Webb',role:'Customer Advocate',emoji:'🎯',color:'#10b981',focus:'Real customer pain, usability, and whether people will actually use this.',questions:['Have you spoken with real customers? What did they say about this problem?','What does the customer journey look like from discovery to paying customer?']},
  {id:'angel',name:'Priya Nair',role:'Angel Investor',emoji:'💰',color:'#f59e0b',focus:'Profitability path, burn rate, and capital efficiency. I invest my own money.',questions:['What is your path to profitability and projected timeline to break-even?','How long will this funding last and what milestones will you hit before the next raise?']}
];
const SCORE_DIMS = ['Market Potential','Innovation','Business Model','Execution','Investment Worthiness'];
let pitchData = {};
let answers = {};
let scores = {};
let decision = {};
let leaderboardData = JSON.parse(localStorage.getItem('shark_lb') || '[]');
let currentJudgeIdx = 0;
let currentQuestionIdx = 0;
let judgeStates = {};
let totalScoreVal = 0;

function setLoading(visible, text='') {
  document.getElementById('loading-overlay').classList.toggle('visible', visible);
  if(text) document.getElementById('loading-text').textContent = text;
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-' + id).classList.add('active');
}

function startPitch() {
  const name = document.getElementById('startup-name').value.trim();
  const problem = document.getElementById('problem').value.trim();
  const solution = document.getElementById('solution').value.trim();
  const revenue = document.getElementById('revenue-model').value.trim();
  const audience = document.getElementById('target-audience').value.trim();
  const ask = document.getElementById('funding-ask').value.trim();
  if(!name || !problem || !solution || !revenue || !audience || !ask) {
    alert('Please fill in all fields before entering the tank!');
    return;
  }
  pitchData = {name, problem, solution, revenue, audience, ask};
  answers = {};
  judgeStates = {};
  currentJudgeIdx = 0;
  currentQuestionIdx = 0;

  document.getElementById('ph-name').textContent = name;
  document.getElementById('ph-problem').textContent = problem;
  document.getElementById('pm-audience').textContent = '🎯 ' + audience;
  document.getElementById('pm-model').textContent = '💰 ' + revenue;
  document.getElementById('pm-ask').textContent = '🤝 ' + ask;

  showScreen('pitch');
  buildJudgeCards();
  setTimeout(() => runJudge(0), 800);
}

function buildJudgeCards() {
  const grid = document.getElementById('judges-grid');
  grid.innerHTML = '';
  JUDGES.forEach((j, i) => {
    const card = document.createElement('div');
    card.className = 'judge-card';
    card.id = 'judge-card-' + j.id;
    card.innerHTML = `
      <div style="display:flex;align-items:center;gap:12px;margin-bottom:8px">
        <div class="judge-avatar" style="background:${j.color}22">${j.emoji}</div>
        <div>
          <div class="judge-name">${j.name}</div>
          <div class="judge-role">${j.role}</div>
          <div class="judge-status"><div class="status-dot" id="dot-${j.id}"></div><span id="status-text-${j.id}">Waiting...</span></div>
        </div>
      </div>
      <div class="judge-focus">${j.focus}</div>
      <div class="judge-chat" id="chat-${j.id}"></div>
    `;
    grid.appendChild(card);
  });
}

async function callClaude(prompt) {
  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method:'POST',
    headers:{'Content-Type':'application/json'},
    body:JSON.stringify({
      model:'claude-sonnet-4-6',
      max_tokens:1000,
      messages:[{role:'user',content:prompt}]
    })
  });
  const data = await res.json();
  return data.content?.[0]?.text || '';
}

function setProgress(pct) {
  document.getElementById('progress-fill').style.width = pct + '%';
}

function setJudgeActive(judgeId, active) {
  const card = document.getElementById('judge-card-' + judgeId);
  if(card) card.classList.toggle('active', active);
  const dot = document.getElementById('dot-' + judgeId);
  if(dot) dot.classList.toggle('active', active);
}

function setJudgeThinking(judgeId, thinking) {
  const card = document.getElementById('judge-card-' + judgeId);
  if(card) card.classList.toggle('thinking', thinking);
}

function setJudgeStatus(judgeId, text) {
  const el = document.getElementById('status-text-' + judgeId);
  if(el) el.textContent = text;
}

function addBubble(judgeId, text, type='question') {
  const chat = document.getElementById('chat-' + judgeId);
  if(!chat) return;
  const div = document.createElement('div');
  div.className = 'judge-bubble ' + type;
  div.textContent = text;
  div.style.opacity = '0';
  div.style.transform = 'translateY(8px)';
  div.style.transition = 'all .4s';
  chat.appendChild(div);
  setTimeout(() => { div.style.opacity='1'; div.style.transform='translateY(0)'; }, 50);
  return div;
}

function addAnswerInput(judgeId, qIdx, onAnswer) {
  const chat = document.getElementById('chat-' + judgeId);
  if(!chat) return;
  const div = document.createElement('div');
  div.className = 'answer-input';
  div.id = 'answer-input-' + judgeId + '-' + qIdx;
  div.innerHTML = `<input type="text" placeholder="Your answer..." id="ans-${judgeId}-${qIdx}"><button onclick="submitAnswer('${judgeId}',${qIdx})">Answer <i class="ti ti-send"></i></button>`;
  chat.appendChild(div);
  document.getElementById(`ans-${judgeId}-${qIdx}`).addEventListener('keydown', e => {
    if(e.key === 'Enter') submitAnswer(judgeId, qIdx);
  });
  setTimeout(() => document.getElementById(`ans-${judgeId}-${qIdx}`)?.focus(), 100);
}

let answerCallbacks = {};
function submitAnswer(judgeId, qIdx) {
  const input = document.getElementById(`ans-${judgeId}-${qIdx}`);
  if(!input) return;
  const val = input.value.trim();
  if(!val) return;
  const inputRow = document.getElementById(`answer-input-${judgeId}-${qIdx}`);
  if(inputRow) inputRow.remove();
  addBubble(judgeId, '🗣️ You: ' + val, 'question');
  if(answerCallbacks[judgeId + '-' + qIdx]) {
    answerCallbacks[judgeId + '-' + qIdx](val);
    delete answerCallbacks[judgeId + '-' + qIdx];
  }
}

function waitForAnswer(judgeId, qIdx) {
  return new Promise(resolve => {
    answerCallbacks[judgeId + '-' + qIdx] = resolve;
    addAnswerInput(judgeId, qIdx);
  });
}

async function runJudge(idx) {
  if(idx >= JUDGES.length) {
    await generateResults();
    return;
  }
  const judge = JUDGES[idx];
  setJudgeActive(judge.id, true);
  setJudgeStatus(judge.id, 'Reviewing pitch...');
  setJudgeThinking(judge.id, true);
  document.getElementById('pitch-status').textContent = `${judge.emoji} ${judge.name} is evaluating your pitch...`;
  setProgress((idx / JUDGES.length) * 70);

  await sleep(1200);
  setJudgeThinking(judge.id, false);

  for(let q = 0; q < judge.questions.length; q++) {
    setJudgeStatus(judge.id, `Asking question ${q+1}...`);
    addBubble(judge.id, judge.questions[q], 'question');
    const userAnswer = await waitForAnswer(judge.id, q);
    if(!answers[judge.id]) answers[judge.id] = [];
    answers[judge.id].push({q: judge.questions[q], a: userAnswer});

    setJudgeStatus(judge.id, 'Thinking...');
    setJudgeThinking(judge.id, true);

    const reactionPrompt = `You are ${judge.name}, a ${judge.role} on a show like Shark Tank. Your focus: ${judge.focus}.

Startup: "${pitchData.name}" - ${pitchData.problem}. Solution: ${pitchData.solution}. Revenue: ${pitchData.revenue}. Audience: ${pitchData.audience}. Asking: ${pitchData.ask}.

You asked: "${judge.questions[q]}"
They answered: "${userAnswer}"

Give a sharp, authentic 1-2 sentence reaction IN CHARACTER. Be direct, maybe a little tough. No fluff. Do not start with "I".`;

    const reaction = await callClaude(reactionPrompt);
    setJudgeThinking(judge.id, false);
    addBubble(judge.id, judge.emoji + ' ' + reaction.trim(), 'reaction');
  }

  setJudgeStatus(judge.id, 'Done');
  setProgress(((idx + 1) / JUDGES.length) * 70);
  await sleep(600);
  runJudge(idx + 1);
}

async function generateResults() {
  document.getElementById('pitch-status').textContent = 'All sharks have spoken. Calculating your score...';
  setProgress(75);
  setLoading(true, 'The sharks are deliberating...');

  const answersText = Object.entries(answers).map(([jid, qa]) => {
    const j = JUDGES.find(x => x.id === jid);
    return qa.map(x => `${j.name}: "${x.q}" → Answer: "${x.a}"`).join('\n');
  }).join('\n');

  const scorePrompt = `You are a panel of investors evaluating a startup pitch. Score this startup on 5 dimensions (0-100 each) and provide an investment decision.

Startup: "${pitchData.name}"
Problem: ${pitchData.problem}
Solution: ${pitchData.solution}
Revenue Model: ${pitchData.revenue}
Target Audience: ${pitchData.audience}
Funding Ask: ${pitchData.ask}

Q&A from pitch:
${answersText}

Respond ONLY with valid JSON, no markdown, no explanation:
{
  "scores": {
    "Market Potential": <0-100>,
    "Innovation": <0-100>,
    "Business Model": <0-100>,
    "Execution": <0-100>,
    "Investment Worthiness": <0-100>
  },
  "decision": "INVEST" or "REJECT" or "ACQUIRE" or "COME BACK LATER",
  "valuation": "<suggested valuation e.g. $5M>",
  "funding": "<funding amount offered e.g. $500K for 10%>",
  "reasoning": "<2-3 sentence honest investor reasoning for the decision>"
}`;

  try {
    const raw = await callClaude(scorePrompt);
    const clean = raw.replace(/```json|```/g, '').trim();
    const parsed = JSON.parse(clean);
    scores = parsed.scores;
    decision = parsed;
    setLoading(false);
    setProgress(100);
    showResultsScreen();
  } catch(e) {
    setLoading(false);
    scores = {
      'Market Potential': Math.floor(Math.random()*30+55),
      'Innovation': Math.floor(Math.random()*30+50),
      'Business Model': Math.floor(Math.random()*30+45),
      'Execution': Math.floor(Math.random()*30+50),
      'Investment Worthiness': Math.floor(Math.random()*30+50)
    };
    decision = {scores, decision:'COME BACK LATER', valuation:'$2M', funding:'$200K for 10%', reasoning:'Your pitch showed promise but the panel needs more data on market traction. Come back with early revenue metrics and customer validation numbers.'};
    showResultsScreen();
  }
}

function showResultsScreen() {
  showScreen('results');
  const vals = Object.values(scores);
  totalScoreVal = Math.round(vals.reduce((a,b)=>a+b,0)/vals.length);

  setTimeout(() => {
    const ring = document.getElementById('score-ring-fill');
    const offset = 314 - (314 * totalScoreVal / 100);
    ring.style.strokeDashoffset = offset;
    ring.style.stroke = totalScoreVal >= 75 ? '#10b981' : totalScoreVal >= 55 ? '#f59e0b' : '#ef4444';
  }, 200);

  let cnt = 0;
  const countUp = setInterval(() => {
    cnt = Math.min(cnt + 2, totalScoreVal);
    document.getElementById('total-score-val').textContent = cnt;
    if(cnt >= totalScoreVal) clearInterval(countUp);
  }, 20);

  const grid = document.getElementById('scores-grid');
  grid.innerHTML = '';
  SCORE_DIMS.forEach(dim => {
    const s = scores[dim] || 50;
    const color = s >= 75 ? '#10b981' : s >= 55 ? '#f59e0b' : '#ef4444';
    const div = document.createElement('div');
    div.className = 'score-card';
    div.innerHTML = `<div class="score-label">${dim}</div><div class="score-val" style="color:${color}">${s}</div><div class="score-bar"><div class="score-fill" style="width:0%;background:${color}" data-target="${s}"></div></div>`;
    grid.appendChild(div);
  });
  setTimeout(() => {
    document.querySelectorAll('.score-fill').forEach(el => {
      el.style.width = el.dataset.target + '%';
    });
  }, 300);

  const d = decision.decision || 'COME BACK LATER';
  const card = document.getElementById('decision-card');
  const icons = {INVEST:'🤝',REJECT:'👋',ACQUIRE:'🏆','COME BACK LATER':'⏳'};
  const classes = {INVEST:'invest',REJECT:'reject',ACQUIRE:'acquire','COME BACK LATER':'later'};
  const labels = {INVEST:'You\'ve Got a Deal!',REJECT:'We\'re Out.',ACQUIRE:'We Want to Acquire You','COME BACK LATER':'Come Back Later'};
  card.className = 'decision-card ' + (classes[d] || 'later');
  document.getElementById('decision-icon').textContent = icons[d] || '⏳';
  document.getElementById('decision-verdict').textContent = labels[d] || d;
  document.getElementById('decision-val').textContent = '📊 Valuation: ' + (decision.valuation || 'TBD');
  document.getElementById('decision-funding').textContent = '💵 Offer: ' + (decision.funding || 'None');
  document.getElementById('decision-reasoning').textContent = decision.reasoning || '';

  if(d === 'INVEST' || d === 'ACQUIRE') launchConfetti();

  addToLeaderboard(totalScoreVal, d);
  renderLeaderboard();
}

function addToLeaderboard(score, verdict) {
  leaderboardData.unshift({
    name: pitchData.name,
    score,
    verdict,
    date: new Date().toLocaleDateString()
  });
  leaderboardData = leaderboardData.slice(0, 10);
  leaderboardData.sort((a,b) => b.score - a.score);
  localStorage.setItem('shark_lb', JSON.stringify(leaderboardData));
}

function renderLeaderboard() {
  const rows = document.getElementById('lb-rows');
  rows.innerHTML = '';
  leaderboardData.forEach((item, i) => {
    const rankColors = ['#f59e0b','#9ca3af','#cd7f32'];
    const rankBg = i < 3 ? rankColors[i] : 'var(--bg4)';
    const vClass = {INVEST:'invest',REJECT:'reject',ACQUIRE:'acquire','COME BACK LATER':'later'}[item.verdict] || 'later';
    rows.innerHTML += `<div class="lb-row">
      <div class="lb-rank" style="background:${rankBg};color:${i<3?'#000':'var(--text2)'}">${i+1}</div>
      <div class="lb-name">${item.name}</div>
      <span class="tag tag-${vClass}">${item.verdict}</span>
      <div class="lb-score" style="margin-left:12px">${item.score}</div>
    </div>`;
  });
  if(!leaderboardData.length) rows.innerHTML = '<div style="color:var(--text3);font-size:13px;text-align:center;padding:12px">No pitches yet. Be the first!</div>';
}

function downloadReport() {
  const vals = Object.values(scores);
  const avg = Math.round(vals.reduce((a,b)=>a+b,0)/vals.length);
  const d = decision.decision || '';
  const content = `SHARK TANK PITCH REPORT
=============================
Startup: ${pitchData.name}
Date: ${new Date().toLocaleDateString()}
Funding Ask: ${pitchData.ask}

PITCH SUMMARY
-------------
Problem: ${pitchData.problem}
Solution: ${pitchData.solution}
Revenue Model: ${pitchData.revenue}
Target Audience: ${pitchData.audience}

SCORES
------
${SCORE_DIMS.map(dim => `${dim}: ${scores[dim] || 0}/100`).join('\n')}

OVERALL SCORE: ${avg}/100

INVESTMENT DECISION
-------------------
Decision: ${d}
Valuation: ${decision.valuation || 'N/A'}
Funding Offer: ${decision.funding || 'N/A'}
Reasoning: ${decision.reasoning || ''}

Q&A TRANSCRIPT
--------------
${Object.entries(answers).map(([jid, qa]) => {
  const j = JUDGES.find(x => x.id === jid);
  return `\n${j.name} (${j.role}):\n` + qa.map(x => `  Q: ${x.q}\n  A: ${x.a}`).join('\n');
}).join('\n')}

Generated by AI Shark Tank Simulator`;

  const blob = new Blob([content], {type:'text/plain'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `${pitchData.name.replace(/\s+/g,'_')}_pitch_report.txt`;
  a.click();
  URL.revokeObjectURL(url);
}

function shareResult() {
  const d = decision.decision || '';
  const text = `🦈 I just pitched "${pitchData.name}" on AI Shark Tank and got: ${d}! Score: ${totalScoreVal}/100. Try it yourself!`;
  if(navigator.clipboard) {
    navigator.clipboard.writeText(text).then(() => {
      const toast = document.getElementById('share-toast');
      toast.classList.add('visible');
      setTimeout(() => toast.classList.remove('visible'), 2800);
    });
  }
}

function resetApp() {
  document.getElementById('startup-name').value='';
  document.getElementById('problem').value='';
  document.getElementById('solution').value='';
  document.getElementById('revenue-model').value='';
  document.getElementById('target-audience').value='';
  document.getElementById('funding-ask').value='';
  pitchData = {}; answers = {}; scores = {}; decision = {};
  answerCallbacks = {};
  showScreen('input');
}

function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

function launchConfetti() {
  const canvas = document.getElementById('confetti-canvas');
  canvas.style.display = 'block';
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  const pieces = Array.from({length:150}, () => ({
    x: Math.random()*canvas.width,
    y: -20,
    r: Math.random()*6+3,
    c: ['#3b82f6','#10b981','#f59e0b','#8b5cf6','#ef4444','#06b6d4'][Math.floor(Math.random()*6)],
    vx: (Math.random()-0.5)*4,
    vy: Math.random()*4+2,
    rot: Math.random()*360,
    rSpeed: (Math.random()-0.5)*8,
    shape: Math.random() > 0.5 ? 'rect' : 'circle'
  }));
  let frame = 0;
  function draw() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    pieces.forEach(p => {
      p.x += p.vx;
      p.y += p.vy;
      p.vy += 0.05;
      p.rot += p.rSpeed;
      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(p.rot * Math.PI/180);
      ctx.fillStyle = p.c;
      if(p.shape === 'rect') ctx.fillRect(-p.r, -p.r/2, p.r*2, p.r);
      else { ctx.beginPath(); ctx.arc(0,0,p.r,0,Math.PI*2); ctx.fill(); }
      ctx.restore();
    });
    frame++;
    if(frame < 180 && pieces.some(p => p.y < canvas.height)) requestAnimationFrame(draw);
    else { ctx.clearRect(0,0,canvas.width,canvas.height); canvas.style.display='none'; }
  }
  draw();
}

renderLeaderboard();
</script>
</body>
</html>
