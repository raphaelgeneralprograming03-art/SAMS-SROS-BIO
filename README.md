<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SIMULADOR DE ENGENHARIA MULTIMEIOS UNIFICADO - S.A.M.S. / SROS / BIOSSEGURANÇA</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        hudBg: '#050811',
                        hudPanel: '#0b1120',
                        hudBorder: '#1e293b',
                        hudCyan: '#00f0ff',
                        hudOrange: '#ffaa00',
                        hudGreen: '#00ff66',
                        hudRed: '#ff3366',
                        hudPurple: '#a855f7'
                    },
                    fontFamily: {
                        mono: ['Courier New', 'Monaco', 'monospace'],
                        sans: ['Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #03060d;
            color: #f1f5f9;
            font-family: 'Inter', sans-serif;
            overflow: hidden;
        }
        canvas {
            display: block;
        }
        .hud-border-cyan {
            border-color: #00f0ff;
            box-shadow: 0 0 10px rgba(0, 240, 255, 0.25);
        }
        .hud-glow-text {
            text-shadow: 0 0 8px rgba(0, 240, 255, 0.6);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 5px;
            height: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #090d14;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00f0ff;
        }
    </style>
</head>
<body class="h-screen w-screen flex flex-col font-mono bg-hudBg select-none overflow-hidden">

    <!-- TOP CONTROL & MODULE SELECTION BAR -->
    <header class="h-16 bg-hudPanel border-b border-hudBorder flex items-center justify-between px-4 z-30 shrink-0">
        <div class="flex items-center space-x-3">
            <div class="w-3 h-3 rounded-full bg-hudCyan animate-ping"></div>
            <div>
                <h1 class="text-sm font-extrabold text-hudCyan tracking-wider hud-glow-text">S.A.M.S. / SROS / BIO-ESCUDO</h1>
                <p class="text-[10px] text-slate-400">TRL 3-7 • Inventor: Rafael Barbosa da Silva</p>
            </div>
        </div>

        <!-- DROPDOWN & FAST SELECT MODULES -->
        <div class="flex items-center space-x-2">
            <label for="module-select" class="text-xs text-slate-400 font-bold hidden md:inline-block">CENÁRIO OPERACIONAL:</label>
            <select id="module-select" onchange="selectModule(parseInt(this.value))" class="bg-slate-900 border border-hudCyan text-hudCyan text-xs rounded px-3 py-1.5 focus:outline-none focus:ring-1 focus:ring-hudCyan font-bold cursor-pointer">
                <option value="0">1. Sonda Espacial (Espaço Profundo)</option>
                <option value="1">2. Satélite Orbital LEO (Anti-Poeira / Debris)</option>
                <option value="2">3. Submarino Mar Profundo (600 bar / 6000m)</option>
                <option value="3">4. Navio de Guerra (Fragata / Corveta)</option>
                <option value="4">5. Caça Aéreo (Supersônico / Heat-Pipes)</option>
                <option value="5">6. Míssil Hipersônico (Mach 20 / Ogiva PZT)</option>
                <option value="6">7. Tanque & Blindado Presidencial (Anti-Mina)</option>
                <option value="7">8. Bunker Subterrâneo (Resiliência Tectônica)</option>
            </select>
        </div>

        <!-- SYSTEM TELEMETRY HIGHLIGHTS -->
        <div class="hidden lg:flex items-center space-x-6 text-xs border-l border-hudBorder pl-6">
            <div>
                <span class="text-slate-400 text-[9px] block">ATENUAÇÃO DE TENSÃO</span>
                <span id="header-sams-eff" class="font-bold text-hudCyan text-sm">94.2%</span>
            </div>
            <div>
                <span class="text-slate-400 text-[9px] block">FLUIDO MR (RESPOSTA)</span>
                <span id="header-mr-time" class="font-bold text-hudOrange text-sm">1.18 ms</span>
            </div>
            <div>
                <span class="text-slate-400 text-[9px] block">REDE ÓPTICA SROS</span>
                <span class="font-bold text-hudGreen text-sm">750 CANAIS (ATIVO)</span>
            </div>
        </div>
    </header>

    <!-- MAIN CONTENT: LEFT CONTROLS, CENTER CANVAS, RIGHT AGENDA -->
    <div class="flex-1 flex flex-col lg:flex-row overflow-hidden relative">

        <!-- LEFT PANEL: CONTROLS & THREAT INJECTION -->
        <aside class="w-full lg:w-80 bg-hudPanel/90 border-r border-hudBorder flex flex-col justify-between p-4 z-20 space-y-4 overflow-y-auto shrink-0">
            
            <!-- PARAMETER SLIDERS -->
            <div class="space-y-3">
                <h2 class="text-xs font-bold text-hudCyan uppercase tracking-wider border-b border-hudBorder pb-1 flex justify-between items-center">
                    <span>Parâmetros Ativos</span>
                    <span class="text-[9px] text-slate-400">Controle Síncrono</span>
                </h2>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-slate-300">Campo Magnético (B):</span>
                        <span id="val-mag" class="text-hudOrange font-bold">1.2 T</span>
                    </div>
                    <input type="range" id="slider-mag" min="0.0" max="1.5" step="0.1" value="1.2" oninput="updateParameters()" class="w-full accent-hudOrange bg-slate-800 rounded">
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-slate-300">Tensão Piezoelétrica (V_pzt):</span>
                        <span id="val-pzt" class="text-hudCyan font-bold">600 V</span>
                    </div>
                    <input type="range" id="slider-pzt" min="0" max="1000" step="25" value="600" oninput="updateParameters()" class="w-full accent-hudCyan bg-slate-800 rounded">
                </div>

                <div>
                    <div class="flex justify-between text-[11px] mb-1">
                        <span class="text-slate-300">Intensidade Ambiental:</span>
                        <span id="val-env" class="text-hudGreen font-bold">Estável</span>
                    </div>
                    <input type="range" id="slider-env" min="1" max="100" step="1" value="50" oninput="updateParameters()" class="w-full accent-hudGreen bg-slate-800 rounded">
                </div>
            </div>

            <!-- THREAT TRIGGER BUTTONS -->
            <div class="space-y-2">
                <h2 class="text-xs font-bold text-hudOrange uppercase tracking-wider border-b border-hudBorder pb-1">Injeção de Ameaças em Tempo Real</h2>
                <div class="grid grid-cols-2 gap-2 text-[10px]">
                    <button onclick="triggerThreat('impact')" class="p-2 bg-red-950/40 hover:bg-red-900/60 border border-hudRed/60 text-red-300 rounded font-bold transition flex items-center justify-center gap-1">
                        💥 Impacto/Meteoro
                    </button>
                    <button onclick="triggerThreat('emp')" class="p-2 bg-amber-950/40 hover:bg-amber-900/60 border border-amber-500/60 text-amber-300 rounded font-bold transition flex items-center justify-center gap-1">
                        ⚡ Jamming / EMP
                    </button>
                    <button onclick="triggerThreat('pressure')" class="p-2 bg-blue-950/40 hover:bg-blue-900/60 border border-blue-500/60 text-blue-300 rounded font-bold transition flex items-center justify-center gap-1">
                        🌊 Pressão / Sismo
                    </button>
                    <button onclick="triggerThreat('bio')" class="p-2 bg-emerald-950/40 hover:bg-emerald-900/60 border border-emerald-500/60 text-emerald-300 rounded font-bold transition flex items-center justify-center gap-1">
                        ☣️ Vírus CHONPS
                    </button>
                </div>
            </div>

            <!-- REAL-TIME TELEMETRY GRAPH (INTEGRATED CANVAS) -->
            <div class="space-y-1">
                <h2 class="text-[11px] font-bold text-slate-300 uppercase tracking-wider">Atenuação de Tensão (S.A.M.S.)</h2>
                <div class="h-24 w-full bg-slate-950 border border-hudBorder rounded overflow-hidden relative">
                    <canvas id="graphCanvas" class="w-full h-full"></canvas>
                </div>
                <div class="flex justify-between text-[9px] text-slate-400">
                    <span class="text-hudRed">— Tensão Passiva</span>
                    <span class="text-hudGreen">— S.A.M.S. Atenuado (94.2%)</span>
                </div>
            </div>

            <!-- LOG STATUS DISPLAY -->
            <div class="bg-slate-950 border border-hudBorder rounded p-2 text-[9.5px] h-24 overflow-y-auto space-y-1" id="event-log">
                <div class="text-slate-500 font-bold">[ SISTEMA INICIALIZADO - NBR/ISO OK ]</div>
            </div>
        </aside>

        <!-- CENTER PANEL: 2D MAIN SIMULATION CANVAS -->
        <main class="flex-1 relative bg-slate-950 overflow-hidden flex items-center justify-center">
            <canvas id="mainCanvas" class="w-full h-full"></canvas>

            <!-- CANVAS OVERLAY INFORMATIONAL HUD -->
            <div class="absolute top-4 left-4 pointer-events-none bg-slate-900/80 border border-hudBorder p-3 rounded text-xs space-y-1 backdrop-blur-md">
                <div class="text-hudCyan font-bold tracking-wider" id="hud-module-title">1. SONDA ESPACIAL</div>
                <div class="text-slate-300 text-[11px]" id="hud-module-env">Espaço Profundo / Vácuo</div>
                <div class="text-slate-400 text-[10px]" id="hud-module-status">Escudo S.A.M.S. Ativo • Transmissão SROS Óptica Contínua</div>
            </div>

            <!-- LAYER LEGEND OVERLAY -->
            <div class="absolute bottom-4 right-4 pointer-events-none bg-slate-900/80 border border-hudBorder p-3 rounded text-[10px] space-y-1 backdrop-blur-md hidden sm:block">
                <div class="font-bold text-slate-200 border-b border-slate-700 pb-1">Casco Multicamadas S.A.M.S.</div>
                <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 rounded-full bg-slate-300"></span> 1. Titânio Grade 5 (Estrutural)</div>
                <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 rounded-full bg-hudCyan"></span> 2. Fibra Óptica BOTDA (Sensorial)</div>
                <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 rounded-full bg-hudOrange"></span> 3. Fluido MR (Viscoelástico Reversível)</div>
                <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 rounded-full bg-hudPurple"></span> 4. Atuadores PZT-8 (Contra-Força)</div>
                <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 rounded-full bg-slate-600"></span> 5. Carbono T800 (Habitáculo Estanque)</div>
            </div>
        </main>

        <!-- RIGHT PANEL: COMPONENT AGENDA NBR/ISO -->
        <aside class="w-full lg:w-72 bg-hudPanel/90 border-l border-hudBorder p-4 flex flex-col space-y-3 z-20 shrink-0 overflow-y-auto">
            <h2 class="text-xs font-bold text-hudCyan uppercase tracking-wider border-b border-hudBorder pb-1 flex justify-between items-center">
                <span>Agenda NBR / ISO</span>
                <span class="text-[9px] text-slate-400">Especificações</span>
            </h2>
            <div id="agenda-container" class="space-y-2 text-[10.5px] text-slate-300 flex-1">
                <!-- Dynamically Injected -->
            </div>
        </aside>

    </div>

    <!-- JAVASCRIPT LOGIC ENGINE -->
    <script>
        // MÓDULOS DE DADOS E ESPECIFICAÇÕES DOS 8 CENÁRIOS
        const modulesData = [
            {
                id: 0,
                name: "Sonda Espacial (Espaço Profundo)",
                env: "Espaço Profundo / Vácuo",
                status: "Escudo S.A.M.S. Ativo • Transmissão SROS Óptica Contínua",
                agenda: [
                    "1. Titânio Ti-6Al-4V Grade 5: Resistência primária no vácuo (-150°C a +450°C).",
                    "2. Fibra Óptica BOTDA/IR: Mapeamento tensional contínuo por espalhamento Brillouin (0.1ms).",
                    "3. Fluido MRF-140CG: Transição viscoelástica sob campo B=1.2T em 1.18ms.",
                    "4. Atuadores PZT-8 Stacked: Compensação piezoelétrica de 15 kN/cm².",
                    "5. Carbono T800 Autoclavado: Estanqueidade e rigidez ultraleve.",
                    "6. SROS Enxame Óptico: 750 canais virtuais imunes a atraso de sinal."
                ]
            },
            {
                id: 1,
                name: "Satélite Orbital LEO (Anti-Poeira / Debris)",
                env: "Órbita Baixa da Terra (LEO)",
                status: "Defesa Ativa Anti-Impacto • Feixe Flexível SROS",
                agenda: [
                    "1. Placa Defletora de Titânio Grade 5: Proteção orbital rápida.",
                    "2. Varredura Preditiva IR/X-Ray: Cálculo vetorial de detritos a 15 metros.",
                    "3. Microbobinas de Gradiente 1.2T: Reticulação magnética em 1.15ms.",
                    "4. Espelhos Refletores Magnéticos SROS: Enlace óptico contínuo sem rastro RF.",
                    "5. Heat Pipes Cobre-Metanol: Refrigeração por mudança de fase (18 W/cm²)."
                ]
            },
            {
                id: 2,
                name: "Submarino em Mar Profundo (600 bar)",
                env: "Oceano Profundo / Pressão Hiperbárica",
                status: "Cancelamento de Pressão Abissal (Redução 12.4mm -> 0.21mm)",
                agenda: [
                    "1. Casco Duplo de Titânio/Carbono: Resistência a 600 bar (6.000m de profundidade).",
                    "2. Malha Sensorial Brillouin: Monitoramento estático e hidrostático contínuo.",
                    "3. Matriz PZT de Alta Potência: Contra-força flexional para estabilização de casco.",
                    "4. Controle PWM de 50kHz: Modulação seletiva de microbobinas magnéticas.",
                    "5. Eclusas de Biossegurança Automáticas: Isolamento estanque preventivo."
                ]
            },
            {
                id: 3,
                name: "Navio de Guerra (Fragata / Corveta)",
                env: "Superfície Naval / Mar Aberto",
                agenda: [
                    "1. Superestrutura Multicamadas S.A.M.S.: Absorção de 94.2% da energia dinânica.",
                    "2. Algoritmo Rainflow-Counting: Cancelamento contínuo de fadiga por ressonância marítima.",
                    "3. Sensores CHONPS em Ventilação: Filtragem e exclusão de bioaerossóis em ms.",
                    "4. Enlace SROS de Luz Contínua: Imunidade total a Jamming e guerra eletrônica."
                ]
            },
            {
                id: 4,
                name: "Caça Aéreo (Voo Supersônico / Hipersônico)",
                env: "Atmosfera / Estresse Dinâmico",
                status: "Estabilização Aerodinâmica • Refrigeração Heat-Pipes",
                agenda: [
                    "1. Asas em Titânio/Carbono: Flexibilidade inteligente piezo-assistida.",
                    "2. Núcleo Fluido MRF: Alteração rápida de rigidez sob alta atrito aerodinâmico.",
                    "3. Microcanais Heat-Pipes: Dissipação de calor Joule para proteção do ponto de Curie (320°C).",
                    "4. Sensores CHONPS de Cabine: Purificação instantânea de ar pressurizado."
                ]
            },
            {
                id: 5,
                name: "Míssil Hipersônico (Mach 20)",
                env: "Regime Aerodinâmico Mach 20",
                status: "Ogiva PZT Compensatória • Criptografia Lattice Kyber-1024",
                agenda: [
                    "1. Nariz de Ogiva Piezoelétrico: Anulação mecânica do choque de compressão (15 kN/cm²).",
                    "2. Revestimento Térmico Titânio Gr.5: Suporte a gradientes de -150°C a +450°C.",
                    "3. FPGA Neural Rad-Hard (LSTM-PINN): Processamento preditivo em < 200µs.",
                    "4. Encriptação Kyber-1024 / AES-256-GCM: Proteção total de telemetria."
                ]
            },
            {
                id: 6,
                name: "Tanque & Blindado Presidencial",
                env: "Terrestre / Zona de Conflito",
                status: "Chassi Ativo Anti-Explosão • Blindagem CHONPS QBRN",
                agenda: [
                    "1. Chassi Absorvente S.A.M.S.: Dissipação de 94.2% do pulso explosivo de minas.",
                    "2. Reticulação Magnética Rápida: Endurecimento instantâneo do piso sob impacto.",
                    "3. Sensores Biométricos LWIR/CHONPS: Barreira molecular contra vírus e biopatógenos.",
                    "4. Rede Mesh Autônoma: Operação isolada sem dependência de satélites terrestres."
                ]
            },
            {
                id: 7,
                name: "Bunker Subterrâneo de Alta Resiliência",
                env: "Subterrâneo Profundo / Tectônico",
                status: "Absorção de Ondas Sísmicas • Suporte Vital CHONPS",
                agenda: [
                    "1. Anéis Perimetrais Ativos: Amortecimento de ondas de choque sísmicas e de impacto.",
                    "2. Filtragem por Exclusão CHONPS: Purificação do ar subterrâneo contra pandemias.",
                    "3. Links Ópticos SROS Subterrâneos: Transmissão por luz blindada contra pulsos EMP.",
                    "4. Sistema Fail-Safe Automático: Reticulação magnética local padrão em emergência."
                ]
            }
        ];

        let activeModuleId = 0;
        let magField = 1.2;
        let pztVoltage = 600;
        let envIntensity = 50;

        let threatState = {
            active: false,
            type: null,
            timer: 0,
            x: 0,
            y: 0
        };

        // PARTICLES & ANIMATION ARRAYS
        let particles = [];
        let debrisList = [];
        let bioList = [];
        let waveTime = 0;

        // CANVAS REFERENCES
        const mainCanvas = document.getElementById('mainCanvas');
        const mainCtx = mainCanvas.getContext('2d');

        const graphCanvas = document.getElementById('graphCanvas');
        const graphCtx = graphCanvas.getContext('2d');

        let graphHistory = new Array(100).fill(0);

        // RESIZE CANVAS HANDLER
        function resizeCanvases() {
            mainCanvas.width = mainCanvas.parentElement.clientWidth;
            mainCanvas.height = mainCanvas.parentElement.clientHeight;

            graphCanvas.width = graphCanvas.parentElement.clientWidth;
            graphCanvas.height = graphCanvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvases);

        // SELECTION HANDLER
        function selectModule(id) {
            activeModuleId = id;
            document.getElementById('module-select').value = id;
            
            const mod = modulesData[id];
            document.getElementById('hud-module-title').innerText = `${id+1}. ${mod.name.toUpperCase()}`;
            document.getElementById('hud-module-env').innerText = mod.env;
            document.getElementById('hud-module-status').innerText = mod.status;

            // Render Agenda
            const agendaBox = document.getElementById('agenda-container');
            agendaBox.innerHTML = mod.agenda.map(item => `
                <div class="bg-slate-900/60 border border-hudBorder p-2 rounded">
                    ${item}
                </div>
            `).join('');

            addLog(`Módulo alterado para: ${mod.name}`);
        }

        // PARAMETER SLIDER HANDLER
        function updateParameters() {
            magField = parseFloat(document.getElementById('slider-mag').value);
            pztVoltage = parseFloat(document.getElementById('slider-pzt').value);
            envIntensity = parseInt(document.getElementById('slider-env').value);

            document.getElementById('val-mag').innerText = `${magField.toFixed(1)} T`;
            document.getElementById('val-pzt').innerText = `${pztVoltage} V`;

            const labels = ["Mínimo", "Nominal", "Elevado", "Extremo", "Crítico"];
            document.getElementById('val-env').innerText = labels[Math.min(Math.floor(envIntensity / 25), 4)];

            // Update Telemetry Header
            const eff = Math.min(94.2, 50 + (magField * 20) + (pztVoltage * 0.03)).toFixed(1);
            const mrTime = Math.max(0.9, 1.8 - (magField * 0.5)).toFixed(2);
            document.getElementById('header-sams-eff').innerText = `${eff}%`;
            document.getElementById('header-mr-time').innerText = `${mrTime} ms`;
        }

        // TRIGGER THREAT
        function triggerThreat(type) {
            threatState.active = true;
            threatState.type = type;
            threatState.timer = 120; // 120 frames
            threatState.x = mainCanvas.width * 0.7;
            threatState.y = mainCanvas.height * 0.4;

            const msgs = {
                impact: "ALERTA: Impacto Balístico / Meteoro detectado! Camadas S.A.M.S. ativadas.",
                emp: "ALERTA: Ataque EMP / Jamming! Rede SROS operando com luz contínua.",
                pressure: "ALERTA: Sobrecarga de Pressão Hiperbárica / Sismo! PZT em compensação.",
                bio: "ALERTA: Bioaerossol Virótico detectado! Barreira CHONPS acionada."
            };
            addLog(msgs[type], "text-hudOrange");
        }

        function addLog(msg, colorClass = "text-hudCyan") {
            const logBox = document.getElementById('event-log');
            const item = document.createElement('div');
            item.className = colorClass;
            item.innerText = `> ${msg}`;
            logBox.appendChild(item);
            logBox.scrollTop = logBox.scrollHeight;
        }

        // ====================================================================
        // RENDER SCENARIOS (MAIN CANVAS 2D ENGINES FOR ALL 8 MODULES)
        // ====================================================================

        function drawScenario(cx, cy) {
            waveTime += 0.05;

            switch(activeModuleId) {
                case 0: drawSpaceProbe(cx, cy); break;
                case 1: drawLEOSatellite(cx, cy); break;
                case 2: drawDeepSubmarine(cx, cy); break;
                case 3: drawWarship(cx, cy); break;
                case 4: drawFighterJet(cx, cy); break;
                case 5: drawHypersonicMissile(cx, cy); break;
                case 6: drawArmoredTank(cx, cy); break;
                case 7: drawUndergroundBunker(cx, cy); break;
            }

            // Global Overlay Elements: SROS Laser Mesh & CHONPS Particles
            drawSROSMesh(cx, cy);
            drawCHONPSParticles(cx, cy);

            if(threatState.active) {
                drawThreatOverlay(cx, cy);
                threatState.timer--;
                if(threatState.timer <= 0) threatState.active = false;
            }
        }

        // 1. SONDA ESPACIAL (ESPAÇO PROFUNDO)
        function drawSpaceProbe(cx, cy) {
            // Space Background Stars
            drawStarfield();

            mainCtx.save();
            mainCtx.translate(cx, cy);

            // SAMS Shell Layers Around Body
            drawSAMSShellRing(0, 0, 70);

            // Probe Body Structure
            mainCtx.fillStyle = "#1e293b";
            mainCtx.fillRect(-35, -45, 70, 90);
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.strokeRect(-35, -45, 70, 90);

            // High-Gain Antenna Dish
            mainCtx.beginPath();
            mainCtx.arc(0, -65, 35, Math.PI, 0);
            mainCtx.strokeStyle = "#94a3b8";
            mainCtx.lineWidth = 3;
            mainCtx.stroke();
            mainCtx.fillStyle = "rgba(0, 240, 255, 0.2)";
            mainCtx.fill();

            // Solar Array Wings
            mainCtx.fillStyle = "#0284c7";
            mainCtx.fillRect(-135, -15, 90, 30);
            mainCtx.fillRect(45, -15, 90, 30);
            mainCtx.strokeStyle = "#38bdf8";
            mainCtx.strokeRect(-135, -15, 90, 30);
            mainCtx.strokeRect(45, -15, 90, 30);

            // Labels & Dimensions
            mainCtx.fillStyle = "#00f0ff";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("SONDA S.A.M.S. ESPAÇO PROFUNDO", -80, 75);
            mainCtx.fillText("Ø 2.30m | 15 kN/cm² PZT", -60, 90);

            mainCtx.restore();
        }

        // 2. SATÉLITE ORBITAL LEO
        function drawLEOSatellite(cx, cy) {
            // Earth Curve at Bottom
            mainCtx.save();
            mainCtx.beginPath();
            mainCtx.arc(cx, cy + 650, 600, 0, Math.PI * 2);
            mainCtx.fillStyle = "#0369a1";
            mainCtx.fill();
            mainCtx.strokeStyle = "#38bdf8";
            mainCtx.lineWidth = 4;
            mainCtx.stroke();

            // Satellite Chassis
            mainCtx.translate(cx, cy - 50);

            // Solar Panels
            mainCtx.fillStyle = "#0369a1";
            mainCtx.fillRect(-160, -20, 100, 40);
            mainCtx.fillRect(60, -20, 100, 40);

            // Central SAMS Bus
            drawSAMSShellRing(0, 0, 50);
            mainCtx.fillStyle = "#0f172a";
            mainCtx.beginPath();
            mainCtx.arc(0, 0, 30, 0, Math.PI*2);
            mainCtx.fill();

            // Reflector Mirror
            mainCtx.fillStyle = "#ffaa00";
            mainCtx.beginPath();
            mainCtx.arc(0, -45, 12, 0, Math.PI*2);
            mainCtx.fill();

            mainCtx.fillStyle = "#00ff66";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("SATÉLITE LEO ANTI-POEIRA / DEBRIS", -90, 70);
            mainCtx.fillText("ALTITUDE: 550km | VEL: 7.8 km/s", -85, 85);

            mainCtx.restore();
        }

        // 3. SUBMARINO MAR PROFUNDO (600 BAR)
        function drawDeepSubmarine(cx, cy) {
            // Deep Sea Background Gradient
            mainCtx.save();
            let grad = mainCtx.createLinearGradient(0, 0, 0, mainCanvas.height);
            grad.addColorStop(0, "#0284c7");
            grad.addColorStop(1, "#030712");
            mainCtx.fillStyle = grad;
            mainCtx.fillRect(0, 0, mainCanvas.width, mainCanvas.height);

            // Water Particles/Bubbles
            mainCtx.fillStyle = "rgba(255, 255, 255, 0.2)";
            for(let i=0; i<20; i++) {
                let bx = (cx - 200 + (i * 25) + waveTime * 10) % mainCanvas.width;
                let by = (cy - 100 + (i * 17)) % mainCanvas.height;
                mainCtx.beginPath();
                mainCtx.arc(bx, by, 2, 0, Math.PI*2);
                mainCtx.fill();
            }

            mainCtx.translate(cx, cy);

            // Submarine Hull (SAMS Titanium Double Hull)
            mainCtx.beginPath();
            mainCtx.ellipse(0, 0, 160, 50, 0, 0, Math.PI * 2);
            mainCtx.fillStyle = "#1e293b";
            mainCtx.fill();
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.lineWidth = 4;
            mainCtx.stroke();

            // Inner Active SAMS Ring
            drawSAMSShellRing(0, 0, 42);

            // Conning Tower (Tower)
            mainCtx.fillRect(-20, -75, 40, 30);
            mainCtx.strokeRect(-20, -75, 40, 30);

            // Pressure Vector Arrows Pushing Hull (Attenuated)
            mainCtx.strokeStyle = "rgba(239, 68, 68, 0.7)";
            mainCtx.lineWidth = 2;
            for(let a=0; a<Math.PI*2; a+=Math.PI/4) {
                let px = Math.cos(a) * 180;
                let py = Math.sin(a) * 70;
                mainCtx.beginPath();
                mainCtx.moveTo(px, py);
                mainCtx.lineTo(px * 0.85, py * 0.85);
                mainCtx.stroke();
            }

            // Real-Time Pressure Indicator
            mainCtx.fillStyle = "#00f0ff";
            mainCtx.font = "11px Courier New";
            mainCtx.fillText("PROFUNDIDADE: 6.000m | PRESSÃO: 600 bar", -120, 80);
            mainCtx.fillStyle = "#00ff66";
            mainCtx.fillText("DEFLEXÃO CASCO: 0.21mm (SEM SAMS: 12.4mm)", -130, 98);

            mainCtx.restore();
        }

        // 4. NAVIO DE GUERRA (FRAGATA / CORVETA)
        function drawWarship(cx, cy) {
            // Ocean Surface & Waves
            mainCtx.save();
            mainCtx.fillStyle = "#0c4a6e";
            mainCtx.fillRect(0, cy + 20, mainCanvas.width, mainCanvas.height);

            mainCtx.strokeStyle = "#38bdf8";
            mainCtx.lineWidth = 2;
            mainCtx.beginPath();
            for(let x=0; x<mainCanvas.width; x+=10) {
                let y = cy + 20 + Math.sin(x * 0.03 + waveTime * 2) * 8;
                if(x===0) mainCtx.moveTo(x, y);
                else mainCtx.lineTo(x, y);
            }
            mainCtx.stroke();

            // Ship Hull
            mainCtx.translate(cx, cy + 10 + Math.sin(waveTime) * 3);

            mainCtx.beginPath();
            mainCtx.moveTo(-160, -20);
            mainCtx.lineTo(120, -20);
            mainCtx.lineTo(160, 25);
            mainCtx.lineTo(-140, 25);
            mainCtx.closePath();
            mainCtx.fillStyle = "#1e293b";
            mainCtx.fill();
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.lineWidth = 3;
            mainCtx.stroke();

            // Superstructure Multilayer Active Core
            drawSAMSShellRing(0, -45, 30);

            // Radar Mast & SROS Laser Transmitter
            mainCtx.fillStyle = "#475569";
            mainCtx.fillRect(-10, -85, 20, 40);
            mainCtx.fillStyle = "#00ff66";
            mainCtx.beginPath();
            mainCtx.arc(0, -90, 8, 0, Math.PI*2);
            mainCtx.fill();

            mainCtx.fillStyle = "#00f0ff";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("FRAGATA / CORVETA GUERRA NAVAL", -100, 50);
            mainCtx.fillText("RAINFLOW-COUNTING FADIGA MARÍTIMA: ATIVO", -125, 65);

            mainCtx.restore();
        }

        // 5. CAÇA AÉREO SUPERSÔNICO
        function drawFighterJet(cx, cy) {
            drawStarfield();

            mainCtx.save();
            mainCtx.translate(cx, cy);

            // Shockwave Cone
            mainCtx.strokeStyle = "rgba(0, 240, 255, 0.4)";
            mainCtx.lineWidth = 2;
            mainCtx.beginPath();
            mainCtx.moveTo(150, 0);
            mainCtx.lineTo(-180, -110);
            mainCtx.moveTo(150, 0);
            mainCtx.lineTo(-180, 110);
            mainCtx.stroke();

            // Fighter Fuselage
            mainCtx.beginPath();
            mainCtx.moveTo(150, 0); // Nose
            mainCtx.lineTo(-40, -30);
            mainCtx.lineTo(-120, -90); // Left Wing
            mainCtx.lineTo(-90, -20);
            mainCtx.lineTo(-150, 0); // Tail
            mainCtx.lineTo(-90, 20);
            mainCtx.lineTo(-120, 90); // Right Wing
            mainCtx.lineTo(-40, 30);
            mainCtx.closePath();
            mainCtx.fillStyle = "#0f172a";
            mainCtx.fill();
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.lineWidth = 3;
            mainCtx.stroke();

            // Active Wings SAMS Shell
            drawSAMSShellRing(-20, 0, 35);

            // Heat Pipes Indicators (Red/Orange Lines)
            mainCtx.strokeStyle = "#ffaa00";
            mainCtx.lineWidth = 2;
            mainCtx.beginPath();
            mainCtx.moveTo(-30, -15); mainCtx.lineTo(50, -5);
            mainCtx.moveTo(-30, 15); mainCtx.lineTo(50, 5);
            mainCtx.stroke();

            mainCtx.fillStyle = "#00f0ff";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("CAÇA AÉREO - HEAT-PIPES COBRE-METANOL", -110, 120);
            mainCtx.fillText("DISSIPAÇÃO JOULE: 18 W/cm²", -85, 135);

            mainCtx.restore();
        }

        // 6. MÍSSIL HIPERSÔNICO MACH 20
        function drawHypersonicMissile(cx, cy) {
            mainCtx.save();
            mainCtx.translate(cx, cy);

            // Plasma Fire Trail (Mach 20)
            let plasmaGrad = mainCtx.createLinearGradient(-220, 0, -80, 0);
            plasmaGrad.addColorStop(0, "rgba(239, 68, 68, 0)");
            plasmaGrad.addColorStop(0.5, "rgba(249, 115, 22, 0.8)");
            plasmaGrad.addColorStop(1, "rgba(234, 179, 8, 1)");

            mainCtx.fillStyle = plasmaGrad;
            mainCtx.beginPath();
            mainCtx.moveTo(-80, -20);
            mainCtx.lineTo(-240, -40 + Math.sin(waveTime*5)*5);
            mainCtx.lineTo(-240, 40 + Math.sin(waveTime*5)*5);
            mainCtx.lineTo(-80, 20);
            mainCtx.closePath();
            mainCtx.fill();

            // Hypersonic Body
            mainCtx.beginPath();
            mainCtx.moveTo(160, 0); // Nose Tip
            mainCtx.lineTo(60, -20);
            mainCtx.lineTo(-100, -20);
            mainCtx.lineTo(-120, -35); // Fin
            mainCtx.lineTo(-100, 0);
            mainCtx.lineTo(-120, 35); // Fin
            mainCtx.lineTo(-100, 20);
            mainCtx.lineTo(60, 20);
            mainCtx.closePath();
            mainCtx.fillStyle = "#1e293b";
            mainCtx.fill();
            mainCtx.strokeStyle = "#ff3366";
            mainCtx.lineWidth = 3;
            mainCtx.stroke();

            // Piezoelectric Nose Counter-Force Stack
            mainCtx.fillStyle = "#a855f7";
            mainCtx.beginPath();
            mainCtx.arc(140, 0, 12, 0, Math.PI*2);
            mainCtx.fill();

            mainCtx.fillStyle = "#ffaa00";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("MÍSSIL HIPERSÔNICO MACH 20", -70, 70);
            mainCtx.fillText("CONTRA-FORÇA PZT OGIVA: 15 kN/cm²", -90, 85);
            mainCtx.fillStyle = "#00ff66";
            mainCtx.fillText("CRIPTOGRAFIA: LATTICE KYBER-1024", -85, 100);

            mainCtx.restore();
        }

        // 7. TANQUE & BLINDADO PRESIDENCIAL
        function drawArmoredTank(cx, cy) {
            // Ground Terrain
            mainCtx.save();
            mainCtx.fillStyle = "#1c1917";
            mainCtx.fillRect(0, cy + 35, mainCanvas.width, mainCanvas.height);

            mainCtx.strokeStyle = "#78350f";
            mainCtx.lineWidth = 2;
            mainCtx.beginPath();
            mainCtx.moveTo(0, cy + 35);
            mainCtx.lineTo(mainCanvas.width, cy + 35);
            mainCtx.stroke();

            mainCtx.translate(cx, cy + 5);

            // Tracks (Treads)
            mainCtx.fillStyle = "#27272a";
            mainCtx.fillRect(-130, 10, 260, 25);
            mainCtx.strokeStyle = "#52525b";
            mainCtx.strokeRect(-130, 10, 260, 25);

            // Active Chassis (SAMS Shell Floor)
            mainCtx.fillStyle = "#1e293b";
            mainCtx.fillRect(-120, -25, 240, 35);
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.lineWidth = 3;
            mainCtx.strokeRect(-120, -25, 240, 35);

            // Active Core Shell
            drawSAMSShellRing(0, -10, 28);

            // Turret & Cannon
            mainCtx.fillRect(-50, -55, 100, 30);
            mainCtx.fillRect(50, -45, 90, 10);

            mainCtx.fillStyle = "#00f0ff";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("BLINDADO PRESIDENCIAL S.A.M.S.", -90, 60);
            mainCtx.fillText("CHASSI ANTI-MINA: 94.2% ABSORÇÃO", -95, 75);

            mainCtx.restore();
        }

        // 8. BUNKER SUBTERRÂNEO RESILIENTE
        function drawUndergroundBunker(cx, cy) {
            // Earth Cross-Section
            mainCtx.save();
            mainCtx.fillStyle = "#292524";
            mainCtx.fillRect(0, 0, mainCanvas.width, mainCanvas.height);

            // Surface Layer
            mainCtx.fillStyle = "#15803d";
            mainCtx.fillRect(0, 0, mainCanvas.width, 90);

            // Underground Pit Outer Border
            mainCtx.translate(cx, cy + 20);

            // Subterranean Vault Structure
            mainCtx.fillStyle = "#0f172a";
            mainCtx.fillRect(-160, -90, 320, 180);
            mainCtx.strokeStyle = "#00f0ff";
            mainCtx.lineWidth = 4;
            mainCtx.strokeRect(-160, -90, 320, 180);

            // Active Seismic Damping Ring SAMS
            drawSAMSShellRing(0, 0, 75);

            // Internal Floors & Chambers
            mainCtx.strokeStyle = "#334155";
            mainCtx.lineWidth = 2;
            mainCtx.beginPath();
            mainCtx.moveTo(-160, 0); mainCtx.lineTo(160, 0);
            mainCtx.stroke();

            mainCtx.fillStyle = "#00ff66";
            mainCtx.font = "10px Courier New";
            mainCtx.fillText("BUNKER SUBTERRÂNEO ALTA RESILIÊNCIA", -110, -105);
            mainCtx.fillText("ABSORÇÃO SÍSMICA / TECTÔNICA ATIVA", -105, 115);

            mainCtx.restore();
        }

        // HELPER: DRAW SAMS MULTILAYER RING
        function drawSAMSShellRing(x, y, radius) {
            const layers = [
                { color: "#94a3b8", width: 6 }, // C1: Titânio
                { color: "#00f0ff", width: 4 }, // C2: BOTDA
                { color: magField > 0.4 ? "#ffaa00" : "#78350f", width: 5 }, // C3: Fluido MR
                { color: "#a855f7", width: 4 }, // C4: PZT-8
                { color: "#475569", width: 3 }  // C5: Carbono
            ];

            let curR = radius;
            layers.forEach(l => {
                mainCtx.beginPath();
                mainCtx.arc(x, y, curR, 0, Math.PI * 2);
                mainCtx.strokeStyle = l.color;
                mainCtx.lineWidth = l.width;
                mainCtx.stroke();
                curR -= l.width;
            });
        }

        // HELPER: STARFIELD FOR SPACE/JET
        function drawStarfield() {
            if(particles.length < 60) {
                particles.push({
                    x: Math.random() * mainCanvas.width,
                    y: Math.random() * mainCanvas.height,
                    s: Math.random() * 2,
                    v: Math.random() * 2 + 0.5
                });
            }
            mainCtx.fillStyle = "rgba(255,255,255,0.5)";
            particles.forEach(p => {
                mainCtx.beginPath();
                mainCtx.arc(p.x, p.y, p.s, 0, Math.PI*2);
                mainCtx.fill();
                p.x -= p.v * (envIntensity / 30);
                if(p.x < 0) p.x = mainCanvas.width;
            });
        }

        // HELPER: SROS OPTICAL LASER MESH
        function drawSROSMesh(cx, cy) {
            mainCtx.save();
            mainCtx.strokeStyle = threatState.type === 'emp' ? "#ffaa00" : "#00ff66";
            mainCtx.lineWidth = 1.5;
            mainCtx.setLineDash([6, 3]);

            // Satellite Repeaters
            const nodes = [
                {x: 80, y: 80},
                {x: mainCanvas.width - 80, y: 80},
                {x: mainCanvas.width - 80, y: mainCanvas.height - 80},
                {x: 80, y: mainCanvas.height - 80}
            ];

            nodes.forEach(n => {
                mainCtx.beginPath();
                mainCtx.arc(n.x, n.y, 5, 0, Math.PI*2);
                mainCtx.fillStyle = "#00f0ff";
                mainCtx.fill();

                mainCtx.beginPath();
                mainCtx.moveTo(n.x, n.y);
                mainCtx.lineTo(cx, cy);
                mainCtx.stroke();
            });

            mainCtx.restore();
        }

        // HELPER: CHONPS BIO-AEROSOL SCANNER & PARTICLES
        function drawCHONPSParticles(cx, cy) {
            if(bioList.length < 15) {
                let angle = Math.random() * Math.PI * 2;
                bioList.push({
                    x: cx + Math.cos(angle) * 220,
                    y: cy + Math.sin(angle) * 220,
                    blocked: false
                });
            }

            mainCtx.save();
            bioList.forEach(p => {
                let dist = Math.hypot(p.x - cx, p.y - cy);
                if(dist < 110) p.blocked = true;

                if(p.blocked) {
                    p.x += (p.x - cx) * 0.08;
                    p.y += (p.y - cy) * 0.08;
                    mainCtx.fillStyle = "#ff3366";
                } else {
                    p.x += (cx - p.x) * 0.02;
                    p.y += (cy - p.y) * 0.02;
                    mainCtx.fillStyle = "#00ff66";
                }

                mainCtx.beginPath();
                mainCtx.arc(p.x, p.y, 3, 0, Math.PI*2);
                mainCtx.fill();
            });

            bioList = bioList.filter(p => Math.hypot(p.x - cx, p.y - cy) < 260);
            mainCtx.restore();
        }

        // HELPER: THREAT EFFECT OVERLAY
        function drawThreatOverlay(cx, cy) {
            mainCtx.save();
            if(threatState.type === 'impact') {
                mainCtx.strokeStyle = "#ff3366";
                mainCtx.lineWidth = 4;
                mainCtx.beginPath();
                mainCtx.moveTo(threatState.x + 80, threatState.y - 80);
                mainCtx.lineTo(cx, cy);
                mainCtx.stroke();

                mainCtx.fillStyle = "rgba(255, 170, 0, 0.6)";
                mainCtx.beginPath();
                mainCtx.arc(cx, cy, 35, 0, Math.PI*2);
                mainCtx.fill();
            } else if(threatState.type === 'emp') {
                mainCtx.strokeStyle = "rgba(255, 170, 0, 0.8)";
                mainCtx.lineWidth = 3;
                mainCtx.beginPath();
                mainCtx.arc(cx, cy, 140, 0, Math.PI*2);
                mainCtx.stroke();
            } else if(threatState.type === 'pressure') {
                mainCtx.strokeStyle = "rgba(0, 240, 255, 0.8)";
                mainCtx.lineWidth = 4;
                mainCtx.beginPath();
                mainCtx.arc(cx, cy, 90 + (threatState.timer % 20)*3, 0, Math.PI*2);
                mainCtx.stroke();
            } else if(threatState.type === 'bio') {
                mainCtx.fillStyle = "rgba(0, 255, 102, 0.25)";
                mainCtx.beginPath();
                mainCtx.arc(cx, cy, 160, 0, Math.PI*2);
                mainCtx.fill();
            }
            mainCtx.restore();
        }

        // ====================================================================
        // REAL-TIME GRAPH CANVAS ENGINE
        // ====================================================================

        function updateGraph() {
            graphCtx.clearRect(0, 0, graphCanvas.width, graphCanvas.height);

            // Shift history
            graphHistory.shift();
            let rawInput = Math.sin(waveTime * 2) * 30 + (threatState.active ? (Math.random()*25 - 12) : 0);
            graphHistory.push(rawInput);

            let w = graphCanvas.width;
            let h = graphCanvas.height;
            let midY = h / 2;

            // Draw Passive Unattenuated Curve (Red)
            graphCtx.strokeStyle = "#ff3366";
            graphCtx.lineWidth = 1;
            graphCtx.beginPath();
            for(let i=0; i<graphHistory.length; i++) {
                let x = (i / graphHistory.length) * w;
                let y = midY + graphHistory[i];
                if(i===0) graphCtx.moveTo(x, y);
                else graphCtx.lineTo(x, y);
            }
            graphCtx.stroke();

            // Draw Active S.A.M.S. Attenuated Curve (Green - 94.2% Reduction)
            graphCtx.strokeStyle = "#00ff66";
            graphCtx.lineWidth = 2;
            graphCtx.beginPath();
            for(let i=0; i<graphHistory.length; i++) {
                let x = (i / graphHistory.length) * w;
                let y = midY + (graphHistory[i] * 0.058); // 94.2% Attenuation
                if(i===0) graphCtx.moveTo(x, y);
                else graphCtx.lineTo(x, y);
            }
            graphCtx.stroke();
        }

        // MAIN ANIMATION LOOP
        function animateLoop() {
            mainCtx.clearRect(0, 0, mainCanvas.width, mainCanvas.height);

            const cx = mainCanvas.width / 2;
            const cy = mainCanvas.height / 2;

            drawScenario(cx, cy);
            updateGraph();

            requestAnimationFrame(animateLoop);
        }

        // WINDOW ONLOAD INITIALIZER
        window.onload = function() {
            resizeCanvases();
            selectModule(0);
            updateParameters();
            animateLoop();
        };
    </script>
</body>
</html>
