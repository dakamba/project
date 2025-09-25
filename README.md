<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student App</title>
<style>
/* CSS Variables for consistent theming */
:root {
    --primary-color: #0984e3;
    --primary-light: #74b9ff;
    --danger-color: #e74c3c;
    --success-color: #27ae60;
    --warning-color: #f39c12;
    --text-color: #2f3640;
    --text-light: #555;
    --bg-color: #f5f6fa;
    --card-bg: #fff;
    --border-color: #dcdde1;
    --border-radius: 8px;
    --shadow: 0 3px 8px rgba(0,0,0,0.1);
}

/* Reset and base styles */
* {
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    background: var(--bg-color);
    color: var(--text-color);
    line-height: 1.5;
}

h1, h2, h3 {
    margin-top: 0;
}

h1 { 
    text-align: center; 
    margin-bottom: 20px; 
    color: var(--primary-color);
}

/* Tab styles */
.tab {
    display: flex;
    border-bottom: 2px solid var(--border-color);
    margin-bottom: 20px;
    flex-wrap: wrap;
}

.tab button {
    background-color: var(--border-color);
    border: none;
    padding: 12px 25px;
    cursor: pointer;
    font-size: 16px;
    transition: all 0.3s ease;
    margin-right: 5px;
    border-radius: var(--border-radius) var(--border-radius) 0 0;
    flex: 1;
    min-width: 120px;
}

.tab button.active { 
    background-color: var(--primary-color); 
    color: white; 
}

.tab button:hover { 
    background-color: var(--primary-light); 
    color: white;
}

/* Tab content */
.tabcontent { 
    display: none; 
    animation: fadeIn 0.3s ease; 
}

@keyframes fadeIn { 
    from { opacity: 0; transform: translateY(10px); } 
    to { opacity: 1; transform: translateY(0); } 
}

/* Form elements */
.form-group {
    margin-bottom: 15px;
}

label {
    display: block;
    margin-bottom: 5px;
    font-weight: 600;
}

input, select, textarea, button {
    padding: 10px;
    margin: 5px 0;
    width: 100%;
    border-radius: var(--border-radius);
    border: 1px solid var(--border-color);
    font-size: 16px;
    transition: border-color 0.3s;
}

input:focus, select:focus, textarea:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 2px rgba(9, 132, 227, 0.2);
}

textarea { 
    resize: vertical; 
    min-height: 100px;
}

button { 
    background-color: var(--primary-color);
    color: white;
    border: none;
    cursor: pointer;
    transition: 0.3s;
    font-weight: 600;
}

button:hover { 
    background-color: var(--primary-light); 
}

button.secondary {
    background-color: #6c757d;
}

button.secondary:hover {
    background-color: #5a6268;
}

button.danger {
    background-color: var(--danger-color);
}

button.danger:hover {
    background-color: #c0392b;
}

/* Lists */
ul { 
    list-style: none; 
    padding: 0; 
    margin: 10px 0; 
}

li {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 15px;
    margin-bottom: 12px;
    border-radius: var(--border-radius);
    background: var(--card-bg);
    box-shadow: var(--shadow);
    transition: 0.3s;
    position: relative;
}

li:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

li.completed { 
    opacity: 0.7; 
    background-color: #f8f9fa;
}

li.completed span:first-child {
    text-decoration: line-through;
}

li span { 
    display: block; 
    margin-bottom: 6px; 
    font-size: 16px; 
}

li .desc { 
    font-size: 14px; 
    color: var(--text-light); 
}

li .buttons { 
    display: flex; 
    gap: 8px; 
    margin-top: 8px; 
    width: 100%;
}

li button { 
    flex: 1; 
    padding: 8px; 
    font-size: 14px; 
    border-radius: 5px; 
}

li.urgent { 
    border-left: 5px solid var(--danger-color); 
}

