# SCT_WD_2
Stopwatch Web Application
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Precision Stopwatch</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700;900&family=JetBrains+Mono:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-primary: #0a0e17;
            --bg-secondary: #151b2b;
            --bg-tertiary: #1e2739;
            --accent-primary: #00d9ff;
            --accent-secondary: #7d5ffe;
            --accent-glow: rgba(0, 217, 255, 0.3);
            --text-primary: #ffffff;
            --text-secondary: #8b95b0;
            --text-muted: #525f7a;
            --border-color: rgba(255, 255, 255, 0.1);
            --success: #00ff88;
            --danger: #ff4757;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'JetBrains Mono', monospace;
            background: var(--bg-primary);
            color: var(--text-primary);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow-x: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: fixed;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: 
                radial-gradient(circle at 20% 50%, rgba(125, 95, 254, 0.15) 0%, transparent 50%),
                radial-gradient(circle at 80% 50%, rgba(0, 217, 255, 0.1) 0%, transparent 50%);
            animation: bgShift 20s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes bgShift {
            0%, 100% { transform: translate(0, 0) rotate(0deg); }
            50% { transform: translate(-5%, -5%) rotate(5deg); }
        }

        .container {
            max-width: 600px;
            width: 100%;
            position: relative;
            z-index: 1;
        }

        .stopwatch-card {
            background: var(--bg-secondary);
            border-radius: 24px;
            padding: 48px 40px;
            box-shadow: 
                0 20px 60px rgba(0, 0, 0, 0.5),
                inset 0 1px 0 rgba(255, 255, 255, 0.05);
            border: 1px solid var(--border-color);
            position: relative;
            overflow: hidden;
        }

        .stopwatch-card::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            height: 2px;
            background: linear-gradient(90deg, 
                transparent,
                var(--accent-primary),
                var(--accent-secondary),
                transparent
            );
            animation: shimmer 3s ease-in-out infinite;
        }

        @keyframes shimmer {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .header {
            text-align: center;
            margin-bottom: 48px;
        }

        .header h1 {
            font-family: 'Orbitron', sans-serif;
            font-size: 24px;
            font-weight: 700;
            letter-spacing: 4px;
            text-transform: uppercase;
            background: linear-gradient(135deg, var(--accent-primary), var(--accent-secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 8px;
        }

        .header p {
            font-size: 12px;
            color: var(--text-muted);
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .display {
            text-align: center;
            margin-bottom: 48px;
            position: relative;
        }

        .time-display {
            font-family: 'Orbitron', monospace;
            font-size: 72px;
            font-weight: 900;
            letter-spacing: 4px;
            color: var(--text-primary);
            text-shadow: 0 0 40px var(--accent-glow);
            transition: all 0.3s ease;
            position: relative;
        }

        .time-display.running {
            color: var(--accent-primary);
            text-shadow: 0 0 60px var(--accent-glow), 0 0 100px var(--accent-glow);
        }

        .milliseconds {
            font-size: 36px;
            color: var(--text-secondary);
            opacity: 0.8;
        }

        .controls {
            display: flex;
            gap: 16px;
            margin-bottom: 32px;
        }

        .btn {
            flex: 1;
            padding: 18px 32px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 14px;
            font-weight: 500;
            letter-spacing: 1px;
            text-transform: uppercase;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            background: var(--bg-tertiary);
            color: var(--text-primary);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.1);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .btn:hover::before {
            width: 300px;
            height: 300px;
        }

        .btn:active {
            transform: scale(0.96);
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--accent-primary), var(--accent-secondary));
            color: white;
            font-weight: 700;
            box-shadow: 0 4px 20px rgba(0, 217, 255, 0.4);
        }

        .btn-primary:hover {
            box-shadow: 0 6px 30px rgba(0, 217, 255, 0.6);
            transform: translateY(-2px);
        }

        .btn-secondary {
            background: var(--bg-tertiary);
            border: 1px solid var(--border-color);
        }

        .btn-secondary:hover {
            border-color: var(--accent-primary);
            box-shadow: 0 4px 20px rgba(0, 217, 255, 0.2);
        }

        .btn-danger {
            background: rgba(255, 71, 87, 0.1);
            border: 1px solid rgba(255, 71, 87, 0.3);
            color: var(--danger);
        }

        .btn-danger:hover {
            background: rgba(255, 71, 87, 0.2);
            border-color: var(--danger);
            box-shadow: 0 4px 20px rgba(255, 71, 87, 0.3);
        }

        .btn:disabled {
            opacity: 0.4;
            cursor: not-allowed;
            transform: none;
        }

        .btn:disabled:hover {
            transform: none;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }

        .laps-section {
            margin-top: 32px;
        }

        .laps-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
            padding-bottom: 12px;
            border-bottom: 1px solid var(--border-color);
        }

        .laps-header h3 {
            font-family: 'Orbitron', sans-serif;
            font-size: 14px;
            font-weight: 700;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--text-secondary);
        }

        .lap-count {
            font-size: 12px;
            color: var(--text-muted);
            background: var(--bg-tertiary);
            padding: 4px 12px;
            border-radius: 20px;
        }

        .laps-list {
            max-height: 300px;
            overflow-y: auto;
            padding-right: 8px;
        }

        .laps-list::-webkit-scrollbar {
            width: 6px;
        }

        .laps-list::-webkit-scrollbar-track {
            background: var(--bg-tertiary);
            border-radius: 3px;
        }

        .laps-list::-webkit-scrollbar-thumb {
            background: var(--accent-primary);
            border-radius: 3px;
        }

        .laps-list::-webkit-scrollbar-thumb:hover {
            background: var(--accent-secondary);
        }

        .lap-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 14px 16px;
            margin-bottom: 8px;
            background: var(--bg-tertiary);
            border-radius: 10px;
            border: 1px solid transparent;
            transition: all 0.3s ease;
            animation: slideIn 0.4s ease;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .lap-item:hover {
            border-color: var(--accent-primary);
            background: rgba(0, 217, 255, 0.05);
        }

        .lap-item.fastest {
            border-color: var(--success);
            background: rgba(0, 255, 136, 0.05);
        }

        .lap-item.slowest {
            border-color: var(--danger);
            background: rgba(255, 71, 87, 0.05);
        }

        .lap-info {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .lap-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 14px;
            font-weight: 700;
            color: var(--text-muted);
            min-width: 60px;
        }

        .lap-time {
            font-family: 'Orbitron', monospace;
            font-size: 18px;
            font-weight: 700;
            letter-spacing: 1px;
        }

        .lap-badge {
            font-size: 10px;
            padding: 4px 8px;
            border-radius: 4px;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .lap-badge.fastest {
            background: rgba(0, 255, 136, 0.2);
            color: var(--success);
        }

        .lap-badge.slowest {
            background: rgba(255, 71, 87, 0.2);
            color: var(--danger);
        }

        .empty-state {
            text-align: center;
            padding: 48px 20px;
            color: var(--text-muted);
        }

        .empty-state svg {
            width: 64px;
            height: 64px;
            margin-bottom: 16px;
            opacity: 0.3;
        }

        .empty-state p {
            font-size: 14px;
            letter-spacing: 1px;
        }

        @media (max-width: 640px) {
            .stopwatch-card {
                padding: 32px 24px;
            }

            .time-display {
                font-size: 56px;
            }

            .milliseconds {
                font-size: 28px;
            }

            .controls {
                flex-direction: column;
            }

            .btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="stopwatch-card">
            <div class="header">
                <h1>Precision Timer</h1>
                <p>Track Every Millisecond</p>
            </div>

            <div class="display">
                <div class="time-display" id="timeDisplay">
                    00:00:00<span class="milliseconds">.000</span>
                </div>
            </div>

            <div class="controls">
                <button class="btn btn-primary" id="startPauseBtn">Start</button>
                <button class="btn btn-secondary" id="lapBtn" disabled>Lap</button>
                <button class="btn btn-danger" id="resetBtn" disabled>Reset</button>
            </div>

            <div class="laps-section" id="lapsSection" style="display: none;">
                <div class="laps-header">
                    <h3>Lap Times</h3>
                    <span class="lap-count" id="lapCount">0 Laps</span>
                </div>
                <div class="laps-list" id="lapsList"></div>
            </div>
        </div>
    </div>

    <script>
        class Stopwatch {
            constructor() {
                this.startTime = 0;
                this.elapsedTime = 0;
                this.timerInterval = null;
                this.isRunning = false;
                this.laps = [];
                this.lastLapTime = 0;

                this.initializeElements();
                this.attachEventListeners();
            }

            initializeElements() {
                this.timeDisplay = document.getElementById('timeDisplay');
                this.startPauseBtn = document.getElementById('startPauseBtn');
                this.lapBtn = document.getElementById('lapBtn');
                this.resetBtn = document.getElementById('resetBtn');
                this.lapsSection = document.getElementById('lapsSection');
                this.lapsList = document.getElementById('lapsList');
                this.lapCount = document.getElementById('lapCount');
            }

            attachEventListeners() {
                this.startPauseBtn.addEventListener('click', () => this.toggleStartPause());
                this.lapBtn.addEventListener('click', () => this.recordLap());
                this.resetBtn.addEventListener('click', () => this.reset());
            }

            toggleStartPause() {
                if (this.isRunning) {
                    this.pause();
                } else {
                    this.start();
                }
            }

            start() {
                this.startTime = Date.now() - this.elapsedTime;
                this.timerInterval = setInterval(() => this.updateDisplay(), 10);
                this.isRunning = true;
                
                this.startPauseBtn.textContent = 'Pause';
                this.startPauseBtn.classList.remove('btn-primary');
                this.startPauseBtn.classList.add('btn-secondary');
                this.lapBtn.disabled = false;
                this.resetBtn.disabled = false;
                this.timeDisplay.classList.add('running');
            }

            pause() {
                clearInterval(this.timerInterval);
                this.isRunning = false;
                
                this.startPauseBtn.textContent = 'Resume';
                this.startPauseBtn.classList.add('btn-primary');
                this.startPauseBtn.classList.remove('btn-secondary');
                this.timeDisplay.classList.remove('running');
            }

            reset() {
                clearInterval(this.timerInterval);
                this.startTime = 0;
                this.elapsedTime = 0;
                this.isRunning = false;
                this.laps = [];
                this.lastLapTime = 0;
                
                this.updateDisplay();
                this.startPauseBtn.textContent = 'Start';
                this.startPauseBtn.classList.add('btn-primary');
                this.startPauseBtn.classList.remove('btn-secondary');
                this.lapBtn.disabled = true;
                this.resetBtn.disabled = true;
                this.timeDisplay.classList.remove('running');
                this.lapsSection.style.display = 'none';
                this.lapsList.innerHTML = '';
            }

            updateDisplay() {
                this.elapsedTime = Date.now() - this.startTime;
                const time = this.formatTime(this.elapsedTime);
                this.timeDisplay.innerHTML = `${time.main}<span class="milliseconds">.${time.ms}</span>`;
            }

            formatTime(ms) {
                const totalSeconds = Math.floor(ms / 1000);
                const hours = Math.floor(totalSeconds / 3600);
                const minutes = Math.floor((totalSeconds % 3600) / 60);
                const seconds = totalSeconds % 60;
                const milliseconds = Math.floor((ms % 1000));

                return {
                    main: `${this.pad(hours)}:${this.pad(minutes)}:${this.pad(seconds)}`,
                    ms: this.pad(milliseconds, 3)
                };
            }

            pad(num, size = 2) {
                return num.toString().padStart(size, '0');
            }

            recordLap() {
                const currentTime = this.elapsedTime;
                const lapTime = currentTime - this.lastLapTime;
                
                this.laps.unshift({
                    number: this.laps.length + 1,
                    time: lapTime,
                    totalTime: currentTime
                });
                
                this.lastLapTime = currentTime;
                this.displayLaps();
            }

            displayLaps() {
                this.lapsSection.style.display = 'block';
                this.lapCount.textContent = `${this.laps.length} ${this.laps.length === 1 ? 'Lap' : 'Laps'}`;
                
                // Find fastest and slowest laps
                const times = this.laps.map(lap => lap.time);
                const fastest = Math.min(...times);
                const slowest = Math.max(...times);
                
                this.lapsList.innerHTML = this.laps.map((lap, index) => {
                    const time = this.formatTime(lap.time);
                    const isFastest = this.laps.length > 1 && lap.time === fastest;
                    const isSlowest = this.laps.length > 1 && lap.time === slowest;
                    
                    let badge = '';
                    let itemClass = 'lap-item';
                    
                    if (isFastest) {
                        badge = '<span class="lap-badge fastest">Fastest</span>';
                        itemClass += ' fastest';
                    } else if (isSlowest) {
                        badge = '<span class="lap-badge slowest">Slowest</span>';
                        itemClass += ' slowest';
                    }
                    
                    return `
                        <div class="${itemClass}">
                            <div class="lap-info">
                                <span class="lap-number">Lap ${lap.number}</span>
                                <span class="lap-time">${time.main}.${time.ms}</span>
                            </div>
                            ${badge}
                        </div>
                    `;
                }).join('');
            }
        }

        // Initialize the stopwatch when the page loads
        document.addEventListener('DOMContentLoaded', () => {
            new Stopwatch();
        });
    </script>
</body>
</html>
