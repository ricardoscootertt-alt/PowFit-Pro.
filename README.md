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

        /* Oculta a área de impressão na tela */
        #print-area { display: none; }

        /* Estilos de Impressão (Estilo Planilha Profissional A4) */
        @media print {
            body { background: white !important; color: black !important; margin: 0; padding: 0; }
            #app-container, #history-modal, #exercise-modal, .no-print { display: none !important; }
            #print-area {
                display: block !important;
                padding: 10mm 15mm;
                font-family: 'Arial', sans-serif; /* Fonte estilo planilha */
                font-size: 10px;
            }
            @page { size: A4; margin: 0; }
            
            /* Cabeçalho */
            .print-header { border-bottom: 2px solid #000; padding-bottom: 8px; margin-bottom: 12px; text-align: center; }
            .print-title { font-size: 18px; font-weight: bold; margin: 0; text-transform: uppercase; }
            .prof-info { font-size: 12px; margin-top: 5px; font-weight: bold; }
            
            /* Grid Aluno */
            .print-grid {
                display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 4px;
                margin-bottom: 15px; border: 1px solid #000; padding: 8px; background: #fff;
            }
            .print-grid div { font-size: 11px; }
            
            /* Diretrizes Automáticas */
            .print-guidelines { border: 1px solid #000; padding: 8px; margin-bottom: 15px; font-size: 9.5px; }
            .print-guidelines h4 { margin: 0 0 4px 0; font-size: 11px; text-transform: uppercase; background: #eee; padding: 3px; border: 1px solid #000;}
            .print-guidelines ul { margin: 0; padding-left: 15px; }
            .print-guidelines li { margin-bottom: 3px; }

            /* Tabelas Estilo Planilha */
            .print-workout { margin-bottom: 15px; page-break-inside: avoid; }
            .print-workout h3 {
                background: #e0e0e0; border: 1px solid #000; border-bottom: none;
                padding: 4px 8px; margin: 0; font-size: 12px; text-transform: uppercase; font-weight: bold;
                -webkit-print-color-adjust: exact; color-adjust: exact;
            }
            table { width: 100%; border-collapse: collapse; margin-bottom: 0; border: 1px solid #000; }
            th, td { border: 1px solid #000; padding: 4px 6px; text-align: left; font-size: 10px; }
            th { background-color: #f0f0f0; font-weight: bold; text-transform: uppercase; -webkit-print-color-adjust: exact; color-adjust: exact; }
            
            /* Recomendações Livres */
            .print-recs { border: 1px solid #000; padding: 8px; margin-bottom: 15px; page-break-inside: avoid;}
            .print-recs h4 { margin: 0 0 5px 0; font-size: 11px; text-transform: uppercase;}
            
            /* Rodapé Legal */
            .legal-footer { margin-top: 15px; border-top: 2px solid #000; padding-top: 10px; font-size: 8px; color: #333; text-align: justify; page-break-inside: avoid;}
            .legal-footer strong { font-size: 9px; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 4px; }
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
            <div class="flex gap-2 w-full sm:w-auto">
                <button onclick="openHistory()" class="flex-1 sm:flex-none bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg font-medium shadow flex items-center justify-center gap-2 transition">
                    <i class="fas fa-history"></i> Memória
                </button>
                <button onclick="generatePrint()" class="flex-1 sm:flex-none btn-primary px-6 py-2.5 rounded-lg font-medium shadow-lg flex items-center justify-center gap-2">
                    <i class="fas fa-save"></i> <i class="fas fa-print"></i> Salvar & Imprimir
                </button>
            </div>
        </div>

        <div class="grid grid-cols-1 xl:grid-cols-12 gap-6">
            
            <!-- COLUNA ESQUERDA: Configurações -->
            <div class="xl:col-span-4 space-y-6">
                
                <!-- Card: Profissional -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-lg font-semibold mb-4 flex items-center gap-2 border-b border-opacity-20 pb-2" style="border-color: var(--border-color)">
                        <i class="fas fa-id-badge text-primary"></i> Dados do Profissional
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Categoria de Atuação</label>
                            <select id="prof-type" onchange="toggleProfType()" class="input-field w-full rounded-lg px-3 py-2 text-sm font-medium">
                                <option value="PEF">Profissional de Educação Física</option>
                                <option value="TE">Treinador Esportivo</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Nome do Profissional</label>
                            <input type="text" id="prof-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Seu nome completo">
                        </div>
                        <div class="grid grid-cols-2 gap-3" id="cref-container">
                            <div>
                                <label class="block text-sm font-medium mb-1">CREF</label>
                                <input type="text" id="prof-cref" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: 000000">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Estado</label>
                                <input type="text" id="prof-state" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Ex: SP, RJ">
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
                            <label class="block text-sm font-medium mb-1">Nome do Aluno</label>
                            <input type="text" id="stu-name" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Nome completo">
                        </div>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <label class="block text-sm font-medium mb-1">Idade</label>
                                <input type="number" id="stu-age" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Anos">
                            </div>
                            <div>
                                <label class="block text-sm font-medium mb-1">Peso (kg)</label>
                                <input type="number" id="stu-weight" oninput="calculateIMC()" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="kg">
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
                    <p class="text-[11px] opacity-70 mb-3 leading-tight">Gera recomendações automáticas na ficha. Baseado na sua categoria profissional.</p>
                    <div class="grid grid-cols-2 gap-2 text-xs" id="health-container">
                        <!-- Gerado via JS -->
                    </div>
                </div>

            </div>

            <!-- COLUNA DIREITA: Montagem dos Treinos -->
            <div class="xl:col-span-8 space-y-6">
                
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-opacity-10 p-4 rounded-xl card border-dashed border-2 gap-4">
                    <div>
                        <h2 class="text-xl font-bold flex items-center gap-2">
                            <i class="fas fa-clipboard-list text-primary"></i> Montagem da Ficha
                        </h2>
                        <div class="flex gap-2 mt-2">
                            <select id="stu-validity" class="input-field rounded px-2 py-1 text-xs">
                                <option>Validade: 15 dias</option>
                                <option selected>Validade: 30 dias</option>
                                <option>Validade: 60 dias</option>
                                <option>Validade: 90 dias</option>
                            </select>
                        </div>
                    </div>
                    <button onclick="addWorkout()" class="btn-primary px-4 py-2 rounded-lg text-sm font-medium flex items-center gap-2 w-full sm:w-auto justify-center">
                        <i class="fas fa-plus"></i> Adicionar Dia de Treino
                    </button>
                </div>

                <!-- Container de Treinos -->
                <div id="workouts-container" class="space-y-6">
                    <!-- Treinos JS -->
                </div>

                <!-- Recomendações Livres -->
                <div class="card rounded-xl p-5 shadow-sm">
                    <h2 class="text-md font-semibold mb-2"><i class="fas fa-pen text-primary"></i> Recomendações do Profissional (Editável)</h2>
                    <textarea id="stu-recs" rows="3" class="input-field w-full rounded-lg px-3 py-2 text-sm" placeholder="Escreva observações gerais sobre hidratação, descanso, cuidados e intensidade..."></textarea>
                </div>

            </div>
        </div>
    </div>

    <!-- MODAL DE EXERCÍCIOS -->
    <div id="exercise-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[60] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-5xl rounded-xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-search text-primary mr-2"></i> Selecionar Exercício</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="flex flex-col md:flex-row flex-1 overflow-hidden">
                <div class="w-full md:w-1/4 border-r overflow-y-auto p-2 space-y-1" style="border-color: var(--border-color)" id="modal-categories"></div>
                <div class="w-full md:w-3/4 overflow-y-auto p-3 bg-black bg-opacity-5" id="modal-exercises"></div>
            </div>
        </div>
    </div>

    <!-- MODAL DE HISTÓRICO (MEMÓRIA) -->
    <div id="history-modal" class="fixed inset-0 bg-black bg-opacity-70 backdrop-blur-sm hidden z-[70] flex items-center justify-center p-2 sm:p-4">
        <div class="card w-full max-w-3xl rounded-xl shadow-2xl flex flex-col max-h-[90vh]">
            <div class="p-4 border-b flex justify-between items-center" style="border-color: var(--border-color)">
                <h3 class="text-lg font-bold"><i class="fas fa-history text-primary mr-2"></i> Memória de Fichas Salvas</h3>
                <button onclick="closeHistory()" class="text-gray-400 hover:text-red-500 transition-colors text-2xl leading-none">&times;</button>
            </div>
            <div class="p-4 overflow-y-auto flex-1 space-y-3" id="history-list">
                <!-- Itens do histórico JS -->
            </div>
        </div>
    </div>

    <!-- ========================================== -->
    <!-- ÁREA DE IMPRESSÃO (Oculta na tela)         -->
    <!-- ========================================== -->
    <div id="print-area"></div>

    <!-- SCRIPTS -->
    <script>
        // --- DADOS DO SISTEMA E DIRETRIZES LEGAIS ---
        
        const healthDataPEF = {
            "🟢 Saudável": "Manter treinos regulares 3–5x por semana, combinando musculação e cardio. Foco em evolução e constância.",
            "⚪ Sedentário": "Iniciar com treinos leves 2–3x por semana. Priorizar adaptação, técnica e evitar excesso de carga.",
            "🟡 Sobrepeso": "Treinar 3–5x por semana com foco em gasto calórico. Combinar musculação e cardio moderado.",
            "🔴 Obesidade": "Iniciar com exercícios de baixo impacto. Evolução gradual, priorizando saúde e segurança.",
            "⚖️ Baixo peso": "Foco em musculação para ganho de massa. Treinos moderados com alimentação adequada.",
            "🍬 Diabetes": "Treinos regulares moderados. Monitorar glicemia e evitar treinos em jejum.",
            "❤️ Hipertensão": "Treinos moderados, evitar prender a respiração. Priorizar controle da intensidade.",
            "🔵 Hipotensão": "Evitar mudanças bruscas. Manter hidratação e intensidade leve a moderada.",
            "💔 Problemas cardíacos": "Treinos leves com liberação médica. Monitorar frequência cardíaca.",
            "🦴 Problemas articulares": "Evitar impacto. Priorizar exercícios controlados e máquinas.",
            "🫁 Problemas respiratórios": "Treinos leves a moderados com progressão gradual. Atenção à respiração.",
            "⚠️ Lesões": "Adaptar exercícios. Evitar dor e focar na recuperação.",
            "🤰 Gestante": "Treinos leves a moderados, sem impacto ou risco. Foco em mobilidade e bem-estar.",
            "🤱 Lactante": "Treinar normalmente com atenção à hidratação e energia.",
            "👴 Idoso": "Foco em força, equilíbrio e mobilidade. Intensidade moderada com segurança."
        };

        const healthDataTE = {
            "🟢 Saudável": "Manter treinos regulares de 3–5x por semana, combinando musculação e atividades cardiovasculares. A constância, descanso adequado e boa alimentação contribuem para melhores resultados.",
            "⚪ Sedentário": "O início deve ser gradual, com treinos leves e foco na adaptação do corpo. A prioridade é desenvolver constância, aprender a execução correta e evitar excesso de carga.",
            "🟡 Sobrepeso": "A prática regular de musculação associada ao cardio pode contribuir para melhora do condicionamento físico e composição corporal. A progressão deve respeitar a individualidade e o nível de adaptação.",
            "🔴 Obesidade": "É recomendado iniciar com exercícios de menor impacto e progressão gradual. A segurança, mobilidade e constância são prioridades durante o processo.",
            "⚖️ Baixo peso": "A musculação pode auxiliar no ganho de massa muscular quando associada a alimentação adequada e descanso. O treino deve priorizar evolução progressiva e recuperação muscular.",
            "🍬 Diabetes": "Pessoas com diabetes devem manter acompanhamento médico regular antes e durante a prática de exercícios. A musculação pode auxiliar na rotina de atividade física quando liberada por profissional de saúde. Em casos de tontura, mal-estar ou alteração de glicemia, o treino deve ser interrompido e o aluno deve procurar orientação médica.",
            "❤️ Hipertensão": "Pessoas com hipertensão devem manter acompanhamento médico regular e respeitar as orientações profissionais. Durante o treino, é importante evitar prender a respiração e controlar a intensidade dos exercícios.",
            "🔵 Hipotensão": "Pessoas com hipotensão devem evitar mudanças bruscas de posição durante o treino. Manter boa hidratação e respeitar a intensidade adequada ajuda a reduzir episódios de mal-estar.",
            "💔 Problemas cardíacos": "A prática de exercícios deve ocorrer apenas com liberação e acompanhamento médico. O treino deve respeitar limites individuais, com controle de intensidade e atenção aos sinais do corpo.",
            "🦴 Problemas articulares": "Exercícios com menor impacto e maior controle de movimento costumam ser mais indicados. O acompanhamento profissional e a adaptação dos exercícios ajudam na segurança durante o treino.",
            "🫁 Problemas respiratórios": "A progressão do treino deve ser gradual e respeitar a capacidade respiratória individual. Em caso de falta de ar excessiva ou desconforto, o exercício deve ser interrompido.",
            "⚠️ Lesões": "A adaptação dos exercícios deve respeitar a limitação existente e evitar dor durante a execução. Em situações de lesão, é importante seguir orientação profissional adequada antes da prática.",
            "🤰 Gestante": "A prática de exercícios deve ocorrer com liberação médica e acompanhamento adequado. O foco deve ser segurança, mobilidade e bem-estar, evitando impacto excessivo e situações de risco.",
            "🤱 Lactante": "A atividade física pode ser mantida normalmente, respeitando a recuperação individual e mantendo boa hidratação e alimentação adequada.",
            "👴 Idoso": "A musculação pode contribuir para força, equilíbrio, mobilidade e autonomia. O treino deve respeitar limitações individuais, com intensidade moderada e foco na segurança."
        };

        const objectiveData = {
            "Emagrecimento": "Foco em déficit calórico com treinos mistos de força (manter massa magra) e aeróbicos (maior gasto).",
            "Hipertrofia": "Prioridade na progressão de carga e volume adequado. Essencial superávit calórico e descanso.",
            "Definição": "Manutenção de massa muscular enquanto reduz o percentual de gordura. Atenção estrita à dieta.",
            "Condicionamento": "Treinos com menor tempo de intervalo, circuitos e alta integração cardiopulmonar.",
            "Resistência": "Séries mais longas, cadência controlada e aprimoramento da capacidade muscular.",
            "Força": "Cargas altas, baixas repetições e intervalos de descanso maiores.",
            "Reabilitação": "Treino focado em fortalecimento específico, mobilidade e controle motor. Respeitar limites de dor.",
            "Saúde geral": "Equilíbrio entre força, cardio e flexibilidade. O principal objetivo é a constância e bem-estar."
        };

        const dbCategories = {
            "🔥 PEITO": [
                "Supino Reto", "Supino Inclinado", "Supino Inclinado com Halteres", "Supino Fechado com Halteres", 
                "Cross Over", "Cross Over Alto", "Cross Over Baixo", "Crucifixo Reto", "Crucifixo Inclinado com Halteres", 
                "Crucifixo na Máquina", "Peck Fly", "Peck Fly Unilateral", "Pullover", 
                "Flexão de Braço", "Flexão com Pés Elevados", "Flexão Explosiva"
            ],
            "🦍 COSTAS": [
                "Puxada Alta", "Puxada de Frente Supinada", "Pulldown", "Remada Aberta", "Remada Baixa", 
                "Remada Curvada", "Remada Curvada Supinada", "Remada Cavalinho (T-Bar)", "Serrote", "Facepull (puxada de cima para baixo)"
            ],
            "🦵 PERNAS": [
                "Agachamento Livre", "Agachamento Taça", "Agachamento no Smith", "Agachamento com passada lateral", 
                "Squat", "Hack Machine", "Leg 45°", "Leg 90°", "Agachamento Sumô", "Agachamento Sissy (Livre)", 
                "Afundo", "Recuo", "Avanço", "Passada", "Búlgaro", "Step-up", "Levantamento Terra", 
                "Levantamento Terra Romeno", "Terra Sumô", "Stiff", "Bom Dia", "Mesa Flexora", "Cadeira Flexora", 
                "Elevação Pélvica no Banco", "Elevação Pélvica no Chão", "Elevação Pélvica Unilateral no Chão", 
                "Extensão de Quadril (Glúteo Máximo)", "Extensão Cruzada (Glúteo Médio)", "Coice", "Cachorrinho", 
                "Cadeira Extensora", "Adução", "Abdução", "Abdução Inclinada", "Flexão Nórdica", "Flexão Nórdica Invertida", 
                "Panturrilha Livre", "Panturrilha no Leg Press", "Panturrilha Banco", "Panturrilha Squat", "Panturrilha Unilateral"
            ],
            "💪 BRAÇOS": [
                "(Bíceps) Rosca Direta", "(Bíceps) Rosca Alternada", "(Bíceps) Rosca 21", "(Bíceps) Rosca Scott Barra W", 
                "(Bíceps) Rosca Scott Unilateral", "(Bíceps) Rosca Scott com Halteres", "(Bíceps) Rosca Martelo", 
                "(Bíceps) Rosca Cross", "(Bíceps) Rosca Inversa", "(Bíceps) Rosca 45°",
                "(Tríceps) Pulley Unilateral", "(Tríceps) Pulley Barra", "(Tríceps) Pulley Corda", "(Tríceps) Pulley Pegada Inversa", 
                "(Tríceps) Francês na Corda", "(Tríceps) Francês com Halter", "(Tríceps) Francês Unilateral", 
                "(Tríceps) Cruzado Polia Dupla", "(Tríceps) Coice Unilateral", "(Tríceps) Arremesso", "(Tríceps) Testa", "(Tríceps) Mergulho no Banco"
            ],
            "🪨 OMBROS": [
                "Elevação Frontal", "Elevação Frontal no Cross", "Elevação Lateral", "Elevação Lateral Unilateral Cross", 
                "Elevação Lateral Sentado", "Desenvolvimento com Halteres", "Desenvolvimento com Barra", "Arnold Press", 
                "Elevação Borboleta", "Crucifixo Inverso Sentado com Halteres", "Crucifixo Inverso na Polia", 
                "Crucifixo Inverso Unilateral na Polia", "Facepull (puxada reta)", "Remada Alta", "Encolhimento (Trapézio)"
            ],
            "🧠 ABDÔMEN": [
                "Infra com Elevação de Perna", "Abdominal Supra", "Abdominal Remador", "Abdominal Bicicleta", 
                "Abdominal Twister com Peso", "Prancha", "Prancha Lateral", "Trituração de Cabos em Pé", "Isometria na parede"
            ],
            "🫀 CARDIO": [
                "Bicicleta 10 Minutos", "Bicicleta 15 Minutos", "Bicicleta 20 Minutos",
                "Esteira 10 Minutos", "Esteira 15 Minutos", "Esteira 20 Minutos",
                "Pular Corda"
            ]
        };

        const dbTechniques = [
            "Nenhuma", "Drop set", "Bi-set", "Tri-set", "Série gigante", 
            "Rest-pause", "FST-7", "Pré-exaustão", "Pós-exaustão", 
            "Negativa", "Isometria", "Parciais", "Pirâmide"
        ];

        const daysOfWeek = ["SEGUNDA-FEIRA", "TERÇA-FEIRA", "QUARTA-FEIRA", "QUINTA-FEIRA", "SEXTA-FEIRA", "SÁBADO", "DOMINGO"];

        // --- ESTADO DA APLICAÇÃO ---
        let state = {
            currentId: null, // ID para controle do Histórico/Memória
            workouts: [],
            activeModalWorkoutId: null,
            activeCategory: "🔥 PEITO"
        };

        // --- INICIALIZAÇÃO ---
        function init() {
            toggleProfType(); // Inicia com as configs do perfil e gera as labels
            addWorkout(); // Cria automaticamente a SEGUNDA-FEIRA
            loadGlobalProfData(); // Restaura dados do prof caso já tenha usado o sistema
        }

        function loadGlobalProfData() {
            const type = localStorage.getItem('powfit_prof_type');
            const name = localStorage.getItem('powfit_prof_name');
            const cref = localStorage.getItem('powfit_prof_cref');
            const st = localStorage.getItem('powfit_prof_state');
            
            if(type) {
                document.getElementById('prof-type').value = type;
                toggleProfType();
            }
            if(name) document.getElementById('prof-name').value = name;
            if(cref) document.getElementById('prof-cref').value = cref;
            if(st) document.getElementById('prof-state').value = st;
        }

        // --- LÓGICA DE INTERFACE ---
        function toggleProfType() {
            const type = document.getElementById('prof-type').value;
            const crefContainer = document.getElementById('cref-container');
            
            if(type === 'TE') {
                crefContainer.classList.add('hidden');
                renderHealthOptions(healthDataTE);
            } else {
                crefContainer.classList.remove('hidden');
                renderHealthOptions(healthDataPEF);
            }
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
                let classif = "";
                if (imc < 18.5) classif = "Abaixo do peso";
                else if (imc < 24.9) classif = "Normal";
                else if (imc < 29.9) classif = "Sobrepeso";
                else classif = "Obesidade";
                
                display.innerHTML = `<span class="bg-primary bg-opacity-20 text-primary px-3 py-1 rounded-full text-xs font-bold border border-primary border-opacity-30">IMC: ${imc} (${classif})</span>`;
            } else {
                display.innerHTML = '';
            }
        }

        function generateId() { return Math.random().toString(36).substr(2, 9); }

        function renderHealthOptions(dataObj) {
            const container = document.getElementById('health-container');
            const selected = Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value);
            
            container.innerHTML = Object.keys(dataObj).map(opt => {
                const isChecked = selected.includes(opt) ? 'checked' : '';
                return `
                <label class="flex items-start space-x-2 cursor-pointer p-1.5 rounded hover:bg-black hover:bg-opacity-5 transition border border-transparent hover:border-gray-500 hover:border-opacity-20">
                    <input type="checkbox" value="${opt}" ${isChecked} class="health-cb mt-0.5 rounded border-gray-400 text-primary focus:ring-primary w-3.5 h-3.5">
                    <span class="font-medium">${opt}</span>
                </label>`
            }).join('');
        }

        // --- LÓGICA DE TREINOS ---
        function getNextDayName() {
            const count = state.workouts.length;
            if(count < 7) return daysOfWeek[count];
            return `NOVO TREINO ${count + 1}`;
        }

        function addWorkout() {
            state.workouts.push({
                id: generateId(),
                title: getNextDayName(),
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
            if(confirm("Excluir este dia de treino da ficha?")) {
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

        function renderWorkouts() {
            const container = document.getElementById('workouts-container');
            container.innerHTML = '';

            state.workouts.forEach(workout => {
                let exercisesHtml = '';
                if (workout.exercises.length === 0) {
                    exercisesHtml = `<div class="text-center p-4 text-xs opacity-50 italic">Nenhum exercício adicionado a este treino.</div>`;
                } else {
                    exercisesHtml = workout.exercises.map((ex, idx) => `
                        <div class="flex flex-col sm:flex-row gap-2 p-2 items-start sm:items-center border-b last:border-0 border-opacity-10 hover:bg-black hover:bg-opacity-5 transition" style="border-color: var(--border-color)">
                            <div class="flex gap-1 hidden sm:flex">
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'up')" class="text-[10px] p-1 rounded bg-black bg-opacity-5 hover:bg-opacity-20"><i class="fas fa-chevron-up"></i></button>
                                <button onclick="moveExercise('${workout.id}', ${idx}, 'down')" class="text-[10px] p-1 rounded bg-black bg-opacity-5 hover:bg-opacity-20"><i class="fas fa-chevron-down"></i></button>
                            </div>
                            <div class="flex-1 font-medium w-full sm:w-auto">
                                <div class="text-[9px] opacity-60 uppercase tracking-wider">${ex.category}</div>
                                <div class="text-xs font-bold text-primary">${ex.name}</div>
                            </div>
                            <div class="flex flex-wrap gap-1 w-full sm:w-auto mt-2 sm:mt-0">
                                <input type="text" value="${ex.sets}" onchange="updateExercise('${workout.id}', ${idx}, 'sets', this.value)" class="input-field w-12 rounded px-1 py-1 text-xs text-center" placeholder="Séries">
                                <span class="self-center opacity-50 text-xs font-bold">x</span>
                                <input type="text" value="${ex.reps}" onchange="updateExercise('${workout.id}', ${idx}, 'reps', this.value)" class="input-field w-16 rounded px-1 py-1 text-xs text-center" placeholder="Reps">
                                <select onchange="updateExercise('${workout.id}', ${idx}, 'technique', this.value)" class="input-field w-24 rounded px-1 py-1 text-[10px]">
                                    ${dbTechniques.map(t => `<option value="${t}" ${ex.technique === t ? 'selected' : ''}>${t}</option>`).join('')}
                                </select>
                                <input type="text" value="${ex.obs}" onchange="updateExercise('${workout.id}', ${idx}, 'obs', this.value)" class="input-field flex-1 sm:w-40 rounded px-2 py-1 text-xs" placeholder="Observações (ex: drop)">
                            </div>
                            <div class="flex items-center gap-1 w-full sm:w-auto justify-end mt-2 sm:mt-0">
                                <button onclick="removeExercise('${workout.id}', ${idx})" class="text-red-500 hover:text-red-700 p-1.5 rounded transition bg-red-500 bg-opacity-10 hover:bg-opacity-20 text-xs" title="Remover Exercício">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    `).join('');
                }

                const workoutCard = `
                    <div class="card rounded-xl overflow-hidden shadow-sm border-2">
                        <div class="p-3 border-b flex flex-col sm:flex-row justify-between items-start sm:items-center bg-black bg-opacity-5 gap-3" style="border-color: var(--border-color);">
                            <div class="flex w-full sm:w-1/2 items-center gap-2">
                                <i class="fas fa-calendar-day text-primary"></i>
                                <input type="text" value="${workout.title}" onchange="updateWorkoutTitle('${workout.id}', this.value)" class="input-field bg-transparent font-bold text-sm w-full px-2 py-1 rounded uppercase focus:bg-black focus:bg-opacity-10 transition">
                            </div>
                            <div class="flex gap-2 w-full sm:w-auto justify-end">
                                <button onclick="duplicateWorkout('${workout.id}')" class="text-xs p-1.5 rounded bg-black bg-opacity-10 hover:bg-opacity-20 transition flex items-center gap-1">
                                    <i class="fas fa-copy"></i> Duplicar Dia
                                </button>
                                <button onclick="removeWorkout('${workout.id}')" class="text-xs p-1.5 text-red-500 rounded bg-red-500 bg-opacity-10 hover:bg-red-500 hover:text-white transition">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        <div class="p-0">${exercisesHtml}</div>
                        <div class="p-2 border-t" style="border-color: var(--border-color)">
                            <button onclick="openModal('${workout.id}')" class="w-full text-primary py-2 rounded-lg text-xs font-bold hover:bg-primary hover:bg-opacity-10 transition border border-dashed border-primary border-opacity-30">
                                <i class="fas fa-plus"></i> ADICIONAR EXERCÍCIO NESTE TREINO
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
                <button onclick="setModalCategory('${cat}')" class="w-full text-left px-3 py-2.5 rounded-lg text-xs font-bold transition ${state.activeCategory === cat ? 'bg-primary text-white shadow' : 'hover:bg-black hover:bg-opacity-10'}">
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
                        return `<button onclick="addExerciseToWorkout('${ex}', ${isCardio})" class="card p-3 rounded-lg text-left text-xs font-semibold border hover:border-primary transition flex justify-between items-center group shadow-sm">
                            <span>${ex}</span>
                            <i class="fas fa-plus opacity-0 group-hover:opacity-100 transition text-primary"></i>
                        </button>`
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
                container.style.opacity = '0.4';
                setTimeout(() => container.style.opacity = '1', 100);
            }
        }


        // --- SISTEMA DE MEMÓRIA (LOCAL STORAGE) ---
        function getSavedData() {
            return {
                id: state.currentId || generateId(),
                date: new Date().toLocaleDateString('pt-BR') + ' ' + new Date().toLocaleTimeString('pt-BR', {hour: '2-digit', minute:'2-digit'}),
                studentName: document.getElementById('stu-name').value || 'Sem Nome',
                profType: document.getElementById('prof-type').value,
                profName: document.getElementById('prof-name').value,
                profCref: document.getElementById('prof-cref').value,
                profState: document.getElementById('prof-state').value,
                stuAge: document.getElementById('stu-age').value,
                stuWeight: document.getElementById('stu-weight').value,
                stuHeight: document.getElementById('stu-height').value,
                stuGender: document.getElementById('stu-gender').value,
                stuLevel: document.getElementById('stu-level').value,
                stuObjective: document.getElementById('stu-objective').value,
                stuFreq: document.getElementById('stu-freq').value,
                stuValidity: document.getElementById('stu-validity').value,
                stuRecs: document.getElementById('stu-recs').value,
                health: Array.from(document.querySelectorAll('.health-cb:checked')).map(cb => cb.value),
                workouts: state.workouts
            };
        }

        function saveToHistory() {
            const currentData = getSavedData();
            state.currentId = currentData.id; 
            
            // Salva dados do prof globalmente
            localStorage.setItem('powfit_prof_type', currentData.profType);
            localStorage.setItem('powfit_prof_name', currentData.profName);
            localStorage.setItem('powfit_prof_cref', currentData.profCref);
            localStorage.setItem('powfit_prof_state', currentData.profState);

            let history = JSON.parse(localStorage.getItem('powfit_history') || '[]');
            
            const existingIdx = history.findIndex(h => h.id === currentData.id);
            if(existingIdx >= 0) {
                history[existingIdx] = currentData;
            } else {
                history.push(currentData);
            }
            
            localStorage.setItem('powfit_history', JSON.stringify(history));
        }

        function openHistory() {
            document.getElementById('history-modal').classList.remove('hidden');
            const history = JSON.parse(localStorage.getItem('powfit_history') || '[]');
            const list = document.getElementById('history-list');
            
            if(history.length === 0) {
                list.innerHTML = `<div class="text-center opacity-50 py-10 font-medium">Nenhuma ficha salva na memória ainda.</div>`;
                return;
            }

            // Exibir as mais recentes primeiro
            history.reverse();
            
            list.innerHTML = history.map(h => `
                <div class="card p-4 rounded-xl flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3 border border-gray-500 border-opacity-20 shadow-sm">
                    <div>
                        <div class="font-bold text-lg text-primary">${h.studentName}</div>
                        <div class="text-xs opacity-80 font-medium">Salvo em: ${h.date} | ${h.stuObjective}</div>
                    </div>
                    <div class="flex gap-2 w-full sm:w-auto">
                        <button onclick="loadHistoryItem('${h.id}')" class="flex-1 sm:flex-none btn-primary px-4 py-2 rounded-lg text-xs font-bold shadow"><i class="fas fa-folder-open"></i> CARREGAR</button>
                        <button onclick="deleteHistoryItem('${h.id}')" class="flex-none bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg text-xs shadow transition"><i class="fas fa-trash"></i></button>
                    </div>
                </div>
            `).join('');
        }

        function closeHistory() {
            document.getElementById('history-modal').classList.add('hidden');
        }

        function loadHistoryItem(id) {
            const history = JSON.parse(localStorage.getItem('powfit_history') || '[]');
            const item = history.find(h => h.id === id);
            if(!item) return;

            state.currentId = item.id;
            
            document.getElementById('prof-type').value = item.profType || 'PEF';
            toggleProfType(); // Regenera checkboxes 
            
            document.getElementById('prof-name').value = item.profName || '';
            document.getElementById('prof-cref').value = item.profCref || '';
            document.getElementById('prof-state').value = item.profState || '';
            
            document.getElementById('stu-name').value = item.studentName || '';
            document.getElementById('stu-age').value = item.stuAge || '';
            document.getElementById('stu-weight').value = item.stuWeight || '';
            document.getElementById('stu-height').value = item.stuHeight || '';
            document.getElementById('stu-gender').value = item.stuGender || 'Masculino';
            document.getElementById('stu-level').value = item.stuLevel || 'Iniciante';
            document.getElementById('stu-objective').value = item.stuObjective || 'Emagrecimento';
            document.getElementById('stu-freq').value = item.stuFreq || '3 dias';
            document.getElementById('stu-validity').value = item.stuValidity || 'Validade: 30 dias';
            document.getElementById('stu-recs').value = item.stuRecs || '';
            
            changeTheme();
            calculateIMC();

            document.querySelectorAll('.health-cb').forEach(cb => {
                cb.checked = (item.health || []).includes(cb.value);
            });

            state.workouts = item.workouts || [];
            renderWorkouts();
            closeHistory();
        }

        function deleteHistoryItem(id) {
            if(confirm("Deseja excluir esta ficha da memória permanentemente?")) {
                let history = JSON.parse(localStorage.getItem('powfit_history') || '[]');
                history = history.filter(h => h.id !== id);
                localStorage.setItem('powfit_history', JSON.stringify(history));
                openHistory(); // Atualiza a lista na tela
                
                if(state.currentId === id) state.currentId = null;
            }
        }


        // --- LÓGICA DE IMPRESSÃO (ESTILO PLANILHA PROFISSIONAL A4) ---
        function generatePrint() {
            saveToHistory(); // Salva na memória antes de imprimir!

            const d = getSavedData();
            let imcStr = "-";
            const w = parseFloat(d.stuWeight);
            const h = parseFloat(d.stuHeight);
            if(w>0 && h>0) imcStr = (w / (h * h)).toFixed(1);

            const objRecommendation = objectiveData[d.stuObjective] || "";
            const isPEF = d.profType === 'PEF';
            const healthSourceDict = isPEF ? healthDataPEF : healthDataTE;
            
            const printArea = document.getElementById('print-area');
            
            // Tratamento Textos de Cabeçalho / Rodapé
            const profLabel = isPEF ? 'Profissional de Educação Física' : 'Treinador Esportivo';
            const crefText = isPEF ? `CREF: ${d.profCref || '________'} - Estado: ${d.profState || '___'}` : '';
            
            let legalTextPEF = `<strong>⚠️ OBSERVAÇÃO LEGAL – PROFISSIONAL DE EDUCAÇÃO FÍSICA</strong><br>Conforme a Lei nº 9.696/1998, Art. 1º, o exercício das atividades de Educação Física e a designação de Profissional de Educação Física são prerrogativas dos profissionais regularmente registrados no CREF. O Art. 3º da mesma lei estabelece que compete ao profissional de Educação Física coordenar, planejar, programar, supervisionar, organizar, avaliar e executar treinamentos especializados nas áreas de atividades físicas e do desporto.`;
            
            let legalTextTE = `<strong>⚠️ OBSERVAÇÃO LEGAL – TREINADOR ESPORTIVO</strong><br>Conforme a Lei nº 14.597/2023 (Lei Geral do Esporte), Art. 75, a profissão de treinador esportivo é reconhecida e regulada no Brasil, com atuação de caráter técnico e esportivo voltada à preparação, supervisão, orientação e acompanhamento de treinos físicos e esportivos. A atuação possui finalidade orientativa e não substitui avaliação médica, diagnóstico clínico, prescrição medicamentosa ou acompanhamento de profissionais da saúde. Em casos de doenças, lesões, gestação, limitações físicas, uso de medicações ou qualquer condição específica de saúde, recomenda-se avaliação prévia por profissional habilitado antes do início ou continuidade da prática de exercícios. O treinamento proposto respeita os limites individuais, priorizando segurança, execução correta e evolução progressiva, dentro da atuação técnica e esportiva permitida por lei.`;

            let html = `
                <div class="print-header">
                    <h1 class="print-title">Planilha de Treinamento - ${d.stuObjective}</h1>
                    <div class="prof-info">
                        Prescrição feita por: ${d.profName || '________________________'} (${profLabel})<br>
                        ${crefText}
                    </div>
                </div>

                <div class="print-grid">
                    <div><strong>Aluno:</strong> ${d.studentName}</div>
                    <div><strong>Idade:</strong> ${d.stuAge || '-'} anos</div>
                    <div><strong>Gênero:</strong> ${d.stuGender}</div>
                    
                    <div><strong>Peso:</strong> ${d.stuWeight || '-'} kg</div>
                    <div><strong>Altura:</strong> ${d.stuHeight || '-'} m</div>
                    <div><strong>IMC:</strong> ${imcStr}</div>
                    
                    <div><strong>Nível:</strong> ${d.stuLevel}</div>
                    <div><strong>Frequência:</strong> ${d.stuFreq}</div>
                    <div><strong>Validade:</strong> ${d.stuValidity.replace('Validade: ', '')}</div>
                </div>
            `;

            // Diretrizes
            if (objRecommendation || d.health.length > 0) {
                html += `<div class="print-guidelines">
                            <h4>Diretrizes de Saúde e Perfil Automáticas</h4>
                            <ul>`;
                if(objRecommendation) html += `<li><strong>Objetivo (${d.stuObjective}):</strong> ${objRecommendation}</li>`;
                
                d.health.forEach(healthKey => {
                    if(healthSourceDict[healthKey]) {
                        const cleanKey = healthKey.replace(/[\u{1F300}-\u{1F9FF}\u{2600}-\u{26FF}\u{2700}-\u{27BF}\u{FE0F}]/gu, '').trim();
                        html += `<li><strong>Saúde (${cleanKey}):</strong> ${healthSourceDict[healthKey]}</li>`;
                    }
                });
                html += `</ul></div>`;
            }

            // Planilhas
            d.workouts.forEach(w => {
                html += `
                    <div class="print-workout">
                        <h3>Treino: ${w.title}</h3>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 35%">Exercício</th>
                                    <th style="width: 8%; text-align:center;">Séries</th>
                                    <th style="width: 12%; text-align:center;">Reps</th>
                                    <th style="width: 15%">Técnica</th>
                                    <th style="width: 30%">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                `;
                
                if (w.exercises.length === 0) {
                    html += `<tr><td colspan="5" style="text-align:center; font-style:italic; padding:10px;">Sem exercícios vinculados.</td></tr>`;
                } else {
                    w.exercises.forEach(ex => {
                        const techStr = ex.technique !== 'Nenhuma' ? ex.technique : '-';
                        html += `
                            <tr>
                                <td><strong>${ex.name}</strong></td>
                                <td style="text-align:center;">${ex.sets}</td>
                                <td style="text-align:center;">${ex.reps}</td>
                                <td>${techStr}</td>
                                <td>${ex.obs || '-'}</td>
                            </tr>
                        `;
                    });
                }
                html += `</tbody></table></div>`;
            });

            // Recomendações Profissionais Livres
            if(d.stuRecs.trim()) {
                html += `
                    <div class="print-recs">
                        <h4>Recomendações e Observações Extras:</h4>
                        <p style="white-space: pre-wrap; margin:0; font-size: 10px;">${d.stuRecs}</p>
                    </div>
                `;
            }

            // Rodapé Legal
            html += `<div class="legal-footer">
                        ${isPEF ? legalTextPEF : legalTextTE}
                     </div>`;

            printArea.innerHTML = html;
            
            // Dispara
            setTimeout(() => { window.print(); }, 400);
        }

        // Inicia o app
        init();

    </script>
</body>
</html>
