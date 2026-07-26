<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HPSI Safety App</title>
    <style>
        :root {
            --primary-color: #0056b3;
            --bg-color: #f4f7f6;
            --text-color: #333;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
        }
        .screen {
            display: none;
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .active {
            display: block;
        }
        h2, h3 {
            color: var(--primary-color);
        }
        a.course-link {
            display: inline-block;
            font-size: 1.1em;
            font-weight: bold;
            color: var(--primary-color);
            text-decoration: none;
            margin-bottom: 15px;
        }
        a.course-link:hover {
            text-decoration: underline;
        }
        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 1em;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 20px;
        }
        button:hover {
            background-color: #004494;
        }
        /* Video Gallery Styles */
        .video-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        video {
            width: 100%;
            border-radius: 5px;
            background: #000;
        }
        .video-card {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .video-card span {
            margin-bottom: 10px;
            font-weight: bold;
        }
        /* Quiz Styles */
        .question-block {
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }
        .question-text {
            font-weight: bold;
            margin-bottom: 10px;
        }
        .option-label {
            display: block;
            margin-bottom: 8px;
            cursor: pointer;
        }
        #result-message {
            font-size: 1.2em;
            font-weight: bold;
            margin: 20px 0;
        }
        .pass { color: green; }
        .fail { color: red; }
    </style>
</head>
<body>

    <!-- SCREEN 1: Home / Video Hub -->
    <div id="home-screen" class="screen active">
        <!-- New Hyperlink placed above heading -->
        <a href="https://canva.link/v2huqz1m7d1xx9u" target="_blank" class="course-link">HPSI Code of Conduct Course</a>
        <br>
        
        <!-- New Quiz Button placed below hyperlink -->
        <button onclick="showScreen('quiz-screen')">Code of Conduct Quiz</button>

        <!-- Existing Heading -->
        <h2>H2S Safety in Practice</h2>
        
        <div class="video-grid">
            <div class="video-card">
                <span>Intro</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Intro.mp4"></video>
            </div>
            <div class="video-card">
                <span>Question 1</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Q1.mp4"></video>
            </div>
            <div class="video-card">
                <span>Question 3 (Part 1)</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Q3.mp4"></video>
            </div>
            <!-- Assuming the duplicated Q3 link was intentional, added Part 2 -->
            <div class="video-card">
                <span>Question 3 (Part 2)</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Q3.mp4"></video>
            </div>
            <div class="video-card">
                <span>Question 4</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Q4.mp4"></video>
            </div>
            <div class="video-card">
                <span>Question 5</span>
                <video controls src="https://raw.githubusercontent.com/writetoms/h2s/main/Q5.mp4"></video>
            </div>
        </div>
    </div>

    <!-- SCREEN 2: Quiz -->
    <div id="quiz-screen" class="screen">
        <h2>Code of Conduct Quiz</h2>
        <div id="quiz-container">
            <!-- Questions injected via JS -->
        </div>
        <button onclick="submitQuiz()">Submit Answers</button>
        <button onclick="showScreen('home-screen')" style="background-color: #666;">Cancel</button>
    </div>

    <!-- SCREEN 3: Results -->
    <div id="results-screen" class="screen">
        <h2>Quiz Results</h2>
        <p id="score-display"></p>
        <p id="result-message"></p>
        <button onclick="resetApp()">Home</button>
    </div>

    <script>
        /** 
         * IMPORTANT: Extract the exact texts, choices, and correct answers from 
         * HPSI_Training_Academy_Exam_Questions_1785033281_6a657241bf1dc.csv 
         * and place them in this array. 
         * The example below uses placeholders to demonstrate the structure.
         */
        const quizData = [
            {
                question: "1. Replace this text with Question 1 from the CSV?",
                options: ["Option A", "Option B", "Option C", "Option D"],
                correctAnswer: 0 // Index of the correct option (0 = Option A)
            },
            {
                question: "2. Replace this text with Question 2 from the CSV?",
                options: ["Option A", "Option B", "Option C", "Option D"],
                correctAnswer: 1 
            },
            {
                question: "3. Replace this text with Question 3 from the CSV?",
                options: ["Option A", "Option B", "Option C", "Option D"],
                correctAnswer: 2
            },
            {
                question: "4. Replace this text with Question 4 from the CSV?",
                options: ["Option A", "Option B", "Option C", "Option D"],
                correctAnswer: 3
            },
            {
                question: "5. Replace this text with Question 5 from the CSV?",
                options: ["Option A", "Option B", "Option C", "Option D"],
                correctAnswer: 0
            }
        ];

        // Screen Navigation
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
            
            if (screenId === 'quiz-screen') {
                renderQuiz();
            }
        }

        function resetApp() {
            // Pause all videos when navigating home
            document.querySelectorAll('video').forEach(v => v.pause());
            showScreen('home-screen');
        }

        // Render Quiz Questions
        function renderQuiz() {
            const container = document.getElementById('quiz-container');
            container.innerHTML = ''; // Clear previous

            quizData.forEach((q, index) => {
                const qBlock = document.createElement('div');
                qBlock.className = 'question-block';
                
                const qText = document.createElement('div');
                qText.className = 'question-text';
                qText.textContent = q.question;
                qBlock.appendChild(qText);

                q.options.forEach((opt, optIndex) => {
                    const label = document.createElement('label');
                    label.className = 'option-label';
                    
                    const radio = document.createElement('input');
                    radio.type = 'radio';
                    radio.name = 'question' + index;
                    radio.value = optIndex;

                    label.appendChild(radio);
                    label.appendChild(document.createTextNode(' ' + opt));
                    qBlock.appendChild(label);
                });

                container.appendChild(qBlock);
            });
        }

        // Grade Quiz
        function submitQuiz() {
            let score = 0;
            let answeredAll = true;

            quizData.forEach((q, index) => {
                const selected = document.querySelector(`input[name="question${index}"]:checked`);
                if (!selected) {
                    answeredAll = false;
                } else if (parseInt(selected.value) === q.correctAnswer) {
                    score++;
                }
            });

            if (!answeredAll) {
                alert("Please answer all questions before submitting.");
                return;
            }

            // Display Results
            document.getElementById('score-display').textContent = `You scored ${score} out of 5.`;
            
            const resultMsg = document.getElementById('result-message');
            if (score >= 4) {
                resultMsg.textContent = "Congratulations! You passed the quiz.";
                resultMsg.className = "pass";
            } else {
                resultMsg.textContent = "You did not pass. A minimum score of 4/5 is required.";
                resultMsg.className = "fail";
            }

            showScreen('results-screen');
        }
    </script>
</body>
</html>
