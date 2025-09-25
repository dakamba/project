<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Ultimate TaskMaster</title>
<style>
:root {
  --bg-color: #f5f5f5;
  --text-color: #000;
  --card-bg: #fff;
  --progress-color: #4caf50;
  --button-bg: #ddd;
  --button-hover: #ccc;
  --danger-color: #f44336;
  --warning-color: #ff9800;
}

body.dark {
  --bg-color: #1e1e1e;
  --text-color: #fff;
  --card-bg: #2e2e2e;
  --progress-color: #76ff03;
  --button-bg: #333;
  --button-hover: #444;
  --danger-color: #d32f2f;
  --warning-color: #f57c00;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background: var(--bg-color);
  color: var(--text-color);
  transition: 0.25s;
  line-height: 1.4;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  background: var(--card-bg);
  box-shadow: 0 2px 5px rgba(0,0,0,0.08);
  flex-wrap: wrap;
}

header h1 {
  font-size: 20px;
}

.header-controls {
  display: flex;
  gap: 10px;
  align-items: center;
}

.language-selector {
  background: var(--button-bg);
  border: none;
  padding: 6px 10px;
  border-radius: 6px;
  color: var(--text-color);
  cursor: pointer;
}

button {
  cursor: pointer;
  background: var(--button-bg);
  border: none;
  padding: 6px 10px;
  margin: 2px;
  border-radius: 6px;
  transition: 0.15s;
  font-size: 14px;
  color: var(--text-color);
}

button:hover {
  background: var(--button-hover);
}

button.danger {
  background: var(--danger-color);
  color: white;
}

button.danger:hover {
  background: #d32f2f;
}

.container {
  display: flex;
  height: calc(100vh - 60px);
  flex-direction: column;
}

@media (min-width: 768px) {
  .container {
    flex-direction: row;
  }
}

.plans-panel, .tasks-panel {
  padding: 12px;
  overflow-y: auto;
}

.plans-panel {
  width: 100%;
  border-bottom: 1px solid rgba(0,0,0,0.1);
}

@media (min-width: 768px) {
  .plans-panel {
    width: 26%;
    border-right: 1px solid rgba(0,0,0,0.1);
    border-bottom: none;
  }
}

.tasks-panel {
  width: 100%;
  flex: 1;
}

ul {
  list-style: none;
  margin-top: 10px;
  padding: 0;
}

.plans-panel li, .tasks-panel li {
  background: var(--card-bg);
  padding: 10px;
  margin-bottom: 8px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: 0.18s;
  gap: 8px;
  cursor: pointer;
}

.plans-panel li.active {
  border-left: 5px solid var(--progress-color);
}

.filters {
  margin-bottom: 10px;
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
}

.filters button.active {
  border: 2px solid var(--progress-color);
  padding: 4px 8px;
}

.progress-bar {
  margin-top: 10px;
  height: 16px;
  background: rgba(0,0,0,0.08);
  border-radius: 10px;
  overflow: hidden;
}

.progress-bar .progress {
  height: 100%;
  background: var(--progress-color);
  width: 0%;
  transition: 0.4s;
}

.modal {
  background: var(--card-bg);
  padding: 16px;
  border-radius: 10px;
  box-shadow: 0 6px 30px rgba(0,0,0,0.2);
  z-index: 1000;
  width: 320px;
  max-width: 95%;
  max-height: 90vh;
  overflow-y: auto;
}

#modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.45);
  display: none;
  z-index: 999;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.task-history {
  display: flex;
  margin-left: 10px;
}

.task-square {
  width: 18px;
  height: 18px;
  background: gray;
  margin-right: 4px;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.18s;
  flex: 0 0 auto;
}

.task-square.completed {
  background: green;
}

.task-square.missed {
  background: red;
}

.task-square.postponed {
  background: orange;
}

.task-timer {
  min-width: 36px;
  height: 36px;
  border-radius: 50%;
  border: 3px solid green;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  margin-left: 6px;
  padding: 2px;
}

.tasks-panel h2 {
  margin-bottom: 8px;
}

input[type="text"], input[type="number"], input[type="color"], select {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border-radius: 6px;
  border: 1px solid #bbb;
  background: transparent;
  color: inherit;
}

.floating-btn {
  position: fixed;
  bottom: 22px;
  right: 22px;
  border-radius: 50%;
  width: 56px;
  height: 56px;
  font-size: 26px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: var(--progress-color);
  color: #fff;
  border: none;
  cursor: pointer;
  box-shadow: 0 6px 20px rgba(0,0,0,0.18);
  z-index: 100;
}

.complete-btn {
  margin-left: 6px;
  padding: 6px 10px;
  border-radius: 6px;
}

