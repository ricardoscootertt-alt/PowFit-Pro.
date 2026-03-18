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

        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-body);
            color: var(--text-main);
            transition: var(--transition);
            padding: 20px;
            overflow-x: hidden;
        }

        .container { max-width: 900px; margin: 0 auto; }

        header {
            text-align: center;
            padding: 60px 0;
        }

        header h1 {
            font-family: 'Playfair Display', serif;
            font-size: 3.5rem;
            color: var(--primary);
            text-transform: uppercase;
            letter-spacing: -2px;
            font-weight: 900;
            font-style: italic;
        }

        header p {
            color: var(--text-muted);
            letter-spacing: 4px;
            font-size: 0.75rem;
            font-weight: 600;
        }

        /* Form Card */
        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 24px;
            padding: 30px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            margin-bottom: 40px;
        }

        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; }
        .form-group { margin-bottom: 20px; }
        
        label {
            display: block;
            font-size: 0.7rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 8px;
            color: var(--text-muted);
        }

        input, select {
            width: 100%;
            padding: 14px 18px;
            background: rgba(255,255,255,0.05);
            border: 2px solid var(--border);
            border-radius: 12px;
            color: var(--text-main);
            font-size: 0.95rem;
            outline: none;
            transition: var(--transition);
        }

        [data-theme="feminino"] input, 
        [data-theme="feminino"] select {
            background: #fff;
            border-color: #fce7f3;
            color: #1e293b;
        }

        input:focus, select:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(251, 191, 36, 0.1);
        }

        .btn {
            width: 100%;
            padding: 20px;
            background: var(--primary);
            color: #000;
            border: none;
            border-radius: 14px;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: 1px;
            cursor: pointer;
            transition: var(--transition);
            font-size: 1rem;
            margin-top: 10px;
        }

        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { transform: translateY(-3px); filter: brightness(1.1); box-shadow: 0 10px 20px rgba(0,0,0,0.2); }

        /* Ficha A4 Styles (Hidden for UI, used for generation) */
        #printable-sheet {
            display: none; /* Só aparece no momento de gerar ou no preview */
            background: white !important;
            color: black !important;
            width: 210mm;
            min-height: 297mm;
            padding: 20mm;
            margin: 0 auto;
            box-sizing: border-box;
            position: relative;
        }

        .sheet-header {
            border-bottom: 4px solid #000;
            padding-bottom: 15px;
            margin-bottom: 25px;
            display: flex;
            justify-content: space-between;
            align-items: flex-end;
        }

        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.8rem; font-weight: 900; font-style: italic; margin: 0; }

        .patient-info {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 30px;
            background: #f1f5f9;
            padding: 15px;
            border-radius: 8px;
        }

        .info-box b { display: block; font-size: 0.6rem; text-transform: uppercase; color: #64748b; }
        .info-box span { font-weight: 800; color: #0f172a; font-size: 0.85rem; }

        .workout-block { margin-bottom: 35px; page-break-inside: avoid; }
        .workout-title {
            background: #000;
            color: #fff;
            padding: 8px 15px;
            font-weight: 900;
            margin-bottom: 12px;
            font-size: 1.1rem;
            display: flex;
            justify-content: space-between;
        }

        table { width: 100%; border-collapse: collapse; }
        th { background: #e2e8f0; text-align: left; padding: 10px; border: 1px solid #cbd5e1; font-size: 0.7rem; text-transform: uppercase; }
        td { padding: 10px; border: 1px solid #cbd5e1; font-size: 0.85rem; color: #334155; }

        .signature {
            margin-top: 60px;
            text-align: right;
            border-top: 2px solid #000;
            padding-top: 20px;
        }

        .signature b { font-size: 1.4rem; display: block; font-weight: 900; font-style: italic; }
        .signature span { color: #64748b; font-size: 0.8rem; font-weight: bold; text-transform: uppercase; }

        /* Loader */
        #loader {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.95);
            z-index: 9999;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            color: white;
        }

        .spinner {
            width: 60px; height: 60px;
            border: 6px solid #1a1a1a;
            border-top: 6px solid var(--primary);
            border-radius: 50%;
            animation: spin 1s infinite linear;
            margin-bottom: 20px;
        }

        @keyframes spin { to { transform: rotate(360deg); } }

        #preview-section {
            display: none;
            margin-top: 40px;
        }

        .actions-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-bottom: 40px;
        }

        /* Ajuste para mobile viewing da ficha */
        .sheet-container {
            overflow-x: auto;
            background: rgba(0,0,0,0.1);
            padding: 20px 0;
        }

        @media (max-width: 850px) {
            header h1 { font-size: 2.5rem; }
            #printable-sheet { width: 100%; padding: 10mm; min-height: auto; }
            .patient-info { grid-template-columns: 1fr 1fr; }
        }
    </style>
</head>
<body>

    <div id="loader">
        <div class="spinner"></div>
        <p style="font-weight: 800; letter-spacing: 3px; font-size: 0.8rem;">SINCRO-BIOMECÂNICA ATIVA</p>
    </div>

    <div class="container">
        <header>
            <h1>PowFit Pro</h1>
            <p>SISTEMA DE PRESCRIÇÃO E ALTA PERFORMANCE</p>
        </header>

        <section class="card">
            <form id="workoutForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome do Aluno(a)</label>
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
                        <div style="display: flex; gap: 10px;">
                            <input type="number" id="age" placeholder="Idade" required>
                            <input type="number" id="weight" placeholder="Peso" required>
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
                            <option>Saúde e Longevidade</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Condição de Saúde</label>
                        <select id="health">
                            <option value="saudavel">Saudável</option>
                            <option value="sedentario">Sedentário</option>
                            <option value="hipertensao">Hipertensão / Cardíaco</option>
                            <option value="diabetes">Diabetes</option>
                            <option value="joelho">Lesão no Joelho</option>
                            <option value="lombar">Hérnia de Disco / Lombar</option>
                            <option value="idoso">Idoso (Mobilidade)</option>
                            <option value="gestante">Gestante</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência Semanal</label>
                        <select id="freq">
                            <option value="3">3 Dias (A-B-C)</option>
                            <option value="6">6 Dias (A-B-C-A-B-C)</option>
                        </select>
                    </div>
                </div>

                <button type="submit" class="btn">Gerar Prescrição Inteligente</button>
            </form>
        </section>

        <section id="preview-section">
            <div class="actions-group">
                <button class="btn" onclick="exportPDF()" style="max-width: 300px;">Download Ficha PDF (A4)</button>
                <button class="btn" onclick="window.print()" style="max-width: 150px; background: #334155; color: white;">Imprimir</button>
            </div>
            
            <div class="sheet-container">
                <div id="printable-sheet">
                    <!-- Conteúdo será injetado aqui -->
                </div>
            </div>
        </section>
    </div>

    <script>
        const EXERCISES_DB = {
            PERNAS: ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô", "Elevação de Perna lateral", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro"],
            PEITO: ["Supino Reto", "Supino inclinado", "Suplino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly", "Poollover"],
            COSTAS: ["Puxada Alta", "Remada Baixa", "Puxada Unilateral", "Serrote", "Pooldowm"],
            BICEPS: ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo"],
            TRICEPS: ["Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            OMBROS: ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação borboleta", "Facepool"],
            FUNCIONAL: ["Mobilidade de Quadril", "Prancha", "Caminhada Inclinada", "Agachamento Global"]
        };

        const form = document.getElementById('workoutForm');
        const loader = document.getElementById('loader');
        const preview = document.getElementById('preview-section');
        const sheet = document.getElementById('printable-sheet');

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            loader.style.display = 'flex';

            setTimeout(() => {
                const data = {
                    name: document.getElementById('name').value,
                    gender: document.getElementById('gender').value,
                    age: document.getElementById('age').value,
                    weight: document.getElementById('weight').value,
                    goal: document.getElementById('goal').value,
                    health: document.getElementById('health').value,
                    freq: parseInt(document.getElementById('freq').value)
                };

                renderWorkout(data);
                loader.style.display = 'none';
                preview.style.display = 'block';
                sheet.style.display = 'block';
                
                window.scrollTo({ top: preview.offsetTop - 20, behavior: 'smooth' });
            }, 1200);
        });

        function renderWorkout(data) {
            let html = `
                <div class="sheet-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="font-size: 0.75rem; color: #444; font-weight: 700;">PRESCRIÇÃO TÉCNICA - LUCAS ANDRÉ PERSONAL</p>
                    </div>
                    <div style="text-align: right; font-size: 0.75rem; color: #444; font-weight: 600;">
                        EMISSÃO: ${new Date().toLocaleDateString('pt-br')}
                    </div>
                </div>

                <div class="patient-info">
                    <div class="info-box"><b>Aluno(a)</b> <span>${data.name}</span></div>
                    <div class="info-box"><b>Objetivo</b> <span>${data.goal}</span></div>
                    <div class="info-box"><b>Condição</b> <span>${data.health.toUpperCase()}</span></div>
                    <div class="info-box"><b>Género</b> <span>${data.gender.toUpperCase()}</span></div>
                    <div class="info-box"><b>Idade / Peso</b> <span>${data.age}a / ${data.weight}kg</span></div>
                    <div class="info-box"><b>Frequência</b> <span>${data.freq}x por semana</span></div>
                </div>
            `;

            const dias = data.freq;
            const isFem = data.gender === 'feminino';

            for (let i = 0; i < dias; i++) {
                const letra = ["A", "B", "C"][i % 3];
                const foco = getFocusName(letra, isFem, data.health);
                
                html += `
                    <div class="workout-block">
                        <div class="workout-title">
                            <span>TREINO ${letra}</span>
                            <span style="font-size: 0.7rem; opacity: 0.8;">FOCO: ${foco}</span>
                        </div>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 45%;">Exercício</th>
                                    <th style="width: 10%;">Séries</th>
                                    <th style="width: 15%;">Reps</th>
                                    <th style="width: 30%;">Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${getExercisesRows(letra, data)}
                            </tbody>
                        </table>
                    </div>
                `;
            }

            html += `
                <div class="signature">
                    <p style="font-size: 0.7rem; color: #666; margin-bottom: 5px;">Documento assinado digitalmente por</p>
                    <b>Lucas André</b>
                    <span>Personal Trainer & Biomecanista</span>
                </div>
            `;

            sheet.innerHTML = html;
        }

        function getFocusName(letra, isFem, health) {
            if (health === 'idoso' || health === 'gestante') return "Qualidade de Vida e Mobilidade";
            if (isFem) {
                if (letra === 'A') return "Quadríceps e Glúteo 🍑";
                if (letra === 'B') return "Superiores e Core";
                return "Posterior e Glúteo Máximo 🍑";
            } else {
                if (letra === 'A') return "Empurrar (Peito/Ombro/Tríceps)";
                if (letra === 'B') return "Puxar (Costas/Bíceps)";
                return "Membros Inferiores Completos";
            }
        }

        function getExercisesRows(letra, data) {
            let pool = [];
            const isFem = data.gender === 'feminino';

            if (isFem) {
                if (letra === 'A') pool = [...EXERCISES_DB.PERNAS.slice(0, 8), "Elevação pelvica"];
                else if (letra === 'B') pool = [...EXERCISES_DB.PEITO.slice(0,3), ...EXERCISES_DB.COSTAS.slice(0,3), ...EXERCISES_DB.OMBROS.slice(0,3), "Prancha"];
                else pool = [...EXERCISES_DB.PERNAS.slice(8), "Stiff", "Elevação pelvica", "Mesa Flexora"];
            } else {
                if (letra === 'A') pool = [...EXERCISES_DB.PEITO, ...EXERCISES_DB.OMBROS, ...EXERCISES_DB.TRICEPS];
                else if (letra === 'B') pool = [...EXERCISES_DB.COSTAS, ...EXERCISES_DB.BICEPS];
                else pool = [...EXERCISES_DB.PERNAS];
            }

            // Filtros de Lesão
            if (data.health === 'joelho') pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro", "Avanço", "Hack"].includes(e));
            if (data.health === 'lombar') pool = pool.filter(e => !["Levantamento terra", "Stiff", "Agachamento com Barra"].includes(e));
            if (data.health === 'idoso' || data.health === 'gestante') pool = EXERCISES_DB.FUNCIONAL;

            const selected = pool.sort(() => 0.5 - Math.random()).slice(0, 6);
            
            let s = "4", r = "12", o = "Descanso 60''";
            if (data.goal === 'Ganho de Força') { s = "5"; r = "5"; o = "Descanso 3'"; }
            if (data.health === 'hipertensao') { s = "3"; r = "20"; o = "Carga Leve / Sem Apneia"; }

            return selected.map(ex => `
                <tr>
                    <td style="font-weight: 700;">${ex}</td>
                    <td style="text-align: center;">${s}</td>
                    <td style="text-align: center;">${r}</td>
                    <td style="font-style: italic; color: #666; font-size: 0.75rem;">${o}</td>
                </tr>
            `).join('');
        }

        async function exportPDF() {
            const element = document.getElementById('printable-sheet');
            
            // Forçamos largura fixa e visibilidade para o PDF não cortar
            const opt = {
                margin: [10, 10, 10, 10],
                filename: `Ficha_PowFit_${document.getElementById('name').value.replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 1.0 },
                html2canvas: { 
                    scale: 3, 
                    useCORS: true, 
                    logging: false,
                    scrollY: 0,
                    scrollX: 0,
                    width: 794 // 210mm a 96dpi
                },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            try {
                // Pequena espera para garantir que o DOM está pronto para o motor
                await new Promise(resolve => setTimeout(resolve, 500));
                await html2pdf().set(opt).from(element).save();
            } catch (err) {
                console.error("Erro na geração:", err);
                alert("Houve um problema ao gerar o PDF. Tente usar o botão 'Imprimir' nativo.");
            }
        }
    </script>
</body>
</html>

