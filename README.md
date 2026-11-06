<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PROJETO S.A.M.S. / SROS / BIOSSEGURANÇA - SIMULADOR UNIFICADO</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        hudBg: '#090d14',
                        hudPanel: '#0f172a',
                        hudBorder: '#1e293b',
                        hudCyan: '#06b6d4',
                        hudOrange: '#f97316',
                        hudGreen: '#10b981',
                        hudRed: '#ef4444',
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
            background-color: #05080e;
            color: #e2e8f0;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
        }
        canvas {
            display: block;
            background: radial-gradient(circle at center, #0b1329 0%, #030712 100%);
        }
        .glow-cyan {
            box-shadow: 0 0 15px rgba(6, 182, 212, 0.4);
        }
        .glow-orange {
            box-shadow: 0 0 15px rgba(249, 115, 22, 0.4);
        }
        .glow-green {
            box-shadow: 0 0 15px rgba(16, 185, 129, 0.4);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #090d14;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #06b6d4;
        }
    </style>
</head>
<body class="h-screen flex flex-col md:flex-row overflow-hidden font-mono">

    <!-- SIDEBAR / HUD CONTROL PANEL -->
    <aside class="w-full md:w-96 bg-hudPanel border-b md:border-b-0 md:border-r border-hudBorder flex flex-col justify-between z-20 h-auto md:h-screen overflow-y-auto">
        <div class="p-4 border-b border-hudBorder bg-slate-900/50">
            <div class="flex items-center justify-between">
                <span class="text-xs font-bold text-hudCyan tracking-widest uppercase">Tecnologia Tríplice Unificada</span>
                <span class="px-2 py-0.5 text-[10px] bg-cyan-950 text-hudCyan rounded border border-hudCyan/40">TRL 3-7</span>
            </div>
            <h1 class="text-lg font-extrabold text-white tracking-wider mt-1">S.A.M.S. / SROS / BIO</h1>
            <p class="text-[11px] text-slate-400">Inventor: Rafael Barbosa da Silva</p>
        </div>

        <!-- MÓDULOS OPERACIONAIS SELECTION -->
        <div class="p-4 space-y-3">
            <h2 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Módulos de Operação Multimeios</h2>
            <div class="grid grid-cols-2 gap-2 text-xs">
                <button onclick="selectModule(0)" id="mod-btn-0" class="mod-btn p-2 rounded bg-cyan-950/60 border border-hudCyan text-hudCyan text-left hover:bg-cyan-900 transition flex flex-col">
                    <span class="font-bold">Mód. 1: Sonda</span>
                    <span class="text-[10px] opacity-75">Espaço Profundo</span>
                </button>
                <button onclick="selectModule(1)" id="mod-btn-1" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 2: Satélite</span>
                    <span class="text-[10px] opacity-75">Órbita LEO / Anti-Poeira</span>
                </button>
                <button onclick="selectModule(2)" id="mod-btn-2" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 3: Submarino</span>
                    <span class="text-[10px] opacity-75">Mar Profundo / 600 bar</span>
                </button>
                <button onclick="selectModule(3)" id="mod-btn-3" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 4: Fragata/Navio</span>
                    <span class="text-[10px] opacity-75">Superfície Naval</span>
                </button>
                <button onclick="selectModule(4)" id="mod-btn-4" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 5: Caça Aéreo</span>
                    <span class="text-[10px] opacity-75">Sustentação Mach 3+</span>
                </button>
                <button onclick="selectModule(5)" id="mod-btn-5" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 6: Míssil Mach 20</span>
                    <span class="text-[10px] opacity-75">Trajetória Hipersônica</span>
                </button>
                <button onclick="selectModule(6)" id="mod-btn-6" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 7: Blindado Tanque</span>
                    <span class="text-[10px] opacity-75">Presidencial / Balístico</span>
                </button>
                <button onclick="selectModule(7)" id="mod-btn-7" class="mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col">
                    <span class="font-bold">Mód. 8: Bunker</span>
                    <span class="text-[10px] opacity-75">Subterrâneo Resiliente</span>
                </button>
            </div>
        </div>

        <!-- SIMULADOR DE AMEAÇAS / GATILHOS DE ESTRESSE -->
        <div class="p-4 border-t border-hudBorder space-y-3">
            <h2 class="text-xs font-semibold text-hudOrange uppercase tracking-wider flex items-center gap-1">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
                Disparar Ameaças em Tempo Real
            </h2>
            <div class="grid grid-cols-2 gap-2 text-[11px]">
                <button onclick="triggerThreat('impact')" class="p-2 bg-red-950/40 hover:bg-red-900/60 border border-hudRed/60 text-red-300 rounded font-bold transition">
                    💥 Impacto / Meteoro
                </button>
                <button onclick="triggerThreat('pressure')" class="p-2 bg-blue-950/40 hover:bg-blue-900/60 border border-blue-500/60 text-blue-300 rounded font-bold transition">
                    🌊 Pressão / Sismo
                </button>
                <button onclick="triggerThreat('emp')" class="p-2 bg-amber-950/40 hover:bg-amber-900/60 border border-amber-500/60 text-amber-300 rounded font-bold transition">
                    ⚡ Cyber EMP / Jamming
                </button>
                <button onclick="triggerThreat('bio')" class="p-2 bg-emerald-950/40 hover:bg-emerald-900/60 border border-emerald-500/60 text-emerald-300 rounded font-bold transition">
                    ☣️ Ameaça CHONPS
                </button>
            </div>
        </div>

        <!-- CONTROLES DE PARÂMETROS MULTIFÍSICOS -->
        <div class="p-4 border-t border-hudBorder space-y-3 text-xs">
            <h2 class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Ajuste de Parâmetros Operacionais</h2>
            
            <div>
                <div class="flex justify-between text-[11px] mb-1">
                    <span class="text-slate-300">Campo Magnético (Microbobinas B):</span>
                    <span id="val-mag" class="text-hudCyan font-bold">1.2 T</span>
                </div>
                <input type="range" id="slider-mag" min="0.0" max="1.5" step="0.1" value="1.2" oninput="updateSliders()" class="w-full accent-hudCyan bg-slate-800 rounded">
            </div>

            <div>
                <div class="flex justify-between text-[11px] mb-1">
                    <span class="text-slate-300">Tensão Piezoelétrica (V_pzt):</span>
                    <span id="val-pzt" class="text-hudOrange font-bold">15.0 kV</span>
                </div>
                <input type="range" id="slider-pzt" min="0.0" max="25.0" step="0.5" value="15.0" oninput="updateSliders()" class="w-full accent-hudOrange bg-slate-800 rounded">
            </div>

            <div>
                <div class="flex justify-between text-[11px] mb-1">
                    <span class="text-slate-300">Velocidade / Pressão do Meio:</span>
                    <span id="val-env" class="text-hudGreen font-bold">Orbital / 0 bar</span>
                </div>
                <input type="range" id="slider-env" min="0" max="100" step="1" value="50" oninput="updateSliders()" class="w-full accent-hudGreen bg-slate-800 rounded">
            </div>
        </div>

        <!-- AGENDA DE COMPONENTES / ESPECIFICAÇÕES -->
        <div class="p-4 border-t border-hudBorder bg-slate-900/80 max-h-48 overflow-y-auto text-[11px] space-y-1">
            <h3 class="font-bold text-slate-300 uppercase tracking-wider border-b border-slate-700 pb-1">Agenda de Componentes NBR/ISO</h3>
            <div id="component-agenda" class="text-slate-400 space-y-1 text-[10px]">
                <!-- Injetado dinamicamente via JS -->
            </div>
        </div>
    </aside>

    <!-- MAIN CANVAS & HUD MONITOR DISPLAY -->
    <main class="flex-1 flex flex-col relative h-full overflow-hidden">
        
        <!-- HEADER HUD TELEMETRY BAR -->
        <header class="h-14 bg-hudPanel/90 border-b border-hudBorder flex items-center justify-between px-6 z-10 backdrop-blur-md">
            <div class="flex items-center space-x-6 text-xs">
                <div>
                    <span class="text-slate-500 block text-[9px]">S.A.M.S. CANCELAMENTO DE TENSÃO</span>
                    <span id="tel-sams" class="font-bold text-hudCyan text-sm">94.2%</span>
                </div>
                <div class="h-6 w-px bg-hudBorder"></div>
                <div>
                    <span class="text-slate-500 block text-[9px]">RESPOSTA FLUIDO MR</span>
                    <span id="tel-mr" class="font-bold text-hudOrange text-sm">1.18 ms</span>
                </div>
                <div class="h-6 w-px bg-hudBorder"></div>
                <div>
                    <span class="text-slate-500 block text-[9px]">RETENÇÃO BIOSSEGURA CHONPS</span>
                    <span id="tel-bio" class="font-bold text-hudGreen text-sm">100.0%</span>
                </div>
                <div class="h-6 w-px bg-hudBorder"></div>
                <div>
                    <span class="text-slate-500 block text-[9px]">REDE ÓPTICA SROS</span>
                    <span id="tel-sros" class="font-bold text-emerald-400 text-sm">750 CANAIS (ATIVO)</span>
                </div>
            </div>

            <div class="flex items-center space-x-2">
                <span id="status-tag" class="px-2 py-1 bg-emerald-950 border border-emerald-500 text-emerald-400 text-[10px] font-bold rounded animate-pulse">
                    ESTÁVEL / NOMINAL
                </span>
            </div>
        </header>

        <!-- CANVAS MAIN SCREEN -->
        <div class="flex-1 relative">
            <canvas id="simCanvas" class="w-full h-full"></canvas>
            
            <!-- OVERLAY LEGEND FOR MULTILAYER COATING -->
            <div class="absolute bottom-4 left-4 bg-slate-900/80 border border-hudBorder p-3 rounded text-[11px] backdrop-blur-sm pointer-events-none max-w-sm space-y-1">
                <div class="font-bold text-slate-200 border-b border-slate-700 pb-1 mb-1">Arquitetura Multicamadas Ativa S.A.M.S.</div>
                <div class="flex items-center gap-2"><span class="w-3 h-3 rounded-full bg-slate-400 inline-block border border-white"></span> C1: Titânio Ti-6Al-4V Grade 5 (Resistência)</div>
                <div class="flex items-center gap-2"><span class="w-3 h-3 rounded-full bg-cyan-400 inline-block animate-pulse"></span> C2: Fibra Óptica BOTDA / IR (Sensoriamento Brillouin)</div>
                <div class="flex items-center gap-2"><span class="w-3 h-3 rounded-full bg-amber-500 inline-block"></span> C3: Fluido Magnetorreológico MRF-140CG (Fase Líquido/Sólido)</div>
                <div class="flex items-center gap-2"><span class="w-3 h-3 rounded-full bg-purple-500 inline-block"></span> C4: Atuadores Piezoelétricos PZT-8 Stacked (Contra-Força)</div>
                <div class="flex items-center gap-2"><span class="w-3 h-3 rounded-full bg-slate-700 inline-block border border-slate-500"></span> C5: Carbono Aeroespacial T800 (Habitáculo Estanque)</div>
            </div>

            <!-- OVERLAY FLOATING LOG -->
            <div id="event-log" class="absolute top-4 right-4 bg-slate-950/80 border border-hudBorder p-3 rounded text-[10px] w-64 max-h-40 overflow-y-auto space-y-1 backdrop-blur-sm">
                <div class="text-slate-400 font-bold border-b border-slate-800 pb-0.5 mb-1">[ LOG DE EVENTOS DO SISTEMA ]</div>
                <div class="text-emerald-400">> Sistema inicializado em TRL 3.</div>
                <div class="text-hudCyan">> Malha óptica SROS sincronizada.</div>
            </div>
        </div>

    </main>

    <!-- JAVASCRIPT APP ENGINE -->
    <script>
        // ESTADO GLOBAL DO SIMULADOR
        const modulesData = [
            {
                name: "Sonda Espacial (Espaço Profundo)",
                env: "Vácuo / Radiação Cósmica",
                agenda: [
                    "1.1 Placa Titânio Grade 5 (8mm) - Proteção Térmica (-150°C a +450°C)",
                    "1.2 Fibra Óptica BOTDA (500m) - Resolução temporal 0.1ms",
                    "1.3 Fluido MRF-140CG (15L) - Reologia reversível < 1.2ms",
                    "1.4 Stacks Piezoelétricos PZT-8 (64 un) - Contra-força 15kN/cm²",
                    "1.5 Painel Carbono T800 - Estanqueidade e peso ultraleve",
                    "1.6 FPGA Rad-Hard Ultrascale+ (IA de Borda Kyber-1024)"
                ]
            },
            {
                name: "Satélite Orbital LEO (Anti-Poeira / Meteoros)",
                env: "Órbita Baixa (LEO) / Debris Hipervelozes",
                agenda: [
                    "2.1 Defletor de Titânio - Amortecimento inicial de detritos",
                    "2.2 Varredura IR/X-Ray de Borda - Cálculo preditivo de vetor < 200µs",
                    "2.3 Microbobinas Magnéticas 1.2T - Ativação local em 1.15ms",
                    "2.4 Espelhos Refletores Ópticos SROS - Comunicação contínua sem delay",
                    "2.5 Heat Pipes Cobre-Metanol - Gerenciamento Joule 18W/cm²"
                ]
            },
            {
                name: "Submarino em Mar Profundo (600 bar)",
                env: "Pressão Hiperbárica Abissal (6.000m)",
                agenda: [
                    "3.1 Casco Duplo Titânio-Carbono - Tensão limite 8.4 MPa",
                    "3.2 Atuadores PZT Estabilizadores - Redução de deflexão de 12.4mm para 0.21mm",
                    "3.3 Matriz de Pressão Hidrostática - Controle PWM 50kHz",
                    "3.4 Eclusas de Biossegurança Automáticas - Isolamento autônomo",
                    "3.5 Sensores Fisiológicos LWIR - Fotopletismografia remota da tripulação"
                ]
            },
            {
                name: "Navio de Guerra (Fragata / Corveta)",
                env: "Superfície Naval / Onda de Choque Balística",
                agenda: [
                    "4.1 Superestrutura Multicamada S.A.M.S. - Atenuação de 94.2% de estresse",
                    "4.2 Algoritmo Rainflow-Counting - Cancelamento de fadiga harmônica",
                    "4.3 Sensores CHONPS em Ventilação - Filtragem de bioaerossóis",
                    "4.4 Antena Óptica SROS Laser - Imunidade total a Jamming RF"
                ]
            },
            {
                name: "Caça Aéreo Supersônico / Hipersônico",
                env: "Atmosfera / Estresse Dinâmico Mach 3+",
                agenda: [
                    "5.1 Asas de Reologia Magnética Adaptativa - Rigidez dinâmica",
                    "5.2 Matriz Sensorial Brillouin - Mapeamento continuo de strain nas asas",
                    "5.3 Sensores de Fluxo de Ar CHONPS - Exclusão de biocontaminantes em ms",
                    "5.4 Uplink SROS Inter-aeronaves - Malha Mesh tática imune"
                ]
            },
            {
                name: "Míssil Hipersônico Mach 20",
                env: "Plasma Aerodinâmico / Mach 20",
                agenda: [
                    "6.1 Ogiva Piezoelétrica Stacked - Resposta de contra-força a 1450m/s",
                    "6.2 Heat Pipes Integrados - Proteção do ponto de Curie (320°C)",
                    "6.3 FPGA Neural LSTM-PINN - Correção vetorial em microssegundos",
                    "6.4 Protocolo SAMS-SecProtocol - Encriptação pós-quântica"
                ]
            },
            {
                name: "Tanque & Blindado Presidencial",
                env: "Terrestre / Balístico Anti-Mina",
                agenda: [
                    "7.1 Chassi Ativo Absorvente - Redução de pulso explosivo de piso",
                    "7.2 Sensores Biométricos LWIR/CHONPS - Proteção contra contágio biológico",
                    "7.3 Malha Criptográfica Kyber-1024 - Proteção anti-hackeamento",
                    "7.4 Atuadores PZT Modulares - Amortecimento de impacto direto"
                ]
            },
            {
                name: "Bunker Subterrâneo Resiliente",
                env: "Subterrâneo Profundo / Impacto Catastrófico",
                agenda: [
                    "8.1 Anéis Perimetrais S.A.M.S. - Amortecimento de ondas sísmicas",
                    "8.2 Sistema Vital por Exclusão Molecular CHONPS - 100% retenção de vírus",
                    "8.3 Rede Subterrânea SROS Óptica - Comunicação blindada contra EMP",
                    "8.4 Modo Fail-Safe Autônomo - Reticulação magnética padrão em emergência"
                ]
            }
        ];

        let activeModule = 0;
        let magField = 1.2;
        let pztVoltage = 15.0;
        let envVal = 50;
        
        let threatState = {
            active: false,
            type: null,
            timer: 0,
            x: 0,
            y: 0
        };

        // PARTICULAS E ONDAS DE SIMULAÇÃO
        let particles = [];
        let laserBeams = [];
        let bioParticles = [];

        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // INICIALIZAÇÃO DO CANVAS E RE-ESCALONAMENTO RESPONSIVO
        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // LOG DE EVENTOS
        function addLog(msg, colorClass = 'text-hudCyan') {
            const logBox = document.getElementById('event-log');
            const entry = document.createElement('div');
            entry.className = colorClass;
            entry.innerText = `> ${msg}`;
            logBox.appendChild(entry);
            logBox.scrollTop = logBox.scrollHeight;
        }

        // SELEÇÃO DE MÓDULO
        function selectModule(index) {
            activeModule = index;
            document.querySelectorAll('.mod-btn').forEach((btn, idx) => {
                if(idx === index) {
                    btn.className = "mod-btn p-2 rounded bg-cyan-950/60 border border-hudCyan text-hudCyan text-left hover:bg-cyan-900 transition flex flex-col glow-cyan";
                } else {
                    btn.className = "mod-btn p-2 rounded bg-slate-800/40 border border-hudBorder text-slate-300 text-left hover:bg-slate-800 transition flex flex-col";
                }
            });

            // Atualiza agenda de componentes
            const agendaBox = document.getElementById('component-agenda');
            agendaBox.innerHTML = modulesData[index].agenda.map(item => `<div>• ${item}</div>`).join('');
            
            addLog(`Módulo alterado: ${modulesData[index].name}`);
        }

        // AJUSTE DOS SLIDERS
        function updateSliders() {
            magField = parseFloat(document.getElementById('slider-mag').value);
            pztVoltage = parseFloat(document.getElementById('slider-pzt').value);
            envVal = parseInt(document.getElementById('slider-env').value);

            document.getElementById('val-mag').innerText = `${magField.toFixed(1)} T`;
            document.getElementById('val-pzt').innerText = `${pztVoltage.toFixed(1)} kV`;
            
            const envLabels = ["Mínimo", "Orbital", "Severo", "Hiperbárico 600bar", "Mach 20 Extreme"];
            const labelIdx = Math.min(Math.floor(envVal / 25), envLabels.length - 1);
            document.getElementById('val-env').innerText = envLabels[labelIdx];

            // Atualiza telemetria calculada
            const samsEff = Math.min(94.2, 50 + (magField * 20) + (pztVoltage * 1.5)).toFixed(1);
            const mrTime = Math.max(0.9, 1.8 - (magField * 0.5)).toFixed(2);
            document.getElementById('tel-sams').innerText = `${samsEff}%`;
            document.getElementById('tel-mr').innerText = `${mrTime} ms`;
        }

        // DISPARO DE AMEAÇAS
        function triggerThreat(type) {
            threatState.active = true;
            threatState.type = type;
            threatState.timer = 100; // 100 frames
            threatState.x = canvas.width / 2 + (Math.random() * 200 - 100);
            threatState.y = canvas.height / 2 + (Math.random() * 200 - 100);

            if(type === 'impact') {
                addLog("ALERTA: Impacto Balístico / Meteoro detectado! Camadas S.A.M.S. ativadas.", "text-hudOrange");
            } else if(type === 'pressure') {
                addLog("ALERTA: Surto de Pressão Hiperbárica / Sismo! Atuadores PZT em compensação.", "text-blue-400");
            } else if(type === 'emp') {
                addLog("ALERTA: Ataque EMP / Jamming! Rede SROS em modo óptico contínuo imune.", "text-amber-400");
            } else if(type === 'bio') {
                addLog("ALERTA: Bioaerossol Nocivo detectado! Mapeamento CHONPS isolando eclusas.", "text-emerald-400");
            }
        }

        // ENGINE DE DESENHO CANVAS 2D
        let frameCount = 0;

        function animate() {
            frameCount++;
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            const cx = canvas.width / 2;
            const cy = canvas.height / 2;

            // 1. DESENHO DO AMBIENTE DE FUNGOS / ESPAÇO / ÁGUA
            drawEnvironment(cx, cy);

            // 2. DESENHO DAS REDES ÓPTICAS SROS (LASER BEAMS)
            drawSROSNetwork(cx, cy);

            // 3. DESENHO DO VEÍCULO / ESTRUTURA CENTRAL COM ARMADURA MULTICAMADAS S.A.M.S.
            drawSAMSShell(cx, cy);

            // 4. DESENHO DE VARREDURA DE BIOSSEGURANÇA CHONPS
            drawCHONPSScanner(cx, cy);

            // 5. PROCESSAMENTO E RENDERIZAÇÃO DE AMEAÇAS ATIVAS
            if(threatState.active) {
                renderThreatEffect(cx, cy);
                threatState.timer--;
                if(threatState.timer <= 0) {
                    threatState.active = false;
                    addLog("Ameaça neutralizada com 100% de integridade mantida.", "text-emerald-400");
                }
            }

            // 6. OVERLAY GRÁFICO DE TELEMETRIA EM TEMPO REAL NO CANVAS
            drawCanvasTelemetry();

            requestAnimationFrame(animate);
        }

        // FUNÇÃO: AMBIENTE
        function drawEnvironment(cx, cy) {
            // Estrelas ou Partículas de fundo
            if(particles.length < 50) {
                particles.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    size: Math.random() * 2,
                    speed: Math.random() * 1.5 + 0.5
                });
            }

            ctx.fillStyle = "rgba(255, 255, 255, 0.4)";
            particles.forEach(p => {
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fill();
                p.x -= p.speed * (envVal / 50 + 0.2);
                if(p.x < 0) p.x = canvas.width;
            });

            // Grid tático de fundo
            ctx.strokeStyle = "rgba(15, 23, 42, 0.5)";
            ctx.lineWidth = 1;
            const gridSize = 40;
            for(let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            for(let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }
        }

        // FUNÇÃO: REDE SROS LASER
        function drawSROSNetwork(cx, cy) {
            ctx.save();
            ctx.strokeStyle = threatState.type === 'emp' ? "#f59e0b" : "#10b981"; // Amarelo se EMP, Verde Nominal
            ctx.lineWidth = 2;
            ctx.setLineDash([8, 4]);

            // Nós de satélites repetidores em volta
            const satNodes = [
                {x: cx - 280, y: cy - 180},
                {x: cx + 280, y: cy - 180},
                {x: cx + 280, y: cy + 180},
                {x: cx - 280, y: cy + 180}
            ];

            satNodes.forEach((node, idx) => {
                // Desenha Nó
                ctx.fillStyle = "#06b6d4";
                ctx.beginPath();
                ctx.arc(node.x, node.y, 6, 0, Math.PI * 2);
                ctx.fill();

                // Feixe Laser SROS para a estrutura central
                ctx.beginPath();
                ctx.moveTo(node.x, node.y);
                ctx.lineTo(cx, cy);
                ctx.stroke();

                // Pulso de dados navegando no laser
                const pulsePos = (frameCount * 0.03 + idx * 0.25) % 1;
                const px = node.x + (cx - node.x) * pulsePos;
                const py = node.y + (cy - node.y) * pulsePos;
                ctx.fillStyle = "#34d399";
                ctx.beginPath();
                ctx.arc(px, py, 3, 0, Math.PI * 2);
                ctx.fill();
            });

            ctx.restore();
        }

        // FUNÇÃO: ESTRUTURA E ARMAÇÃO MULTICAMADAS S.A.M.S.
        function drawSAMSShell(cx, cy) {
            ctx.save();

            const radius = 110;
            const layers = [
                { name: "C1: Titânio Ti-6Al-4V", color: "#94a3b8", width: 14 },
                { name: "C2: Fibra Óptica BOTDA", color: "#06b6d4", width: 6 },
                { name: "C3: Fluido MR (MRF-140CG)", color: magField > 0.5 ? "#f97316" : "#78350f", width: 10 },
                { name: "C4: Piezoelétrico PZT-8", color: "#a855f7", width: 8 },
                { name: "C5: Carbono T800", color: "#334155", width: 6 }
            ];

            let currentR = radius;

            // Animação de pulso piezoelétrico e fluido MR sob estresse
            const pulse = Math.sin(frameCount * 0.1) * (pztVoltage / 5);

            // Renderiza Camadas de fora para dentro
            layers.forEach((layer, i) => {
                ctx.beginPath();
                ctx.arc(cx, cy, currentR + (i === 3 ? pulse : 0), 0, Math.PI * 2);
                ctx.strokeStyle = layer.color;
                ctx.lineWidth = layer.width;
                
                // Se for Fluido MR, desenha partículas solidificadas sob campo B
                if(i === 2 && magField > 0.3) {
                    ctx.setLineDash([4, 2]); // Estrutura reticulada
                } else {
                    ctx.setLineDash([]);
                }

                ctx.stroke();

                currentR -= layer.width;
            });

            // Núcleo / Habitáculo Estanque Central
            ctx.beginPath();
            ctx.arc(cx, cy, currentR, 0, Math.PI * 2);
            ctx.fillStyle = "#0f172a";
            ctx.fill();
            ctx.strokeStyle = "#06b6d4";
            ctx.lineWidth = 2;
            ctx.stroke();

            // Ícone representativo do Módulo no centro
            ctx.fillStyle = "#e2e8f0";
            ctx.font = "12px Courier New";
            ctx.textAlign = "center";
            ctx.fillText(modulesData[activeModule].name.split(" ")[0], cx, cy - 5);
            ctx.fillStyle = "#06b6d4";
            ctx.font = "9px Courier New";
            ctx.fillText(modulesData[activeModule].env, cx, cy + 10);

            ctx.restore();
        }

        // FUNÇÃO: BIOSSEGURANÇA CHONPS (ESCANEAR E REJEITAR MOLECULAS VIRAL)
        function drawCHONPSScanner(cx, cy) {
            ctx.save();

            // Anel de Varredura Circular Molecular
            const scanR = 145 + Math.sin(frameCount * 0.05) * 10;
            ctx.strokeStyle = "rgba(16, 185, 129, 0.4)";
            ctx.lineWidth = 1;
            ctx.beginPath();
            ctx.arc(cx, cy, scanR, 0, Math.PI * 2);
            ctx.stroke();

            // Gerar partículas de Bioaerossóis CHONPS se não houver
            if(bioParticles.length < 12) {
                const angle = Math.random() * Math.PI * 2;
                bioParticles.push({
                    x: cx + Math.cos(angle) * (scanR + 60),
                    y: cy + Math.sin(angle) * (scanR + 60),
                    targetX: cx,
                    targetY: cy,
                    status: 'active' // 'active' ou 'rejected'
                });
            }

            // Renderizar e Filtrar bioaerossóis
            bioParticles.forEach((p, idx) => {
                const dist = Math.hypot(p.x - cx, p.y - cy);

                // Barreira de Exclusão CHONPS no Raio da Blindagem
                if(dist < scanR && p.status === 'active') {
                    p.status = 'rejected';
                }

                if(p.status === 'rejected') {
                    // Repelido para fora
                    p.x += (p.x - cx) * 0.05;
                    p.y += (p.y - cy) * 0.05;
                    ctx.fillStyle = "#ef4444"; // Vermelho neutralizado
                } else {
                    // Avançando
                    p.x += (cx - p.x) * 0.01;
                    p.y += (cy - p.y) * 0.01;
                    ctx.fillStyle = "#10b981"; // Verde rastreado
                }

                ctx.beginPath();
                ctx.arc(p.x, p.y, 3, 0, Math.PI * 2);
                ctx.fill();

                // Rótulo CHONPS em algumas partículas
                if(idx % 4 === 0) {
                    ctx.fillStyle = "rgba(255,255,255,0.6)";
                    ctx.font = "8px Courier New";
                    ctx.fillText(p.status === 'rejected' ? 'ISOLADO' : 'CHONPS', p.x + 5, p.y);
                }
            });

            // Limpa partículas repelidas para longe
            bioParticles = bioParticles.filter(p => Math.hypot(p.x - cx, p.y - cy) < scanR + 100);

            ctx.restore();
        }

        // RENDERIZAÇÃO DE EFEITO DE AMEAÇAS (IMPACTO, EMP, PRESSÃO, BIO)
        function renderThreatEffect(cx, cy) {
            ctx.save();
            if(threatState.type === 'impact') {
                // Projétil / Meteoro colidindo na blindagem C1
                ctx.strokeStyle = "#ef4444";
                ctx.lineWidth = 4;
                ctx.beginPath();
                ctx.moveTo(threatState.x - 50, threatState.y - 50);
                ctx.lineTo(cx + 80, cy - 40);
                ctx.stroke();

                // Flash de dissipação piezoelétrica
                ctx.fillStyle = "rgba(249, 115, 22, 0.5)";
                ctx.beginPath();
                ctx.arc(cx + 80, cy - 40, 25, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = "#ffffff";
                ctx.font = "10px Courier New";
                ctx.fillText("ENERGIA ABSORVIDA: 91.8%", cx + 90, cy - 50);

            } else if(threatState.type === 'pressure') {
                // Ondas de Choque Isométricas
                ctx.strokeStyle = "rgba(59, 130, 246, 0.8)";
                ctx.lineWidth = 3;
                const rWave = (frameCount % 30) * 6;
                ctx.beginPath();
                ctx.arc(cx, cy, 100 + rWave, 0, Math.PI * 2);
                ctx.stroke();

                ctx.fillStyle = "#60a5fa";
                ctx.font = "10px Courier New";
                ctx.fillText("COMPENSAÇÃO PZT: 0.21mm (OK)", cx - 60, cy - 130);

            } else if(threatState.type === 'emp') {
                // Pulso Eletromagnético
                ctx.strokeStyle = "rgba(245, 158, 11, 0.9)";
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(cx, cy, 160, 0, Math.PI * 2);
                ctx.stroke();

                ctx.fillStyle = "#fbbf24";
                ctx.font = "10px Courier New";
                ctx.fillText("SROS: IMPRESCINDÍVEL / IMUNE A JAMMING", cx - 90, cy + 145);

            } else if(threatState.type === 'bio') {
                // Pulso de Descontaminação por Exclusão
                ctx.fillStyle = "rgba(16, 185, 129, 0.2)";
                ctx.beginPath();
                ctx.arc(cx, cy, 150, 0, Math.PI * 2);
                ctx.fill();

                ctx.fillStyle = "#34d399";
                ctx.font = "10px Courier New";
                ctx.fillText("ECLUSAS RETICULADAS EM 0.1ms", cx - 70, cy + 130);
            }
            ctx.restore();
        }

        // DESENHO DA TELEMETRIA GRÁFICA NO CANVAS (CANVAS OVERLAY)
        function drawCanvasTelemetry() {
            ctx.save();
            ctx.fillStyle = "rgba(15, 23, 42, 0.8)";
            ctx.fillRect(15, 15, 220, 95);
            ctx.strokeStyle = "#1e293b";
            ctx.strokeRect(15, 15, 220, 95);

            ctx.fillStyle = "#06b6d4";
            ctx.font = "10px Courier New";
            ctx.fillText("GRÁFICO DE CANCELAMENTO ATIVO", 25, 30);

            // Gráfico em tempo real de senoide compensada
            ctx.strokeStyle = "#ef4444"; // Carga Sem S.A.M.S. (Vermelho)
            ctx.lineWidth = 1;
            ctx.beginPath();
            for(let x = 0; x < 190; x += 5) {
                const y = Math.sin((frameCount + x) * 0.1) * 20;
                if(x === 0) ctx.moveTo(25 + x, 65 + y);
                else ctx.lineTo(25 + x, 65 + y);
            }
            ctx.stroke();

            ctx.strokeStyle = "#10b981"; // Carga Com S.A.M.S. (Verde Atenuado)
            ctx.lineWidth = 2;
            ctx.beginPath();
            for(let x = 0; x < 190; x += 5) {
                const y = Math.sin((frameCount + x) * 0.1) * 2; // Atenuado 94%
                if(x === 0) ctx.moveTo(25 + x, 65 + y);
                else ctx.lineTo(25 + x, 65 + y);
            }
            ctx.stroke();

            ctx.fillStyle = "#94a3b8";
            ctx.font = "8px Courier New";
            ctx.fillText("— Passivo (Sem SAMS)  — Ativo (Com SAMS)", 25, 98);

            ctx.restore();
        }

        // INICIALIZAÇÃO E START LOOP
        window.onload = function() {
            selectModule(0);
            updateSliders();
            animate();
            addLog("Simulador de Engenharia Inicializado com Sucesso.", "text-emerald-400");
        };
    </script>
</body>
</html>
