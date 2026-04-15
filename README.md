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

        .card { background-color: var(--bg-card); border: 1px solid var(--border-color); }
        .input-field { background-color: var(--input-bg); border: 1px solid var(--input-border); color: var(--text-main); }
        .input-field:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 1px var(--primary); }
        .btn-primary { background-color: var(--primary); color: white; transition: background-color 0.2s; }
        .btn-primary:hover { background-color: var(--primary-hover); }
        .text-primary { color: var(--primary); }

        /* Oculta impressão na tela */
        #print-area { display: none; }

        /* Estilos A4 Lado a Lado (1 Página) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #records-modal, #exercise-modal, .no-print { display: none !important; }
            #print-area { display: block !important; padding: 10mm; font-family: 'Inter', sans-serif; font-size: 9px; box-sizing: border-box;}
            @page { size: A4 landscape; margin: 0; } /* Paisagem ajuda a colocar mais dias lado a lado, mas pode ser mudado no navegador */
            
            .print-header { border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: flex-end; }
            .print-prof-title { font-size: 14px; font-weight: bold; margin: 0; text-transform: uppercase; color: #1f2937; }
            .print-prof-subtitle { font-size: 10px; color: #4b5563; margin-top: 2px; }
            
            .print-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; margin-bottom: 10px; background: #f9fafb; padding: 8px; border: 1px solid #e5e7eb; border-radius: 4px; }
            .print-grid div { margin-bottom: 2px; }

            .print-guidelines { border-left: 3px solid #3b82f6; background: #f0fdf4; padding: 6px; margin-bottom: 10px; font-size: 8.5px; }
            .print-guidelines h4 { margin: 0 0 3px 0; font-size: 9px; }
            .print-guidelines ul { margin: 0; padding-left: 15px; }

            /* Grid para dias lado a lado */
            .print-workouts-container { display: flex; gap: 8px; align-items: flex-start; justify-content: space-between; width: 100%; margin-bottom: 10px;}
            .print-workout { flex: 1; min-width: 0; page-break-inside: avoid; }
            .print-workout h3 { background: #1f2937; color: white; padding: 4px; margin: 0 0 4px 0; font-size: 10px; text-align: center; border-radius: 2px;}
            
            table { width: 100%; border-collapse: collapse; }
            th, td { border: 1px solid #d1d5db; padding: 3px; text-align: left; font-size: 8.5px; word-wrap: break-word; }
            th { background-color: #f3f4f6; font-weight: 600; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            .print-recommendations { background: #fdf8f6; border: 1px solid #fed7aa; padding: 8px; border-radius: 4px; font-size: 8.5px; page-break-inside: avoid;}
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 3px; }
    </style>
</head>
<body data-theme="Masculino" class="min-h-screen pb-20">

    <div id="app-container" class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <!-- Header & Ações -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-6 gap-4">
            <div class="flex items-center gap-3">
                <i class="fas fa-dumbbell text-3xl text-primary"></i>
                <div>
                    <h1 class="text-2xl font-bold tracking-tight">PowFit Pro</h1>
                    <p class="text-xs var(--text-muted)">Plataforma Profissional</p>
                </div>
            </div>
            <div class="flex gap-3 w-full sm:w-auto">
                <button onclick="openRecords()" class="flex-1 sm:flex-none bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg font-medium shadow-md flex items-center justify-center gap-2 transition">
                    <i class="fas fa-folder-open"></i> Fichas Salvas
                </button>
                <button onclick="generatePrint()" class="flex-1 sm:flex-none btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center justify-center gap-2">
                    <i class="fas fa-print"></i> Salvar e Imprimir
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA -->
            <div class="lg:col-span-4 space-y-6">
                
                <!-- Cadastro do Profissional -->
                <div class="card rounded-xl p-5 shadow-sm border-l-4 border-l-primary">
                    <h2 class="text-sm font-semibold mb-3 flex items-center gap-2 opacity-80">
                        <i class="fas fa-id-badge text-primary"></i> Dados do Profissional (Cabeçalho)
                    </h2>
                    <div class="space-y-3">
                        <div>
                            <input type="text" id="prof-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do Personal Trainer">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <input type="text" id="prof-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="CREF (Ex: 008094 - G)">
                            <select id="prof-state" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option value="">Estado (UF)</option>
                                <option value="AC">AC</option><option value="AL">AL</option><option value="AP">AP</option><option value="AM">AM</option><option value="BA">BA</option><option value="CE">CE</option><option value="DF">DF</option><option value="ES">ES</option><option value="GO">GO</option><option value="MA">MA</option><option value="MT">MT</option><option value="MS">MS</option><option value="MG">MG</option><option value="PA">PA</option><option value="PB">PB</option><option value="PR">PR</option><option value="PE">PE</option><option value="PI">PI</option><option value="RJ">RJ</option><option value="RN">RN</option><option value="RS">RS</option><option value="RO">RO</option><option value="RR">RR</option><option value="SC">SC</option><option value="SP">SP</option><option value="SE">SE</option><option value="TO">TO</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Cadastro do Aluno -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <div class="flex justify-between items-center mb-4 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <h2 class="text-lg font-semibold flex items-center gap-2">
                            <i class="fas fa-user text-primary"></i> Dados do Aluno
                        </h2>
                        <div id="imc-display"></div>
                    </div>
                    
                    <div class="space-y-3">
                        <input type="hidden" id="record-id">
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Nome Completo</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome do aluno">
                        </div>
                        <div class="grid grid-cols-3 gap-2">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 25">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 75">
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Altura (m)</label>
                                <input type="number" id="stu-height" step="0.01" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 1.75">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Gênero</label>
                                <select id="stu-gender" onchange="changeTheme()" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option value="Masculino">Masculino</option>
                                    <option value="Feminino">Feminino</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium mb-1 opacity-70">Nível</label>
                                <select id="stu-level" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                    <option>Iniciante</option>
                                    <option>Intermediário</option>
                                    <option>Avançado</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-medium mb-1 opacity-70">Objetivo (Gera recomendação)</label>
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
                            <label class="block text-xs font-medium mb-1 opacity-70">Frequência Semanal</label>
                            <select id="stu-freq" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>3 dias</option>
                                <option>5 dias</option>
                                <option>6 dias</option>
                                <option>Personalizado</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Estado de Saúde -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-3 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-notes-medical text-primary"></i> Estado de Saúde
                    </h2>
                    <div class="grid grid-cols-1 gap-1.5 text-xs h-48 overflow-y-auto pr-2" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

                <!-- Recomendações Extras -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-3 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-pen text-primary"></i> Recomendações Manuais
                    </h2>
                    <div class="space-y-3">
                        <div>
                            <select id="stu-validity" class="input-field w-full rounded-lg px-3 py-2 text-sm">
                                <option>15 dias</option>
                                <option selected>30 dias</option>
                                <option>60 dias</option>
                                <option>90 dias</option>
                            </select>
                        </div>
                        <div>
                            <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Escreva observações livres (Hidratação, descanso, etc)..."></textarea>
                        </div>
                    </div>
                </div>
            </div>

            <!-- COLUNA DIREITA: Treinos -->
            <div class="lg:col-span-8 space-y-4">
                
                <div class="flex justify-between items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2">
                    <div>
                        <h2 class="text-lg font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem da Ficha
                        </h2>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2">
                        <i class="fas fa-plus"></i> Adicionar Dia de Treino
                    </button>
                </div>

                <div id="workouts-container" class="space-y-4">
                    <!-- Treinos gerados via JS -->
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Adicionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors"><i class="fas fa-times text-xl"></i></button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-3 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-4 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- MODAL FICHAS SALVAS (MEMÓRIA) -->
    <div id="records-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-50 flex items-center justify-center p-4">
        <div class="card w-full max-w-3xl rounded-2xl shadow-2xl flex flex-col max-h-[80vh]">
            <div class="p-5 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-xl font-bold"><i class="fas fa-folder-open text-primary mr-2"></i> Fichas Salvas na Memória</h3>
                <button onclick="closeRecords()" class="text-gray-400 hover:text-red-500 transition-colors"><i class="fas fa-times text-2xl"></i></button>
            </div>
            <div class="p-5 overflow-y-auto flex-1 space-y-3" id="records-container">
                <!-- Preenchido via JS -->
            </div>
        </div>
    </div>

    <!-- ÁREA DE IMPRESSÃO -->
    <div id="print-area"></div>

    <script>
        // --- BANCO DE DADOS ---
        const healthData = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Alimentação adequada.",
            "🍬 Diabetes": "Monitorar glicemia e evitar treinos em jejum.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar máquinas e controle.",
            "🫁 Problemas respiratórios": "Progressão gradual. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Focar na recuperação.",
            "🤰 Gestante": "Treinos leves sem impacto. Foco em mobilidade.",
            "🤱 Lactante": "Treinar com atenção à hidratação e energia.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade com segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos (força + aeróbicos).",
            "Hipertrofia": "Prioridade na progressão de carga, volume e superávit calórico.",
            "Definição": "Manutenção muscular com redução de percentual de gordura.",
            "Condicionamento": "Treinos com menor tempo de intervalo e circuitos.",
            "Resistência": "Séries mais longas e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Fortalecimento específico, mobilidade e respeito à dor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade constante."
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
                "Agachamento Sumô", "Agachamento Sissy (Livre)", "Afundo", "Recuo", "Avanço", 
                "Passada", "Búlgaro", "Step-up", "Levantamento Terra", "Levantamento Terra Romeno", 
                "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                "Elevação Pélvica", "Extensão de Quadril", "Extensão Cruzada (Médio)", 
                "Abdução no Cross", "Coice", "Cachorrinho", "Caranguejo", 
                "Cadeira Extensora", "Adução", "Abdução", "Cadeira Abdutora Inclinada", 
                "Flexão Nórdica", "Flexão Nórdica Invertida", "Panturrilha em Pé (Máquina)", 
                "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", 
                "Panturrilha Squat", "Panturrilha Unilateral"
            ],
            "💪 BRAÇOS": [
                "(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", 
                "(Bíceps) Rosca Scott Barra W", "(Bíceps) Rosca Scott Unilateral", 
                "(Bíceps) Rosca Scott Halteres", "(Bíceps) Rosca Martelo", "(Bíceps) Rosca Cross", 
                "(Bíceps) Rosca Concentrada", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°",
                "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", 
                "(Tríceps) Pulley Pegada Inversa", "(Tríceps) Francês Corda", "(Tríceps) Francês Halter", 
                "(Tríceps) Francês Unilateral", "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice", 
                "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho Banco"
            ],
            "🪨 OMBROS": [
                "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", 
                "Elevação Lateral na Polia", "Elevação Lateral Sentado", "Desenvolvimento Halteres", 
                "Desenvolvimento com Barra", "Arnold Press", "Elevação Borboleta", 
                "Crucifixo Inverso Sentado", "Crucifixo Inverso Polia", 
                "Crucifixo Inverso Unilateral", "Facepull (puxada reta)", "Remada Alta"
            ],
            "🧠 ABDÔMEN": [
                "Infra com Elevação", "Abdominal Supra", "Abdominal Remador", 
                "Abdominal Bicicleta", "Abdominal Twister", "Prancha", 
                "Prancha Lateral", "Trituração de Cabos", "Isometria na parede", "Abd isometrico"
            ],
            "🫀 CARDIO": [
                "Bicicleta - 10 Min", "Bicicleta - 15 Min", "Bicicleta - 20 Min",
                "Esteira - 10 Min", "Esteira - 15 Min", "Esteira - 20 Min",
                "Pular Corda - 10 Min", "Pular Corda - 15 Min", "Pular Corda - 20 Min"
            ]
        };

        const dbTechniques = [
            "Normal", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
            "Rest-pause", "FST-7", "Pré-exaust", "Pós-exaust", 
            "Negativa", "Isometria", "Parciais", "Pirâmide"
        ];

        const daysOfWeek = ["Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado", "Domingo"];

        // --- ESTADO ---
        let state = {
            workouts: [ { id: generateId(), title: "Treino Segunda", exercises: [] } ],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- INICIALIZAÇÃO E UI ---
        function init() {
            renderHealthOptions();
            renderWorkouts();
            loadProfData(); // Carrega dados do prof q ficam salvos no navegador
        }

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
                let classif = imc < 18.5 ? "Abaixo" : imc < 24.9 ? "Normal" : imc < 29.9 ? "Sobrepeso" : "Obesidade";
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-2 py-1 rounded text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else { display.innerHTML = ''; }
        }

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        function renderHealthOptions() {
            const container = document.getElementById('health-container');
            container.innerHTML = Object.keys(healthData).map(opt => `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent">
                    <input type="checkbox" value="${opt}" class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium truncate">${opt}</span>
                </label>
            `).join('');
        }

        // --- TREINOS ---
        function getNextDay() {
            const count = state.workouts.length;
            return `Treino ${daysOfWeek[count % 7]}`;
        }

        function addWorkout() {
            state.workouts.push({ id: generateId(), title: getNextDay(), exercises: [] });
            renderWorkouts();
        }

        function removeWorkout(id) {
            if(confirm("Remover este dia de treino?")) {
                state.workouts = state.workouts.filter(w => w.id !== id);
                renderWorkouts();
            }
        }

        function updateWorkoutTitle(id, newTitle) {
            const w = state.workouts.find(w => w.id === id);
            if(w) w.title = newTitle;
        }

        // --- EXERCÍCIOS ---
        function removeExercise(wId, exIndex) {
            state.workouts.find(w => w.id === wId).exercises.splice(exIndex, 1);
            renderWorkouts();
        }
        function moveExercise(wId, exIndex, dir) {
            const w = state.workouts.find(w => w.id === wId);
            if (dir === 'up' && exIndex > 0) {
                [w.exercises[exIndex], w.exercises[exIndex-1]] = [w.exercises[exIndex-1], w.exercises[exIndex]];
            } else if (dir === 'down' && exIndex < w.exercises.length - 1) {
                [w.exercises[exIndex], w.exercises[exIndex+1]] = [w.exercises[exIndex+1], w.exercises[exIndex]];
            }
            renderWorkouts();
        }
        function updateExercise(wId, exIndex, field, val) {
            state.workouts.find(w => w.id === wId).exercises[exIndex][field] = val;
        }

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';
            state.workouts.forEach(workout => {
                let exercisesHtml = workout.exercises.length === 0 ? `<div class="text-center p-4 text-xs opacity-50 italic">Nenhum exercício.</div>` : 
                    workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10" style="border-color: var(--border-color)">
                            <div class="flex-1 w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase">${ex.category}</div>
                                <div class="text-sm font-medium leading-tight">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1.5 w-full sm:w-auto">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Sér">
                                <span class="self-center opacity-50 text-xs">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-xs">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-32 rounded px-1 py-1 text-xs" placeholder="Obs...">
                            </div>
                            <div class="flex items-center gap-1 justify-end mt-1 sm:mt-0">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="p-1 rounded opacity-50 hover:opacity-100"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="p-1 rounded opacity-50 hover:opacity-100"><i class="fas fa-chevron-down"></i></button>
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 p-1 rounded"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                    `).join('');

                container.insertAdjacentHTML('beforeend', `
                    <div class="card rounded-lg overflow-hidden shadow-sm">
                        <div class="p-3 border-b flex justify-between items-center bg-black bg-opacity-5" style="border-color: var(--border-color);">
                            <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-base w-2/3 px-1 rounded">
                            <button onclick="removeWorkout('${workout.id}')" class="text-xs text-red-500 hover:text-white hover:bg-red-500 px-2 py-1 rounded transition"><i class="fas fa-trash"></i> Excluir Dia</button>
                        </div>
                        <div class="p-0">${exercisesHtml}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full btn-primary py-1.5 rounded text-xs font-medium border border-dashed border-opacity-50"><i class="fas fa-plus"></i> Inserir Exercício</button>
                        </div>
                    </div>
                `);
            });
        }

        // --- MODAL ---
        function openModal(wId) { state.activeModalWorkoutId = wId; document.getElementById('exercise-modal').classList.remove('hidden'); renderModalCategories(); renderModalExercises(); }
        function closeModal() { document.getElementById('exercise-modal').classList.add('hidden'); state.activeModalWorkoutId = null; }
        function renderModalCategories() {
            document.getElementById('modal-categories').innerHTML = Object.keys(dbCategories).map(cat => `
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2 rounded text-xs font-medium transition ${state.activeCategory === cat ? 'bg-primary text-white' : 'hover:bg-black hover:bg-opacity-10'}">${cat}</button>
            `).join('');
        }
        function setModalCategory(cat) { state.activeCategory = cat; renderModalCategories(); renderModalExercises(); }
        function renderModalExercises() {
            const exList = dbCategories[state.activeCategory];
            const isCardio = state.activeCategory === "🫀 CARDIO";
            document.getElementById('modal-exercises').innerHTML = `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2">
                ${exList.map(ex => `<button onclick="addExercise('${ex}', ${isCardio})" class="card p-2 rounded text-left text-xs font-medium border hover:border-primary transition flex justify-between"><span>${ex}</span><i class="fas fa-plus text-primary"></i></button>`).join('')}
            </div>`;
        }
        function addExercise(name, isCardio) {
            state.workouts.find(w => w.id === state.activeModalWorkoutId).exercises.push({
                category: state.activeCategory, name: name, sets: isCardio ? '1' : '3', reps: isCardio ? '-' : '10 a 12', technique: 'Normal', obs: ''
            });
            renderWorkouts();
        }

        // --- LOCAL STORAGE (MEMÓRIA) ---
        function loadProfData() {
            document.getElementById('prof-name').value = localStorage.getItem('powfit_prof_name') || '';
            document.getElementById('prof-cref').value = localStorage.getItem('powfit_prof_cref') || '';
            document.getElementById('prof-state').value = localStorage.getItem('powfit_prof_state') || '';
        }
        function saveProfData() {
            localStorage.setItem('powfit_prof_name', document.getElementById('prof-name').value);
            localStorage.setItem('powfit_prof_cref', document.getElementById('prof-cref').value);
            localStorage.setItem('powfit_prof_state', document.getElementById('prof-state').value);
        }

        function getSavedRecords() { return JSON.parse(localStorage.getItem('powfit_records') || '[]'); }
        
        function savePrescription() {
            saveProfData(); // Salva prof sempre
            
            const currentId = document.getElementById('record-id').value || generateId();
            const name = document.getElementById('stu-name').value || 'Sem Nome';
            
            const record = {
                id: currentId,
                date: new Date().toISOString(),
                student: {
                    name: name,
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
                workouts: JSON.parse(JSON.stringify(state.workouts))
            };

            let records = getSavedRecords();
            const existingIndex = records.findIndex(r => r.id === currentId);
            if(existingIndex >= 0) records[existingIndex] = record;
            else records.push(record);

            localStorage.setItem('powfit_records', JSON.stringify(records));
            document.getElementById('record-id').value = currentId; // seta id atual
        }

        function openRecords() {
            const records = getSavedRecords();
            const container = document.getElementById('records-container');
            if(records.length === 0) {
                container.innerHTML = '<p class="text-center opacity-50 py-10">Nenhuma ficha salva na memória.</p>';
            } else {
                records.sort((a,b) => new Date(b.date) - new Date(a.date));
                container.innerHTML = records.map(r => `
                    <div class="card p-4 rounded-lg flex justify-between items-center border-l-4 border-l-primary">
                        <div>
                            <h4 class="font-bold">${r.student.name}</h4>
                            <p class="text-xs opacity-70">Salvo em: ${new Date(r.date).toLocaleDateString('pt-BR')} - Obj: ${r.student.objective}</p>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="loadRecord('${r.id}')" class="bg-blue-500 hover:bg-blue-600 text-white px-3 py-1.5 rounded text-xs transition"><i class="fas fa-edit"></i> Editar</button>
                            <button onclick="deleteRecord('${r.id}')" class="bg-red-500 hover:bg-red-600 text-white px-3 py-1.5 rounded text-xs transition"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>
                `).join('');
            }
            document.getElementById('records-modal').classList.remove('hidden');
        }

        function closeRecords() { document.getElementById('records-modal').classList.add('hidden'); }

        function deleteRecord(id) {
            if(confirm("Excluir esta ficha permanentemente?")) {
                let records = getSavedRecords().filter(r => r.id !== id);
                localStorage.setItem('powfit_records', JSON.stringify(records));
                if(document.getElementById('record-id').value === id) document.getElementById('record-id').value = '';
                openRecords();
            }
        }

        function loadRecord(id) {
            const record = getSavedRecords().find(r => r.id === id);
            if(record) {
                document.getElementById('record-id').value = record.id;
                document.getElementById('stu-name').value = record.student.name;
                document.getElementById('stu-age').value = record.student.age;
                document.getElementById('stu-weight').value = record.student.weight;
                document.getElementById('stu-height').value = record.student.height;
                document.getElementById('stu-gender').value = record.student.gender;
                document.getElementById('stu-level').value = record.student.level;
                document.getElementById('stu-objective').value = record.student.objective;
                document.getElementById('stu-freq').value = record.student.freq;
                document.getElementById('stu-validity').value = record.student.validity;
                document.getElementById('stu-recs').value = record.student.recs;
                
                // Health boxes
                document.querySelectorAll('.health-cb').forEach(cb => cb.checked = false);
                if(record.student.health) {
                    record.student.health.forEach(h => {
                        const cb = document.querySelector(`.health-cb[value="${h}"]`);
                        if(cb) cb.checked = true;
                    });
                }
                
                state.workouts = record.workouts;
                changeTheme();
                calculateIMC();
                renderWorkouts();
                closeRecords();
            }
        }


        // --- IMPRESSÃO A4 (LADO A LADO) ---
        function generatePrint() {
            savePrescription(); // Salva automaticamente ao imprimir

            const profName = document.getElementById('prof-name').value || 'Não informado';
            const profCref = document.getElementById('prof-cref').value || '-';
            const profState = document.getElementById('prof-state').value || '-';

            const w = parseFloat(document.getElementById('stu-weight').value);
            const h = parseFloat(document.getElementById('stu-height').value);
            let imcStr = "-"; if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const obj = document.getElementById('stu-objective').value;
            const recsText = document.getElementById('stu-recs').value;
            const healthBoxes = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);

            let html = `
                <div class="print-header">
                    <div>
                        <h1 class="print-prof-title">PRESCRIÇÃO FEITA POR PERSONAL TRAINER: ${profName}</h1>
                        <p class="print-prof-subtitle">CREF: ${profCref} - ESTADO: ${profState}</p>
                    </div>
                    <div style="text-align: right; font-size: 9px; font-weight: bold;">
                        Data: ${new Date().toLocaleDateString('pt-BR')} | Validade: ${document.getElementById('stu-validity').value}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno(a):</strong> ${document.getElementById('stu-name').value || '-'}</div>
                    <div><strong>Objetivo:</strong> ${obj}</div>
                    <div><strong>Nível:</strong> ${document.getElementById('stu-level').value}</div>
                    <div><strong>Frequência:</strong> ${document.getElementById('stu-freq').value}</div>
                    <div><strong>Idade:</strong> ${document.getElementById('stu-age').value || '-'} anos</div>
                    <div><strong>Peso:</strong> ${document.getElementById('stu-weight').value || '-'} kg</div>
                    <div><strong>Altura:</strong> ${document.getElementById('stu-height').value || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                </div>
            `;

            // Diretrizes Automáticas
            if (objectiveData[obj] || healthBoxes.length > 0) {
                html += `<div class="print-guidelines"><h4>Diretrizes de Perfil (Automático)</h4><ul>`;
                html += `<li><strong>${obj}:</strong> ${objectiveData[obj]}</li>`;
                healthBoxes.forEach(k => {
                    const cleanKey = k.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                    html += `<li><strong>${cleanKey}:</strong> ${healthData[k]}</li>`;
                });
                html += `</ul></div>`;
            }

            // CONTAINER FLEX PARA DIAS LADO A LADO
            html += `<div class="print-workouts-container">`;
            
            state.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 45%">Exercício</th>
                                    <th style="width: 15%">S x R</th>
                                    <th style="width: 15%">Técn.</th>
                                    <th style="width: 25%">Obs</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                if (w.exercises.length === 0) {
                    html += `<tr><td colspan="4" style="text-align:center; color:#6b7280;">Vazio</td></tr>`;
                } else {
                    w.exercises.forEach(ex => {
                        const tech = ex.technique === 'Normal' ? '-' : ex.technique;
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td>${ex.sets}x${ex.reps}</td>
                                <td>${tech}</td>
                                <td>${ex.obs || '-'}</td>
                            </tr>
                        `;
                    });
                }
                html += `</tbody></table></div>`;
            });
            html += `</div>`; // Fechar flex container

            if(recsText.trim()) {
                html += `<div class="print-recommendations"><strong>Recomendações do Personal:</strong> ${recsText}</div>`;
            }

            document.getElementById('print-area').innerHTML = html;
            setTimeout(() => window.print(), 300);
        }

        init();
    </script>
</body>
</html>