.tasks-panel li.dragging {
  opacity: 0.5;
}

.task-color {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  margin-right: 8px;
  flex: 0 0 auto;
}

.left-group {
  display: flex;
  align-items: center;
  gap: 8px;
  flex: 1;
}

.meta-group {
  display: flex;
  align-items: center;
  gap: 8px;
}

.small-muted {
  font-size: 12px;
  color: rgba(0,0,0,0.6);
}

body.dark .small-muted {
  color: rgba(255,255,255,0.6);
}

.history-modal-content {
  max-height: 400px;
  overflow-y: auto;
}

.history-item {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  align-items: center;
}

body.dark .history-item {
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.history-status {
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 12px;
  color: white;
}

.history-status.completed {
  background: green;
}

.history-status.missed {
  background: red;
}

.history-status.postponed {
  background: orange;
}

.missed-badge {
  background: var(--danger-color);
  color: white;
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 12px;
  margin-left: 8px;
}

.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 12px 16px;
  border-radius: 6px;
  background: var(--warning-color);
  color: white;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  z-index: 1001;
  display: none;
  max-width: 300px;
}
</style>
</head>
<body>
<header>
  <h1 id="appTitle">Ultimate TaskMaster</h1>
  <div class="header-controls">
    <select id="languageSelect" class="language-selector">
      <option value="ru">Русский</option>
      <option value="kk">Қазақша</option>
      <option value="en">English</option>
    </select>
    <button id="themeToggle">🌙</button>
  </div>
</header>
<main class="container">
  <aside class="plans-panel">
    <h2 id="plansTitle">Планы</h2>
    <ul id="plansList"></ul>
    <button id="addPlanBtn">+ Добавить план</button>
  </aside>
  <section class="tasks-panel">
    <div class="tasks-header">
      <h2 id="currentPlanName">Выбрать план</h2>
      <div class="filters" id="filters">
        <button data-filter="today" class="active" id="filterToday">Сегодня</button>
        <button data-filter="completed" id="filterCompleted">Выполненные</button>
        <button data-filter="missed" id="filterMissed">Пропущенные</button>
        <button data-filter="postponed" id="filterPostponed">Отложенные</button>
        <button data-filter="all" id="filterAll">Все</button>
      </div>
    </div>
    <ul id="tasksList"></ul>
    <div class="progress-bar" title="Прогресс задач на сегодня">
      <div class="progress"></div>
    </div>
    <button id="addTaskBtn" class="floating-btn">+</button>
  </section>
</main>

<div id="modalOverlay">
  <div class="modal" id="modal">
    <h3 id="modalTitle"></h3>
    <input type="text" id="modalName" placeholder="Название">
    <div id="planFields">
      <input type="number" id="modalDuration" placeholder="Периодный план (дней)" min="1">
      <input type="color" id="modalColorPlan" value="#1976D2">
    </div>
    <div id="taskFields">
      <input type="number" id="modalDeadline" placeholder="Дедлайн (часы)" min="1">
      <input type="number" id="modalFrequency" placeholder="Каждый n-й день (частота)" min="1">
      <input type="color" id="modalColorTask" value="#1976D2">
      <input type="number" id="modalPriority" placeholder="Приоритет (число)" min="1">
    </div>
    <div style="display:flex;gap:8px;justify-content:flex-end;">
      <button id="modalSave">Сохранить</button>
      <button id="modalClose">Отмена</button>
    </div>
  </div>
</div>

<div id="historyModalOverlay" class="modal-overlay">
  <div class="modal">
    <h3 id="historyModalTitle">История задачи</h3>
    <div class="history-modal-content" id="historyContent"></div>
    <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:16px;">
      <button id="historyModalClose">Закрыть</button>
    </div>
  </div>
</div>

<div id="notification" class="notification"></div>

