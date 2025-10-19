<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Мой План — Dashboard</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Inter", system-ui, sans-serif;
    }
    body {
      margin: 0;
      background: #f6f8fb;
      color: #1e293b;
      display: flex;
      min-height: 100vh;
    }

    /* --- Sidebar --- */
    aside {
      width: 240px;
      background: #ffffff;
      border-right: 1px solid #e2e8f0;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 24px 18px;
    }
    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .logo-icon {
      width: 40px;
      height: 40px;
      border-radius: 12px;
      background: linear-gradient(135deg, #3b82f6, #06b6d4);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: 600;
      font-size: 16px;
    }
    .logo-text { font-weight: 600; font-size: 15px; line-height: 1.2; }
    .logo-sub { font-size: 12px; color: #64748b; }

    nav {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 30px;
    }
    nav button {
      all: unset;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 14px;
      color: #334155;
      padding: 8px 10px;
      border-radius: 8px;
      transition: background 0.2s;
    }
    nav button:hover { background: #f1f5f9; }

    aside footer {
      font-size: 12px;
      color: #94a3b8;
      margin-top: 30px;
    }

    /* --- Layout --- */
    main {
      flex: 1;
      padding: 30px;
      display: grid;
      grid-template-columns: repeat(12, 1fr);
      gap: 20px;
    }
    section {
      background: #ffffff;
      border: 1px solid #e2e8f0;
      border-radius: 12px;
      padding: 18px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.03);
    }
    h2 {
      margin: 0 0 10px;
      font-size: 16px;
      font-weight: 600;
      color: #0f172a;
    }

    input, select, textarea {
      border: 1px solid #cbd5e1;
      border-radius: 8px;
      padding: 8px 10px;
      font-size: 14px;
      width: 100%;
    }
    input:focus, textarea:focus, select:focus {
      border-color: #3b82f6;
      outline: none;
    }
    button.action {
      background: #3b82f6;
      border: none;
      color: white;
      border-radius: 8px;
      padding: 8px 14px;
      font-size: 14px;
      cursor: pointer;
      transition: 0.2s;
    }
    button.action:hover { background: #2563eb; }

    ul { list-style: none; padding: 0; margin: 0; }
    li {
      background: #f8fafc;
      border-radius: 8px;
      padding: 8px 10px;
      margin-bottom: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 14px;
    }
    .done { text-decoration: line-through; color: #94a3b8; }

    .goal-progress {
      height: 6px;
      background: #e2e8f0;
      border-radius: 4px;
      overflow: hidden;
      margin-top: 4px;
    }
    .goal-bar {
      height: 100%;
      background: linear-gradient(90deg, #3b82f6, #10b981);
    }

    textarea {
      resize: none;
      height: 120px;
      margin-top: 6px;
    }

    /* --- Finance --- */
    #financeSummary {
      font-size: 15px;
      line-height: 1.6;
      margin-bottom: 10px;
    }
    #financeList {
      margin-top: 10px;
      max-height: 200px;
      overflow-y: auto;
    }
    .finance-entry {
      font-size: 15px;
      display: flex;
      justify-content: space-between;
      background: #f8fafc;
      border-radius: 6px;
      padding: 8px 10px;
      margin-bottom: 6px;
    }
    .income { color: #10b981; }
    .expense { color: #ef4444; }

    /* Layout columns */
    .col-3 { grid-column: span 3; }
    .col-2 { grid-column: span 2; }
    .col-8 { grid-column: span 8; }

    /* Reset button */
    #resetBtn {
      position: fixed;
      bottom: 25px;
      right: 30px;
      background: #ef4444;
      border: none;
      color: white;
      border-radius: 50%;
      width: 54px;
      height: 54px;
      font-size: 22px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
      transition: all 0.25s ease;
    }
    #resetBtn:hover {
      background: #dc2626;
      transform: scale(1.1);
      box-shadow: 0 6px 16px rgba(0,0,0,0.25);
    }
    #resetBtn::after {
      content: "🗑️";
      font-size: 22px;
    }
  </style>
</head>
<body>
  <aside>
    <div>
      <div class="logo">
        <div class="logo-icon">VS</div>
        <div>
          <div class="logo-text">Мой План</div>
          <div class="logo-sub">Панель задач и финансы</div>
        </div>
      </div>
      <nav>
        <button>📋 Задачи</button>
        <button>🎯 Цели</button>
        <button>💳 Финансы</button>
        <button>🗓️ Календарь</button>
      </nav>
    </div>
    <footer>Сохранено в localStorage</footer>
  </aside>

  <main>
    <section class="col-3">
      <h2>Задачи</h2>
      <div style="display:flex;gap:8px;margin-bottom:10px;">
        <input id="taskInput" placeholder="Добавить задачу..." />
        <button class="action" onclick="addTask()">Добавить</button>
      </div>
      <ul id="taskList"></ul>
    </section>

    <section class="col-3">
      <h2>Цели</h2>
      <div style="display:flex;gap:8px;margin-bottom:10px;">
        <input id="goalInput" placeholder="Новая цель..." />
        <button class="action" onclick="addGoal()">Добавить</button>
      </div>
      <div id="goalList"></div>
    </section>

    <section class="col-2">
      <h2>Финансы</h2>
      <div id="financeSummary"></div>
      <div style="display:flex;gap:6px;flex-wrap:wrap;margin-bottom:10px;">
        <select id="financeType">
          <option value="expense">Расход</option>
          <option value="income">Доход</option>
        </select>
        <input id="financeLabel" placeholder="Описание" style="flex:1;" />
        <input id="financeAmount" type="number" placeholder="Сумма" style="width:120px;" />
        <button class="action" onclick="addFinance()">+</button>
      </div>
      <div id="financeList"></div>
    </section>

    <section class="col-8">
      <h2>Дорожная карта / Заметки</h2>
      <textarea id="notes" placeholder="Пиши заметки сюда..."></textarea>
    </section>
  </main>

  <button id="resetBtn" onclick="resetAll()" title="Сбросить всё"></button>

  <script>
    const save = (k,v)=>localStorage.setItem(k,JSON.stringify(v));
    const load = (k,f)=>{try{return JSON.parse(localStorage.getItem(k))||f}catch{return f}};

    // --- Tasks ---
    let tasks=load("tasks",[]);
    function renderTasks(){
      const list=document.getElementById("taskList");
      list.innerHTML="";
      tasks.forEach(t=>{
        const li=document.createElement("li");
        li.innerHTML=`<span class="${t.done?'done':''}">${t.title}</span>
          <div><button onclick="toggleTask(${t.id})">✔</button>
          <button onclick="removeTask(${t.id})">✖</button></div>`;
        list.appendChild(li);
      });
      save("tasks",tasks);
    }
    function addTask(){
      const i=document.getElementById("taskInput");
      if(!i.value.trim())return;
      tasks.unshift({id:Date.now(),title:i.value.trim(),done:false});
      i.value=""; renderTasks();
    }
    function toggleTask(id){tasks=tasks.map(t=>t.id===id?{...t,done:!t.done}:t);renderTasks();}
    function removeTask(id){tasks=tasks.filter(t=>t.id!==id);renderTasks();}

    // --- Goals ---
    let goals=load("goals",[]);
    function renderGoals(){
      const area=document.getElementById("goalList");
      area.innerHTML="";
      goals.forEach(g=>{
        const d=document.createElement("div");
        d.style.marginBottom="10px";
        d.innerHTML=`
          <div style="display:flex;justify-content:space-between;align-items:center;">
            <div>${g.title} — ${g.progress}%</div>
            <div>
              <button onclick="updateGoal(${g.id},10)">+10%</button>
              <button onclick="removeGoal(${g.id})">✖</button>
            </div>
          </div>
          <div class="goal-progress"><div class="goal-bar" style="width:${g.progress}%"></div></div>
        `;
        area.appendChild(d);
      });
      save("goals",goals);
    }
    function addGoal(){
      const i=document.getElementById("goalInput");
      if(!i.value.trim())return;
      goals.push({id:Date.now(),title:i.value.trim(),progress:0});
      i.value=""; renderGoals();
    }
    function updateGoal(id,d){goals=goals.map(g=>g.id===id?{...g,progress:Math.min(100,g.progress+d)}:g);renderGoals();}
    function removeGoal(id){goals=goals.filter(g=>g.id!==id);renderGoals();}

    // --- Finance ---
    let finance=load("finance",[]);
    function renderFinance(){
      const income=finance.filter(f=>f.type==='income').reduce((s,f)=>s+f.amount,0);
      const expense=finance.filter(f=>f.type==='expense').reduce((s,f)=>s+f.amount,0);
      const bal=income-expense;
      document.getElementById("financeSummary").innerHTML=`
        <b>Доход:</b> ${income.toLocaleString()}₸<br>
        <b>Расход:</b> ${expense.toLocaleString()}₸<br>
        <b>Баланс:</b> <span style="color:${bal>=0?'#10b981':'#ef4444'}">${bal.toLocaleString()}₸</span>`;
      const list=document.getElementById("financeList");
      list.innerHTML="";
      finance.forEach(f=>{
        const d=document.createElement("div");
        d.className="finance-entry "+f.type;
        d.textContent=`${f.label}: ${f.amount.toLocaleString()}₸`;
        list.appendChild(d);
      });
      save("finance",finance);
    }
    function addFinance(){
      const type=document.getElementById("financeType").value;
      const label=document.getElementById("financeLabel").value.trim();
      const amount=Number(document.getElementById("financeAmount").value);
      if(!label||!amount)return;
      finance.unshift({id:Date.now(),type,label,amount});
      document.getElementById("financeLabel").value="";
      document.getElementById("financeAmount").value="";
      renderFinance();
    }

    // --- Notes ---
    const notesEl=document.getElementById("notes");
    notesEl.value=load("notes","");
    notesEl.addEventListener("input",()=>save("notes",notesEl.value));

    // --- Reset ---
    function resetAll(){
      if(confirm("Удалить все данные?")) {
        localStorage.clear();
        tasks=[]; goals=[]; finance=[];
        notesEl.value="";
        renderTasks(); renderGoals(); renderFinance();
        alert("Все данные сброшены ✅");
      }
    }

    renderTasks(); renderGoals(); renderFinance();
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Мой План — Dashboard</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: "Inter", system-ui, sans-serif;
    }
    body {
      margin: 0;
      background: #f6f8fb;
      color: #1e293b;
      display: flex;
      min-height: 100vh;
    }

    /* --- Sidebar --- */
    aside {
      width: 240px;
      background: #ffffff;
      border-right: 1px solid #e2e8f0;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 24px 18px;
    }
    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .logo-icon {
      width: 40px;
      height: 40px;
      border-radius: 12px;
      background: linear-gradient(135deg, #3b82f6, #06b6d4);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: 600;
      font-size: 16px;
    }
    .logo-text { font-weight: 600; font-size: 15px; line-height: 1.2; }
    .logo-sub { font-size: 12px; color: #64748b; }

    nav {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 30px;
    }
    nav button {
      all: unset;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 14px;
      color: #334155;
      padding: 8px 10px;
      border-radius: 8px;
      transition: background 0.2s;
    }
    nav button:hover { background: #f1f5f9; }

    aside footer {
      font-size: 12px;
      color: #94a3b8;
      margin-top: 30px;
    }

    /* --- Layout --- */
    main {
      flex: 1;
      padding: 30px;
      display: grid;
      grid-template-columns: repeat(12, 1fr);
      gap: 20px;
    }
    section {
      background: #ffffff;
      border: 1px solid #e2e8f0;
      border-radius: 12px;
      padding: 18px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.03);
    }
    h2 {
      margin: 0 0 10px;
      font-size: 16px;
      font-weight: 600;
      color: #0f172a;
    }

    input, select, textarea {
      border: 1px solid #cbd5e1;
      border-radius: 8px;
      padding: 8px 10px;
      font-size: 14px;
      width: 100%;
    }
    input:focus, textarea:focus, select:focus {
      border-color: #3b82f6;
      outline: none;
    }
    button.action {
      background: #3b82f6;
      border: none;
      color: white;
      border-radius: 8px;
      padding: 8px 14px;
      font-size: 14px;
      cursor: pointer;
      transition: 0.2s;
    }
    button.action:hover { background: #2563eb; }

    ul { list-style: none; padding: 0; margin: 0; }
    li {
      background: #f8fafc;
      border-radius: 8px;
      padding: 8px 10px;
      margin-bottom: 6px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 14px;
    }
    .done { text-decoration: line-through; color: #94a3b8; }

    .goal-progress {
      height: 6px;
      background: #e2e8f0;
      border-radius: 4px;
      overflow: hidden;
      margin-top: 4px;
    }
    .goal-bar {
      height: 100%;
      background: linear-gradient(90deg, #3b82f6, #10b981);
    }

    textarea {
      resize: none;
      height: 120px;
      margin-top: 6px;
    }

    /* --- Finance --- */
    #financeSummary {
      font-size: 15px;
      line-height: 1.6;
      margin-bottom: 10px;
    }
    #financeList {
      margin-top: 10px;
      max-height: 200px;
      overflow-y: auto;
    }
    .finance-entry {
      font-size: 15px;
      display: flex;
      justify-content: space-between;
      background: #f8fafc;
      border-radius: 6px;
      padding: 8px 10px;
      margin-bottom: 6px;
    }
    .income { color: #10b981; }
    .expense { color: #ef4444; }

    /* Layout columns */
    .col-3 { grid-column: span 3; }
    .col-2 { grid-column: span 2; }
    .col-8 { grid-column: span 8; }

    /* Reset button */
    #resetBtn {
      position: fixed;
      bottom: 25px;
      right: 30px;
      background: #ef4444;
      border: none;
      color: white;
      border-radius: 50%;
      width: 54px;
      height: 54px;
      font-size: 22px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
      transition: all 0.25s ease;
    }
    #resetBtn:hover {
      background: #dc2626;
      transform: scale(1.1);
      box-shadow: 0 6px 16px rgba(0,0,0,0.25);
    }
    #resetBtn::after {
      content: "🗑️";
      font-size: 22px;
    }
  </style>
</head>
<body>
  <aside>
    <div>
      <div class="logo">
        <div class="logo-icon">VS</div>
        <div>
          <div class="logo-text">Мой План</div>
          <div class="logo-sub">Панель задач и финансы</div>
        </div>
      </div>
      <nav>
        <button>📋 Задачи</button>
        <button>🎯 Цели</button>
        <button>💳 Финансы</button>
        <button>🗓️ Календарь</button>
      </nav>
    </div>
    <footer>Сохранено в localStorage</footer>
  </aside>

  <main>
    <section class="col-3">
      <h2>Задачи</h2>
      <div style="display:flex;gap:8px;margin-bottom:10px;">
        <input id="taskInput" placeholder="Добавить задачу..." />
        <button class="action" onclick="addTask()">Добавить</button>
      </div>
      <ul id="taskList"></ul>
    </section>

    <section class="col-3">
      <h2>Цели</h2>
      <div style="display:flex;gap:8px;margin-bottom:10px;">
        <input id="goalInput" placeholder="Новая цель..." />
        <button class="action" onclick="addGoal()">Добавить</button>
      </div>
      <div id="goalList"></div>
    </section>

    <section class="col-2">
      <h2>Финансы</h2>
      <div id="financeSummary"></div>
      <div style="display:flex;gap:6px;flex-wrap:wrap;margin-bottom:10px;">
        <select id="financeType">
          <option value="expense">Расход</option>
          <option value="income">Доход</option>
        </select>
        <input id="financeLabel" placeholder="Описание" style="flex:1;" />
        <input id="financeAmount" type="number" placeholder="Сумма" style="width:120px;" />
        <button class="action" onclick="addFinance()">+</button>
      </div>
      <div id="financeList"></div>
    </section>

    <section class="col-8">
      <h2>Дорожная карта / Заметки</h2>
      <textarea id="notes" placeholder="Пиши заметки сюда..."></textarea>
    </section>
  </main>

  <button id="resetBtn" onclick="resetAll()" title="Сбросить всё"></button>

  <script>
    const save = (k,v)=>localStorage.setItem(k,JSON.stringify(v));
    const load = (k,f)=>{try{return JSON.parse(localStorage.getItem(k))||f}catch{return f}};

    // --- Tasks ---
    let tasks=load("tasks",[]);
    function renderTasks(){
      const list=document.getElementById("taskList");
      list.innerHTML="";
      tasks.forEach(t=>{
        const li=document.createElement("li");
        li.innerHTML=`<span class="${t.done?'done':''}">${t.title}</span>
          <div><button onclick="toggleTask(${t.id})">✔</button>
          <button onclick="removeTask(${t.id})">✖</button></div>`;
        list.appendChild(li);
      });
      save("tasks",tasks);
    }
    function addTask(){
      const i=document.getElementById("taskInput");
      if(!i.value.trim())return;
      tasks.unshift({id:Date.now(),title:i.value.trim(),done:false});
      i.value=""; renderTasks();
    }
    function toggleTask(id){tasks=tasks.map(t=>t.id===id?{...t,done:!t.done}:t);renderTasks();}
    function removeTask(id){tasks=tasks.filter(t=>t.id!==id);renderTasks();}

    // --- Goals ---
    let goals=load("goals",[]);
    function renderGoals(){
      const area=document.getElementById("goalList");
      area.innerHTML="";
      goals.forEach(g=>{
        const d=document.createElement("div");
        d.style.marginBottom="10px";
        d.innerHTML=`
          <div style="display:flex;justify-content:space-between;align-items:center;">
            <div>${g.title} — ${g.progress}%</div>
            <div>
              <button onclick="updateGoal(${g.id},10)">+10%</button>
              <button onclick="removeGoal(${g.id})">✖</button>
            </div>
          </div>
          <div class="goal-progress"><div class="goal-bar" style="width:${g.progress}%"></div></div>
        `;
        area.appendChild(d);
      });
      save("goals",goals);
    }
    function addGoal(){
      const i=document.getElementById("goalInput");
      if(!i.value.trim())return;
      goals.push({id:Date.now(),title:i.value.trim(),progress:0});
      i.value=""; renderGoals();
    }
    function updateGoal(id,d){goals=goals.map(g=>g.id===id?{...g,progress:Math.min(100,g.progress+d)}:g);renderGoals();}
    function removeGoal(id){goals=goals.filter(g=>g.id!==id);renderGoals();}

    // --- Finance ---
    let finance=load("finance",[]);
    function renderFinance(){
      const income=finance.filter(f=>f.type==='income').reduce((s,f)=>s+f.amount,0);
      const expense=finance.filter(f=>f.type==='expense').reduce((s,f)=>s+f.amount,0);
      const bal=income-expense;
      document.getElementById("financeSummary").innerHTML=`
        <b>Доход:</b> ${income.toLocaleString()}₸<br>
        <b>Расход:</b> ${expense.toLocaleString()}₸<br>
        <b>Баланс:</b> <span style="color:${bal>=0?'#10b981':'#ef4444'}">${bal.toLocaleString()}₸</span>`;
      const list=document.getElementById("financeList");
      list.innerHTML="";
      finance.forEach(f=>{
        const d=document.createElement("div");
        d.className="finance-entry "+f.type;
        d.textContent=`${f.label}: ${f.amount.toLocaleString()}₸`;
        list.appendChild(d);
      });
      save("finance",finance);
    }
    function addFinance(){
      const type=document.getElementById("financeType").value;
      const label=document.getElementById("financeLabel").value.trim();
      const amount=Number(document.getElementById("financeAmount").value);
      if(!label||!amount)return;
      finance.unshift({id:Date.now(),type,label,amount});
      document.getElementById("financeLabel").value="";
      document.getElementById("financeAmount").value="";
      renderFinance();
    }

    // --- Notes ---
    const notesEl=document.getElementById("notes");
    notesEl.value=load("notes","");
    notesEl.addEventListener("input",()=>save("notes",notesEl.value));

    // --- Reset ---
    function resetAll(){
      if(confirm("Удалить все данные?")) {
        localStorage.clear();
        tasks=[]; goals=[]; finance=[];
        notesEl.value="";
        renderTasks(); renderGoals(); renderFinance();
        alert("Все данные сброшены ✅");
      }
    }

    renderTasks(); renderGoals(); renderFinance();
  </script>
</body>
</html>
