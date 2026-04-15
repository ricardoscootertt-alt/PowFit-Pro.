<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro - Plataforma Profissional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Tema Masculino (Escuro Fitness) */
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
            /* Tema Feminino (Rosa Moderno) */
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
            transition: all 0.3s ease;
        }

        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
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

        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }

        /* Impressão: Estilo Planilha Profissional */
        #print-area { display: none; }

        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #dashboard-view, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Arial', sans-serif; font-size: 11px; }
            @page { size: A4; margin: 0; }
            
            /* Cabeçalho Profissional */
            .print-header-box {
                border: 2px solid #000;
                padding: 10px;
                margin-bottom: 15px;
                background-color: #f3f4f6;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
            }
            .print-header-box h1 { margin: 0; font-size: 16px; text-transform: uppercase; text-align: center; border-bottom: 1px solid #ccc; padding-bottom: 5px; margin-bottom: 5px;}
            .print-header-box p { margin: 3px 0; font-size: 12px; font-weight: bold; text-align: center; }
            
            /* Grid Aluno Planilha */
            .print-student-table {
                width: 100%;
                border-collapse: collapse;
                margin-bottom: 15px;
            }
            .print-student-table th, .print-student-table td {
                border: 1px solid #000;
                padding: 5px 8px;
                font-size: 11px;
            }
            .print-student-table th { background-color: #e5e7eb; font-weight: bold; width: 15%; -webkit-print-color-adjust: exact; }

            /* Recomendações Automáticas */
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 10.5px; }
            .print-guidelines h4 { margin: 0 0 5px 0; font-size: 11px; background: #e5e7eb; padding: 4px; border-bottom: 1px solid #000; -webkit-print-color-adjust: exact; }
            .print-guidelines ul { margin: 5px 0 0 0; padding-left: 20px; }

            /* Tabelas de Treino (Estilo Planilha Rigorosa) */
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 { 
                background: #000; color: #fff; padding: 6px; margin: 0; font-size: 13px; text-transform: uppercase; text-align: center; border: 1px solid #000; -webkit-print-color-adjust: exact;
            }
            .workout-table { width: 100%; border-collapse: collapse; }
            .workout-table th, .workout-table td {
                border: 1px solid #000; padding: 6px 4px; text-align: center; font-size: 11px;
            }
            .workout-table th { background-color: #e5e7eb; -webkit-print-color-adjust: exact; }
            .workout-table td:first-child { text-align: left; font-weight: bold; width: 40%; }
            .workout-table td:last-child { text-align: left; width: 25%; font-style: italic; }

            /* Footer Recomendações Manuais */
            .print-manual-recs { border: 1px solid #000; padding: 8px; margin-top: 10px; page-break-inside: avoid;}
            .print-manual-recs h4 { margin: 0 0 5px 0; font-size: 12px; border-bottom: 1px solid #000; padding-bottom: 3px; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <!-- BARRA DE NAVEGAÇÃO -->
    <nav class="border-b border-opacity-20 mb-6 sticky top-0 z-40 backdrop-blur-md" style="background-color: var(--bg-card); border-color: var(--border-color)">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-[10px] uppercase tracking-wider opacity-60">Plataforma Profissional</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="toggleView('dashboard')" id="btn-nav-dashboard" class="px-4 py-2 rounded-lg text-sm font-medium transition hover:bg-black hover:bg-opacity-10 border border-transparent">
                    <i class="fas fa-folder-open mr-2"></i> Fichas Salvas
                </button>
                <button onclick="toggleView('editor')" id="btn-nav-editor" class="px-4 py-2 rounded-lg text-sm font-medium transition bg-primary text-white shadow-md">
                    <i class="fas fa-edit mr-2"></i> Nova Ficha
                </button>
            </div>
        </div>
    </nav>

    <!-- PAINEL: FICHAS SALVAS (MEMÓRIA) -->
    <div id="dashboard-view" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8 hidden">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold"><i class="fas fa-database text-primary mr-2"></i> Fichas de Treino Salvas</h2>
        </div>
        
        <div id="saved-sheets-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <!-- Renderizado via JS -->
        </div>
    </div>


    <!-- PAINEL: EDITOR DE FICHA -->
    <div id="editor-view" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Cabeçalho Ações -->
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-xl font-bold" id="editor-title">Criando Nova Ficha</h2>
            <button onclick="generatePrint()" class="btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center gap-2">
                <i class="fas fa-print"></i> Salvar e Imprimir
            </button>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Card: Dados do Profissional -->
                <div class="card rounded-xl p-5 shadow-sm border-l-4" style="border-left-color: var(--primary)">
                    <h2 class="text-md font-semibold mb-3 flex items-center gap-2">
                        <i class="fas fa-id-badge text-primary"></i> Dados do Personal Trainer
                    </h2>
                    <div class="space-y-3">
                        <div>
                            <input type="text" id="prof-name" onchange="saveProfData()" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Nome do Personal">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <input type="text" id="prof-cref" onchange="saveProfData()" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="CREF (Ex: 008094)">
                            <input type="text" id="prof-state" onchange="saveProfData()" class="input-field w-full rounded px-3 py-2 text-sm" placeholder="Estado (Ex: G/RN)">
                        </div>
                    </div>
                </div>

                <!-- Card: Dados do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-md font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Dados do Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-4">
                        <div>
                            <input type="text" id="stu-name" class="input-field w-full rounded px-3 py-2 text-sm font-bold" placeholder="Nome Completo do Aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded px-2 py-2 text-sm text-center">
                            </div>
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-2 text-sm text-center">
                            </div>
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded px-2 py-2 text-sm text-center">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Gênero</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded px-2 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded px-2 py-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-[10px] uppercase opacity-70 mb-1">Objetivo Primário</label>
                            <select id="stu-objective" class="input-field w-full rounded px-3 py-2 text-sm font-medium text-primary">
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
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Frequência</label>
                                <select id="stu-freq" class="input-field w-full rounded px-2 py-2 text-sm">
                                    <option>3 dias</option>
                                    <option>5 dias</option>
                                    <option>6 dias</option>
                                    <option>Personalizado</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] uppercase opacity-70 mb-1">Validade</label>
                                <select id="stu-validity" class="input-field w-full rounded px-2 py-2 text-sm">
                                    <option>15 dias</option>
                                    <option selected>30 dias</option>
                                    <option>60 dias</option>
                                    <option>90 dias</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Card: Estado de Saúde -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-md font-semibold mb-2 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-heartbeat text-primary"></i> Estado de Saúde
                    </h2>
                    <div class="grid grid-cols-1 gap-1 text-xs" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="lg:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                    <div>
                        <h2 class="text-lg font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Rotina de Treinos
                        </h2>
                        <p class="text-xs opacity-70 mt-1">Montagem 100% manual. Adicione os dias da semana.</p>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 w-full sm:w-auto justify-center">
                        <i class="fas fa-plus"></i> Novo Dia / Treino
                    </button>
                </div>

                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos gerados via JS -->
                </div>
                
                <!-- Recomendações Finais Livres -->
                <div class="card rounded-xl p-5 shadow-sm mt-6">
                    <h2 class="text-md font-semibold mb-3 flex items-center gap-2">
                        <i class="fas fa-comment-dots text-primary"></i> Recomendações do Profissional (Texto Livre)
                    </h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: Cuidado com a hidratação, descanse 1 min entre aparelhos..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col h-[85vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Banco de Exercícios</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors">
                    <i class="fas fa-times text-xl"></i>
                </button>
            </div>
            
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO PLANILHA -->
    <div id="print-area"></div>

    <script>
        // --- BASE DE DADOS (Atualizada c/ Solicitações) ---
        const healthData = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas de postura. Manter hidratação e intensidade leve a moderada.",
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
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Superávit calórico necessário.",
            "Definição": "Manutenção de massa muscular reduzindo % de gordura. Foco na dieta.",
            "Condicionamento": "Treinos com menor intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries longas (15+ reps), cadência controlada.",
            "Força": "Cargas altas, baixas repetições (1-5) e intervalos maiores.",
            "Reabilitação": "Fortalecimento específico e mobilidade. Respeitar limites de dor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade."
        };

        const dbCategories = {
            "🔥 PEITO": ["Supino Reto", "Supino Inclinado", "Supino com Halteres", "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado", "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"],
            "🦍 COSTAS": ["Puxada Alta", "Puxada Unilateral", "Pulldown", "Remada Aberta", "Remada Baixa", "Remada Curvada", "Remada Curvada na Polia", "Remada Unilateral", "Remada Cavalinho (T-Bar)", "Remada no Cross", "Serrote", "Facepull (puxada de cima para baixo)", "Encolhimento (Trapézio)"],
            "🦵 PERNAS": ["Agachamento Livre", "Agachamento Taça", "Agachamento com Barra", "Agachamento no Smith", "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", "Elevação Pélvica", "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Abdução no Cross (Glúteo Médio + Mínimo)", "Coice", "Cachorrinho", "Caranguejo", "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"],
            "💪 BRAÇOS": ["(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°", "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"],
            "🪨 OMBROS": ["Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral na Polia", "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta"],
            "🧠 ABDÔMEN": ["Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede", "Abdominal isometrico"],
            "🫀 CARDIO": ["Bicicleta - 10 Minutos", "Bicicleta - 15 Minutos", "Bicicleta - 20 Minutos", "Esteira - 10 Minutos", "Esteira - 15 Minutos", "Esteira - 20 Minutos", "Pular Corda - 10 Minutos", "Pular Corda - 15 Minutos", "Pular Corda - 20 Minutos"]
        };

        const dbTechniques = ["-", "Drop set", "Bi-set", "Tri-set", "Série gigante", "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", "Negativa", "Isometria", "Parciais", "Pirâmide"];
        const defaultDays = ["Treino Segunda", "Treino Terça", "Treino Quarta", "Treino Quinta", "Treino Sexta", "Treino Sábado"];

        // --- ESTADO DA APLICAÇÃO ---
        let currentSheetId = generateId();
        let state = {
            workouts: [ { id: generateId(), title: "Treino Segunda", exercises: [] } ],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- INICIALIZAÇÃO E MEMÓRIA ---
        function init() {
            renderHealthOptions();
            loadProfData();
            toggleView('editor');
            renderSavedSheets();
        }

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        function toggleView(view) {
            document.getElementById('dashboard-view').classList.add('hidden');
            document.getElementById('editor-view').classList.add('hidden');
            document.getElementById('btn-nav-dashboard').classList.remove('bg-primary', 'text-white', 'shadow-md');
            document.getElementById('btn-nav-editor').classList.remove('bg-primary', 'text-white', 'shadow-md');

            if(view === 'dashboard') {
                document.getElementById('dashboard-view').classList.remove('hidden');
                document.getElementById('btn-nav-dashboard').classList.add('bg-primary', 'text-white', 'shadow-md');
                renderSavedSheets();
            } else {
                document.getElementById('editor-view').classList.remove('hidden');
                document.getElementById('btn-nav-editor').classList.add('bg-primary', 'text-white', 'shadow-md');
                renderWorkouts();
            }
        }

        // --- DADOS DO PROFISSIONAL ---
        function saveProfData() {
            const data = {
                name: document.getElementById('prof-name').value,
                cref: document.getElementById('prof-cref').value,
                state: document.getElementById('prof-state').value
            };
            localStorage.setItem('powfit_prof', JSON.stringify(data));
        }
        function loadProfData() {
            const data = JSON.parse(localStorage.getItem('powfit_prof') || '{}');
            if(data.name) document.getElementById('prof-name').value = data.name;
            if(data.cref) document.getElementById('prof-cref').value = data.cref;
            if(data.state) document.getElementById('prof-state').value = data.state;
        }

        // --- SALVAMENTO DE FICHAS DE ALUNOS ---
        function getSavedSheets() {
            return JSON.parse(localStorage.getItem('powfit_sheets') || '[]');
        }

        function saveCurrentSheetToMemory() {
            const sheet = {
                id: currentSheetId,
                date: new Date().toLocaleDateString('pt-BR'),
                student: {
                    name: document.getElementById('stu-name').value || 'Sem Nome',
                    age: document.getElementById('stu-age').value,
                    weight: document.getElementById('stu-weight').value,
                    height: document.getElementById('stu-height').value,
                    gender: document.getElementById('stu-gender').value,
                    level: document.getElementById('stu-level').value,
                    objective: document.getElementById('stu-objective').value,
                    freq: document.getElementById('stu-freq').value,
                    validity: document.getElementById('stu-validity').value,
                    recs: document.getElementById('stu-recs').value,
                    health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value)
                },
                workouts: state.workouts
            };

            let sheets = getSavedSheets();
            const index = sheets.findIndex(s => s.id === currentSheetId);
            if(index >= 0) sheets[index] = sheet;
            else sheets.push(sheet);
            
            localStorage.setItem('powfit_sheets', JSON.stringify(sheets));
            renderSavedSheets();
        }

        function loadSheet(id) {
            const sheet = getSavedSheets().find(s => s.id === id);
            if(!sheet) return;

            currentSheetId = sheet.id;
            document.getElementById('editor-title').innerText = `Editando Ficha: ${sheet.student.name}`;
            
            // Popula form
            document.getElementById('stu-name').value = sheet.student.name;
            document.getElementById('stu-age').value = sheet.student.age;
            document.getElementById('stu-weight').value = sheet.student.weight;
            document.getElementById('stu-height').value = sheet.student.height;
            document.getElementById('stu-gender').value = sheet.student.gender;
            document.getElementById('stu-level').value = sheet.student.level;
            document.getElementById('stu-objective').value = sheet.student.objective;
            document.getElementById('stu-freq').value = sheet.student.freq;
            document.getElementById('stu-validity').value = sheet.student.validity;
            document.getElementById('stu-recs').value = sheet.student.recs;
            
            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = sheet.student.health.includes(cb.value);
            });

            state.workouts = sheet.workouts;
            calculateIMC();
            changeTheme();
            toggleView('editor');
        }

        function deleteSheet(id) {
            if(confirm('Tem certeza que deseja excluir esta ficha da memória?')) {
                let sheets = getSavedSheets().filter(s => s.id !== id);
                localStorage.setItem('powfit_sheets', JSON.stringify(sheets));
                if(currentSheetId === id) startNewSheet();
                renderSavedSheets();
            }
        }

        function startNewSheet() {
            currentSheetId = generateId();
            document.getElementById('editor-title').innerText = `Criando Nova Ficha`;
            document.getElementById('stu-name').value = '';
            document.getElementById('stu-age').value = '';
            document.getElementById('stu-weight').value = '';
            document.getElementById('stu-height').value = '';
            document.getElementById('stu-recs').value = '';
            document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
            calculateIMC();
            state.workouts = [{ id: generateId(), title: "Treino Segunda", exercises: [] }];
            toggleView('editor');
        }

        function renderSavedSheets() {
            const container = document.getElementById('saved-sheets-container');
            const sheets = getSavedSheets();
            
            if(sheets.length === 0) {
                container.innerHTML = `<div class="col-span-full p-8 text-center opacity-50 border border-dashed rounded-xl" style="border-color: var(--border-color)">Nenhuma ficha salva no sistema ainda.</div>`;
                return;
            }

            container.innerHTML = sheets.map(s => `
                <div class="card p-4 rounded-xl shadow-sm flex flex-col gap-3">
                    <div class="flex justify-between items-start">
                        <div>
                            <h3 class="font-bold text-lg">${s.student.name}</h3>
                            <p class="text-xs opacity-70">Salvo em: ${s.date} | Objetivo: ${s.student.objective}</p>
                        </div>
                        <span class="bg-primary bg-opacity-20 text-primary px-2 py-1 rounded text-xs font-bold">${s.workouts.length} Treinos</span>
                    </div>
                    <div class="flex gap-2 mt-auto pt-3 border-t border-opacity-20" style="border-color: var(--border-color)">
                        <button onclick="loadSheet('${s.id}')" class="flex-1 bg-black bg-opacity-10 hover:bg-primary hover:text-white transition py-1.5 rounded text-sm font-medium"><i class="fas fa-edit"></i> Editar</button>
                        <button onclick="deleteSheet('${s.id}')" class="px-3 py-1.5 rounded bg-red-500 bg-opacity-10 text-red-500 hover:bg-red-500 hover:text-white transition"><i class="fas fa-trash"></i></button>
                    </div>
                </div>
            `).join('');
        }


        // --- LÓGICA DE INTERFACE ---
        function changeTheme() {
            document.body.setAttribute('data-theme', document.getElementById('stu-gender').value);
        }

        function calculateIMC() {
            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            const display = document.getElementById('imc-display');

            if (w > 0 && h > 0) {
                const imc = (w / (h * h)).toFixed(1);
                let classif = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else { display.innerHTML = ''; }
        }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = Object.keys(healthData).map(opt => `
                <label class="flex items-center space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent">
                    <input type="checkbox" value="${opt}" class="health-cb rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>
            `).join('');
        }

        // --- TREINOS E EXERCÍCIOS ---
        function getNextDayTitle() {
            const count = state.workouts.length;
            return defaultDays[count % defaultDays.length];
        }

        function addWorkout() {
            state.workouts.push({ id: generateId(), title: getNextDayTitle(), exercises: [] });
            renderWorkouts();
        }

        function duplicateWorkout(id) {
            const w = state.workouts.find(w => w.id === id);
            if(w) {
                const newW = JSON.parse(JSON.stringify(w));
                newW.id = generateId();
                newW.title = newW.title + " (Cópia)";
                state.workouts.push(newW);
                renderWorkouts();
            }
        }

        function removeWorkout(id) {
            if(confirm("Remover este treino inteiro?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        }

        function updateWorkoutTitle(id, val) {
            const w = state.workouts.find(w => w.id === id);
            if(w) w.title = val;
        }

        function moveExercise(wId, idx, dir) {
            const w = state.workouts.find(w => w.id === wId);
            if (dir === 'up' && idx > 0) [w.exercises[idx], w.exercises[idx-1]] = [w.exercises[idx-1], w.exercises[idx]];
            else if (dir === 'down' && idx < w.exercises.length-1) [w.exercises[idx], w.exercises[idx+1]] = [w.exercises[idx+1], w.exercises[idx]];
            renderWorkouts();
        }

        function removeExercise(wId, idx) {
            state.workouts.find(w => w.id === wId).exercises.splice(idx, 1);
            renderWorkouts();
        }

        function updateExercise(wId, idx, field, val) {
            state.workouts.find(w => w.id === wId).exercises[idx][field] = val;
        }

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            state.workouts.forEach(workout => {
                let exercisesHtml = workout.exercises.length === 0 ? 
                    `<div class="text-center p-4 text-xs opacity-50 italic">Sem exercícios adicionados.</div>` : 
                    workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col lg:flex-row gap-2 p-2 items-start lg:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex lg:flex-col gap-1 hidden lg:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1 hover:bg-black hover:bg-opacity-10 rounded"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1 hover:bg-black hover:bg-opacity-10 rounded"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 min-w-[200px]">
                                <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                                <div class="text-sm font-medium leading-tight">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1 w-full lg:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-xs">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 lg:w-32 rounded px-2 py-1 text-xs" placeholder="Obs...">
                            </div>
                            <div class="flex items-center gap-1 w-full lg:w-auto justify-end">
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 p-1.5 rounded hover:bg-red-500 hover:bg-opacity-10 transition"><i class="fas fa-trash text-xs"></i></button>
                            </div>
                        </div>
                    `).join('');

                container.insertAdjacentHTML('beforeend', `
                    <div class="card rounded-xl overflow-hidden shadow-sm border-l-2" style="border-left-color: var(--primary)">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color)">
                            <div class="flex items-center gap-2 flex-1">
                                <i class="fas fa-calendar-day text-primary"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-md w-full px-1 py-0.5 rounded border-transparent">
                            </div>
                            <div class="flex gap-1">
                                <button onclick="duplicateWorkout('${workout.id}')" class="p-1.5 rounded hover:bg-black hover:bg-opacity-10"><i class="fas fa-copy text-xs"></i></button>
                                <button onclick="removeWorkout('${workout.id}')" class="p-1.5 text-red-500 rounded hover:bg-red-500 hover:text-white"><i class="fas fa-trash text-xs"></i></button>
                            </div>
                        </div>
                        <div class="p-0">${exercisesHtml}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-1.5 rounded text-xs font-medium bg-opacity-10 text-primary border border-primary border-dashed hover:border-solid transition">
                                + Adicionar Exercício
                            </button>
                        </div>
                    </div>
                `);
            });
        }

        // --- MODAL ---
        function openModal(wId) {
            state.activeModalWorkoutId = wId;
            document.getElementById('exercise-modal').classList.remove('hidden');
            renderModalCategories();
            renderModalExercises();
        }
        function closeModal() { document.getElementById('exercise-modal').classList.add('hidden'); }

        function renderModalCategories() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="state.activeCategory='${cat}'; renderModalCategories(); renderModalExercises();" class="w-full text-left px-3 py-2 rounded text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }

        function renderModalExercises() {
            const isCardio = state.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 gap-2">` + 
                dbCategories[state.activeCategory].map(ex => `
                    <button onclick="addExercise('${ex}', ${isCardio})" class="card p-2.5 rounded text-left text-xs font-medium border hover:border-primary transition flex justify-between group">
                        <span>${ex}</span> <i class="fas fa-plus text-primary opacity-0 group-hover:opacity-100"></i>
                    </button>
                `).join('') + `</div>`;
        }

        function addExercise(name, isCardio) {
            const w = state.workouts.find(w => w.id === state.activeModalWorkoutId);
            if(w) {
                w.exercises.push({ category: state.activeCategory, name: name, sets: isCardio?'1':'3', reps: isCardio?'-':'10 a 12', technique: '-', obs: '' });
                renderWorkouts();
            }
        }

        // --- IMPRESSÃO PLANILHA ---
        function generatePrint() {
            // Salva na memória sempre que for imprimir
            saveCurrentSheetToMemory();

            const pName = document.getElementById('prof-name').value || 'Não informado';
            const pCref = document.getElementById('prof-cref').value || 'Não informado';
            const pState = document.getElementById('prof-state').value || '-';
            const sName = document.getElementById('stu-name').value || 'Aluno não identificado';
            const sAge = document.getElementById('stu-age').value || '-';
            const sWeight = document.getElementById('stu-weight').value || '-';
            const sHeight = document.getElementById('stu-height').value || '-';
            const w = parseFloat(sWeight); const h = parseFloat(sHeight);
            const imcStr = (w>0 && h>0) ? (w/(h*h)).toFixed(1) : '-';
            
            const obj = document.getElementById('stu-objective').value;
            const health = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);

            let html = `
                <div class="print-header-box">
                    <h1>Prescrição de Treinamento Individualizada</h1>
                    <p>PERSONAL TRAINER: ${pName}</p>
                    <p style="font-weight: normal; font-size: 11px;">CREF: ${pCref} - Estado: ${pState}</p>
                </div>

                <table class="print-student-table">
                    <tr>
                        <th>Aluno(a)</th><td colspan="3"><strong>${sName}</strong></td>
                        <th>Objetivo</th><td>${obj}</td>
                    </tr>
                    <tr>
                        <th>Idade / Gênero</th><td>${sAge} anos / ${document.getElementById('stu-gender').value}</td>
                        <th>Peso / Altura</th><td>${sWeight} kg / ${sHeight} m (IMC: ${imcStr})</td>
                        <th>Nível / Freq.</th><td>${document.getElementById('stu-level').value} / ${document.getElementById('stu-freq').value}</td>
                    </tr>
                </table>
            `;

            if(objectiveData[obj] || health.length > 0) {
                html += `<div class="print-guidelines"><h4>DIRETRIZES DE SAÚDE E OBJETIVO</h4><ul>`;
                html += `<li><strong>${obj}:</strong> ${objectiveData[obj]}</li>`;
                health.forEach(h => html += `<li><strong>${h.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim()}:</strong> ${healthData[h]}</li>`);
                html += `</ul></div>`;
            }

            state.workouts.forEach(w => {
                html += `<div class="print-workout"><h3>${w.title}</h3><table class="workout-table">
                    <thead><tr><th>Exercício</th><th style="width:10%">Séries</th><th style="width:15%">Reps</th><th style="width:10%">Técnica</th><th>Observações</th></tr></thead><tbody>`;
                
                if(w.exercises.length === 0) html += `<tr><td colspan="5" style="text-align:center; color:#666;">Sem exercícios cadastrados</td></tr>`;
                w.exercises.forEach(ex => {
                    html += `<tr><td>${ex.name}</td><td>${ex.sets}</td><td>${ex.reps}</td><td>${ex.technique}</td><td>${ex.obs||'-'}</td></tr>`;
                });
                html += `</tbody></table></div>`;
            });

            const recs = document.getElementById('stu-recs').value;
            if(recs.trim()) {
                html += `<div class="print-manual-recs"><h4>Observações Gerais do Profissional</h4><p style="white-space:pre-wrap; margin:0; font-size:11px;">${recs}</p></div>`;
            }

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 250);
        }

        // Boot
        init();
    </script>
</body>
</html>