<script>
// ---------- Многоязычность ----------
const translations = {
  ru: {
    appTitle: "Ultimate TaskMaster",
    plansTitle: "Планы",
    selectPlan: "Выбрать план",
    addPlan: "+ Добавить план",
    filters: {
      today: "Сегодня",
      completed: "Выполненные",
      missed: "Пропущенные",
      postponed: "Отложенные",
      all: "Все"
    },
    modal: {
      addPlan: "Добавить план",
      addTask: "Добавить задачу",
      save: "Сохранить",
      cancel: "Отмена",
      namePlaceholder: "Название",
      durationPlaceholder: "Периодный план (дней)",
      deadlinePlaceholder: "Дедлайн (часы)",
      frequencyPlaceholder: "Каждый n-й день (частота)",
      priorityPlaceholder: "Приоритет (число)"
    },
    task: {
      deadline: "Дедлайн",
      frequency: "Частота",
      priority: "Приоритет",
      hours: "ч",
      days: "д",
      markComplete: "Отметить как выполненное (сегодня)",
      delete: "Удалить задачу",
      history: "История задачи",
      restore: "Восстановить"
    },
    plan: {
      delete: "Удалить план",
      confirmDelete: "Удалить план и все его задачи?"
    },
    history: {
      title: "История задачи",
      date: "Дата",
      status: "Статус",
      completed: "Выполнено",
      missed: "Пропущено",
      postponed: "Отложено",
      close: "Закрыть"
    },
    notification: {
      missedTasks: "У вас есть просроченные задачи!"
    }
  },
  kk: {
    appTitle: "Ultimate TaskMaster",
    plansTitle: "Жоспарлар",
    selectPlan: "Жоспарды таңдаңыз",
    addPlan: "+ Жоспар қосу",
    filters: {
      today: "Бүгін",
      completed: "Орындалған",
      missed: "Өткізілген",
      postponed: "Кейінге қалдырылған",
      all: "Барлығы"
    },
    modal: {
      addPlan: "Жоспар қосу",
      addTask: "Тапсырма қосу",
      save: "Сақтау",
      cancel: "Болдырмау",
      namePlaceholder: "Атауы",
      durationPlaceholder: "Мерзімді жоспар (күн)",
      deadlinePlaceholder: "Мерзімі (сағат)",
      frequencyPlaceholder: "Әрбір n-ші күн (жиілік)",
      priorityPlaceholder: "Басымдық (сан)"
    },
    task: {
      deadline: "Мерзімі",
      frequency: "Жиілік",
      priority: "Басымдық",
      hours: "с",
      days: "к",
      markComplete: "Орындалған деп белгілеу (бүгін)",
      delete: "Тапсырманы жою",
      history: "Тапсырма тарихы",
      restore: "Қалпына келтіру"
    },
    plan: {
      delete: "Жоспарды жою",
      confirmDelete: "Жоспарды және оның барлық тапсырмаларын жою керек пе?"
    },
    history: {
      title: "Тапсырма тарихы",
      date: "Күні",
      status: "Күйі",
      completed: "Орындалды",
      missed: "Өткізілді",
      postponed: "Кейінге қалдырылды",
      close: "Жабу"
    },
    notification: {
      missedTasks: "Сізде мерзімі өткен тапсырмалар бар!"
    }
  },
  en: {
    appTitle: "Ultimate TaskMaster",
    plansTitle: "Plans",
    selectPlan: "Select a plan",
    addPlan: "+ Add plan",
    filters: {
      today: "Today",
      completed: "Completed",
      missed: "Missed",
      postponed: "Postponed",
      all: "All"
    },
    modal: {
      addPlan: "Add plan",
      addTask: "Add task",
      save: "Save",
      cancel: "Cancel",
      namePlaceholder: "Name",
      durationPlaceholder: "Plan period (days)",
      deadlinePlaceholder: "Deadline (hours)",
      frequencyPlaceholder: "Every n-th day (frequency)",
      priorityPlaceholder: "Priority (number)"
    },
    task: {
      deadline: "Deadline",
      frequency: "Frequency",
      priority: "Priority",
      hours: "h",
      days: "d",
      markComplete: "Mark as completed (today)",
      delete: "Delete task",
      history: "Task history",
      restore: "Restore"
    },
    plan: {
      delete: "Delete plan",
      confirmDelete: "Delete plan and all its tasks?"
    },
    history: {
      title: "Task history",
      date: "Date",
      status: "Status",
      completed: "Completed",
      missed: "Missed",
      postponed: "Postponed",
      close: "Close"
    },
    notification: {
      missedTasks: "You have overdue tasks!"
    }
  }
};

let currentLanguage = 'ru';

function setLanguage(lang) {
  currentLanguage = lang;
  const t = translations[lang];
  
  // Обновляем тексты в интерфейсе
  document.getElementById('appTitle').textContent = t.appTitle;
  document.getElementById('plansTitle').textContent = t.plansTitle;
  document.getElementById('addPlanBtn').textContent = t.addPlan;
  document.getElementById('currentPlanName').textContent = t.selectPlan;
  
  // Фильтры
  document.getElementById('filterToday').textContent = t.filters.today;
  document.getElementById('filterCompleted').textContent = t.filters.completed;
  document.getElementById('filterMissed').textContent = t.filters.missed;
  document.getElementById('filterPostponed').textContent = t.filters.postponed;
  document.getElementById('filterAll').textContent = t.filters.all;
  
  // Модальные окна
  document.getElementById('modalSave').textContent = t.modal.save;
  document.getElementById('modalClose').textContent = t.modal.cancel;
  
  // Плейсхолдеры
  document.getElementById('modalName').placeholder = t.modal.namePlaceholder;
  document.getElementById('modalDuration').placeholder = t.modal.durationPlaceholder;
  document.getElementById('modalDeadline').placeholder = t.modal.deadlinePlaceholder;
  document.getElementById('modalFrequency').placeholder = t.modal.frequencyPlaceholder;
  document.getElementById('modalPriority').placeholder = t.modal.priorityPlaceholder;
  
  // Перерисовываем интерфейс
  renderPlans();
  renderTasks();
}

