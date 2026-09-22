<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Lesson Studio v2 — RAG Educational Content Factory</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            900: '#1e3a8a'
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col">

    <!-- Header / Navbar -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="bg-brand-600 text-white p-2 rounded-xl shadow-md">
                    <i class="fa-solid fa-brain-circuit text-xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg text-slate-900 leading-tight">AI Content Factory <span class="text-xs bg-brand-100 text-brand-700 px-2 py-0.5 rounded-full font-semibold ml-1">RAG Architecture</span></h1>
                    <p class="text-xs text-slate-500">Syllabus Grounded Educational Package Generator</p>
                </div>
            </div>

            <!-- Header Controls -->
            <div class="flex items-center space-x-3">
                <!-- Demo Quick Run Button -->
                <button onclick="runDemoScenario()" class="bg-amber-500 hover:bg-amber-600 text-white px-3 py-1.5 rounded-lg text-xs font-semibold shadow flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-play"></i> 🎬 Run Demo Scenario
                </button>

                <!-- Mode Switcher -->
                <div class="bg-slate-100 p-1 rounded-lg flex items-center text-xs font-medium border border-slate-200">
                    <button id="btnTeacherMode" onclick="setMode('teacher')" class="px-3 py-1 rounded-md bg-white text-slate-800 shadow-sm transition">
                        <i class="fa-solid fa-chalkboard-user mr-1 text-brand-600"></i> Мұғалім режимі
                    </button>
                    <button id="btnStudentMode" onclick="setMode('student')" class="px-3 py-1 rounded-md text-slate-500 hover:text-slate-800 transition">
                        <i class="fa-solid fa-graduation-cap mr-1"></i> Оқушы режимі
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Role Mode Explanation -->
    <section class="max-w-7xl w-full mx-auto px-4 md:px-6 pt-5">
        <div id="roleInfoPanel" class="bg-white border border-slate-200 rounded-2xl p-4 shadow-sm flex items-start gap-3">
            <div id="roleInfoIcon" class="w-10 h-10 shrink-0 rounded-xl bg-brand-100 text-brand-600 flex items-center justify-center">
                <i class="fa-solid fa-chalkboard-user"></i>
            </div>
            <div>
                <h3 id="roleInfoTitle" class="font-bold text-sm text-slate-900">Мұғалім режимі</h3>
                <p id="roleInfoText" class="text-xs text-slate-600 mt-1">
                    Сабақ жоспары, практикалық тапсырма, деңгейлік тапсырмалар, бағалау рубрикасы және AI құралдары көрсетіледі.
                </p>
            </div>
        </div>
    </section>

    <!-- Main Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-6">

        <!-- Left Sidebar: Inputs & Pipeline (4 cols) -->
        <section class="lg:col-span-4 space-y-6">
            
            <!-- Step 1: Syllabus Upload / Input -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm">
                <div class="flex items-center justify-between mb-4">
                    <h2 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                        <span class="w-6 h-6 rounded-full bg-brand-100 text-brand-700 text-xs flex items-center justify-center font-bold">1</span>
                        Оқу бағдарламасы және параметрлер
                    </h2>
                    <span class="text-xs text-slate-400">Білім көзі</span>
                </div>

                <div class="space-y-4">
                    <!-- Functional Syllabus Upload -->
                    <div onclick="document.getElementById('syllabusFile').click()"
                         class="border-2 border-dashed border-slate-300 rounded-xl p-5 text-center hover:border-brand-500 hover:bg-brand-50/40 bg-slate-50/50 cursor-pointer transition">
                        <input id="syllabusFile" type="file" accept=".pdf,.doc,.docx,.txt" class="hidden" onchange="handleSyllabusUpload(event)">
                        <div class="w-11 h-11 mx-auto rounded-xl bg-brand-100 text-brand-600 flex items-center justify-center mb-2">
                            <i class="fa-solid fa-cloud-arrow-up text-xl"></i>
                        </div>
                        <p class="text-xs font-bold text-slate-800">Оқу бағдарламасын жүктеу</p>
                        <p class="text-[10px] text-slate-500 mt-1">PDF, DOCX немесе TXT файлын таңдаңыз</p>
                        <button type="button" class="mt-3 bg-white border border-brand-300 text-brand-700 px-3 py-1.5 rounded-lg text-xs font-semibold">
                            <i class="fa-solid fa-folder-open mr-1"></i> Файлды таңдау
                        </button>
                        <div id="syllabusFileInfo" class="hidden mt-3 p-2 bg-emerald-50 border border-emerald-200 rounded-lg text-left">
                            <p id="syllabusFileName" class="text-[11px] font-semibold text-emerald-800"></p>
                            <p class="text-[10px] text-emerald-600">✓ Файл қабылданды • AI талдауға дайын</p>
                        </div>
                    </div>

                    <!-- Topic Selection -->
                    <div>
                        <label class="block text-xs font-medium text-slate-700 mb-1">Тақырыпты таңдаңыз / Topic:</label>
                        <select id="topicSelect" onchange="onTopicChange()" class="w-full bg-white border border-slate-300 rounded-lg px-3 py-2 text-xs font-medium focus:ring-2 focus:ring-brand-500 focus:outline-none">
                            <option value="Inheritance">Topic 4: OOP Inheritance & Polymorphism</option>
                            <option value="SQL_Joins">Topic 7: Database SQL Joins & Optimization</option>
                            <option value="Kinematics">Topic 2: Physics - Classical Kinematics</option>
                        </select>
                    </div>

                    <!-- Parameters Grid -->
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-[11px] font-medium text-slate-600 mb-1">Ұзақтығы (Duration):</label>
                            <select id="durationSelect" class="w-full bg-white border border-slate-300 rounded-lg px-2.5 py-1.5 text-xs">
                                <option value="45">45 мин (Short)</option>
                                <option value="90" selected>90 мин (Standard)</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-[11px] font-medium text-slate-600 mb-1">Деңгей (Level):</label>
                            <select id="levelSelect" class="w-full bg-white border border-slate-300 rounded-lg px-2.5 py-1.5 text-xs">
                                <option value="Intermediate">2nd Year Undergrad</option>
                                <option value="Beginner">1st Year Undergrad</option>
                            </select>
                        </div>
                    </div>

                    <!-- Generate Button -->
                    <button id="btnGenerate" onclick="startGenerationPipeline()" class="w-full bg-brand-600 hover:bg-brand-700 text-white font-semibold py-2.5 rounded-xl shadow-md text-xs flex items-center justify-center gap-2 transition">
                        <i class="fa-solid fa-gears"></i> AI Package Генерациялау
                    </button>
                </div>
            </div>

            <!-- Step 2: Live Pipeline Tracker -->
            <div class="bg-white rounded-2xl p-5 border border-slate-200 shadow-sm">
                <h2 class="font-bold text-slate-900 text-sm mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-list-check text-brand-600"></i> AI Generation Pipeline
                </h2>
                <div class="space-y-2.5 text-xs" id="pipelineTracker">
                    <div class="flex items-center gap-2 text-emerald-600 font-medium">
                        <i class="fa-solid fa-circle-check"></i> <span>1. Syllabus received & parsed</span>
                    </div>
                    <div class="flex items-center gap-2 text-emerald-600 font-medium">
                        <i class="fa-solid fa-circle-check"></i> <span>2. Knowledge Base indexed</span>
                    </div>
                    <div id="stepRetrieval" class="flex items-center gap-2 text-slate-400">
                        <i class="fa-regular fa-circle"></i> <span>3. Context retrieved</span>
                    </div>
                    <div id="stepGeneration" class="flex items-center gap-2 text-slate-400">
                        <i class="fa-regular fa-circle"></i> <span>4. Educational Package generated</span>
                    </div>
                    <div id="stepValidation" class="flex items-center gap-2 text-slate-400">
                        <i class="fa-regular fa-circle"></i> <span>5. Quality Check & Alignment</span>
                    </div>
                </div>
            </div>

            <!-- Step 3: Extracted Knowledge Base Cards -->
            <div class="bg-slate-900 text-slate-100 rounded-2xl p-5 shadow-lg space-y-4">
                <div class="flex items-center justify-between border-b border-slate-800 pb-2">
                    <h3 class="font-bold text-xs text-brand-400 uppercase tracking-wider flex items-center gap-1.5">
                        <i class="fa-solid fa-database"></i> Knowledge Base Extracted
                    </h3>
                    <span class="text-[10px] bg-slate-800 text-slate-300 px-2 py-0.5 rounded">RAG Active</span>
                </div>

                <div class="space-y-3 text-xs">
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-semibold">Course Context</span>
                        <p class="font-medium text-slate-200" id="kbCourse">Object-Oriented Programming (CS201)</p>
                    </div>

                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-semibold">Target Learning Outcomes</span>
                        <ul class="list-disc list-inside text-slate-300 space-y-1 text-[11px]" id="kbOutcomes">
                            <li>LO1: Explain inheritance principles</li>
                            <li>LO2: Implement child classes in Python</li>
                            <li>LO3: Compare composition vs inheritance</li>
                        </ul>
                    </div>

                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase font-semibold">Retrieved Source Chunks</span>
                        <div class="flex flex-wrap gap-1.5 mt-1" id="kbSources">
                            <span class="bg-slate-800 text-brand-300 border border-slate-700 px-2 py-0.5 rounded text-[10px]">Syllabus.pdf §4.2</span>
                            <span class="bg-slate-800 text-brand-300 border border-slate-700 px-2 py-0.5 rounded text-[10px]">Lecture_Notes_04.docx</span>
                        </div>
                    </div>
                </div>
            </div>

        </section>

        <!-- Right Content Area: Generated Package (8 cols) -->
        <section class="lg:col-span-8 space-y-6">

            <!-- Retrieved Context Callout -->
            <div class="bg-brand-50 border border-brand-200 rounded-2xl p-4 flex items-start gap-3 shadow-sm">
                <div class="p-2 bg-brand-600 text-white rounded-xl mt-0.5">
                    <i class="fa-solid fa-magnifying-glass font-bold text-sm"></i>
                </div>
                <div class="flex-1 text-xs">
                    <div class="flex items-center justify-between">
                        <h4 class="font-bold text-brand-900">🔎 Retrieved Context from Knowledge Base</h4>
                        <span class="text-[10px] bg-brand-200 text-brand-800 font-semibold px-2 py-0.5 rounded">Traceable</span>
                    </div>
                    <p class="text-brand-800 mt-1" id="retrievedContextText">
                        "Inheritance allows a class to derive properties and characteristics from another class. Topic focuses on method overriding and reusability in Python."
                    </p>
                    <div class="mt-2 flex items-center gap-2 text-[10px] text-brand-700">
                        <span class="font-bold">Grounded Status:</span>
                        <span class="bg-emerald-100 text-emerald-800 font-semibold px-2 py-0.5 rounded">✓ Verified Against Syllabus</span>
                        <span>• 5 Chunks retrieved</span>
                    </div>
                </div>
            </div>

            <!-- Quality Check Banner (Grounding Validation) -->
            <div class="bg-white border border-slate-200 rounded-2xl p-5 shadow-sm space-y-3">
                <div class="flex items-center justify-between">
                    <h3 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                        <i class="fa-solid fa-shield-check text-emerald-600 text-base"></i> AI Quality & Grounding Validation
                    </h3>
                    <span class="text-xs bg-slate-100 text-slate-600 font-medium px-2.5 py-1 rounded-lg">Demo Validation — Simulated</span>
                </div>

                <div class="grid grid-cols-2 md:grid-cols-4 gap-3 text-center">
                    <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-100">
                        <span class="block text-[10px] text-slate-400 uppercase font-bold">KB Connection</span>
                        <span class="text-xs font-bold text-emerald-600">Connected ✓</span>
                    </div>
                    <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-100">
                        <span class="block text-[10px] text-slate-400 uppercase font-bold">LOs Mapped</span>
                        <span class="text-xs font-bold text-slate-800">3 / 3 Complete</span>
                    </div>
                    <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-100">
                        <span class="block text-[10px] text-slate-400 uppercase font-bold">Level Fit</span>
                        <span class="text-xs font-bold text-slate-800">Undergrad Level</span>
                    </div>
                    <div class="bg-slate-50 p-2.5 rounded-xl border border-slate-100">
                        <span class="block text-[10px] text-slate-400 uppercase font-bold">Traceability</span>
                        <span class="text-xs font-bold text-brand-600">100% Grounded</span>
                    </div>
                </div>
            </div>

            <!-- Educational Package Output Tabs -->
            <div class="bg-white border border-slate-200 rounded-2xl shadow-sm overflow-hidden">
                <!-- Top Bar & Export Button -->
                <div class="p-4 border-b border-slate-200 flex flex-wrap items-center justify-between gap-3 bg-slate-50/50">
                    <div>
                        <h3 class="font-bold text-slate-900 text-base" id="packageTitle">Educational Package: Inheritance</h3>
                        <p class="text-xs text-slate-500">Auto-generated educational modules grounded in syllabus context</p>
                    </div>
                    <button onclick="exportPackage()" class="bg-slate-900 hover:bg-slate-800 text-white text-xs font-semibold px-4 py-2 rounded-xl shadow flex items-center gap-2 transition">
                        <i class="fa-solid fa-box-archive"></i> 📦 Export Complete Package
                    </button>
                </div>

                <!-- Alignment Map Visualiser -->
                <div class="p-4 bg-slate-900 text-white border-b border-slate-800 text-xs">
                    <span class="text-[10px] font-bold text-brand-400 uppercase block mb-1">🎯 Learning Outcome Alignment Map</span>
                    <div class="flex flex-wrap gap-4 text-slate-300" id="alignmentMapContainer">
                        <div><span class="font-semibold text-white">LO1:</span> Theory ✓ → Quiz ✓</div>
                        <div><span class="font-semibold text-white">LO2:</span> Practice ✓ → Case Study ✓</div>
                        <div><span class="font-semibold text-white">LO3:</span> Differentiation ✓ → Rubric ✓</div>
                    </div>
                </div>

                <!-- Package Modules Content -->
                <div class="p-5 space-y-6" id="packageContent">

                    <!-- Section 1: Lesson Plan Architecture -->
                    <div class="space-y-3">
                        <div class="flex items-center justify-between">
                            <h4 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                                <span class="w-2 h-2 rounded-full bg-brand-600"></span>
                                ⏱ Lesson Plan Architecture (<span id="durationBadge">90 min</span>)
                            </h4>
                            <span class="text-[10px] text-slate-400 italic">Source: Syllabus §4.2</span>
                        </div>
                        <div class="bg-slate-50 rounded-xl p-4 border border-slate-200 text-xs space-y-2" id="lessonArchitecture">
                            <!-- Injected dynamically -->
                        </div>
                    </div>

                    <!-- Section 2: Code Practice / Interactive Task -->
                    <div class="space-y-3">
                        <h4 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-brand-600"></span>
                            🧪 Practical Task & Code Exercise
                        </h4>
                        <div class="bg-slate-900 text-slate-100 rounded-xl p-4 font-mono text-xs overflow-x-auto" id="codePracticeBlock">
                            <!-- Code injected dynamically -->
                        </div>
                    </div>

                    <!-- Section 3: Interactive Quiz -->
                    <div class="space-y-3">
                        <h4 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-brand-600"></span>
                            ❓ Interactive Assessment Quiz
                        </h4>
                        <div class="bg-slate-50 border border-slate-200 rounded-xl p-4 text-xs space-y-3" id="quizContainer">
                            <!-- Quiz injected dynamically -->
                        </div>
                    </div>

                    <!-- Section 4: Differentiated Tasks (Inclusive) -->
                    <div class="space-y-3">
                        <h4 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-brand-600"></span>
                            🧩 Differentiated Tasks (Inclusive Learning)
                        </h4>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 text-xs" id="diffTasksContainer">
                            <!-- Injected dynamically -->
                        </div>
                    </div>

                    <!-- Section 5: Dynamic Rubric (Teacher Mode Only) -->
                    <div class="space-y-3 teacher-only">
                        <h4 class="font-bold text-sm text-slate-900 flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-brand-600"></span>
                            📋 Assessment Rubric (10 Points)
                        </h4>
                        <div class="overflow-x-auto">
                            <table class="w-full text-xs text-left text-slate-600 border border-slate-200 rounded-xl overflow-hidden">
                                <thead class="bg-slate-100 text-slate-800 font-semibold border-b border-slate-200">
                                    <tr>
                                        <th class="p-2.5">Criterion</th>
                                        <th class="p-2.5">Description</th>
                                        <th class="p-2.5 text-right">Points</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-slate-200" id="rubricTableBody">
                                    <!-- Injected dynamically -->
                                </tbody>
                            </table>
                        </div>
                    </div>

                </div>
            </div>

        </section>

    </main>


    <!-- External AI Tools -->
    <section class="max-w-7xl w-full mx-auto px-4 md:px-6 pb-6 teacher-only">
        <div class="bg-white border border-slate-200 rounded-2xl p-5 shadow-sm">
            <div class="flex items-center justify-between gap-3 mb-4">
                <div>
                    <h3 class="font-bold text-slate-900 text-sm flex items-center gap-2">
                        <i class="fa-solid fa-wand-magic-sparkles text-brand-600"></i>
                        Қосымша AI құралдары
                    </h3>
                    <p class="text-xs text-slate-500 mt-1">
                        Дайын материалды презентацияға немесе интерактивті тестке айналдырыңыз.
                    </p>
                </div>
                <span class="text-[10px] bg-brand-100 text-brand-700 px-2 py-1 rounded-full font-semibold">AI Tools</span>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Gamma -->
                <div class="rounded-2xl border border-slate-200 bg-slate-50 p-4">
                    <div class="flex items-start justify-between gap-3">
                        <div>
                            <div class="w-10 h-10 rounded-xl bg-violet-100 text-violet-700 flex items-center justify-center mb-3">
                                <i class="fa-solid fa-display"></i>
                            </div>
                            <h4 class="font-bold text-slate-900">Gamma AI</h4>
                            <p class="text-xs text-slate-600 mt-1">
                                Сабақ жоспары мен тақырып бойынша AI презентация жасау.
                            </p>
                        </div>
                        <span class="text-[10px] bg-violet-100 text-violet-700 px-2 py-1 rounded-full font-semibold">Presentation</span>
                    </div>
                    <button onclick="openGamma()" class="mt-4 w-full bg-violet-600 hover:bg-violet-700 text-white font-semibold py-2.5 rounded-xl text-xs transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-arrow-up-right-from-square"></i>
                        Gamma-да презентация жасау
                    </button>
                </div>

                <!-- Quizizz / Wayground -->
                <div class="rounded-2xl border border-slate-200 bg-slate-50 p-4">
                    <div class="flex items-start justify-between gap-3">
                        <div>
                            <div class="w-10 h-10 rounded-xl bg-fuchsia-100 text-fuchsia-700 flex items-center justify-center mb-3">
                                <i class="fa-solid fa-circle-question"></i>
                            </div>
                            <h4 class="font-bold text-slate-900">Quizizz / Wayground AI</h4>
                            <p class="text-xs text-slate-600 mt-1">
                                Тақырып немесе дайын материал негізінде интерактивті quiz құрастыру.
                            </p>
                        </div>
                        <span class="text-[10px] bg-fuchsia-100 text-fuchsia-700 px-2 py-1 rounded-full font-semibold">Quiz</span>
                    </div>
                    <button onclick="openQuizAI()" class="mt-4 w-full bg-fuchsia-600 hover:bg-fuchsia-700 text-white font-semibold py-2.5 rounded-xl text-xs transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-arrow-up-right-from-square"></i>
                        AI Quiz жасау
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-3 text-center text-xs text-slate-500">
        AI Content Factory v2.0 • Grounded Educational Prototype for Selection Defense
    </footer>

    <!-- JavaScript Logic -->
    <script>
        // Data Store for Dynamic Topics
        const topicData = {
            Inheritance: {
                course: "Object-Oriented Programming (CS201)",
                outcomes: [
                    "LO1: Explain OOP Inheritance principles",
                    "LO2: Implement child classes in Python",
                    "LO3: Apply method overriding in real scenarios"
                ],
                sources: ["Syllabus.pdf §4.2", "Python_Docs_Classes.pdf"],
                retrievedContext: "Inheritance allows child classes to derive attributes and methods from a parent class. Syllabus requires practical implementation in Python with method overriding.",
                lessonArch90: [
                    "00-10 min: Core Theory — Parent & Child Class Hierarchy",
                    "10-35 min: Code Demo — Inheritance syntax & super() function",
                    "35-65 min: Practical Lab — Library System Architecture Case",
                    "65-80 min: Interactive Quiz & Peer Review",
                    "80-90 min: Reflection & Extension Assignment"
                ],
                code: `class Vehicle:\n    def __init__(self, brand):\n        self.brand = brand\n\nclass Car(Vehicle):\n    def drive(self):\n        return f"{self.brand} is driving on the road."\n\n# Task: Implement Dog class inheriting from Animal`,
                quiz: {
                    question: "Which mechanism allows a class to inherit methods from another class?",
                    options: ["Encapsulation", "Inheritance", "Polymorphism", "Abstraction"],
                    correct: 1,
                    feedback: "Correct! Inheritance provides code reusability by inheriting parent traits."
                },
                diff: {
                    foundation: "A — Foundation: Match base class definitions with child classes.",
                    application: "B — Application: Complete the missing super() call in given code.",
                    advanced: "C — Advanced: Build a multi-level inheritance hierarchy for a Banking System."
                },
                rubric: [
                    { crit: "Concept Understanding", desc: "Explains inheritance mechanisms correctly", pts: "3 pts" },
                    { crit: "Code Implementation", desc: "Correctly implements child class & super()", pts: "4 pts" },
                    { crit: "Case Application", desc: "Solves real-world inheritance case study", pts: "3 pts" }
                ]
            },
            SQL_Joins: {
                course: "Database Systems (CS305)",
                outcomes: [
                    "LO1: Distinguish between INNER, LEFT, and RIGHT joins",
                    "LO2: Write multi-table SQL join queries",
                    "LO3: Optimize query performance using primary keys"
                ],
                sources: ["Database_Syllabus.pdf §7.1", "SQL_Guide.pdf"],
                retrievedContext: "SQL Joins are used to combine rows from two or more tables based on a related column. Focus on relational algebra and practical multi-table joins.",
                lessonArch90: [
                    "00-15 min: Relational Algebra & Venn Diagrams for Joins",
                    "15-40 min: Live SQL Sandbox — INNER vs LEFT JOIN",
                    "40-70 min: E-Commerce Database Queries Case Study",
                    "70-85 min: Interactive Query Quiz",
                    "85-90 min: Wrap-up & Optimization Tips"
                ],
                code: `SELECT Customers.CustomerName, Orders.OrderID\nFROM Customers\nINNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;\n\n-- Task: Modify query to include customers without orders (LEFT JOIN)`,
                quiz: {
                    question: "Which SQL JOIN returns all records from the left table and matched records from the right?",
                    options: ["INNER JOIN", "LEFT JOIN", "RIGHT JOIN", "FULL JOIN"],
                    correct: 1,
                    feedback: "Correct! LEFT JOIN includes all left-side rows regardless of right-side matches."
                },
                diff: {
                    foundation: "A — Foundation: Draw Venn diagrams representing INNER vs LEFT join.",
                    application: "B — Application: Write a query joining Students and Enrolment tables.",
                    advanced: "C — Advanced: Optimize a 4-table complex SQL JOIN for large datasets."
                },
                rubric: [
                    { crit: "Syntax Accuracy", desc: "Writes valid SQL JOIN syntax", pts: "4 pts" },
                    { crit: "Logic Fit", desc: "Chooses correct JOIN type for given problem", pts: "4 pts" },
                    { crit: "Optimization", desc: "Applies indexes correctly", pts: "2 pts" }
                ]
            },
            Kinematics: {
                course: "General Physics (PHYS101)",
                outcomes: [
                    "LO1: Derive displacement equations for uniform acceleration",
                    "LO2: Solve 2D projectile motion problems",
                    "LO3: Analyze velocity-time graphs"
                ],
                sources: ["Physics_Syllabus.pdf §2.3", "Kinematics_Lab.pdf"],
                retrievedContext: "Kinematics describes the motion of points, bodies, and systems without considering forces. Syllabus focuses on constant acceleration equations.",
                lessonArch90: [
                    "00-15 min: Kinematic Equations Derivation (v = u + at)",
                    "15-40 min: Projectile Motion Experiment Analysis",
                    "40-70 min: Real-World Vehicle Braking Distance Problem",
                    "70-85 min: Interactive Physics Calculations Quiz",
                    "85-90 min: Homework Assignment Walkthrough"
                ],
                code: `# Kinematic Calculation Script\ndef calculate_distance(u, a, t):\n    return u * t + 0.5 * a * (t ** 2)\n\n# Task: Calculate distance for u=0, a=9.8m/s^2, t=5s`,
                quiz: {
                    question: "What is the acceleration of a free-falling body near Earth's surface?",
                    options: ["0 m/s²", "9.8 m/s²", "98 m/s²", "Depends on mass"],
                    correct: 1,
                    feedback: "Correct! Standard acceleration due to gravity is ~9.8 m/s²."
                },
                diff: {
                    foundation: "A — Foundation: Identify variables u, v, a, t, s from word problems.",
                    application: "B — Application: Calculate stopping distance of a car at 60 km/h.",
                    advanced: "C — Advanced: Solve 2D projectile trajectory with launch angles."
                },
                rubric: [
                    { crit: "Formula Application", desc: "Selects correct kinematic equation", pts: "4 pts" },
                    { crit: "Calculation", desc: "Accurate numerical output with units", pts: "4 pts" },
                    { crit: "Graph Analysis", desc: "Interprets v-t graph slopes correctly", pts: "2 pts" }
                ]
            }
        };

        // Render Functions
        function renderTopicContent() {
            const topicKey = document.getElementById('topicSelect').value;
            const data = topicData[topicKey];

            // Update KB Extracted
            document.getElementById('kbCourse').innerText = data.course;
            document.getElementById('kbOutcomes').innerHTML = data.outcomes.map(o => `<li>${o}</li>`).join('');
            document.getElementById('kbSources').innerHTML = data.sources.map(s => `<span class="bg-slate-800 text-brand-300 border border-slate-700 px-2 py-0.5 rounded text-[10px]">${s}</span>`).join('');
            document.getElementById('retrievedContextText').innerText = data.retrievedContext;

            // Package Title
            document.getElementById('packageTitle').innerText = `Educational Package: ${topicKey.replace('_', ' ')}`;

            // Architecture
            document.getElementById('lessonArchitecture').innerHTML = data.lessonArch90.map(step => `<div class="p-2 bg-white rounded border border-slate-100">${step}</div>`).join('');

            // Code Practice
            document.getElementById('codePracticeBlock').innerText = data.code;

            // Quiz
            document.getElementById('quizContainer').innerHTML = `
                <p class="font-semibold text-slate-800">${data.quiz.question}</p>
                <div class="space-y-1.5 mt-2">
                    ${data.quiz.options.map((opt, idx) => `
                        <label class="flex items-center gap-2 p-2 bg-white rounded border border-slate-200 cursor-pointer hover:bg-slate-100">
                            <input type="radio" name="quizOpt" value="${idx}" onchange="checkQuiz(${idx}, ${data.quiz.correct}, '${data.quiz.feedback}')">
                            <span>${opt}</span>
                        </label>
                    `).join('')}
                </div>
                <div id="quizFeedback" class="hidden p-2 rounded text-xs font-semibold"></div>
            `;

            // Differentiated Tasks
            document.getElementById('diffTasksContainer').innerHTML = `
                <div class="p-3 bg-emerald-50 border border-emerald-200 rounded-xl">
                    <span class="font-bold text-emerald-800 block mb-1">🟢 Level A</span>
                    <p class="text-emerald-900">${data.diff.foundation}</p>
                </div>
                <div class="p-3 bg-amber-50 border border-amber-200 rounded-xl">
                    <span class="font-bold text-amber-800 block mb-1">🟡 Level B</span>
                    <p class="text-amber-900">${data.diff.application}</p>
                </div>
                <div class="p-3 bg-rose-50 border border-rose-200 rounded-xl">
                    <span class="font-bold text-rose-800 block mb-1">🔴 Level C</span>
                    <p class="text-rose-900">${data.diff.advanced}</p>
                </div>
            `;

            // Rubric
            document.getElementById('rubricTableBody').innerHTML = data.rubric.map(r => `
                <tr>
                    <td class="p-2.5 font-medium text-slate-800">${r.crit}</td>
                    <td class="p-2.5">${r.desc}</td>
                    <td class="p-2.5 text-right font-bold text-brand-600">${r.pts}</td>
                </tr>
            `).join('');
        }

        function checkQuiz(selected, correct, feedback) {
            const fbDiv = document.getElementById('quizFeedback');
            fbDiv.classList.remove('hidden', 'bg-emerald-100', 'text-emerald-800', 'bg-rose-100', 'text-rose-800');
            if(selected === correct) {
                fbDiv.classList.add('bg-emerald-100', 'text-emerald-800');
                fbDiv.innerText = `✓ ${feedback}`;
            } else {
                fbDiv.classList.add('bg-rose-100', 'text-rose-800');
                fbDiv.innerText = "❌ Қате жауап. Қайтадан байқап көріңіз.";
            }
        }

        function onTopicChange() {
            renderTopicContent();
        }

        // Animated Pipeline Execution
        function startGenerationPipeline() {
            const btn = document.getElementById('btnGenerate');
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i> Processing RAG Pipeline...`;
            btn.disabled = true;

            const step3 = document.getElementById('stepRetrieval');
            const step4 = document.getElementById('stepGeneration');
            const step5 = document.getElementById('stepValidation');

            setTimeout(() => {
                step3.className = "flex items-center gap-2 text-emerald-600 font-medium";
                step3.innerHTML = `<i class="fa-solid fa-circle-check"></i> <span>3. Context retrieved</span>`;
            }, 600);

            setTimeout(() => {
                step4.className = "flex items-center gap-2 text-emerald-600 font-medium";
                step4.innerHTML = `<i class="fa-solid fa-circle-check"></i> <span>4. Educational Package generated</span>`;
            }, 1200);

            setTimeout(() => {
                step5.className = "flex items-center gap-2 text-emerald-600 font-medium";
                step5.innerHTML = `<i class="fa-solid fa-circle-check"></i> <span>5. Quality Check & Alignment</span>`;
                
                renderTopicContent();
                btn.innerHTML = `<i class="fa-solid fa-gears"></i> AI Package Генерациялау`;
                btn.disabled = false;
            }, 1800);
        }

        // Demo Quick Run
        function runDemoScenario() {
            document.getElementById('topicSelect').value = 'Inheritance';
            startGenerationPipeline();
        }

        // Mode Switcher
        function setMode(mode) {
            const teacherBtn = document.getElementById('btnTeacherMode');
            const studentBtn = document.getElementById('btnStudentMode');
            const teacherElements = document.querySelectorAll('.teacher-only');

            if(mode === 'teacher') {
                teacherBtn.className = "px-3 py-1 rounded-md bg-white text-slate-800 shadow-sm transition";
                studentBtn.className = "px-3 py-1 rounded-md text-slate-500 hover:text-slate-800 transition";
                teacherElements.forEach(el => el.style.display = 'block');
            } else {
                studentBtn.className = "px-3 py-1 rounded-md bg-white text-slate-800 shadow-sm transition";
                teacherBtn.className = "px-3 py-1 rounded-md text-slate-500 hover:text-slate-800 transition";
                teacherElements.forEach(el => el.style.display = 'none');
            }
        }


        // External AI tools
        function openGamma() {
            window.open("https://gamma.app/", "_blank", "noopener,noreferrer");
        }

        function openQuizAI() {
            window.open("https://wayground.com/", "_blank", "noopener,noreferrer");
        }

        function exportPackage() {
            alert("📦 Complete Package Exported! (Includes Lesson Plan, Practice, Quiz, Rubric, and Alignment Map in Markdown/PDF format)");
        }

        // Init on load
        window.onload = function() {
            renderTopicContent();
        }
    </script>
</body>
</html>
