<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PowFit Pro | Prescrição Inteligente</title>
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
        body { font-family: 'Inter', sans-serif; background: var(--bg-body); color: var(--text-main); transition: all 0.5s ease; padding: 20px; }
        .container { max-width: 1000px; margin: 0 auto; }

        header { text-align: center; padding: 40px 0; border-bottom: 1px solid var(--border); margin-bottom: 40px; }
        header h1 { font-family: 'Playfair Display', serif; font-size: 3.2rem; color: var(--primary); font-weight: 900; }
        header p { text-transform: uppercase; letter-spacing: 4px; font-size: 0.7rem; color: var(--text-muted); margin-top: 10px; }

        .card { background: var(--bg-card); border: 1px solid var(--border); border-radius: 20px; padding: 30px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); margin-bottom: 30px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; }
        .form-group { margin-bottom: 20px; }
        label { display: block; font-size: 0.7rem; font-weight: 700; text-transform: uppercase; margin-bottom: 8px; color: var(--text-muted); }

        input, select {
            width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid var(--border);
            border-radius: 12px; color: var(--text-main); font-size: 0.95rem; outline: none; transition: 0.3s;
        }
        [data-theme="feminino"] input, [data-theme="feminino"] select { background: #fff; color: #18181b; }
        input:focus, select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(251, 191, 36, 0.1); }

        .btn {
            width: 100%; padding: 18px; background: var(--primary); color: #000; border: none; border-radius: 12px;
            font-weight: 900; text-transform: uppercase; cursor: pointer; transition: 0.3s; font-size: 0.9rem;
        }
        [data-theme="feminino"] .btn { color: #fff; }
        .btn:hover { transform: translateY(-2px); filter: brightness(1.1); }

        /* Estilo da Ficha - Otimizado para PDF */
        #preview-section { display: none; margin-top: 40px; }
        
        .pdf-container {
            background: #ffffff !important;
            width: 210mm;
            min-height: 297mm;
            margin: 0 auto;
            padding: 15mm;
            color: #000000 !important;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
            position: relative;
            z-index: 1;
        }

        .sheet-header { border-bottom: 3px solid #000; padding-bottom: 15px; margin-bottom: 25px; display: flex; justify-content: space-between; align-items: flex-end; }
        .sheet-header h2 { font-family: 'Playfair Display', serif; font-size: 2.2rem; color: #000 !important; }
        
        .user-info { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 25px; background: #f3f4f6; padding: 15px; border-radius: 6px; }
        .info-item b { display: block; font-size: 0.6rem; text-transform: uppercase; color: #4b5563; }
        .info-item span { font-weight: 800; font-size: 0.85rem; color: #000; }

        .workout-box { margin-bottom: 30px; page-break-inside: avoid; }
        .workout-title { background: #000; color: #fff !important; padding: 8px 15px; font-weight: 900; text-transform: uppercase; font-size: 1rem; margin-bottom: 8px; }
        
        table { width: 100%; border-collapse: collapse; margin-bottom: 5px; }
        th { background: #e5e7eb; text-align: left; padding: 8px; border: 1px solid #9ca3af; font-size: 0.65rem; text-transform: uppercase; color: #000; }
        td { padding: 10px 8px; border: 1px solid #9ca3af; font-size: 0.8rem; color: #000; }

        .signature-area { margin-top: 40px; text-align: right; border-top: 1px solid #d1d5db; padding-top: 15px; }
        .signature-area b { font-size: 1.1rem; display: block; color: #000; }

        .loading-screen {
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.95); z-index: 9999; flex-direction: column; justify-content: center; align-items: center;
        }
        .spinner { width: 50px; height: 50px; border: 4px solid #333; border-top-color: var(--primary); border-radius: 50%; animation: spin 1s infinite linear; }
        @keyframes spin { to { transform: rotate(360deg); } }

        @media (max-width: 850px) { .pdf-container { width: 100%; padding: 10mm; min-height: auto; } }
    </style>
</head>
<body>

    <div class="loading-screen" id="loading">
        <div class="spinner"></div>
        <p style="margin-top: 20px; font-weight: 700; color: #fff; letter-spacing: 2px;">CALIBRANDO BIOMECÂNICA...</p>
    </div>

    <div class="container">
        <header>
            <h1>PowFit Pro</h1>
            <p>Advanced Prescriptive Analytics</p>
        </header>

        <section class="card">
            <form id="mainForm">
                <div class="grid">
                    <div class="form-group">
                        <label>Nome do Atleta</label>
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
                        <label>Idade</label>
                        <input type="number" id="age" required>
                    </div>
                </div>

                <div class="grid">
                    <div class="form-group">
                        <label>Objetivo</label>
                        <select id="goal">
                            <option value="Hipertrofia">Hipertrofia</option>
                            <option value="Emagrecimento">Emagrecimento</option>
                            <option value="Definição">Definição Muscular</option>
                            <option value="Saúde Geral">Saúde Geral</option>
                            <option value="Ganho de Força">Ganho de Força</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Condição de Saúde</label>
                        <select id="health">
                            <option value="Saudável">Saudável</option>
                            <option value="Sedentário">Sedentário</option>
                            <option value="Hipertensão">Hipertensão / Cardíaco</option>
                            <option value="Lesão no Joelho">Lesão no Joelho</option>
                            <option value="Hérnia de Disco">Hérnia de Disco / Lombar</option>
                            <option value="Idoso">Idoso (Funcional)</option>
                            <option value="Gestante">Gestante / Lactante</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>Frequência</label>
                        <select id="freq">
                            <option value="3">3 Dias (Foco Total)</option>
                            <option value="6">6 Dias (Alta Performance)</option>
                        </select>
                    </div>
                </div>
                <button type="submit" class="btn">Gerar Ficha Profissional</button>
            </form>
        </section>

        <section id="preview-section">
            <div id="pdf-content" class="pdf-container">
                <!-- Conteúdo Injetado via JS -->
            </div>

            <div style="display: flex; gap: 15px; justify-content: center; margin: 30px 0;">
                <button class="btn" onclick="downloadPDF()" style="max-width: 250px;">Baixar PDF (A4)</button>
                <button class="btn" onclick="window.scrollTo({top:0, behavior:'smooth'})" style="max-width: 200px; background: #333; color: #fff;">Editar Dados</button>
            </div>
        </section>
    </div>

    <script>
        const DB = {
            QUADRICEPS: ["Agachamento Livre", "Leg-press", "Hack", "Smith", "Cadeira extensora", "Afundo", "Avanço", "Búlgaro"],
            GLUTEO_POSTERIOR: ["Elevação pelvica", "Stiff", "Mesa Flexora", "Cadeira Flexora", "Sumô", "Levantamento Terra Sumô", "Coice", "Cachorrinho", "Caranguejo", "Elevação de Perna lateral"],
            PEITO: ["Supino Reto", "Supino inclinado", "Suplino com Halteres", "Cross Over", "Pack Fly"],
            COSTAS: ["Puxada Alta", "Remada Baixa", "Puxada Unilateral", "Serrote", "Pooldowm"],
            BRACOS: ["Rosca direta", "Rosca Martelo", "Triceps puley", "Triceps na Corda", "Triceps Testa"],
            OMBROS: ["Desenvolvimento com halteres", "Elevação lateral", "Facepool", "Elevação Frontal"],
            FUNCIONAL: ["Prancha", "Mobilidade de Quadril", "Alongamento", "Caminhada", "Agachamento Global"]
        };

        document.getElementById('mainForm').addEventListener('submit', function(e) {
            e.preventDefault();
            document.getElementById('loading').style.display = 'flex';

            setTimeout(() => {
                const data = {
                    name: document.getElementById('name').value,
                    gender: document.getElementById('gender').value,
                    age: document.getElementById('age').value,
                    goal: document.getElementById('goal').value,
                    health: document.getElementById('health').value,
                    freq: parseInt(document.getElementById('freq').value)
                };

                generateWorkoutSheet(data);
                document.getElementById('loading').style.display = 'none';
                document.getElementById('preview-section').style.display = 'block';
                window.scrollTo({ top: document.getElementById('preview-section').offsetTop - 20, behavior: 'smooth' });
            }, 1000);
        });

        function generateWorkoutSheet(data) {
            const container = document.getElementById('pdf-content');
            let content = "";
            const isFeminino = data.gender === 'feminino';
            const numDias = data.freq;

            for (let i = 0; i < numDias; i++) {
                const diaLetra = ["A", "B", "C"][i % 3];
                content += `
                    <div class="workout-box">
                        <div class="workout-title">TREINO ${diaLetra} - ${getWorkoutFocus(diaLetra, data)}</div>
                        <table>
                            <thead>
                                <tr>
                                    <th style="width: 45%">Exercício</th>
                                    <th style="width: 15%">Séries</th>
                                    <th style="width: 15%">Reps</th>
                                    <th style="width: 25%">Obs. Técnica</th>
                                </tr>
                            </thead>
                            <tbody>
                                ${getExercises(diaLetra, data)}
                            </tbody>
                        </table>
                    </div>
                `;
            }

            container.innerHTML = `
                <div class="sheet-header">
                    <div>
                        <h2>POWFIT PRO</h2>
                        <p style="color: #555; font-size: 0.75rem; font-weight: 600;">CONSULTORIA DE TREINAMENTO ESPORTIVO</p>
                    </div>
                    <div style="text-align: right; font-size: 0.8rem; color: #444;">
                        EMISSÃO: ${new Date().toLocaleDateString('pt-br')}
                    </div>
                </div>

                <div class="user-info">
                    <div class="info-item"><b>Aluno(a)</b> <span>${data.name}</span></div>
                    <div class="info-item"><b>Objetivo</b> <span>${data.goal}</span></div>
                    <div class="info-item"><b>Condição</b> <span>${data.health}</span></div>
                    <div class="info-item"><b>Gênero</b> <span>${data.gender.toUpperCase()}</span></div>
                    <div class="info-item"><b>Idade</b> <span>${data.age} Anos</span></div>
                    <div class="info-item"><b>Frequência</b> <span>${data.freq}x Semana</span></div>
                </div>

                ${content}

                <div class="signature-area">
                    <p style="font-size: 0.7rem; color: #666; margin-bottom: 5px;">Prescrição assinada eletronicamente por</p>
                    <b>Lucas André</b>
                    <p style="font-size: 0.75rem;">Personal Trainer - CREF Elite</p>
                </div>
            `;
        }

        function getWorkoutFocus(letra, data) {
            if (data.gender === 'feminino') {
                if (letra === "A") return "Quadríceps & Glúteo 🍑";
                if (letra === "B") return "Superiores & Abdômen";
                return "Posterior & Glúteo Máximo 🍑";
            } else {
                if (letra === "A") return "Peito, Ombros e Tríceps";
                if (letra === "B") return "Costas e Bíceps";
                return "Membros Inferiores Completos";
            }
        }

        function getExercises(letra, data) {
            let pool = [];
            const isFem = data.gender === 'feminino';

            // Lógica de Periodização Feminina (Prioridade Inferiores)
            if (isFem) {
                if (letra === "A") pool = [...DB.QUADRICEPS.slice(0, 4), ...DB.GLUTEO_POSTERIOR.slice(0, 3)];
                else if (letra === "B") pool = [...DB.PEITO.slice(0, 2), ...DB.COSTAS.slice(0, 2), ...DB.OMBROS.slice(0, 2), "Prancha"];
                else pool = [...DB.GLUTEO_POSTERIOR.slice(3, 8), "Stiff", "Elevação pelvica"];
            } else {
                if (letra === "A") pool = [...DB.PEITO, ...DB.OMBROS, ...DB.BRACOS.slice(2)];
                else if (letra === "B") pool = [...DB.COSTAS, ...DB.BRACOS.slice(0, 2)];
                else pool = DB.QUADRICEPS.concat(DB.GLUTEO_POSTERIOR);
            }

            // Filtros de Saúde
            if (data.health === "Lesão no Joelho") pool = pool.filter(e => !["Agachamento Livre", "Afundo", "Búlgaro", "Avanço"].includes(e));
            if (data.health === "Hérnia de Disco") pool = pool.filter(e => !["Stiff", "Levantamento Terra Sumô", "Smith"].includes(e));
            if (data.health === "Idoso" || data.health === "Gestante") pool = DB.FUNCIONAL.concat(["Agachamento Sentar/Levantar"]);

            const final = pool.sort(() => 0.5 - Math.random()).slice(0, 6);
            
            let s = "3", r = "12-15", o = "60s Descanso";
            if (data.goal === "Hipertrofia") { s = "4"; r = "8-10"; o = "Carga Moderada"; }
            if (data.goal === "Ganho de Força") { s = "5"; r = "4-6"; o = "Intervalo 2-3min"; }

            return final.map(ex => `
                <tr>
                    <td>${ex}</td>
                    <td>${s}</td>
                    <td>${r}</td>
                    <td>${o}</td>
                </tr>
            `).join('');
        }

        async function downloadPDF() {
            const element = document.getElementById('pdf-content');
            
            // Força renderização visível para o motor de captura
            element.style.display = 'block';

            const opt = {
                margin: 0,
                filename: `Ficha_PowFit_${document.getElementById('name').value.replace(/\s+/g, '_')}.pdf`,
                image: { type: 'jpeg', quality: 1.0 },
                html2canvas: { 
                    scale: 3, 
                    useCORS: true, 
                    logging: false,
                    scrollY: 0,
                    scrollX: 0,
                    windowWidth: 1200 // Estabiliza o layout para captura
                },
                jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            try {
                // Pequeno delay para garantir que o navegador "pintou" o elemento no DOM
                await new Promise(resolve => setTimeout(resolve, 300));
                await html2pdf().set(opt).from(element).save();
            } catch (err) {
                console.error("Erro no PDF:", err);
                alert("Houve um problema ao gerar o PDF. Tente usar o botão Imprimir do navegador.");
            }
        }
    </script>
</body>
</html>