// ---------- Утилиты для работы с датами ----------
function todayISO() {
  return (new Date()).toISOString().slice(0, 10);
}

function addDaysISO(isoDate, days) {
  const d = new Date(isoDate + 'T00:00:00');
  d.setDate(d.getDate() + Number(days));
  return d.toISOString().slice(0, 10);
}

function cmpISO(a, b) {
  if (!a) return false;
  return a === b ? 0 : (a < b ? -1 : 1);
}

function formatDate(isoDate) {
  const date = new Date(isoDate + 'T00:00:00');
  return date.toLocaleDateString(currentLanguage);
}

// ---------- Хранилище и состояние ----------
let plans = JSON.parse(localStorage.getItem('plans')) || [];
let settings = JSON.parse(localStorage.getItem('settings')) || { theme: 'light', language: 'ru' };
let currentPlanId = null;
let currentFilter = 'today';

// Элементы DOM
const plansList = document.getElementById('plansList');
const tasksList = document.getElementById('tasksList');
const currentPlanName = document.getElementById('currentPlanName');
const modalOverlay = document.getElementById('modalOverlay');
const modal = document.getElementById('modal');
const modalTitle = document.getElementById('modalTitle');
const modalName = document.getElementById('modalName');
const modalDuration = document.getElementById('modalDuration');
const modalColorPlan = document.getElementById('modalColorPlan');
const modalDeadline = document.getElementById('modalDeadline');
const modalFrequency = document.getElementById('modalFrequency');
const modalColorTask = document.getElementById('modalColorTask');
const modalPriority = document.getElementById('modalPriority');
const modalSave = document.getElementById('modalSave');
const modalClose = document.getElementById('modalClose');
const themeToggle = document.getElementById('themeToggle');
const progressBar = document.querySelector('.progress-bar .progress');
const planFields = document.getElementById('planFields');
const taskFields = document.getElementById('taskFields');
const languageSelect = document.getElementById('languageSelect');
const historyModalOverlay = document.getElementById('historyModalOverlay');
const historyModalTitle = document.getElementById('historyModalTitle');
const historyContent = document.getElementById('historyContent');
const historyModalClose = document.getElementById('historyModalClose');
const notification = document.getElementById('notification');

let modalType = '';

// Применяем настройки
if (settings.theme === 'dark') document.body.classList.add('dark');
if (settings.language) {
  currentLanguage = settings.language;
  languageSelect.value = currentLanguage;
}
setLanguage(currentLanguage);

// События
themeToggle.addEventListener('click', () => {
  document.body.classList.toggle('dark');
  settings.theme = document.body.classList.contains('dark') ? 'dark' : 'light';
  localStorage.setItem('settings', JSON.stringify(settings));
});

languageSelect.addEventListener('change', (e) => {
  settings.language = e.target.value;
  localStorage.setItem('settings', JSON.stringify(settings));
  setLanguage(e.target.value);
});

// ---------- Инициализация данных ----------
function normalizeData() {
  const now = todayISO();
  plans.forEach(plan => {
    plan.tasks = plan.tasks || [];
    plan.tasks.forEach(task => {
      task.history = task.history || [];
      if (!task.created) task.created = Date.now();
      if (!task.nextRunDate) task.nextRunDate = now;
      task.frequency = Number(task.frequency) || 1;
      task.deadline = Number(task.deadline) || 1;
      task.priority = Number(task.priority) || 1;
      if (task.color === undefined) task.color = '#1976D2';
    });
  });
  localStorage.setItem('plans', JSON.stringify(plans));
}

normalizeData();

// ---------- Автоматическая пометка пропущенных задач ----------
function autoMarkMissed() {
  const today = todayISO();
  let hasMissed = false;
  
  plans.forEach(plan => {
    plan.tasks.forEach(task => {
      if (task.nextRunDate && cmpISO(task.nextRunDate, today) < 0) {
        const exist = task.history.some(h => h.date === task.nextRunDate);
        if (!exist) {
          task.history.push({ date: task.nextRunDate, missed: true });
          hasMissed = true;
          
          // Пересчитываем следующую дату для пропущенной задачи
          const freq = Math.max(1, Number(task.frequency) || 1);
          task.nextRunDate = addDaysISO(task.nextRunDate, freq);
        }
      }
    });
  });
  
  if (hasMissed) {
    showNotification(translations[currentLanguage].notification.missedTasks);
    saveAndRender();
  }
}

