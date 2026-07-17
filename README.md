[index.html](https://github.com/user-attachments/files/30111195/index.html)[Uploading inde<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>TER — Генератор паролей</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* Логотип */
        .logo-container {
            position: absolute;
            top: 20px;
            left: 20px;
        }
        .logo {
            font-size: 24px;
            font-weight: 900;
            color: #000000;
            background-color: #ffb6c1;
            border: 3px solid #000000;
            border-radius: 12px;
            padding: 8px 16px;
            letter-spacing: 1px;
            box-shadow: 2px 2px 0px #000;
            user-select: none;
        }

        /* Контент сайта */
        .main-content {
            flex: 1;
            margin-top: 120px;
            padding: 20px;
        }
        
        /* Розовые кнопки с черной рамкой */
        .btn {
            padding: 14px 28px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            color: #000000;
            border: 3px solid #000000;
            border-radius: 25px;
            margin: 10px;
            outline: none;
        }

        /* Цвета кнопок сложности */
        #btn-easy { background-color: #2ecc71; }
        #btn-easy:hover { background-color: #27ae60; }

        #btn-good { background-color: #f1c40f; }
        #btn-good:hover { background-color: #f39c12; }

        #btn-hard { background-color: #e74c3c; }
        #btn-hard:hover { background-color: #c0392b; }

        /* Кнопка копирования */
        .btn-copy {
            background-color: #ff69b4;
            display: inline-block;
            margin-top: 15px;
        }
        .btn-copy:hover {
            background-color: #ff1493;
            color: #fff;
        }
        
        .buttons-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            margin-top: 20px;
        }
        
        #password-display {
            font-size: 26px;
            font-weight: bold;
            color: #000;
            margin-top: 25px;
            letter-spacing: 2px;
            background: #fff;
            display: inline-block;
            padding: 12px 25px;
            border-radius: 15px;
            border: 3px solid #000;
            min-width: 200px;
        }

        /* Хакерский эффект Матрицы */
        .matrix-hack {
            font-family: 'Courier New', Courier, monospace !important;
            color: #2ecc71 !important;
            background: #000000 !important;
            text-shadow: 0 0 8px #2ecc71;
            border-color: #2ecc71 !important;
        }
        
        /* Пожелание безопасности */
        .secure-wish {
            font-size: 16px;
            font-style: italic;
            color: #28a745;
            font-weight: bold;
            margin-top: 15px;
        }
        
        .hidden { display: none !important; }
        #timer-text { font-size: 20px; font-weight: bold; color: #ff1493; margin-top: 20px; }

        /* Подвал */
        footer {
            background-color: #ffffff;
            border-top: 3px solid #000000;
            padding: 20px;
            margin-top: auto;
        }
        .abbreviation-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
            color: #000;
        }
        .abbreviation-list {
            display: flex;
            justify-content: center;
            gap: 40px;
            font-size: 16px;
            font-weight: bold;
        }
        .abbreviation-item span {
            background-color: #ffb6c1;
            padding: 2px 8px;
            border: 2px solid #000;
            border-radius: 5px;
            margin-right: 5px;
        }
    </style>
