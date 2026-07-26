<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>H2S Safety in Practice</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    /* Vertical Gradient Colors */
    --gradient-top: #9cd326; 
    --gradient-bottom: #006b8f; 
    
    /* Interface Colors */
    --text-primary: #ffffff;
    --text-dark: #18181b;
    --text-secondary: #e2e8f0;
    --border-radius: 24px;
    --transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
    
    /* Glassmorphism Surface */
    --surface-base: rgba(0, 0, 0, 0.25);
    --surface-border: rgba(255, 255, 255, 0.15);
    
    /* Interactive Elements */
    --button-bg: #ffffff;
    --button-text: #006b8f;
    --button-hover: #f1f5f9;
  }

  * { box-sizing: border-box; }

  body {
    font-family: 'Inter', sans-serif;
    margin: 0;
    padding: 15px 15px 80px 15px; 
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    color: var(--text-primary);
    background: linear-gradient(180deg, var(--gradient-top) 0%, var(--gradient-bottom) 100%);
    position: relative;
    z-index: 0;
  }

  /* Procedural Texture Overlay */
  body::before {
    content: "";
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)' opacity='0.12'/%3E%3C/svg%3E");
    z-index: -1;
    pointer-events: none;
  }

  /* Main Glass Container */
  #app {
    width: 100%;
    max-width: 500px;
    border-radius: var(--border-radius);
    background-color: var(--surface-base);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border: 1px solid var(--surface-border);
    box-shadow: 0 25px 50px rgba(0, 0, 0, 0.2);
    overflow: hidden;
    position: relative;
    min-height: 650px;
    display: flex;
    flex-direction: column;
    margin: auto; 
  }

  /* Screen Visibility */
  .screen {
    display: none;
    flex-direction: column;
    padding: 35px 30px 80px 30px; 
    flex: 1;
    height: 100%;
    animation: fadeIn 0.5s ease forwards;
  }
  .screen.active {
    display: flex;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* Typography & Badges */
  .masthead-badge {
    display: inline-block;
    padding: 6px 14px;
    background: rgba(255, 255, 255, 0.15);
    border: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 20px;
    font-size: 0.85rem;
    font-weight: 700;
    letter-spacing: 1px;
    text-transform: uppercase;
    color: #ffffff;
    margin-bottom: 18px;
    width: fit-content;
    backdrop-filter: blur(5px);
  }

  h1 { font-size: 2rem; font-weight: 700; margin: 0 0 15px 0; letter-spacing: -0.5px; line-height: 1.1; }
  p { font-size: 1.05rem; color: var(--text-secondary); line-height: 1.6; margin: 0 0 20px 0; font-weight: 300; }
  
  /* Background Screen Content Styling */
  .content-scroll {
    overflow-y: auto;
    max-height: 440px;
    padding-right: 5px;
    margin-bottom: 20px;
  }
  .content-scroll::-webkit-scrollbar {
    width: 4px;
  }
  .content-scroll::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.3);
    border-radius: 4px;
  }

  /* Primary Buttons */
  .btn-primary {
    background: var(--button-bg);
    color: var(--button-text);
    border: none;
    border-radius: 16px;
    padding: 18px;
    font-size: 1.15rem;
    font-weight: 600;
    cursor: pointer;
    transition: var(--transition);
    margin-top: auto;
    width: 100%;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }
  .btn-primary:hover {
    background: var(--button-hover);
    transform: translateY(-3px);
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.15);
  }

  /* Timer Details */
  .timer-container {
    width: 100%;
    margin-bottom: 25px;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
  }
  .timer-bar-outer {
    width: 100%;
    height: 8px;
    background-color: rgba(255, 255, 255, 0.2); 
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 8px;
  }
  .timer-bar-fill {
    height: 100%;
    background: #ffffff; 
    width: 100%;
    transition: width 1s linear;
    border-radius: 4px;
  }
  .timer-label {
    font-size: 0.95rem;
    color: var(--text-primary);
    font-weight: 600;
    letter-spacing: 0.5px;
  }

  /* Image & Text Areas */
  .question-image-container {
    width: 100%;
    height: 180px;
    border-radius: 18px;
    margin-bottom: 25px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    overflow: hidden;
    background-color: #000;
  }
  .question-image-container img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
  .question-text-area {
    font-size: 1.15rem;
    font-weight: 600;
    margin-bottom: 20px;
    line-height: 1.4;
    text-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }

  /* Answer Tiles */
  .tiles-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    margin-bottom: 20px;
  }
  .tile {
    background-color: #ffffff;
    border: 2px solid transparent;
    border-radius: 16px;
    padding: 16px 20px;
    font-size: 1rem;
    font-weight: 600;
    color: var(--text-dark);
    cursor: pointer;
    text-align: left;
    transition: var(--transition);
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0 8px 16px rgba(0,0,0,0.1);
  }
  .tile:hover {
    border-color: var(--gradient-bottom);
    transform: translateY(-3px);
    box-shadow: 0 12px 24px rgba(0, 107, 143, 0.2);
  }

  /* Feedback Screens */
  .feedback-header {
    font-size: 2.2rem;
    font-weight: 700;
    margin-bottom: 15px;
    text-align: center;
    text-shadow: 0 2px 5px rgba(0,0,0,0.2);
  }
  .explanation-card {
    background-color: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    padding: 20px;
    margin-bottom: 20px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    font-size: 1rem;
    color: #f8fafc;
    line-height: 1.5;
  }
  
  /* Inline Video Container */
  .video-container-outer {
    position: relative;
    width: 100%;
    aspect-ratio: 16 / 9;
    height: auto;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 15px 35px rgba(0,0,0,0.4);
    background: #000;
    border: 1px solid rgba(255, 255, 255, 0.15);
    margin-bottom: 20px;
  }
  .video-container-outer video {
    width: 100%; 
    height: 100%;
    object-fit: cover;
    display: block;
    border: none;
    margin: 0;
    padding: 0;
  }

  /* --- MOBILE RESPONSIVENESS --- */
  @media (max-width: 480px) {
    body {
      padding: 0; 
    }
    #app {
      min-height: 100vh;
      border-radius: 0; 
      border: none;
    }
    .screen {
      padding: 20px 15px 150px 15px; 
    }
    h1 { font-size: 1.6rem; margin-bottom: 10px; }
    p { font-size: 0.95rem; margin-bottom: 15px; }
    .content-scroll { max-height: 380px; }
    .timer-container { margin-bottom: 15px; }
    .question-image-container { height: 140px; margin-bottom: 15px; border-radius: 12px; }
    .question-text-area { font-size: 1.05rem; margin-bottom: 15px; }
    .tiles-container { gap: 12px; }
    .tile { padding: 12px 15px; font-size: 0.95rem; border-radius: 12px; }
    .btn-primary { padding: 14px; font-size: 1.05rem; border-radius: 12px; }
    .feedback-header { font-size: 1.8rem; margin-bottom: 10px; }
    .explanation-card { padding: 15px; font-size: 0.9rem; margin-bottom: 15px; }
    .video-container-outer { border-radius: 12px; }
  }