// Показываем уведомление
function showNotification(message) {
  notification.textContent = message;
  notification.style.display = 'block';
  setTimeout(() => {
    notification.style.display = 'none';
  }, 5000);
}

// ---------- Рендер планов ----------
function renderPlans() {
  plansList.innerHTML = '';
  const t = translations[currentLanguage];
  
  plans.forEach((plan, index) => {
    const li = document.createElement('li');
    if (currentPlanId === index) li.classList.add('active');
    
    const left = document.createElement('div');
    left.className = 'left-group';
    
    const color = document.createElement('span');
    color.className = 'task-color';
    color.style.background = plan.color || '#1976D2';
    left.appendChild(color);
    
    const name = document.createElement('span');
    name.textContent = plan.name;
    left.appendChild(name);
    
    li.appendChild(left);
    
    const right = document.createElement('div');
    const delBtn = document.createElement('button');
    delBtn.textContent = '🗑';
    delBtn.title = t.plan.delete;
    delBtn.addEventListener('click', (e) => {
      e.stopPropagation();
      if (!confirm(t.plan.confirmDelete)) return;
      plans.splice(index, 1);
      if (currentPlanId === index) currentPlanId = null;
      saveAndRender();
    });
    right.appendChild(delBtn);
    li.appendChild(right);
    
    li.addEventListener('click', () => {
      currentPlanId = index;
      renderPlans();
      renderTasks();
    });
    
    plansList.appendChild(li);
  });
}

// ---------- Рендер задач ----------
function renderTasks() {
  tasksList.innerHTML = '';
  const t = translations[currentLanguage];
  
  if (currentPlanId === null) {
    currentPlanName.textContent = t.selectPlan;
    progressBar.style.width = '0%';
    return;
  }
  
  const plan = plans[currentPlanId];
  currentPlanName.textContent = plan.name;
  const today = todayISO();
  
  // Определяем задачи для сегодня
  const allTasks = plan.tasks || [];
  const tasksDueToday = allTasks.filter(t => t.nextRunDate && cmpISO(t.nextRunDate, today) <= 0);
  
  // Фильтруем задачи
  let tasksToShow = allTasks.filter(task => {
    switch (currentFilter) {
      case 'today':
        if (!task.nextRunDate) return false;
        if (cmpISO(task.nextRunDate, today) > 0) return false;
        return !task.history.some(h => h.date === today && h.completed);
      case 'completed':
        return task.history.some(h => h.completed);
      case 'missed':
        return task.history.some(h => h.missed);
      case 'postponed':
        return task.history.some(h => h.postponed);
      case 'all':
        return true;
      default:
        return true;
    }
  });
  
  // Сортировка
  tasksToShow.sort((a, b) => {
    if (a.priority !== b.priority) return b.priority - a.priority;
    if (a.nextRunDate !== b.nextRunDate) return (a.nextRunDate || '') < (b.nextRunDate || '') ? -1 : 1;
    return a.name.localeCompare(b.name, currentLanguage);
  });
  
  // Создаем DOM
  tasksToShow.forEach((task, index) => {
    const li = document.createElement('li');
    li.draggable = true;
    
    const left = document.createElement('div');
    left.className = 'left-group';
    
    const color = document.createElement('span');
    color.className = 'task-color';
    color.style.background = task.color;
    left.appendChild(color);
    
    const title = document.createElement('div');
    title.innerHTML = `<strong>${task.name}</strong><div class="small-muted">${t.task.deadline}: ${task.deadline}${t.task.hours} · ${t.task.frequency}: ${task.frequency}${t.task.days} · ${t.task.priority}: ${task.priority}</div>`;
    left.appendChild(title);
    
    li.appendChild(left);
    
    const meta = document.createElement('div');
    meta.className = 'meta-group';
    
    // История задач
    const squaresDiv = document.createElement('div');
    squaresDiv.className = 'task-history';
    
    const freq = Math.max(1, Number(task.frequency) || 1);
    const recentDates = [];
    for (let i = 0; i < freq; i++) {
      recentDates.push(addDaysISO(today, -i));
    }
    
    recentDates.forEach(d => {
      const sq = document.createElement('div');
      sq.className = 'task-square';
      const rec = task.history.find(h => h.date === d);
      
      if (rec?.completed) sq.classList.add('completed');
      if (rec?.missed) sq.classList.add('missed');
      if (rec?.postponed) sq.classList.add('postponed');
      
      sq.title = formatDate(d);
      
      sq.addEventListener('click', () => {
        const found = task.history.find(h => h.date === d);
        if (found) {
          found.completed = !found.completed;
          found.missed = false;
          found.postponed = false;
        } else {
          task.history.push({ date: d, completed: true });
        }
        
        if (d === today && task.history.find(h => h.date === today)?.completed) {
          onTaskCompleted(task, today);
        }
        saveAndRender();
      });
      
      squaresDiv.appendChild(sq);
    });
    
    meta.appendChild(squaresDiv);
    
    // Кнопка истории
    const historyBtn = document.createElement('button');
    historyBtn.textContent = '📊';
    historyBtn.title = t.task.history;
    historyBtn.addEventListener('click', () => {
      showTaskHistory(task);
    });
    meta.appendChild(historyBtn);
    
    // Таймер
    const timerDiv = document.createElement('div');
    timerDiv.className = 'task-timer';
    const hoursLeft = Math.max(0, task.deadline - Math.floor((Date.now() - task.created) / 3600000));
    timerDiv.textContent = Math.ceil(hoursLeft) + t.task.hours;
    
    if (hoursLeft > task.deadline * 0.66) timerDiv.style.borderColor = 'green';
    else if (hoursLeft > task.deadline * 0.33) timerDiv.style.borderColor = 'orange';
    else timerDiv.style.borderColor = 'red';
    
    meta.appendChild(timerDiv);
    
    // Кнопка выполнения/восстановления
    const todayRecord = task.history.find(h => h.date === today);
    const actionBtn = document.createElement('button');
    
    if (todayRecord?.completed) {
      actionBtn.textContent = '↶';
      actionBtn.title = t.task.restore;
      actionBtn.addEventListener('click', () => {
        task.history = task.history.filter(h => h.date !== today);
        saveAndRender();
      });
    } else {
      actionBtn.textContent = '✓';
      actionBtn.title = t.task.markComplete;
      actionBtn.addEventListener('click', () => {
        task.history.push({ date: today, completed: true });
        onTaskCompleted(task, today);
        saveAndRender();
      });
    }
    
    actionBtn.className = 'complete-btn';
    meta.appendChild(actionBtn);
    
    // Кнопка удаления
    const delBtn = document.createElement('button');
    delBtn.textContent = '🗑';
    delBtn.title = t.task.delete;
    delBtn.addEventListener('click', () => {
      const taskIndex = plan.tasks.indexOf(task);
      if (taskIndex >= 0) {
        plan.tasks.splice(taskIndex, 1);
        saveAndRender();
      }
    });
    meta.appendChild(delBtn);
    
    li.appendChild(meta);
    tasksList.appendChild(li);
  });
  
  // Прогресс
  const due = tasksDueToday.length;
  const doneToday = tasksDueToday.filter(t => t.history.some(h => h.date === today && h.completed)).length;
  const percent = due === 0 ? 0 : Math.round((doneToday / due) * 100);
  progressBar.style.width = percent + '%';
  progressBar.style.background = percent > 70 ? 'green' : percent > 30 ? 'orange' : 'red';
}

