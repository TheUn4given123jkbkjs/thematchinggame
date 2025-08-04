<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trò Chơi Ghép Màu Cho Trẻ Em</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .game-container {
            max-width: 800px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            padding: 30px;
            text-align: center;
        }

        h1 {
            color: #4a5568;
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .subtitle {
            color: #718096;
            font-size: 1.2rem;
            margin-bottom: 30px;
        }

        .score-board {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #f7fafc;
            padding: 15px 25px;
            border-radius: 15px;
            margin-bottom: 30px;
            border: 3px solid #e2e8f0;
        }

        .score {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2d3748;
        }

        .reset-btn {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
        }

        .reset-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255, 107, 107, 0.4);
        }

        .game-area {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 40px;
            margin-bottom: 30px;
        }

        .colors-section, .objects-section {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            border: 3px solid #e9ecef;
        }

        .section-title {
            font-size: 1.4rem;
            color: #495057;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .colors-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }

        .color-item {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 4px solid transparent;
            margin: 0 auto;
            position: relative;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .color-item:hover {
            transform: scale(1.1);
            border-color: #fff;
            box-shadow: 0 6px 20px rgba(0,0,0,0.3);
        }

        .color-item.selected {
            border-color: #ffd700;
            border-width: 6px;
            transform: scale(1.15);
        }

        .objects-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
        }

        .object-item {
            background: white;
            border: 3px solid #dee2e6;
            border-radius: 15px;
            padding: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            min-height: 100px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .object-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.15);
        }

        .object-item.correct {
            border-color: #28a745;
            background: #d4edda;
            animation: correctMatch 0.6s ease;
        }

        .object-item.wrong {
            border-color: #dc3545;
            background: #f8d7da;
            animation: wrongMatch 0.6s ease;
        }

        @keyframes correctMatch {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        @keyframes wrongMatch {
            0% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
            100% { transform: translateX(0); }
        }

        .object-emoji {
            font-size: 3rem;
            margin-bottom: 10px;
        }

        .object-name {
            font-size: 1.1rem;
            font-weight: bold;
            color: #495057;
        }

        .message {
            font-size: 1.3rem;
            font-weight: bold;
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            transition: all 0.3s ease;
        }

        .message.success {
            background: #d4edda;
            color: #155724;
            border: 2px solid #c3e6cb;
        }

        .message.error {
            background: #f8d7da;
            color: #721c24;
            border: 2px solid #f5c6cb;
        }

        .message.info {
            background: #cce7ff;
            color: #004085;
            border: 2px solid #b3d7ff;
        }

        @media (max-width: 768px) {
            .game-area {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .colors-grid, .objects-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .color-item {
                width: 60px;
                height: 60px;
            }
            
            .object-emoji {
                font-size: 2.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>🎨 Trò Chơi Ghép Màu</h1>
        <p class="subtitle">Chọn màu và ghép với đồ vật có cùng màu!</p>
        
        <div class="score-board">
            <div class="score">Điểm: <span id="score">0</span></div>
            <button class="reset-btn" onclick="resetGame()">🔄 Chơi Lại</button>
            <div class="score">Lượt: <span id="attempts">0</span></div>
        </div>

        <div class="game-area">
            <div class="colors-section">
                <h3 class="section-title">🎨 Chọn Màu</h3>
                <div class="colors-grid" id="colorsGrid">
                    <!-- Colors will be generated by JavaScript -->
                </div>
            </div>

            <div class="objects-section">
                <h3 class="section-title">🎯 Chọn Đồ Vật</h3>
                <div class="objects-grid" id="objectsGrid">
                    <!-- Objects will be generated by JavaScript -->
                </div>
            </div>
        </div>

        <div id="message" class="message" style="display: none;"></div>
    </div>

    <script>
        const gameData = {
            red: {
                color: '#ff4757',
                objects: [
                    { emoji: '🍎', name: 'Táo đỏ' },
                    { emoji: '🌹', name: 'Hoa hồng' },
                    { emoji: '❤️', name: 'Trái tim' },
                    { emoji: '🍓', name: 'Dâu tây' }
                ]
            },
            blue: {
                color: '#3742fa',
                objects: [
                    { emoji: '🌊', name: 'Sóng biển' },
                    { emoji: '🐋', name: 'Cá voi' },
                    { emoji: '💙', name: 'Trái tim xanh' },
                    { emoji: '🫐', name: 'Quả việt quất' }
                ]
            },
            green: {
                color: '#2ed573',
                objects: [
                    { emoji: '🍀', name: 'Lá cỏ' },
                    { emoji: '🐸', name: 'Ếch xanh' },
                    { emoji: '🥒', name: 'Dưa chuột' },
                    { emoji: '🌿', name: 'Lá cây' }
                ]
            },
            yellow: {
                color: '#ffa502',
                objects: [
                    { emoji: '🌞', name: 'Mặt trời' },
                    { emoji: '🍌', name: 'Chuối' },
                    { emoji: '⭐', name: 'Ngôi sao' },
                    { emoji: '🌻', name: 'Hoa hướng dương' }
                ]
            }
        };

        let selectedColor = null;
        let score = 0;
        let attempts = 0;
        let currentRound = [];

        function initGame() {
            generateColors();
            generateNewRound();
            updateScore();
        }

        function generateColors() {
            const colorsGrid = document.getElementById('colorsGrid');
            colorsGrid.innerHTML = '';
            
            Object.keys(gameData).forEach(colorKey => {
                const colorDiv = document.createElement('div');
                colorDiv.className = 'color-item';
                colorDiv.style.backgroundColor = gameData[colorKey].color;
                colorDiv.dataset.color = colorKey;
                colorDiv.onclick = () => selectColor(colorKey, colorDiv);
                colorsGrid.appendChild(colorDiv);
            });
        }

        function selectColor(colorKey, element) {
            // Remove previous selection
            document.querySelectorAll('.color-item').forEach(item => {
                item.classList.remove('selected');
            });
            
            // Select new color
            element.classList.add('selected');
            selectedColor = colorKey;
            showMessage('Bạn đã chọn màu! Giờ hãy chọn đồ vật có cùng màu.', 'info');
        }

        function generateNewRound() {
            const objectsGrid = document.getElementById('objectsGrid');
            objectsGrid.innerHTML = '';
            currentRound = [];

            // Get one object from each color
            Object.keys(gameData).forEach(colorKey => {
                const objects = gameData[colorKey].objects;
                const randomObject = objects[Math.floor(Math.random() * objects.length)];
                currentRound.push({
                    ...randomObject,
                    color: colorKey
                });
            });

            // Shuffle the objects
            currentRound.sort(() => Math.random() - 0.5);

            // Create object elements
            currentRound.forEach((obj, index) => {
                const objDiv = document.createElement('div');
                objDiv.className = 'object-item';
                objDiv.innerHTML = `
                    <div class="object-emoji">${obj.emoji}</div>
                    <div class="object-name">${obj.name}</div>
                `;
                objDiv.onclick = () => selectObject(obj, objDiv);
                objectsGrid.appendChild(objDiv);
            });
        }

        function selectObject(obj, element) {
            if (!selectedColor) {
                showMessage('Hãy chọn một màu trước!', 'error');
                return;
            }

            attempts++;
            
            if (obj.color === selectedColor) {
                // Correct match
                score += 10;
                element.classList.add('correct');
                showMessage('🎉 Chính xác! Bạn đã ghép đúng màu!', 'success');
                
                setTimeout(() => {
                    generateNewRound();
                    resetSelection();
                }, 1500);
            } else {
                // Wrong match
                element.classList.add('wrong');
                showMessage('❌ Sai rồi! Hãy thử lại nhé!', 'error');
                
                setTimeout(() => {
                    element.classList.remove('wrong');
                }, 1000);
            }
            
            updateScore();
        }

        function resetSelection() {
            selectedColor = null;
            document.querySelectorAll('.color-item').forEach(item => {
                item.classList.remove('selected');
            });
            showMessage('Chọn màu mới cho lượt tiếp theo!', 'info');
        }

        function showMessage(text, type) {
            const messageDiv = document.getElementById('message');
            messageDiv.textContent = text;
            messageDiv.className = `message ${type}`;
            messageDiv.style.display = 'block';
            
            if (type === 'info') {
                setTimeout(() => {
                    messageDiv.style.display = 'none';
                }, 3000);
            }
        }

        function updateScore() {
            document.getElementById('score').textContent = score;
            document.getElementById('attempts').textContent = attempts;
        }

        function resetGame() {
            score = 0;
            attempts = 0;
            selectedColor = null;
            updateScore();
            generateNewRound();
            document.querySelectorAll('.color-item').forEach(item => {
                item.classList.remove('selected');
            });
            showMessage('Trò chơi đã được khởi động lại! Chúc bạn chơi vui vẻ!', 'info');
        }

        // Initialize the game when page loads
        window.onload = initGame;
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'969e3a9602bdfe11',t:'MTc1NDMxMjMzNC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
