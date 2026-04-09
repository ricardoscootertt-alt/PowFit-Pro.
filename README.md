<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Prescrição Profissional</title>
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
            --auto-rec-bg: rgba(59, 130, 246, 0.1);
            --auto-rec-border: rgba(59, 130, 246, 0.3);
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
            --auto-rec-bg: rgba(236, 72, 153, 0.05);
            --auto-rec-border: rgba(236, 72, 153, 0.2);
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

        .text-primary { color: var(--primary); }

        /* Oculta a área de impressão na tela normal */
        #print-area { display: none; }

        /* Scrollbar customizada */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: var(--bg-body); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }

        /* ESTILOS DE IMPRESSÃO A4 */
        @media print {
            body {
                background: white !important;
                color: black !important;
                margin: 0;
                padding: 0;
            }
            #app-container, .no-print, #exercise-modal {
                display: none !important;
            }
            #print-area {
                display: block !important;
                padding: 15mm;
                font-family: 'Inter', sans-serif;
                font-size: 11px;
            }
            @page {
                size: A4;
                margin: 0;
            }
            .print-header {
                border-bottom: 2px solid #111827;
                padding-bottom: 10px;
                margin-bottom: 15px;
                display: flex;
                justify-content: space-between;
                align-items: flex-end;
            }
            .print-title {
                font-size: 22px;
                font-weight: 800;
                margin: 0;
                text-transform: uppercase;
                letter-spacing: 1px;
            }
            .print-grid {
                display: grid;
                grid-template-columns: repeat(3, 1fr);
                gap: 10px;
                margin-bottom: 20px;
                background: #f9fafb;
                padding: 12px;
                border-radius: 6px;
                border: 1px solid #e5e7eb;
            }
            .print-grid div { margin-bottom: 2px; }
            
            .print-recommendations-box {
                border-left: 4px solid #4b5563;
                background: #f3f4f6;
                padding: 10px 15px;
                margin-bottom: 20px;
                border-radius: 0 6px 6px 0;
                page-break-inside: avoid;
            }
            .print-recommendations-box h4 {
                margin: 0 0 5px 0;
                font-size: 12px;
                text-transform: uppercase;
                color: #1f2937;
            }

            .print-workout {
                margin-bottom: 20px;
                page-break-inside: avoid;
            }
            .print-workout h3 {
                background: #111827;
                color: white;
                padding: 6px 10px;
                margin: 0 0 8px 0;
                font-size: 13px;
                border-radius: 4px;
                text-transform: uppercase;
            }
            table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 5px;
            }
            th, td {
                border: 1px solid #d1d5db;
                padding: 6px 8px;
                text-align: left;
                font-size: 11px;
            }
            th {
                background-color: #f3f4f6;
                font-weight: 700;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
                text-transform: uppercase;
                font-size: 10px;
            }
            .print-footer {
                margin-top: 30px;
                border-top: 1px solid #e5e7eb;
                padding-top: 15px;
                text-align: center;
                page-break-inside: avoid;
            }
        }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- ÁREA PRINCIPAL DO SISTEMA -->
    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-8 gap-4">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-4xl text-primary"></i>
                <div>
                    <h1 class="text-3xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-sm opacity-80">Plataforma Avançada de Prescrição Manual</p>
                </div>
            </div>
            <button onclick="generatePrint()" class="btn-primary px-6 py-3 rounded-lg font-bold shadow-lg flex items-center gap-2 hover:scale-105 transition-transform">
                <i class="fas fa-print"></i> Imprimir Ficha A4
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Dados e Diretrizes -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-user text-primary"></i> Cadastro do Aluno
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
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 75.5">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                <input type="number" step="0.01" id="stu-height" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 1.75">
                            </div>
                        </div>
                        
                        <!-- IMC Display -->
                        <div id="imc-display" class="text-xs font-medium px-3 py-2 rounded-lg bg-opacity-10 hidden border" style="background-color: var(--primary); border-color: var(--primary)">
                            IMC Calculado: <span id="imc-value" class="font-bold"></span> - <span id="imc-status"></span>
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
                            <select id="stu-objective" onchange="updateRecommendations()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="Emagrecimento">Emagrecimento</option>
                                <option value="Hipertrofia">Hipertrofia</option>
                                <option value="Definição">Definição</option>
                                <option value="Condicionamento">Condicionamento</option>
                                <option value="Resistência">Resistência</option>
                                <option value="Força">Força</option>
                                <option value="Reabilitação">Reabilitação</option>
                                <option value="Saúde geral">Saúde geral</option>
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
                        <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                    </h2>
                    <p class="text-xs opacity-70 mb-3">Selecione as opções (gera diretrizes automáticas):</p>
                    <div class="grid grid-cols-1 gap-2 text-sm max-h-48 overflow-y-auto pr-2" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Card: Recomendações (Auto + Manual) -->
                <div class="card rounded-xl p-5 shadow-sm flex flex-col gap-4">
                    <h2 class="text-lg font-semibold flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-comment-medical text-primary"></i> Recomendações
                    </h2>
                    
                    <!-- Diretrizes Automáticas (Read Only) -->
                    <div class="p-3 rounded-lg text-xs" style="background-color: var(--auto-rec-bg); border: 1px solid var(--auto-rec-border)">
                        <div class="font-bold mb-1 flex items-center gap-1"><i class="fas fa-robot"></i> Diretrizes Automáticas (Sistema)</div>
                        <div id="auto-rec-text" class="opacity-80">Selecione o objetivo e estado de saúde para gerar diretrizes.</div>
                    </div>

                    <!-- Diretrizes do Personal (Manual) -->
                    <div>
                        <label class="block text-sm font-medium mb-1 flex items-center gap-1">
                            <i class="fas fa-pen text-primary"></i> Suas Notas (Opcional)
                        </label>
                        <textarea id="stu-recs" rows="4" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Escreva aqui cuidados com hidratação, intensidade, etc..."></textarea>
                    </div>

                    <div>
                        <label class="block text-sm font-medium mb-1">Validade do Treino</label>
                        <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                            <option>15 dias</option>
                            <option selected>30 dias</option>
                            <option>60 dias</option>
                            <option>90 dias</option>
                        </select>
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="lg:col-span-8 space-y-6">
                
                <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2" style="border-color: var(--primary)">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem Livre
                        </h2>
                        <p class="text-sm opacity-70 mt-1">Crie treinos e adicione exercícios. O controle é 100% seu.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2">
                        <i class="fas fa-plus"></i> Novo Treino
                    </button>
                </div>

                <!-- Container dos Treinos -->
                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos gerados via JS -->
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors">
                    <i class="fas fa-times text-2xl"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <!-- Categorias -->
                <div class="w-full md:w-1/3 lg:w-1/4 border-r overflow-y-auto p-4 space-y-1" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Categorias via JS -->
                </div>
                <!-- Exercícios -->
                <div class="w-full md:w-2/3 lg:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises">
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
        // --- BANCO DE DADOS ATUALIZADO ---
        const db = {
            healthOptions: [
                { id: "h1", icon: "🟢", name: "Saudável", rec: "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância." },
                { id: "h2", icon: "⚪", name: "Sedentário", rec: "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga." },
                { id: "h3", icon: "🟡", name: "Sobrepeso", rec: "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado." },
                { id: "h4", icon: "🔴", name: "Obesidade", rec: "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança." },
                { id: "h5", icon: "⚖️", name: "Baixo peso", rec: "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada." },
                { id: "h6", icon: "🍬", name: "Diabetes", rec: "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum." },
                { id: "h7", icon: "❤️", name: "Hipertensão", rec: "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade." },
                { id: "h8", icon: "🔵", name: "Hipotensão", rec: "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada." },
                { id: "h9", icon: "💔", name: "Problemas cardíacos", rec: "Treinos leves com liberação médica. Monitorar frequência cardíaca." },
                { id: "h10", icon: "🦴", name: "Problemas articulares", rec: "Evitar impacto. Priorizar exercícios controlados e máquinas." },
                { id: "h11", icon: "🫁", name: "Problemas respiratórios", rec: "Treinos leves a moderados com progressão gradual. Atenção à respiração." },
                { id: "h12", icon: "⚠️", name: "Lesões", rec: "Adaptar exercícios. Evitar dor e focar na recuperação." },
                { id: "h13", icon: "🤰", name: "Gestante", rec: "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar." },
                { id: "h14", icon: "🤱", name: "Lactante", rec: "Treinar normalmente com atenção à hidratação e energia." },
                { id: "h15", icon: "👴", name: "Idoso", rec: "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança." }
            ],
            objectiveRecs: {
                "Emagrecimento": "Priorizar gasto calórico total, aliando treino de força intenso com sessões de cardio consistentes.",
                "Hipertrofia": "Focar em progressão de carga, estímulo mecânico adequado e superávit calórico na dieta.",
                "Definição": "Manter intensidade alta na musculação para preservar massa magra, combinado com leve déficit calórico.",
                "Condicionamento": "Mesclar treinos resistidos com exercícios cardiovasculares de intensidade variada.",
                "Resistência": "Priorizar maior volume (15+ repetições), cargas moderadas e intervalos de descanso reduzidos.",
                "Força": "Foco em exercícios multiarticulares, baixas repetições (1-6) e intervalos de descanso longos (2-5 min).",
                "Reabilitação": "Priorizar execução perfeita, estabilidade articular e progressão de carga extremamente gradual.",
                "Saúde geral": "Manter constância e equilíbrio entre treino de força, mobilidade e aptidão cardiovascular."
            },
            categories: {
                "🔥 PEITO": [
                    "Supino Reto", "Supino Inclinado", "Supino com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", 
                    "Crucifixo Reto", "Crucifixo Inclinado", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                    "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
                ],
                "🦍 COSTAS": [
                    "Puxada Alta", "Puxada Frontal", "Puxada Unilateral", "Pulldown", "Remada Aberta", "Remada Baixa", 
                    "Remada Curvada", "Remada Unilateral", "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", 
                    "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
                ],
                "🦵 PERNAS": [
                    "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Squat", "Hack Machine", 
                    "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Passada", "Avanço", "Búlgaro", "Step-up", 
                    "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                    "Elevação Pélvica", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", 
                    "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", "Flexão Nórdica", 
                    "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", 
                    "Panturrilha Squat", "Panturrilha Unilateral"
                ],
                "💪 BRAÇOS": [
                    "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", "Rosca Scott Unilateral", "Rosca Scott com Halteres", 
                    "Rosca Martelo", "Rosca Cross", "Rosca Concentrada", "Rosca Inversa", "Rosca Banco Inclinado", "Triceps Pulley Unilateral", 
                    "Tríceps Pulley Barra", "Tríceps Pulley Corda", "Tríceps Pulley Pegada Inversa", "Tríceps Francês na Corda", "Tríceps Francês com Halter", 
                    "Tríceps Francês Unilateral", "Tríceps Cruzado Polia Dupla", "Tríceps Coice Unilateral", "Tríceps Arremesso", "Tríceps Testa", "Mergulho no Banco"
                ],
                "🪨 OMBROS": [
                    "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Sentado", 
                    "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", 
                    "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta"
                ],
                "🧠 ABDÔMEN": [
                    "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", 
                    "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"
                ],
                "🫀 CARDIO": [
                    "Bicicleta", "Esteira", "Pular Corda"
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
            updateRecommendations();
            renderWorkouts();
        }

        // --- FUNÇÕES DE LÓGICA E INTERFACE ---
        function changeTheme() {
            const gender = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', gender);
        }

        function generateId() {
            return Math.random().toString(36).substr(2, 9);
        }

        function calculateIMC() {
            const weight = parseFloat(document.getElementById('stu-weight').value);
            const height = parseFloat(document.getElementById('stu-height').value);
            const displayBox = document.getElementById('imc-display');
            const valueSpan = document.getElementById('imc-value');
            const statusSpan = document.getElementById('imc-status');

            if (weight > 0 && height > 0) {
                const imc = (weight / (height * height)).toFixed(1);
                valueSpan.innerText = imc;
                
                let status = "";
                if (imc < 18.5) status = "Abaixo do peso";
                else if (imc < 24.9) status = "Peso normal";
                else if (imc < 29.9) status = "Sobrepeso";
                else if (imc < 34.9) status = "Obesidade Grau I";
                else if (imc < 39.9) status = "Obesidade Grau II";
                else status = "Obesidade Grau III";

                statusSpan.innerText = status;
                displayBox.classList.remove('hidden');
            } else {
                displayBox.classList.add('hidden');
            }
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = db.healthOptions.map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition">
                    <input type="checkbox" value="${opt.id}" onchange="updateRecommendations()" class="health-cb mt-1 rounded border-gray-400 text-primary focus:ring-primary w-4 h-4">
                    <span><strong>${opt.icon} ${opt.name}</strong></span>
                </label>
            `).join('');
        }

        function updateRecommendations() {
            const objective = document.getElementById('stu-objective').value;
            const objRec = db.objectiveRecs[objective];
            
            const checkedHealthBoxes = document.querySelectorAll('.health-cb:checked');
            let healthRecs = [];
            
            checkedHealthBoxes.forEach(cb => {
                const healthObj = db.healthOptions.find(h => h.id === cb.value);
                if (healthObj) {
                    healthRecs.push(`<strong>${healthObj.name}:</strong> ${healthObj.rec}`);
                }
            });

            const container = document.getElementById('auto-rec-text');
            
            let html = `<p class="mb-2"><strong>Objetivo (${objective}):</strong> ${objRec}</p>`;
            if (healthRecs.length > 0) {
                html += `<p class="mt-2 mb-1 border-t border-opacity-20 pt-1 border-current"><strong>Condições Clínicas:</strong></p>`;
                html += `<ul class="list-disc pl-4 space-y-1">${healthRecs.map(r => `<li>${r}</li>`).join('')}</ul>`;
            } else {
                html += `<p class="mt-2 opacity-70 italic">Nenhuma condição clínica selecionada.</p>`;
            }

            container.innerHTML = html;
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
                    exercisesHtml = `<div class="text-center p-6 text-sm opacity-50 italic">Nenhum exercício adicionado a este treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-3 p-3 items-start sm:items-center border-b last:border-0 border-opacity-10 hover:bg-black hover:bg-opacity-5 transition" style="border-color: var(--border-color)">
                            
                            <!-- Controles de ordem -->
                            <div class="flex sm:flex-col gap-1 hidden sm:flex opacity-50 hover:opacity-100 transition">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-xs p-1 rounded hover:bg-gray-300" title="Subir"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-xs p-1 rounded hover:bg-gray-300" title="Descer"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <!-- Nome -->
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[10px] opacity-70 font-bold tracking-wider">${ex.category}</div>
                                <div>${ex.name}</div>
                            </div>

                            <!-- Campos Editáveis -->
                            <div class="flex flex-wrap gap-2 w-full sm:w-auto items-center">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-16 rounded px-2 py-1.5 text-sm text-center" placeholder="Séries">
                                <span class="opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-24 rounded px-2 py-1.5 text-sm text-center" placeholder="Reps/Tempo">
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-32 rounded px-2 py-1.5 text-sm">
                                    ${db.techniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-48 rounded px-2 py-1.5 text-sm" placeholder="Observações livres">
                            </div>

                            <!-- Ações -->
                            <div class="flex items-center gap-2 w-full sm:w-auto justify-end mt-2 sm:mt-0">
                                <div class="sm:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="btn-primary text-xs p-2 rounded"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="btn-primary text-xs p-2 rounded"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-2 rounded transition bg-red-100 hover:bg-red-200 bg-opacity-10" title="Remover Exercício">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color); background-color: rgba(0,0,0,0.03)">
                            <div class="flex items-center gap-2 w-1/2">
                                <i class="fas fa-layer-group opacity-50"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-lg w-full px-2 py-1 rounded">
                            </div>
                            <div class="flex gap-2">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-sm px-3 py-1.5 bg-black bg-opacity-10 rounded hover:bg-opacity-20 transition tooltip font-medium flex items-center gap-1" title="Duplicar Treino">
                                    <i class="fas fa-copy"></i> Duplicar
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-sm px-3 py-1.5 text-red-500 bg-red-500 bg-opacity-10 rounded hover:bg-opacity-20 transition tooltip font-medium flex items-center gap-1" title="Excluir Treino">
                                    <i class="fas fa-trash"></i> Excluir
                                </button>
                            </div>
                        </div>
                        
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <div class="p-3 bg-black bg-opacity-5 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-2.5 rounded-lg font-medium text-sm border border-dashed border-opacity-50 hover:border-solid transition flex justify-center items-center gap-2">
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
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-4 py-3 rounded-lg text-sm font-bold transition ${state.activeCategory === cat ? 'bg-primary text-white shadow-md' : 'hover:bg-black hover:bg-opacity-10'}">
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
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${exercises.map(ex => `
                        <button onclick="addExerciseToWorkout('${ex}')" class="card p-3 rounded-lg text-left text-sm font-medium border hover:border-primary hover:text-primary transition flex justify-between items-center group">
                            <span>${ex}</span>
                            <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100 transition"></i>
                        </button>
                    `).join('')}
                </div>
            `;
        }

        function addExerciseToWorkout(exerciseName) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                
                // Inteligência simples: se for cardio, muda o padrão de reps para minutos
                const isCardio = state.activeCategory === "🫀 CARDIO";

                workout.exercises.push({
                    category: state.activeCategory,
                    name: exerciseName,
                    sets: isCardio ? '1' : '3',
                    reps: isCardio ? '15 a 20 min' : '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                renderWorkouts();
                
                // Feedback visual
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 150);
            }
        }


        // --- LÓGICA DE IMPRESSÃO PROFISSIONAL A4 ---
        function generatePrint() {
            const name = document.getElementById('stu-name').value || 'Não informado';
            const age = document.getElementById('stu-age').value || '-';
            const weight = document.getElementById('stu-weight').value || '-';
            const height = document.getElementById('stu-height').value || '-';
            const gender = document.getElementById('stu-gender').value;
            const level = document.getElementById('stu-level').value;
            const objective = document.getElementById('stu-objective').value;
            const freq = document.getElementById('stu-freq').value;
            const validity = document.getElementById('stu-validity').value;
            const personalRecs = document.getElementById('stu-recs').value;

            // Puxar auto-recs
            const autoRecHtml = document.getElementById('auto-rec-text').innerHTML;
            const imcValue = document.getElementById('imc-value').innerText;
            const imcStr = imcValue ? `${imcValue} (${document.getElementById('imc-status').innerText})` : '-';

            const printArea = document.getElementById('print-area');
            
            let html = `
                <div class="print-header">
                    <div>
                        <h1 class="print-title">Plano de Treinamento</h1>
                        <p style="margin: 3px 0 0 0; color: #4b5563; font-size: 13px;">Prescrição Individualizada</p>
                    </div>
                    <div style="text-align: right;">
                        <strong>Data:</strong> ${new Date().toLocaleDateString('pt-BR')}<br>
                        <strong>Validade:</strong> ${validity}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${name}</div>
                    <div><strong>Objetivo:</strong> ${objective}</div>
                    <div><strong>Nível:</strong> ${level}</div>
                    
                    <div><strong>Idade:</strong> ${age} anos</div>
                    <div><strong>Gênero:</strong> ${gender}</div>
                    <div><strong>Frequência:</strong> ${freq}</div>
                    
                    <div><strong>Peso:</strong> ${weight} kg</div>
                    <div><strong>Altura:</strong> ${height} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                </div>

                <div class="print-recommendations-box">
                    <h4>Diretrizes e Recomendações</h4>
                    <div style="margin-bottom: 8px;">
                        ${autoRecHtml}
                    </div>
                    ${personalRecs ? `<div style="border-top: 1px dashed #9ca3af; padding-top: 8px; margin-top: 8px;"><strong>Notas do Personal:</strong><br><span style="white-space: pre-wrap;">${personalRecs}</span></div>` : ''}
                </div>
            `;

            state.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%">Exercício</th>
                                    <th style="width: 8%">Séries</th>
                                    <th style="width: 15%">Reps/Tempo</th>
                                    <th style="width: 15%">Técnica</th>
                                    <th style="width: 27%">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                
                if (w.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center; color:#6b7280; font-style:italic;">Nenhum exercício cadastrado</td></tr>`;
                } else {
                    w.exercises.forEach(ex => {
                        const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td style="text-align:center;">${ex.sets}</td>
                                <td style="text-align:center;">${ex.reps}</td>
                                <td style="text-align:center;">${techStr}</td>
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

            html += `
                <div class="print-footer">
                    <p style="font-weight: 800; margin: 0; font-size: 14px; text-transform: uppercase;">Luiz André</p>
                    <p style="margin: 2px 0 0 0; color: #4b5563;">Personal Trainer</p>
                    <p style="margin: 2px 0 0 0; font-size: 11px; font-weight: bold;">CREF: 008094 - G/RN</p>
                </div>
            `;

            printArea.innerHTML = html;
            
            setTimeout(() => {
                window.print();
            }, 300);
        }

        // Iniciar App
        init();

    </script>
</body>
</html>
