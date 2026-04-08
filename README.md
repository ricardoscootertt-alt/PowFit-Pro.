<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Treinos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* ==========================================
           SISTEMA DE TEMAS (Variáveis CSS)
           ========================================== */
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
            --header-bg: #1e293b;
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
            --input-border: #e2e8f0;
            --header-bg: #fce7f3;
        }

        /* ==========================================
           ESTILOS GERAIS
           ========================================== */
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
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        
        .input-field:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 1px var(--primary);
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
            transition: background-color 0.2s, transform 0.1s;
        }

        .btn-primary:hover {
            background-color: var(--primary-hover);
        }
        
        .btn-primary:active {
            transform: scale(0.98);
        }

        .text-primary { color: var(--primary); }
        .bg-primary-light { background-color: var(--primary); opacity: 0.1; }

        /* Animação para adição de exercício */
        @keyframes flashAdd {
            0% { background-color: transparent; }
            50% { background-color: var(--primary); color: white; }
            100% { background-color: transparent; }
        }
        .flash-animation { animation: flashAdd 0.4s ease-out; }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }

        /* ==========================================
           ESTILOS DE IMPRESSÃO PROFISSIONAL A4
           ========================================== */
        #print-area { display: none; }

        @media print {
            body {
                background: white !important;
                color: black !important;
                margin: 0;
                padding: 0;
            }
            #app-container, #exercise-modal {
                display: none !important;
            }
            #print-area {
                display: block !important;
                padding: 15mm 20mm;
                font-family: 'Inter', sans-serif;
                font-size: 11px;
                color: #111827;
            }
            @page {
                size: A4;
                margin: 0;
            }
            
            .print-header {
                display: flex;
                justify-content: space-between;
                align-items: center;
                border-bottom: 2px solid #111827;
                padding-bottom: 10px;
                margin-bottom: 15px;
            }
            .print-header h1 {
                font-size: 22px;
                font-weight: 800;
                margin: 0;
                text-transform: uppercase;
                letter-spacing: 1px;
            }
            .print-header p { margin: 0; font-size: 10px; color: #4b5563; }
            
            .print-patient-info {
                display: grid;
                grid-template-columns: repeat(3, 1fr);
                gap: 8px;
                background: #f9fafb;
                padding: 12px;
                border-radius: 6px;
                border: 1px solid #e5e7eb;
                margin-bottom: 20px;
            }
            .print-patient-info div { margin-bottom: 4px; }
            .print-patient-info strong { color: #374151; }
            .col-span-full { grid-column: 1 / -1; }

            .print-workout {
                margin-bottom: 20px;
                page-break-inside: avoid;
            }
            .print-workout-title {
                background: #111827;
                color: white;
                padding: 6px 12px;
                margin: 0;
                font-size: 14px;
                font-weight: bold;
                border-top-left-radius: 6px;
                border-top-right-radius: 6px;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            
            table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 0;
                border-bottom-left-radius: 6px;
                border-bottom-right-radius: 6px;
                overflow: hidden;
            }
            th, td {
                border: 1px solid #d1d5db;
                padding: 6px 8px;
                text-align: left;
            }
            th {
                background-color: #f3f4f6;
                font-weight: 600;
                font-size: 10px;
                text-transform: uppercase;
                color: #4b5563;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            td { font-size: 11px; }
            
            .print-recommendations {
                margin-top: 20px;
                background: #fdf8f6;
                border: 1px solid #fed7aa;
                padding: 12px;
                border-radius: 6px;
                page-break-inside: avoid;
            }
            .print-recommendations h4 { margin: 0 0 5px 0; font-size: 12px; color: #9a3412; }
            .print-recommendations p { margin: 0; white-space: pre-wrap; font-size: 10px; color: #431407; }

            .print-footer {
                margin-top: 30px;
                padding-top: 15px;
                border-top: 1px solid #d1d5db;
                text-align: center;
                page-break-inside: avoid;
            }
            .print-footer .prof-name { font-weight: bold; font-size: 14px; margin: 0; text-transform: uppercase; }
            .print-footer .prof-title { margin: 2px 0; color: #4b5563; font-size: 11px; }
            .print-footer .prof-cref { margin: 0; font-weight: bold; font-size: 11px; color: #111827; }
        }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- ==========================================
         INTERFACE PRINCIPAL DO SISTEMA
         ========================================== -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Cabeçalho -->
        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-8 gap-4 pb-4 border-b border-opacity-20" style="border-color: var(--border-color)">
            <div class="flex items-center gap-3">
                <div class="w-12 h-12 rounded-xl flex items-center justify-center bg-primary text-white text-2xl shadow-lg">
                    <i class="fas fa-dumbbell"></i>
                </div>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs var(--text-muted)">Plataforma Profissional de Prescrição</p>
                </div>
            </div>
            <button onclick="generatePrint()" class="btn-primary w-full sm:w-auto px-6 py-3 rounded-xl font-medium shadow-lg flex items-center justify-center gap-2 text-sm">
                <i class="fas fa-print"></i> Gerar Ficha para Impressão
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- ==========================================
                 COLUNA ESQUERDA: DADOS DO ALUNO
                 ========================================== -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Dados Pessoais -->
                <div class="card rounded-2xl p-5 shadow-sm">
                    <h2 class="text-sm font-bold uppercase tracking-wider mb-4 flex items-center gap-2 text-primary">
                        <i class="fas fa-user-circle"></i> Perfil do Aluno
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-80">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2.5 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-80">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2.5 text-sm" placeholder="Ex: 28">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-80">Peso (kg)</label>
                                <input type="number" id="stu-weight" step="0.1" class="input-field w-full rounded-lg px-3 py-2.5 text-sm" placeholder="Ex: 75.5">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-80">Gênero (Muda o Tema)</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2.5 text-sm font-medium">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-80">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2.5 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-80">Objetivo Principal</label>
                            <select id="stu-objective" class="input-field w-full rounded-lg px-3 py-2.5 text-sm">
                                <option>Hipertrofia</option>
                                <option>Emagrecimento</option>
                                <option>Definição</option>
                                <option>Condicionamento</option>
                                <option>Resistência</option>
                                <option>Força</option>
                                <option>Reabilitação</option>
                                <option>Saúde geral</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-80">Frequência Semanal</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2.5 text-sm">
                                <option>3 dias</option>
                                <option>4 dias</option>
                                <option selected>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Estado de Saúde -->
                <div class="card rounded-2xl p-5 shadow-sm">
                    <h2 class="text-sm font-bold uppercase tracking-wider mb-4 flex items-center gap-2 text-primary">
                        <i class="fas fa-notes-medical"></i> Estado de Saúde
                    </h2>
                    <p class="text-xs opacity-60 mb-3">Apenas informativo. Marque as opções aplicáveis.</p>
                    <div class="grid grid-cols-2 gap-y-2 gap-x-1 text-sm" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Configurações e Recomendações -->
                <div class="card rounded-2xl p-5 shadow-sm">
                    <h2 class="text-sm font-bold uppercase tracking-wider mb-4 flex items-center gap-2 text-primary">
                        <i class="fas fa-clipboard-check"></i> Detalhes da Ficha
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-80">Validade do Treino</label>
                            <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2.5 text-sm">
                                <option>15 dias</option>
                                <option selected>30 dias</option>
                                <option>45 dias</option>
                                <option>60 dias</option>
                                <option>90 dias</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-80">Recomendações (Descanso, Cardio, Hidratação...)</label>
                            <textarea id="stu-recs" rows="4" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Beber 3L de água por dia. Fazer 20 min de cardio após o treino."></textarea>
                        </div>
                    </div>
                </div>

            </div>

            <!-- ==========================================
                 COLUNA DIREITA: MONTAGEM DOS TREINOS
                 ========================================== -->
            <div class="lg:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-center bg-opacity-10 p-5 rounded-2xl card border-dashed border-2 gap-4" style="border-color: var(--primary)">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            Área de Montagem Livre
                        </h2>
                        <p class="text-sm opacity-70 mt-1">Crie treinos, adicione exercícios e organize conforme sua metodologia.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary w-full sm:w-auto px-5 py-2.5 rounded-xl text-sm font-bold flex items-center justify-center gap-2">
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

    <!-- ==========================================
         MODAL DE BANCO DE EXERCÍCIOS
         ========================================== -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-2 sm:p-4 opacity-0 transition-opacity duration-300">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col h-[90vh] sm:h-[85vh] transform scale-95 transition-transform duration-300" id="modal-content">
            
            <div class="p-4 sm:p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <div>
                    <h3 class="text-lg sm:text-xl font-bold flex items-center gap-2">
                        <i class="fas fa-search text-primary"></i> Banco de Exercícios
                    </h3>
                    <p class="text-xs opacity-60 mt-1">Clique nos exercícios para adicioná-los diretamente ao treino.</p>
                </div>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 bg-black bg-opacity-10 hover:bg-opacity-20 w-8 h-8 rounded-full flex items-center justify-center transition-colors">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Categorias -->
                <div class="w-full md:w-1/4 border-b md:border-b-0 md:border-r overflow-x-auto md:overflow-y-auto p-3 flex md:flex-col gap-2" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Categorias via JS -->
                </div>
                <!-- Exercícios -->
                <div class="w-full md:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5 relative" id="modal-exercises">
                    <!-- Toast Notification inside modal -->
                    <div id="add-toast" class="absolute top-4 left-1/2 transform -translate-x-1/2 bg-green-500 text-white px-4 py-2 rounded-full text-sm font-bold shadow-lg opacity-0 transition-opacity pointer-events-none z-10 flex items-center gap-2">
                        <i class="fas fa-check-circle"></i> Adicionado!
                    </div>
                    <!-- Lista via JS -->
                    <div id="exercises-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2 pb-10"></div>
                </div>
            </div>
            
            <div class="p-4 border-t flex justify-end" style="border-color: var(--border-color)">
                <button onclick="closeModal()" class="btn-primary px-6 py-2 rounded-xl text-sm font-bold">
                    Concluir Seleção
                </button>
            </div>
        </div>
    </div>


    <!-- ==========================================
         ÁREA DE IMPRESSÃO (Invisível na tela)
         ========================================== -->
    <div id="print-area">
        <!-- Preenchido via JS antes de imprimir -->
    </div>

    <!-- ==========================================
         SCRIPTS E LÓGICA DO SISTEMA
         ========================================== -->
    <script>
        // --- BANCO DE DADOS ATUALIZADO ---
        const db = {
            healthOptions: [
                "Saudável", "Sedentário", "Sobrepeso", "Obesidade", 
                "Diabetes", "Hipertensão", "Problemas cardíacos", 
                "Lesões", "Gestante", "Idoso"
            ],
            categories: {
                "🔥 PEITO": [
                    "Supino Reto", "Supino Inclinado", "Supino com Halteres", "Cross Over", 
                    "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado", 
                    "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                    "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
                ],
                "🦍 COSTAS": [
                    "Puxada Alta", "Puxada Frontal", "Puxada Unilateral", "Pulldown", 
                    "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Unilateral", 
                    "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", 
                    "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
                ],
                "🦵 PERNAS": [
                    "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", 
                    "Squat", "Hack Machine", "Leg Press", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                    "Afundo", "Passada", "Avanço", "Búlgaro", "Step-up", "Levantamento Terra", 
                    "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", 
                    "Cadeira Flexora", "Elevação Pélvica", "Extensão de Quadril (Glúteo Máximo)", 
                    "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", 
                    "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", 
                    "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", 
                    "Panturrilha em Pé (Máquina)", "Panturrilha no Leg Press", "Panturrilha Banco", 
                    "Panturrilha Squat", "Panturrilha Unilateral"
                ],
                "💪 BRAÇOS": [
                    "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", 
                    "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", 
                    "Rosca Cross", "Rosca Concentrada", "Rosca Inversa", "Rosca Banco Inclinado",
                    "Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", 
                    "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", 
                    "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", 
                    "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"
                ],
                "🪨 OMBROS": [
                    "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", 
                    "Elevação Lateral na Polia", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", 
                    "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", 
                    "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", 
                    "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta"
                ],
                "🧠 ABDÔMEN": [
                    "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", 
                    "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", 
                    "Prancha Lateral", "Trituração de Cabos em Pé"
                ],
                "🫀 CARDIO": [
                    "Bicicleta", "Esteira", "Pular Corda", "Isometria na Parede"
                ]
            },
            techniques: [
                "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
                "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
                "Negativa", "Isometria", "Parciais", "Pirâmide"
            ]
        };

        // --- ESTADO GLOBAL DA APLICAÇÃO ---
        let state = {
            workouts: [
                { id: generateId(), title: "Treino A", exercises: [] }
            ],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- INICIALIZAÇÃO ---
        function init() {
            renderHealthOptions();
            renderWorkouts();
        }

        // --- FUNÇÕES DE UTILIDADE E INTERFACE ---
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
                <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded-lg hover:bg-black hover:bg-opacity-5 transition">
                    <input type="checkbox" value="${opt}" class="health-cb rounded border-gray-400 text-primary focus:ring-primary w-4 h-4">
                    <span>${opt}</span>
                </label>
            `).join('');
        }

        // --- LÓGICA DE GERENCIAMENTO DE TREINOS ---
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
            if(confirm("Confirma a exclusão deste treino e todos os seus exercícios?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        }

        function updateWorkoutTitle(id, newTitle) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) workout.title = newTitle;
        }

        // --- LÓGICA DE GERENCIAMENTO DE EXERCÍCIOS ---
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

        // --- RENDERIZAÇÃO DOS TREINOS NA TELA ---
        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            if (state.workouts.length === 0) {
                container.innerHTML = `<div class="text-center p-10 opacity-50 border-2 border-dashed rounded-xl" style="border-color: var(--border-color)">Nenhum treino criado. Clique em "Novo Treino" para começar.</div>`;
                return;
            }

            state.workouts.forEach(workout => {
                let exercisesHtml = '';
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-8 text-sm opacity-50 italic bg-black bg-opacity-5 rounded-lg m-3">Nenhum exercício neste treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-3 p-4 items-start sm:items-center border-b last:border-0 border-opacity-10 hover:bg-black hover:bg-opacity-[0.02] transition" style="border-color: var(--border-color)">
                            
                            <!-- Botões de ordenação -->
                            <div class="flex sm:flex-col gap-1 hidden sm:flex opacity-50 hover:opacity-100 transition">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-xs p-1 rounded hover:bg-gray-300 dark:hover:bg-gray-700" title="Mover para cima"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-xs p-1 rounded hover:bg-gray-300 dark:hover:bg-gray-700" title="Mover para baixo"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <!-- Nome do Exercício -->
                            <div class="flex-1 font-medium w-full sm:w-1/3">
                                <div class="text-[10px] font-bold opacity-60 uppercase tracking-wider mb-0.5" style="color: var(--primary)">${ex.category.replace(/[^a-zA-ZÀ-ÿ\s]/g, '')}</div>
                                <div class="leading-tight text-sm">${ex.name}</div>
                            </div>

                            <!-- Inputs Editáveis -->
                            <div class="flex flex-wrap sm:flex-nowrap gap-2 w-full sm:flex-1">
                                <div class="flex items-center gap-1">
                                    <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 sm:w-16 rounded-md px-2 py-1.5 text-sm text-center" placeholder="Séries">
                                    <span class="opacity-50 text-xs px-1">x</span>
                                    <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 sm:w-20 rounded-md px-2 py-1.5 text-sm text-center" placeholder="Reps">
                                </div>
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-full sm:w-32 rounded-md px-2 py-1.5 text-sm">
                                    ${db.techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field w-full sm:flex-1 rounded-md px-2 py-1.5 text-sm" placeholder="Observações livres...">
                            </div>

                            <!-- Ações Extras (Mobile + Delete) -->
                            <div class="flex items-center gap-2 w-full sm:w-auto justify-end mt-2 sm:mt-0">
                                <div class="sm:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="bg-gray-200 dark:bg-gray-700 text-xs px-3 py-1.5 rounded"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="bg-gray-200 dark:bg-gray-700 text-xs px-3 py-1.5 rounded"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:bg-red-500 hover:text-white w-8 h-8 rounded-md flex items-center justify-center transition" title="Remover Exercício">
                                    <i class="fas fa-trash-alt"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-2xl overflow-hidden shadow-sm">
                        <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color); background-color: var(--header-bg);">
                            <div class="flex items-center gap-3 w-2/3">
                                <div class="w-8 h-8 rounded bg-black bg-opacity-10 flex items-center justify-center text-primary">
                                    <i class="fas fa-list-ol"></i>
                                </div>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-lg w-full px-2 py-1 rounded border-transparent focus:border-primary focus:bg-opacity-50">
                            </div>
                            <div class="flex gap-2">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-sm px-3 py-1.5 rounded-lg bg-black bg-opacity-5 hover:bg-opacity-10 transition font-medium flex items-center gap-2" title="Duplicar Treino Inteiro">
                                    <i class="fas fa-copy"></i> <span class="hidden sm:inline">Duplicar</span>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-sm px-3 py-1.5 text-red-500 rounded-lg bg-red-500 bg-opacity-10 hover:bg-opacity-20 transition font-medium" title="Excluir Treino">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <div class="p-4 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full py-3 rounded-xl font-bold text-sm border-2 border-dashed border-opacity-50 hover:border-solid hover:bg-primary hover:text-white hover:border-primary transition flex justify-center items-center gap-2 text-primary" style="border-color: var(--primary)">
                                <i class="fas fa-plus-circle"></i> Adicionar Exercício neste Treino
                            </button>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', workoutCard);
            });
        }

        // --- LÓGICA DO MODAL (BANCO DE EXERCÍCIOS) ---
        function openModal(workoutId) {
            state.activeModalWorkoutId = workoutId;
            const modal = document.getElementById('exercise-modal');
            const content = document.getElementById('modal-content');
            
            modal.classList.remove('hidden');
            // Pequeno delay para animação de fade in e scale
            setTimeout(() => {
                modal.classList.remove('opacity-0');
                content.classList.remove('scale-95');
                content.classList.add('scale-100');
            }, 10);
            
            renderModalCategories();
            renderModalExercises();
        }

        function closeModal() {
            const modal = document.getElementById('exercise-modal');
            const content = document.getElementById('modal-content');
            
            modal.classList.add('opacity-0');
            content.classList.remove('scale-100');
            content.classList.add('scale-95');
            
            setTimeout(() => {
                modal.classList.add('hidden');
                state.activeModalWorkoutId = null;
            }, 300);
        }

        function renderModalCategories() {
            const container = document.getElementById('modal-categories');
            container.innerHTML = Object.keys(db.categories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="whitespace-nowrap md:whitespace-normal w-full text-left px-4 py-3 rounded-xl text-sm font-bold transition ${state.activeCategory === cat ? 'bg-primary text-white shadow-md' : 'hover:bg-black hover:bg-opacity-10 opacity-70'}">
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
            const container = document.getElementById('exercises-grid');
            const exercises = db.categories[state.activeCategory];
            
            container.innerHTML = exercises.map((ex, idx) => `
                <button id="btn-ex-${idx}" onclick="addExerciseToWorkout('${ex}', 'btn-ex-${idx}')" class="card p-3 rounded-xl text-left text-sm font-medium border hover:border-primary transition flex justify-between items-center group relative overflow-hidden">
                    <span class="relative z-10">${ex}</span>
                    <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100 transition relative z-10"></i>
                </button>
            `).join('');
        }

        function addExerciseToWorkout(exerciseName, btnId) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                // Adiciona ao estado
                workout.exercises.push({
                    category: state.activeCategory,
                    name: exerciseName,
                    sets: '3',
                    reps: '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                
                // Atualiza a tela de treinos no fundo
                renderWorkouts();
                
                // Feedback visual no botão clicado
                const btn = document.getElementById(btnId);
                btn.classList.add('flash-animation');
                setTimeout(() => btn.classList.remove('flash-animation'), 400);

                // Feedback visual de Toast
                const toast = document.getElementById('add-toast');
                toast.classList.remove('opacity-0');
                toast.classList.add('opacity-100');
                
                // Reseta o toast após 1.5s
                clearTimeout(window.toastTimeout);
                window.toastTimeout = setTimeout(() => {
                    toast.classList.remove('opacity-100');
                    toast.classList.add('opacity-0');
                }, 1500);
            }
        }


        // --- LÓGICA DE IMPRESSÃO PROFISSIONAL A4 ---
        function generatePrint() {
            // Coletar dados
            const data = {
                name: document.getElementById('stu-name').value || 'Não informado',
                age: document.getElementById('stu-age').value || '--',
                weight: document.getElementById('stu-weight').value || '--',
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value || 'Siga o planejamento respeitando os intervalos e execuções.',
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value).join(', ') || 'Nenhuma condição reportada'
            };

            const printArea = document.getElementById('print-area');
            
            // Construir HTML da Impressão
            let html = `
                <div class="print-header">
                    <div>
                        <h1>Plano de Treinamento</h1>
                        <p>PRESCRIÇÃO INDIVIDUALIZADA E EXCLUSIVA</p>
                    </div>
                    <div style="text-align: right;">
                        <div style="font-size: 10px; color: #6b7280;">Data de Emissão</div>
                        <div style="font-weight: bold;">${new Date().toLocaleDateString('pt-BR')}</div>
                        <div style="font-size: 10px; color: #6b7280; margin-top: 5px;">Validade</div>
                        <div style="font-weight: bold;">${data.validity}</div>
                    </div>
                </div>

                <div class="print-patient-info">
                    <div><strong>Aluno(a):</strong> ${data.name}</div>
                    <div><strong>Idade:</strong> ${data.age} anos</div>
                    <div><strong>Peso:</strong> ${data.weight} kg</div>
                    
                    <div><strong>Objetivo:</strong> ${data.objective}</div>
                    <div><strong>Nível:</strong> ${data.level}</div>
                    <div><strong>Frequência:</strong> ${data.freq}</div>
                    
                    <div class="col-span-full" style="margin-top: 4px; border-top: 1px solid #e5e7eb; padding-top: 8px;">
                        <strong>Condições de Saúde / Observações:</strong> ${data.health}
                    </div>
                </div>
            `;

            // Construir Tabelas de Treino
            if (state.workouts.length === 0) {
                html += `<div style="text-align:center; padding: 20px; font-style:italic;">Nenhum treino prescrito.</div>`;
            } else {
                state.workouts.forEach(w => {
                    html += `
                        <div class="print-workout">
                            <h3 class="print-workout-title">${w.title.toUpperCase()}</h3>
                            <table>
                                <thead>
                                    <tr>
                                        <th style="width: 35%">Exercício</th>
                                        <th style="width: 10%; text-align: center;">Séries</th>
                                        <th style="width: 15%; text-align: center;">Repetições</th>
                                        <th style="width: 15%; text-align: center;">Técnica</th>
                                        <th style="width: 25%">Observações</th>
                                    </tr>
                                </thead>
                                <tbody>
                    `;
                    
                    if (w.exercises.length === 0) {
                        html += `<tr><td colspan="5" style="text-align:center; color:#6b7280; font-style:italic; padding: 15px;">Treino sem exercícios cadastrados</td></tr>`;
                    } else {
                        w.exercises.forEach(ex => {
                            const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                            html += `
                                <tr>
                                    <td><strong>${ex.name}</strong></td>
                                    <td style="text-align: center;">${ex.sets}</td>
                                    <td style="text-align: center;">${ex.reps}</td>
                                    <td style="text-align: center;">${techStr}</td>
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
            }

            // Recomendações Livres e Rodapé Profissional (Obrigatório)
            html += `
                <div class="print-recommendations">
                    <h4>Recomendações e Cuidados</h4>
                    <p>${data.recs}</p>
                </div>

                <div class="print-footer">
                    <p class="prof-name">Luiz André</p>
                    <p class="prof-title">Personal Trainer</p>
                    <p class="prof-cref">CREF: 008094 - G/RN</p>
                </div>
            `;

            printArea.innerHTML = html;
            
            // Timeout para garantir renderização do DOM antes de chamar print
            setTimeout(() => {
                window.print();
            }, 300);
        }

        // --- INICIAR APLICAÇÃO ---
        init();

    </script>
</body>
</html>
