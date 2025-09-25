<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student App</title>
<style>
/* Основные стили */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    background: #f5f6fa;
    color: #2f3640;
}

h1 {
    text-align:center;
    margin-bottom:20px;
}

/* Вкладки */
.tab {
    display: flex;
    border-bottom: 2px solid #dcdde1;
    margin-bottom: 20px;
}
.tab button {
    background-color: #dcdde1;
    border: none;
    padding: 12px 25px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s, color 0.3s;
    margin-right: 5px;
    border-radius: 8px 8px 0 0;
}
.tab button.active {
    background-color: #0984e3;
    color: white;
}
.tab button:hover { background-color:#74b9ff; }

/* Контент вкладки */
.tabcontent { display: none; animation: fadeIn 0.3s ease; }
@keyframes fadeIn { from {opacity:0} to {opacity:1} }

/* Формы */
input, select, textarea, button {
    padding:10px;
    margin:5px 0;
    width:100%;
    box-sizing:border-box;
    border-radius:5px;
    border:1px solid #dcdde1;
    font-size:16px;
}
textarea { resize: vertical; }
button { 
    background-color:#0984e3;
    color:white;
    border:none;
    cursor:pointer;
    transition:0.3s;
}
button:hover { background-color:#74b9ff; }

/* Списки */
ul { list-style:none; padding:0; margin:10px 0; }

li {
    flex-direction: column;
    align-items: flex-start;
    padding: 12px 15px;
    margin-bottom: 12px;
    border-radius: 10px;
    background: #fff;
    box-shadow: 0 3px 8px rgba(0,0,0,0.1);
    transition: 0.3s;
}
li.completed { opacity: 0.6; text-decoration: line-through; }
li span { display: block; margin-bottom: 6px; font-size: 16px; }
li .desc { font-size: 14px; color: #555; }
li .buttons { display: flex; gap: 8px; margin-top: 8px; }
li button { flex: 1; padding: 6px; font-size: 14px; border-radius: 5px; }
li.urgent { border-left:5px solid #e74c3c; }

/* Прогрессбар */
.progress-container { background:#dcdde1; border-radius:5px; overflow:hidden; margin:15px 0; height:22px; }
.progress-bar { width:0%; height:100%; background:#27ae60; transition:0.5s; }

/* Статистика бюджета */
#percentages { text-align:center; margin-top:10px; font-size:16px; }

/* Фильтры уроков */
.lesson-filters { margin:15px 0; }
.lesson-filters button { width:auto; display:inline-block; margin-right:10px; font-size:14px; padding:6px 12px; border-radius:5px; background:#dcdde1; color:#2f3640; }
.lesson-filters button.active { background:#0984e3; color:white; }

/* Адаптив */
@media(max-width:600px){
    body { padding:10px; }
    .tab button { font-size:14px; padding:8px 12px; }
    input, select, textarea, button { font-size:14px; }
}
</style>
</head>
<body>

<h1>Student App</h1>

<div class="tab">
    <button class="tablinks active" onclick="openTab(event,'Budget')">Бюджет трекер</button>
    <button class="tablinks" onclick="openTab(event,'Lessons')">Уроки и задания</button>
</div>

<!-- Вкладка 1: Бюджет -->
<div id="Budget" class="tabcontent" style="display:block;">
    <h2>Бюджет трекер</h2>
    <input type="number" id="budget" placeholder="Введите бюджет на месяц" value="100000">
    <input type="text" id="description" placeholder="Описание">
    <input type="number" id="amount" placeholder="Сумма">
    <select id="category">
        <option value="Еда">Еда</option>
        <option value="Транспорт">Транспорт</option>
        <option value="Учёба">Учёба</option>
        <option value="Развлечения">Развлечения</option>
    </select>
    <button onclick="addExpense()">Добавить расход</button>

    <div class="progress-container">
        <div id="budgetBar" class="progress-bar"></div>
    </div>

    <p>Всего расходов: <span id="total">0</span> ₸</p>
    <p>Остаток бюджета: <span id="remaining">0</span> ₸</p>
    <p id="alert" style="color:#e74c3c;font-weight:bold;"></p>

    <h3>Расходы:</h3>
    <ul id="expenseList"></ul>

    <h3>Статистика:</h3>
    <canvas id="chart" width="400" height="200"></canvas>
    <div id="percentages"></div>
</div>

<!-- Вкладка 2: Уроки -->
<div id="Lessons" class="tabcontent">
    <h2>Задания</h2>
    <input type="text" id="lessonTitle" placeholder="Название урока">
    <textarea id="lessonDesc" placeholder="Описание задания"></textarea>
    <input type="date" id="lessonDate">

    <select id="lessonPriority">
        <option value="High">Высокий</option>
        <option value="Medium">Средний</option>
        <option value="Low">Низкий</option>
    </select>

    <button onclick="addLesson()">Добавить задание</button>

    <div class="lesson-filters">
        <button onclick="filterLessons('All')" class="active">Все</button>
        <button onclick="filterLessons('High')">Высокий</button>
        <button onclick="filterLessons('Medium')">Средний</button>
        <button onclick="filterLessons('Low')">Низкий</button>
    </div>

    <ul id="lessonList"></ul>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
// ====== Вкладки ======
function openTab(evt, tabName){
    document.querySelectorAll('.tabcontent').forEach(tc => tc.style.display='none');
    document.querySelectorAll('.tablinks').forEach(tb => tb.classList.remove('active'));
    document.getElementById(tabName).style.display='block';
    evt.currentTarget.classList.add('active');
}

// ====== Бюджет ======
let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
let budget = parseFloat(localStorage.getItem('budget')) || 100000;
document.getElementById('budget').value = budget;

function addExpense(){
    const desc = document.getElementById('description').value;
    const amount = parseFloat(document.getElementById('amount').value);
    const cat = document.getElementById('category').value;
    if(!desc||!amount) return alert("Заполните все поля!");
    expenses.push({description:desc, amount, category:cat});
    saveData(); document.getElementById('description').value=''; document.getElementById('amount').value='';
    updateList();
}

function updateList(){
    const list = document.getElementById('expenseList'); list.innerHTML='';
    let total = 0;
    expenses.forEach((exp,i)=>{
        total+=exp.amount;
        const li=document.createElement('li');
        li.innerHTML = `
            <span>${exp.description} - ${exp.amount} ₸</span>
            <span class="desc">Категория: ${exp.category}</span>
            <div class="buttons">
                <button onclick="editExpense(${i})">Редактировать</button>
                <button onclick="deleteExpense(${i})">Удалить</button>
            </div>`;
        list.appendChild(li);
    });
    document.getElementById('total').textContent = total;
    const remaining = budget-total;
    const remElem = document.getElementById('remaining');
    remElem.textContent = remaining;
    remElem.style.color = remaining<0?'#e74c3c':'#27ae60';
    document.getElementById('alert').textContent = remaining<0?"Бюджет превышен!":""; 
    updateBudgetBar(); updateChart();
}

function deleteExpense(i){ expenses.splice(i,1); saveData(); updateList(); }
function editExpense(i){
    const exp=expenses[i];
    const desc=prompt("Описание:",exp.description);
    const amount=parseFloat(prompt("Сумма:",exp.amount));
    const cat=prompt("Категория:",exp.category);
    if(desc && amount && cat){ expenses[i]={description:desc,amount,category:cat}; saveData(); updateList();}
}

function updateBudgetBar(){
    const total=expenses.reduce((s,e)=>s+e.amount,0);
    const percent=Math.min((total/budget)*100,100);
    const bar=document.getElementById('budgetBar'); bar.style.width=percent+'%';
    bar.style.background = percent>100?'#e74c3c':'#27ae60';
}

function updateChart(){
    const catTotals={};
    expenses.forEach(e=>catTotals[e.category]=(catTotals[e.category]||0)+e.amount);
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.myChart) window.myChart.destroy();
    window.myChart=new Chart(ctx,{type:'pie',data:{labels:Object.keys(catTotals),datasets:[{data:Object.values(catTotals),backgroundColor:['#ff7675','#74b9ff','#ffeaa7','#55efc4']}]}});

    const total=Object.values(catTotals).reduce((a,b)=>a+b,0);
    let text='';
    for(const cat in catTotals){ text+=`${cat}: ${((catTotals[cat]/total)*100).toFixed(1)}% &nbsp;`; }
    document.getElementById('percentages').innerHTML=text;
}

function saveData(){ localStorage.setItem('expenses',JSON.stringify(expenses)); budget=parseFloat(document.getElementById('budget').value)||100000; localStorage.setItem('budget',budget);}
updateList();

// ====== Уроки ======
let lessons = JSON.parse(localStorage.getItem('lessons'))||[];
let currentFilter = 'All';

function addLesson(){
    const title = document.getElementById('lessonTitle').value;
    const desc = document.getElementById('lessonDesc').value;
    const date = document.getElementById('lessonDate').value;
    const priority = document.getElementById('lessonPriority').value;
    if(!title||!desc||!date) return alert("Заполните все поля!");
    lessons.push({title,desc,date,priority,createdDate:new Date().toISOString().split('T')[0],completed:false});
    saveLessons(); updateLessonList();
    document.getElementById('lessonTitle').value=''; document.getElementById('lessonDesc').value=''; document.getElementById('lessonDate').value='';
}

function updateLessonList(){
    const list = document.getElementById('lessonList'); list.innerHTML='';
    const today = new Date();
    const priorityOrder = ['Low','Medium','High'];

    lessons.forEach((lesson,i)=>{
        const lessonDate = new Date(lesson.date);
        const createdDate = new Date(lesson.createdDate);
        const totalTime = lessonDate - createdDate;
        const elapsed = today - createdDate;
        const progress = totalTime>0 ? elapsed/totalTime : 1;
        let step=0; if(progress>=0.9) step=2; else if(progress>=0.6) step=1;
        let dynamicIndex = Math.min(priorityOrder.indexOf(lesson.priority)+step,2);
        const dynamicPriority = priorityOrder[dynamicIndex];

        if(currentFilter!=='All' && dynamicPriority!==currentFilter) return;

        const li=document.createElement('li');
        li.className=lesson.completed?'completed':'';
        if(!lesson.completed){
            if(dynamicPriority==='High') li.style.background='#ffe6e6';
            else if(dynamicPriority==='Medium') li.style.background='#fff0b3';
            else li.style.background='#e6ffe6';
        }
        if(!lesson.completed && today>lessonDate) li.classList.add('urgent');

        li.innerHTML = `
            <span><strong>${lesson.title}</strong></span>
            <span class="desc">${lesson.desc}</span>
            <span class="desc">Срок: ${lesson.date}</span>
            <span class="desc">Приоритет: ${dynamicPriority}</span>
            <div class="buttons">
                <button onclick="toggleLesson(${i})">${lesson.completed?'Отметить как невыполнено':'Выполнено'}</button>
                <button onclick="deleteLesson(${i})">Удалить</button>
            </div>`;
        list.appendChild(li);
    });
}

function toggleLesson(i){ lessons[i].completed = !lessons[i].completed; saveLessons(); updateLessonList(); }
function deleteLesson(i){ lessons.splice(i,1); saveLessons(); updateLessonList(); }
function saveLessons(){ localStorage.setItem('lessons',JSON.stringify(lessons)); }

function filterLessons(priority){
    currentFilter = priority;
    document.querySelectorAll('.lesson-filters button').forEach(b=>b.classList.remove('active'));
    document.querySelector(`.lesson-filters button[onclick="filterLessons('${priority}')"]`).classList.add('active');
    updateLessonList();
}

// Автообновление приоритетов каждые 30 секунд
setInterval(updateLessonList,30000);

updateLessonList();
</script>
</body>
</html>
