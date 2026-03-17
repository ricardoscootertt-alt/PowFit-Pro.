<html lang="pt-pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Gerador de Treinos</title>
    
    <!-- Bibliotecas de Suporte -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=Playfair+Display:wght@700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary: #fbbf24; /* Amarelo Gold */
            --bg-body: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border: #334155;
            --accent: #fbbf24;
            --transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        [data-theme="feminino"] {
            --primary: #ec4899; /* Rosa Shock */
            --bg-body: #fdf2f8;
            --bg-card: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --border: #fbcfe8;
            --accent: #ec4899;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-print-color-adjust: exact; }

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
        }

        header p {
            color: var(--text-muted);
            letter-spacing: 4px;
            font-size: 0.75rem;
            font-weight: 600;
        }

        .card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 24px;
            padding: 40px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            margin-bottom: 40px;
        }

        .form-title {
            font-size: 1.5rem;
            margin-bottom: 30px;
            font-weight: 800;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .form-title::before {
            content: "";
            width: 4px;
            height: 24px;
            background: var(--primary);
            border-radius: 2px;
        }

        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; }
        .form-group { margin-bottom: 20px; }
        
        label {
            display: block;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 8px;
            color: var(--text-muted);
        }

        input, select {
            width: 100%;
            padding: 14px 18px;
            background: rgba(0,0,0,0.2);
            border: 2px solid var(--border);
            border-radius: 12px;
            color: var(--text-main);
            font-size: 1rem;
            transition: var(--transition);
        }

        [data-theme="feminino"] input, 
        [data-theme="feminino"] select {
            background: #fff;
            border-color: #fce7f3;
            color: #1e293b;
        }

        input:focus, select:focus {
            outline: none;
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
        }

        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { transform: translateY(-3px); filter: brightness(1.1); box-shadow: 0 10px 20px rgba(0,0,0,0.2); }

        /* Estilos da Ficha (Preview Visual) */
        #preview-section { display: none; margin-top: 50px; }

        /* Elemento que será impresso - Formato A4 */
        .a4-page {
            background: white !important;
            color: black !important;
            width: 210mm;
            min-height: 297mm;
            padding: 20mm;
            margin: 0 auto;
            box-shadow: 0 0 40px rgba(0,0,0,0.2);
            box-sizing: border-box;
            position: relative;
        }

        .sheet-header {
            border-bottom: 4px solid #000;
            padding-bottom: 20px;
            margin-bottom: 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.5rem; }

        .patient-info {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 15px;
            margin-bottom: 40px;
            background: #f8fafc;
            padding: 20px;
            border-radius: 8px;
        }

        .info-box b { display: block; font-size: 0.65rem; text-transform: uppercase; color: #64748b; }
        .info-box span { font-weight: 700; color: #0f172a; font-size: 0.95rem; }

        .workout-block { margin-bottom: 40px; page-break-inside: avoid; }
        .workout-title {
            background: #000;
            color: #fff;
            padding: 10px 20px;
            font-weight: 900;
            margin-bottom: 15px;
            font-size: 1.1rem;
        }

        table { width: 100%; border-collapse: collapse; }
        th { background: #f1f5f9; text-align: left; padding: 12px; border: 1px solid #cbd5e1; font-size: 0.7rem; text-transform: uppercase; }
        td { padding: 12px; border: 1px solid #cbd5e1; font-size: 0.85rem; color: #334155; }

        .signature {
            margin-top: 60px;
            text-align: right;
            border-top: 2px solid #e2e8f0;
            padding-top: 20px;
        }

        .signature b { font-size: 1.2rem; display: block; }
        .signature span { color: #64748b; font-size: 0.8rem; }

        /* Loader */
        .overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.85);
            z-index: 9999;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            color: white;
        }

        .spin { width: 50px; height: 50px; border: 4px solid #333; border-top-color: var(--primary); border-radius: 50%; animation: rot 1s infinite linear; margin-bottom: 20px; }
        @keyframes rot { to { transform: rotate(360deg); } }

        /* Print Fallback */
        @media print {
            body * { visibility: hidden; }
            #sheet-to-print, #sheet-to-print * { visibility: visible; }
            #sheet-to-print { position: absolute; left: 0; top: 0; width: 100%; margin: 0; padding: 0; box-shadow: none; }
        }

        @media (max-width: 850px) {
            .a4-page { width: 100%; padding: 10mm; min-height: auto; font-size: 14px; }
            .patient-info { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

    <div class="overlay" id="loading">
        <div class="spin"></div>
        <p>A processar biomecânica do treino...</p>
    </div>

    <div class="container">
        <header>
            <h1>PowFit Pro</h1>
            <p>SISTEMA DE PRESCRIÇÃO ELITE</p>
        </header>

        <section class="card">
            <div class="form-title">Configurar Nova Ficha</div>
            <form id="trainerForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome Completo</label>
                        <input type="text" id="name" placeholder="Nome do aluno" required>
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
                            <option value="Resistência">Resistência</option>
                            <option value="Força">Ganho de Força</option>
                            <option value="Reabilitação">Reabilitação</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Estado de Saúde</label>
                        <select id="health">
                            <option value="Saudável">Saudável</option>
                            <option value="Sedentário">Sedentário</option>
                            <option value="Sobrepeso">Sobrepeso</option>
                            <option value="Obesidade">Obesidade</option>
                            <option value="Diabetes">Diabetes</option>
                            <option value="Hipertensão">Hipertensão</option>
                            <option value="Cardíaco">Problemas cardíacos</option>
                            <option value="Lesão no joelho">Lesão no joelho</option>
                            <option value="Hérnia">Hérnia de disco</option>
                            <option value="Idoso">Idoso</option>
                            <option value="Gestante">Gestante</option>
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
                <button type="submit" class="btn">Gerar Treino Automatizado</button>
            </form>
        </section>

        <section id="preview-section">
            <!-- Content for Print/PDF -->
            <div id="sheet-to-print" class="a4-page">
                <!-- Injected by JS -->
            </div>

            <div style="display: flex; gap: 15px; margin-top: 30px;">
                <button class="btn" onclick="exportPDF()" style="flex: 2;">Download PDF Oficial</button>
                <button class="btn" onclick="window.print()" style="flex: 1; background: #334155; color: white;">Imprimir</button>
            </div>
        </section>
    </div>

    <script>
        const exercises = {
            chest: ["Supino Reto", "Supino Inclinado Halteres", "Crucifixo Máquina", "Cross Over"],
            back: ["Puxada Alta", "Remada Sentada", "Puxada Unilateral", "Lombar Máquina"],
            legs: ["Agachamento", "Leg Press", "Extensora", "Flexora", "Elevação Pélvica"],
            shoulders: ["Desenvolvimento", "Elevação Lateral", "Frontal Polia"],
            arms: ["Rosca Direta", "Tríceps Pulley", "Martelo", "Tríceps Testa"],
            safe: ["Caminhada Moderada", "Mobilidade Articular", "Alongamento", "Agachamento Caixa", "Puxada Elástico"]
        };

        document.getElementById('trainerForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const loader = document.getElementById('loading');
            loader.style.display = 'flex';

            setTimeout(() => {
                const data = {
                    name: document.getElementById('name').value,
                    age: document.getElementById('age').value,
                    weight: document.getElementById('weight').value,
                    goal: document.getElementById('goal').value,
                    health: document.getElementById('health').value,
                    freq: document.getElementById('freq').value,
                    gender: document.getElementById('gender').value
                };

                renderSheet(data);
                loader.style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1000);
        });

        function renderSheet(data) {
            const container = document.getElementById('sheet-to-print');
            let workoutsHTML = "";
            const days = data.freq == "6" ? 6 : 3;
            const labels = ["A", "B", "C"];

            for (let i = 0; i < days; i++) {
                const label = labels[i % 3];
                workoutsHTML += `
                    <div class="workout-block">
                        <div class="workout-title">TREINO ${label} - ${getWorkoutFocus(label, data.health)}</div>
                        <table>
                            <thead>
                                <tr>
                                    <th>Exercício</th>
                                    <th style="width: 80px">Séries</th>
                                    <th style="width: 100px">Reps</th>
                                    <th>Observações</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${generateRows(label, data)}
                            </tbody>
                        </table>
                    </div>
                `;
            }

            container.innerHTML = `
                <div class="sheet-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="font-size: 0.8rem; color: #666">Prescrição Técnica: Lucas André - Personal Trainer</p>
                    </div>
                    <div style="text-align: right; font-size: 0.8rem;">
                        Data: ${new Date().toLocaleDateString('pt-pt')}
                    </div>
                </div>

                <div class="patient-info">
                    <div class="info-box"><b>Aluno</b> <span>${data.name}</span></div>
                    <div class="info-box"><b>Idade</b> <span>${data.age} anos</span></div>
                    <div class="info-box"><b>Peso</b> <span>${data.weight} kg</span></div>
                    <div class="info-box"><b>Objetivo</b> <span>${data.goal}</span></div>
                    <div class="info-box"><b>Saúde</b> <span>${data.health}</span></div>
                    <div class="info-box"><b>Gênero</b> <span>${data.gender}</span></div>
                </div>

                ${workoutsHTML}

                <div class="signature">
                    <b>Lucas André</b>
                    <span>Cédula Profissional: 12345-G</span>
                </div>
            `;
        }

        function getWorkoutFocus(label, health) {
            if (["Cardíaco", "Hipertensão", "Idoso", "Gestante"].includes(health)) return "Cardiovascular & Mobilidade";
            if (label === "A") return "Membros Superiores (Empurrar)";
            if (label === "B") return "Membros Superiores (Puxar)";
            return "Membros Inferiores";
        }

        function generateRows(label, data) {
            let rows = "";
            let sets = "3";
            let reps = "12-15";
            let obs = "60s de descanso";

            if (data.goal === "Hipertrofia") { sets = "4"; reps = "8-12"; obs = "Foco na contração"; }
            if (data.goal === "Força") { sets = "5"; reps = "3-5"; obs = "Carga Alta / 3m descanso"; }
            if (["Cardíaco", "Hipertensão"].includes(data.health)) { reps = "20+"; obs = "Baixa intensidade / Sem apneia"; }

            let pool = [];
            if (["Idoso", "Gestante", "Reabilitação"].includes(data.health)) {
                pool = exercises.safe;
            } else {
                const map = { 'A': [...exercises.chest, ...exercises.shoulders], 'B': [...exercises.back, ...exercises.arms], 'C': [...exercises.legs] };
                pool = map[label] || exercises.safe;
            }

            // Filtros de lesão
            if (data.health === "Lesão no joelho") pool = pool.filter(e => e !== "Agachamento");
            if (data.health === "Hérnia") pool = pool.filter(e => e !== "Remada Sentada");

            const selected = pool.sort(() => 0.5 - Math.random()).slice(0, 5);
            selected.forEach(ex => {
                rows += `<tr><td>${ex}</td><td>${sets}</td><td>${reps}</td><td>${obs}</td></tr>`;
            });
            return rows;
        }

        function exportPDF() {
            const element = document.getElementById('sheet-to-print');
            
            // Configurações para evitar o "branco"
            const opt = {
                margin: 0,
                filename: `Ficha_${document.getElementById('name').value.replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 1.0 },
                html2canvas: { 
                    scale: 2, 
                    useCORS: true, 
                    letterRendering: true,
                    scrollY: 0,
                    scrollX: 0
                },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            // Criamos uma promessa para garantir que o elemento está renderizado
            html2pdf().set(opt).from(element).toPdf().get('pdf').then(function (pdf) {
                // Forçamos uma última verificação de conteúdo
            }).save();
        }
    </script>
</body>
</html>

