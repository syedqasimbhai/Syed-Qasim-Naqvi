<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SpeedTyper | Improve Your Typing Skills</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .typing-text span {
            position: relative;
        }
        .typing-text span.active {
            background-color: #22c55e;
            color: white;
        }
        .typing-text span.active::before {
            content: '';
            position: absolute;
            left: 0;
            bottom: 0;
            height: 2px;
            width: 100%;
            background: #22c55e;
            animation: blink 1s ease-in-out infinite;
        }
        .typing-text span.correct {
            color: #22c55e;
        }
        .typing-text span.incorrect {
            color: #ef4444;
            text-decoration: underline;
        }
        @keyframes blink {
            50% { opacity: 0; }
        }
        .progress-ring__circle {
            transition: stroke-dashoffset 0.3s;
            transform: rotate(-90deg);
            transform-origin: 50% 50%;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
        <!-- Header -->
        <header class="text-center mb-10">
            <h1 class="text-4xl font-bold mb-2 bg-gradient-to-r from-green-400 to-blue-500 text-transparent bg-clip-text">SpeedTyper</h1>
            <p class="text-slate-400">Practice typing and improve your speed & accuracy</p>
        </header>

        <!-- Stats Dashboard -->
        <div class="bg-slate-800 rounded-xl p-6 mb-8 grid grid-cols-1 md:grid-cols-3 gap-6 shadow-lg">
            <!-- WPM -->
            <div class="flex flex-col items-center">
                <svg width="80" height="80" viewBox="0 0 80 80" class="mb-2 relative">
                    <circle cx="40" cy="40" r="36" stroke="#334155" stroke-width="8" fill="none"/>
                    <circle cx="40" cy="40" r="36" stroke="#22c55e" stroke-width="8" fill="none" 
                            stroke-dasharray="226" stroke-dashoffset="calc(226 - (226 * 0) / 100)" class="progress-ring__circle"/>
                    <text x="50%" y="50%" text-anchor="middle" dy=".3em" fill="white" class="text-xl font-bold">0</text>
                </svg>
                <p class="text-lg font-semibold">WPM</p>
                <p class="text-slate-400 text-sm">Words Per Minute</p>
            </div>
            
            <!-- Accuracy -->
            <div class="flex flex-col items-center">
                <svg width="80" height="80" viewBox="0 0 80 80" class="mb-2 relative">
                    <circle cx="40" cy="40" r="36" stroke="#334155" stroke-width="8" fill="none"/>
                    <circle cx="40" cy="40" r="36" stroke="#3b82f6" stroke-width="8" fill="none" 
                            stroke-dasharray="226" stroke-dashoffset="calc(226 - (226 * 0) / 100)" class="progress-ring__circle"/>
                    <text x="50%" y="50%" text-anchor="middle" dy=".3em" fill="white" class="text-xl font-bold">0%</text>
                </svg>
                <p class="text-lg font-semibold">Accuracy</p>
                <p class="text-slate-400 text-sm">Keystroke Accuracy</p>
            </div>
            
            <!-- Time -->
            <div class="flex flex-col items-center">
                <div class="w-20 h-20 rounded-full border-4 border-purple-500 flex items-center justify-center mb-2">
                    <p class="text-2xl font-bold">60</p>
                </div>
                <p class="text-lg font-semibold">Time</p>
                <p class="text-slate-400 text-sm">Seconds Left</p>
            </div>
        </div>

        <!-- Content Box -->
        <div class="bg-slate-800 rounded-xl p-6 mb-8 shadow-lg">
            <!-- Difficulty Selector -->
            <div class="flex justify-between items-center mb-6">
                <div>
                    <label for="difficulty" class="block text-sm font-medium text-slate-300 mb-1">Difficulty</label>
                    <select id="difficulty" class="bg-slate-700 border border-slate-600 text-slate-100 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">
                        <option value="easy">Easy</option>
                        <option value="medium" selected>Medium</option>
                        <option value="hard">Hard</option>
                    </select>
                </div>
                <div>
                    <label for="time" class="block text-sm font-medium text-slate-300 mb-1">Duration</label>
                    <select id="time" class="bg-slate-700 border border-slate-600 text-slate-100 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">
                        <option value="30">30 seconds</option>
                        <option value="60" selected>60 seconds</option>
                        <option value="120">120 seconds</option>
                    </select>
                </div>
                <button id="startBtn" class="bg-green-600 hover:bg-green-700 text-white font-medium rounded-lg px-5 py-2.5 transition-all flex items-center">
                    <i class="fas fa-play mr-2"></i> Start
                </button>
            </div>

            <!-- Typing Area -->
            <div class="typing-text mb-6 p-4 bg-slate-700 rounded-lg text-lg h-48 overflow-y-auto leading-relaxed"></div>

            <!-- Input Area -->
            <div class="relative">
                <textarea id="userInput" rows="3" class="w-full p-4 bg-slate-700 border border-slate-600 rounded-lg text-slate-100 focus:outline-none focus:ring-2 focus:ring-blue-500 resize-none" placeholder="Type the text above here..." disabled></textarea>
                <div class="absolute right-3 bottom-3 bg-blue-600 text-xs text-white px-2 py-1 rounded-full flex items-center">
                    <span id="charCount">0</span>
                    <span class="mx-1">/</span>
                    <span id="totalChars">0</span>
                </div>
            </div>
        </div>

        <!-- Results Modal -->
        <div id="resultsModal" class="hidden fixed inset-0 bg-black bg-opacity-70 z-50 flex items-center justify-center p-4">
            <div class="bg-slate-800 rounded-xl p-8 max-w-md w-full">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold">Your Results</h2>
                    <button id="closeModal" class="text-slate-400 hover:text-white">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                <div class="space-y-4 mb-6">
                    <div class="flex justify-between">
                        <span class="text-slate-400">WPM</span>
                        <span id="resultWpm" class="font-bold text-xl">24</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-slate-400">Accuracy</span>
                        <span id="resultAccuracy" class="font-bold text-xl">85%</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-slate-400">Correct</span>
                        <span id="resultCorrect" class="font-bold text-green-500">102</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-slate-400">Incorrect</span>
                        <span id="resultIncorrect" class="font-bold text-red-500">8</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-slate-400">Duration</span>
                        <span id="resultTime" class="font-bold">60s</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-slate-400">Difficulty</span>
                        <span id="resultDifficulty" class="font-bold">Medium</span>
                    </div>
                </div>
                <div class="flex justify-center">
                    <button id="tryAgainBtn" class="bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-lg px-5 py-2.5 transition-all">
                        <i class="fas fa-redo mr-2"></i> Try Again
                    </button>
                </div>
            </div>
        </div>

        <!-- Footer -->
        <footer class="text-center text-slate-500 text-sm mt-12">
            <p>Made with <i class="fas fa-heart text-red-500"></i> to help you type faster</p>
            <p class="mt-1">Press <span class="bg-slate-700 px-2 py-1 rounded">ESC</span> to restart the test</p>
        </footer>
    </div>

    <script>
        // Sample texts for each difficulty level
        const texts = {
            easy: [
                "The quick brown fox jumps over the lazy dog. Simple sentences help beginners practice typing.",
                "Learning to type fast requires regular practice. Start with easy words and build your speed.",
                "Touch typing is a skill that improves with time. Keep your fingers on the home row keys."
            ],
            medium: [
                "The agile developer sprinted through the JavaScript code, fixing bugs with each keystroke. Debugging requires patience and attention to detail, especially with complex algorithms.",
                "Modern web applications require efficient coding practices. Responsive design ensures compatibility across different devices and screen sizes.",
                "Programming languages evolve constantly; staying updated is crucial for developers. Frameworks like React and Vue have transformed front-end development."
            ],
            hard: [
                "The juxtaposition of quantum mechanics with classical Newtonian physics presents a fascinating dichotomy that continues to perplex even the most erudite physicists. Heisenberg's uncertainty principle fundamentally alters our perception of subatomic particles.",
                "Obstreperous interlocutors often exacerbate contentious debates with their vociferous objections and truculent demeanor, making diplomatic resolutions seem like Sisyphean tasks in the geopolitical arena.",
                "Pneumonoultramicroscopicsilicovolcanoconiosis, an esoteric term from medical lexicon, refers to a lung disease caused by inhalation of fine silicate or quartz dust, typically found in volcanos. This exemplifies the complexity of pathological nomenclature."
            ]
        };

        // DOM Elements
        const typingText = document.querySelector('.typing-text');
        const userInput = document.getElementById('userInput');
        const startBtn = document.getElementById('startBtn');
        const difficultySelect = document.getElementById('difficulty');
        const timeSelect = document.getElementById('time');
        const charCount = document.getElementById('charCount');
        const totalChars = document.getElementById('totalChars');
        const resultsModal = document.getElementById('resultsModal');
        const closeModal = document.getElementById('closeModal');
        const tryAgainBtn = document.getElementById('tryAgainBtn');
        const wpmText = document.querySelector('text:nth-of-type(1)');
        const accuracyText = document.querySelector('text:nth-of-type(2)');
        const timeText = document.querySelector('.w-20 p');
        const wpmCircle = document.querySelector('circle:nth-of-type(2)');
        const accuracyCircle = document.querySelector('circle:nth-of-type(4)');
        const resultWpm = document.getElementById('resultWpm');
        const resultAccuracy = document.getElementById('resultAccuracy');
        const resultCorrect = document.getElementById('resultCorrect');
        const resultIncorrect = document.getElementById('resultIncorrect');
        const resultTime = document.getElementById('resultTime');
        const resultDifficulty = document.getElementById('resultDifficulty');

        // Variables
        let timer;
        let timeLeft;
        let isTyping = false;
        let totalTyped = 0;
        let correctTyped = 0;
        let incorrectTyped = 0;
        let startTime;
        let endTime;

        // Initialize
        function init() {
            userInput.value = '';
            charCount.textContent = '0';
            userInput.disabled = true;
            userInput.placeholder = 'Click Start to begin the test';
            typingText.textContent = 'Select difficulty and duration, then click Start to begin your typing test.';
            resetStats();
        }

        // Reset stats
        function resetStats() {
            wpmText.textContent = '0';
            accuracyText.textContent = '0%';
            timeText.textContent = '60';
            wpmCircle.style.strokeDashoffset = '226';
            accuracyCircle.style.strokeDashoffset = '226';
        }

        // Start test
        function startTest() {
            const difficulty = difficultySelect.value;
            timeLeft = parseInt(timeSelect.value);
            timeText.textContent = timeLeft;
            
            // Select random text for the chosen difficulty
            const randomIndex = Math.floor(Math.random() * texts[difficulty].length);
            const text = texts[difficulty][randomIndex];
            
            // Set up the text with spans for each character
            typingText.innerHTML = '';
            text.split('').forEach(char => {
                const span = document.createElement('span');
                span.textContent = char === ' ' ? ' ' : char;
                typingText.appendChild(span);
            });
            
            // Highlight first character
            const firstChar = typingText.querySelector('span');
            if (firstChar) firstChar.classList.add('active');
            
            // Reset stats
            totalTyped = 0;
            correctTyped = 0;
            incorrectTyped = 0;
            charCount.textContent = '0';
            totalChars.textContent = text.length;
            userInput.value = '';
            userInput.disabled = false;
            userInput.placeholder = 'Start typing...';
            userInput.focus();
            
            // Start timer
            isTyping = true;
            startTime = new Date();
            
            timer = setInterval(() => {
                timeLeft--;
                timeText.textContent = timeLeft;
                
                if (timeLeft <= 0) {
                    endTest();
                }
            }, 1000);
            
            startBtn.disabled = true;
            startBtn.innerHTML = '<i class="fas fa-play mr-2"></i> Running...';
            startBtn.classList.remove('bg-green-600', 'hover:bg-green-700');
            startBtn.classList.add('bg-yellow-600', 'hover:bg-yellow-700');
        }

        // End test
        function endTest() {
            clearInterval(timer);
            isTyping = false;
            endTime = new Date();
            userInput.disabled = true;
            startBtn.disabled = false;
            startBtn.innerHTML = '<i class="fas fa-play mr-2"></i> Start';
            startBtn.classList.remove('bg-yellow-600', 'hover:bg-yellow-700');
            startBtn.classList.add('bg-green-600', 'hover:bg-green-700');
            
            // Calculate results
            const timeInMinutes = (endTime - startTime) / 60000; // in minutes
            const charactersTyped = totalTyped;
            const charactersCorrect = correctTyped;
            const wpm = Math.round((charactersCorrect / 5) / timeInMinutes) || 0;
            const accuracy = Math.round((charactersCorrect / charactersTyped) * 100) || 0;
            
            // Update result modal
            resultWpm.textContent = wpm;
            resultAccuracy.textContent = accuracy + '%';
            resultCorrect.textContent = charactersCorrect;
            resultIncorrect.textContent = incorrectTyped;
            resultTime.textContent = parseInt(timeSelect.value) + 's';
            resultDifficulty.textContent = difficultySelect.options[difficultySelect.selectedIndex].text;
            
            // Update progress rings
            wpmText.textContent = wpm;
            accuracyText.textContent = accuracy + '%';
            wpmCircle.style.strokeDashoffset = `calc(226 - (226 * ${Math.min(wpm, 100)}) / 100)`;
            accuracyCircle.style.strokeDashoffset = `calc(226 - (226 * ${accuracy}) / 100)`;
            
            // Show modal
            resultsModal.classList.remove('hidden');
        }

        // Handle user input
        function handleInput(e) {
            if (!isTyping) return;
            
            const textSpans = typingText.querySelectorAll('span');
            const inputValue = userInput.value;
            const inputLength = inputValue.length;
            
            // Update character count
            charCount.textContent = inputLength;
            totalTyped = inputLength;
            
            // Reset all spans
            textSpans.forEach(span => {
                span.classList.remove('active', 'correct', 'incorrect');
            });
            
            // Check each character
            let allCorrect = true;
            textSpans.forEach((span, index) => {
                if (index < inputLength) {
                    if (span.textContent === inputValue[index] || 
                        (span.textContent === ' ' && inputValue[index] === ' ')) {
                        span.classList.add('correct');
                        if (index === inputLength - 1) {
                            correctTyped++;
                        }
                    } else {
                        span.classList.add('incorrect');
                        allCorrect = false;
                        if (index === inputLength - 1) {
                            incorrectTyped++;
                        }
                    }
                }
            });
            
            // Highlight current position
            if (inputLength < textSpans.length) {
                textSpans[inputLength].classList.add('active');
            }
            
            // If all characters typed correctly, end test
            if (inputLength === textSpans.length && allCorrect) {
                endTest();
            }
        }

        // Event Listeners
        startBtn.addEventListener('click', startTest);
        userInput.addEventListener('input', handleInput);
        closeModal.addEventListener('click', () => resultsModal.classList.add('hidden'));
        tryAgainBtn.addEventListener('click', () => {
            resultsModal.classList.add('hidden');
            startTest();
        });
        
        // ESC key to restart test
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape' && isTyping) {
                clearInterval(timer);
                init();
            }
        });

        // Initialize
        init();
    </script>
</body>
</html>