// ---------- Показ истории задачи ----------
function showTaskHistory(task) {
  const t = translations[currentLanguage];
  historyModalTitle.textContent = t.history.title + ': ' + task.name;
  historyContent.innerHTML = '';
  
  // Сортируем историю по дате (новые сверху)
  const sortedHistory = [...task.history].sort((a, b) => cmpISO(b.date, a.date));
  
  if (sortedHistory.length === 0) {
    historyContent.innerHTML = '<p>История отсутствует</p>';
  } else {
    sortedHistory.forEach(record => {
      const item = document.createElement('div');
      item.className = 'history-item';
      
      const date = document.createElement('span');
      date.textContent = formatDate(record.date);
      
      const status = document.createElement('span');
      status.className = 'history-status';
      
      if (record.completed) {
        status.textContent = t.history.completed;
        status.classList.add('completed');
      } else if (record.missed) {
        status.textContent = t.history.missed;
        status.classList.add('missed');
      } else if (record.postponed) {
        status.textContent = t.history.postponed;
        status.classList.add('postponed');
      }
      
      item.appendChild(date);
      item.appendChild(status);
      historyContent.appendChild(item);
    });
  }
  
  historyModalOverlay.style.display = 'flex';
}

// ---------- Логика при выполнении задачи ----------
function onTaskCompleted(task, dateISO) {
  const freq = Math.max(1, Number(task.frequency) || 1);
  if (freq > 0) {
    task.nextRunDate = addDaysISO(dateISO, freq);
    task.completed = false;
  } else {
    task.completed = true;
    task.nextRunDate = null;
  }
}

