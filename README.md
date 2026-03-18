<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Gestão de Treinos</title>
    <!-- Bibliotecas -->
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
            --accent: #fbbf24;
        }

        [data-theme="feminino"] {
            --primary: #f472b6;
            --bg-body: #fff1f2;
            --bg-card: #ffffff;
            --text-main: #18181b;
            --text-muted: #71717a;
            --border: #fbcfe8;
            --accent: #f472b6;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: var(--bg-body); color: var(--text-main); transition: all 0.4s ease; padding: 20px; }
        .container { max-width: 1000px; margin: 0 auto; }

        header { text-align: center; padding: 50px 0; border-bottom: 1px solid var(--border); margin-bottom: 40px; }
        header h1 { font-family: 'Playfair Display', serif; font-size: 3.5rem; color: var(--primary); font-weight: 900; }
        header p { text-transform: uppercase; letter-spacing: 5px; font-size: 0.7rem; color: var(--text-muted); margin-top: 10px; }

        .card { background: var(--bg-card); border: 1px solid var(--border); border-radius: 20px; padding: 40px; box-shadow: 0 20px 40px rgba(0,0,0,0.4); margin-bottom: 30px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; }
        .form-group { margin-bottom: 25px; }
        label { display: block; font-size: 0.75rem; font-weight: 700; text-transform: uppercase; margin-bottom: 10px; color: var(--text-muted); }

        input, select {
            width: 100%; padding: 15px; background: rgba(255,255,255,0.05); border: 1px solid var(--border);
            border-radius: 12px; color: var(--text-main); font-size: 1rem; outline: none; transition: 0.3s;
        }
        [data-theme="feminino"] input, [data-theme="feminino"] select { background: #fff; color: #18181b; }
        input:focus, select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(251, 191, 36, 0.2); }

        .btn {
            width: 100%; padding: 20px; background: var(--primary); color: #000; border: none; border-radius: 12px;
            font-weight: 900; text-transform: uppercase; cursor: pointer; transition: 0.3s; font-size: 1rem;
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { transform: translateY(-3px); filter: brightness(1.1); box-shadow: 0 10px 20px rgba(0,0,0,0.2); }

        /* Estilo da Ficha Técnica - A4 */
        #print-area { display: none; }
        .sheet {
            background: #fff !important; color: #000 !important; width: 210mm; min-height: 297mm;
            padding: 20mm; margin: 0 auto; box-sizing: border-box; position: relative;
        }
        .sheet-header { border-bottom: 4px solid #000; padding-bottom: 20px; margin-bottom: 30px; display: flex; justify-content: space-between; align-items: flex-end; }
        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.5rem; margin: 0; }
        
        .user-data { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-bottom: 30px; background: #f4f4f5; padding: 20px; border-radius: 8px; }
        .data-box b { display: block; font-size: 0.6rem; text-transform: uppercase; color: #71717a; }
        .data-box span { font-weight: 800; font-size: 0.95rem; }

        .workout-block { margin-bottom: 40px; page-break-inside: avoid; }
        .workout-title { background: #000; color: #fff; padding: 10px 15px; font-weight: 900; text-transform: uppercase; margin-bottom: 10px; }
        
        table { width: 100%; border-collapse: collapse; margin-bottom: 10px; }
        th { background: #e4e4e7; text-align: left; padding: 10px; border: 1px solid #d4d4d8; font-size: 0.7rem; text-transform: uppercase; }
        td { padding: 12px 10px; border: 1px solid #d4d4d8; font-size: 0.85rem; }

        .signature { margin-top: 50px; text-align: right; border-top: 2px solid #e4e4e7; padding-top: 20px; }
        .signature b { font-size: 1.2rem; display: block; }

        #preview-section { display: none; margin-top: 50px; text-align: center; }

        /* Loader */
        .loader {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 10000; flex-direction: column; justify-content: center; align-items: center;
        }
        .spin { width: 50px; height: 50px; border: 5px solid #333; border-top-color: var(--primary); border-radius: 50%; animation: s 1s infinite linear; }
        @keyframes s { to { transform: rotate(360deg); } }

        @media (max-width: 800px) { .sheet { width: 100%; padding: 10px; } }
    </style>
</head>
<body>

    <div class="loader" id="loader">
        <div class="spin"></div>
        <p style="margin-top: 20px; font-weight: 700; color: #fff;">Sincronizando Biomecânica...</p>
    </div>

    <div class="container">
        <header>
            <h1>PowFit Pro</h1>
            <p>High Performance Training Systems</p>
        </header>

        <section class="card">
            <form id="workoutForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome do Atleta</label>
                        <input type="text" id="name" placeholder="Ex: João Silva" required>
                    </div>
                    <div class="form-group">
                        <label>Idade</label>
                        <input type="number" id="age" required>
                    </div>
                    <div class="form-group">
                        <label>Peso (kg)</label>
                        <input type="number" id="weight" required>
                    </div>
                    <div class="form-group">
                        <label>Gênero</label>
                        <select id="gender" onchange="document.body.setAttribute('data-theme', this.value)">
                            <option value="masculino">Masculino</option>
                            <option value="feminino">Feminino</option>
                        </select>
                    </div>
                </div>

                <div class="grid">
                    <div class="form-group">
                        <label>Objetivo</label>
                        <select id="goal">
                            <option value="Hipertrofia">Hipertrofia</option>
                            <option value="Emagrecimento">Emagrecimento</option>
                            <option value="Definição">Definição Muscular</option>
                            <option value="Condicionamento">Condicionamento Físico</option>
                            <option value="Resistência">Resistência Muscular</option>
                            <option value="Força">Ganho de Força</option>
                            <option value="Reabilitação">Reabilitação</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Estado de Saúde</label>
                        <select id="health">
                            <option value="Saudável">Saudável</option>
                            <option value="Sedentário">Sedentário</option>
                            <option value="Sobrepeso">Sobrepeso / Obesidade</option>
                            <option value="Hipertensão">Hipertensão / Cardíaco</option>
                            <option value="Diabetes">Diabetes / Pré-diabetes</option>
                            <option value="Lesão no joelho">Lesão no Joelho</option>
                            <option value="Lesão lombar">Lesão Lombar / Hérnia</option>
                            <option value="Idoso">Idoso (Mobilidade)</option>
                            <option value="Gestante">Gestante / Lactante</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência</label>
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
            <div id="capture-zone">
                <div class="sheet" id="sheet-content">
                    <!-- Gerado pelo JS -->
                </div>
            </div>

            <div style="display: flex; gap: 20px; justify-content: center; margin-top: 30px;">
                <button class="btn" style="max-width: 300px;" onclick="downloadPDF()">Download Ficha PDF</button>
                <button class="btn" style="max-width: 200px; background: #3f3f46; color: #fff;" onclick="window.scrollTo({top:0, behavior:'smooth'})">Novo Treino</button>
            </div>
        </section>
    </div>

    <script>
        const EXERCICIOS = {
            PERNAS: ["Leg-press", "Hack", "Squat", "Smith", "Agachamento Livre", "Agachamento com Barra", "Mesa Flexora", "Cadeira Flexora", "Cadeira extensora", "Adução", "Abdução", "Stiff", "Sumô", "Levantamento terra", "Levantamento Terra Sumô", "Elevação de Perna lateral", "Coice", "Cachorrinho", "Caranguejo", "Elevação pelvica", "Avanço", "Afundo", "Búlgaro"],
            PEITO: ["Supino Reto", "Supino inclinado", "Suplino com Halteres", "Cross Over", "Crucifixo inclinado", "Pack Fly", "Poollover"],
            COSTAS: ["Puxada Alta", "Remada Baixa", "Puxada Unilateral", "Serrote", "Pooldowm"],
            BICEPS: ["Rosca Scot com barra", "Rosca scot com halteres", "Rosca scot unilateral", "Rosca direta", "Rosca 21", "Rosca Alternada", "Rosca no cross", "Rosca Martelo"],
            TRICEPS: ["Triceps puley", "Triceps na Corda", "Triceps Coice", "Triceps Testa", "Triceps Francês"],
            OMBROS: ["Elevação Frontal", "Desenvolvimento com halteres", "Desenvolvimento Arnold", "Elevação lateral", "Elevação borboleta", "Facepool"],
            FUNCIONAL: ["Alongamento Ativo", "Mobilidade de Quadril", "Caminhada Inclinada", "Prancha", "Equilíbrio"]
        };

        document.getElementById('workoutForm').addEventListener('submit', function(e) {
            e.preventDefault();
            document.getElementById('loader').style.display = 'flex';

            setTimeout(() => {
                const data = {
                    name: document.getElementById('name').value,
                    age: document.getElementById('age').value,
                    weight: document.getElementById('weight').value,
                    goal: document.getElementById('goal').value,
                    health: document.getElementById('health').value,
                    freq: parseInt(document.getElementById('freq').value)
                };

                buildSheet(data);
                document.getElementById('loader').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1200);
        });

        function buildSheet(data) {
            const sheet = document.getElementById('sheet-content');
            let workoutsHTML = "";
            const totalWorkouts = data.freq;
            const labels = ["A", "B", "C"];

            for (let i = 0; i < totalWorkouts; i++) {
                const currentLabel = labels[i % 3];
                workoutsHTML += `
                    <div class="workout-block">
                        <div class="workout-title">TREINO ${currentLabel} - ${getFocus(currentLabel, data.health)}</div>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 40%">Exercício</th>
                                    <th style="width: 15%">Séries</th>
                                    <th style="width: 15%">Reps</th>
                                    <th style="width: 30%">Obs. Personalizadas</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${generateRows(currentLabel, data)}
                            </tbody>
                        </table>
                    </div>
                `;
            }

            sheet.innerHTML = `
                <div class="sheet-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="margin:0; font-size: 0.7rem; color: #555;">Prescrição de Treinamento Automatizada</p>
                    </div>
                    <div style="text-align: right; font-size: 0.75rem;">
                        Data: ${new Date().toLocaleDateString('pt-br')}
                    </div>
                </div>

                <div class="user-data">
                    <div class="data-box"><b>Atleta</b> <span>${data.name}</span></div>
                    <div class="data-box"><b>Idade</b> <span>${data.age} Anos</span></div>
                    <div class="data-box"><b>Peso</b> <span>${data.weight} kg</span></div>
                    <div class="data-box"><b>Objetivo</b> <span>${data.goal}</span></div>
                    <div class="data-box"><b>Condição</b> <span>${data.health}</span></div>
                    <div class="data-box"><b>Frequência</b> <span>${data.freq} Dias</span></div>
                </div>

                ${workoutsHTML}

                <div class="signature">
                    <p style="font-size: 0.7rem; color: #777;">Prescrição validada biomecanicamente por</p>
                    <b>Lucas André</b>
                    <p style="font-size: 0.75rem;">Personal Trainer - CREF Elite</p>
                </div>
            `;
        }

        function getFocus(label, health) {
            if (["Idoso", "Gestante", "Hipertensão"].includes(health)) return "Saúde & Funcionalidade";
            const map = { 'A': 'Peitorais, Ombros e Tríceps', 'B': 'Costas e Bíceps', 'C': 'Membros Inferiores Completos' };
            return map[label];
        }

        function generateRows(label, data) {
            let sets = "3", reps = "12-15", obs = "Intervalo 60s";
            
            // Lógica de Adaptação
            if (data.goal === "Hipertrofia") { sets = "4"; reps = "8-12"; obs = "Carga Moderada"; }
            if (data.goal === "Força") { sets = "5"; reps = "3-5"; obs = "Carga Alta / 3m Descanso"; }
            if (["Hipertensão", "Cardíaco"].includes(data.health)) { reps = "20"; obs = "Intensidade Leve / Sem Apneia"; }

            let pool = [];
            if (["Idoso", "Gestante", "Reabilitação"].includes(data.health)) {
                pool = EXERCICIOS.FUNCIONAL;
            } else {
                const map = {
                    'A': [...EXERCICIOS.PEITO, ...EXERCICIOS.OMBROS, ...EXERCICIOS.TRICEPS],
                    'B': [...EXERCICIOS.COSTAS, ...EXERCICIOS.BICEPS],
                    'C': [...EXERCICIOS.PERNAS]
                };
                pool = map[label];
            }

            // Filtros de Segurança
            if (data.health === "Lesão no joelho") {
                pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro", "Avanço", "Hack"].includes(e));
            }
            if (data.health === "Lesão lombar") {
                pool = pool.filter(e => !["Levantamento terra", "Agachamento com Barra", "Stiff"].includes(e));
            }

            const shuffled = pool.sort(() => 0.5 - Math.random()).slice(0, 6);
            return shuffled.map(ex => `<tr><td>${ex}</td><td>${sets}</td><td>${reps}</td><td>${obs}</td></tr>`).join('');
        }

        async function downloadPDF() {
            const element = document.getElementById('sheet-content');
            
            // Garantir que o elemento esteja visível no DOM mas formatado para PDF
            const opt = {
                margin: 0,
                filename: `Treino_${document.getElementById('name').value.replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 1.0 },
                html2canvas: { 
                    scale: 3, 
                    useCORS: true, 
                    logging: false,
                    scrollY: 0,
                    scrollX: 0
                },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            try {
                // Força o fundo branco e visibilidade antes de capturar
                element.style.display = 'block';
                await html2pdf().set(opt).from(element).save();
            } catch (err) {
                console.error(err);
                alert("Erro ao gerar PDF. Verifique as permissões do navegador.");
            }
        }
    </script>
</body>
</html>

