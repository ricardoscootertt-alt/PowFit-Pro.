<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional de Prescrição</title>
    <!-- Tailwind CSS para estilização rápida e responsiva -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome para ícones -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        /* ==========================================
           TEMAS E VARIÁVEIS CSS
        ========================================== */
        :root {
            /* Tema Padrão: Masculino (Escuro Fitness) */
            --bg-main: #0f172a;
            --bg-card: #1e293b;
            --bg-input: #0f172a;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #334155;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --danger: #ef4444;
            --success: #10b981;
        }

        [data-theme="Feminino"] {
            /* Tema Feminino (Rosa Moderno Elegante) */
            --bg-main: #fdf2f8;
            --bg-card: #ffffff;
            --bg-input: #f8fafc;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --border-color: #fbcfe8;
            --primary: #ec4899;
            --primary-hover: #db2777;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-main);
            color: var(--text-main);
            transition: background-color 0.3s, color 0.3s;
        }

        /* Classes Customizadas para reaproveitamento */
        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            transition: all 0.3s;
        }

        .input-field {
            background-color: var(--bg-input);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            transition: border-color 0.2s;
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

        .text-primary { color: var(--primary); }
        .border-primary { border-color: var(--primary); }

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: var(--bg-main); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }

        /* ==========================================
           ÁREA DE IMPRESSÃO (A4 Profissional)
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
                font-size: 11pt;
            }
            @page {
                size: A4;
                margin: 0;
            }
            
            /* Tipografia Impressão */
            .p-title { font-size: 24pt; font-weight: 800; text-transform: uppercase; margin: 0; color: #111827; }
            .p-subtitle { font-size: 10pt; color: #6b7280; margin-top: 2px; text-transform: uppercase; letter-spacing: 1px; }
            
            /* Header e Grid Info */
            .p-header { display: flex; justify-content: space-between; align-items: flex-end; border-bottom: 2px solid #000; padding-bottom: 15px; margin-bottom: 20px; }
            .p-info-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; background: #f9fafb; padding: 15px; border-radius: 8px; border: 1px solid #e5e7eb; margin-bottom: 25px; font-size: 10pt; }
            .p-info-grid div span { font-weight: 600; color: #374151; }
            .p-health-box { grid-column: span 2; border-top: 1px solid #e5e7eb; padding-top: 10px; margin-top: 5px; }
            
            /* Treinos e Tabelas */
            .p-workout { margin-bottom: 30px; page-break-inside: avoid; }
            .p-workout-title { background: #111827; color: white; padding: 8px 12px; margin: 0 0 10px 0; font-size: 12pt; border-radius: 4px; text-transform: uppercase; display: inline-block; }
            .p-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; font-size: 10pt; }
            .p-table th, .p-table td { border: 1px solid #d1d5db; padding: 8px 10px; text-align: left; }
            .p-table th { background-color: #f3f4f6; font-weight: 700; color: #374151; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
            .p-table tr:nth-child(even) { background-color: #f9fafb; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
            
            /* Recomendações e Rodapé */
            .p-recs { background: #fdf8f6; border-left: 4px solid #f97316; padding: 12px 15px; border-radius: 0 8px 8px 0; margin-bottom: 30px; font-size: 10pt; page-break-inside: avoid; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
            .p-recs h4 { margin: 0 0 8px 0; color: #c2410c; font-size: 11pt; }
            .p-footer { margin-top: 40px; padding-top: 20px; border-top: 1px solid #e5e7eb; text-align: center; page-break-inside: avoid; }
            .p-footer-name { font-weight: 800; font-size: 14pt; margin: 0; color: #111827; }
            .p-footer-role { font-size: 10pt; color: #4b5563; margin: 2px 0; }
            .p-footer-cref { font-size: 10pt; font-weight: 600; margin: 0; }
        }

        /* Animações */
        .fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- CONTAINER PRINCIPAL -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- HEADER DO SISTEMA -->
        <header class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 border-b pb-6" style="border-color: var(--border-color);">
            <div class="flex items-center gap-4">
                <div class="w-14 h-14 rounded-xl flex items-center justify-center btn-primary text-2xl shadow-lg">
                    <i class="fas fa-dumbbell"></i>
                </div>
                <div>
                    <h1 class="text-3xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-sm var(--text-muted) font-medium opacity-80">Plataforma Profissional de Montagem de Treinos</p>
                </div>
            </div>
            <button onclick="generatePrint()" class="btn-primary px-6 py-3 rounded-xl font-bold shadow-lg flex items-center gap-2 hover:scale-105 transition-transform">
                <i class="fas fa-print text-xl"></i> Imprimir Ficha A4
            </button>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- ==========================================
                 COLUNA ESQUERDA: DADOS DO ALUNO
            ========================================== -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Dados Básicos -->
                <div class="card rounded-2xl p-6 shadow-sm fade-in">
                    <h2 class="text-lg font-bold mb-5 flex items-center gap-2">
                        <i class="fas fa-user-circle text-primary text-xl"></i> Dados do Aluno
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-semibold mb-1 opacity-80">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-xl px-4 py-2.5 text-sm" placeholder="Ex: João da Silva">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-semibold mb-1 opacity-80">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-xl px-4 py-2.5 text-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-sm font-semibold mb-1 opacity-80">Peso (kg)</label>
                                <input type="number" step="0.1" id="stu-weight" class="input-field w-full rounded-xl px-4 py-2.5 text-sm" placeholder="Ex: 80.5">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-semibold mb-1 opacity-80">Gênero (Tema)</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-xl px-4 py-2.5 text-sm font-medium">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-semibold mb-1 opacity-80">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-xl px-4 py-2.5 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-sm font-semibold mb-1 opacity-80">Objetivo</label>
                            <select id="stu-objective" class="input-field w-full rounded-xl px-4 py-2.5 text-sm">
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
                            <label class="block text-sm font-semibold mb-1 opacity-80">Frequência Semanal</label>
                            <select id="stu-freq" class="input-field w-full rounded-xl px-4 py-2.5 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Estado de Saúde -->
                <div class="card rounded-2xl p-6 shadow-sm fade-in" style="animation-delay: 0.1s;">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2">
                        <i class="fas fa-heartbeat text-primary text-xl"></i> Estado de Saúde
                        <span class="text-xs font-normal opacity-50 ml-auto bg-black bg-opacity-10 px-2 py-1 rounded">Informativo</span>
                    </h2>
                    <div class="grid grid-cols-2 gap-3 text-sm" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Configurações e Recomendações -->
                <div class="card rounded-2xl p-6 shadow-sm fade-in" style="animation-delay: 0.2s;">
                    <h2 class="text-lg font-bold mb-4 flex items-center gap-2">
                        <i class="fas fa-clipboard-check text-primary text-xl"></i> Prescrição Final
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-semibold mb-1 opacity-80">Validade da Ficha</label>
                            <select id="stu-validity" class="input-field w-full rounded-xl px-4 py-2.5 text-sm">
                                <option>15 dias</option>
                                <option selected>30 dias</option>
                                <option>60 dias</option>
                                <option>90 dias</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-semibold mb-1 opacity-80">Recomendações Livres</label>
                            <textarea id="stu-recs" rows="5" class="input-field w-full rounded-xl px-4 py-3 text-sm" placeholder="Ex: Beba 3L de água, faça 20min de cardio após o treino..."></textarea>
                        </div>
                    </div>
                </div>

            </div>

            <!-- ==========================================
                 COLUNA DIREITA: MONTAGEM DO TREINO
            ========================================== -->
            <div class="lg:col-span-8 space-y-6">
                
                <!-- Barra de Ferramentas de Treino -->
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-black bg-opacity-5 p-5 rounded-2xl card border-dashed border-2 fade-in">
                    <div>
                        <h2 class="text-2xl font-bold flex items-center gap-2">
                            <i class="fas fa-layer-group text-primary"></i> Montagem Manual
                        </h2>
                        <p class="text-sm opacity-70 mt-1">Crie os treinos e adicione os exercícios com total controle.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-5 py-2.5 rounded-xl text-sm font-bold shadow flex items-center gap-2 mt-4 sm:mt-0 hover:scale-105 transition-transform">
                        <i class="fas fa-plus"></i> NOVO TREINO
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
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 transition-opacity">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh] overflow-hidden">
            <!-- Header do Modal -->
            <div class="p-5 border-b flex justify-between items-center bg-black bg-opacity-10" style="border-color: var(--border-color)">
                <div>
                    <h3 class="text-xl font-bold flex items-center gap-2">
                        <i class="fas fa-list-ul text-primary"></i> Adicionar Exercícios
                    </h3>
                    <p class="text-xs opacity-70 mt-1">Clique nos exercícios para adicionar ao treino selecionado. Você pode adicionar vários.</p>
                </div>
                <button onclick="closeModal()" class="w-10 h-10 rounded-full flex items-center justify-center bg-red-500 text-white hover:bg-red-600 transition-colors shadow">
                    <i class="fas fa-times text-lg"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Coluna de Categorias -->
                <div class="w-full md:w-1/3 border-r overflow-y-auto p-3 space-y-1 bg-black bg-opacity-5" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Gerado via JS -->
                </div>
                <!-- Coluna de Exercícios -->
                <div class="w-full md:w-2/3 overflow-y-auto p-5 relative" id="modal-exercises">
                    <!-- Gerado via JS -->
                </div>
            </div>
            
            <!-- Footer do Modal (Feedback) -->
            <div class="p-3 border-t text-center text-sm font-medium text-primary bg-black bg-opacity-10" style="border-color: var(--border-color)" id="modal-feedback">
                Pronto para adicionar. Selecione uma categoria e clique no exercício.
            </div>
        </div>
    </div>

    <!-- ==========================================
         ÁREA DE IMPRESSÃO (Oculta na tela)
    ========================================== -->
    <div id="print-area">
        <!-- Preenchido dinamicamente via JS antes de imprimir -->
    </div>

    <!-- ==========================================
         LÓGICA JAVASCRIPT
    ========================================== -->
    <script>
        // --- 1. BANCO DE DADOS (Conforme exigido) ---
        const db = {
            healthOptions: [
                "Saudável", "Sedentário", "Sobrepeso", "Obesidade", 
                "Diabetes", "Hipertensão", "Problemas cardíacos", 
                "Lesões", "Gestante", "Idoso"
            ],
            categories: {
                "🔥 PEITO": [
                    "Supino Reto", "Supino Inclinado", "Supino com Halteres", "Supino Máquina", 
                    "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", 
                    "Crucifixo Inclinado", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", 
                    "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
                ],
                "🦍 COSTAS": [
                    "Puxada Alta", "Puxada Frontal", "Puxada Unilateral", "Pulldown", 
                    "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Unilateral", 
                    "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", 
                    "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
                ],
                "🦵 PERNAS": [
                    "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Squat", 
                    "Hack Machine", "Leg Press", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                    "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", 
                    "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                    "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", 
                    "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", 
                    "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", 
                    "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", 
                    "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
                ],
                "💪 BRAÇOS": [
                    "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", 
                    "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", 
                    "Rosca Concentrada", "Rosca Inversa", "Rosca Banco Inclinado", 
                    "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", 
                    "Tríceps Francês na Corda", "Tríceps Francês com Halter", "Tríceps Francês Unilateral", 
                    "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", 
                    "Tríceps Testa", "Mergulho no Banco"
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

        // --- 2. ESTADO DA APLICAÇÃO ---
        let state = {
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- 3. INICIALIZAÇÃO ---
        function init() {
            renderHealthOptions();
            // Inicia com um treino vazio por padrão
            addWorkout();
        }

        // --- 4. FUNÇÕES GERAIS E INTERFACE ---
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
                <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded-lg hover:bg-black hover:bg-opacity-10 transition">
                    <input type="checkbox" value="${opt}" class="health-cb rounded border-gray-400 text-primary focus:ring-primary w-4 h-4">
                    <span class="opacity-90">${opt}</span>
                </label>
            `).join('');
        }

        // --- 5. GERENCIAMENTO DE TREINOS ---
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
                const newWorkout = JSON.parse(JSON.stringify(workout)); // Deep copy
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

        // --- 6. GERENCIAMENTO DE EXERCÍCIOS DENTRO DO TREINO ---
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

        // --- 7. RENDERIZAÇÃO DA LISTA DE TREINOS ---
        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            if(state.workouts.length === 0) {
                container.innerHTML = `<div class="text-center p-10 card rounded-2xl opacity-60 font-medium">Nenhum treino criado. Clique em "Novo Treino" para começar.</div>`;
                return;
            }

            state.workouts.forEach(workout => {
                let exercisesHtml = '';
                
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-8 text-sm opacity-50 italic">Área livre. Adicione exercícios a este treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col xl:flex-row gap-4 p-4 items-start xl:items-center border-b last:border-0 hover:bg-black hover:bg-opacity-5 transition" style="border-color: var(--border-color)">
                            
                            <!-- Controles de Ordem -->
                            <div class="flex xl:flex-col gap-1 hidden xl:flex text-gray-400">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="p-1 hover:text-primary transition" title="Mover para cima"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="p-1 hover:text-primary transition" title="Mover para baixo"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <!-- Info do Exercício -->
                            <div class="flex-1 w-full xl:w-auto">
                                <div class="text-[10px] font-bold opacity-50 tracking-widest uppercase mb-1">${ex.category.replace(/[^\w\sÀ-ÿ]/g, '').trim()}</div>
                                <div class="font-bold text-lg leading-tight">${ex.name}</div>
                            </div>

                            <!-- Campos Editáveis (Séries, Reps, Técnica, Obs) -->
                            <div class="flex flex-wrap lg:flex-nowrap gap-2 w-full xl:w-auto mt-2 xl:mt-0 items-center bg-black bg-opacity-10 p-2 rounded-xl border border-transparent focus-within:border-primary transition-colors">
                                <div class="flex items-center gap-1">
                                    <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-14 rounded-lg px-2 py-1.5 text-sm text-center font-bold" placeholder="Sér">
                                    <span class="opacity-40 text-sm font-bold">X</span>
                                    <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-20 rounded-lg px-2 py-1.5 text-sm text-center font-bold" placeholder="Reps">
                                </div>
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-full sm:w-36 rounded-lg px-2 py-1.5 text-sm">
                                    ${db.techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field w-full sm:w-48 rounded-lg px-3 py-1.5 text-sm" placeholder="Observações livres">
                            </div>

                            <!-- Ações (Mobile Move & Excluir) -->
                            <div class="flex items-center gap-2 w-full xl:w-auto justify-end mt-2 xl:mt-0">
                                <div class="xl:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="bg-black bg-opacity-20 hover:bg-primary hover:text-white text-xs p-2 rounded-lg transition"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="bg-black bg-opacity-20 hover:bg-primary hover:text-white text-xs p-2 rounded-lg transition"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:bg-red-500 hover:text-white p-2.5 rounded-lg transition-colors shadow-sm bg-black bg-opacity-10" title="Remover Exercício">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-2xl overflow-hidden shadow-md fade-in">
                        <!-- Cabeçalho do Treino -->
                        <div class="p-4 border-b flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3 bg-black bg-opacity-20" style="border-color: var(--border-color)">
                            <div class="flex items-center gap-3 w-full sm:w-1/2">
                                <div class="w-10 h-10 rounded-lg bg-primary text-white flex items-center justify-center font-bold shadow">
                                    ${workout.title.split(' ').pop() || '?'}
                                </div>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-extrabold text-xl w-full px-2 py-1 rounded-lg border-transparent focus:border-primary">
                            </div>
                            <div class="flex gap-2 w-full sm:w-auto justify-end">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-sm px-3 py-2 rounded-lg hover:bg-primary hover:text-white bg-black bg-opacity-10 transition font-medium flex items-center gap-2">
                                    <i class="fas fa-copy"></i> <span class="hidden sm:inline">Duplicar</span>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-sm px-3 py-2 text-red-500 rounded-lg hover:bg-red-500 hover:text-white bg-black bg-opacity-10 transition font-medium flex items-center gap-2">
                                    <i class="fas fa-trash"></i> <span class="hidden sm:inline">Excluir</span>
                                </button>
                            </div>
                        </div>
                        
                        <!-- Lista de Exercícios -->
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <!-- Botão Adicionar -->
                        <div class="p-4 bg-black bg-opacity-10 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-3 rounded-xl font-bold text-sm border-2 border-dashed border-white border-opacity-30 hover:border-solid transition-all flex justify-center items-center gap-2">
                                <i class="fas fa-plus-circle text-lg"></i> ADICIONAR EXERCÍCIO AO ${workout.title.toUpperCase()}
                            </button>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', workoutCard);
            });
        }

        // --- 8. LÓGICA DO MODAL (BANCO DE EXERCÍCIOS) ---
        function openModal(workoutId) {
            state.activeModalWorkoutId = workoutId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderModalCategories();
            renderModalExercises();
            document.getElementById('modal-feedback').innerHTML = `Selecionando exercícios para: <strong>${state.workouts.find(w=>w.id === workoutId).title}</strong>`;
        }

        function closeModal() {
            document.getElementById('exercise-modal').classList.add('hidden');
            state.activeModalWorkoutId = null;
        }

        function renderModalCategories() {
            const container = document.getElementById('modal-categories');
            container.innerHTML = Object.keys(db.categories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-4 py-3.5 rounded-xl text-sm font-bold transition-all ${state.activeCategory === cat ? 'bg-primary text-white shadow-md translate-x-1' : 'hover:bg-black hover:bg-opacity-10 opacity-70 hover:opacity-100'}">
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
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3">
                    ${exercises.map(ex => `
                        <button onclick="addExerciseToWorkout('${ex}')" class="card p-4 rounded-xl text-left text-sm font-semibold border-2 border-transparent hover:border-primary hover:text-primary transition-all flex justify-between items-center group shadow-sm bg-opacity-50">
                            <span>${ex}</span>
                            <div class="w-6 h-6 rounded-full bg-primary text-white flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity scale-75 group-hover:scale-100">
                                <i class="fas fa-plus text-xs"></i>
                            </div>
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
                
                // Feedback visual de sucesso sem fechar o modal
                const feedback = document.getElementById('modal-feedback');
                feedback.innerHTML = `<span class="text-success"><i class="fas fa-check-circle"></i> <strong>${exerciseName}</strong> adicionado com sucesso! Adicione mais ou feche a janela.</span>`;
                setTimeout(() => {
                    if(state.activeModalWorkoutId) {
                        feedback.innerHTML = `Selecionando exercícios para: <strong>${workout.title}</strong>`;
                    }
                }, 3000);
            }
        }

        // --- 9. MOTOR DE IMPRESSÃO (GERAÇÃO DA FICHA A4) ---
        function generatePrint() {
            // 1. Coletar Dados do Formulário
            const data = {
                name: document.getElementById('stu-name').value || 'Não informado',
                age: document.getElementById('stu-age').value || '-',
                weight: document.getElementById('stu-weight').value || '-',
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value || 'Nenhuma recomendação adicional.',
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value).join(', ') || 'Nenhuma condição assinalada'
            };

            const printArea = document.getElementById('print-area');
            
            // 2. Construir Cabeçalho e Informações
            let html = `
                <div class="p-header">
                    <div>
                        <h1 class="p-title">Ficha de Treinamento</h1>
                        <p class="p-subtitle">Prescrição Individualizada</p>
                    </div>
                    <div style="text-align: right;">
                        <strong>Data:</strong> ${new Date().toLocaleDateString('pt-BR')}<br>
                        <strong>Validade:</strong> ${data.validity}
                    </div>
                </div>

                <div class="p-info-grid">
                    <div><span>Aluno(a):</span> ${data.name}</div>
                    <div><span>Objetivo:</span> ${data.objective}</div>
                    <div><span>Idade:</span> ${data.age} anos | <span>Peso:</span> ${data.weight} kg | <span>Gênero:</span> ${data.gender}</div>
                    <div><span>Nível:</span> ${data.level} | <span>Frequência:</span> ${data.freq}</div>
                    <div class="p-health-box"><span>Condições Clínicas / Estado de Saúde:</span> ${data.health}</div>
                </div>
            `;

            // 3. Construir as Tabelas de Treino
            state.workouts.forEach(w => {
                html += `
                    <div class="p-workout">
                        <h3 class="p-workout-title">${w.title}</h3>
                        <table class="p-table">
                            <thead>
                                <tr>
                                    <th style="width: 35%">Exercício</th>
                                    <th style="width: 10%; text-align: center;">Séries</th>
                                    <th style="width: 15%; text-align: center;">Repetições</th>
                                    <th style="width: 15%">Técnica</th>
                                    <th style="width: 25%">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                
                if (w.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center; color:#6b7280; font-style:italic;">Nenhum exercício prescrito neste bloco.</td></tr>`;
                } else {
                    w.exercises.forEach(ex => {
                        const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td style="text-align: center; font-weight: bold;">${ex.sets}</td>
                                <td style="text-align: center; font-weight: bold;">${ex.reps}</td>
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

            // 4. Construir Recomendações e Rodapé Profissional
            html += `
                <div class="p-recs">
                    <h4><i class="fas fa-exclamation-circle"></i> Recomendações e Observações Gerais</h4>
                    <p style="white-space: pre-wrap; margin:0; line-height: 1.6;">${data.recs}</p>
                </div>

                <div class="p-footer">
                    <p class="p-footer-name">LUIZ ANDRÉ</p>
                    <p class="p-footer-role">Personal Trainer</p>
                    <p class="p-footer-cref">CREF: 008094 - G/RN</p>
                </div>
            `;

            printArea.innerHTML = html;
            
            // 5. Disparar a impressão do navegador após montar o DOM
            setTimeout(() => {
                window.print();
            }, 300);
        }

        // Inicia a aplicação
        window.onload = init;

    </script>
</body>
</html>