// ---------- Сохранение и рендер ----------
function saveAndRender() {
  localStorage.setItem('plans', JSON.stringify(plans));
  renderPlans();
  renderTasks();
}

// ---------- Модальные окна ----------
document.getElementById('addPlanBtn').addEventListener('click', () => {
  modalType = 'plan';
  const t = translations[currentLanguage];
  modalTitle.textContent = t.modal.addPlan;
  modalName.value = '';
  modalDuration.value = 30;
  modalColorPlan.value = '#1976D2';
  planFields.style.display = 'block';
  taskFields.style.display = 'none';
    modalOverlay.style.display = 'flex';
});

document.getElementById('addTaskBtn').addEventListener('click', () => {
  if (currentPlanId === null) {
    alert(translations[currentLanguage].selectPlan);
    return;
  }
  
  modalType = 'task';
  const t = translations[currentLanguage];
  modalTitle.textContent = t.modal.addTask;
  modalName.value = '';
  modalDeadline.value = 24;
  modalFrequency.value = 1;
  modalColorTask.value = '#1976D2';
  modalPriority.value = 1;
  planFields.style.display = 'none';
  taskFields.style.display = 'block';
  modalOverlay.style.display = 'flex';
});

modalSave.addEventListener('click', () => {
  const name = modalName.value.trim();
  if (!name) return;
  
  if (modalType === 'plan') {
    const duration = parseInt(modalDuration.value) || 30;
    const color = modalColorPlan.value;
    
    plans.push({
      name,
      duration,
      color,
      tasks: []
    });
  } else if (modalType === 'task' && currentPlanId !== null) {
    const deadline = parseInt(modalDeadline.value) || 24;
    const frequency = parseInt(modalFrequency.value) || 1;
    const color = modalColorTask.value;
    const priority = parseInt(modalPriority.value) || 1;
    
    plans[currentPlanId].tasks.push({
      name,
      deadline,
      frequency,
      color,
      priority,
      history: [],
      created: Date.now(),
      nextRunDate: todayISO(),
      completed: false
    });
  }
  
  modalOverlay.style.display = 'none';
  saveAndRender();
});

modalClose.addEventListener('click', () => {
  modalOverlay.style.display = 'none';
});

historyModalClose.addEventListener('click', () => {
  historyModalOverlay.style.display = 'none';
});

// Обработчики фильтров
document.querySelectorAll('.filters button').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.filters button').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentFilter = btn.dataset.filter;
    renderTasks();
  });
});

// Закрытие модальных окон по клику вне области
modalOverlay.addEventListener('click', (e) => {
  if (e.target === modalOverlay) {
    modalOverlay.style.display = 'none';
  }
});

historyModalOverlay.addEventListener('click', (e) => {
  if (e.target === historyModalOverlay) {
    historyModalOverlay.style.display = 'none';
  }
});

// Drag and Drop для переупорядочивания задач
let dragStartIndex = null;

tasksList.addEventListener('dragstart', (e) => {
  if (e.target.tagName === 'LI') {
    dragStartIndex = Array.from(tasksList.children).indexOf(e.target);
    e.target.classList.add('dragging');
  }
});

tasksList.addEventListener('dragover', (e) => {
  e.preventDefault();
});

tasksList.addEventListener('drop', (e) => {
  e.preventDefault();
  const draggingElement = document.querySelector('.dragging');
  if (draggingElement && dragStartIndex !== null && currentPlanId !== null) {
    const dropIndex = Array.from(tasksList.children).indexOf(e.target.closest('li'));
    if (dropIndex >= 0 && dragStartIndex !== dropIndex) {
      // Перемещаем задачу в массиве
      const plan = plans[currentPlanId];
      const [movedTask] = plan.tasks.splice(dragStartIndex, 1);
      plan.tasks.splice(dropIndex, 0, movedTask);
      saveAndRender();
    }
    draggingElement.classList.remove('dragging');
  }
});

tasksList.addEventListener('dragend', (e) => {
  e.target.classList.remove('dragging');
  dragStartIndex = null;
});

// Функция для отложенной задачи
function postponeTask(task, days = 1) {
  const today = todayISO();
  const todayRecord = task.history.find(h => h.date === today);
  
  if (!todayRecord) {
    task.history.push({ date: today, postponed: true });
  } else {
    todayRecord.postponed = true;
    todayRecord.completed = false;
    todayRecord.missed = false;
  }
  
  task.nextRunDate = addDaysISO(today, days);
  saveAndRender();
}

// Добавляем кнопку отложить в интерфейс задач
function addPostponeButton(task, container) {
  const t = translations[currentLanguage];
  const postponeBtn = document.createElement('button');
  postponeBtn.textContent = '⏰';
  postponeBtn.title = 'Отложить на завтра';
  postponeBtn.addEventListener('click', () => {
    postponeTask(task, 1);
  });
  container.appendChild(postponeBtn);
}

