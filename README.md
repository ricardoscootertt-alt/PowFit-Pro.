<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Sistema de Prescrição Profissional</title>
    
    <!-- Fontes -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #fbbf24;
            --bg-body: #0a0a0a;
            --bg-card: #141414;
            --text-main: #ffffff;
            --text-muted: #a1a1aa;
            --border: #27272a;
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
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
            padding: 10px;
            line-height: 1.4;
        }

        .container { max-width: 100%; margin: 0 auto; }

        /* Header */
        header { text-align: center; padding: 30px 0; }
        header h1 { 
            font-family: 'Playfair Display', serif; 
            font-size: 2.5rem; 
            color: var(--primary); 
            font-weight: 900; 
            font-style: italic;
            line-height: 1;
        }
        header p { letter-spacing: 3px; font-size: 0.6rem; color: var(--text-muted); text-transform: uppercase; margin-top: 5px; }

        /* Card de Formulário */
        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            margin-bottom: 20px;
        }

        .section-title {
            font-size: 0.75rem;
            font-weight: 900;
            text-transform: uppercase;
            color: var(--primary);
            margin-bottom: 15px;
            border-bottom: 1px solid var(--border);
            padding-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
        @media (min-width: 600px) { .grid { grid-template-columns: repeat(3, 1fr); } }

        .form-group { margin-bottom: 12px; }
        label { display: block; font-size: 0.6rem; font-weight: 700; text-transform: uppercase; margin-bottom: 5px; color: var(--text-muted); }

        input, select {
            width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid var(--border);
            border-radius: 10px; color: var(--text-main); font-size: 0.9rem; outline: none; transition: 0.3s;
        }

        [data-theme="feminino"] input, [data-theme="feminino"] select { background: #fff; color: #18181b; }

        /* Painel Manual de Exercícios */
        .manual-selector {
            display: none;
            margin-top: 15px;
            border: 1px solid var(--border);
            border-radius: 15px;
            padding: 15px;
            background: rgba(0,0,0,0.1);
        }

        .exercise-category { margin-bottom: 25px; }
        .category-name {
            font-size: 0.6rem; background: var(--primary); color: #000; padding: 4px 10px;
            border-radius: 4px; font-weight: 900; margin-bottom: 12px; display: inline-block;
        }

        .exercise-item-row {
            display: flex;
            flex-direction: column;
            background: rgba(255,255,255,0.03);
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 8px;
            border: 1px solid var(--border);
        }

        .ex-info { font-size: 0.8rem; font-weight: 600; margin-bottom: 8px; }
        .day-selection { display: flex; gap: 5px; overflow-x: auto; padding-bottom: 5px; }
        
        .day-chip {
            flex: 0 0 auto;
            display: flex;
            align-items: center;
            gap: 4px;
            background: var(--border);
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.65rem;
            cursor: pointer;
        }

        .day-chip input { width: auto; height: auto; }
        .day-chip.active { background: var(--primary); color: #000; }

        .btn {
            width: 100%; padding: 16px; background: var(--primary); color: #000; border: none; border-radius: 12px;
            font-weight: 900; text-transform: uppercase; cursor: pointer; transition: 0.3s; font-size: 0.85rem;
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:active { transform: scale(0.98); }

        /* Ficha Técnica (A4 Visual) */
        #preview-section { display: none; margin-top: 20px; }
        
        #sheet-content {
            background: #fff; color: #000; padding: 15mm; width: 100%; max-width: 210mm;
            margin: 0 auto; box-shadow: 0 0 20px rgba(0,0,0,0.2);
        }

        .sheet-header { border-bottom: 3px solid #000; padding-bottom: 10px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end; }
        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2rem; font-weight: 900; font-style: italic; color: #000; margin: 0; }
        
        .info-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 20px; background: #f8fafc; padding: 12px; border-radius: 6px; }
        .info-box b { display: block; font-size: 0.5rem; text-transform: uppercase; color: #64748b; }
        .info-box span { font-weight: 800; color: #0f172a; font-size: 0.75rem; }

        .workout-day { margin-bottom: 25px; page-break-inside: avoid; }
        .day-title { background: #000; color: #fff; padding: 6px 12px; font-weight: 900; margin-bottom: 6px; font-size: 0.8rem; display: flex; justify-content: space-between; }
        
        table { width: 100%; border-collapse: collapse; margin-bottom: 5px; }
        th { background: #f1f5f9; text-align: left; padding: 8px; border: 1px solid #cbd5e1; font-size: 0.55rem; text-transform: uppercase; }
        td { padding: 8px; border: 1px solid #cbd5e1; font-size: 0.7rem; color: #334155; }

        .footer { margin-top: 40px; text-align: right; border-top: 2px solid #000; padding-top: 15px; color: #000; }
        .footer b { font-size: 1.1rem; display: block; font-weight: 900; font-style: italic; }
        .footer p { font-size: 0.7rem; font-weight: 700; color: #334155; }

        /* Loader */
        #loader {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 10000; justify-content: center; align-items: center; flex-direction: column;
        }
        .spin { width: 40px; height: 40px; border: 4px solid #1a1a1a; border-top: 4px solid var(--primary); border-radius: 50%; animation: spin 1s infinite linear; }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* PRINT SETTINGS */
        @media print {
            body { background: #fff !important; padding: 0; margin: 0; }
            .no-print { display: none !important; }
            #preview-section { display: block !important; margin: 0 !important; }
            #sheet-content { box-shadow: none !important; width: 100% !important; max-width: none !important; padding: 10mm !important; }
            @page { size: A4; margin: 0; }
        }
    </style>
</head>
<body>

    <div id="loader">
        <div class="spin"></div>
        <p style="margin-top: 15px; font-weight: 900; font-size: 0.6rem; color: #fff; letter-spacing: 2px;">PROCESSANDO...</p>
    </div>

    <div class="container no-print">
        <header>
            <h1>PowFit Pro</h1>
            <p>High Performance Systems</p>
        </header>

        <section class="card">
            <div class="section-title">
                <span>Dados do Aluno</span>
                <select id="creationMode" style="width: auto; padding: 4px; font-size: 0.6rem;" onchange="updateMode()">
                    <option value="auto">Automático</option>
                    <option value="manual">Módulo Personal</option>
                </select>
            </div>

            <form id="trainerForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome Completo</label>
                        <input type="text" id="userName" placeholder="Nome do aluno" required>
                    </div>
                    <div class="form-group">
                        <label>Gênero</label>
                        <select id="userGender" onchange="document.body.setAttribute('data-theme', this.value)">
                            <option value="masculino">Masculino</option>
                            <option value="feminino">Feminino</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Idade / Peso</label>
                        <div style="display: flex; gap: 5px;">
                            <input type="number" id="userAge" placeholder="Idade" required>
                            <input type="number" id="userWeight" placeholder="Kg" required>
                        </div>
                    </div>
                </div>

                <div class="grid">
                    <div class="form-group">
                        <label>Objetivo</label>
                        <select id="userGoal">
                            <option>Hipertrofia</option>
                            <option>Emagrecimento</option>
                            <option>Definição</option>
                            <option>Ganho de Força</option>
                            <option>Condicionamento</option>
                            <option>Reabilitação</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Estado de Saúde</label>
                        <select id="userHealth">
                            <option value="saudavel">Saudável</option>
                            <option value="hipertensao">Hipertensão / Cardíaco</option>
                            <option value="diabetes">Diabetes</option>
                            <option value="joelho">Lesão Joelho</option>
                            <option value="lombar">Lesão Lombar / Hérnia</option>
                            <option value="idoso">Idoso (Mobilidade)</option>
                            <option value="gestante">Gestante / Lactante</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência</label>
                        <select id="userFreq" onchange="updateDayChips()">
                            <option value="3">3 Dias (A-B-C)</option>
                            <option value="5">5 Dias</option>
                            <option value="6">6 Dias</option>
                        </select>
                    </div>
                </div>

                <!-- Painel de Seleção Personal -->
                <div id="manualPanel" class="manual-selector">
                    <div class="section-title"><span>Prescrição Dinâmica</span></div>
                    <div id="exerciseListContainer"></div>
                </div>

                <button type="submit" class="btn" style="margin-top: 20px;">Gerar Ficha de Treino</button>
            </form>
        </section>
    </div>

    <!-- Seção de Visualização e Impressão -->
    <section id="preview-section">
        <div class="no-print" style="padding: 15px; text-align: center;">
            <button class="btn" style="max-width: 320px;" onclick="window.print()">🖨️ IMPRIMIR / SALVAR PDF</button>
            <p style="font-size: 0.6rem; color: #888; margin-top: 10px;">Selecione "Salvar como PDF" no destino da impressão para baixar o arquivo.</p>
        </div>
        
        <div id="sheet-content">
            <!-- Gerado via JavaScript -->
        </div>
    </section>

    <script>
        const EXERCISES_DATABASE = {
            "PEITO": ["Supino Reto", "Supino inclinado", "Supino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly (Crucifixo Máquina)", "Pack Fly unilateral", "Poollover"],
            "COSTAS": ["Puxada Alta", "Remada aberta", "Remada Baixa", "Remada Curvada", "Puxada Unilateral", "Serrote", "Pooldowm", "Facepull"],
            "PERNAS": ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô"],
            "GLÚTEO": ["Elevação de Perna lateral", "Elevação De quadril", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro", "Panturrilha Banco", "Panturrilha squat"],
            "BRAÇOS": ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo", "Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            "OMBROS": ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação Borboleta"],
            "ABDOMEN": ["Abdôminal Infra", "Abdôminal remador", "Abdôminal Prancha 5× de 30seg"],
            "CÁRDIO": ["Bicicleta 10min", "Bicicleta 15min", "Bicicleta 20min", "Esteira 10min", "Esteira 15min", "Esteira 20min", "Pula Corda 5× de 30seg"]
        };

        function updateMode() {
            const mode = document.getElementById('creationMode').value;
            document.getElementById('manualPanel').style.display = (mode === 'manual') ? 'block' : 'none';
            if(mode === 'manual') renderManualList();
        }

        function updateDayChips() {
            if(document.getElementById('creationMode').value === 'manual') renderManualList();
        }

        function renderManualList() {
            const container = document.getElementById('exerciseListContainer');
            const freq = parseInt(document.getElementById('userFreq').value);
            container.innerHTML = "";

            for (const cat in EXERCISES_DATABASE) {
                const catDiv = document.createElement('div');
                catDiv.className = 'exercise-category';
                catDiv.innerHTML = `<div class="category-name">${cat}</div>`;
                
                EXERCISES_DATABASE[cat].forEach(ex => {
                    const row = document.createElement('div');
                    row.className = 'exercise-item-row';
                    
                    let chips = "";
                    for(let d=1; d <= freq; d++) {
                        chips += `
                            <label class="day-chip" id="chip-${ex}-${d}">
                                <input type="checkbox" name="manual-ex" value="${ex}" data-day="${d}" onchange="this.parentElement.classList.toggle('active', this.checked)">
                                D${d}
                            </label>
                        `;
                    }

                    row.innerHTML = `
                        <div class="ex-info">${ex}</div>
                        <div class="day-selection">${chips}</div>
                    `;
                    catDiv.appendChild(row);
                });
                container.appendChild(catDiv);
            }
        }

        document.getElementById('trainerForm').addEventListener('submit', function(e) {
            e.preventDefault();
            document.getElementById('loader').style.display = 'flex';
            
            setTimeout(() => {
                const data = {
                    name: document.getElementById('userName').value,
                    gender: document.getElementById('userGender').value,
                    age: document.getElementById('userAge').value,
                    weight: document.getElementById('userWeight').value,
                    goal: document.getElementById('userGoal').value,
                    health: document.getElementById('userHealth').value,
                    freq: parseInt(document.getElementById('userFreq').value),
                    mode: document.getElementById('creationMode').value
                };

                renderSheet(data);
                document.getElementById('loader').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1000);
        });

        function renderSheet(data) {
            const sheet = document.getElementById('sheet-content');
            let workoutHTML = "";
            const labels = ["A", "B", "C", "D", "E", "F"];

            if(data.mode === 'manual') {
                const checks = Array.from(document.querySelectorAll('input[name="manual-ex"]:checked'));
                if(checks.length === 0) { alert("Selecione os dias para os exercícios!"); document.getElementById('loader').style.display = 'none'; return; }

                for(let d=1; d <= data.freq; d++) {
                    const dailyEx = checks.filter(c => parseInt(c.dataset.day) === d);
                    if(dailyEx.length > 0) {
                        workoutHTML += `<div class="workout-day"><div class="day-title">DIA ${d} - PRESCRIÇÃO PERSONALIZADA</div><table><thead><tr><th>Exercício</th><th style="width:50px">Séries</th><th style="width:70px">Reps</th><th>Observações</th></tr></thead><tbody>
                            ${dailyEx.map(c => `<tr><td>${c.value}</td><td>4</td><td>12</td><td>Execução focada.</td></tr>`).join('')}
                        </tbody></table></div>`;
                    }
                }
            } else {
                // Auto Mode com Inteligência de Gênero
                const isFem = data.gender === 'feminino';
                for(let i=0; i < data.freq; i++) {
                    const dia = i + 1;
                    const letra = labels[i % 3];
                    workoutHTML += `<div class="workout-day"><div class="day-title"><span>DIA ${dia} - TREINO ${letra}</span><span style="font-size:0.55rem">${getFoco(letra, isFem)}</span></div><table><thead><tr><th>Exercício</th><th style="width:50px">Séries</th><th style="width:70px">Reps</th><th>Observações</th></tr></thead><tbody>
                        ${getAutoRows(letra, data)}
                    </tbody></table></div>`;
                }
            }

            sheet.innerHTML = `
                <div class="sheet-header">
                    <div><h2>POWFIT PRO</h2><p style="font-size:0.5rem; color:#444; font-weight:800">SISTEMA DE PRESCRIÇÃO DE ELITE</p></div>
                    <div style="text-align:right; font-size:0.6rem">DATA: ${new Date().toLocaleDateString('pt-br')}</div>
                </div>
                <div class="info-grid">
                    <div class="info-box"><b>Aluno(a)</b><span>${data.name}</span></div>
                    <div class="info-box"><b>Objetivo</b><span>${data.goal}</span></div>
                    <div class="info-box"><b>Saúde</b><span>${data.health.toUpperCase()}</span></div>
                    <div class="info-box"><b>Gênero</b><span>${data.gender.toUpperCase()}</span></div>
                    <div class="info-box"><b>Idade</b><span>${data.age} anos</span></div>
                    <div class="info-box"><b>Peso</b><span>${data.weight} kg</span></div>
                </div>
                ${workoutHTML}
                <div class="footer">
                    <p style="font-size: 0.5rem; color: #777; margin-bottom: 2px;">Prescrição validada biomecanicamente por</p>
                    <b>Lucas André</b>
                    <p>Personal Trainer - CREF: 008094 - G/RN</p>
                </div>
            `;
        }

        function getFoco(letra, isFem) {
            if (isFem) {
                if (letra === 'A') return "Quadríceps e Glúteo 🍑";
                if (letra === 'B') return "Superiores e Abdômen";
                return "Posterior e Glúteo Máximo 🍑";
            }
            if (letra === 'A') return "Peito, Ombros e Tríceps";
            if (letra === 'B') return "Costas e Bíceps";
            return "Inferiores e Panturrilha";
        }

        function getAutoRows(letra, data) {
            let pool = [];
            const isFem = data.gender === 'feminino';
            
            if (isFem) {
                if (letra === 'A') pool = [...EXERCISES_DATABASE.PERNAS.slice(0, 6), "Elevação pelvica", "Afundo"];
                else if (letra === 'B') pool = [...EXERCISES_DATABASE.PEITO.slice(0,2), ...EXERCISES_DATABASE.COSTAS.slice(0,2), ...EXERCISES_DATABASE.ABDOMEN];
                else pool = [...EXERCISES_DATABASE.GLÚTEO, "Stiff", "Mesa Flexora"];
            } else {
                if (letra === 'A') pool = [...EXERCISES_DATABASE.PEITO, ...EXERCISES_DATABASE.OMBROS, "Triceps puley"];
                else if (letra === 'B') pool = [...EXERCISES_DATABASE.COSTAS, "Rosca direta", "Rosca Martelo", "Abdômen remador"];
                else pool = [...EXERCISES_DATABASE.PERNAS, "Panturrilha Banco"];
            }

            // Filtros de Segurança
            if (data.health === 'joelho') pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro"].includes(e));
            if (data.health === 'lombar') pool = pool.filter(e => !["Levantamento terra", "Stiff", "Agachamento com Barra"].includes(e));

            const sel = pool.sort(() => 0.5 - Math.random()).slice(0, 7);
            const r = data.goal === 'Hipertrofia' ? '8-12' : '15-20';
            return sel.map(ex => `<tr><td>${ex}</td><td>4</td><td>${r}</td><td>Cadência controlada.</td></tr>`).join('');
        }
    </script>
</body>
</html>