</head>
<body>

    <!-- Логотип -->
    <div class="logo-container">
        <div class="logo">TER</div>
    </div>

    <!-- Главный контент -->
    <div class="main-content">
        <h1 id="main-title">Выберите тип пароля:</h1>

        <!-- Блок генерации -->
        <div id="generation-block">
            <div id="buttons-group" class="buttons-container">
                <button id="btn-easy" class="btn">Легкий пароль (6 символов — 4 сек)</button>
                <button id="btn-good" class="btn">Хороший пароль (8 символов — 6 сек)</button>
                <button id="btn-hard" class="btn">Надежный пароль (10 символов — 15 сек)</button>
            </div>
            
            <div id="timer-text" class="hidden">Генерация... Осталось секунд: <span id="seconds-left">0</span></div>
            
            <!-- Поле результата -->
            <div id="result-block" class="hidden">
                <div id="password-display"></div>
                <div id="wish-block" class="secure-wish hidden">Надеюсь, наш пароль надёжно защитит ваш аккаунт! 🛡️</div>
                <br>
                <button id="copy-btn" class="btn btn-copy hidden">Выделить пароль</button>
            </div>
        </div>
    </div>

    <!-- Подвал -->
    <footer>
        <div class="abbreviation-title">Расшифровка названия платформы:</div>
        <div class="abbreviation-list">
            <div class="abbreviation-item"><span>T</span> Технологии</div>
            <div class="abbreviation-item"><span>E</span> Единство</div>
            <div class="abbreviation-item"><span>R</span> Результат</div>
        </div>
    </footer>

    <script>
        const title = document.getElementById('main-title');
        const buttonsGroup = document.getElementById('buttons-group');
        const timerText = document.getElementById('timer-text');
        const secondsLeftText = document.getElementById('seconds-left');
        const resultBlock = document.getElementById('result-block');
        const passwordDisplay = document.getElementById('password-display');
        const wishBlock = document.getElementById('wish-block');
        const copyBtn = document.getElementById('copy-btn');

        let countdownInterval;
        let matrixInterval;

        function startTimer(length, time) {
            clearInterval(countdownInterval);
            clearInterval(matrixInterval);

            buttonsGroup.classList.add('hidden');
            wishBlock.classList.add('hidden');
            copyBtn.classList.add('hidden');
            passwordDisplay.classList.remove('matrix-hack');
            
            title.textContent = 'Идет дешифровка и подбор комбинаций...';
            
            let secondsLeft = time;
            secondsLeftText.textContent = secondsLeft;
            timerText.classList.remove('hidden');
            
            passwordDisplay.classList.add('matrix-hack');
            resultBlock.classList.remove('hidden');

            const matrixChars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()';
            
            matrixInterval = setInterval(function() {
                let fakePass = '';
                for (let i = 0; i < length; i++) {
                    fakePass += matrixChars.charAt(Math.floor(Math.random() * matrixChars.length));
                }
                passwordDisplay.textContent = fakePass;
            }, 50);

            countdownInterval = setInterval(function() {
                secondsLeft--;
                secondsLeftText.textContent = secondsLeft;
                
                if (secondsLeft <= 0) {
                    clearInterval(countdownInterval);
                    clearInterval(matrixInterval);
                    timerText.classList.add('hidden');
                    generatePassword(length);
                }
            }, 1000);
        }

        document.getElementById('btn-easy').onclick = function() { startTimer(6, 4); };
        document.getElementById('btn-good').onclick = function() { startTimer(8, 6); };
        document.getElementById('btn-hard').onclick = function() { startTimer(10, 15); };

        function generatePassword(length) {
            passwordDisplay.classList.remove('matrix-hack');

            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let result = '';
            for (let i = 0; i < length; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }

            title.textContent = 'Ваш пароль успешно сгенерирован!';
            passwordDisplay.textContent = result;
            
            buttonsGroup.classList.remove('hidden');
            wishBlock.classList.remove('hidden');
            copyBtn.classList.remove('hidden');
        }

        // Самый примитивный способ: кнопка просто мгновенно выделяет текст цветом для ручного Ctrl+C
        copyBtn.onclick = function() {
            const range = document.createRange();
            range.selectNode(passwordDisplay);
            window.getSelection().removeAllRanges();
            window.getSelection().addRange(range);
            
            const originalText = copyBtn.textContent;
            copyBtn.textContent = 'Теперь нажмите Ctrl + C!';
            copyBtn.style.backgroundColor = '#007bff';
            
            setTimeout(function() {
                copyBtn.textContent = originalText;
                copyBtn.style.backgroundColor = '#ff69b4';
                window.getSelection().removeAllRanges();
            }, 2500);
        };
    </script>

</body>
</html>
x.html…]()
