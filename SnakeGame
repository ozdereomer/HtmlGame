<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Yılan Oyunu 🐍</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Arial', sans-serif;
            color: white;
        }

        h1 {
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            font-size: 2.5em;
        }

        .game-container {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
        }

        #gameCanvas {
            border: 4px solid white;
            border-radius: 10px;
            background: #2d3748;
            box-shadow: 0 4px 16px rgba(0,0,0,0.4);
        }

        .score-container {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            font-size: 1.3em;
            font-weight: bold;
        }

        .fruit-legend {
            margin-top: 15px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
            text-align: center;
        }

        .fruit-legend h3 {
            margin-bottom: 10px;
            font-size: 1.2em;
        }

        .fruit-items {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }

        .fruit-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1.1em;
        }

        .controls {
            margin-top: 20px;
            text-align: center;
            font-size: 1em;
            background: rgba(255, 255, 255, 0.15);
            padding: 15px;
            border-radius: 10px;
        }

        .game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.9);
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            display: none;
            z-index: 10;
        }

        .game-over h2 {
            font-size: 2.5em;
            margin-bottom: 20px;
            color: #ff6b6b;
        }

        .game-over button {
            margin-top: 20px;
            padding: 15px 40px;
            font-size: 1.2em;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            border-radius: 10px;
            color: white;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .game-over button:hover {
            transform: scale(1.05);
        }
    </style>
</head>
<body>
<h1>🐍 Yılan Oyunu 🐍</h1>

<div class="game-container">
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <div class="score-container">
        <div>Skor: <span id="score">0</span></div>
        <div>Yüksek Skor: <span id="highScore">0</span></div>
    </div>

    <div class="fruit-legend">
        <h3>Meyveler ve Puanları:</h3>
        <div class="fruit-items">
            <div class="fruit-item">🍎 Elma: +10</div>
            <div class="fruit-item">🍌 Muz: +15</div>
            <div class="fruit-item">🍍 Ananas: +20</div>
            <div class="fruit-item">⭐ Yıldız Meyvesi: +30</div>
        </div>
    </div>

    <div class="controls">
        <p><strong>Kontroller:</strong> Ok tuşları veya W-A-S-D ile oyna</p>
        <p>Başlamak için herhangi bir ok tuşuna bas!</p>
    </div>
</div>

<div class="game-over" id="gameOver">
    <h2>Oyun Bitti!</h2>
    <p style="font-size: 1.5em; margin: 10px 0;">Final Skoru: <span id="finalScore">0</span></p>
    <button onclick="restartGame()">Tekrar Oyna</button>
</div>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const scoreElement = document.getElementById('score');
    const highScoreElement = document.getElementById('highScore');
    const gameOverDiv = document.getElementById('gameOver');
    const finalScoreElement = document.getElementById('finalScore');

    const gridSize = 20;
    const tileCount = canvas.width / gridSize;

    let snake = [
        {x: 10, y: 10}
    ];
    let velocityX = 0;
    let velocityY = 0;
    let score = 0;
    let highScore = localStorage.getItem('snakeHighScore') || 0;
    highScoreElement.textContent = highScore;
    let gameStarted = false;
    let gameLoop;

    const fruits = [
        {emoji: '🍎', name: 'Elma', points: 10},
        {emoji: '🍌', name: 'Muz', points: 15},
        {emoji: '🍍', name: 'Ananas', points: 20},
        {emoji: '⭐', name: 'Yıldız Meyvesi', points: 30}
    ];

    let currentFruit = {
        x: 15,
        y: 15,
        type: fruits[0]
    };

    function randomFruit() {
        currentFruit = {
            x: Math.floor(Math.random() * tileCount),
            y: Math.floor(Math.random() * tileCount),
            type: fruits[Math.floor(Math.random() * fruits.length)]
        };
    }

    function drawGame() {
        // Arka plan
        ctx.fillStyle = '#2d3748';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // Grid çizgileri
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)';
        ctx.lineWidth = 1;
        for (let i = 0; i <= tileCount; i++) {
            ctx.beginPath();
            ctx.moveTo(i * gridSize, 0);
            ctx.lineTo(i * gridSize, canvas.height);
            ctx.stroke();
            ctx.beginPath();
            ctx.moveTo(0, i * gridSize);
            ctx.lineTo(canvas.width, i * gridSize);
            ctx.stroke();
        }

        // Yılanı çiz
        snake.forEach((segment, index) => {
            if (index === 0) {
                // Kafa - daha parlak yeşil
                ctx.fillStyle = '#48bb78';
            } else {
                // Gövde - gradyan efekti
                const opacity = 1 - (index / snake.length) * 0.3;
                ctx.fillStyle = `rgba(56, 161, 105, ${opacity})`;
            }

            ctx.fillRect(
                segment.x * gridSize + 1,
                segment.y * gridSize + 1,
                gridSize - 2,
                gridSize - 2
            );

            // Gözler (sadece kafa için)
            if (index === 0) {
                ctx.fillStyle = 'white';
                const eyeSize = 4;
                const eyeOffset = 6;

                if (velocityX !== 0 || velocityY !== 0) {
                    if (velocityY === -1) { // Yukarı
                        ctx.fillRect(segment.x * gridSize + 5, segment.y * gridSize + 4, eyeSize, eyeSize);
                        ctx.fillRect(segment.x * gridSize + 11, segment.y * gridSize + 4, eyeSize, eyeSize);
                    } else if (velocityY === 1) { // Aşağı
                        ctx.fillRect(segment.x * gridSize + 5, segment.y * gridSize + 12, eyeSize, eyeSize);
                        ctx.fillRect(segment.x * gridSize + 11, segment.y * gridSize + 12, eyeSize, eyeSize);
                    } else if (velocityX === -1) { // Sol
                        ctx.fillRect(segment.x * gridSize + 4, segment.y * gridSize + 5, eyeSize, eyeSize);
                        ctx.fillRect(segment.x * gridSize + 4, segment.y * gridSize + 11, eyeSize, eyeSize);
                    } else if (velocityX === 1) { // Sağ
                        ctx.fillRect(segment.x * gridSize + 12, segment.y * gridSize + 5, eyeSize, eyeSize);
                        ctx.fillRect(segment.x * gridSize + 12, segment.y * gridSize + 11, eyeSize, eyeSize);
                    }
                }
            }
        });

        // Meyve çiz - yanıp sönen efekt ve daha gerçekçi
        const pulseTime = Date.now() % 1000 / 1000; // 0-1 arası değer
        const pulseScale = 1 + Math.sin(pulseTime * Math.PI * 2) * 0.15; // %15 büyüme-küçülme
        const glowIntensity = 0.5 + Math.sin(pulseTime * Math.PI * 2) * 0.3;

        // Parıltı efekti
        const gradient = ctx.createRadialGradient(
            currentFruit.x * gridSize + gridSize / 2,
            currentFruit.y * gridSize + gridSize / 2,
            0,
            currentFruit.x * gridSize + gridSize / 2,
            currentFruit.y * gridSize + gridSize / 2,
            gridSize * pulseScale
        );
        gradient.addColorStop(0, `rgba(255, 255, 100, ${glowIntensity})`);
        gradient.addColorStop(1, 'rgba(255, 255, 100, 0)');

        ctx.fillStyle = gradient;
        ctx.fillRect(
            currentFruit.x * gridSize - gridSize / 2,
            currentFruit.y * gridSize - gridSize / 2,
            gridSize * 2,
            gridSize * 2
        );

        // Meyve emoji - daha büyük
        ctx.font = `${gridSize * 1.5}px Arial`;
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';

        ctx.save();
        ctx.translate(
            currentFruit.x * gridSize + gridSize / 2,
            currentFruit.y * gridSize + gridSize / 2
        );
        ctx.scale(pulseScale, pulseScale);

        // Gölge efekti
        ctx.shadowColor = 'rgba(0, 0, 0, 0.5)';
        ctx.shadowBlur = 10;
        ctx.shadowOffsetX = 2;
        ctx.shadowOffsetY = 2;

        ctx.fillText(currentFruit.type.emoji, 0, 0);
        ctx.restore();
    }

    function moveSnake() {
        if (!gameStarted) return;

        const head = {
            x: snake[0].x + velocityX,
            y: snake[0].y + velocityY
        };

        // Duvara çarpma kontrolü
        if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
            endGame();
            return;
        }

        // Kendine çarpma kontrolü
        for (let segment of snake) {
            if (head.x === segment.x && head.y === segment.y) {
                endGame();
                return;
            }
        }

        snake.unshift(head);

        // Meyve yeme kontrolü
        if (head.x === currentFruit.x && head.y === currentFruit.y) {
            score += currentFruit.type.points;
            scoreElement.textContent = score;

            if (score > highScore) {
                highScore = score;
                highScoreElement.textContent = highScore;
                localStorage.setItem('snakeHighScore', highScore);
            }

            randomFruit();
        } else {
            snake.pop();
        }
    }

    function endGame() {
        gameStarted = false;
        clearInterval(gameLoop);
        finalScoreElement.textContent = score;
        gameOverDiv.style.display = 'block';
    }

    function restartGame() {
        snake = [{x: 10, y: 10}];
        velocityX = 0;
        velocityY = 0;
        score = 0;
        scoreElement.textContent = score;
        gameStarted = false;
        gameOverDiv.style.display = 'none';
        randomFruit();
        drawGame();
    }

    function gameUpdate() {
        moveSnake();
        drawGame();
    }

    document.addEventListener('keydown', (e) => {
        if (!gameStarted && (e.key.startsWith('Arrow') || ['w', 'a', 's', 'd'].includes(e.key.toLowerCase()))) {
            gameStarted = true;
            gameLoop = setInterval(gameUpdate, 100);
        }

        switch(e.key) {
            case 'ArrowUp':
            case 'w':
            case 'W':
                if (velocityY === 0) {
                    velocityX = 0;
                    velocityY = -1;
                }
                break;
            case 'ArrowDown':
            case 's':
            case 'S':
                if (velocityY === 0) {
                    velocityX = 0;
                    velocityY = 1;
                }
                break;
            case 'ArrowLeft':
            case 'a':
            case 'A':
                if (velocityX === 0) {
                    velocityX = -1;
                    velocityY = 0;
                }
                break;
            case 'ArrowRight':
            case 'd':
            case 'D':
                if (velocityX === 0) {
                    velocityX = 1;
                    velocityY = 0;
                }
                break;
        }
    });

    // İlk çizim
    randomFruit();
    drawGame();
</script>
</body>
</html>
