<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Treinos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Padrão Masculino (Escuro Fitness) */
            --bg-body: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --input-bg: #0f172a;
            --input-border: #475569;
        }

        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
            --bg-body: #fdf2f8;
            --bg-card: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --border-color: #fbcfe8;
            --primary: #ec4899;
            --primary-hover: #db2777;
            --input-bg: #f8fafc;
            --input-border: #f1f5f9;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            transition: background-color 0.3s ease, border-color 0.3s ease;
        }

        .input-field {
            background-color: var(--input-bg);
            border: 1px solid var(--input-border);
            color: var(--text-main);
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 1px var(--primary);
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
            transition: background-color 0.2s;
        }

        .btn-primary:hover {
            background-color: var(--primary-hover);
        }

        .text-primary {
            color: var(--primary);
        }

        /* Oculta a área de impressão na tela */
        #print-area {
            display: none;
        }

        /* Estilos específicos para a impressão A4 */
        @media print {
            body {
                background: white !important;
                color: black !important;
                margin: 0;
                padding: 0;
            }
            #app-container, .no-print {
                display: none !important;
            }
            #print-area {
                display: block !important;
                padding: 20mm;
                font-family: 'Inter', sans-serif;
                font-size: 12px;
            }
            @page {
                size: A4;
                margin: 0;
            }
            .print-header {
                border-bottom: 2px solid #000;
                padding-bottom: 15px;
                margin-bottom: 20px;
                display: flex;
                justify-content: space-between;
                align-items: flex-end;
            }
            .print-title {
                font-size: 24px;
                font-weight: bold;
                margin: 0;
                text-transform: uppercase;
            }
            .print-grid {
                display: grid;
                grid-template-columns: 1fr 1fr;
                gap: 15px;
                margin-bottom: 25px;
                background: #f9fafb;
                padding: 15px;
                border-radius: 8px;
                border: 1px solid #e5e7eb;
            }
            .print-grid div { margin-bottom: 5px; }
            .print-workout {
                margin-bottom: 30px;
                page-break-inside: avoid;
            }
            .print-workout h3 {
                background: #1f2937;
                color: white;
                padding: 8px 12px;
                margin: 0 0 10px 0;
                font-size: 14px;
                border-radius: 4px;
                text-transform: uppercase;
            }
            table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 10px;
            }
            th, td {
                border: 1px solid #d1d5db;
                padding: 8px;
                text-align: left;
                font-size: 12px;
            }
            th {
                background-color: #f3f4f6;
                font-weight: 600;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            .print-footer {
                margin-top: 40px;
                border-top: 1px solid #e5e7eb;
                padding-top: 20px;
                text-align: center;
                page-break-inside: avoid;
            }
            .print-recommendations {
                background: #fdf8f6;
                border: 1px solid #fed7aa;
                padding: 15px;
                border-radius: 8px;
                margin-bottom: 20px;
            }
        }

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg-body); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- ÁREA PRINCIPAL DO SISTEMA -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-8 gap-4">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs var(--text-muted)">Plataforma Profissional de Prescrição</p>
                </div>
            </div>
            <button onclick="generatePrint()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2">
                <i class="fas fa-print"></i> Imprimir Ficha A4
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Dados do Aluno e Configurações -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-user text-primary"></i> Dados do Aluno
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 25">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 75.5">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Gênero</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Objetivo</label>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>Emagrecimento</option>
                                <option>Hipertrofia</option>
                                <option>Definição</option>
                                <option>Condicionamento</option>
                                <option>Resistência</option>
                                <option>Força</option>
                                <option>Reabilitação</option>
                                <option>Saúde geral</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Frequência Semanal</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Card: Estado de Saúde -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-heartbeat text-primary"></i> Estado de Saúde
                        <span class="text-xs font-normal opacity-60 ml-auto">(Informativo)</span>
                    </h2>
                    <div class="grid grid-cols-2 gap-2 text-sm" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Card: Configurações Finais -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-cog text-primary"></i> Detalhes da Prescrição
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Validade do Treino</label>
                            <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>15 dias</option>
                                <option selected>30 dias</option>
                                <option>60 dias</option>
                                <option>90 dias</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Recomendações (Descanso, Cardio extra, etc.)</label>
                            <textarea id="stu-recs" rows="4" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Escreva as recomendações livres aqui..."></textarea>
                        </div>
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="lg:col-span-8 space-y-6">
                
                <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem Livre
                        </h2>
                        <p class="text-sm opacity-70 mt-1">Adicione treinos e exercícios manualmente. Controle total.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2">
                        <i class="fas fa-plus"></i> Novo Treino
                    </button>
                </div>

                <!-- Container onde os treinos serão renderizados -->
                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos gerados via JS -->
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-60 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-4xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors">
                    <i class="fas fa-times text-2xl"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Categorias -->
                <div class="w-full md:w-1/3 border-r overflow-y-auto p-4 space-y-1" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Categorias via JS -->
                </div>
                <!-- Exercícios -->
                <div class="w-full md:w-2/3 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises">
                    <!-- Lista via JS -->
                </div>
            </div>
        </div>
    </div>


    <!-- ========================================== -->
    <!-- ÁREA DE IMPRESSÃO (Oculta na tela)         -->
    <!-- ========================================== -->
    <div id="print-area">
        <!-- Preenchido via JS antes de imprimir -->
    </div>

    <!-- SCRIPTS -->
    <script>
        // --- BANCO DE DADOS ---
        const db = {
            healthOptions: [
                "Saudável", "Sedentário", "Sobrepeso", "Obesidade", 
                "Diabetes", "Hipertensão", "Problemas cardíacos", 
                "Lesões", "Gestante", "Idoso"
            ],
            categories: {
                "PEITO": ["Supino Reto", "Supino Inclinado", "Supino com Halteres", "Cross Over", "Crucifixo Inclinado", "Peck Fly", "Peck Fly Unilateral", "Pullover"],
                "COSTAS": ["Puxada Alta", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Puxada Unilateral", "Serrote", "Pulldown", "Facepull"],
                "PERNAS": ["Leg Press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira Extensora", "Adução", "Abdução", "Stiff", "Bom Dia", "Sumô", "Levantamento Terra", "Terra Sumô", "Levantamento Terra Romeno"],
                "GLÚTEO": ["Elevação Lateral", "Elevação de Quadril", "Coice", "Cachorrinho", "Caranguejo", "Elevação Pélvica", "Avanço", "Afundo", "Búlgaro", "Panturrilha Banco", "Panturrilha Squat"],
                "BRAÇOS": ["Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Direta", "Rosca 21", "Rosca Alternada", "Rosca Cross", "Rosca Martelo", "Tríceps Pulley", "Tríceps Corda", "Tríceps Coice", "Tríceps Testa", "Tríceps Francês", "Triceps Francês Unilateral"],
                "OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Desenvolvimento Halteres", "Arnold", "Elevação Lateral", "Elevação Borboleta"],
                "ABDÔMEN": ["Infra com Elevação de perna", "Remador", "Prancha"],
                "CARDIO": ["Bicicleta", "Esteira", "Pular Corda"]
            },
            techniques: [
                "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
                "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
                "Negativa", "Isometria", "Parciais", "Pirâmide"
            ]
        };

        // --- ESTADO DA APLICAÇÃO ---
        let state = {
            workouts: [
                { id: generateId(), title: "Treino A", exercises: [] }
            ],
            activeModalWorkoutId: null,
            activeCategory: "PEITO"
        };

        // --- INICIALIZAÇÃO ---
        function init() {
            renderHealthOptions();
            renderWorkouts();
        }

        // --- FUNÇÕES DE INTERFACE ---
        function changeTheme() {
            const gender = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', gender);
        }

        function generateId() {
            return Math.random().toString(36).substr(2, 9);
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = db.healthOptions.map(opt => `
                <label class="flex items-center space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5 transition">
                    <input type="checkbox" value="${opt}" class="health-cb rounded border-gray-400 text-primary focus:ring-primary w-4 h-4">
                    <span>${opt}</span>
                </label>
            `).join('');
        }

        // --- LÓGICA DE TREINOS ---
        function getNextWorkoutLetter() {
            const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
            const currentCount = state.workouts.length;
            return letters[currentCount % 26];
        }

        function addWorkout() {
            const letter = getNextWorkoutLetter();
            state.workouts.push({
                id: generateId(),
                title: `Treino ${letter}`,
                exercises: []
            });
            renderWorkouts();
        }

        function duplicateWorkout(id) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) {
                const newWorkout = JSON.parse(JSON.stringify(workout));
                newWorkout.id = generateId();
                newWorkout.title = newWorkout.title + " (Cópia)";
                state.workouts.push(newWorkout);
                renderWorkouts();
            }
        }

        function removeWorkout(id) {
            if(confirm("Tem certeza que deseja remover este treino inteiro?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        }

        function updateWorkoutTitle(id, newTitle) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) workout.title = newTitle;
        }

        // --- LÓGICA DE EXERCÍCIOS ---
        function removeExercise(workoutId, exIndex) {
            const workout = state.workouts.find(w => w.id === workoutId);
            workout.exercises.splice(exIndex, 1);
            renderWorkouts();
        }

        function moveExercise(workoutId, exIndex, direction) {
            const workout = state.workouts.find(w => w.id === workoutId);
            if (direction === 'up' && exIndex > 0) {
                const temp = workout.exercises[exIndex];
                workout.exercises[exIndex] = workout.exercises[exIndex - 1];
                workout.exercises[exIndex - 1] = temp;
            } else if (direction === 'down' && exIndex < workout.exercises.length - 1) {
                const temp = workout.exercises[exIndex];
                workout.exercises[exIndex] = workout.exercises[exIndex + 1];
                workout.exercises[exIndex + 1] = temp;
            }
            renderWorkouts();
        }

        function updateExercise(workoutId, exIndex, field, value) {
            const workout = state.workouts.find(w => w.id === workoutId);
            if(workout && workout.exercises[exIndex]) {
                workout.exercises[exIndex][field] = value;
            }
        }

        // --- RENDERIZAÇÃO DE TREINOS ---
        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            state.workouts.forEach(workout => {
                const techOptions = db.techniques.map(t => `<option value="${t}">${t}</option>`).join('');

                let exercisesHtml = '';
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-6 text-sm opacity-50 italic">Nenhum exercício adicionado a este treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-3 p-3 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            
                            <!-- Controles de ordem -->
                            <div class="flex sm:flex-col gap-1 hidden sm:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-xs p-1 rounded hover:bg-black hover:bg-opacity-10" title="Subir"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-xs p-1 rounded hover:bg-black hover:bg-opacity-10" title="Descer"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <!-- Nome -->
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-xs opacity-60 uppercase tracking-wider">${ex.category}</div>
                                <div>${ex.name}</div>
                            </div>

                            <!-- Campos Editáveis -->
                            <div class="flex flex-wrap gap-2 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-16 rounded px-2 py-1 text-sm text-center" placeholder="Séries">
                                <span class="self-center opacity-50">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-20 rounded px-2 py-1 text-sm text-center" placeholder="Reps">
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-32 rounded px-2 py-1 text-sm">
                                    ${db.techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-40 rounded px-2 py-1 text-sm" placeholder="Observações (ex: rest 60s)">
                            </div>

                            <!-- Ações -->
                            <div class="flex items-center gap-2 w-full sm:w-auto justify-end mt-2 sm:mt-0">
                                <div class="sm:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="btn-primary text-xs p-2 rounded"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="btn-primary text-xs p-2 rounded"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-2 rounded transition" title="Remover Exercício">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color); background-color: rgba(0,0,0,0.02)">
                            <div class="flex items-center gap-2 w-1/2">
                                <i class="fas fa-dumbbell opacity-50"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-lg w-full px-2 py-1 rounded">
                            </div>
                            <div class="flex gap-2">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-sm p-2 rounded hover:bg-black hover:bg-opacity-10 transition tooltip" title="Duplicar Treino">
                                    <i class="fas fa-copy"></i>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-sm p-2 text-red-500 rounded hover:bg-red-500 hover:text-white transition tooltip" title="Excluir Treino">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <div class="p-3 bg-black bg-opacity-5 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-2 rounded-lg font-medium text-sm border border-dashed border-opacity-50 hover:border-solid transition flex justify-center items-center gap-2">
                                <i class="fas fa-plus-circle"></i> Adicionar Exercício neste Treino
                            </button>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', workoutCard);
            });
        }

        // --- MODAL E BANCO DE EXERCÍCIOS ---
        function openModal(workoutId) {
            state.activeModalWorkoutId = workoutId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderModalCategories();
            renderModalExercises();
        }

        function closeModal() {
            document.getElementById('exercise-modal').classList.add('hidden');
            state.activeModalWorkoutId = null;
        }

        function renderModalCategories() {
            const container = document.getElementById('modal-categories');
            container.innerHTML = Object.keys(db.categories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-4 py-3 rounded-lg text-sm font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white shadow-md' : 'hover:bg-black hover:bg-opacity-10'}">
                    ${cat}
                </button>
            `).join('');
        }

        function setModalCategory(cat) {
            state.activeCategory = cat;
            renderModalCategories();
            renderModalExercises();
        }

        function renderModalExercises() {
            const container = document.getElementById('modal-exercises');
            const exercises = db.categories[state.activeCategory];
            
            container.innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                    ${exercises.map(ex => `
                        <button onclick="addExerciseToWorkout('${ex}')" class="card p-3 rounded-lg text-left text-sm font-medium border hover:border-primary hover:text-primary transition flex justify-between items-center group">
                            ${ex}
                            <i class="fas fa-plus opacity-0 group-hover:opacity-100 transition"></i>
                        </button>
                    `).join('')}
                </div>
            `;
        }

        function addExerciseToWorkout(exerciseName) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                workout.exercises.push({
                    category: state.activeCategory,
                    name: exerciseName,
                    sets: '3',
                    reps: '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                renderWorkouts();
                
                // Feedback visual leve (opcional)
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 150);
            }
        }


        // --- LÓGICA DE IMPRESSÃO PROFISSIONAL A4 ---
        function generatePrint() {
            // Coletar dados do formulário
            const data = {
                name: document.getElementById('stu-name').value || 'Não informado',
                age: document.getElementById('stu-age').value || '-',
                weight: document.getElementById('stu-weight').value || '-',
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value || 'Siga o plano adequadamente.',
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value).join(', ') || 'Nenhuma restrição assinalada'
            };

            const printArea = document.getElementById('print-area');
            
            // Construir HTML da impressão
            let html = `
                <div class="print-header">
                    <div>
                        <h1 class="print-title">Ficha de Treinamento</h1>
                        <p style="margin: 5px 0 0 0; color: #4b5563;">Prescrição Individualizada</p>
                    </div>
                    <div style="text-align: right; font-size: 11px;">
                        Data: ${new Date().toLocaleDateString('pt-BR')}<br>
                        Validade: ${data.validity}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${data.name}</div>
                    <div><strong>Objetivo:</strong> ${data.objective}</div>
                    <div><strong>Idade:</strong> ${data.age} anos | <strong>Peso:</strong> ${data.weight} kg | <strong>Gênero:</strong> ${data.gender}</div>
                    <div><strong>Nível:</strong> ${data.level}</div>
                    <div><strong>Frequência:</strong> ${data.freq}</div>
                    <div><strong>Condições Clínicas:</strong> ${data.health}</div>
                </div>
            `;

            // Adicionar tabelas de treinos
            state.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%">Exercício</th>
                                    <th style="width: 10%">Séries</th>
                                    <th style="width: 15%">Repetições</th>
                                    <th style="width: 15%">Técnica</th>
                                    <th style="width: 25%">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                
                if (w.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center; color:#6b7280; font-style:italic;">Sem exercícios cadastrados</td></tr>`;
                } else {
                    w.exercises.forEach(ex => {
                        const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td>${ex.sets}</td>
                                <td>${ex.reps}</td>
                                <td>${techStr}</td>
                                <td>${ex.obs || '-'}</td>
                            </tr>
                        `;
                    });
                }
                
                html += `
                            </tbody>
                        </table>
                    </div>
                `;
            });

            // Recomendações e Rodapé Obrigatório
            html += `
                <div class="print-recommendations">
                    <h4 style="margin-top:0; margin-bottom: 10px;">Recomendações do Profissional</h4>
                    <p style="white-space: pre-wrap; margin:0; font-size: 11px; line-height: 1.5;">${data.recs}</p>
                </div>

                <div class="print-footer">
                    <p style="font-weight: bold; margin: 0; font-size: 14px;">Lucas André</p>
                    <p style="margin: 2px 0 0 0; color: #4b5563;">Personal Trainer</p>
                    <p style="margin: 2px 0 0 0; font-size: 11px;">CREF: 008094 - G/RN</p>
                </div>
            `;

            printArea.innerHTML = html;
            
            // Disparar impressão
            setTimeout(() => {
                window.print();
            }, 200);
        }

        // Iniciar
        init();

    </script>
</body>
</html>

