<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Sistema de Prescrição Elite</title>
    
    <!-- Bibliotecas e Fontes -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #fbbf24;
            --bg-body: #0a0a0a;
            --bg-card: #141414;
            --text-main: #ffffff;
            --text-muted: #a1a1aa;
            --border: #27272a;
            --ai-glow: #8b5cf6;
            --transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        [data-theme="feminino"] {
            --primary: #f472b6;
            --bg-body: #fff1f2;
            --bg-card: #ffffff;
            --text-main: #18181b;
            --text-muted: #71717a;
            --border: #fbcfe8;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: var(--transition);
            padding: 15px;
            line-height: 1.5;
        }

        .container { max-width: 900px; margin: 0 auto; }

        header { text-align: center; padding: 40px 0; }
        header h1 { 
            font-family: 'Playfair Display', serif; 
            font-size: 2.8rem; 
            color: var(--primary); 
            font-weight: 900; 
            font-style: italic;
        }
        header p { letter-spacing: 4px; font-size: 0.65rem; color: var(--text-muted); text-transform: uppercase; margin-top: 5px; }

        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.4);
            margin-bottom: 25px;
        }

        .section-title {
            font-size: 0.8rem;
            font-weight: 900;
            text-transform: uppercase;
            color: var(--primary);
            margin-bottom: 20px;
            border-bottom: 1px solid var(--border);
            padding-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
        @media (min-width: 650px) { .grid { grid-template-columns: repeat(3, 1fr); } }

        .form-group { margin-bottom: 15px; }
        label { display: block; font-size: 0.65rem; font-weight: 700; text-transform: uppercase; margin-bottom: 6px; color: var(--text-muted); }

        input, select {
            width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid var(--border);
            border-radius: 12px; color: var(--text-main); font-size: 0.95rem; outline: none; transition: 0.3s;
        }

        [data-theme="feminino"] input, [data-theme="feminino"] select { background: #fff; color: #18181b; }

        /* Módulo Personal - Seleção Manual */
        .manual-selector {
            display: none;
            margin-top: 15px;
            border: 1px solid var(--border);
            border-radius: 15px;
            padding: 15px;
            background: rgba(0,0,0,0.1);
        }

        .exercise-category { margin-bottom: 30px; }
        .category-name {
            font-size: 0.6rem; background: var(--primary); color: #000; padding: 4px 10px;
            border-radius: 4px; font-weight: 900; margin-bottom: 15px; display: inline-block;
        }

        .exercise-item-box {
            background: rgba(255,255,255,0.03);
            padding: 12px;
            border-radius: 10px;
            margin-bottom: 10px;
            border: 1px solid var(--border);
        }

        .ex-label { font-size: 0.85rem; font-weight: 700; margin-bottom: 8px; display: block; }
        .day-chips { display: flex; gap: 6px; overflow-x: auto; padding-bottom: 5px; }
        
        .chip {
            flex: 0 0 auto;
            display: flex;
            align-items: center;
            gap: 5px;
            background: var(--border);
            padding: 5px 10px;
            border-radius: 8px;
            font-size: 0.65rem;
            cursor: pointer;
            transition: 0.2s;
        }
        .chip input { width: auto; height: auto; }
        .chip.active { background: var(--primary); color: #000; }

        .btn {
            width: 100%; padding: 18px; background: var(--primary); color: #000; border: none; border-radius: 14px;
            font-weight: 900; text-transform: uppercase; cursor: pointer; transition: 0.3s; font-size: 0.95rem;
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { filter: brightness(1.1); transform: translateY(-2px); }

        .btn-ai {
            background: linear-gradient(135deg, #6366f1 0%, #a855f7 100%);
            color: white !important;
            margin-top: 15px;
        }

        /* Ficha Profissional Preview */
        #preview-section { display: none; margin-top: 30px; }
        #sheet-view {
            background: #fff; color: #000; padding: 15mm; width: 100%; max-width: 210mm;
            margin: 0 auto; box-shadow: 0 0 30px rgba(0,0,0,0.2);
            position: relative;
        }

        .sheet-header { border-bottom: 4px solid #000; padding-bottom: 15px; margin-bottom: 25px; display: flex; justify-content: space-between; align-items: flex-end; }
        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.2rem; font-weight: 900; font-style: italic; color: #000; margin: 0; }
        
        .info-strip { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 25px; background: #f1f5f9; padding: 15px; border-radius: 8px; }
        .info-item b { display: block; font-size: 0.55rem; text-transform: uppercase; color: #64748b; }
        .info-item span { font-weight: 800; color: #0f172a; font-size: 0.8rem; }

        .workout-day-block { margin-bottom: 30px; page-break-inside: avoid; }
        .day-banner { background: #000; color: #fff; padding: 8px 15px; font-weight: 900; margin-bottom: 10px; font-size: 0.9rem; display: flex; justify-content: space-between; }
        
        table { width: 100%; border-collapse: collapse; margin-bottom: 15px; }
        th { background: #e2e8f0; text-align: left; padding: 10px; border: 1px solid #cbd5e1; font-size: 0.65rem; text-transform: uppercase; }
        td { padding: 10px; border: 1px solid #cbd5e1; font-size: 0.8rem; color: #334155; }

        .ai-recommendations {
            margin-top: 25px; padding: 20px; background: #fdf4ff; border: 1px solid #f0abfc;
            border-radius: 12px; font-size: 0.75rem; color: #701a75; display: none; line-height: 1.6;
        }

        .footer-presc { margin-top: 50px; text-align: right; border-top: 2px solid #000; padding-top: 20px; color: #000; }
        .footer-presc b { font-size: 1.3rem; display: block; font-weight: 900; font-style: italic; }

        /* Loader */
        #loader {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 10000; flex-direction: column; justify-content: center; align-items: center;
        }
        .spin { width: 50px; height: 50px; border: 5px solid #1a1a1a; border-top: 5px solid var(--primary); border-radius: 50%; animation: rotate 1s infinite linear; }
        @keyframes rotate { to { transform: rotate(360deg); } }

        /* Impressão */
        @media print {
            .no-print { display: none !important; }
            body { background: white !important; padding: 0; margin: 0; }
            #preview-section { display: block !important; margin: 0 !important; }
            #sheet-view { box-shadow: none !important; width: 100% !important; max-width: none !important; padding: 10mm !important; }
            @page { size: A4; margin: 0; }
        }
    </style>
</head>
<body>

    <div id="loader">
        <div class="spin"></div>
        <p id="loader-msg" style="margin-top: 20px; font-weight: 900; font-size: 0.7rem; color: #fff; letter-spacing: 2px;">PROCESSANDO DADOS...</p>
    </div>

    <div class="container no-print">
        <header>
            <h1>PowFit Pro</h1>
            <p>SISTEMA PROFISSIONAL DE PRESCRIÇÃO</p>
        </header>

        <section class="card">
            <div class="section-title">
                <span>Perfil do Atleta</span>
                <div style="font-size: 0.65rem;">
                    Modo: 
                    <select id="creationMode" style="width: auto; padding: 2px 5px;" onchange="toggleManualPanel()">
                        <option value="auto">Automático</option>
                        <option value="manual">Personal (Manual)</option>
                    </select>
                </div>
            </div>

            <form id="workoutForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome do Aluno</label>
                        <input type="text" id="name" placeholder="Nome Completo" required>
                    </div>
                    <div class="form-group">
                        <label>Gênero</label>
                        <select id="gender" onchange="document.body.setAttribute('data-theme', this.value)">
                            <option value="masculino">Masculino</option>
                            <option value="feminino">Feminino</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Idade / Peso</label>
                        <div style="display: flex; gap: 5px;">
                            <input type="number" id="age" placeholder="Anos" required>
                            <input type="number" id="weight" placeholder="Kg" required>
                        </div>
                    </div>
                </div>

                <div class="grid">
                    <div class="form-group">
                        <label>Objetivo</label>
                        <select id="goal">
                            <option value="Hipertrofia">Hipertrofia</option>
                            <option value="Emagrecimento">Emagrecimento</option>
                            <option value="Definição">Definição Muscular</option>
                            <option value="Ganho de Força">Ganho de Força</option>
                            <option value="Resistência">Resistência Muscular</option>
                            <option value="Reabilitação">Reabilitação</option>
                            <option value="Saúde Geral">Saúde Geral</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Estado de Saúde Atual</label>
                        <select id="health">
                            <option value="Saudável">Saudável</option>
                            <option value="Sedentário">Sedentário</option>
                            <option value="Hipertensão">Hipertensão / Cardíaco</option>
                            <option value="Diabetes">Diabetes / Pré-diabetes</option>
                            <option value="Obesidade">Sobrepeso / Obesidade</option>
                            <option value="Joelho">Lesão no Joelho</option>
                            <option value="Lombar">Lesão Lombar / Hérnia</option>
                            <option value="Idoso">Idoso (Mobilidade/Equilíbrio)</option>
                            <option value="Gestante">Gestante / Lactante</option>
                            <option value="Epiléptico">Epiléptico(a)</option>
                            <option value="Asma">Asma / Respiratório</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência Semanal</label>
                        <select id="freq" onchange="refreshManualList()">
                            <option value="3">3 Dias (A-B-C)</option>
                            <option value="6">6 Dias (Semanal)</option>
                        </select>
                    </div>
                </div>

                <!-- Painel de Seleção Manual do Personal -->
                <div id="manualPanel" class="manual-selector">
                    <div class="section-title"><span>Seleção Manual do Personal</span></div>
                    <div id="exerciseList"></div>
                </div>

                <button type="submit" class="btn" style="margin-top: 20px;">Gerar Prescrição Profissional</button>
            </form>
        </section>
    </div>

    <!-- Preview e Exportação -->
    <section id="preview-section">
        <div class="no-print" style="padding: 15px; text-align: center; display: flex; flex-direction: column; gap: 15px; align-items: center;">
            <div style="display: flex; gap: 10px;">
                <button class="btn" style="max-width: 250px;" onclick="window.print()">🖨️ IMPRIMIR / PDF</button>
                <button class="btn btn-ai" style="max-width: 250px;" onclick="getAIConsultancy()">✨ RECOMENDAÇÕES IA</button>
            </div>
            <p style="font-size: 0.6rem; color: #888;">A IA analisa biomecanicamente sua condição de saúde e prescreve dicas extras.</p>
        </div>
        
        <div id="sheet-view">
            <!-- Gerado via JavaScript -->
        </div>
    </section>

    <script>
        const apiKey = ""; // Ambiente configurado
        const EXERCISES_DB = {
            "PEITO": ["Supino Reto", "Supino inclinado", "Supino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly (Crucifixo Máquina)", "Pack Fly unilateral", "Poollover"],
            "COSTAS": ["Puxada Alta", "Remada aberta", "Remada Baixa", "Remada Curvada", "Puxada Unilateral", "Serrote", "Pooldowm", "Facepool"],
            "PERNAS": ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô"],
            "GLÚTEO": ["Elevação de Perna lateral", "Elevação De quadril", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro", "Panturrilha Banco", "Panturrilha squat"],
            "BRAÇOS": ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo", "Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            "OMBROS": ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação Borboleta"],
            "ABDOMEN": ["Abdôminal Infra", "Abdôminal remador", "Abdôminal Prancha 5× de 30seg"],
            "CÁRDIO": ["Bicicleta 10min", "Bicicleta 15min", "Bicicleta 20min", "Esteira 10min", "Esteira 15min", "Esteira 20min", "Pula Corda 5× de 30seg"]
        };

        function toggleManualPanel() {
            const mode = document.getElementById('creationMode').value;
            document.getElementById('manualPanel').style.display = (mode === 'manual') ? 'block' : 'none';
            if(mode === 'manual') refreshManualList();
        }

        function refreshManualList() {
            const container = document.getElementById('exerciseList');
            const freq = parseInt(document.getElementById('freq').value);
            container.innerHTML = "";

            for (const cat in EXERCISES_DB) {
                const catDiv = document.createElement('div');
                catDiv.className = 'exercise-category';
                catDiv.innerHTML = `<div class="category-name">${cat}</div>`;
                
                EXERCISES_DB[cat].forEach(ex => {
                    const box = document.createElement('div');
                    box.className = 'exercise-item-box';
                    
                    let chips = "";
                    for(let d=1; d <= freq; d++) {
                        chips += `
                            <label class="chip" id="chip-${ex}-${d}">
                                <input type="checkbox" name="manual-ex" value="${ex}" data-day="${d}" onchange="this.parentElement.classList.toggle('active', this.checked)">
                                D${d}
                            </label>
                        `;
                    }

                    box.innerHTML = `<span class="ex-label">${ex}</span><div class="day-chips">${chips}</div>`;
                    catDiv.appendChild(box);
                });
                container.appendChild(catDiv);
            }
        }

        document.getElementById('workoutForm').addEventListener('submit', function(e) {
            e.preventDefault();
            document.getElementById('loader').style.display = 'flex';
            
            setTimeout(() => {
                renderWorkoutSheet();
                document.getElementById('loader').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1200);
        });

        function renderWorkoutSheet() {
            const sheet = document.getElementById('sheet-view');
            const data = {
                name: document.getElementById('name').value,
                gender: document.getElementById('gender').value,
                age: document.getElementById('age').value,
                weight: document.getElementById('weight').value,
                goal: document.getElementById('goal').value,
                health: document.getElementById('health').value,
                freq: parseInt(document.getElementById('freq').value),
                mode: document.getElementById('creationMode').value
            };

            let workoutHTML = "";
            const isFem = data.gender === 'feminino';

            if(data.mode === 'manual') {
                const checks = Array.from(document.querySelectorAll('input[name="manual-ex"]:checked'));
                if(checks.length === 0) { alert("Por favor, selecione os exercícios no módulo personal!"); document.getElementById('loader').style.display = 'none'; return; }

                for(let d=1; d <= data.freq; d++) {
                    const dailyEx = checks.filter(c => parseInt(c.dataset.day) === d);
                    if(dailyEx.length > 0) {
                        workoutHTML += `
                            <div class="workout-day-block">
                                <div class="day-banner">DIA ${d} - PRESCRIÇÃO PERSONALIZADA</div>
                                <table>
                                    <thead><tr><th>Exercício</th><th style="width:60px">Séries</th><th style="width:70px">Reps</th><th>Observações</th></tr></thead>
                                    <tbody>
                                        ${dailyEx.map(c => `<tr><td class="ex-name-ref">${c.value}</td><td>4</td><td>12-15</td><td>Prescrito manualmente.</td></tr>`).join('')}
                                    </tbody>
                                </table>
                            </div>
                        `;
                    }
                }
            } else {
                // Modo Automático Inteligente
                for(let i=0; i < data.freq; i++) {
                    const dia = i + 1;
                    const letra = ["A", "B", "C"][i % 3];
                    workoutHTML += `
                        <div class="workout-day-block">
                            <div class="day-banner">
                                <span>DIA ${dia} - TREINO ${letra}</span>
                                <span style="font-size:0.55rem; opacity:0.7;">FOCO: ${getAutoFoco(letra, isFem, data.health)}</span>
                            </div>
                            <table>
                                <thead><tr><th>Exercício</th><th style="width:60px">Séries</th><th style="width:70px">Reps</th><th>Observações</th></tr></thead>
                                <tbody>
                                    ${getAutoRows(letra, data)}
                                </tbody>
                            </table>
                        </div>
                    `;
                }
            }

            sheet.innerHTML = `
                <div class="sheet-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="font-size:0.5rem; color:#444; font-weight:800; margin-top:5px;">SISTEMA DE ALTA PERFORMANCE</p>
                    </div>
                    <div style="text-align:right; font-size:0.6rem; color:#444;">DATA: ${new Date().toLocaleDateString('pt-br')}</div>
                </div>

                <div class="info-strip">
                    <div class="info-item"><b>Aluno</b><span>${data.name}</span></div>
                    <div class="info-item"><b>Objetivo</b><span>${data.goal}</span></div>
                    <div class="info-item"><b>Saúde</b><span>${data.health.toUpperCase()}</span></div>
                    <div class="info-item"><b>Gênero</b><span>${data.gender.toUpperCase()}</span></div>
                    <div class="info-item"><b>Idade</b><span>${data.age} anos</span></div>
                    <div class="info-item"><b>Peso</b><span>${data.weight} kg</span></div>
                </div>

                <div style="background: #fdf2f2; border: 1px solid #fecaca; padding: 10px; border-radius: 6px; margin-bottom: 20px; font-size: 0.7rem; color: #991b1b;">
                    <b>⚠️ RECOMENDAÇÃO DE SEGURANÇA:</b> ${getHealthNote(data.health)}
                </div>

                ${workoutHTML}

                <div id="ai-recom-box" class="ai-recommendations"></div>

                <div class="footer-presc">
                    <p style="font-size: 0.5rem; color: #777; margin-bottom: 5px;">Prescrição biomecanicamente validada por</p>
                    <b>LUCAS ANDRÉ</b>
                    <p style="font-size: 0.7rem; font-weight: 700;">Personal Trainer - CREF: 008094 - G/RN</p>
                </div>
            `;
        }

        function getAutoFoco(letra, isFem, health) {
            if (["Idoso", "Gestante", "Hipertensão", "Epiléptico"].includes(health)) return "Saúde e Controle";
            if (isFem) {
                if (letra === 'A') return "Quadríceps e Glúteo 🍑";
                if (letra === 'B') return "Superiores e Core";
                return "Posterior e Glúteo Máximo 🍑";
            }
            if (letra === 'A') return "Peito, Ombros e Tríceps";
            if (letra === 'B') return "Costas e Bíceps";
            return "Membros Inferiores Completos";
        }

        function getHealthNote(health) {
            const notes = {
                "Hipertensão": "Evite manobra de Valsalva (trancar a respiração). Foco em treinos rítmicos e contínuos.",
                "Gestante": "Sem exercícios em decúbito ventral (barriga para baixo) após 1º trimestre. Carga leve a moderada.",
                "Joelho": "Evite exercícios de alto impacto. Foco em fortalecimento isométrico e cadeia aberta.",
                "Lombar": "Evite sobrecarga axial e flexões excessivas de tronco. Priorize estabilização de core.",
                "Epiléptico": "Treine sempre acompanhado(a). Evite exercícios em alturas ou com cargas que não possa soltar.",
                "Asma": "Mantenha medicação por perto. Controle a intensidade conforme o fluxo respiratório.",
                "Obesidade": "Foco em exercícios de baixo impacto articular e controle da frequência cardíaca."
            };
            return notes[health] || "Consulte sempre um profissional de saúde antes de iniciar atividades intensas.";
        }

        function getAutoRows(letra, data) {
            let pool = [];
            const isFem = data.gender === 'feminino';
            const health = data.health;

            if (isFem) {
                if (letra === 'A') pool = [...EXERCISES_DB.PERNAS.slice(0, 5), "Elevação pelvica", "Afundo"];
                else if (letra === 'B') pool = [...EXERCISES_DB.PEITO.slice(0,2), ...EXERCISES_DB.COSTAS.slice(0,2), ...EXERCISES_DB.ABDOMEN];
                else pool = [...EXERCISES_DB.GLÚTEO, "Stiff", "Mesa Flexora", "Sumô"];
            } else {
                if (letra === 'A') pool = [...EXERCISES_DB.PEITO.slice(0,4), ...EXERCISES_DB.OMBROS.slice(0,3), "Triceps puley"];
                else if (letra === 'B') pool = [...EXERCISES_DB.COSTAS.slice(0,5), ...EXERCISES_DB.BRAÇOS.slice(0,3)];
                else pool = [...EXERCISES_DB.PERNAS, "Panturrilha Banco"];
            }

            // Filtros de Segurança Biomecânica
            if (health === "Joelho") pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro", "Avanço", "Pula Corda"].includes(e));
            if (health === "Lombar") pool = pool.filter(e => !["Levantamento terra", "Stiff", "Agachamento com Barra", "Remada Curvada"].includes(e));
            if (health === "Hipertensão" || health === "Cardíaco") pool = pool.filter(e => !["Burpee", "Pula Corda"].includes(e));

            const sel = pool.sort(() => 0.5 - Math.random()).slice(0, 6);
            
            let s = "4", r = "12-15", o = "Intervalo 60s";
            if (data.goal === "Hipertrofia") { s = "4"; r = "8-12"; o = "Foco em falha técnica."; }
            if (data.goal === "Ganho de Força") { s = "5"; r = "4-6"; o = "Carga alta. Descanso 2-3min."; }
            if (["Idoso", "Gestante", "Hipertensão"].includes(health)) { s = "3"; r = "15-20"; o = "Intensidade controlada."; }

            return sel.map(ex => `<tr><td class="ex-name-ref">${ex}</td><td>${s}</td><td>${r}</td><td>${o}</td></tr>`).join('');
        }

        // Integração Gemini API para Recomendações
        async function getAIConsultancy() {
            const loader = document.getElementById('loader');
            const box = document.getElementById('ai-recom-box');
            loader.style.display = 'flex';
            document.getElementById('loader-msg').innerText = "IA ANALISANDO BIOMECÂNICA...";

            const aluno = {
                nome: document.getElementById('name').value,
                idade: document.getElementById('age').value,
                saude: document.getElementById('health').value,
                objetivo: document.getElementById('goal').value,
                exercicios: Array.from(document.querySelectorAll('.ex-name-ref')).map(el => el.innerText).join(', ')
            };

            const systemPrompt = "Você é um mestre em fisiologia e biomecânica. Sua missão é analisar o perfil do aluno e o treino gerado, fornecendo 3 dicas curtas e científicas de segurança e performance. Use tom profissional e encorajador.";
            const userPrompt = `Aluno: ${aluno.nome}, ${aluno.idade} anos. Saúde: ${aluno.saude}. Objetivo: ${aluno.objetivo}. Exercícios no treino: ${aluno.exercicios}. Forneça uma breve análise de segurança biomecânica em Português.`;

            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: userPrompt }] }],
                        systemInstruction: { parts: [{ text: systemPrompt }] }
                    })
                });

                const json = await response.json();
                const text = json.candidates[0].content.parts[0].text;
                box.innerHTML = `<strong>✨ CONSULTORIA TÉCNICA (IA):</strong><br><br>${text.replace(/\n/g, '<br>')}`;
                box.style.display = 'block';
                window.scrollTo({ top: box.offsetTop - 50, behavior: 'smooth' });
            } catch (error) {
                console.error("AI Error:", error);
                box.innerHTML = "Não foi possível carregar as dicas da IA agora. Siga as orientações padrão.";
                box.style.display = 'block';
            } finally {
                loader.style.display = 'none';
            }
        }
    </script>
</body>
</html>

