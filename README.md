<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Für dich ❤️</title>
    <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@500;700&family=Plus+Jakarta+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #0f0a0d;
            --card-bg: #1a1218;
            --primary-pink: #ff3366;
            --hover-pink: #e62e5c;
            --letter-bg: #fffbf2;
            --letter-text: #2c2226;
            --gold: #dfb15b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Plus Jakarta Sans', sans-serif;
        }

        body {
            background-color: var(--bg-color);
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(255, 51, 102, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 90% 80%, rgba(223, 177, 91, 0.06) 0%, transparent 40%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            color: #fff;
        }

        .container {
            position: relative;
            width: 100%;
            max-width: 440px;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Start Screen */
        .start-screen {
            text-align: center;
            animation: fadeIn 1s ease forwards;
        }

        .start-screen h1 {
            font-size: 1.8rem;
            font-weight: 500;
            margin-bottom: 25px;
            color: #fce7f0;
            letter-spacing: -0.5px;
        }

        .btn {
            background: linear-gradient(135deg, var(--primary-pink), #ff6b8b);
            color: white;
            border: none;
            padding: 14px 32px;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 10px 25px rgba(255, 51, 102, 0.3);
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .btn:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 15px 30px rgba(255, 51, 102, 0.4);
        }

        .btn:active {
            transform: translateY(1px);
        }

        /* Envelope Container */
        .envelope-wrapper {
            position: relative;
            width: 320px;
            height: 220px;
            background: #241920;
            border-radius: 12px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.5);
            cursor: pointer;
            display: none;
            transform: scale(0.8);
            opacity: 0;
            animation: popIn 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
            border: 1px solid rgba(255, 51, 102, 0.2);
        }

        .envelope-wrapper.open {
            animation: openEnvelope 0.8s forwards;
        }

        .envelope-flap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #2d1f28, #20151d);
            clip-path: polygon(0 0, 50% 55%, 100% 0);
            transform-origin: top;
            transition: transform 0.6s ease;
            z-index: 4;
            border-radius: 12px 12px 0 0;
        }

        .envelope-wrapper.open .envelope-flap {
            transform: rotateX(180deg);
            z-index: 1;
        }

        .envelope-seal {
            position: absolute;
            top: 48%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 45px;
            height: 45px;
            background: var(--primary-pink);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: 0 4px 12px rgba(255, 51, 102, 0.4);
            z-index: 5;
            font-size: 1.2rem;
            transition: opacity 0.3s ease;
        }

        .envelope-wrapper.open .envelope-seal {
            opacity: 0;
            pointer-events: none;
        }

        /* Letter Styling */
        .letter {
            position: absolute;
            bottom: 10px;
            left: 15px;
            right: 15px;
            height: 190px;
            background: var(--letter-bg);
            color: var(--letter-text);
            border-radius: 8px;
            padding: 25px 20px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.15);
            z-index: 2;
            transition: all 0.8s cubic-bezier(0.77, 0, 0.175, 1);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .envelope-wrapper.slide-out .letter {
            transform: translateY(-160px) scale(1.15);
            height: 380px;
            z-index: 10;
            box-shadow: 0 25px 50px rgba(0,0,0,0.4);
        }

        .letter-content {
            font-family: 'Caveat', cursive;
            font-size: 1.55rem;
            line-height: 1.4;
            opacity: 0;
            transition: opacity 0.5s ease 0.4s;
            overflow-y: auto;
            max-height: 290px;
            padding-right: 5px;
        }

        .envelope-wrapper.slide-out .letter-content {
            opacity: 1;
        }

        .letter-footer {
            display: flex;
            justify-content: flex-end;
            align-items: center;
            opacity: 0;
            transition: opacity 0.5s ease 0.4s;
        }

        .envelope-wrapper.slide-out .letter-footer {
            opacity: 1;
        }

        .next-btn, .answer-btn {
            background: var(--letter-text);
            color: var(--letter-bg);
            border: none;
            padding: 8px 18px;
            font-family: 'Plus Jakarta Sans', sans-serif;
            font-size: 0.85rem;
            font-weight: 600;
            border-radius: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
            transition: background 0.2s;
        }

        .next-btn:hover, .answer-btn:hover {
            background: var(--primary-pink);
            color: white;
        }

        /* Question screen inside letter */
        .question-box {
            display: none;
            flex-direction: column;
            height: 100%;
            justify-content: space-between;
        }

        .question-title {
            font-family: 'Caveat', cursive;
            font-size: 1.75rem;
            line-height: 1.3;
            color: var(--letter-text);
        }

        .btn-group {
            display: flex;
            gap: 12px;
            margin-top: 15px;
        }

        .btn-yes {
            background: #2ecc71;
            color: white;
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            font-size: 1rem;
            transition: transform 0.2s, background 0.2s;
        }

        .btn-yes:hover {
            background: #27ae60;
            transform: scale(1.03);
        }

        .btn-no {
            background: #e74c3c;
            color: white;
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            font-size: 1rem;
            transition: transform 0.2s;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes popIn {
            to { opacity: 1; transform: scale(1); }
        }

        @keyframes openEnvelope {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        /* Custom scrollbar for letter */
        .letter-content::-webkit-scrollbar {
            width: 4px;
        }
        .letter-content::-webkit-scrollbar-thumb {
            background: rgba(44, 34, 38, 0.2);
            border-radius: 4px;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- Start Screen -->
        <div class="start-screen" id="startScreen">
            <h1>Hey du... ich muss dir was zeigen ✨</h1>
            <button class="btn" id="startBtn">Brief öffnen</button>
        </div>

        <!-- Envelope -->
        <div class="envelope-wrapper" id="envelopeWrapper">
            <div class="envelope-flap"></div>
            <div class="envelope-seal">💌</div>
            <div class="letter" id="letter">
                <!-- Page 1: Feelings -->
                <div class="letter-content" id="page1">
                    <p>Hey...<br><br>
                    Eigentlich bin ich ja eher der Typ fürs Programmieren und Code schreiben, aber für dich mache ich gerne eine Ausnahme. Seitdem wir uns kennen, gehst du mir einfach nicht mehr aus dem Kopf. Jede Minute mit dir ist unglaublich, und du bringst mich jedes Mal zum Lächeln, auch wenn du es vielleicht gar nicht merkst. 🤍</p>
                </div>
                <div class="letter-footer" id="footer1">
                    <button class="next-btn" id="nextBtn">Weiter ➔</button>
                </div>

                <!-- Page 2: The Question -->
                <div class="question-box" id="page2">
                    <div class="question-title" id="questionText">
                        Ich weiß, wir sind offiziell noch nicht zusammen... aber ich will das ändern. Willst du meine Freundin sein? ❤️
                    </div>
                    <div class="btn-group" id="btnGroup">
                        <button class="btn-yes" id="yesBtn">Ja! 🥰</button>
                        <button class="btn-no" id="noBtn">Nein 🙈</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const startBtn = document.getElementById('startBtn');
        const startScreen = document.getElementById('startScreen');
        const envelopeWrapper = document.getElementById('envelopeWrapper');
        const letter = document.getElementById('letter');
        const nextBtn = document.getElementById('nextBtn');
        const page1 = document.getElementById('page1');
        const footer1 = document.getElementById('footer1');
        const page2 = document.getElementById('page2');
        const yesBtn = document.getElementById('yesBtn');
        const noBtn = document.getElementById('noBtn');
        const questionText = document.getElementById('questionText');
        const btnGroup = document.getElementById('btnGroup');

        // Step 1: Click start -> hide start, show envelope and trigger animation
        startBtn.addEventListener('click', () => {
            startScreen.style.display = 'none';
            envelopeWrapper.style.display = 'block';
            
            setTimeout(() => {
                envelopeWrapper.classList.add('open');
                setTimeout(() => {
                    envelopeWrapper.classList.add('slide-out');
                }, 500);
            }, 300);
        });

        // Step 2: Click "Weiter" -> Switch to Question Page inside the letter
        nextBtn.addEventListener('click', () => {
            page1.style.display = 'none';
            footer1.style.display = 'none';
            page2.style.display = 'flex';
        });

        // Step 3: Click "Ja" -> Yay celebration
        yesBtn.addEventListener('click', () => {
            questionText.innerHTML = "Yay, endlich! ❤️ Ich freu mich so unglaublich doll auf uns. Du & Ich! 🥰✨";
            btnGroup.style.display = 'none';
        });

        // Step 4: Click "Nein" -> Playful reaction
        noBtn.addEventListener('click', () => {
            questionText.innerHTML = "Willst du mich verarschen? Du musst trotzdem. 😉❤️";
            btnGroup.innerHTML = '<button class="btn-yes" id="okayBtn" style="width: 100%;">Okay, na gut 🥰</button>';
            
            document.getElementById('okayBtn').addEventListener('click', () => {
                questionText.innerHTML = "Juhu! Das war die einzig richtige Entscheidung. ❤️✨";
                btnGroup.style.display = 'none';
            });
        });
    </script>
</body>
</html>