// Обновляем renderTasks чтобы добавить кнопку отложить
// Вставляем этот код в секцию создания кнопок (после historyBtn и перед timerDiv)
function updateRenderTasksWithPostpone() {
  // ... существующий код renderTasks ...
  
  // В цикле создания задач, после historyBtn добавляем:
  const postponeBtn = document.createElement('button');
  postponeBtn.textContent = '⏰';
  postponeBtn.title = 'Отложить на завтра';
  postponeBtn.addEventListener('click', (e) => {
    e.stopPropagation();
    postponeTask(task, 1);
  });
  meta.appendChild(postponeBtn);
  
  // ... остальной код ...
}

// Статистика и аналитика
function getPlanStatistics(planIndex) {
  const plan = plans[planIndex];
  const today = todayISO();
  const totalTasks = plan.tasks.length;
  const completedToday = plan.tasks.filter(task => 
    task.history.some(h => h.date === today && h.completed)
  ).length;
  const missedTasks = plan.tasks.filter(task => 
    task.history.some(h => h.missed)
  ).length;
  const postponedTasks = plan.tasks.filter(task => 
    task.history.some(h => h.postponed)
  ).length;
  
  return {
    totalTasks,
    completedToday,
    missedTasks,
    postponedTasks,
    completionRate: totalTasks ? Math.round((completedToday / totalTasks) * 100) : 0
  };
}

// Уведомления для срочных задач
function checkUrgentTasks() {
  const today = todayISO();
  let urgentTasks = [];
  
  plans.forEach((plan, planIndex) => {
    plan.tasks.forEach(task => {
      if (task.nextRunDate === today && !task.history.some(h => h.date === today && h.completed)) {
        const hoursLeft = Math.max(0, task.deadline - Math.floor((Date.now() - task.created) / 3600000));
        if (hoursLeft < 3) { // Менее 3 часов осталось
          urgentTasks.push({
            plan: plan.name,
            task: task.name,
            hoursLeft: Math.ceil(hoursLeft)
          });
        }
      }
    });
  });
  
  if (urgentTasks.length > 0) {
    const t = translations[currentLanguage];
    showNotification(`${t.task.deadline}! ${urgentTasks.length} ${t.task.hours}`);
  }
}

// Автоматическое сохранение при закрытии вкладки
window.addEventListener('beforeunload', () => {
  localStorage.setItem('plans', JSON.stringify(plans));
  localStorage.setItem('settings', JSON.stringify(settings));
});

// Горячие клавиши
document.addEventListener('keydown', (e) => {
  if (e.ctrlKey || e.metaKey) {
    switch(e.key) {
      case 'n':
        e.preventDefault();
        if (currentPlanId === null) {
          document.getElementById('addPlanBtn').click();
        } else {
          document.getElementById('addTaskBtn').click();
        }
        break;
      case 'd':
        e.preventDefault();
        document.body.classList.toggle('dark');
        settings.theme = document.body.classList.contains('dark') ? 'dark' : 'light';
        break;
    }
  }
  
  if (e.key === 'Escape') {
    modalOverlay.style.display = 'none';
    historyModalOverlay.style.display = 'none';
  }
});

// Инициализация при загрузке
function init() {
  // Проверяем, есть ли данные в localStorage
  if (plans.length === 0) {
    // Создаем демо-данные для нового пользователя
    plans = [
      {
        name: translations[currentLanguage].plansTitle + " 1",
        duration: 30,
        color: '#1976D2',
        tasks: [
          {
            name: translations[currentLanguage].modal.namePlaceholder + " 1",
            deadline: 24,
            frequency: 1,
            color: '#4CAF50',
            priority: 1,
            history: [],
            created: Date.now(),
            nextRunDate: todayISO(),
            completed: false
          }
        ]
      }
    ];
  }
  
  autoMarkMissed();
  renderPlans();
  renderTasks();
  checkUrgentTasks();
  
  // Проверяем срочные задачи каждые 30 минут
  setInterval(checkUrgentTasks, 30 * 60 * 1000);
  
  // Проверяем пропущенные задачи каждый день
  setInterval(autoMarkMissed, 24 * 60 * 60 * 1000);
}

// Запускаем приложение
init();

// Экспортируем функции для глобального доступа (для отладки)
window.taskManager = {
  plans,
  settings,
  currentPlanId,
  currentFilter,
  getPlanStatistics,
  postponeTask,
  addDaysISO,
  todayISO,
  normalizeData,
  saveAndRender
};

console.log('Ultimate TaskMaster инициализирован! Используйте window.taskManager для отладки.');
</script>
</body>
</html>