</style>
</head>
<body>

<div id="app">
  
  <!-- INTRO SCREEN 1: Welcome -->
  <div id="screen-intro-1" class="screen active">
    <div style="margin: auto 0;">
      <div class="masthead-badge">H2S Awareness</div>
      <h1>H2S Safety in Practice</h1>
      <p>Test applying your knowledge of emergency procedures in an H2S crisis situation. You will watch a brief introduction, followed by 5 timed scenarios. You have 20 seconds per question.</p>
    </div>
    <button class="btn-primary" onclick="initiateAudioAndProceed('screen-background')">Next Step</button>
  </div>

  <!-- SCREEN 2: Background -->
  <div id="screen-background" class="screen">
    <h1>Background</h1>
    <div class="content-scroll">
      <p><strong>Objective:</strong> To evaluate how the participants will apply the gained knowledge of H2S safety in realistic scenarios before completing the training.</p>
      
      <p>This is a short, performance-based module designed to be learner-centered and highly interactive. It incorporates learner engagement elements such as decision-making scenarios and simulation-styled assessment of learners' knowledge.</p>
      
      <p>This was a very tech-driven design process: it involved using Veo 3.1 for ai video generation, Gemini 3.6 for ai image generation and webapp design and debugging, and Github for web hosting--using completely new workflows. The content design employed (among others) genuine Saudi Aramco H2S Awareness learning text from the open web.</p>
    </div>
    <button class="btn-primary" onclick="goToScreen('screen-intro-2')">Continue to Briefing</button>
  </div>

  <!-- INTRO SCREEN 3: Video Briefing -->
  <div id="screen-intro-2" class="screen">
    <h1>Mission Briefing</h1>
    <div class="video-container-outer">
      <!-- HTML5 Inline Intro Video (No Loop) -->
      <video id="intro-video" playsinline preload="auto">
        <source id="intro-video-source" src="" type="video/mp4">
        Your browser does not support inline video.
      </video>
    </div>
    <p>You are about to enter an H2S emergency situation at an Aramco facility! Use the knowledge you gained about H2S Safety Awareness to help you stay safe! This will require you to focus, think fast, and be sure of the protocols.</p>
    <p style="margin-bottom: 0;">Click below when you are ready to begin the timer for Question 1.</p>
    <button class="btn-primary" onclick="startQuestion(1)">Start Quiz</button>
  </div>

  <!-- DYNAMIC QUESTION SCREEN -->
  <div id="screen-question" class="screen">
    <div class="timer-container">
      <div class="timer-bar-outer">
        <div id="timer-fill" class="timer-bar-fill"></div>
      </div>
      <div class="timer-label"><span id="timer-text">20</span>s</div>
    </div>
    
    <div class="question-image-container">
      <img id="q-image" src="" alt="Scenario">
    </div>
    
    <div id="q-text" class="question-text-area"></div>
    
    <div id="q-choices" class="tiles-container">
      <!-- Choices injected here via JS -->
    </div>
  </div>

  <!-- DYNAMIC FEEDBACK SCREEN -->
  <div id="screen-feedback" class="screen">
    <div id="feedback-status" class="feedback-header"></div>
    
    <div class="explanation-card">
      <p id="f-explanation"></p>
    </div>

    <div class="video-container-outer">
      <!-- HTML5 Inline Feedback Video (No Loop) -->
      <video id="f-video" playsinline preload="auto">
        <source id="f-video-source" src="" type="video/mp4">
        Your browser does not support inline video.
      </video>
    </div>
    
    <button id="btn-next-question" class="btn-primary" onclick="nextQuestion()">Next Question</button>
  </div>

  <!-- FINAL RESULT SCREEN -->
  <div id="screen-final" class="screen">
    <div style="margin: auto 0; text-align: center;">
      <h1>Challenge Complete</h1>
      <p>You have successfully completed the safety protocol review.</p>
    </div>
    <button class="btn-primary" onclick="location.reload()">Restart Quiz</button>
  </div>

