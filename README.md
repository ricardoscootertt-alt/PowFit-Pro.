<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Sistema de Prescrição Elite</title>
    
    <!-- Fontes -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            /* Tema Masculino (Padrão) */
            --primary: #fbbf24;
            --bg-body: #0a0a0a;
            --bg-card: #141414;
            --text-main: #ffffff;
            --text-muted: #a1a1aa;
            --border: #27272a;
            --transition: all 0.3s ease;
        }

        [data-theme="feminino"] {
            /* Tema Feminino */
            --primary: #f472b6;
            --bg-body: #fff1f2;
            --bg-card: #ffffff;
            --text-main: #18181b;
            --text-muted: #71717a;
            --border: #fbcfe8;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: var(--transition);
            padding: 20px;
        }

        .container { max-width: 1000px; margin: 0 auto; }

        header { text-align: center; padding: 40px 0; }
        header h1 { font-family: 'Playfair Display', serif; font-size: 3rem; color: var(--primary); font-weight: 900; font-style: italic; }
        header p { letter-spacing: 4px; font-size: 0.7rem; color: var(--text-muted); text-transform: uppercase; }

        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            margin-bottom: 30px;
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

        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .form-group { margin-bottom: 15px; }
        
        label { display: block; font-size: 0.65rem; font-weight: 700; text-transform: uppercase; margin-bottom: 5px; color: var(--text-muted); }

        input, select {
            width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid var(--border);
            border-radius: 10px; color: var(--text-main); font-size: 0.9rem; outline: none; transition: 0.3s;
        }

        [data-theme="feminino"] input, [data-theme="feminino"] select { background: #fff; color: #18181b; }

        /* Módulo Personal */
        .manual-selector {
            display: none;
            margin-top: 20px;
            max-height: 400px;
            overflow-y: auto;
            padding-right: 10px;
            border: 1px solid var(--border);
            padding: 15px;
            border-radius: 10px;
        }

        .exercise-category { margin-bottom: 20px; }
        .category-name {
            font-size: 0.6rem; background: var(--primary); color: #000; padding: 3px 8px;
            border-radius: 4px; font-weight: 900; margin-bottom: 10px; display: inline-block;
        }

        .exercise-list { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 8px; }
        .exercise-item {
            display: flex; align-items: center; gap: 8px; font-size: 0.75rem;
            background: rgba(255,255,255,0.03); padding: 8px; border-radius: 6px; cursor: pointer;
        }
        .exercise-item input { width: auto; }

        .btn {
            width: 100%; padding: 18px; background: var(--primary); color: #000; border: none; border-radius: 12px;
            font-weight: 900; text-transform: uppercase; cursor: pointer; transition: 0.3s; font-size: 0.9rem;
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { filter: brightness(1.1); transform: translateY(-2px); }

        /* Área de Visualização da Ficha (Tela) */
        #preview-section { display: none; margin-top: 30px; }
        
        #sheet-container {
            background: #fff; color: #000; padding: 20mm; width: 210mm; min-height: 297mm;
            margin: 0 auto; box-shadow: 0 0 40px rgba(0,0,0,0.5);
        }

        /* Estilos da Ficha */
        .sheet-header { border-bottom: 4px solid #000; padding-bottom: 10px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end; }
        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.5rem; font-weight: 900; font-style: italic; color: #000; margin: 0; }
        
        .patient-info { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 20px; background: #f1f5f9; padding: 15px; border-radius: 8px; color: #000; }
        .info-box b { display: block; font-size: 0.55rem; text-transform: uppercase; color: #64748b; }
        .info-box span { font-weight: 800; color: #0f172a; font-size: 0.8rem; }

        .workout-block { margin-bottom: 25px; page-break-inside: avoid; color: #000; }
        .workout-title { background: #000; color: #fff; padding: 6px 15px; font-weight: 900; margin-bottom: 8px; font-size: 0.9rem; display: flex; justify-content: space-between; }
        
        table { width: 100%; border-collapse: collapse; color: #000; }
        th { background: #e2e8f0; text-align: left; padding: 8px; border: 1px solid #cbd5e1; font-size: 0.6rem; text-transform: uppercase; }
        td { padding: 8px; border: 1px solid #cbd5e1; font-size: 0.75rem; color: #334155; }

        .signature { margin-top: 40px; text-align: right; border-top: 2px solid #000; padding-top: 15px; color: #000; }
        .signature b { font-size: 1.2rem; display: block; font-weight: 900; font-style: italic; }

        /* Loader */
        #loader {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.95); z-index: 9999; justify-content: center; align-items: center; flex-direction: column;
        }
        .spinner { width: 50px; height: 50px; border: 5px solid #1a1a1a; border-top: 5px solid var(--primary); border-radius: 50%; animation: spin 1s infinite linear; }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* CONFIGURAÇÃO DE IMPRESSÃO */
        @media print {
            body { background: #fff !important; padding: 0; margin: 0; }
            .container > *:not(#preview-section) { display: none !important; }
            #preview-section { margin: 0; padding: 0; display: block !important; }
            .no-print { display: none !important; }
            #sheet-container { box-shadow: none !important; margin: 0 !important; width: 100% !important; padding: 0 !important; }
            @page { margin: 1cm; }
        }

        @media (max-width: 850px) {
            #sheet-container { width: 100%; padding: 10mm; min-height: auto; }
            .patient-info { grid-template-columns: 1fr 1fr; }
        }
    </style>
</head>
<body>

    <div id="loader">
        <div class="spinner"></div>
        <p style="font-weight: 900; margin-top: 15px; letter-spacing: 2px; color: #fff;">SISTEMA ATIVO...</p>
    </div>

    <div class="container">
        <header class="no-print">
            <h1>PowFit Pro</h1>
            <p>Gerador Profissional de Fichas de Treino</p>
        </header>

        <section class="card no-print">
            <div class="section-title">
                <span>Dados do Usuário</span>
                <div style="font-size: 0.6rem;">
                    Módulo Personal: 
                    <select id="mode" style="width: auto; padding: 2px 5px;" onchange="toggleManual()">
                        <option value="auto">Automático</option>
                        <option value="manual">Manual</option>
                    </select>
                </div>
            </div>

            <form id="mainForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome Completo</label>
                        <input type="text" id="name" placeholder="Ex: Maria Oliveira" required>
                    </div>
                    <div class="form-group">
                        <label>Género</label>
                        <select id="gender" onchange="document.body.setAttribute('data-theme', this.value)">
                            <option value="masculino">Masculino</option>
                            <option value="feminino">Feminino</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Idade / Peso (kg)</label>
                        <div style="display: flex; gap: 5px;">
                            <input type="number" id="age" placeholder="Anos" required>
                            <input type="number" id="weight" placeholder="Kg" required>
                        </div>
                    </div>
                </div>

                <div class="grid">
                    <div class="form-group">
                        <label>Objetivo Principal</label>
                        <select id="goal">
                            <option>Hipertrofia</option>
                            <option>Emagrecimento</option>
                            <option>Definição Muscular</option>
                            <option>Ganho de Força</option>
                            <option>Saúde Geral</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Estado de Saúde</label>
                        <select id="health">
                            <option value="saudavel">Saudável</option>
                            <option value="hipertensao">Hipertensão / Cardíaco</option>
                            <option value="lesao_joelho">Lesão no Joelho</option>
                            <option value="lesao_lombar">Hérnia / Lombar</option>
                            <option value="idoso">Idoso (Mobilidade)</option>
                            <option value="gestante">Gestante</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência</label>
                        <select id="freq">
                            <option value="3">3 Dias (A-B-C)</option>
                            <option value="6">6 Dias (Semanal)</option>
                        </select>
                    </div>
                </div>

                <!-- Painel Manual -->
                <div id="manualPanel" class="manual-selector">
                    <div class="section-title"><span>Seleção Manual de Exercícios</span></div>
                    <div id="exerciseCategories"></div>
                </div>

                <button type="submit" class="btn" style="margin-top: 20px;">Gerar Prévia da Ficha</button>
            </form>
        </section>

        <section id="preview-section">
            <div class="no-print" style="text-align: center; margin-bottom: 20px;">
                <button class="btn" onclick="window.print()" style="max-width: 300px;">🖨️ IMPRIMIR / SALVAR PDF</button>
                <p style="font-size: 0.6rem; color: var(--text-muted); margin-top: 10px;">Dica: Selecione "Salvar como PDF" no destino da impressão.</p>
            </div>
            
            <div id="sheet-container">
                <!-- Conteúdo gerado -->
            </div>
        </section>
    </div>

    <script>
        const EXERCISES = {
            "PEITO": ["Supino Reto", "Supino inclinado", "Supino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly (Crucifixo Máquina)", "Pack Fly unilateral", "Poollover"],
            "COSTAS": ["Puxada Alta", "Remada Baixa", "Puxada Unilateral", "Serrote", "Pooldowm"],
            "PERNAS": ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô"],
            "GLÚTEO": ["Elevação de Perna lateral", "Elevação De quadril", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro"],
            "BRAÇOS": ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo", "Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            "OMBROS": ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação Borboleta"],
            "CÁRDIO": ["Bicicleta 10min", "Bicicleta 15min", "Bicicleta 20min", "Esteira 10min", "Esteira 15min", "Esteira 20min", "Pula Corda 5× de 30min"]
        };

        const categoriesDiv = document.getElementById('exerciseCategories');
        for (const cat in EXERCISES) {
            const div = document.createElement('div');
            div.className = 'exercise-category';
            div.innerHTML = `<div class="category-name">${cat}</div><div class="exercise-list">
                ${EXERCISES[cat].map(ex => `<label class="exercise-item"><input type="checkbox" name="ex-sel" value="${ex}" data-cat="${cat}"> ${ex}</label>`).join('')}
            </div>`;
            categoriesDiv.appendChild(div);
        }

        function toggleManual() {
            document.getElementById('manualPanel').style.display = document.getElementById('mode').value === 'manual' ? 'block' : 'none';
        }

        document.getElementById('mainForm').addEventListener('submit', (e) => {
            e.preventDefault();
            document.getElementById('loader').style.display = 'flex';
            setTimeout(() => {
                renderSheet();
                document.getElementById('loader').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1000);
        });

        function renderSheet() {
            const container = document.getElementById('sheet-container');
            const data = {
                name: document.getElementById('name').value,
                gender: document.getElementById('gender').value,
                age: document.getElementById('age').value,
                weight: document.getElementById('weight').value,
                goal: document.getElementById('goal').value,
                health: document.getElementById('health').value,
                freq: parseInt(document.getElementById('freq').value),
                mode: document.getElementById('mode').value
            };

            let workoutContent = "";
            const isFem = data.gender === 'feminino';

            if (data.mode === 'manual') {
                const selected = Array.from(document.querySelectorAll('input[name="ex-sel"]:checked')).map(cb => ({ name: cb.value, cat: cb.dataset.cat }));
                if (selected.length === 0) { alert("Selecione exercícios!"); document.getElementById('loader').style.display = 'none'; return; }
                
                workoutContent = `<div class="workout-block"><div class="workout-title">TREINO PERSONALIZADO</div><table><thead><tr><th>Exercício</th><th style="width:80px">Séries</th><th style="width:80px">Reps</th><th>Observações</th></tr></thead><tbody>
                    ${selected.map(ex => `<tr><td><b>${ex.name}</b><br><small>${ex.cat}</small></td><td>4</td><td>12</td><td>Foco na técnica.</td></tr>`).join('')}
                </tbody></table></div>`;
            } else {
                for (let i = 0; i < data.freq; i++) {
                    const letra = ["A", "B", "C"][i % 3];
                    workoutContent += `<div class="workout-block"><div class="workout-title"><span>TREINO ${letra}</span><span style="font-size:0.6rem">${getFoco(letra, isFem)}</span></div><table><thead><tr><th>Exercício</th><th style="width:80px">Séries</th><th style="width:80px">Reps</th><th>Observações</th></tr></thead><tbody>
                        ${getRows(letra, data)}
                    </tbody></table></div>`;
                }
            }

            container.innerHTML = `
                <div class="sheet-header">
                    <div><h2>POWFIT PRO</h2><p style="font-size:0.6rem; color:#444; font-weight:700">SISTEMA DE PRESCRIÇÃO - LUCAS ANDRÉ</p></div>
                    <div style="text-align:right; font-size:0.7rem">EMISSÃO: ${new Date().toLocaleDateString('pt-br')}</div>
                </div>
                <div class="patient-info">
                    <div class="info-box"><b>Aluno(a)</b><span>${data.name}</span></div>
                    <div class="info-box"><b>Objetivo</b><span>${data.goal}</span></div>
                    <div class="info-box"><b>Saúde</b><span>${data.health.toUpperCase()}</span></div>
                    <div class="info-box"><b>Gênero</b><span>${data.gender.toUpperCase()}</span></div>
                    <div class="info-box"><b>Idade</b><span>${data.age} anos</span></div>
                    <div class="info-box"><b>Peso</b><span>${data.weight} kg</span></div>
                </div>
                ${workoutContent}
                <div class="signature">
                    <p style="font-size:0.6rem; color:#666">Prescrição feita por</p>
                    <b>Lucas André | CREF: 008094-G/RN </b><span>Personal Trainer</span>
                </div>`;
        }

        function getFoco(letra, isFem) {
            if (isFem) {
                if (letra === 'A') return "Quadríceps e Glúteo 🍑";
                if (letra === 'B') return "Superiores e Abdômen";
                return "Posterior e Glúteo Máximo 🍑";
            }
            if (letra === 'A') return "Peito, Ombros e Tríceps";
            if (letra === 'B') return "Costas e Bíceps";
            return "Membros Inferiores";
        }

        function getRows(letra, data) {
            let pool = [];
            const isFem = data.gender === 'feminino';
            if (isFem) {
                if (letra === 'A') pool = [...EXERCISES.PERNAS.slice(0, 6), "Elevação pelvica", "Afundo"];
                else if (letra === 'B') pool = [...EXERCISES.PEITO.slice(0,2), ...EXERCISES.COSTAS.slice(0,2), ...EXERCISES.OMBROS.slice(0,2)];
                else pool = [...EXERCISES.GLÚTEO, "Stiff", "Mesa Flexora"];
            } else {
                if (letra === 'A') pool = [...EXERCISES.PEITO, ...EXERCISES.OMBROS, "Triceps puley"];
                else if (letra === 'B') pool = [...EXERCISES.COSTAS, "Rosca direta", "Rosca Martelo"];
                else pool = EXERCISES.PERNAS;
            }

            if (data.health === 'lesao_joelho') pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro"].includes(e));
            if (data.health === 'lesao_lombar') pool = pool.filter(e => !["Levantamento terra", "Stiff"].includes(e));

            const sel = pool.sort(() => 0.5 - Math.random()).slice(0, 6);
            return sel.map(ex => `<tr><td>${ex}</td><td>4</td><td>${data.goal === 'Hipertrofia' ? '8-12' : '15'}</td><td>Intervalo 60s.</td></tr>`).join('');
        }
    </script>
</body>
</html>

