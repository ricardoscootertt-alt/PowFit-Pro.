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
            --health-bg: #0f172a;
            --health-hover: #1e293b;
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
            --health-bg: #fdf2f8;
            --health-hover: #fce7f3;
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

        .health-card {
            background-color: var(--health-bg);
            border: 1px solid var(--border-color);
            transition: all 0.2s;
        }
        
        .health-card:hover {
            background-color: var(--health-hover);
        }

        .health-cb:checked + div {
            border-color: var(--primary);
            box-shadow: 0 0 0 1px var(--primary);
        }

        /* Oculta a área de impressão na tela */
        #print-area {
            display: none;
        }

        /* Toast Notification */
        #toast {
            visibility: hidden;
            min-width: 250px;
            background-color: #10b981;
            color: #fff;
            text-align: center;
            border-radius: 8px;
            padding: 12px;
            position: fixed;
            z-index: 100;
            bottom: 30px;
            right: 30px;
            font-size: 14px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            opacity: 0;
            transition: opacity 0.3s, visibility 0.3s;
        }

        #toast.show {
            visibility: visible;
            opacity: 1;
        }

        /* Estilos específicos para a impressão A4 */
        @media print {
            body {
                background: white !important;
                color: black !important;
                margin: 0;
                padding: 0;
            }
            #app-container, .no-print, #toast, #exercise-modal {
                display: none !important;
            }
            #print-area {
                display: block !important;
                padding: 15mm;
                font-family: 'Inter', sans-serif;
                font-size: 12px;
            }
            @page {
                size: A4;
                margin: 0;
            }
            .print-header {
                border-bottom: 2px solid #000;
                padding-bottom: 10px;
                margin-bottom: 15px;
                display: flex;
                justify-content: space-between;
                align-items: flex-end;
            }
            .print-title {
                font-size: 22px;
                font-weight: bold;
                margin: 0;
                text-transform: uppercase;
            }
            .print-grid {
                display: grid;
                grid-template-columns: 1fr 1fr;
                gap: 10px;
                margin-bottom: 20px;
                background: #f9fafb;
                padding: 12px;
                border-radius: 8px;
                border: 1px solid #e5e7eb;
            }
            .print-grid div { margin-bottom: 4px; font-size: 11px; }
            .print-workout {
                margin-bottom: 20px;
                page-break-inside: avoid;
            }
            .print-workout h3 {
                background: #1f2937;
                color: white;
                padding: 6px 10px;
                margin: 0 0 8px 0;
                font-size: 13px;
                border-radius: 4px;
                text-transform: uppercase;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 10px;
            }
            th, td {
                border: 1px solid #d1d5db;
                padding: 6px 8px;
                text-align: left;
                font-size: 11px;
            }
            th {
                background-color: #f3f4f6;
                font-weight: 600;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            .print-footer {
                margin-top: 30px;
                border-top: 1px solid #e5e7eb;
                padding-top: 15px;
                text-align: center;
                page-break-inside: avoid;
            }
            .print-recommendations {
                background: #fdf8f6;
                border: 1px solid #fed7aa;
                padding: 12px;
                border-radius: 8px;
                margin-bottom: 15px;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
        }

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
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
                <div class="bg-primary p-3 rounded-xl shadow-lg text-white">
                    <i class="fas fa-dumbbell text-2xl"></i>
                </div>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs var(--text-muted)">Plataforma Profissional de Prescrição</p>
                </div>
            </div>
            <button onclick="generatePrint()" class="btn-primary px-6 py-3 rounded-xl font-medium shadow-lg flex items-center gap-2 hover:scale-105 transition-transform">
                <i class="fas fa-print"></i> Imprimir Ficha A4
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Dados do Aluno e Configurações -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Card: Dados do Aluno -->
                <div class="card rounded-2xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-user text-primary"></i> Dados do Aluno
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateBMI()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 75.5">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                <input type="number" step="0.01" id="stu-height" oninput="calculateBMI()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 1.75">
                            </div>
                        </div>
                        
                        <!-- Mostrador de IMC -->
                        <div id="bmi-display" class="hidden bg-black bg-opacity-10 rounded-lg p-2 text-center text-sm font-medium">
                            IMC: <span id="bmi-value" class="text-primary font-bold"></span> - <span id="bmi-status"></span>
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
                <div class="card rounded-2xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                    </h2>
                    <p class="text-xs opacity-60 mb-4">Selecione as condições (apenas informativo, não bloqueia o sistema).</p>
                    
                    <div class="space-y-2 max-h-64 overflow-y-auto pr-2" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Card: Configurações Finais -->
                <div class="card rounded-2xl p-5 shadow-sm">
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
                            <label class="block text-sm font-medium mb-1">Recomendações (Descanso, Cardio, Cuidados)</label>
                            <textarea id="stu-recs" rows="4" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Manter hidratação alta. Descanso de 60s entre séries."></textarea>
                        </div>
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="lg:col-span-8 space-y-6">
                
                <div class="flex justify-between items-center bg-opacity-10 p-5 rounded-2xl card border-dashed border-2" style="border-color: var(--primary)">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem Livre de Treinos
                        </h2>
                        <p class="text-sm opacity-70 mt-1">Crie as fichas e adicione os exercícios com controle total.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-5 py-2.5 rounded-xl text-sm font-medium flex items-center gap-2 shadow-md">
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
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4 sm:p-6">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col h-[85vh]">
            <div class="p-5 border-b flex justify-between items-center bg-black bg-opacity-10" style="border-color: var(--border-color)">
                <div>
                    <h3 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-search text-primary"></i> Banco de Exercícios</h3>
                    <p class="text-xs opacity-70 mt-1">Clique nos exercícios para adicionar à ficha. Você pode adicionar vários antes de fechar.</p>
                </div>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors bg-white bg-opacity-10 p-2 rounded-full w-10 h-10 flex items-center justify-center">
                    <i class="fas fa-times text-xl"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Categorias -->
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-4 space-y-2 bg-black bg-opacity-5" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Categorias via JS -->
                </div>
                <!-- Exercícios -->
                <div class="w-full md:w-3/4 overflow-y-auto p-6" id="modal-exercises">
                    <!-- Lista via JS -->
                </div>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast"><i class="fas fa-check-circle mr-2"></i> Exercício adicionado ao treino!</div>

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
                { icon: '🟢', title: 'Saudável', desc: 'Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.' },
                { icon: '⚪', title: 'Sedentário', desc: 'Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.' },
                { icon: '🟡', title: 'Sobrepeso', desc: 'Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.' },
                { icon: '🔴', title: 'Obesidade', desc: 'Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.' },
                { icon: '⚖️', title: 'Baixo peso', desc: 'Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.' },
                { icon: '🍬', title: 'Diabetes', desc: 'Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.' },
                { icon: '❤️', title: 'Hipertensão', desc: 'Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.' },
                { icon: '🔵', title: 'Hipotensão', desc: 'Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.' },
                { icon: '💔', title: 'Problemas cardíacos', desc: 'Treinos leves com liberação médica. Monitorar frequência cardíaca.' },
                { icon: '🦴', title: 'Problemas articulares', desc: 'Evitar impacto. Priorizar exercícios controlados e máquinas.' },
                { icon: '🫁', title: 'Problemas respiratórios', desc: 'Treinos leves a moderados com progressão gradual. Atenção à respiração.' },
                { icon: '⚠️', title: 'Lesões', desc: 'Adaptar exercícios. Evitar dor e focar na recuperação.' },
                { icon: '🤰', title: 'Gestante', desc: 'Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.' },
                { icon: '🤱', title: 'Lactante', desc: 'Treinar normalmente com atenção à hidratação e energia.' },
                { icon: '👴', title: 'Idoso', desc: 'Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança.' }
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
                    "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", 
                    "Agachamento no Smith", "Squat", "Hack Machine", "Leg Press", 
                    "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Passada", 
                    "Avanço", "Búlgaro", "Step-up", "Levantamento Terra", 
                    "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", 
                    "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica", 
                    "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", 
                    "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", 
                    "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", 
                    "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", 
                    "Panturrilha em Pé (Máquina)", "Panturrilha no Leg Press", 
                    "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
                ],
                "💪 BRAÇOS": {
                    "🔹 Bíceps": [
                        "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", 
                        "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", 
                        "Rosca Cross", "Rosca Concentrada", "Rosca Inversa", "Rosca Banco Inclinado"
                    ],
                    "🔹 Tríceps": [
                        "Triceps Pulley Unilateral", "Tríceps Pulley Barra", "Tríceps Pulley Corda", 
                        "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", 
                        "Tríceps Francês com Halter", "Tríceps Francês Unilateral", 
                        "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", 
                        "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"
                    ]
                },
                "🪨 OMBROS": [
                    "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", 
                    "Elevação Lateral na Polia", "Elevação Lateral Sentado", 
                    "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", 
                    "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", 
                    "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", 
                    "Facepull (puxada reta)", "Remada Alta"
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

        // --- ESTADO DA APLICAÇÃO ---
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

        // --- FUNÇÕES DE INTERFACE ---
        function changeTheme() {
            const gender = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', gender);
        }

        function generateId() {
            return Math.random().toString(36).substr(2, 9);
        }

        function calculateBMI() {
            const weight = parseFloat(document.getElementById('stu-weight').value);
            const height = parseFloat(document.getElementById('stu-height').value);
            const bmiDisplay = document.getElementById('bmi-display');
            const bmiValue = document.getElementById('bmi-value');
            const bmiStatus = document.getElementById('bmi-status');

            if (weight > 0 && height > 0) {
                const bmi = (weight / (height * height)).toFixed(1);
                bmiValue.innerText = bmi;
                
                let status = "";
                if (bmi < 18.5) status = "Baixo peso";
                else if (bmi < 24.9) status = "Peso normal";
                else if (bmi < 29.9) status = "Sobrepeso";
                else if (bmi < 34.9) status = "Obesidade I";
                else if (bmi < 39.9) status = "Obesidade II";
                else status = "Obesidade III";

                bmiStatus.innerText = status;
                bmiDisplay.classList.remove('hidden');
            } else {
                bmiDisplay.classList.add('hidden');
            }
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = db.healthOptions.map(opt => `
                <label class="block relative cursor-pointer">
                    <input type="checkbox" value="${opt.title}" class="health-cb sr-only">
                    <div class="health-card rounded-xl p-3 flex gap-3 items-start">
                        <div class="text-xl mt-0.5">${opt.icon}</div>
                        <div>
                            <div class="font-semibold text-sm">${opt.title}</div>
                            <div class="text-xs opacity-70 mt-1 leading-relaxed">${opt.desc}</div>
                        </div>
                    </div>
                </label>
            `).join('');
        }

        function showToast(message) {
            const toast = document.getElementById('toast');
            toast.innerHTML = `<i class="fas fa-check-circle mr-2"></i> ${message}`;
            toast.classList.add('show');
            setTimeout(() => { toast.classList.remove('show'); }, 2000);
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
                let exercisesHtml = '';
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-8 text-sm opacity-50 italic">Nenhum exercício adicionado a este treino. Clique no botão abaixo para adicionar.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col xl:flex-row gap-3 p-4 items-start xl:items-center border-b last:border-0 border-opacity-10 hover:bg-black hover:bg-opacity-5 transition" style="border-color: var(--border-color)">
                            
                            <!-- Controles de ordem -->
                            <div class="flex xl:flex-col gap-1 hidden xl:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-xs p-1 rounded hover:bg-black hover:bg-opacity-10" title="Subir"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-xs p-1 rounded hover:bg-black hover:bg-opacity-10" title="Descer"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <!-- Nome -->
                            <div class="flex-1 font-medium w-full xl:w-auto">
                                <div class="text-[10px] opacity-60 uppercase tracking-wider font-bold">${ex.category}</div>
                                <div class="text-sm mt-0.5">${ex.name}</div>
                            </div>

                            <!-- Campos Editáveis -->
                            <div class="flex flex-wrap gap-2 w-full xl:w-auto items-center">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-16 rounded px-2 py-1.5 text-sm text-center font-semibold" placeholder="Séries">
                                <span class="opacity-50 text-xs">X</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-24 rounded px-2 py-1.5 text-sm text-center font-semibold" placeholder="Reps">
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-36 rounded px-2 py-1.5 text-xs">
                                    ${db.techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 xl:w-48 rounded px-2 py-1.5 text-xs" placeholder="Observações livres...">
                            </div>

                            <!-- Ações -->
                            <div class="flex items-center gap-2 w-full xl:w-auto justify-end mt-2 xl:mt-0">
                                <div class="xl:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="bg-black bg-opacity-10 text-xs p-2 rounded"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="bg-black bg-opacity-10 text-xs p-2 rounded"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:bg-red-500 hover:text-white p-2 rounded transition" title="Remover Exercício">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-2xl overflow-hidden shadow-md border-t-4" style="border-top-color: var(--primary)">
                        <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color); background-color: rgba(0,0,0,0.02)">
                            <div class="flex items-center gap-3 w-2/3">
                                <div class="bg-primary bg-opacity-20 text-primary p-2 rounded-lg"><i class="fas fa-dumbbell"></i></div>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-lg w-full px-2 py-1 rounded border-transparent focus:border-primary">
                            </div>
                            <div class="flex gap-2">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-sm px-3 py-1.5 rounded bg-black bg-opacity-5 hover:bg-opacity-10 transition tooltip flex items-center gap-2" title="Duplicar Treino">
                                    <i class="fas fa-copy"></i> <span class="hidden sm:inline">Duplicar</span>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-sm px-3 py-1.5 text-red-500 rounded bg-red-500 bg-opacity-10 hover:bg-opacity-100 hover:text-white transition tooltip" title="Excluir Treino">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <div class="p-4 bg-black bg-opacity-5 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-3 rounded-xl font-medium text-sm border border-dashed border-opacity-50 hover:border-solid transition flex justify-center items-center gap-2">
                                <i class="fas fa-plus-circle"></i> Adicionar Exercício Neste Treino
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
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-4 py-3 rounded-xl text-sm font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white shadow-md' : 'hover:bg-black hover:bg-opacity-10'}">
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
            const categoryData = db.categories[state.activeCategory];
            
            container.innerHTML = '';

            // Se for um objeto (como BRAÇOS que tem Bíceps e Tríceps)
            if (!Array.isArray(categoryData) && typeof categoryData === 'object') {
                for (const subCat in categoryData) {
                    container.innerHTML += `
                        <h4 class="text-sm font-bold text-primary mb-3 mt-4 border-b border-opacity-20 pb-1" style="border-color: var(--border-color)">${subCat}</h4>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3">
                            ${categoryData[subCat].map(ex => generateExerciseButton(ex, state.activeCategory)).join('')}
                        </div>
                    `;
                }
            } 
            // Se for array normal
            else {
                container.innerHTML = `
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3 mt-2">
                        ${categoryData.map(ex => generateExerciseButton(ex, state.activeCategory)).join('')}
                    </div>
                `;
            }
        }

        function generateExerciseButton(exName, catName) {
            return `
                <button onclick="addExerciseToWorkout('${exName}', '${catName}')" class="card p-3 rounded-xl text-left text-sm font-medium border hover:border-primary hover:text-primary transition flex justify-between items-center group shadow-sm">
                    <span>${exName}</span>
                    <i class="fas fa-plus bg-primary bg-opacity-10 text-primary p-1.5 rounded-lg opacity-0 group-hover:opacity-100 transition"></i>
                </button>
            `;
        }

        function addExerciseToWorkout(exerciseName, categoryName) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                // Remove emoji para visualização limpa na ficha
                const cleanCategory = categoryName.replace(/[\u{1F300}-\u{1F9FF}]/gu, '').trim();

                workout.exercises.push({
                    category: cleanCategory,
                    name: exerciseName,
                    sets: '3',
                    reps: '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                renderWorkouts();
                showToast(`"${exerciseName}" adicionado!`);
            }
        }


        // --- LÓGICA DE IMPRESSÃO PROFISSIONAL A4 ---
        function generatePrint() {
            // Coletar dados
            const weight = parseFloat(document.getElementById('stu-weight').value);
            const height = parseFloat(document.getElementById('stu-height').value);
            let imcStr = '-';
            
            if (weight > 0 && height > 0) {
                imcStr = (weight / (height * height)).toFixed(1);
            }

            const data = {
                name: document.getElementById('stu-name').value || 'Não informado',
                age: document.getElementById('stu-age').value || '-',
                weight: weight ? weight + ' kg' : '-',
                height: height ? height + ' m' : '-',
                imc: imcStr,
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: document.getElementById('stu-objective').value,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value || 'Siga o plano adequadamente respeitando seus limites.',
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value).join(', ') || 'Nenhuma restrição assinalada'
            };

            const printArea = document.getElementById('print-area');
            
            let html = `
                <div class="print-header">
                    <div>
                        <h1 class="print-title">Plano de Treinamento</h1>
                        <p style="margin: 5px 0 0 0; color: #4b5563; font-size: 13px;">Prescrição Individualizada</p>
                    </div>
                    <div style="text-align: right; font-size: 11px;">
                        Data: ${new Date().toLocaleDateString('pt-BR')}<br>
                        Validade: ${data.validity}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${data.name}</div>
                    <div><strong>Objetivo:</strong> ${data.objective}</div>
                    <div><strong>Idade:</strong> ${data.age} anos | <strong>Gênero:</strong> ${data.gender}</div>
                    <div><strong>Peso:</strong> ${data.weight} | <strong>Altura:</strong> ${data.height} | <strong>IMC:</strong> ${data.imc}</div>
                    <div><strong>Nível:</strong> ${data.level} | <strong>Freq:</strong> ${data.freq}</div>
                    <div><strong>Condições Clínicas:</strong> ${data.health}</div>
                </div>
            `;

            // Treinos
            state.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 32%">Exercício</th>
                                    <th style="width: 8%">Séries</th>
                                    <th style="width: 14%">Repetições</th>
                                    <th style="width: 16%">Técnica</th>
                                    <th style="width: 30%">Observações</th>
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
                                <td style="text-align:center">${ex.sets}</td>
                                <td style="text-align:center">${ex.reps}</td>
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

            // Recomendações e Rodapé
            html += `
                <div class="print-recommendations">
                    <h4 style="margin-top:0; margin-bottom: 8px; font-size: 12px; color: #9a3412;">Recomendações do Profissional</h4>
                    <p style="white-space: pre-wrap; margin:0; font-size: 11px; line-height: 1.5; color: #431407;">${data.recs}</p>
                </div>

                <div class="print-footer">
                    <p style="font-weight: bold; margin: 0; font-size: 14px;">Luiz André</p>
                    <p style="margin: 2px 0 0 0; color: #4b5563;">Personal Trainer</p>
                    <p style="margin: 2px 0 0 0; font-size: 11px; font-weight: bold;">CREF: 008094 - G/RN</p>
                </div>
            `;

            printArea.innerHTML = html;
            
            setTimeout(() => {
                window.print();
            }, 300);
        }

        // Iniciar
        init();

    </script>
</body>
</html>
