<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>MateAventuras - El Camino de la Montaña</title>
    <style>
        :root {
            --primary-gold: #fef08a;
            --primary-green: #22c55e;
            --primary-green-dark: #15803d;
            --accent-orange: #f97316;
            --glass-bg: rgba(15, 23, 42, 0.55);
            --glass-border: rgba(255, 255, 255, 0.25);
            --radius-xl: 32px;
            --radius-lg: 22px;
            --radius-pill: 50px;
        }

        * {
            box-sizing: border-box;
            user-select: none;
            font-family: 'Segoe UI', Roboto, -apple-system, sans-serif;
            margin: 0;
            padding: 0;
        }

        /* Fondo de la pantalla con la imagen de la montaña */
        html, body {
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            background: url('fondo.jpg') center center / cover no-repeat fixed;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #ffffff;
        }

        /* ----------------------------------------------------
           PANTALLA DE INICIO (MODAL BIENVENIDA)
        ---------------------------------------------------- */
        .welcome-overlay {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(8, 15, 8, 0.7);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            transition: opacity 0.4s ease, visibility 0.4s ease;
        }

        .welcome-overlay.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        .welcome-card {
            background: linear-gradient(145deg, rgba(30, 41, 59, 0.92), rgba(15, 23, 42, 0.96));
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: var(--radius-xl);
            box-shadow: 0 30px 70px rgba(0, 0, 0, 0.8), 0 0 30px rgba(34, 197, 94, 0.3);
            padding: clamp(20px, 4vh, 35px) clamp(20px, 4vw, 40px);
            max-width: 650px;
            width: 95%;
            text-align: center;
            animation: cardPop 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        @keyframes cardPop {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }

        .welcome-title {
            font-size: clamp(1.4rem, 3.8vh, 2.2rem);
            color: var(--primary-gold);
            text-shadow: 0 3px 10px rgba(0,0,0,0.8);
            margin-bottom: 6px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .welcome-subtitle {
            font-size: clamp(0.85rem, 2vh, 1.05rem);
            color: #cbd5e1;
            margin-bottom: 20px;
            font-weight: 500;
        }

        .instructions-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 25px;
            text-align: left;
        }

        .instruction-item {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: var(--radius-lg);
            padding: 10px 16px;
            display: flex;
            align-items: center;
            gap: 14px;
            font-size: clamp(0.85rem, 2vh, 1rem);
            color: #f1f5f9;
        }

        .step-badge {
            width: 36px;
            height: 36px;
            min-width: 36px;
            border-radius: 50%;
            background: linear-gradient(135deg, #22c55e, #15803d);
            border: 2px solid #86efac;
            color: white;
            font-weight: 900;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.1rem;
            box-shadow: 0 4px 10px rgba(34, 197, 94, 0.4);
        }

        .start-btn {
            background: linear-gradient(135deg, #22c55e, #16a34a);
            color: white;
            border: 2px solid #86efac;
            border-radius: var(--radius-pill);
            padding: 12px 36px;
            font-size: clamp(1.1rem, 2.5vh, 1.3rem);
            font-weight: 800;
            cursor: pointer;
            box-shadow: 0 8px 25px rgba(34, 197, 94, 0.5);
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .start-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 12px 30px rgba(34, 197, 94, 0.7);
        }

        /* ----------------------------------------------------
           CONTENEDOR PRINCIPAL DEL JUEGO (DISEÑO CURVO)
        ---------------------------------------------------- */
        .game-container {
            width: 98vw;
            height: 96vh;
            max-width: 1100px;
            max-height: 96vh;
            background: var(--glass-bg);
            border: 2px solid var(--glass-border);
            border-radius: var(--radius-xl);
            box-shadow: 0 25px 60px rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            padding: 1.2vh 2vw;
            position: relative;
        }

        /* Header con curvas finas */
        .header-bar {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
        }

        h1 {
            font-size: clamp(1.1rem, 2.8vh, 1.7rem);
            color: var(--primary-gold);
            text-shadow: 0 3px 10px rgba(0,0,0,0.9);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .mode-selector {
            display: flex;
            gap: 8px;
            background: rgba(0, 0, 0, 0.3);
            padding: 4px;
            border-radius: var(--radius-pill);
            border: 1px solid rgba(255, 255, 255, 0.15);
        }

        .mode-btn {
            padding: 0.5vh 1.2vw;
            border: none;
            border-radius: var(--radius-pill);
            background: transparent;
            color: #d1d5db;
            font-weight: bold;
            font-size: clamp(0.75rem, 1.8vh, 0.95rem);
            cursor: pointer;
            transition: all 0.25s ease;
        }

        .mode-btn.active {
            background: linear-gradient(135deg, #22c55e, #15803d);
            color: white;
            box-shadow: 0 4px 12px rgba(34, 197, 94, 0.5);
        }

        .stats-bar {
            display: flex;
            gap: 15px;
            background: rgba(0,0,0,0.4);
            padding: 0.6vh 1.2vw;
            border-radius: var(--radius-pill);
            font-size: clamp(0.85rem, 2vh, 1.05rem);
            font-weight: bold;
            border: 1px solid rgba(255,255,255,0.2);
            text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        /* Tarjeta Curva de Operación */
        .operation-card {
            width: 100%;
            background: linear-gradient(135deg, rgba(67, 34, 12, 0.8), rgba(30, 15, 5, 0.85));
            border: 2px solid #b45309;
            border-radius: var(--radius-lg);
            padding: 0.8vh 1vw;
            text-align: center;
            box-shadow: 0 8px 20px rgba(0,0,0,0.5);
            backdrop-filter: blur(6px);
        }

        .operation-text {
            font-size: clamp(2.2rem, 5.2vh, 3.4rem);
            font-weight: 900;
            letter-spacing: 3px;
            color: #fef08a;
            text-shadow: 0 3px 8px rgba(0,0,0,0.9);
            line-height: 1.1;
        }

        .instruction-box {
            display: flex;
            justify-content: center;
            gap: 15px;
            font-size: clamp(0.75rem, 1.8vh, 0.95rem);
            font-weight: 700;
            margin-top: 2px;
        }

        .step-1 { color: #60a5fa; text-shadow: 0 2px 4px rgba(0,0,0,0.8); }
        .step-2 { color: #4ade80; text-shadow: 0 2px 4px rgba(0,0,0,0.8); }

        /* Contenedor del Escenario integrado */
        .number-line-container {
            width: 100%;
            flex: 1 1 auto;
            max-height: 48vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            background: rgba(0, 0, 0, 0.22);
            border-radius: var(--radius-lg);
            border: 1px solid rgba(255,255,255,0.2);
            box-shadow: inset 0 0 25px rgba(0,0,0,0.5);
            overflow: hidden;
        }

        svg {
            width: 100%;
            height: 100%;
            display: block;
        }

        /* Animación para resaltar al dinosaurio */
        @keyframes dinoGlow {
            0%, 100% { filter: drop-shadow(0 0 8px rgba(34, 197, 94, 0.9)); transform: translateY(0); }
            50% { filter: drop-shadow(0 0 18px rgba(250, 204, 21, 1)); transform: translateY(-5px); }
        }

        .dino-interactive {
            cursor: pointer;
            animation: dinoGlow 1.5s infinite ease-in-out;
        }

        /* Teclado de números curvo */
        .answers-grid {
            display: flex;
            justify-content: center;
            gap: clamp(4px, 1vw, 10px);
            width: 100%;
        }

        .num-btn {
            width: clamp(32px, 5.5vh, 52px);
            height: clamp(32px, 5.5vh, 52px);
            border-radius: 50%;
            border: 2px solid #b45309;
            background: linear-gradient(145deg, #78350f, #451a03);
            font-size: clamp(0.9rem, 2.2vh, 1.3rem);
            font-weight: 800;
            color: #fef08a;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 6px 12px rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .num-btn:hover:not(:disabled) {
            transform: translateY(-3px) scale(1.12);
            border-color: #86efac;
            background: linear-gradient(145deg, #15803d, #166534);
            color: #ffffff;
            box-shadow: 0 0 18px rgba(34, 197, 94, 0.8);
        }

        .num-btn:disabled {
            opacity: 0.35;
            cursor: not-allowed;
        }

        /* Mensajes de resultado y botones */
        .footer-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            min-height: 5.5vh;
            width: 100%;
        }

        .feedback-message {
            font-size: clamp(1rem, 2.6vh, 1.5rem);
            font-weight: 800;
            text-shadow: 0 2px 8px rgba(0,0,0,0.9);
        }

        .feedback-message.correct { color: #4ade80; }
        .feedback-message.incorrect { color: #f87171; }

        .next-btn {
            padding: 0.8vh 2.2vw;
            font-size: clamp(0.9rem, 2.2vh, 1.2rem);
            font-weight: 800;
            background: linear-gradient(135deg, #22c55e, #16a34a);
            color: white;
            border: 2px solid #86efac;
            border-radius: var(--radius-pill);
            cursor: pointer;
            box-shadow: 0 6px 20px rgba(34, 197, 94, 0.5);
            display: none;
            transition: transform 0.2s;
        }

        .next-btn:hover {
            transform: scale(1.06);
            background: linear-gradient(135deg, #16a34a, #15803d);
        }

        canvas#confetti {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            pointer-events: none;
            z-index: 10;
        }
    </style>
</head>
<body>

<!-- PANTALLA DE INICIO (MODAL) -->
<div class="welcome-overlay" id="welcome-screen">
    <div class="welcome-card">
        <h2 class="welcome-title">🦖 El Camino de la Montaña</h2>
        <p class="welcome-subtitle">¡Ayuda al dinosaurio explorador a calcular la ruta correcta!</p>
        
        <div class="instructions-container">
            <div class="instruction-item">
                <div class="step-badge">1</div>
                <div>Mira la operación en el cartel superior (por ejemplo: <b>3 + 4</b>).</div>
            </div>
            <div class="instruction-item">
                <div class="step-badge">2</div>
                <div>El dinosaurio se colocará en la piedra del primer número (<b>3</b>).</div>
            </div>
            <div class="instruction-item">
                <div class="step-badge">3</div>
                <div><b>Toca al dinosaurio 🦖</b> para hacerlo dar los pasos correspondientes (<b>4 pasos</b>).</div>
            </div>
            <div class="instruction-item">
                <div class="step-badge">4</div>
                <div>¡Haz clic en la piedra final con el resultado correcto para ganar estrellas! ⭐</div>
            </div>
        </div>

        <button class="start-btn" onclick="startGame()">¡Comenzar Aventura! 🗺️</button>
    </div>
</div>

<!-- JUEGO PRINCIPAL -->
<div class="game-container">
    <canvas id="confetti"></canvas>

    <!-- Encabezado -->
    <div class="header-bar">
        <h1>🦖 MateAventuras</h1>
        
        <div class="mode-selector">
            <button class="mode-btn active" onclick="setMode('suma')">➕ Suma</button>
            <button class="mode-btn" onclick="setMode('resta')">➖ Resta</button>
            <button class="mode-btn" onclick="setMode('mixto')">🔀 Mixto</button>
        </div>

        <div class="stats-bar">
            <span>⭐ <span id="score">0</span></span>
            <span>🔥 <span id="streak">0</span></span>
        </div>
    </div>

    <!-- Tarjeta de Operación -->
    <div class="operation-card">
        <div class="operation-text" id="operation-display">3 + 5 = ?</div>
        <div class="instruction-box">
            <span class="step-1" id="instruction-step1">1️⃣ Toca al dinosaurio 🦖 para avanzar.</span>
            <span class="step-2">2️⃣ ¡Toca la piedra con la respuesta!</span>
        </div>
    </div>

    <!-- Escenario SVG con Sendero Curvo -->
    <div class="number-line-container">
        <svg id="number-line-svg" viewBox="0 0 900 230" preserveAspectRatio="xMidYMid meet">
            <defs>
                <filter id="shadow3d" x="-20%" y="-20%" width="140%" height="140%">
                    <feDropShadow dx="0" dy="6" stdDeviation="4" flood-color="#000000" flood-opacity="0.8"/>
                </filter>
                
                <linearGradient id="stoneGrad" x1="0%" y1="0%" x2="0%" y2="100%">
                    <stop offset="0%" stop-color="#ffffff"/>
                    <stop offset="50%" stop-color="#9ca3af"/>
                    <stop offset="100%" stop-color="#374151"/>
                </linearGradient>

                <linearGradient id="stoneActiveGrad" x1="0%" y1="0%" x2="0%" y2="100%">
                    <stop offset="0%" stop-color="#fef08a"/>
                    <stop offset="50%" stop-color="#eab308"/>
                    <stop offset="100%" stop-color="#854d0e"/>
                </linearGradient>
            </defs>
        </svg>
    </div>

    <!-- Teclado de números -->
    <div class="answers-grid" id="answers-grid"></div>

    <!-- Footer Controls -->
    <div class="footer-controls">
        <div class="feedback-message" id="feedback"></div>
        <button class="next-btn" id="next-btn" onclick="generateNewProblem()">¡Siguiente Camino! ➡️</button>
    </div>
</div>

<script>
    // --- ESTADO DEL JUEGO ---
    let gameMode = 'suma';
    let currentOp = { num1: 3, op: '+', num2: 5, result: 8 };
    let currentPos = 3;        
    let jumpsMade = 0;         
    let jumpsHistory = [];     
    let isFinished = false;
    let score = 0;
    let streak = 0;

    const startX = 70;
    const endX = 830;
    const lineY = 160;
    const stepWidth = (endX - startX) / 10;

    // --- SÍNTESIS DE VOZ Y SONIDO (Web Audio API) ---
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    function startGame() {
        if (audioCtx.state === 'suspended') {
            audioCtx.resume();
        }
        document.getElementById('welcome-screen').classList.add('hidden');
        speakText("¡Bienvenido a la aventura matemática!");
    }

    function speakText(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'es-ES';
            utterance.rate = 0.95;
            utterance.pitch = 1.1;
            window.speechSynthesis.speak(utterance);
        }
    }

    function playTone(freq, type, duration, delay = 0) {
        setTimeout(() => {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = type;
            osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
            gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            osc.start();
            osc.stop(audioCtx.currentTime + duration);
        }, delay);
    }

    function playStepSound() {
        playTone(280, 'triangle', 0.08);
        setTimeout(() => playTone(420, 'sine', 0.1), 40);
    }

    function playWinSound() {
        const notes = [261.63, 329.63, 392.00, 523.25];
        notes.forEach((note, i) => playTone(note, 'triangle', 0.22, i * 100));
    }

    function playErrorSound() {
        playTone(200, 'sawtooth', 0.2);
        playTone(140, 'sawtooth', 0.28, 140);
    }

    // --- LÓGICA DEL JUEGO ---
    function setMode(mode) {
        gameMode = mode;
        document.querySelectorAll('.mode-btn').forEach(btn => {
            btn.classList.remove('active');
            if (btn.textContent.toLowerCase().includes(mode)) btn.classList.add('active');
        });
        generateNewProblem();
    }

    function generateNewProblem() {
        isFinished = false;
        jumpsMade = 0;
        jumpsHistory = [];

        document.getElementById('feedback').textContent = '';
        document.getElementById('feedback').className = 'feedback-message';
        document.getElementById('next-btn').style.display = 'none';

        let chosenOp = gameMode;
        if (gameMode === 'mixto') chosenOp = Math.random() > 0.5 ? 'suma' : 'resta';

        let a, b, res;
        if (chosenOp === 'suma') {
            a = Math.floor(Math.random() * 8) + 1; 
            b = Math.floor(Math.random() * (10 - a)) + 1; 
            res = a + b;
            currentOp = { num1: a, op: '+', num2: b, result: res };
        } else {
            a = Math.floor(Math.random() * 8) + 2; 
            b = Math.floor(Math.random() * a) + 1; 
            res = a - b;
            currentOp = { num1: a, op: '-', num2: b, result: res };
        }

        currentPos = currentOp.num1;

        document.getElementById('operation-display').textContent = `${currentOp.num1} ${currentOp.op} ${currentOp.num2} = ?`;
        document.getElementById('instruction-step1').textContent = 
            `1️⃣ Toca al dinosaurio 🦖 para dar ${currentOp.num2} paso(s) (${currentOp.op === '+' ? 'adelante ➡️' : 'atrás ⬅️'}).`;

        renderNumberLine();
        enableAnswerButtons(true);
    }

    function makeDinoStep() {
        if (isFinished) return;

        const direction = currentOp.op === '+' ? 1 : -1;
        const nextPos = currentPos + direction;

        if (nextPos < 0 || nextPos > 10) return;

        jumpsHistory.push({ from: currentPos, to: nextPos });
        currentPos = nextPos;
        jumpsMade++;

        playStepSound();
        renderNumberLine();
    }

    function submitAnswer(selectedNum) {
        if (isFinished) return;
        isFinished = true;
        enableAnswerButtons(false);

        const feedback = document.getElementById('feedback');

        if (selectedNum === currentOp.result) {
            feedback.textContent = `¡CORRECTO! 🎉 ${currentOp.num1} ${currentOp.op} ${currentOp.num2} = ${currentOp.result}`;
            feedback.className = 'feedback-message correct';
            
            playWinSound();
            speakText("¡Excelente! ¡Respuesta correcta!");
            launchConfetti();

            score += 10;
            streak += 1;
            document.getElementById('score').textContent = score;
            document.getElementById('streak').textContent = streak;
        } else {
            feedback.textContent = `¡Casi! Elegiste el ${selectedNum}, pero el resultado es ${currentOp.result}.`;
            feedback.className = 'feedback-message incorrect';
            
            playErrorSound();
            speakText("¡Casi! Inténtalo de nuevo en el siguiente nivel.");
            streak = 0;
            document.getElementById('streak').textContent = streak;
        }

        renderNumberLine();
        document.getElementById('next-btn').style.display = 'inline-block';
    }

    function getXPos(num) {
        return startX + num * stepWidth;
    }

    // --- DIBUJO DEL ESCENARIO SVG ---
    function renderNumberLine() {
        const svg = document.getElementById('number-line-svg');
        svg.innerHTML = svg.querySelector('defs').outerHTML; 

        // 1. Sendero Curvo
        const pathBg = document.createElementNS('http://www.w3.org/2000/svg', 'path');
        const dPath = `M 30,${lineY + 5} Q 450,${lineY + 15} 870,${lineY + 5}`;
        
        pathBg.setAttribute('d', dPath);
        pathBg.setAttribute('fill', 'none');
        pathBg.setAttribute('stroke', '#78350f');
        pathBg.setAttribute('stroke-width', '28');
        pathBg.setAttribute('stroke-linecap', 'round');
        pathBg.setAttribute('filter', 'url(#shadow3d)');
        svg.appendChild(pathBg);

        const pathTexture = document.createElementNS('http://www.w3.org/2000/svg', 'path');
        pathTexture.setAttribute('d', dPath);
        pathTexture.setAttribute('fill', 'none');
        pathTexture.setAttribute('stroke', '#d97706');
        pathTexture.setAttribute('stroke-width', '20');
        pathTexture.setAttribute('stroke-linecap', 'round');
        svg.appendChild(pathTexture);

        // 2. Rocas 3D con Números
        for (let i = 0; i <= 10; i++) {
            const x = getXPos(i);

            const rockShadow = document.createElementNS('http://www.w3.org/2000/svg', 'ellipse');
            rockShadow.setAttribute('cx', x);
            rockShadow.setAttribute('cy', lineY + 32);
            rockShadow.setAttribute('rx', '18');
            rockShadow.setAttribute('ry', '8');
            rockShadow.setAttribute('fill', 'rgba(0,0,0,0.6)');
            svg.appendChild(rockShadow);

            const rock = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
            rock.setAttribute('cx', x);
            rock.setAttribute('cy', lineY + 28);
            rock.setAttribute('r', '17');
            
            let grad = 'url(#stoneGrad)';
            let stroke = '#1f2937';

            if (i === currentOp.num1) {
                grad = 'url(#stoneActiveGrad)';
                stroke = '#b45309';
            }
            if (i === currentPos && jumpsMade > 0) {
                stroke = '#22c55e';
            }

            rock.setAttribute('fill', grad);
            rock.setAttribute('stroke', stroke);
            rock.setAttribute('stroke-width', '2.5');
            rock.style.cursor = isFinished ? 'default' : 'pointer';
            rock.onclick = () => submitAnswer(i);
            svg.appendChild(rock);

            const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
            text.setAttribute('x', x);
            text.setAttribute('y', lineY + 34);
            text.setAttribute('text-anchor', 'middle');
            text.setAttribute('font-size', '16');
            text.setAttribute('font-weight', '900');
            text.setAttribute('fill', '#0f172a');
            text.style.cursor = isFinished ? 'default' : 'pointer';
            text.textContent = i;
            text.onclick = () => submitAnswer(i);
            svg.appendChild(text);
        }

        // 3. Arcos de Pasos
        jumpsHistory.forEach((jump, index) => {
            const x1 = getXPos(jump.from);
            const x2 = getXPos(jump.to);
            const midX = (x1 + x2) / 2;
            const yArc = lineY - 60;

            const arcPath = document.createElementNS('http://www.w3.org/2000/svg', 'path');
            const d = `M ${x1} ${lineY + 10} Q ${midX} ${yArc} ${x2} ${lineY + 10}`;
            arcPath.setAttribute('d', d);
            arcPath.setAttribute('fill', 'none');
            arcPath.setAttribute('stroke', currentOp.op === '+' ? '#60a5fa' : '#f97316');
            arcPath.setAttribute('stroke-width', '4');
            arcPath.setAttribute('stroke-dasharray', '5 3');
            arcPath.setAttribute('filter', 'url(#shadow3d)');
            svg.appendChild(arcPath);

            const label = document.createElementNS('http://www.w3.org/2000/svg', 'text');
            label.setAttribute('x', midX);
            label.setAttribute('y', yArc + 12);
            label.setAttribute('text-anchor', 'middle');
            label.setAttribute('font-size', '13');
            label.setAttribute('font-weight', 'bold');
            label.setAttribute('fill', '#fef08a');
            label.textContent = index + 1;
            svg.appendChild(label);
        });

        // 4. DINOSAURIO EXPLORADOR
        const charX = getXPos(currentPos);

        const dinoGroup = document.createElementNS('http://www.w3.org/2000/svg', 'g');
        dinoGroup.setAttribute('transform', `translate(${charX}, ${lineY + 12})`);
        
        const animGroup = document.createElementNS('http://www.w3.org/2000/svg', 'g');
        animGroup.setAttribute('class', isFinished ? '' : 'dino-interactive');
        animGroup.onclick = makeDinoStep;

        const scaleX = currentOp.op === '-' ? -1 : 1;
        const dinoContainer = document.createElementNS('http://www.w3.org/2000/svg', 'g');
        dinoContainer.setAttribute('transform', `scale(${scaleX}, 1)`);

        let stepBadge = !isFinished ? `
            <g transform="scale(${scaleX}, 1) translate(0, -115)">
                <rect x="-28" y="-10" width="56" height="22" rx="11" fill="#22c55e" stroke="#86efac" stroke-width="1.5"/>
                <text x="0" y="5" text-anchor="middle" font-size="11" font-weight="bold" fill="#ffffff">¡PASO!</text>
            </g>` : '';

        dinoContainer.innerHTML = `
            <g filter="url(#shadow3d)">
                ${stepBadge}
                
                <ellipse cx="0" cy="-2" rx="24" ry="7" fill="rgba(0,0,0,0.5)"/>

                <image href="dinosaurio.png" x="-60" y="-115" width="120" height="120" 
                       preserveAspectRatio="xMidYMax meet"
                       onerror="this.setAttribute('href', 'dinosaurio.jpg'); this.onerror=null;" />
            </g>
        `;

        animGroup.appendChild(dinoContainer);
        dinoGroup.appendChild(animGroup);
        svg.appendChild(dinoGroup);
    }

    // --- TECLADO DE NÚMEROS INFERIOR ---
    function createAnswerButtons() {
        const grid = document.getElementById('answers-grid');
        grid.innerHTML = '';
        for (let i = 0; i <= 10; i++) {
            const btn = document.createElement('button');
            btn.className = 'num-btn';
            btn.textContent = i;
            btn.onclick = () => submitAnswer(i);
            grid.appendChild(btn);
        }
    }

    function enableAnswerButtons(enable) {
        document.querySelectorAll('.num-btn').forEach(btn => btn.disabled = !enable);
    }

    // --- EFECTO CONFETI ---
    function launchConfetti() {
        const canvas = document.getElementById('confetti');
        const ctx = canvas.getContext('2d');
        canvas.width = canvas.offsetWidth;
        canvas.height = canvas.offsetHeight;

        const particles = [];
        const colors = ['#f43f5e', '#3b82f6', '#10b981', '#f59e0b', '#8b5cf6'];

        for (let i = 0; i < 60; i++) {
            particles.push({
                x: canvas.width / 2,
                y: canvas.height / 2,
                vx: (Math.random() - 0.5) * 12,
                vy: (Math.random() - 0.7) * 12,
                color: colors[Math.floor(Math.random() * colors.length)],
                radius: Math.random() * 5 + 3,
                gravity: 0.22
            });
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            let active = false;

            particles.forEach(p => {
                p.x += p.vx;
                p.y += p.vy;
                p.vy += p.gravity;

                if (p.y < canvas.height) active = true;

                ctx.beginPath();
                ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                ctx.fillStyle = p.color;
                ctx.fill();
            });

            if (active) requestAnimationFrame(animate);
            else ctx.clearRect(0, 0, canvas.width, canvas.height);
        }
        animate();
    }

    // INICIALIZACIÓN
    window.onload = () => {
        createAnswerButtons();
        generateNewProblem();
    };
</script>

</body>
</html>
