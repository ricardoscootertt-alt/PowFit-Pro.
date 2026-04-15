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

        /* ESTILOS ESPECÍFICOS PARA A IMPRESSÃO A4 (Estilo Planilha) */
        @media print {
            body {
                background: white !important;
                color: black !important;
                margin: 0;
                padding: 0;
                font-family: 'Arial', sans-serif; /* Fonte mais limpa para impressão */
            }
            #app-container, .no-print, #exercise-modal {
                display: none !important;
            }
            #print-area {
                display: block !important;
                padding: 10mm 15mm;
                font-size: 11px;
            }
            @page {
                size: A4;
                margin: 0;
            }
            
            /* Cabeçalho Obrigatório do Personal */
            .print-mandatory-header {
                text-align: center;
                border: 2px solid #000;
                padding: 8px;
                margin-bottom: 15px;
                background-color: #f3f4f6;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
                border-radius: 4px;
            }
            .print-mandatory-header h2 { margin: 0; font-size: 14px; text-transform: uppercase; }
            .print-mandatory-header p { margin: 3px 0 0 0; font-size: 12px; font-weight: bold; }

            /* Dados do Aluno */
            .print-student-data {
                display: grid;
                grid-template-columns: 1fr 1fr;
                gap: 5px 15px;
                border: 1px solid #000;
                padding: 10px;
                margin-bottom: 15px;
                background-color: #fff;
            }
            .print-student-data div { margin-bottom: 2px; }

            /* Recomendações Automáticas */
            .print-guidelines {
                border: 1px solid #000;
                padding: 10px;
                margin-bottom: 15px;
                font-size: 10.5px;
            }
            .print-guidelines h4 { margin: 0 0 5px 0; font-size: 11px; text-transform: uppercase; border-bottom: 1px solid #ccc; padding-bottom: 3px;}
            .print-guidelines ul { margin: 5px 0 0 0; padding-left: 20px;}
            .print-guidelines li { margin-bottom: 3px; }

            /* Treinos Estilo Planilha */
            .print-workout {
                margin-bottom: 20px;
                page-break-inside: avoid;
            }
            .print-workout h3 {
                background: #000;
                color: #fff;
                padding: 6px 10px;
                margin: 0;
                font-size: 12px;
                text-transform: uppercase;
                text-align: center;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 0;
            }
            th, td {
                border: 1px solid #000;
                padding: 6px;
                text-align: center;
                font-size: 11px;
            }
            td:first-child { text-align: left; font-weight: bold; } /* Nome do exercício alinhado à esquerda */
            th {
                background-color: #e5e7eb;
                font-weight: bold;
                text-transform: uppercase;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }

            /* Recomendações Manuais */
            .print-recommendations {
                border: 1px solid #000;
                padding: 10px;
                margin-top: 15px;
            }
            .print-recommendations h4 { margin: 0 0 5px 0; font-size: 11px; text-transform: uppercase;}
            .print-recommendations p { white-space: pre-wrap; margin:0; font-size: 11px; line-height: 1.4;}
        }

        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg-body); }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--primary); }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header Principal -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs var(--text-muted)">Plataforma Profissional de Prescrição</p>
                </div>
            </div>
            <div class="flex gap-3">
                <button onclick="createNewPlan()" class="card px-4 py-2.5 rounded-lg font-medium shadow-sm flex items-center gap-2 hover:border-primary transition">
                    <i class="fas fa-file text-primary"></i> Nova Ficha
                </button>
                <button onclick="generatePrintAndSave()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2">
                    <i class="fas fa-print"></i> Imprimir & Salvar
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Cadastros, Histórico e Configurações -->
            <div class="lg:col-span-4 space-y-6">

                <!-- Card: Histórico de Fichas (Memória) -->
                <div class="card rounded-xl p-5 shadow-sm border-l-4 border-l-primary">
                    <h2 class="text-lg font-semibold mb-3 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-history text-primary"></i> Fichas Salvas (Memória)
                    </h2>
                    <div id="history-container" class="space-y-2 max-h-48 overflow-y-auto pr-2">
                        <!-- Preenchido via JS -->
                    </div>
                </div>
                
                <!-- Card: Cadastro do Personal -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-id-badge text-primary"></i> Dados do Personal
                    </h2>
                    <div class="space-y-3">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Nome do Personal</label>
                            <input type="text" id="pt-name" oninput="savePTData()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Seu nome">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">CREF</label>
                                <input type="text" id="pt-cref" oninput="savePTData()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="000000-G">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Estado (UF)</label>
                                <input type="text" id="pt-state" oninput="savePTData()" maxlength="2" class="input-field w-full rounded-lg px-3 py-2 text-sm uppercase" placeholder="SP">
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Dados do Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 25">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 75">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 1.75">
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
                    <h2 class="text-lg font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                    </h2>
                    <div class="grid grid-cols-1 gap-1 text-sm max-h-48 overflow-y-auto" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Card: Configurações Finais -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-pen text-primary"></i> Recomendações Extras
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
                            <label class="block text-sm font-medium mb-1">Texto Livre (Hidratação, descanso...)</label>
                            <textarea id="stu-recs" rows="4" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Escreva as recomendações manuais aqui..."></textarea>
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
                        <p class="text-sm opacity-70 mt-1">Adicione treinos e exercícios manualmente.</p>
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
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors">
                    <i class="fas fa-times text-2xl"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-4 space-y-1" style="border-color: var(--border-color)" id="modal-categories">
                    <!-- Categorias via JS -->
                </div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises">
                    <!-- Lista via JS -->
                </div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO (Oculta na tela) -->
    <div id="print-area">
        <!-- Preenchido via JS antes de imprimir -->
    </div>

    <script>
        // --- BASE DE DADOS DE RECOMENDAÇÕES E EXERCÍCIOS ---
        const healthData = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração (Valsalva). Priorizar controle da intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.",
            "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.",
            "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.",
            "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força e aeróbicos.",
            "Hipertrofia": "Prioridade na progressão de carga, volume adequado, superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor intervalo, circuitos e integração cardiopulmonar.",
            "Resistência": "Séries longas (15+ reps), cadência controlada.",
            "Força": "Cargas altas, baixas repetições (1-5) e descansos maiores (2-5 min).",
            "Reabilitação": "Fortalecimento específico e controle motor. Respeitar limites de dor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. Constância é chave."
        };

        const dbCategories = {
            "🔥 PEITO": [
                "Supino Reto", "Supino Inclinado", "Supino com Halteres", "Cross Over", 
                "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado", 
                "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
            ],
            "🦍 COSTAS": [
                "Puxada Alta", "Puxada Unilateral", "Pulldown", "Remada Aberta", 
                "Remada Baixa", "Remada Curvada", "Remada Curvada na Polia", "Remada Unilateral", 
                "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", 
                "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"
            ],
            "🦵 PERNAS": [
                "Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", 
                "Agachamento no Smith", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", 
                "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", 
                "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", 
                "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                "Elevação Pélvica", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", 
                "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", 
                "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", 
                "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", 
                "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", 
                "Panturrilha Squat", "Panturrilha Unilateral"
            ],
            "💪 BRAÇOS (Bíceps)": [
                "Rosca Direta", "Rosca Alternada", "Rosca 21", "Rosca Scott Barra W", 
                "Rosca Scott Unilateral", "Rosca Scott com Halteres", "Rosca Martelo", "Rosca Cross", 
                "Rosca Concentrada", "Rosca Inversa", "Rosca 45°"
            ],
            "💪 BRAÇOS (Tríceps)": [
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
                "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"
            ],
            "🫀 CARDIO": [
                "Bicicleta - 10 Minutos", "Bicicleta - 15 Minutos", "Bicicleta - 20 Minutos",
                "Esteira - 10 Minutos", "Esteira - 15 Minutos", "Esteira - 20 Minutos",
                "Pular Corda"
            ]
        };

        const dbTechniques = [
            "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
            "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
            "Negativa", "Isometria", "Parciais", "Pirâmide"
        ];

        const weekDays = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO GLOBAL DA APLICAÇÃO ---
        let currentPlanId = null;
        let state = {
            workouts: [ { id: generateId(), title: "SEGUNDA-FEIRA", exercises: [] } ],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };
        let savedPlans = [];

        // --- INICIALIZAÇÃO E MEMÓRIA LOCAL ---
        function init() {
            renderHealthOptions();
            loadPTData();
            loadSavedPlansFromStorage();
            renderHistory();
            renderWorkouts();
        }

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        function savePTData() {
            const ptData = {
                name: document.getElementById('pt-name').value,
                cref: document.getElementById('pt-cref').value,
                state: document.getElementById('pt-state').value
            };
            localStorage.setItem('powfit_pt_info', JSON.stringify(ptData));
        }

        function loadPTData() {
            const ptData = JSON.parse(localStorage.getItem('powfit_pt_info'));
            if(ptData) {
                document.getElementById('pt-name').value = ptData.name || '';
                document.getElementById('pt-cref').value = ptData.cref || '';
                document.getElementById('pt-state').value = ptData.state || '';
            }
        }

        function loadSavedPlansFromStorage() {
            savedPlans = JSON.parse(localStorage.getItem('powfit_plans')) || [];
        }

        function createNewPlan() {
            currentPlanId = null;
            document.getElementById('stu-name').value = '';
            document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = '';
            document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = '';
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            document.getElementById('imc-display').innerHTML = '';
            
            state.workouts = [ { id: generateId(), title: "SEGUNDA-FEIRA", exercises: [] } ];
            renderWorkouts();
            alert("Nova ficha em branco criada!");
        }

        function saveCurrentPlanToMemory() {
            const studentName = document.getElementById('stu-name').value || 'Aluno sem nome';
            const healthOptions = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            const planData = {
                id: currentPlanId || generateId(),
                dateStr: new Date().toLocaleDateString('pt-BR') + ' ' + new Date().toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'}),
                student: {
                    name: studentName,
                    age: document.getElementById('stu-age').value,
                    weight: document.getElementById('stu-weight').value,
                    height: document.getElementById('stu-height').value,
                    gender: document.getElementById('stu-gender').value,
                    level: document.getElementById('stu-level').value,
                    objective: document.getElementById('stu-objective').value,
                    freq: document.getElementById('stu-freq').value,
                    validity: document.getElementById('stu-validity').value,
                    recs: document.getElementById('stu-recs').value,
                    health: healthOptions
                },
                workouts: state.workouts
            };

            // Atualiza ou Insere
            savedPlans = savedPlans.filter(p => p.id !== planData.id);
            savedPlans.push(planData);
            localStorage.setItem('powfit_plans', JSON.stringify(savedPlans));
            
            currentPlanId = planData.id;
            renderHistory();
        }

        function loadPlan(id) {
            const plan = savedPlans.find(p => p.id === id);
            if(!plan) return;

            currentPlanId = plan.id;
            const s = plan.student;
            
            document.getElementById('stu-name').value = s.name || '';
            document.getElementById('stu-age').value = s.age || '';
            document.getElementById('stu-weight').value = s.weight || '';
            document.getElementById('stu-height').value = s.height || '';
            document.getElementById('stu-gender').value = s.gender || 'Masculino';
            document.getElementById('stu-level').value = s.level || 'Iniciante';
            document.getElementById('stu-objective').value = s.objective || 'Emagrecimento';
            document.getElementById('stu-freq').value = s.freq || '3 dias';
            document.getElementById('stu-validity').value = s.validity || '30 dias';
            document.getElementById('stu-recs').value = s.recs || '';

            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = s.health && s.health.includes(cb.value);
            });

            state.workouts = plan.workouts || [];
            
            changeTheme();
            calculateIMC();
            renderWorkouts();
        }

        function deletePlan(id) {
            if(confirm("Tem certeza que deseja excluir esta ficha da memória?")) {
                savedPlans = savedPlans.filter(p => p.id !== id);
                localStorage.setItem('powfit_plans', JSON.stringify(savedPlans));
                if(currentPlanId === id) createNewPlan();
                renderHistory();
            }
        }

        function renderHistory() {
            const container = document.getElementById('history-container');
            if(savedPlans.length === 0) {
                container.innerHTML = `<p class="text-xs opacity-50 italic">Nenhuma ficha salva.</p>`;
                return;
            }

            // Ordenar da mais recente para mais antiga
            const sortedPlans = [...savedPlans].reverse();

            container.innerHTML = sortedPlans.map(p => `
                <div class="flex justify-between items-center p-2 rounded bg-black bg-opacity-5 hover:bg-opacity-10 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <div class="cursor-pointer flex-1" onclick="loadPlan('${p.id}')">
                        <p class="text-sm font-bold truncate w-32 sm:w-48">${p.student.name}</p>
                        <p class="text-[10px] opacity-60">${p.dateStr}</p>
                    </div>
                    <button onclick="deletePlan('${p.id}')" class="text-red-500 hover:text-red-700 p-2" title="Excluir Ficha">
                        <i class="fas fa-trash"></i>
                    </button>
                </div>
            `).join('');
        }

        // --- INTERFACE E CÁLCULOS ---
        function changeTheme() {
            const gender = document.getElementById('stu-gender').value;
            document.body.setAttribute('data-theme', gender);
        }

        function calculateIMC() {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');

            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = "";
                if (imc < 18.5) classif = "Abaixo do peso";
                else if (imc < 24.9) classif = "Peso normal";
                else if (imc < 29.9) classif = "Sobrepeso";
                else if (imc < 34.9) classif = "Obesidade I";
                else if (imc < 39.9) classif = "Obesidade II";
                else classif = "Obesidade III";
                
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else {
                display.innerHTML = '';
            }
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = Object.keys(healthData).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1 rounded hover:bg-black hover:bg-opacity-5 transition">
                    <input type="checkbox" value="${opt}" class="health-cb mt-1 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium text-xs">${opt}</span>
                </label>
            `).join('');
        }

        // --- LÓGICA DE TREINOS ---
        function getNextDayTitle() {
            const currentTitles = state.workouts.map(w => w.title.toUpperCase());
            for(let day of weekDays) {
                if(!currentTitles.includes(day)) return day;
            }
            return "NOVO TREINO";
        }

        function addWorkout() {
            state.workouts.push({
                id: generateId(),
                title: getNextDayTitle(),
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
            if(confirm("Excluir este treino inteiro?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        }

        function updateWorkoutTitle(id, newTitle) {
            const workout = state.workouts.find(w => w.id === id);
            if(workout) workout.title = newTitle;
        }

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
                    exercisesHtml = `<div class="text-center p-6 text-sm opacity-50 italic">Nenhum exercício neste treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col xl:flex-row gap-3 p-3 items-start xl:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex xl:flex-col gap-1 hidden xl:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-chevron-down"></i></button>
                            </div>

                            <div class="flex-1 font-medium w-full xl:w-auto">
                                <div class="text-[10px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                <div class="text-sm">${ex.name}</div>
                            </div>

                            <div class="flex flex-wrap gap-2 w-full xl:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-14 rounded px-2 py-1 text-sm text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-sm">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-20 rounded px-2 py-1 text-sm text-center" placeholder="Reps">
                                
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-28 rounded px-2 py-1 text-xs">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 xl:w-40 rounded px-2 py-1 text-sm" placeholder="Obs...">
                            </div>

                            <div class="flex items-center gap-2 w-full xl:w-auto justify-end mt-2 xl:mt-0">
                                <div class="xl:hidden flex gap-2 mr-auto">
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="btn-primary text-xs p-1.5 rounded"><i class="fas fa-arrow-up"></i></button>
                                    <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="btn-primary text-xs p-1.5 rounded"><i class="fas fa-arrow-down"></i></button>
                                </div>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-2 rounded transition">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-xl overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between items-center" style="border-color: var(--border-color); background-color: rgba(0,0,0,0.02)">
                            <div class="flex items-center gap-2 w-2/3">
                                <i class="fas fa-calendar-day opacity-50 text-primary"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-base uppercase w-full px-2 py-1 rounded border-transparent focus:border-primary">
                            </div>
                            <div class="flex gap-1">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-xs p-2 rounded hover:bg-black hover:bg-opacity-10 tooltip" title="Duplicar">
                                    <i class="fas fa-copy"></i>
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-xs p-2 text-red-500 rounded hover:bg-red-500 hover:text-white" title="Excluir">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        
                        <div class="p-0">
                            ${exercisesHtml}
                        </div>

                        <div class="p-2 bg-black bg-opacity-5 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-1.5 rounded-lg font-medium text-xs border border-dashed border-opacity-50 hover:border-solid transition flex justify-center items-center gap-2">
                                <i class="fas fa-plus-circle"></i> Adicionar Exercício
                            </button>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', workoutCard);
            });
        }

        // --- MODAL DE EXERCÍCIOS ---
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
            container.innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2.5 rounded-lg text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white shadow-md' : 'hover:bg-black hover:bg-opacity-10'}">
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
            const exercises = dbCategories[state.activeCategory];
            
            container.innerHTML = `
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                    ${exercises.map(ex => {
                        const isCardio = state.activeCategory === "🫀 CARDIO";
                        return `
                        <button onclick="addExerciseToWorkout('${ex}', ${isCardio})" class="card p-2.5 rounded-lg text-left text-xs font-medium border hover:border-primary hover:text-primary transition flex justify-between items-center group">
                            <span>${ex}</span>
                            <i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i>
                        </button>
                        `
                    }).join('')}
                </div>
            `;
        }

        function addExerciseToWorkout(exerciseName, isCardio) {
            const workout = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(workout) {
                workout.exercises.push({
                    category: state.activeCategory,
                    name: exerciseName,
                    sets: isCardio ? '1' : '3',
                    reps: isCardio ? '-' : '10 a 12',
                    technique: 'Nenhuma',
                    obs: ''
                });
                renderWorkouts();
                
                const container = document.getElementById('modal-exercises');
                container.style.opacity = '0.5';
                setTimeout(() => container.style.opacity = '1', 150);
            }
        }

        // --- GERAÇÃO DE IMPRESSÃO (ESTILO PLANILHA) E SALVAMENTO ---
        function generatePrintAndSave() {
            // Salvar automaticamente na memória
            saveCurrentPlanToMemory();

            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            let imcStr = "-";
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const ptName = document.getElementById('pt-name').value || 'Não informado';
            const ptCref = document.getElementById('pt-cref').value || 'Não informado';
            const ptState = document.getElementById('pt-state').value || 'UF';

            const selectedObjective = document.getElementById('stu-objective').value;
            const objRecommendation = objectiveData[selectedObjective] || "";
            const selectedHealthBoxes = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            const data = {
                name: document.getElementById('stu-name').value || 'Não informado',
                age: document.getElementById('stu-age').value || '-',
                weight: document.getElementById('stu-weight').value || '-',
                height: document.getElementById('stu-height').value || '-',
                imc: imcStr,
                gender: document.getElementById('stu-gender').value,
                level: document.getElementById('stu-level').value,
                objective: selectedObjective,
                freq: document.getElementById('stu-freq').value,
                validity: document.getElementById('stu-validity').value,
                recs: document.getElementById('stu-recs').value
            };

            const printArea = document.getElementById('print-area');
            
            // CABEÇALHO OBRIGATÓRIO E DADOS DO ALUNO
            let html = `
                <div class="print-mandatory-header">
                    <h2>Prescrição feita por Personal Trainer: ${ptName}</h2>
                    <p>CREF: ${ptCref} - ESTADO: ${ptState}</p>
                </div>

                <div style="display:flex; justify-content:space-between; margin-bottom:5px;">
                    <strong style="font-size:14px; text-transform:uppercase;">Ficha de Treinamento</strong>
                    <span>Data: ${new Date().toLocaleDateString('pt-BR')} | Validade: ${data.validity}</span>
                </div>

                <div class="print-student-data">
                    <div><strong>Nome:</strong> ${data.name}</div>
                    <div><strong>Idade:</strong> ${data.age} anos | <strong>Gênero:</strong> ${data.gender}</div>
                    <div><strong>Peso:</strong> ${data.weight} kg | <strong>Altura:</strong> ${data.height} m | <strong>IMC:</strong> ${data.imc}</div>
                    <div><strong>Nível:</strong> ${data.level}</div>
                    <div><strong>Objetivo:</strong> ${data.objective}</div>
                    <div><strong>Frequência:</strong> ${data.freq}</div>
                </div>
            `;

            // RECOMENDAÇÕES AUTOMÁTICAS (OBJETIVO E SAÚDE)
            if (objRecommendation || selectedHealthBoxes.length > 0) {
                html += `<div class="print-guidelines">
                            <h4>Diretrizes de Saúde e Objetivo</h4>
                            <ul>`;
                
                if(objRecommendation) {
                    html += `<li><strong>Objetivo (${data.objective}):</strong> ${objRecommendation}</li>`;
                }

                selectedHealthBoxes.forEach(healthKey => {
                    if(healthData[healthKey]) {
                        const cleanKey = healthKey.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        html += `<li><strong>Atenção (${cleanKey}):</strong> ${healthData[healthKey]}</li>`;
                    }
                });
                html += `</ul></div>`;
            }

            // TREINOS (ESTILO PLANILHA)
            state.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%;">Exercício</th>
                                    <th style="width: 10%;">Séries</th>
                                    <th style="width: 15%;">Repetições</th>
                                    <th style="width: 15%;">Técnica / Método</th>
                                    <th style="width: 25%;">Observações</th>
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
                                <td>${ex.name}</td>
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

            // RECOMENDAÇÕES MANUAIS (LIVRES)
            if(data.recs.trim()) {
                html += `
                    <div class="print-recommendations">
                        <h4>Recomendações do Profissional (Hidratação, Descanso, Intensidade, etc.)</h4>
                        <p>${data.recs}</p>
                    </div>
                `;
            }

            printArea.innerHTML = html;
            
            // Disparar impressão
            setTimeout(() => {
                window.print();
            }, 300);
        }

        // Iniciar App
        init();

    </script>
</body>
</html>
