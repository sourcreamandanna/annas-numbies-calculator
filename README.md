<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Timer Widget</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Avenir:wght@300;400;500;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Avenir', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .timer-container {
            background: rgba(251, 253, 212, 0.95); /* Light yellow background */
            border-radius: 24px;
            padding: 32px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            max-width: 400px;
            width: 100%;
            text-align: center;
            transition: all 0.3s ease;
        }
        
        .timer-container:hover {
            transform: translateY(-2px);
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
        }
        
        .timer-title {
            font-size: 24px;
            font-weight: 500;
            color: #333;
            margin-bottom: 24px;
            letter-spacing: -0.5px;
        }
        
        .time-selector {
            display: flex;
            gap: 8px;
            justify-content: center;
            margin-bottom: 24px;
            padding: 8px;
            background: rgba(255, 255, 255, 0.6);
            border-radius: 20px;
            backdrop-filter: blur(5px);
        }
        
        .time-btn {
            padding: 8px 16px;
            border: none;
            border-radius: 12px;
            font-family: inherit;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            background: transparent;
            color: #6c757d;
        }
        
        .time-btn:hover {
            background: rgba(244, 196, 218, 0.3);
            color: #333;
        }
        
        .time-btn.active {
            background: linear-gradient(135deg, #f4c4da, #f8d7e6);
            color: #333;
            box-shadow: 0 2px 8px rgba(244, 196, 218, 0.3);
        }
        
        .time-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .timer-display {
            font-size: 48px;
            font-weight: 300;
            color: #6c757d;
            margin-bottom: 32px;
            font-variant-numeric: tabular-nums;
            letter-spacing: -1px;
            transition: color 0.3s ease;
        }
        
        .timer-display.active {
            color: #f4c4da; /* Light pink when active */
        }
        
        .timer-controls {
            display: flex;
            gap: 16px;
            justify-content: center;
            margin-bottom: 32px;
        }
        
        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 16px;
            font-family: inherit;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
            letter-spacing: -0.2px;
        }
        
        .btn:before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            transition: left 0.5s;
        }
        
        .btn:hover:before {
            left: 100%;
        }
        
        .btn-start {
            background: linear-gradient(135deg, #f4c4da, #f8d7e6); /* Light pink gradient */
            color: #333;
            box-shadow: 0 8px 16px rgba(244, 196, 218, 0.3);
        }
        
        .btn-start:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 24px rgba(244, 196, 218, 0.4);
        }
        
        .btn-pause {
            background: linear-gradient(135deg, #f4c4da, #f8d7e6); /* Light pink gradient */
            color: #333;
            box-shadow: 0 8px 16px rgba(244, 196, 218, 0.3);
        }
        
        .btn-pause:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 24px rgba(244, 196, 218, 0.4);
        }
        
        .btn-resume {
            background: linear-gradient(135deg, #f4c4da, #f8d7e6); /* Light pink gradient */
            color: #333;
            box-shadow: 0 8px 16px rgba(244, 196, 218, 0.3);
        }
        
        .btn-resume:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 24px rgba(244, 196, 218, 0.4);
        }
        
        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }
        
        .sessions-container {
            margin-top: 32px;
        }
        
        .sessions-title {
            font-size: 18px;
            font-weight: 500;
            color: #6c757d;
            margin-bottom: 16px;
            letter-spacing: -0.3px;
        }
        
        .sessions-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(24px, 1fr));
            gap: 8px;
            max-width: 300px;
            margin: 0 auto;
        }
        
        .session-dot {
            width: 24px;
            height: 24px;
            border-radius: 12px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
            border: 2px solid rgba(255, 255, 255, 0.8);
        }
        
        .session-dot:before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            transition: all 0.6s ease;
            transform: translate(-50%, -50%);
        }
        
        .session-dot.animate:before {
            width: 100%;
            height: 100%;
        }
        
        .session-dot:hover {
            transform: scale(1.2);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        /* Ensure colors display properly */
        .session-dot[style*="#f4c4da"] {
            background-color: #f4c4da !important;
        }
        
        .session-dot[style*="#fbfdd4"] {
            background-color: #fbfdd4 !important;
        }
        
        .session-counter {
            font-size: 14px;
            color: #9ca3af;
            margin-top: 16px;
            font-weight: 400;
        }
        
        .progress-ring {
            position: relative;
            width: 120px;
            height: 120px;
            margin: 0 auto 24px;
        }
        
        .progress-ring-circle {
            stroke: #e5e7eb;
            stroke-width: 8;
            fill: transparent;
            transition: stroke-dashoffset 1s ease;
        }
        
        .progress-ring-progress {
            stroke: #f4c4da; /* Light pink for progress ring */
            stroke-width: 8;
            stroke-linecap: round;
            fill: transparent;
            stroke-dasharray: 339.292;
            stroke-dashoffset: 339.292;
            transition: stroke-dashoffset 1s ease;
            filter: drop-shadow(0 0 8px rgba(244, 196, 218, 0.5));
        }
        
        .completion-animation {
            animation: completionPulse 1s ease-in-out;
        }
        
        @keyframes completionPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        
        .fade-in-up {
            animation: fadeInUp 0.6s ease-out;
        }
        
        @media (max-width: 480px) {
            .timer-container {
                padding: 24px;
                margin: 16px;
            }
            
            .timer-display {
                font-size: 40px;
            }
            
            .timer-controls {
                flex-direction: column;
                gap: 12px;
            }
            
            .btn {
                padding: 14px 24px;
            }
            
            .sessions-grid {
                grid-template-columns: repeat(auto-fill, minmax(20px, 1fr));
                gap: 6px;
            }
            
            .session-dot {
                width: 20px;
                height: 20px;
            }
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #10b981, #34d399);
            color: white;
            padding: 16px 24px;
            border-radius: 16px;
            box-shadow: 0 8px 24px rgba(16, 185, 129, 0.3);
            transform: translateX(400px);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            z-index: 1000;
            font-weight: 500;
        }
        
        .notification.show {
            transform: translateX(0);
        }
    </style>
</head>
<body>
        <div class="timer-container">
        <h1 class="timer-title">Study Timer</h1>
        
        <div class="time-selector">
            <button class="time-btn" data-time="15">15min</button>
            <button class="time-btn active" data-time="30">30min</button>
            <button class="time-btn" data-time="45">45min</button>
            <button class="time-btn" data-time="60">60min</button>
        </div>
        
        <div class="progress-ring">
            <svg width="120" height="120">
                <circle class="progress-ring-circle" cx="60" cy="60" r="54"></circle>
                <circle class="progress-ring-progress" cx="60" cy="60" r="54"></circle>
            </svg>
        </div>
        
        <div class="timer-display" id="timerDisplay">30:00</div>
        
        <div class="timer-controls">
            <button class="btn btn-start" id="startBtn">Start</button>
            <button class="btn btn-pause" id="pauseBtn" style="display: none;">Pause</button>
            <button class="btn btn-resume" id="resumeBtn" style="display: none;">Resume</button>
        </div>
        
        <div class="sessions-container">
            <h3 class="sessions-title">Completed Sessions</h3>
            <div class="sessions-grid" id="sessionsGrid"></div>
            <div class="session-counter" id="sessionCounter">0 sessions completed</div>
        </div>
    </div>
    
    <div class="notification" id="notification">
        🎉 Great job! Session completed!
    </div>

    <script>
        class StudyTimer {
            constructor() {
                this.selectedMinutes = 30; // Default to 30 minutes
                this.totalTime = this.selectedMinutes * 60; // Convert to seconds
                this.timeLeft = this.totalTime;
                this.isRunning = false;
                this.isPaused = false;
                this.interval = null;
                this.completedSessions = parseInt(localStorage.getItem('studyTimerSessions') || '0');
                
                this.colors = ['#f4c4da', '#fbfdd4']; // Light pink and light yellow
                
                this.initElements();
                this.initEventListeners();
                this.updateDisplay();
                this.renderSessions();
                this.updateProgressRing();
            }
            
            initElements() {
                this.timerDisplay = document.getElementById('timerDisplay');
                this.startBtn = document.getElementById('startBtn');
                this.pauseBtn = document.getElementById('pauseBtn');
                this.resumeBtn = document.getElementById('resumeBtn');
                this.sessionsGrid = document.getElementById('sessionsGrid');
                this.sessionCounter = document.getElementById('sessionCounter');
                this.notification = document.getElementById('notification');
                this.progressRing = document.querySelector('.progress-ring-progress');
                this.timeButtons = document.querySelectorAll('.time-btn');
            }
            
            initEventListeners() {
                this.startBtn.addEventListener('click', () => this.start());
                this.pauseBtn.addEventListener('click', () => this.pause());
                this.resumeBtn.addEventListener('click', () => this.resume());
                
                // Time selector event listeners
                this.timeButtons.forEach(btn => {
                    btn.addEventListener('click', (e) => this.selectTime(e.target));
                });
            }
            
            selectTime(button) {
                // Don't allow time changes while timer is running
                if (this.isRunning || this.isPaused) return;
                
                // Update active button
                this.timeButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                
                // Update timer duration
                this.selectedMinutes = parseInt(button.dataset.time);
                this.totalTime = this.selectedMinutes * 60;
                this.timeLeft = this.totalTime;
                
                // Update display
                this.updateDisplay();
                this.updateProgressRing();
            }
            
            start() {
                if (this.isRunning) return;
                
                this.isRunning = true;
                this.isPaused = false;
                this.timeLeft = this.totalTime;
                
                // Disable time selector buttons
                this.timeButtons.forEach(btn => btn.disabled = true);
                
                this.startBtn.style.display = 'none';
                this.pauseBtn.style.display = 'inline-block';
                this.resumeBtn.style.display = 'none';
                
                this.timerDisplay.classList.add('active');
                
                this.interval = setInterval(() => {
                    this.timeLeft--;
                    this.updateDisplay();
                    this.updateProgressRing();
                    
                    if (this.timeLeft <= 0) {
                        this.complete();
                    }
                }, 1000);
            }
            
            pause() {
                if (!this.isRunning) return;
                
                this.isPaused = true;
                clearInterval(this.interval);
                
                this.pauseBtn.style.display = 'none';
                this.resumeBtn.style.display = 'inline-block';
            }
            
            resume() {
                if (!this.isPaused) return;
                
                this.isPaused = false;
                
                this.pauseBtn.style.display = 'inline-block';
                this.resumeBtn.style.display = 'none';
                
                this.interval = setInterval(() => {
                    this.timeLeft--;
                    this.updateDisplay();
                    this.updateProgressRing();
                    
                    if (this.timeLeft <= 0) {
                        this.complete();
                    }
                }, 1000);
            }
            
            complete() {
                this.isRunning = false;
                this.isPaused = false;
                clearInterval(this.interval);
                
                // Re-enable time selector buttons
                this.timeButtons.forEach(btn => btn.disabled = false);
                
                this.completedSessions++;
                localStorage.setItem('studyTimerSessions', this.completedSessions.toString());
                
                this.startBtn.style.display = 'inline-block';
                this.pauseBtn.style.display = 'none';
                this.resumeBtn.style.display = 'none';
                
                this.timerDisplay.classList.remove('active');
                this.timeLeft = this.totalTime;
                this.updateDisplay();
                this.updateProgressRing();
                
                // Add completion animation
                document.querySelector('.timer-container').classList.add('completion-animation');
                setTimeout(() => {
                    document.querySelector('.timer-container').classList.remove('completion-animation');
                }, 1000);
                
                // Show notification
                this.showNotification();
                
                // Play completion sound
                this.playCompletionSound();
                
                // Add new session dot with animation
                this.addSessionDot();
                this.updateSessionCounter();
            }
            
            updateDisplay() {
                const minutes = Math.floor(this.timeLeft / 60);
                const seconds = this.timeLeft % 60;
                this.timerDisplay.textContent = `${minutes}:${seconds.toString().padStart(2, '0')}`;
            }
            
            updateProgressRing() {
                const progress = (this.totalTime - this.timeLeft) / this.totalTime;
                const circumference = 2 * Math.PI * 54; // radius is 54
                const offset = circumference - (progress * circumference);
                this.progressRing.style.strokeDashoffset = offset;
            }
            
            renderSessions() {
                this.sessionsGrid.innerHTML = '';
                for (let i = 0; i < this.completedSessions; i++) {
                    const dot = document.createElement('div');
                    dot.className = 'session-dot';
                    dot.style.backgroundColor = this.colors[i % this.colors.length];
                    this.sessionsGrid.appendChild(dot);
                }
                this.updateSessionCounter();
            }
            
            addSessionDot() {
                const dot = document.createElement('div');
                dot.className = 'session-dot fade-in-up';
                dot.style.backgroundColor = this.colors[(this.completedSessions - 1) % this.colors.length];
                this.sessionsGrid.appendChild(dot);
                
                // Add animation effect
                setTimeout(() => {
                    dot.classList.add('animate');
                }, 100);
            }
            
            updateSessionCounter() {
                const sessions = this.completedSessions;
                const text = sessions === 1 ? '1 session completed' : `${sessions} sessions completed`;
                this.sessionCounter.textContent = text;
            }
            
            showNotification() {
                this.notification.classList.add('show');
                setTimeout(() => {
                    this.notification.classList.remove('show');
                }, 3000);
            }
            
            playCompletionSound() {
                // Create a gentle completion sound using Web Audio API
                try {
                    const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                    
                    // Create a gentle bell-like sound
                    const oscillator1 = audioContext.createOscillator();
                    const oscillator2 = audioContext.createOscillator();
                    const gainNode = audioContext.createGain();
                    
                    oscillator1.connect(gainNode);
                    oscillator2.connect(gainNode);
                    gainNode.connect(audioContext.destination);
                    
                    oscillator1.frequency.setValueAtTime(523.25, audioContext.currentTime); // C5
                    oscillator2.frequency.setValueAtTime(659.25, audioContext.currentTime); // E5
                    
                    oscillator1.type = 'sine';
                    oscillator2.type = 'sine';
                    
                    gainNode.gain.setValueAtTime(0, audioContext.currentTime);
                    gainNode.gain.linearRampToValueAtTime(0.1, audioContext.currentTime + 0.1);
                    gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 1);
                    
                    oscillator1.start(audioContext.currentTime);
                    oscillator2.start(audioContext.currentTime);
                    oscillator1.stop(audioContext.currentTime + 1);
                    oscillator2.stop(audioContext.currentTime + 1);
                } catch (error) {
                    console.log('Audio not supported or blocked');
                }
            }
        }
        
        // Initialize the timer when the page loads
        document.addEventListener('DOMContentLoaded', () => {
            new StudyTimer();
        });
    </script>
</body>
</html>