/* Priority indicators */
.priority-high { border-left: 5px solid var(--danger-color); }
.priority-medium { border-left: 5px solid var(--warning-color); }
.priority-low { border-left: 5px solid var(--success-color); }

/* Progress bar */
.progress-container { 
    background: var(--border-color); 
    border-radius: 5px; 
    overflow: hidden; 
    margin: 15px 0; 
    height: 22px; 
    position: relative;
}

.progress-bar { 
    width: 0%; 
    height: 100%; 
    background: var(--success-color); 
    transition: 0.5s; 
    position: relative;
}

.progress-text {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    text-align: center;
    font-size: 12px;
    font-weight: bold;
    color: #fff;
    text-shadow: 0 0 2px rgba(0,0,0,0.5);
    line-height: 22px;
}

/* Statistics */
.statistics {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 15px;
    margin: 20px 0;
}

.stat-card {
    background: var(--card-bg);
    padding: 15px;
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    text-align: center;
}

.stat-value {
    font-size: 24px;
    font-weight: bold;
    margin: 10px 0;
}

.stat-label {
    font-size: 14px;
    color: var(--text-light);
}

/* Filters */
.lesson-filters { 
    margin: 15px 0; 
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

.lesson-filters button { 
    width: auto; 
    display: inline-block; 
    font-size: 14px; 
    padding: 8px 15px; 
    border-radius: 20px; 
    background: var(--border-color); 
    color: var(--text-color); 
    transition: all 0.3s;
}

.lesson-filters button.active { 
    background: var(--primary-color); 
    color: white; 
}

/* Empty state */
.empty-state {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
}

.empty-state i {
    font-size: 48px;
    margin-bottom: 15px;
    opacity: 0.5;
}

/* Alert messages */
.alert {
    padding: 12px 15px;
    border-radius: var(--border-radius);
    margin: 15px 0;
    font-weight: 600;
}

.alert-danger {
    background-color: #f8d7da;
    color: #721c24;
    border-left: 4px solid var(--danger-color);
}

.alert-success {
    background-color: #d1edff;
    color: #0c5460;
    border-left: 4px solid var(--primary-color);
}

/* Responsive design */
@media (max-width: 768px) {
    body { 
        padding: 15px; 
    }
    
    .tab button { 
        font-size: 14px; 
        padding: 10px 15px; 
        min-width: 100px;
    }
    
    input, select, textarea, button { 
        font-size: 16px; /* Keep readable on mobile */
    }
    
    .statistics {
        grid-template-columns: 1fr;
    }
    
    li .buttons {
        flex-direction: column;
    }
}

@media (max-width: 480px) {
    body { 
        padding: 10px; 
    }
    
    .tab {
        flex-direction: column;
    }
    
    .tab button {
        border-radius: var(--border-radius);
        margin-bottom: 5px;
    }
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
    
    <div class="form-group">
        <label for="budget">Бюджет на месяц (₸)</label>
        <input type="number" id="budget" placeholder="Введите бюджет на месяц" value="100000" min="0">
    </div>
    
    <div class="form-group">
        <label for="description">Описание расхода</label>
        <input type="text" id="description" placeholder="Описание">
    </div>
    
    <div class="form-group">
        <label for="amount">Сумма (₸)</label>
        <input type="number" id="amount" placeholder="Сумма" min="0" step="0.01">
    </div>
    
    <div class="form-group">
        <label for="category">Категория</label>
        <select id="category">
            <option value="Еда">Еда</option>
            <option value="Транспорт">Транспорт</option>
            <option value="Учёба">Учёба</option>
            <option value="Развлечения">Развлечения</option>
            <option value="Другое">Другое</option>
        </select>
    </div>
    
    <button onclick="addExpense()">Добавить расход</button>

    <div class="progress-container">
        <div id="budgetBar" class="progress-bar">
            <div class="progress-text" id="progressText">0%</div>
        </div>
    </div>

    <div class="statistics">
        <div class="stat-card">
            <div class="stat-label">Всего расходов</div>
            <div class="stat-value" id="total">0 ₸</div>
        </div>
        <div class="stat-card">
            <div class="stat-label">Остаток бюджета</div>
            <div class="stat-value" id="remaining">0 ₸</div>
        </div>
        <div class="stat-card">
            <div class="stat-label">Использовано</div>
            <div class="stat-value" id="usedPercentage">0%</div>
        </div>
    </div>

    <div id="alert" class="alert" style="display: none;"></div>

    <h3>История расходов:</h3>
    <ul id="expenseList"></ul>

    <h3>Статистика по категориям:</h3>
    <canvas id="chart" width="400" height="200"></canvas>
    <div id="percentages"></div>
</div>

<!-- Вкладка 2: Уроки -->
<div id="Lessons" class="tabcontent">
    <h2>Задания</h2>
    
    <div class="form-group">
        <label for="lessonTitle">Название урока</label>
        <input type="text" id="lessonTitle" placeholder="Название урока">
    </div>
    
    <div class="form-group">
        <label for="lessonDesc">Описание задания</label>
        <textarea id="lessonDesc" placeholder="Описание задания"></textarea>
    </div>
    
    <div class="form-group">
        <label for="lessonDate">Срок выполнения</label>
        <input type="date" id="lessonDate">
    </div>
    
    <div class="form-group">
        <label for="lessonPriority">Приоритет</label>
        <select id="lessonPriority">
            <option value="High">Высокий</option>
            <option value="Medium" selected>Средний</option>
            <option value="Low">Низкий</option>
        </select>
    </div>

    <button onclick="addLesson()">Добавить задание</button>

    <div class="lesson-filters">
        <button onclick="filterLessons('All')" class="active">Все</button>
        <button onclick="filterLessons('High')">Высокий приоритет</button>
        <button onclick="filterLessons('Medium')">Средний приоритет</button>
        <button onclick="filterLessons('Low')">Низкий приоритет</button>
        <button onclick="filterLessons('Completed')">Выполненные</button>
        <button onclick="filterLessons('Pending')">Невыполненные</button>
    </div>

    <ul id="lessonList"></ul>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
// ====== Глобальные переменные ======
let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
let lessons = JSON.parse(localStorage.getItem('lessons')) || [];
let budget = parseFloat(localStorage.getItem('budget')) || 100000;
let currentFilter = 'All';

// ====== Инициализация при загрузке ======
document.addEventListener('DOMContentLoaded', function() {
    // Установка бюджета
    document.getElementById('budget').value = budget;
    
    // Установка минимальной даты как сегодняшней
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('lessonDate').min = today;
    
    // Обновление интерфейсов
    updateBudgetUI();
    updateLessonList();
});

// ====== Вкладки ======
function openTab(evt, tabName){
    document.querySelectorAll('.tabcontent').forEach(tc => tc.style.display='none');
    document.querySelectorAll('.tablinks').forEach(tb => tb.classList.remove('active'));
    document.getElementById(tabName).style.display='block';
    evt.currentTarget.classList.add('active');
    
    // Перерисовка графика при переключении на вкладку бюджета
    if (tabName === 'Budget') {
        updateChart();
    }
}

// ====== Бюджет трекер ======
function addExpense(){
    const desc = document.getElementById('description').value.trim();
    const amount = parseFloat(document.getElementById('amount').value);
    const cat = document.getElementById('category').value;
    
    // Валидация
    if(!desc) {
        showAlert('Введите описание расхода!', 'danger');
        document.getElementById('description').focus();
        return;
    }
    
    if(!amount || amount <= 0) {
        showAlert('Введите корректную сумму!', 'danger');
        document.getElementById('amount').focus();
        return;
    }
    
    // Автоматическая дата и время
    const dateTime = new Date().toISOString();
    
    expenses.push({description: desc, amount, category: cat, dateTime});
    saveData();
    
    // Очистка формы
    document.getElementById('description').value = '';
    document.getElementById('amount').value = '';
    document.getElementById('description').focus();
    
    updateBudgetUI();
    showAlert('Расход успешно добавлен!', 'success');
}

function updateBudgetUI(){
    const list = document.getElementById('expenseList'); 
    list.innerHTML = '';
    
    if (expenses.length === 0) {
        list.innerHTML = '<div class="empty-state"><i>📊</i><p>Пока нет расходов</p></div>';
        return;
    }
    
    let total = 0;

    // Сортировка по дате (новые сверху)
    expenses.sort((a,b) => new Date(b.dateTime) - new Date(a.dateTime));

    expenses.forEach((exp,i)=>{
        total += exp.amount;
        const li = document.createElement('li');
        const formattedDate = new Date(exp.dateTime).toLocaleString();
        li.innerHTML = `
            <span>${escapeHTML(exp.description)} - ${exp.amount.toFixed(2)} ₸</span>
            <span class="desc">Категория: ${exp.category}</span>
            <span class="desc">Дата и время: ${formattedDate}</span>
            <div class="buttons">
                <button onclick="editExpense(${i})" class="secondary">Редактировать</button>
                <button onclick="deleteExpense(${i})" class="danger">Удалить</button>
            </div>`;
        list.appendChild(li);
    });

    document.getElementById('total').textContent = total.toFixed(2) + ' ₸';
    
    budget = parseFloat(document.getElementById('budget').value) || 0;
    const remaining = budget - total;
    const remElem = document.getElementById('remaining');
    remElem.textContent = remaining.toFixed(2) + ' ₸';
    remElem.style.color = remaining < 0 ? 'var(--danger-color)' : 'var(--success-color)';
    
    // Обновление процента использования
    const usedPercentage = budget > 0 ? Math.min((total / budget) * 100, 100) : 0;
    document.getElementById('usedPercentage').textContent = usedPercentage.toFixed(1) + '%';
    
    // Обновление алерта
    const alertElem = document.getElementById('alert');
    if (remaining < 0) {
        alertElem.textContent = "Бюджет превышен!";
        alertElem.className = 'alert alert-danger';
        alertElem.style.display = 'block';
    } else if (usedPercentage > 80) {
        alertElem.textContent = "Бюджет почти исчерпан!";
        alertElem.className = 'alert alert-danger';
        alertElem.style.display = 'block';
    } else {
        alertElem.style.display = 'none';
    }

    updateBudgetBar();
    updateChart();
    saveData();
}

function deleteExpense(i){ 
    if (confirm('Вы уверены, что хотите удалить этот расход?')) {
        expenses.splice(i,1); 
        saveData(); 
        updateBudgetUI();
        showAlert('Расход удален!', 'success');
    }
}

function editExpense(i){
    const exp = expenses[i];
    const desc = prompt("Описание:", exp.description);
    if (desc === null) return; // Отмена
    
    const amountStr = prompt("Сумма:", exp.amount);
    if (amountStr === null) return; // Отмена
    
    const amount = parseFloat(amountStr);
    if (isNaN(amount) || amount <= 0) {
        showAlert('Некорректная сумма!', 'danger');
        return;
    }
    
    const cat = prompt("Категория:", exp.category);
    if (cat === null) return; // Отмена
    
    if(desc && amount && cat){
        // Дата и время сохраняем прежние
        expenses[i] = {description: desc, amount, category: cat, dateTime: exp.dateTime};
        saveData();
        updateBudgetUI();
        showAlert('Расход обновлен!', 'success');
    }
}

function updateBudgetBar(){
    const total = expenses.reduce((s,e) => s+e.amount,0);
    const percent = budget > 0 ? Math.min((total/budget)*100,100) : 0;
    const bar = document.getElementById('budgetBar'); 
    bar.style.width = percent+'%';
    bar.style.background = percent>100 ? 'var(--danger-color)' : 
                           percent>80 ? 'var(--warning-color)' : 'var(--success-color)';
    
    document.getElementById('progressText').textContent = percent.toFixed(1) + '%';
}

function updateChart(){
    const catTotals = {};
    expenses.forEach(e => catTotals[e.category] = (catTotals[e.category] || 0) + e.amount);
    
    const ctx = document.getElementById('chart').getContext('2d');
    if(window.myChart) window.myChart.destroy();
    
    if (Object.keys(catTotals).length === 0) {
        // Скрываем график, если нет данных
        document.getElementById('chart').style.display = 'none';
        document.getElementById('percentages').innerHTML = '<p>Нет данных для отображения</p>';
        return;
    }
    
    document.getElementById('chart').style.display = 'block';
    
    window.myChart = new Chart(ctx,{
        type:'pie',
        data:{
            labels:Object.keys(catTotals),
            datasets:[{
                data:Object.values(catTotals),
                backgroundColor:['#ff7675','#74b9ff','#ffeaa7','#55efc4','#a29bfe']
            }]
        },
        options: {
            responsive: true,
            plugins: {
                legend: {
                    position: 'bottom'
                }
            }
        }
    });

    const total = Object.values(catTotals).reduce((a,b) => a+b,0);
    let text = '';
    for(const cat in catTotals){ 
        text += `<span style="display: inline-block; margin-right: 15px;"><strong>${cat}</strong>: ${((catTotals[cat]/total)*100).toFixed(1)}%</span>`; 
    }
    document.getElementById('percentages').innerHTML = text;
}

// ====== Уроки и задания ======
function addLesson(){
    const title = document.getElementById('lessonTitle').value.trim();
    const desc = document.getElementById('lessonDesc').value.trim();
    const date = document.getElementById('lessonDate').value;
    const priority = document.getElementById('lessonPriority').value;
    
    if(!title || !desc || !date) {
        showAlert('Заполните все поля!', 'danger');
        return;
    }
    
    // Проверка даты
    const today = new Date().toISOString().split('T')[0];
    if (date < today) {
        showAlert('Дата выполнения не может быть в прошлом!', 'danger');
        return;
    }
    
    lessons.push({
        title, 
        desc, 
        date, 
        priority, 
        createdDate: new Date().toISOString().split('T')[0], 
        completed: false
    });
    
    saveLessons(); 
    updateLessonList();
    
    // Очистка формы
    document.getElementById('lessonTitle').value = '';
    document.getElementById('lessonDesc').value = '';
    document.getElementById('lessonDate').value = '';
    document.getElementById('lessonTitle').focus();
    
    showAlert('Задание добавлено!', 'success');
}

function updateLessonList(){
    const list = document.getElementById('lessonList'); 
    list.innerHTML = '';
    
    if (lessons.length === 0) {
        list.innerHTML = '<div class="empty-state"><i>📚</i><p>Пока нет заданий</p></div>';
        return;
    }
    
    const today = new Date();
    const priorityOrder = {High: 3, Medium: 2, Low: 1};

    // Фильтрация и сортировка
    let filteredLessons = lessons.filter(lesson => {
        if (currentFilter === 'All') return true;
        if (currentFilter === 'Completed') return lesson.completed;
        if (currentFilter === 'Pending') return !lesson.completed;
        return lesson.priority === currentFilter;
    });
    
    // Сортировка по приоритету и дате
    filteredLessons.sort((a, b) => {
        // Сначала по выполненности (невыполненные выше)
        if (a.completed !== b.completed) return a.completed ? 1 : -1;
        
        // Затем по приоритету
        if (priorityOrder[b.priority] !== priorityOrder[a.priority]) {
            return priorityOrder[b.priority] - priorityOrder[a.priority];
        }
        
        // Затем по дате (ближайшие выше)
        return new Date(a.date) - new Date(b.date);
    });

    filteredLessons.forEach((lesson, i) => {
        const lessonDate = new Date(lesson.date);
        const createdDate = new Date(lesson.createdDate);
        const totalTime = lessonDate - createdDate;
        const elapsed = today - createdDate;
        const progress = totalTime > 0 ? elapsed / totalTime : 1;
        
        let step = 0; 
        if (progress >= 0.9) step = 2; 
        else if (progress >= 0.6) step = 1;
        
        let dynamicIndex = Math.min(Object.keys(priorityOrder).indexOf(lesson.priority) + step, 2);
        const dynamicPriority = Object.keys(priorityOrder)[dynamicIndex];

        const li = document.createElement('li');
        li.className = lesson.completed ? 'completed' : '';
        
        // Добавляем класс приоритета
        if (!lesson.completed) {
            li.classList.add(`priority-${dynamicPriority.toLowerCase()}`);
        }
        
        // Добавляем класс срочности, если задание просрочено
        if (!lesson.completed && today > lessonDate) {
            li.classList.add('urgent');
        }

        li.innerHTML = `
            <span><strong>${escapeHTML(lesson.title)}</strong></span>
            <span class="desc">${escapeHTML(lesson.desc)}</span>
            <span class="desc">Срок: ${lesson.date} ${!lesson.completed && today > lessonDate ? '<span style="color:var(--danger-color);">(Просрочено)</span>' : ''}</span>
            <span class="desc">Приоритет: ${dynamicPriority}</span>
            <div class="buttons">
                <button onclick="toggleLesson(${lessons.indexOf(lesson)})" class="${lesson.completed ? 'secondary' : ''}">
                    ${lesson.completed ? 'Отметить как невыполненное' : 'Выполнено'}
                </button>
                <button onclick="deleteLesson(${lessons.indexOf(lesson)})" class="danger">Удалить</button>
            </div>`;
        list.appendChild(li);
    });
}

function toggleLesson(i){ 
    lessons[i].completed = !lessons[i].completed; 
    saveLessons(); 
    updateLessonList();
    showAlert(`Задание ${lessons[i].completed ? 'выполнено' : 'отмечено как невыполненное'}!`, 'success');
}

function deleteLesson(i){ 
    if (confirm('Вы уверены, что хотите удалить это задание?')) {
        lessons.splice(i,1); 
        saveLessons(); 
        updateLessonList();
        showAlert('Задание удалено!', 'success');
    }
}

function filterLessons(priority){
    currentFilter = priority;
    document.querySelectorAll('.lesson-filters button').forEach(b => b.classList.remove('active'));
    event.target.classList.add('active');
    updateLessonList();
}

// ====== Вспомогательные функции ======
function saveData(){ 
    localStorage.setItem('expenses', JSON.stringify(expenses)); 
    budget = parseFloat(document.getElementById('budget').value) || 100000; 
    localStorage.setItem('budget', budget);
}

function saveLessons(){ 
    localStorage.setItem('lessons', JSON.stringify(lessons)); 
}

function showAlert(message, type) {
    // Создаем временное уведомление
    const alert = document.createElement('div');
    alert.className = `alert alert-${type}`;
    alert.textContent = message;
    alert.style.position = 'fixed';
    alert.style.top = '20px';
    alert.style.right = '20px';
    alert.style.zIndex = '1000';
    alert.style.maxWidth = '300px';
    
    document.body.appendChild(alert);
    
    // Автоматическое скрытие через 3 секунды
    setTimeout(() => {
        alert.style.opacity = '0';
        alert.style.transition = 'opacity 0.5s';
        setTimeout(() => {
            document.body.removeChild(alert);
        }, 500);
    }, 3000);
}

function escapeHTML(unsafe) {
    return unsafe
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
}

// Автообновление приоритетов каждые 30 секунд
setInterval(updateLessonList, 30000);

// Обновление бюджета при изменении значения
document.getElementById('budget').addEventListener('change', updateBudgetUI);
</script>
</body>
</html>