</div>

<script>
  // Relative paths for native GitHub Pages streaming & zero CORS issues
  const introVideoSrc = "Intro.mp4";

  const quizData = [
    {
      question: "Hazard Detection (Sec 1.4) Walking past a low-lying cellar pit, you smell a faint 'rotten egg' smell that suddenly disappears, but no stationary alarm sounds yet. What do you do next?",
      imageSrc: "Q1.png",
      choices: [
        { text: "Keep working; if the smell is gone, the gas has dispersed.", isCorrect: false },
        { text: "Alert Ahmad and check your personal H2S monitor.", isCorrect: true }
      ],
      explanation: "A sudden loss of the 'rotten egg' smell can indicate olfactory fatigue (loss of smell), a sign of high H2S concentrations. You must immediately check your monitor and alert your team.",
      videoSrc: "Q1.mp4"
    },
    {
      question: "Alarm Trigger (Sec 3.2) The personal alarm vibrates and flashes red (>10 ppm). Ahmad signals an emergency. What do you do next?",
      imageSrc: "Q2.png",
      choices: [
        { text: "Run quickly toward the office trailer to read the central gas monitor board.", isCorrect: false },
        { text: "Go don an EEBA mask before attempting to communicate or leave site.", isCorrect: true }
      ],
      explanation: "When an H2S alarm triggers, your immediate priority is to don your Emergency Escape Breathing Apparatus (EEBA) to protect your airway before taking any other action.",
      videoSrc: "Q2.mp4"
    },
    {
      question: "Pre-Inspection (Sec 5.2) In the equipment shed, you select an Emergency Escape Breathing Apparatus (EEBA). What is your first step?",
      imageSrc: "Q3.png",
      choices: [
        { text: "Ensure pressure gauge reading is within 10% of maximum operating pressure.", isCorrect: true },
        { text: "Ensure cylinder airflow valve opens easily by turning it twice.", isCorrect: false }
      ],
      explanation: "Before deployment, you must verify the cylinder is full by ensuring the pressure gauge is within 10% of its maximum operating pressure.",
      videoSrc: "Q3.mp4"
    },
    {
      question: "Wind Evaluation (Sec 3.2) Mask on and breathing. You glance at the rig windsock. It is blowing directly toward the North. What do you do next?",
      imageSrc: "Q4.png",
      choices: [
        { text: "Evacuate toward the North (downwind).", isCorrect: false },
        { text: "Evacuate toward the South or Crosswind (East/West).", isCorrect: true }
      ],
      explanation: "Never run downwind with the gas. Always evacuate upwind or crosswind away from the hazard to escape the gas plume safely.",
      videoSrc: "Q4.mp4"
    },
    {
      question: "Assembly Point (Sec 3.2) You reach the designated Safe Briefing Area (SBA). What is your immediate priority upon arriving?",
      imageSrc: "Q5.png",
      choices: [
        { text: "Report to the Muster Captain for headcount and remain in the SBA.", isCorrect: true },
        { text: "Take off your EEBA mask immediately since you reached the SBA.", isCorrect: false }
      ],
      explanation: "Upon reaching the Safe Briefing Area, your first priority is to report for a headcount while keeping your respiratory protection on until the official all-clear is given.",
      videoSrc: "Q5.mp4"
    }
  ];

  const TIME_LIMIT = 20;
  let currentQuestionIndex = 0;
  let timeLeft = TIME_LIMIT;
  let timerInterval;

  const timerFill = document.getElementById('timer-fill');
  const timerText = document.getElementById('timer-text');
  const qImage = document.getElementById('q-image');
  const qText = document.getElementById('q-text');
  const qChoices = document.getElementById('q-choices');
  const feedbackStatus = document.getElementById('feedback-status');
  const fExplanation = document.getElementById('f-explanation');
  
  const introVideo = document.getElementById('intro-video');
  const introVideoSource = document.getElementById('intro-video-source');
  const fVideo = document.getElementById('f-video');
  const fVideoSource = document.getElementById('f-video-source');
  
  const btnNextQuestion = document.getElementById('btn-next-question');

  // Helper function to safely play inline video with audio enabled (Method 1)
  function playVideoWithAudio(videoElement) {
    if (!videoElement) return;
    
    videoElement.muted = false; // Enable clip's embedded audio
    
    let playPromise = videoElement.play();
    if (playPromise !== undefined) {
      playPromise.catch(error => {
        console.warn("Autoplay with audio was restricted. Playing muted fallback:", error);
        videoElement.muted = true;
        videoElement.play();
      });
    }
  }

  // Initial user gesture handler to unlock audio across mobile browsers
  function initiateAudioAndProceed(targetScreen) {
    goToScreen(targetScreen);
  }

  function goToScreen(screenId) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(screenId).classList.add('active');
    
    // Manage intro video
    if (screenId !== 'screen-intro-2') {
      introVideo.pause();
    } else {
      introVideoSource.src = introVideoSrc;
      introVideo.load();
      playVideoWithAudio(introVideo);
    }

    // Manage feedback video
    if (screenId !== 'screen-feedback') {
      fVideo.pause();
    }
  }

  function startQuestion(indexNumber) {
    currentQuestionIndex = indexNumber - 1;
    const data = quizData[currentQuestionIndex];
    
    qImage.src = data.imageSrc;
    qText.textContent = `Q${indexNumber}: ${data.question}`;
    
    qChoices.innerHTML = '';
    data.choices.forEach(choice => {
      const btn = document.createElement('button');
      btn.className = 'tile';
      btn.innerHTML = `${choice.text} <span>→</span>`;
      btn.onclick = () => submitAnswer(choice.isCorrect);
      qChoices.appendChild(btn);
    });

    goToScreen('screen-question');
    startTimer();
  }

  function startTimer() {
    timeLeft = TIME_LIMIT;
    timerText.textContent = timeLeft;
    timerFill.style.width = '100%';
    timerFill.style.background = '#ffffff';

    clearInterval(timerInterval);
    timerInterval = setInterval(() => {
      timeLeft--;
      timerText.textContent = timeLeft;
      
      const percentage = (timeLeft / TIME_LIMIT) * 100;
      timerFill.style.width = `${percentage}%`;

      if (timeLeft <= 5) { timerFill.style.background = '#ff4d4d'; }

      if (timeLeft <= 0) {
        clearInterval(timerInterval);
        submitAnswer(false, true); 
      }
    }, 1000);
  }

  function submitAnswer(isCorrect, isTimeout = false) {
    clearInterval(timerInterval);
    const data = quizData[currentQuestionIndex];

    if (isCorrect) {
      feedbackStatus.textContent = 'Correct';
      feedbackStatus.style.color = '#a3e635';
    } else if (isTimeout) {
      feedbackStatus.textContent = "Time's Up";
      feedbackStatus.style.color = '#ff4d4d';
    } else {
      feedbackStatus.textContent = 'Incorrect';
      feedbackStatus.style.color = '#ff4d4d';
    }

    fExplanation.innerHTML = `<strong>Explanation:</strong> ${data.explanation}`;
    
    // Load and play feedback video with audio
    fVideoSource.src = data.videoSrc;
    fVideo.load();
    playVideoWithAudio(fVideo);

    if (currentQuestionIndex >= quizData.length - 1) {
      btnNextQuestion.textContent = "Finish Challenge";
    } else {
      btnNextQuestion.textContent = "Next Question";
    }

    goToScreen('screen-feedback');
  }

  function nextQuestion() {
    if (currentQuestionIndex >= quizData.length - 1) {
      goToScreen('screen-final');
    } else {
      startQuestion(currentQuestionIndex + 2); 
    }
  }
</script>
</body>
</html>
