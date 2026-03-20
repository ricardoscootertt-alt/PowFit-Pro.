<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Consultoria Esportiva</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #fbbf24;
            --bg-body: #0a0a0a;
            --bg-card: #121212;
            --text-main: #ffffff;
            --text-muted: #a1a1aa;
            --border: #27272a;
            --input-bg: #1c1c1f;
        }

        [data-theme="feminino"] {
            --primary: #f472b6;
            --bg-body: #fff1f2;
            --bg-card: #ffffff;
            --text-main: #18181b;
            --text-muted: #71717a;
            --border: #fbcfe8;
            --input-bg: #fdf2f8;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: background 0.4s, color 0.4s;
            line-height: 1.5;
            padding: 10px;
        }

        .container { max-width: 600px; margin: 0 auto; }

        /* Header */
        header { text-align: center; padding: 30px 0; }
        header h1 { font-family: 'Playfair Display', serif; font-size: 2.5rem; color: var(--primary); font-style: italic; margin-bottom: 5px; }
        header p { text-transform: uppercase; letter-spacing: 3px; font-size: 0.65rem; color: var(--text-muted); font-weight: 700; }

        /* Card Principal */
        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            margin-bottom: 20px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-left: 4px solid var(--primary);
            padding-left: 12px;
        }

        .section-header h2 { font-size: 0.9rem; text-transform: uppercase; font-weight: 900; }

        /* Forms */
        .form-group { margin-bottom: 15px; }
        label { display: block; font-size: 0.7rem; font-weight: 700; text-transform: uppercase; color: var(--text-muted); margin-bottom: 6px; }
        
        input, select {
            width: 100%;
            padding: 12px;
            background: var(--input-bg);
            border: 1px solid var(--border);
            border-radius: 12px;
            color: var(--text-main);
            font-size: 0.95rem;
            outline: none;
            transition: border-color 0.3s;
        }
        input:focus { border-color: var(--primary); }

        .btn {
            width: 100%;
            padding: 16px;
            background: var(--primary);
            color: #000;
            border: none;
            border-radius: 12px;
            font-weight: 900;
            text-transform: uppercase;
            cursor: pointer;
            font-size: 0.9rem;
            transition: transform 0.2s, filter 0.2s;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:active { transform: scale(0.98); }

        /* Painel do Personal (Manual) */
        .manual-panel {
            display: none;
            margin-top: 15px;
            background: rgba(0,0,0,0.05);
            border-radius: 15px;
            padding: 15px;
            border: 1px dashed var(--border);
        }

        .day-selector {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding-bottom: 10px;
            margin-bottom: 15px;
        }

        .day-btn {
            flex: 0 0 50px;
            height: 50px;
            background: var(--input-bg);
            border: 1px solid var(--border);
            border-radius: 10px;
            color: var(--text-muted);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 900;
            cursor: pointer;
        }

        .day-btn.active {
            background: var(--primary);
            color: #000;
            border-color: var(--primary);
        }

        .exercise-search-box {
            max-height: 350px;
            overflow-y: auto;
            border-radius: 10px;
            padding-right: 5px;
        }

        .exercise-cat-group { margin-bottom: 15px; }
        .exercise-cat-title { font-size: 0.6rem; font-weight: 900; background: var(--border); padding: 4px 8px; border-radius: 4px; margin-bottom: 8px; }
        
        .exercise-row {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px;
            background: var(--input-bg);
            margin-bottom: 5px;
            border-radius: 8px;
            font-size: 0.8rem;
        }
        .exercise-row input { width: auto; }

        /* Preview da Ficha */
        #preview-section { display: none; margin-top: 20px; }

        /* Estilo da Ficha (Para Impressão e Preview) */
        #printable-ficha {
            background: white !important;
            color: black !important;
            padding: 15mm;
            width: 210mm; /* A4 width */
            min-height: 297mm;
            margin: 0 auto;
            box-sizing: border-box;
        }

        .ficha-header { border-bottom: 3px solid #000; padding-bottom: 10px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end; }
        .ficha-header h2 { font-family: 'Playfair Display', serif; font-size: 2.2rem; font-weight: 900; color: #000; }
        
        .aluno-info { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 20px; background: #f3f4f6; padding: 12px; border-radius: 8px; }
        .aluno-info div b { display: block; font-size: 0.55rem; text-transform: uppercase; color: #6b7280; }
        .aluno-info div span { font-weight: 800; font-size: 0.8rem; color: #000; }

        .treino-box { margin-bottom: 25px; page-break-inside: avoid; }
        .treino-title { background: #000; color: #fff; padding: 6px 12px; font-weight: 900; font-size: 0.9rem; margin-bottom: 10px; }
        
        table { width: 100%; border-collapse: collapse; }
        th { background: #e5e7eb; padding: 8px; font-size: 0.6rem; text-transform: uppercase; border: 1px solid #d1d5db; text-align: left; }
        td { padding: 8px; font-size: 0.75rem; border: 1px solid #d1d5db; color: #374151; }

        .signature { margin-top: 40px; text-align: right; border-top: 2px solid #000; padding-top: 15px; }
        .signature b { font-size: 1.1rem; display: block; }
        .signature p { font-size: 0.75rem; font-weight: 600; color: #4b5563; }

        /* Loader */
        #loading {
            display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.9);
            z-index: 100; flex-direction: column; align-items: center; justify-content: center;
        }
        .spin { width: 40px; height: 40px; border: 4px solid #222; border-top-color: var(--primary); border-radius: 50%; animation: rot 1s infinite linear; }
        @keyframes rot { to { transform: rotate(360deg); } }

        /* Mobile View Fix */
        .ficha-scroll { overflow-x: auto; background: #222; padding: 10px; border-radius: 10px; }

        @media print {
            body { background: white !important; padding: 0; }
            .container > *:not(#preview-section) { display: none !important; }
            #preview-section { display: block !important; margin: 0; padding: 0; }
            .no-print { display: none !important; }
            #printable-ficha { width: 100% !important; box-shadow: none !important; margin: 0 !important; }
            @page { margin: 0.5cm; }
        }
    </style>
</head>
<body>

    <div id="loading">
        <div class="spin"></div>
        <p style="color: white; margin-top: 15px; font-weight: 800; font-size: 0.7rem; letter-spacing: 2px;">PROCESSANDO DADOS...</p>
    </div>

    <div class="container">
        <header class="no-print">
            <h1>PowFit Pro</h1>
            <p>High Performance Training</p>
        </header>

        <section class="card no-print">
            <div class="section-header">
                <h2>Perfil do Aluno</h2>
                <select id="creationMode" style="width: auto; padding: 4px 8px; font-size: 0.65rem;" onchange="updateMode()">
                    <option value="auto">Geração Automática</option>
                    <option value="manual">Modo Personal (Manual)</option>
                </select>
            </div>

            <form id="trainerForm">
                <div class="form-group">
                    <label>Nome do Atleta</label>
                    <input type="text" id="userName" placeholder="Nome completo" required>
                </div>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                    <div class="form-group">
                        <label>Género</label>
                        <select id="userGender" onchange="document.body.setAttribute('data-theme', this.value)">
                            <option value="masculino">Masculino</option>
                            <option value="feminino">Feminino</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência</label>
                        <select id="userFreq" onchange="updateManualDays()">
                            <option value="3">3 Dias (A-B-C)</option>
                            <option value="6">6 Dias (A ao F)</option>
                        </select>
                    </div>
                </div>

                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                    <div class="form-group">
                        <label>Idade</label>
                        <input type="number" id="userAge" value="25" required>
                    </div>
                    <div class="form-group">
                        <label>Peso (kg)</label>
                        <input type="number" id="userWeight" value="75" required>
                    </div>
                </div>

                <div class="form-group">
                    <label>Objetivo</label>
                    <select id="userGoal">
                        <option>Hipertrofia</option>
                        <option>Emagrecimento</option>
                        <option>Definição Muscular</option>
                        <option>Ganho de Força</option>
                        <option>Saúde Geral</option>
                        <option>Reabilitação</option>
                    </select>
                </div>

                <div class="form-group">
                    <label>Estado de Saúde / Limitações</label>
                    <select id="userHealth">
                        <option value="Saudável">Saudável</option>
                        <option value="Hipertensão">Hipertensão</option>
                        <option value="Diabetes">Diabetes</option>
                        <option value="Lesão no Joelho">Lesão no Joelho</option>
                        <option value="Lesão Lombar">Lesão Lombar / Hérnia</option>
                        <option value="Idoso">Idoso</option>
                        <option value="Gestante">Gestante</option>
                    </select>
                </div>

                <!-- Painel Manual -->
                <div id="manualPanel" class="manual-panel">
                    <label style="color: var(--primary)">Definir Exercícios por Dia:</label>
                    <div class="day-selector" id="daySelector">
                        <!-- Botões de dias injetados -->
                    </div>
                    
                    <div class="exercise-search-box" id="exerciseSearchBox">
                        <!-- Listas de exercícios injetadas -->
                    </div>
                </div>

                <button type="submit" class="btn" style="margin-top: 10px;">Gerar Ficha de Treino</button>
            </form>
        </section>

        <section id="preview-section">
            <div class="no-print" style="margin-bottom: 20px;">
                <button class="btn" onclick="window.print()">🖨️ Imprimir / Baixar PDF</button>
                <button class="btn" style="background: transparent; border: 1px solid var(--primary); color: var(--primary); margin-top: 10px;" onclick="document.getElementById('preview-section').style.display='none'">Editar Dados</button>
            </div>

            <div class="ficha-scroll">
                <div id="printable-ficha">
                    <!-- Conteúdo dinâmico da ficha -->
                </div>
            </div>
        </section>
    </div>

    <script>
        const EXERCISES_DB = {
            "PEITO": ["Supino Reto", "Supino inclinado", "Supino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly (Crucifixo Máquina)", "Pack Fly unilateral", "Poollover"],
            "COSTAS": ["Puxada Alta", "Remada Baixa", "Puxada Unilateral", "Serrote", "Pooldowm"],
            "PERNAS": ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô"],
            "GLÚTEO": ["Elevação de Perna lateral", "Elevação De quadril", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro", "Panturrilha"],
            "BRAÇOS": ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo", "Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            "OMBROS": ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação Borboleta"],
            "CÁRDIO": ["Bicicleta 10min", "Bicicleta 15min", "Bicicleta 20min", "Esteira 10min", "Esteira 15min", "Esteira 20min", "Pula Corda 5× de 30min"]
        };

        let manualSelections = { "A": [], "B": [], "C": [], "D": [], "E": [], "F": [] };
        let currentDay = "A";

        function updateMode() {
            const mode = document.getElementById('creationMode').value;
            document.getElementById('manualPanel').style.display = (mode === 'manual') ? 'block' : 'none';
            if (mode === 'manual') updateManualDays();
        }

        function updateManualDays() {
            const freq = parseInt(document.getElementById('userFreq').value);
            const selector = document.getElementById('daySelector');
            selector.innerHTML = '';
            const days = freq === 3 ? ["A", "B", "C"] : ["A", "B", "C", "D", "E", "F"];
            
            days.forEach(day => {
                const btn = document.createElement('div');
                btn.className = `day-btn ${day === currentDay ? 'active' : ''}`;
                btn.innerText = day;
                btn.onclick = () => { currentDay = day; updateManualDays(); renderExerciseList(); };
                selector.appendChild(btn);
            });
            renderExerciseList();
        }

        function renderExerciseList() {
            const box = document.getElementById('exerciseSearchBox');
            box.innerHTML = '';
            
            for (const cat in EXERCISES_DB) {
                const group = document.createElement('div');
                group.className = 'exercise-cat-group';
                group.innerHTML = `<div class="exercise-cat-title">${cat}</div>`;
                
                EXERCISES_DB[cat].forEach(ex => {
                    const row = document.createElement('label');
                    row.className = 'exercise-row';
                    const checked = manualSelections[currentDay].includes(ex) ? 'checked' : '';
                    row.innerHTML = `
                        <input type="checkbox" ${checked} onchange="toggleExercise('${ex}')">
                        <span>${ex}</span>
                    `;
                    group.appendChild(row);
                });
                box.appendChild(group);
            }
        }

        function toggleExercise(ex) {
            const index = manualSelections[currentDay].indexOf(ex);
            if (index > -1) manualSelections[currentDay].splice(index, 1);
            else manualSelections[currentDay].push(ex);
        }

        document.getElementById('trainerForm').addEventListener('submit', function(e) {
            e.preventDefault();
            document.getElementById('loading').style.display = 'flex';
            
            setTimeout(() => {
                generateSheet();
                document.getElementById('loading').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1000);
        });

        function generateSheet() {
            const container = document.getElementById('printable-ficha');
            const mode = document.getElementById('creationMode').value;
            const user = {
                name: document.getElementById('userName').value,
                gender: document.getElementById('userGender').value,
                age: document.getElementById('userAge').value,
                weight: document.getElementById('userWeight').value,
                goal: document.getElementById('userGoal').value,
                health: document.getElementById('userHealth').value,
                freq: parseInt(document.getElementById('userFreq').value)
            };

            let treinosHTML = "";
            const diasList = user.freq === 3 ? ["A", "B", "C"] : ["A", "B", "C", "D", "E", "F"];

            diasList.forEach(dia => {
                let exercicios = [];
                if (mode === 'manual') {
                    exercicios = manualSelections[dia];
                } else {
                    exercicios = getAutoExercises(dia, user);
                }

                if (exercicios.length > 0) {
                    treinosHTML += `
                        <div class="treino-box">
                            <div class="treino-title">TREINO ${dia} - ${getFoco(dia, user)}</div>
                            <table>
                                <thead>
                                    <tr>
                                        <th>Exercício</th>
                                        <th style="width: 60px;">Séries</th>
                                        <th style="width: 80px;">Reps.</th>
                                        <th>Observações</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${exercicios.map(ex => `
                                        <tr>
                                            <td style="font-weight: 700;">${ex}</td>
                                            <td style="text-align: center;">${user.goal === 'Força' ? '5' : '4'}</td>
                                            <td style="text-align: center;">${user.goal === 'Hipertrofia' ? '8-12' : '15'}</td>
                                            <td style="font-size: 0.65rem; font-style: italic; color: #666;">Descanso 60s. Controle a cadência.</td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>
                    `;
                }
            });

            container.innerHTML = `
                <div class="ficha-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="font-size: 0.6rem; color: #444; font-weight: 800;">CONSULTORIA ESPORTIVA ESPECIALIZADA</p>
                    </div>
                    <div style="text-align: right; font-size: 0.7rem; color: #000;">
                        DATA: ${new Date().toLocaleDateString('pt-br')}
                    </div>
                </div>

                <div class="aluno-info">
                    <div><b>Atleta</b> <span>${user.name}</span></div>
                    <div><b>Objetivo</b> <span>${user.goal}</span></div>
                    <div><b>Saúde</b> <span>${user.health}</span></div>
                    <div><b>Gênero</b> <span>${user.gender.toUpperCase()}</span></div>
                    <div><b>Idade</b> <span>${user.age} anos</span></div>
                    <div><b>Peso</b> <span>${user.weight} kg</span></div>
                </div>

                ${treinosHTML}

                <div class="signature">
                    <p>Prescrição técnica elaborada por</p>
                    <b>Lucas André</b>
                    <p>Personal Trainer | CREF: 008094 - G/RN</p>
                </div>
            `;
        }

        function getFoco(dia, user) {
            if (user.gender === 'feminino') {
                if (dia === "A" || dia === "D") return "Quadríceps e Glúteos 🍑";
                if (dia === "B" || dia === "E") return "Membros Superiores e Core";
                return "Posterior de Coxa e Glúteo Máximo 🍑";
            }
            if (dia === "A" || dia === "D") return "Peito, Ombros e Tríceps";
            if (dia === "B" || dia === "E") return "Costas e Bíceps";
            return "Membros Inferiores Completos";
        }

        function getAutoExercises(dia, user) {
            let pool = [];
            const isFem = user.gender === 'feminino';

            if (isFem) {
                if (dia === "A" || dia === "D") pool = [...EXERCISES_DB.PERNAS.slice(0, 6), "Elevação pelvica", "Afundo"];
                else if (dia === "B" || dia === "E") pool = [...EXERCISES_DB.PEITO.slice(0, 2), ...EXERCISES_DB.COSTAS.slice(0, 2), ...EXERCISES_DB.OMBROS.slice(0, 2)];
                else pool = [...EXERCISES_DB.GLÚTEO, "Stiff", "Mesa Flexora"];
            } else {
                if (dia === "A" || dia === "D") pool = [...EXERCISES_DB.PEITO, ...EXERCISES_DB.OMBROS, "Triceps puley"];
                else if (dia === "B" || dia === "E") pool = [...EXERCISES_DB.COSTAS, "Rosca direta", "Rosca Martelo"];
                else pool = EXERCISES_DB.PERNAS;
            }

            // Filtros de Lesão
            if (user.health === 'Lesão no Joelho') pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro"].includes(e));
            if (user.health === 'Lesão Lombar') pool = pool.filter(e => !["Levantamento terra", "Stiff"].includes(e));

            return pool.sort(() => 0.5 - Math.random()).slice(0, 7);
        }

        // Inicialização
        updateManualDays();
    </script>
</body>
</html>

