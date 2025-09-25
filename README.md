<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Productivity Master</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
    :root {
        --primary: #4361ee;
        --secondary: #3f37c9;
        --success: #4cc9f0;
        --warning: #f72585;
        --danger: #e63946;
        --light: #f8f9fa;
        --dark: #212529;
        --gray: #6c757d;
        --gradient-primary: linear-gradient(135deg, #4361ee, #3a0ca3);
        --gradient-success: linear-gradient(135deg, #4cc9f0, #4895ef);
        --gradient-warning: linear-gradient(135deg, #f72585, #b5179e);
        --texture: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" opacity="0.05"><rect width="100" height="100" fill="none" stroke="currentColor" stroke-width="2"/></svg>');
        --shadow-sm: 0 2px 8px rgba(0,0,0,0.1);
        --shadow-md: 0 4px 16px rgba(0,0,0,0.15);
        --shadow-lg: 0 8px 32px rgba(0,0,0,0.2);
        --radius: 12px;
        --radius-lg: 20px;
        --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .dark-theme {
        --light: #121212;
        --dark: #f8f9fa;
        --gray: #a0a0a0;
        --shadow-sm: 0 2px 8px rgba(0,0,0,0.3);
        --shadow-md: 0 4px 16px rgba(0,0,0,0.4);
        --shadow-lg: 0 8px 32px rgba(0,0,0,0.5);
    }

    * { margin:0; padding:0; box-sizing:border-box; }
    body {
        font-family: 'Segoe UI', sans-serif;
        background: var(--light);
        color: var(--dark);
        min-height:100vh;
        line-height:1.6;
        background-image: var(--texture);
        transition: var(--transition);
    }

    .app-container { 
        display: grid; 
        grid-template-columns: 280px 1fr; 
        grid-template-rows: 80px 1fr; 
        grid-template-areas: "sidebar header" "sidebar main"; 
        min-height:100vh; 
    }
    
    .header { 
        grid-area: header; 
        background: var(--gradient-primary); 
        color:white; 
        padding:1rem 2rem; 
        display:flex; 
        justify-content:space-between; 
        align-items:center; 
        box-shadow: var(--shadow-md); 
        position:relative; 
        z-index:100; 
    }
    
    .header h1 { 
        font-size:1.8rem; 
        font-weight:700; 
    }
    
    .controls { 
        display:flex; 
        gap:1rem; 
        align-items:center; 
    }

    .sidebar { 
        grid-area: sidebar; 
        background: var(--dark); 
        color: var(--light); 
        padding:2rem 1rem; 
        box-shadow: var(--shadow-lg); 
        z-index:200; 
        overflow-y:auto; 
        transition:var(--transition);
    }
    
    .nav-section { 
        margin-bottom:2rem; 
    }
    
    .nav-title { 
        font-size:0.9rem; 
        text-transform:uppercase; 
        letter-spacing:1px; 
        margin-bottom:1rem; 
        color: var(--gray); 
        font-weight:600; 
    }
    
    .nav-item { 
        display:flex; 
        align-items:center; 
        gap:0.75rem; 
        padding:0.75rem 1rem; 
        border-radius:var(--radius); 
        margin-bottom:0.5rem; 
        cursor:pointer; 
        transition:var(--transition); 
        font-weight:500; 
    }
    
    .nav-item:hover, .nav-item.active { 
        background: rgba(255,255,255,0.1); 
        transform: translateX(4px); 
    }
    
    .nav-item i { 
        width:20px; 
        text-align:center; 
    }

    .main-content { 
        grid-area: main; 
        padding:2rem; 
        overflow-y:auto; 
        background: var(--light); 
        transition: var(--transition);
    }
    
    .tab-content { 
        display:none; 
        animation: fadeIn 0.5s ease-in; 
    }
    
    .tab-content.active { 
        display:block; 
    }
    
    @keyframes fadeIn { 
        from { 
            opacity:0; 
            transform:translateY(20px); 
        } 
        to { 
            opacity:1; 
            transform:translateY(0);
        } 
    }

    .overview-grid { 
        display:grid; 
        grid-template-columns:repeat(auto-fit,minmax(300px,1fr)); 
        gap:1.5rem; 
        margin-bottom:2rem;
    }
    
    .stat-card { 
        background:white; 
        padding:1.5rem; 
        border-radius: var(--radius-lg); 
        box-shadow: var(--shadow-sm); 
        border-left: 4px solid var(--primary); 
        transition:var(--transition);
    }
    
    .stat-card:hover { 
        transform:translateY(-5px); 
        box-shadow: var(--shadow-md);
    }
    
    .stat-card.warning { 
        border-left-color: var(--warning); 
    }
    
    .stat-card.success { 
        border-left-color: var(--success); 
    }
    
    .stat-card.danger { 
        border-left-color: var(--danger); 
    }

    .task-grid { 
        display:grid; 
        grid-template-columns:repeat(auto-fill,minmax(350px,1fr)); 
        gap:1.5rem; 
    }
    
    .task-card { 
        background:white; 
        border-radius:var(--radius); 
        padding:1.5rem; 
        box-shadow: var(--shadow-sm); 
        border:2px solid transparent; 
        transition:var(--transition); 
        position:relative; 
        overflow:hidden;
    }
    
    .task-card:hover { 
        box-shadow: var(--shadow-md); 
        border-color: var(--primary); 
    }
    
    .task-card::before { 
        content:''; 
        position:absolute; 
        top:0; 
        left:0; 
        width:100%; 
        height:4px; 
        background: var(--gradient-primary); 
    }
    
    .task-card.high-priority::before { 
        background: var(--gradient-warning); 
    }
    
    .task-card.medium-priority::before { 
        background: var(--gradient-primary); 
    }
    
    .task-card.low-priority::before { 
        background: var(--gradient-success); 
    }

    .form-group { 
        margin-bottom:1rem; 
    }
    
    .form-input { 
        width:100%; 
        padding:0.75rem 1rem; 
        border:2px solid #e9ecef; 
        border-radius:var(--radius); 
        font-size:1rem; 
        transition:var(--transition);
    }
    
    .form-input:focus { 
        outline:none; 
        border-color: var(--primary); 
        box-shadow:0 0 0 3px rgba(67,97,238,0.1);
    }
    
    .btn { 
        padding:0.75rem 1.5rem; 
        border:none; 
        border-radius:var(--radius); 
        font-size:1rem; 
        font-weight:600; 
        cursor:pointer; 
        transition:var(--transition); 
        display:inline-flex; 
        align-items:center; 
        gap:0.5rem; 
    }
    
    .btn-primary { 
        background: var(--gradient-primary); 
        color:white; 
    }
    
    .btn-primary:hover { 
        transform:translateY(-2px); 
        box-shadow: var(--shadow-md); 
    }
    
    .btn-danger { 
        background: var(--danger); 
        color:white; 
    }
    
    .btn-success { 
        background: var(--success); 
        color:white; 
    }

    .modal-overlay { 
        position:fixed; 
        top:0; 
        left:0; 
        width:100%; 
        height:100%; 
        background:rgba(0,0,0,0.5); 
        display:flex; 
        justify-content:center; 
        align-items:center; 
        z-index:1000; 
        backdrop-filter: blur(5px); 
        display:none; 
    }
    
    .modal-content { 
        background:white; 
        border-radius:var(--radius-lg); 
        padding:2rem; 
        max-width:500px; 
        width:90%; 
        max-height:90vh; 
        overflow-y:auto; 
        box-shadow: var(--shadow-lg); 
        animation: modalSlideIn 0.3s ease-out; 
    }
    
    @keyframes modalSlideIn { 
        from { 
            transform:scale(0.9); 
            opacity:0;
        } 
        to { 
            transform:scale(1); 
            opacity:1;
        } 
    }

    .progress-ring {
        width: 100px;
        height: 100px;
        margin: 0 auto 1rem;
    }
    
    .progress-ring svg {
        width: 100%;
        height: 100%;
    }
    
    .progress-bar {
        height: 10px;
        background: #e9ecef;
        border-radius: 5px;
        overflow: hidden;
        margin: 1rem 0;
    }
    
    .progress-fill {
        height: 100%;
        background: var(--gradient-primary);
        transition: width 0.5s ease;
    }
    
    .chart-container {
        background: white;
        border-radius: var(--radius);
        padding: 1.5rem;
        margin-bottom: 2rem;
        box-shadow: var(--shadow-sm);
    }
    
    .dark-theme .stat-card, 
    .dark-theme .task-card, 
    .dark-theme .modal-content,
    .dark-theme .chart-container { 
        background:#1e1e1e; 
        color: var(--light); 
    }
    
    .dark-theme .form-input { 
        background:#2d2d2d; 
        border-color:#404040; 
        color: var(--light); 
    }
    
    .d-flex { display: flex; }
    .align-center { align-items: center; }
    .justify-between { justify-content: space-between; }
    .justify-center { justify-content: center; }
    .gap-2 { gap: 1rem; }
    .mb-3 { margin-bottom: 1.5rem; }
    .mt-3 { margin-top: 1.5rem; }
    .text-large { font-size: 2rem; font-weight: bold; }

    @media (max-width:1024px) { 
        .app-container { 
            grid-template-columns:1fr; 
            grid-template-areas:"header" "main"; 
        } 
        .sidebar { 
            position:fixed; 
            left:-280px; 
            top:80px; 
            height:calc(100vh - 80px); 
            transition: var(--transition); 
            z-index:300;
        } 
        .sidebar.open { 
            left:0; 
        } 
    }
    
    @media (max-width:768px) { 
        .task-grid, .overview-grid { 
            grid-template-columns:1fr;
        } 
        .header { 
            padding:1rem;
        } 
        .main-content { 
            padding:1rem;
        } 
    }
</style>
</head>
<body>
<div class="app-container">
    <header class="header">
        <div class="d-flex align-center gap-2">
            <button class="btn menu-toggle" style="background: rgba(255,255,255,0.1); color: white;">
                <i>☰</i>
            </button>
            <h1 id="appTitle">Productivity Master</h1>
        </div>
        <div class="controls">
            <select id="languageSelect" class="form-input" style="width:auto; background: rgba(255,255,255,0.1); color:white; border:none;">
                <option value="ru">Русский</option>
                <option value="kz">Қазақша</option>
                <option value="en">English</option>
            </select>
            <button class="btn theme-toggle" style="background: rgba(255,255,255,0.1); color:white;">
                <i id="themeIcon">🌙</i>
            </button>
        </div>
    </header>

    <aside class="sidebar">
        <div class="nav-section">
            <div class="nav-title" id="navMain">Основное</div>
            <div class="nav-item active" data-tab="dashboard"><i>📊</i> <span id="navDashboard">Дашборд</span></div>
            <div class="nav-item" data-tab="tasks"><i>✅</i> <span id="navTasks">Задачи и планы</span></div>
            <div class="nav-item" data-tab="budget"><i>💰</i> <span id="navBudget">Бюджет трекер</span></div>
        </div>
        <div class="nav-section">
            <div class="nav-title" id="navAnalytics">Аналитика</div>
            <div class="nav-item" data-tab="stats"><i>📈</i> <span id="navStats">Статистика</span></div>
            <div class="nav-item" data-tab="goals"><i>🎯</i> <span id="navGoals">Цели</span></div>
        </div>
        <div class="nav-section">
            <div class="nav-title" id="navSettings">Настройки</div>
            <div class="nav-item" data-tab="settings"><i>⚙️</i> <span id="navSettingsItem">Настройки</span></div>
        </div>
    </aside>

    <main class="main-content">
        <!-- Dashboard Tab -->
        <div class="tab-content active" id="dashboard">
            <h2 class="mb-3" id="dashboardTitle">Обзор продуктивности</h2>
            <div class="overview-grid">
                <div class="stat-card">
                    <h3 id="todayProgress">Сегодняшний прогресс</h3>
                    <div class="progress-ring">
                        <svg viewBox="0 0 36 36">
                            <circle cx="18" cy="18" r="15.9" stroke="#e9ecef" stroke-width="2" fill="none"/>
                            <circle id="progressCircle" cx="18" cy="18" r="15.9" stroke="#4361ee" stroke-width="2" fill="none" stroke-dasharray="100, 100" stroke-dashoffset="75"/>
                        </svg>
                    </div>
                    <p id="progressText">25% задач выполнено</p>
                </div>
                <div class="stat-card warning">
                    <h3 id="urgentTasks">Срочные задачи</h3>
                    <div class="d-flex align-center justify-between"><span class="text-large" id="urgentCount">3</span><i>⚠️</i></div>
                    <p id="urgentText">Требуют внимания сегодня</p>
                </div>
                <div class="stat-card success">
                    <h3 id="monthBudget">Бюджет месяца</h3>
                    <div class="progress-bar"><div class="progress-fill" id="budgetProgress" style="width: 65%"></div></div>
                    <p id="budgetText">Использовано 65% от 50,000 ₸</p>
                </div>
            </div>
            <h3 class="mb-2" id="recentTasks">Последние задачи</h3>
            <div class="task-grid" id="dashboardTasks"></div>
        </div>

        <!-- Tasks Tab -->
        <div class="tab-content" id="tasks">
            <div class="d-flex justify-between align-center mb-3">
                <h2 id="tasksTitle">Управление задачами</h2>
                <button class="btn btn-primary" id="addTaskBtn"><i>+</i> <span id="newTaskBtn">Новая задача</span></button>
            </div>
            <div class="task-grid" id="taskList"></div>
        </div>

        <!-- Budget Tab -->
        <div class="tab-content" id="budget">
            <div class="d-flex justify-between align-center mb-3">
                <h2 id="budgetTitle">Трекер бюджета</h2>
                <button class="btn btn-primary" id="addExpenseBtn"><i>+</i> <span id="addExpenseText">Добавить расход</span></button>
            </div>
            <div class="overview-grid mb-3">
                <div class="stat-card">
                    <h3 id="totalBudget">Общий бюджет</h3>
                    <div class="text-large" id="totalBudgetAmount">50,000 ₸</div>
                    <p id="budgetPeriod">Сентябрь 2023</p>
                </div>
                <div class="stat-card success">
                    <h3 id="remainingBudget">Остаток</h3>
                    <div class="text-large" id="remainingAmount">17,500 ₸</div>
                    <p id="remainingText">35% от бюджета</p>
                </div>
                <div class="stat-card danger">
                    <h3 id="totalExpenses">Расходы</h3>
                    <div class="text-large" id="expensesAmount">32,500 ₸</div>
                    <p id="expensesText">65% от бюджета</p>
                </div>
            </div>
            <div class="chart-container">
                <h3 id="expensesChartTitle">Расходы по категориям</h3>
                <canvas id="expensesChart"></canvas>
            </div>
            <div id="expensesList"></div>
        </div>

        <!-- Statistics Tab -->
        <div class="tab-content" id="stats">
            <h2 class="mb-3" id="statsTitle">Статистика продуктивности</h2>
            <div class="chart-container">
                <h3 id="taskCompletionTitle">Выполнение задач по дням</h3>
                <canvas id="taskCompletionChart"></canvas>
            </div>
            <div class="chart-container">
                <h3 id="productivityTrendTitle">Тренд продуктивности</h3>
                <canvas id="productivityChart"></canvas>
            </div>
        </div>

        <!-- Goals Tab -->
        <div class="tab-content" id="goals">
            <div class="d-flex justify-between align-center mb-3">
                <h2 id="goalsTitle">Мои цели</h2>
                <button class="btn btn-primary" id="addGoalBtn"><i>+</i> <span id="newGoalBtn">Новая цель</span></button>
            </div>
            <div class="task-grid" id="goalsList"></div>
        </div>

        <!-- Settings Tab -->
        <div class="tab-content" id="settings">
            <h2 class="mb-3" id="settingsTitle">Настройки</h2>
            <div class="stat-card">
                <h3 id="appSettings">Настройки приложения</h3>
                <div class="form-group">
                    <label for="budgetLimit" id="budgetLimitLabel">Лимит бюджета на месяц (₸)</label>
                    <input type="number" class="form-input" id="budgetLimit" value="50000">
                </div>
                <div class="form-group">
                    <label for="notifications" id="notificationsLabel">Уведомления</label>
                    <select class="form-input" id="notifications">
                        <option value="enabled">Включены</option>
                        <option value="disabled">Выключены</option>
                    </select>
                </div>
                <button class="btn btn-primary" id="saveSettingsBtn"><span id="saveSettingsText">Сохранить настройки</span></button>
            </div>
        </div>
    </main>
</div>

<!-- Модальные окна -->
<div class="modal-overlay" id="taskModal">
    <div class="modal-content">
        <h3 id="taskModalTitle">Новая задача</h3>
        <form id="taskForm">
            <div class="form-group">
                <input type="text" class="form-input" id="taskTitle" placeholder="Название задачи" required>
            </div>
            <div class="form-group">
                <textarea class="form-input" id="taskDescription" placeholder="Описание" rows="3"></textarea>
            </div>
            <div class="d-flex gap-2">
                <div class="form-group" style="flex:1;">
                    <input type="date" class="form-input" id="taskDate" required>
                </div>
                <div class="form-group" style="flex:1;">
                    <select class="form-input" id="taskPriority" required>
                        <option value="">Приоритет</option>
                        <option value="high">Высокий</option>
                        <option value="medium">Средний</option>
                        <option value="low">Низкий</option>
                    </select>
                </div>
            </div>
            <div class="d-flex gap-2 mt-3">
                <button type="button" class="btn cancelBtn" style="flex:1; background:#e9ecef;" id="cancelTaskBtn">Отмена</button>
                <button type="submit" class="btn btn-primary" style="flex:1;" id="saveTaskBtn">Сохранить</button>
            </div>
        </form>
    </div>
</div>

<div class="modal-overlay" id="expenseModal">
    <div class="modal-content">
        <h3 id="expenseModalTitle">Добавить расход</h3>
        <form id="expenseForm">
            <div class="form-group">
                <input type="text" class="form-input" id="expenseTitle" placeholder="Название расхода" required>
            </div>
            <div class="form-group">
                <input type="number" class="form-input" id="expenseAmount" placeholder="Сумма (₸)" required>
            </div>
            <div class="form-group">
                <select class="form-input" id="expenseCategory" required>
                    <option value="">Категория</option>
                    <option value="food">Еда</option>
                    <option value="transport">Транспорт</option>
                    <option value="entertainment">Развлечения</option>
                    <option value="bills">Счета</option>
                    <option value="other">Другое</option>
                </select>
            </div>
            <div class="form-group">
                <input type="date" class="form-input" id="expenseDate" required>
            </div>
            <div class="d-flex gap-2 mt-3">
                <button type="button" class="btn cancelBtn" style="flex:1; background:#e9ecef;" id="cancelExpenseBtn">Отмена</button>
                <button type="submit" class="btn btn-primary" style="flex:1;" id="saveExpenseBtn">Сохранить</button>
            </div>
        </form>
    </div>
</div>

<div class="modal-overlay" id="goalModal">
    <div class="modal-content">
        <h3 id="goalModalTitle">Новая цель</h3>
        <form id="goalForm">
            <div class="form-group">
                <input type="text" class="form-input" id="goalTitle" placeholder="Название цели" required>
            </div>
            <div class="form-group">
                <textarea class="form-input" id="goalDescription" placeholder="Описание" rows="3"></textarea>
            </div>
            <div class="d-flex gap-2">
                <div class="form-group" style="flex:1;">
                    <input type="date" class="form-input" id="goalDeadline" required>
                </div>
                <div class="form-group" style="flex:1;">
                    <select class="form-input" id="goalPriority" required>
                        <option value="">Приоритет</option>
                        <option value="high">Высокий</option>
                        <option value="medium">Средний</option>
                        <option value="low">Низкий</option>
                    </select>
                </div>
            </div>
            <div class="d-flex gap-2 mt-3">
                <button type="button" class="btn cancelBtn" style="flex:1; background:#e9ecef;" id="cancelGoalBtn">Отмена</button>
                <button type="submit" class="btn btn-primary" style="flex:1;" id="saveGoalBtn">Сохранить</button>
            </div>
        </form>
    </div>
</div>

<script>
class ProductivityApp {
    constructor() {
        this.tasks = [];
        this.expenses = [];
        this.goals = [];
        this.settings = {
            budgetLimit: 50000,
            notifications: 'enabled',
            language: 'ru',
            theme: 'light'
        };
        this.charts = {};
        this.currentLanguage = 'ru';
        this.languages = {
            ru: {
                appTitle: "Productivity Master",
                navMain: "Основное",
                navDashboard: "Дашборд",
                navTasks: "Задачи и планы",
                navBudget: "Бюджет трекер",
                navAnalytics: "Аналитика",
                navStats: "Статистика",
                navGoals: "Цели",
                navSettings: "Настройки",
                navSettingsItem: "Настройки",
                dashboardTitle: "Обзор продуктивности",
                todayProgress: "Сегодняшний прогресс",
                progressText: " задач выполнено",
                urgentTasks: "Срочные задачи",
                urgentText: "Требуют внимания сегодня",
                monthBudget: "Бюджет месяца",
                budgetText: "Использовано от",
                recentTasks: "Последние задачи",
                tasksTitle: "Управление задачами",
                newTaskBtn: "Новая задача",
                budgetTitle: "Трекер бюджета",
                addExpenseText: "Добавить расход",
                totalBudget: "Общий бюджет",
                budgetPeriod: "Сентябрь 2023",
                remainingBudget: "Остаток",
                remainingText: "от бюджета",
                totalExpenses: "Расходы",
                expensesText: "от бюджета",
                expensesChartTitle: "Расходы по категориям",
                statsTitle: "Статистика продуктивности",
                taskCompletionTitle: "Выполнение задач по дням",
                productivityTrendTitle: "Тренд продуктивности",
                goalsTitle: "Мои цели",
                newGoalBtn: "Новая цель",
                settingsTitle: "Настройки",
                appSettings: "Настройки приложения",
                budgetLimitLabel: "Лимит бюджета на месяц (₸)",
                notificationsLabel: "Уведомления",
                saveSettingsText: "Сохранить настройки",
                taskModalTitle: "Новая задача",
                expenseModalTitle: "Добавить расход",
                goalModalTitle: "Новая цель",
                cancelBtn: "Отмена",
                saveBtn: "Сохранить",
                highPriority: "Высокий",
                mediumPriority: "Средний",
                lowPriority: "Низкий",
                completed: "Выполнено",
                notCompleted: "Не выполнено",
                deleteBtn: "Удалить",
                markCompletedBtn: "Отметить выполненным",
                category: "Категория",
                date: "Дата",
                priority: "Приоритет",
                deadline: "Срок",
                description: "Описание"
            },
            kz: {
                appTitle: "Productivity Master",
                navMain: "Негізгі",
                navDashboard: "Бақылау тақтасы",
                navTasks: "Тапсырмалар және жоспарлар",
                navBudget: "Бюджет трекері",
                navAnalytics: "Аналитика",
                navStats: "Статистика",
                navGoals: "Мақсаттар",
                navSettings: "Баптаулар",
                navSettingsItem: "Баптаулар",
                dashboardTitle: "Өнімділікке шолу",
                todayProgress: "Бүгінгі прогресс",
                progressText: " тапсырма орындалды",
                urgentTasks: "Шұғыл тапсырмалар",
                urgentText: "Бүгін назар аударуды қажет етеді",
                monthBudget: "Айдың бюджеты",
                budgetText: " қолданылды",
                recentTasks: "Соңғы тапсырмалар",
                tasksTitle: "Тапсырмаларды басқару",
                newTaskBtn: "Жаңа тапсырма",
                budgetTitle: "Бюджет трекері",
                addExpenseText: "Шығын қосу",
                totalBudget: "Жалпы бюджет",
                budgetPeriod: "Қыркүйек 2023",
                remainingBudget: "Қалды",
                remainingText: " бюджеттен",
                totalExpenses: "Шығындар",
                expensesText: " бюджеттен",
                expensesChartTitle: "Санат бойынша шығындар",
                statsTitle: "Өнімділік статистикасы",
                taskCompletionTitle: "Күндер бойынша тапсырмаларды орындау",
                productivityTrendTitle: "Өнімділік тенденциясы",
                goalsTitle: "Менің мақсаттарым",
                newGoalBtn: "Жаңа мақсат",
                settingsTitle: "Баптаулар",
                appSettings: "Қолданба баптаулары",
                budgetLimitLabel: "Айына бюджет шегі (₸)",
                notificationsLabel: "Хабарландырулар",
                saveSettingsText: "Баптауларды сақтау",
                taskModalTitle: "Жаңа тапсырма",
                expenseModalTitle: "Шығын қосу",
                goalModalTitle: "Жаңа мақсат",
                cancelBtn: "Болдырмау",
                saveBtn: "Сақтау",
                highPriority: "Жоғары",
                mediumPriority: "Орташа",
                lowPriority: "Төмен",
                completed: "Орындалды",
                notCompleted: "Орындалмады",
                deleteBtn: "Жою",
                markCompletedBtn: "Орындалды деп белгілеу",
                category: "Санат",
                date: "Күні",
                priority: "Басымдық",
                deadline: "Мерзімі",
                description: "Сипаттама"
            },
            en: {
                appTitle: "Productivity Master",
                navMain: "Main",
                navDashboard: "Dashboard",
                navTasks: "Tasks and Plans",
                navBudget: "Budget Tracker",
                navAnalytics: "Analytics",
                navStats: "Statistics",
                navGoals: "Goals",
                navSettings: "Settings",
                navSettingsItem: "Settings",
                dashboardTitle: "Productivity Overview",
                todayProgress: "Today's Progress",
                progressText: " tasks completed",
                urgentTasks: "Urgent Tasks",
                urgentText: "Require attention today",
                monthBudget: "Monthly Budget",
                budgetText: "Used from",
                recentTasks: "Recent Tasks",
                tasksTitle: "Task Management",
                newTaskBtn: "New Task",
                budgetTitle: "Budget Tracker",
                addExpenseText: "Add Expense",
                totalBudget: "Total Budget",
                budgetPeriod: "September 2023",
                remainingBudget: "Remaining",
                remainingText: " of budget",
                totalExpenses: "Expenses",
                expensesText: " of budget",
                expensesChartTitle: "Expenses by Category",
                statsTitle: "Productivity Statistics",
                taskCompletionTitle: "Task Completion by Day",
                productivityTrendTitle: "Productivity Trend",
                goalsTitle: "My Goals",
                newGoalBtn: "New Goal",
                settingsTitle: "Settings",
                appSettings: "Application Settings",
                budgetLimitLabel: "Monthly Budget Limit (₸)",
                notificationsLabel: "Notifications",
                saveSettingsText: "Save Settings",
                taskModalTitle: "New Task",
                expenseModalTitle: "Add Expense",
                goalModalTitle: "New Goal",
                cancelBtn: "Cancel",
                saveBtn: "Save",
                highPriority: "High",
                mediumPriority: "Medium",
                lowPriority: "Low",
                completed: "Completed",
                notCompleted: "Not Completed",
                deleteBtn: "Delete",
                markCompletedBtn: "Mark as Completed",
                category: "Category",
                date: "Date",
                priority: "Priority",
                deadline: "Deadline",
                description: "Description"
            }
        };
        this.init();
    }

    init() {
        this.loadData();
        this.setupEventListeners();
        this.renderAll();
        this.updateLanguage();
    }

    setupEventListeners() {
        // Навигация
        document.querySelectorAll('.nav-item').forEach(item => {
            item.addEventListener('click', () => this.switchTab(item.dataset.tab));
        });

        // Темная тема
        document.querySelector('.theme-toggle').addEventListener('click', () => {
            this.toggleTheme();
        });

        // Язык
        document.getElementById('languageSelect').addEventListener('change', (e) => {
            this.currentLanguage = e.target.value;
            this.updateLanguage();
            this.saveData();
        });

        // Мобильное меню
               document.querySelector('.menu-toggle').addEventListener('click', () => {
            document.querySelector('.sidebar').classList.toggle('open');
        });

        // Модальные окна
        document.getElementById('addTaskBtn').addEventListener('click', () => {
            this.openTaskModal();
        });

        document.getElementById('addExpenseBtn').addEventListener('click', () => {
            this.openExpenseModal();
        });

        document.getElementById('addGoalBtn').addEventListener('click', () => {
            this.openGoalModal();
        });

        // Формы
        document.getElementById('taskForm').addEventListener('submit', (e) => {
            e.preventDefault();
            this.saveTask();
        });

        document.getElementById('expenseForm').addEventListener('submit', (e) => {
            e.preventDefault();
            this.saveExpense();
        });

        document.getElementById('goalForm').addEventListener('submit', (e) => {
            e.preventDefault();
            this.saveGoal();
        });

        // Кнопки отмены
        document.querySelectorAll('.cancelBtn').forEach(btn => {
            btn.addEventListener('click', () => {
                this.closeAllModals();
            });
        });

        // Сохранение настроек
        document.getElementById('saveSettingsBtn').addEventListener('click', () => {
            this.saveSettings();
        });

        // Закрытие модальных окон по клику вне области
        document.querySelectorAll('.modal-overlay').forEach(modal => {
            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    modal.style.display = 'none';
                }
            });
        });
    }

    switchTab(tabName) {
        // Скрыть все вкладки
        document.querySelectorAll('.tab-content').forEach(tab => {
            tab.classList.remove('active');
        });

        // Показать выбранную вкладку
        document.getElementById(tabName).classList.add('active');

        // Обновить активный пункт меню
        document.querySelectorAll('.nav-item').forEach(item => {
            item.classList.remove('active');
            if (item.dataset.tab === tabName) {
                item.classList.add('active');
            }
        });

        // Закрыть мобильное меню
        document.querySelector('.sidebar').classList.remove('open');

        // Обновить контент вкладки
        this.updateTabContent(tabName);
    }

    updateTabContent(tabName) {
        switch(tabName) {
            case 'dashboard':
                this.updateDashboard();
                break;
            case 'tasks':
                this.renderTasks();
                break;
            case 'budget':
                this.renderBudget();
                break;
            case 'stats':
                this.renderStatistics();
                break;
            case 'goals':
                this.renderGoals();
                break;
        }
    }

    toggleTheme() {
        const body = document.body;
        const themeIcon = document.getElementById('themeIcon');
        
        if (body.classList.contains('dark-theme')) {
            body.classList.remove('dark-theme');
            themeIcon.textContent = '🌙';
            this.settings.theme = 'light';
        } else {
            body.classList.add('dark-theme');
            themeIcon.textContent = '☀️';
            this.settings.theme = 'dark';
        }
        this.saveData();
    }

    updateLanguage() {
        const lang = this.languages[this.currentLanguage];
        
        // Обновляем все текстовые элементы
        Object.keys(lang).forEach(key => {
            const elements = document.querySelectorAll(`[id="${key}"]`);
            elements.forEach(element => {
                if (element.tagName === 'INPUT' || element.tagName === 'TEXTAREA') {
                    element.placeholder = lang[key];
                } else {
                    element.textContent = lang[key];
                }
            });
        });

        // Обновляем опции выбора
        const priorityOptions = document.querySelectorAll('#taskPriority option, #goalPriority option');
        priorityOptions[1].textContent = lang.highPriority;
        priorityOptions[2].textContent = lang.mediumPriority;
        priorityOptions[3].textContent = lang.lowPriority;

        // Обновляем кнопки в задачах и целях
        this.renderTasks();
        this.renderGoals();
    }

    openTaskModal(task = null) {
        const modal = document.getElementById('taskModal');
        const form = document.getElementById('taskForm');
        const title = document.getElementById('taskModalTitle');
        
        if (task) {
            title.textContent = this.languages[this.currentLanguage].editTask || 'Редактировать задачу';
            document.getElementById('taskTitle').value = task.title;
            document.getElementById('taskDescription').value = task.description || '';
            document.getElementById('taskDate').value = task.date;
            document.getElementById('taskPriority').value = task.priority;
            form.dataset.editId = task.id;
        } else {
            title.textContent = this.languages[this.currentLanguage].taskModalTitle;
            form.reset();
            delete form.dataset.editId;
        }
        
        modal.style.display = 'flex';
    }

    openExpenseModal(expense = null) {
        const modal = document.getElementById('expenseModal');
        const form = document.getElementById('expenseForm');
        const title = document.getElementById('expenseModalTitle');
        
        if (expense) {
            title.textContent = this.languages[this.currentLanguage].editExpense || 'Редактировать расход';
            document.getElementById('expenseTitle').value = expense.title;
            document.getElementById('expenseAmount').value = expense.amount;
            document.getElementById('expenseCategory').value = expense.category;
            document.getElementById('expenseDate').value = expense.date;
            form.dataset.editId = expense.id;
        } else {
            title.textContent = this.languages[this.currentLanguage].expenseModalTitle;
            form.reset();
            document.getElementById('expenseDate').value = new Date().toISOString().split('T')[0];
            delete form.dataset.editId;
        }
        
        modal.style.display = 'flex';
    }

    openGoalModal(goal = null) {
        const modal = document.getElementById('goalModal');
        const form = document.getElementById('goalForm');
        const title = document.getElementById('goalModalTitle');
        
        if (goal) {
            title.textContent = this.languages[this.currentLanguage].editGoal || 'Редактировать цель';
            document.getElementById('goalTitle').value = goal.title;
            document.getElementById('goalDescription').value = goal.description || '';
            document.getElementById('goalDeadline').value = goal.deadline;
            document.getElementById('goalPriority').value = goal.priority;
            form.dataset.editId = goal.id;
        } else {
            title.textContent = this.languages[this.currentLanguage].goalModalTitle;
            form.reset();
            document.getElementById('goalDeadline').value = new Date(Date.now() + 30 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
            delete form.dataset.editId;
        }
        
        modal.style.display = 'flex';
    }

    closeAllModals() {
        document.querySelectorAll('.modal-overlay').forEach(modal => {
            modal.style.display = 'none';
        });
    }

    saveTask() {
        const form = document.getElementById('taskForm');
        const taskData = {
            title: document.getElementById('taskTitle').value,
            description: document.getElementById('taskDescription').value,
            date: document.getElementById('taskDate').value,
            priority: document.getElementById('taskPriority').value,
            completed: false,
            id: form.dataset.editId || Date.now().toString()
        };

        if (form.dataset.editId) {
            const index = this.tasks.findIndex(t => t.id === form.dataset.editId);
            this.tasks[index] = taskData;
        } else {
            this.tasks.push(taskData);
        }

        this.closeAllModals();
        this.renderTasks();
        this.updateDashboard();
        this.saveData();
    }

    saveExpense() {
        const form = document.getElementById('expenseForm');
        const expenseData = {
            title: document.getElementById('expenseTitle').value,
            amount: parseFloat(document.getElementById('expenseAmount').value),
            category: document.getElementById('expenseCategory').value,
            date: document.getElementById('expenseDate').value,
            id: form.dataset.editId || Date.now().toString()
        };

        if (form.dataset.editId) {
            const index = this.expenses.findIndex(e => e.id === form.dataset.editId);
            this.expenses[index] = expenseData;
        } else {
            this.expenses.push(expenseData);
        }

        this.closeAllModals();
        this.renderBudget();
        this.updateDashboard();
        this.saveData();
    }

    saveGoal() {
        const form = document.getElementById('goalForm');
        const goalData = {
            title: document.getElementById('goalTitle').value,
            description: document.getElementById('goalDescription').value,
            deadline: document.getElementById('goalDeadline').value,
            priority: document.getElementById('goalPriority').value,
            completed: false,
            id: form.dataset.editId || Date.now().toString()
        };

        if (form.dataset.editId) {
            const index = this.goals.findIndex(g => g.id === form.dataset.editId);
            this.goals[index] = goalData;
        } else {
            this.goals.push(goalData);
        }

        this.closeAllModals();
        this.renderGoals();
        this.saveData();
    }

    saveSettings() {
        this.settings.budgetLimit = parseInt(document.getElementById('budgetLimit').value);
        this.settings.notifications = document.getElementById('notifications').value;
        this.saveData();
        
        // Показать уведомление об успешном сохранении
        this.showNotification('Настройки сохранены успешно!');
    }

    showNotification(message) {
        const notification = document.createElement('div');
        notification.style.cssText = `
            position: fixed;
            top: 100px;
            right: 20px;
            background: var(--success);
            color: white;
            padding: 1rem 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow-lg);
            z-index: 10000;
            animation: slideInRight 0.3s ease-out;
        `;
        notification.textContent = message;
        document.body.appendChild(notification);

        setTimeout(() => {
            notification.remove();
        }, 3000);
    }

    deleteTask(taskId) {
        if (confirm('Вы уверены, что хотите удалить эту задачу?')) {
            this.tasks = this.tasks.filter(t => t.id !== taskId);
            this.renderTasks();
            this.updateDashboard();
            this.saveData();
        }
    }

    deleteExpense(expenseId) {
        if (confirm('Вы уверены, что хотите удалить этот расход?')) {
            this.expenses = this.expenses.filter(e => e.id !== expenseId);
            this.renderBudget();
            this.updateDashboard();
            this.saveData();
        }
    }

    deleteGoal(goalId) {
        if (confirm('Вы уверены, что хотите удалить эту цель?')) {
            this.goals = this.goals.filter(g => g.id !== goalId);
            this.renderGoals();
            this.saveData();
        }
    }

    toggleTaskCompletion(taskId) {
        const task = this.tasks.find(t => t.id === taskId);
        if (task) {
            task.completed = !task.completed;
            this.renderTasks();
            this.updateDashboard();
            this.saveData();
        }
    }

    toggleGoalCompletion(goalId) {
        const goal = this.goals.find(g => g.id === goalId);
        if (goal) {
            goal.completed = !goal.completed;
            this.renderGoals();
            this.saveData();
        }
    }

    renderTasks() {
        const container = document.getElementById('taskList');
        const lang = this.languages[this.currentLanguage];
        
        if (this.tasks.length === 0) {
            container.innerHTML = `<div class="stat-card">${lang.noTasks || 'Задачи не найдены'}</div>`;
            return;
        }

        container.innerHTML = this.tasks.map(task => `
            <div class="task-card ${task.priority}-priority ${task.completed ? 'completed' : ''}">
                <div class="d-flex justify-between align-center mb-2">
                    <h3>${task.title}</h3>
                    <span class="priority-badge">${lang[task.priority + 'Priority']}</span>
                </div>
                ${task.description ? `<p class="mb-2">${task.description}</p>` : ''}
                <div class="d-flex justify-between align-center">
                    <small>${task.date}</small>
                    <div class="d-flex gap-1">
                        <button class="btn btn-success" onclick="app.toggleTaskCompletion('${task.id}')">
                            ${task.completed ? lang.notCompleted : lang.markCompletedBtn}
                        </button>
                        <button class="btn" onclick="app.openTaskModal(${JSON.stringify(task).replace(/"/g, '&quot;')})">
                            ✏️
                        </button>
                        <button class="btn btn-danger" onclick="app.deleteTask('${task.id}')">
                            ${lang.deleteBtn}
                        </button>
                    </div>
                </div>
            </div>
        `).join('');
    }

    renderBudget() {
        const totalExpenses = this.expenses.reduce((sum, expense) => sum + expense.amount, 0);
        const remaining = this.settings.budgetLimit - totalExpenses;
        const usedPercentage = (totalExpenses / this.settings.budgetLimit) * 100;

        // Обновляем цифры
        document.getElementById('totalBudgetAmount').textContent = this.formatCurrency(this.settings.budgetLimit);
        document.getElementById('remainingAmount').textContent = this.formatCurrency(remaining);
        document.getElementById('expensesAmount').textContent = this.formatCurrency(totalExpenses);

        // Обновляем прогресс
        document.getElementById('budgetProgress').style.width = `${Math.min(usedPercentage, 100)}%`;

        // Обновляем текст
        const lang = this.languages[this.currentLanguage];
        document.getElementById('budgetText').textContent = 
            `${lang.budgetText} ${usedPercentage.toFixed(0)}% ${lang.from || 'от'} ${this.formatCurrency(this.settings.budgetLimit)}`;
        document.getElementById('remainingText').textContent = 
            `${((remaining / this.settings.budgetLimit) * 100).toFixed(0)}% ${lang.remainingText}`;
        document.getElementById('expensesText').textContent = 
            `${usedPercentage.toFixed(0)}% ${lang.expensesText}`;

        // Рендерим график расходов
        this.renderExpensesChart();

        // Рендерим список расходов
        this.renderExpensesList();
    }

    renderExpensesChart() {
        const ctx = document.getElementById('expensesChart').getContext('2d');
        const categories = {};
        
        this.expenses.forEach(expense => {
            categories[expense.category] = (categories[expense.category] || 0) + expense.amount;
        });

        if (this.charts.expensesChart) {
            this.charts.expensesChart.destroy();
        }

        this.charts.expensesChart = new Chart(ctx, {
            type: 'doughnut',
            data: {
                labels: Object.keys(categories),
                datasets: [{
                    data: Object.values(categories),
                    backgroundColor: ['#4361ee', '#4cc9f0', '#f72585', '#7209b7', '#3a0ca3']
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
    }

    renderExpensesList() {
        const container = document.getElementById('expensesList');
        const lang = this.languages[this.currentLanguage];
        
        if (this.expenses.length === 0) {
            container.innerHTML = `<div class="stat-card">${lang.noExpenses || 'Расходы не найдены'}</div>`;
            return;
        }

        // Сортируем по дате (новые сверху)
        const sortedExpenses = [...this.expenses].sort((a, b) => new Date(b.date) - new Date(a.date));

        container.innerHTML = sortedExpenses.map(expense => `
            <div class="task-card">
                <div class="d-flex justify-between align-center mb-2">
                    <h3>${expense.title}</h3>
                    <span class="text-large">${this.formatCurrency(expense.amount)}</span>
                </div>
                <div class="d-flex justify-between align-center">
                    <div>
                        <small><strong>${lang.category}:</strong> ${expense.category}</small>
                        <small><strong>${lang.date}:</strong> ${expense.date}</small>
                    </div>
                    <div class="d-flex gap-1">
                        <button class="btn" onclick="app.openExpenseModal(${JSON.stringify(expense).replace(/"/g, '&quot;')})">
                            ✏️
                        </button>
                        <button class="btn btn-danger" onclick="app.deleteExpense('${expense.id}')">
                            ${lang.deleteBtn}
                        </button>
                    </div>
                </div>
            </div>
        `).join('');
    }

    renderGoals() {
        const container = document.getElementById('goalsList');
        const lang = this.languages[this.currentLanguage];
        
        if (this.goals.length === 0) {
            container.innerHTML = `<div class="stat-card">${lang.noGoals || 'Цели не найдены'}</div>`;
            return;
        }

        container.innerHTML = this.goals.map(goal => `
            <div class="task-card ${goal.priority}-priority ${goal.completed ? 'completed' : ''}">
                <div class="d-flex justify-between align-center mb-2">
                    <h3>${goal.title}</h3>
                    <span class="priority-badge">${lang[goal.priority + 'Priority']}</span>
                </div>
                ${goal.description ? `<p class="mb-2">${goal.description}</p>` : ''}
                <div class="d-flex justify-between align-center">
                    <small><strong>${lang.deadline}:</strong> ${goal.deadline}</small>
                    <div class="d-flex gap-1">
                        <button class="btn btn-success" onclick="app.toggleGoalCompletion('${goal.id}')">
                            ${goal.completed ? lang.notCompleted : lang.markCompletedBtn}
                        </button>
                        <button class="btn" onclick="app.openGoalModal(${JSON.stringify(goal).replace(/"/g, '&quot;')})">
                            ✏️
                        </button>
                        <button class="btn btn-danger" onclick="app.deleteGoal('${goal.id}')">
                            ${lang.deleteBtn}
                        </button>
                    </div>
                </div>
            </div>
        `).join('');
    }

    renderStatistics() {
        this.renderTaskCompletionChart();
        this.renderProductivityChart();
    }

    renderTaskCompletionChart() {
        const ctx = document.getElementById('taskCompletionChart').getContext('2d');
        
        // Генерируем тестовые данные для демонстрации
        const last7Days = Array.from({length: 7}, (_, i) => {
            const date = new Date();
            date.setDate(date.getDate() - i);
            return date.toLocaleDateString();
        }).reverse();

        const completedData = last7Days.map(() => Math.floor(Math.random() * 10));
        const totalData = completedData.map(val => val + Math.floor(Math.random() * 5));

        if (this.charts.taskCompletionChart) {
            this.charts.taskCompletionChart.destroy();
        }

        this.charts.taskCompletionChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: last7Days,
                datasets: [
                    {
                        label: 'Выполненные',
                        data: completedData,
                        backgroundColor: '#4361ee'
                    },
                    {
                        label: 'Всего задач',
                        data: totalData,
                        backgroundColor: '#4cc9f0'
                    }
                ]
            },
            options: {
                responsive: true,
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });
    }

    renderProductivityChart() {
        const ctx = document.getElementById('productivityChart').getContext('2d');
        
        // Генерируем тестовые данные для демонстрации
        const last30Days = Array.from({length: 30}, (_, i) => {
            const date = new Date();
            date.setDate(date.getDate() - 29 + i);
            return date.getDate() + '/' + (date.getMonth() + 1);
        });

        const productivityData = last30Days.map(() => Math.floor(Math.random() * 100));

        if (this.charts.productivityChart) {
            this.charts.productivityChart.destroy();
        }

        this.charts.productivityChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: last30Days,
                datasets: [{
                    label: 'Продуктивность (%)',
                    data: productivityData,
                    borderColor: '#f72585',
                    backgroundColor: 'rgba(247, 37, 133, 0.1)',
                    tension: 0.4,
                    fill: true
                }]
            },
            options: {
                responsive: true,
                scales: {
                    y: {
                        min: 0,
                        max: 100
                    }
                }
            }
        });
    }

    updateDashboard() {
        // Обновляем прогресс задач
        const today = new Date().toISOString().split('T')[0];
        const todayTasks = this.tasks.filter(task => task.date === today);
        const completedToday = todayTasks.filter(task => task.completed).length;
        const progressPercentage = todayTasks.length > 0 ? (completedToday / todayTasks.length) * 100 : 0;
        
        document.getElementById('progressCircle').style.strokeDashoffset = 100 - progressPercentage;
        document.getElementById('progressText').textContent = 
            `${completedToday}/${todayTasks.length} ${this.languages[this.currentLanguage].progressText}`;

        // Обновляем срочные задачи
        const urgentTasks = this.tasks.filter(task => 
            !task.completed && task.date === today && task.priority === 'high'
        );
        document.getElementById('urgentCount').textContent = urgentTasks.length;

        // Обновляем последние задачи
        this.renderDashboardTasks();
    }

    renderDashboardTasks() {
        const container = document.getElementById('dashboardTasks');
        const recentTasks = this.tasks.slice(-3).reverse(); // Последние 3 задачи
        
        if (recentTasks.length === 0) {
            container.innerHTML = '<div class="stat-card">Нет задач для отображения</div>';
            return;
        }

        container.innerHTML = recentTasks.map(task => `
            <div class="task-card ${task.priority}-priority ${task.completed ? 'completed' : ''}">
                <h4>${task.title}</h4>
                <p class="mb-2">${task.description || ''}</p>
                <div class="d-flex justify-between align-center">
                    <small>${task.date}</small>
                    <span class="priority-badge">${this.languages[this.currentLanguage][task.priority + 'Priority']}</span>
                </div>
            </div>
        `).join('');
    }

    formatCurrency(amount) {
        return new Intl.NumberFormat('ru-RU', {
            style: 'currency',
            currency: 'KZT'
        }).format(amount);
    }

    saveData() {
        const data = {
            tasks: this.tasks,
            expenses: this.expenses,
            goals: this.goals,
            settings: this.settings,
            language: this.currentLanguage
        };
        localStorage.setItem('productivityMaster', JSON.stringify(data));
    }

    loadData() {
        const saved = localStorage.getItem('productivityMaster');
        if (saved) {
            const data = JSON.parse(saved);
            this.tasks = data.tasks || [];
            this.expenses = data.expenses || [];
            this.goals = data.goals || [];
            this.settings = data.settings || this.settings;
            this.currentLanguage = data.language || 'ru';

            // Применяем настройки
            document.getElementById('budgetLimit').value = this.settings.budgetLimit;
            document.getElementById('notifications').value = this.settings.notifications;
            document.getElementById('languageSelect').value = this.currentLanguage;

            if (this.settings.theme === 'dark') {
                document.body.classList.add('dark-theme');
                document.getElementById('themeIcon').textContent = '☀️';
            }
        }
    }

    renderAll() {
        this.updateDashboard();
        this.renderTasks();
        this.renderBudget();
        this.renderGoals();
    }
}

// Инициализация приложения
const app = new ProductivityApp();

// Глобальные функции для вызова из HTML
window.app = app;

// Загружаем начальные данные если их нет
if (!localStorage.getItem('productivityMaster')) {
    // Добавляем демо-данные
    app.tasks = [
        {
            id: '1',
            title: 'Завершить проект',
            description: 'Закончить работу над важным проектом',
            date: new Date().toISOString().split('T')[0],
            priority: 'high',
            completed: false
        },
        {
            id: '2',
            title: 'Купить продукты',
            description: 'Молоко, хлеб, фрукты',
            date: new Date().toISOString().split('T')[0],
            priority: 'medium',
            completed: true
        }
    ];

    app.expenses = [
        {
            id: '1',
            title: 'Продукты',
            amount: 15000,
            category: 'food',
            date: new Date().toISOString().split('T')[0]
        }
    ];

    app.saveData();
}
</script>
</body>
</html>
