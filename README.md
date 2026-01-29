# project1
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Discussion Portal | College Project</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif}
    body{background:#0f172a;color:#e5e7eb}
    header{background:linear-gradient(90deg,#6366f1,#8b5cf6);padding:18px 40px;font-size:24px;font-weight:600}
    .container{max-width:1200px;margin:auto;padding:30px}
    .top-bar{display:flex;justify-content:space-between;gap:20px;flex-wrap:wrap;margin-bottom:25px}
    .top-bar input{flex:1;padding:14px;border-radius:12px;border:none;outline:none}
    .stats{display:flex;gap:20px}
    .stat{background:#1e293b;padding:14px 20px;border-radius:14px;text-align:center}
    .stat h4{font-size:20px;color:#a5b4fc}
    .card{background:#1e293b;border-radius:18px;padding:25px;box-shadow:0 10px 30px rgba(0,0,0,.35);margin-bottom:25px}
    .card h3{margin-bottom:12px;font-weight:500}
    textarea{width:100%;padding:14px;border-radius:14px;border:none;outline:none;resize:none}
    button{background:linear-gradient(90deg,#6366f1,#8b5cf6);border:none;color:#fff;padding:12px 22px;border-radius:14px;cursor:pointer;font-weight:500}
    button:hover{opacity:.9}
    .question{border-left:5px solid #6366f1}
    .answers{margin-top:15px;padding-left:15px;border-left:2px dashed #475569}
    .answer{background:#020617;padding:12px 16px;border-radius:12px;margin-top:10px}
    .resolved{border-left-color:#22c55e}
    .badge{display:inline-block;background:#22c55e;color:#052e16;padding:4px 10px;border-radius:999px;font-size:12px;margin-left:10px}
  </style>
</head>
<body>
<header>Discussion Portal <span style="font-size:14px;font-weight:400">College Submission</span></header>
<div class="container">
  <div class="top-bar">
    <input type="text" id="search" placeholder="Search questions..." onkeyup="searchQuestions()" />
    <div class="stats">
      <div class="stat"><h4 id="qCount">0</h4><p>Questions</p></div>
      <div class="stat"><h4 id="aCount">0</h4><p>Answers</p></div>
    </div>
  </div>

  <div class="card">
    <h3>Ask a New Question</h3>
    <textarea id="questionInput" rows="3" placeholder="Type your question here..."></textarea><br><br>
    <button onclick="addQuestion()">Post Question</button>
  </div>

  <div id="questions"></div>
</div>

<script>
  let data = [];

  function addQuestion(){
    const q = questionInput.value.trim();
    if(!q) return alert('Please enter a question');
    data.push({q, answers:[], resolved:false});
    questionInput.value='';
    render();
  }

  function addAnswer(i){
    const input = document.getElementById('ans'+i);
    if(!input.value.trim()) return;
    data[i].answers.push(input.value);
    input.value='';
    render();
  }

  function markResolved(i){
    data[i].resolved=true;
    render();
  }

  function render(){
    const box=document.getElementById('questions');
    box.innerHTML='';
    let answers=0;
    data.forEach((item,i)=>{
      answers+=item.answers.length;
      box.innerHTML+=`
      <div class="card question ${item.resolved?'resolved':''}">
        <h3>${item.q} ${item.resolved?'<span class="badge">Resolved</span>':''}</h3>
        <textarea id="ans${i}" rows="2" placeholder="Write your answer..."></textarea><br><br>
        <button onclick="addAnswer(${i})">Add Answer</button>
        <button style="margin-left:10px;background:#22c55e" onclick="markResolved(${i})">Mark Resolved</button>
        <div class="answers">
          ${item.answers.map(a=>`<div class='answer'>${a}</div>`).join('')}
        </div>
      </div>`;
    });
    qCount.innerText=data.length;
    aCount.innerText=answers;
  }

  function searchQuestions(){
    const term=search.value.toLowerCase();
    document.querySelectorAll('.question').forEach(q=>{
      q.style.display=q.innerText.toLowerCase().includes(term)?'block':'none';
    });
  }
</script>
</body>
</html>
