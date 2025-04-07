<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Dante's Inferno Adventure</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            margin: 0;
            overflow: hidden;
            background-color: #1a1a1a;
            font-family: 'Arial', sans-serif;
            color: white;
        }

        #gameContainer {
            position: relative;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
        }

        #backgroundLayer {
            position: absolute;
            width: 10000%;
            height: 100%;
            background-image: url('https://i.postimg.cc/FRnyFwQF/fdaf6de2-458b-4e18-8d9d-d3a0a08f6158.png');
            background-size: contain;
            background-position: left top;
            background-repeat: repeat-x;
            z-index: 1;
            transition: transform 0.1s ease-out;
        }

        /* Updated level 2 background with new image */
        .level-two-background {
            background-image: url('https://i.postimg.cc/R0BxKp7p/39c65714-400a-4756-b649-09ed580d2ca1.png') !important;
            background-size: contain;
            background-repeat: repeat-x !important;
            background-color: #000000 !important;
        }
       
        /* Updated level 3 background with new image */
        .level-three-background {
            background-image: url('https://i.postimg.cc/fbxgsw0B/d685965a-3452-435f-9664-f202d5e53633.jpg') !important;
            background-size: contain;
            background-repeat: repeat-x !important;
            background-color: #000000 !important;
        }

        #player {
            position: absolute;
            width: 80px;
            height: 120px;
            background-image: url('https://i.postimg.cc/NfDdpJvP/Senza-titolo-3-20250310144624.png');
            background-size: contain;
            background-repeat: no-repeat;
            z-index: 10;
            transition: transform 0.1s;
        }

        .player-invincible {
            animation: blink 0.3s infinite;
        }

        @keyframes blink {
            0% { opacity: 1; }
            50% { opacity: 0.3; }
            100% { opacity: 1; }
        }

        .enemy {
            position: absolute;
            width: 70px;
            height: 70px;
            background-size: contain;
            background-repeat: no-repeat;
            z-index: 5;
            transition: transform 0.2s, left 0.3s, bottom 0.3s;
        }

        .demon-type-1 {
            background-image: url('https://i.postimg.cc/hPd7dpmj/Senza-titolo-2-20250224154443.png');
            width: 90px;
            height: 90px;
        }

        .demon-type-2 {
            background-image: url('https://i.postimg.cc/90qzyQrS/Senza-titolo-2-20250224154252.png');
            width: 100px;
            height: 100px;
        }
       
        .demon-type-3 {
            background-image: url('https://i.postimg.cc/4d0mK9NV/Senza-titolo-2-20250224154646.png');
            width: 95px;
            height: 95px;
        }
       
        /* Level 2 enemy styles */
        .level-two-enemy-1 {
            background-image: url('https://i.postimg.cc/1zdRkz6h/2-removebg-preview.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-two-enemy-2 {
            background-image: url('https://i.postimg.cc/VdBG6yqt/3-removebg-preview.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-two-enemy-3 {
            background-image: url('https://i.postimg.cc/K1RS6vTf/4-removebg-preview.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-two-enemy-4 {
            background-image: url('https://i.postimg.cc/mz62YFNx/5-removebg-preview.png') !important;
            width: 120px !important;
            height: 120px !important;
        }

        /* New level 3 enemy styles */
        .level-three-enemy-1 {
            background-image: url('https://i.postimg.cc/Qx42Yxp7/Cattura-removebg-preview-7.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-three-enemy-2 {
            background-image: url('https://i.postimg.cc/XqNPxTqx/Senza-titolo-4-20250310150944.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-three-enemy-3 {
            background-image: url('https://i.postimg.cc/NjKW3q4z/Cattura-removebg-preview-6.png') !important;
            width: 120px !important;
            height: 120px !important;
        }
       
        .level-three-enemy-4 {
            background-image: url('https://i.postimg.cc/bNB5HVhH/2222-removebg-preview.png') !important;
            width: 120px !important;
            height: 120px !important;
        }

        .portal {
            position: absolute;
            width: 100px;
            height: 100px;
            background-size: contain;
            background-repeat: no-repeat;
            z-index: 5;
            animation: portalPulse 1.5s infinite alternate;
        }

        /* Different portal for level 1 */
        .level-one-portal {
            background-image: url('https://i.postimg.cc/sBHkdx66/portal.jpg') !important;
        }

        /* Regular portal for other levels */
        .other-level-portal {
            background-image: url('https://i.postimg.cc/jWvCgFnH/portal.png') !important;
        }

        @keyframes portalPulse {
            from { transform: scale(1); }
            to { transform: scale(1.1); }
        }

        #gameStats {
            position: fixed;
            top: 20px;
            left: 20px;
            color: #fff;
            font-size: 24px;
            z-index: 20;
            text-shadow: 2px 2px #000;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 15px;
            border-radius: 10px;
            border: 2px solid #4a4a4a;
        }

        .game-over {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            z-index: 100;
            border: 3px solid #4a4a4a;
            min-width: 300px;
        }

        .game-over h2 {
            font-size: 36px;
            color: #ff0000;
            margin-bottom: 20px;
            text-shadow: 2px 2px #000;
        }

        .game-over p {
            font-size: 24px;
            margin: 10px 0;
            color: #fff;
        }

        .game-over button {
            margin-top: 20px;
            padding: 15px 30px;
            font-size: 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .game-over button:hover {
            background-color: #45a049;
        }

        #controls {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.5);
            padding: 10px;
            border-radius: 10px;
            text-align: center;
            z-index: 20;
            border: 2px solid #4a4a4a;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .level-complete {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            z-index: 100;
            border: 3px solid #4a4a4a;
        }

        .level-complete h2 {
            font-size: 36px;
            color: #4CAF50;
            margin-bottom: 20px;
        }

        #soundButton {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            background: rgba(0, 0, 0, 0.5);
            border: none;
            color: white;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
        }
       
        #startScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            color: white;
            text-align: center;
            padding: 20px;
        }
       
        #startScreen h1 {
            font-size: 60px;
            margin-bottom: 20px;
            text-shadow: 3px 3px 5px rgba(0, 0, 0, 0.8);
        }
       
        #startScreen p {
            font-size: 24px;
            margin-bottom: 15px;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
            border-radius: 10px;
            max-width: 600px;
        }
       
        #startScreen .instructions {
            font-size: 28px;
            margin-top: 40px;
            animation: pulse 1.5s infinite;
        }

        #characterDisplay {
            display: flex;
            justify-content: center;
            margin-top: 30px;
        }

        #danteChar, #demonChar {
            height: 150px;
            margin: 0 20px;
            filter: drop-shadow(5px 5px 5px rgba(0, 0, 0, 0.7));
        }

        #debugInfo {
            position: fixed;
            bottom: 70px;
            left: 20px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 10px;
            border-radius: 5px;
            font-size: 16px;
            z-index: 1000;
        }
       
        #livesContainer {
            display: flex;
            align-items: center;
        }
       
        .life-icon {
            width: 25px;
            height: 25px;
            background-color: #ff0000;
            border-radius: 50%;
            margin-left: 5px;
            display: inline-block;
            box-shadow: 0 0 5px #ff0000;
        }
       
        /* Final image overlay for level 3 completion */
        #finalImage {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://i.postimg.cc/fLnbd73s/b5a34228-cfa9-47e3-8a87-8a7b7aa9d1aa.png');
            background-size: cover;
            background-position: center;
            z-index: 150;
            display: none;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <div id="backgroundLayer"></div>
       
        <div id="startScreen">
            <h1>Dante's Adventure</h1>
            <p>Aiuta Dante a attraversare l'Inferno evitando i demoni!</p>
           
            <div id="characterDisplay">
                <img id="danteChar" src="https://i.postimg.cc/NfDdpJvP/Senza-titolo-3-20250310144624.png" alt="Dante">
                <img id="demonChar" src="https://i.postimg.cc/hPd7dpmj/Senza-titolo-2-20250224154443.png" alt="Demone">
            </div>
           
            <p class="instructions">Premi ENTER per iniziare</p>
        </div>

        <div id="gameStats">
            <div>PUNTEGGIO: <span id="scoreDisplay">0</span></div>
            <div>MONDO: <span id="worldDisplay">1-1</span></div>
            <div>TEMPO: <span id="timeDisplay">400</span></div>
            <div id="livesContainer">VITE: <span id="livesDisplay"></span></div>
        </div>
       
        <div id="controls">
            ← → per muoversi | SPAZIO per saltare | SHIFT per correre
        </div>
       
        <button id="soundButton">🔊</button>
       
        <div id="debugInfo">
            Screen: <span id="screenPositionDisplay">0</span>
        </div>
    </div>

    <script>
        // Wait for DOM to fully load
        document.addEventListener('DOMContentLoaded', function() {
            // Get DOM elements
            const startScreen = document.getElementById('startScreen');
            const gameContainer = document.getElementById('gameContainer');
            const backgroundLayer = document.getElementById('backgroundLayer');
            const screenPositionDisplay = document.getElementById('screenPositionDisplay');
            const livesDisplay = document.getElementById('livesDisplay');
            const scoreDisplay = document.getElementById('scoreDisplay');
            const timeDisplay = document.getElementById('timeDisplay');
            const worldDisplay = document.getElementById('worldDisplay');
            const soundButton = document.getElementById('soundButton');
           
            let player, gameLoopRunning = false;
            let gameElements = [];
            let timerInterval;
           
            // Game variables
            let lives = 15;
            let isInvincible = false;
            let invincibleDuration = 2000;
            let playerX = 100;
            let playerY = 100;
            let isJumping = false;
            let isFalling = false;
            let jumpHeight = 0;
            let jumpMax = 150;
            let moveSpeed = 10; // Reduced from 10 to 5 to fix movement speed
            let facingRight = true;
            let score = 0;
            let timeRemaining = 400;
            let portalObject = null;
            let currentLevel = 1;
           
            // Background variables
            let backgroundPos = 0;
            let levelWidth = 50000;
            let viewportWidth = window.innerWidth;
            let maxBackgroundPos = levelWidth - viewportWidth;
            let currentScreen = 0;
            let screenWidth = viewportWidth;
           
            // Player position constraints
            let playerMinX = 50; // Minimum X position for player (left edge)
            let playerMaxX = viewportWidth / 2; // Maximum X position for player (center of screen)
           
            // Level 1 demon types and positions
            const demonTypes = [
                { type: 1, url: 'https://i.postimg.cc/hPd7dpmj/Senza-titolo-2-20250224154443.png', speed: 0.8 },
                { type: 2, url: 'https://i.postimg.cc/90qzyQrS/Senza-titolo-2-20250224154252.png', speed: 1.2 },
                { type: 3, url: 'https://i.postimg.cc/4d0mK9NV/Senza-titolo-2-20250224154646.png', speed: 1.6 }
            ];
           
            // Level 2 demon types with new images
            const level2DemonTypes = [
                { type: 1, url: 'https://i.postimg.cc/1zdRkz6h/2-removebg-preview.png', speed: 0.8, class: 'level-two-enemy-1' },
                { type: 2, url: 'https://i.postimg.cc/VdBG6yqt/3-removebg-preview.png', speed: 1.2, class: 'level-two-enemy-2' },
                { type: 3, url: 'https://i.postimg.cc/K1RS6vTf/4-removebg-preview.png', speed: 1.6, class: 'level-two-enemy-3' },
                { type: 4, url: 'https://i.postimg.cc/mz62YFNx/5-removebg-preview.png', speed: 1.4, class: 'level-two-enemy-4' }
            ];
           
            // Level 3 demon types with new images
            const level3DemonTypes = [
                { type: 1, url: 'https://i.postimg.cc/Qx42Yxp7/Cattura-removebg-preview-7.png', speed: 0.8, class: 'level-three-enemy-1' },
                { type: 2, url: 'https://i.postimg.cc/XqNPxTqx/Senza-titolo-4-20250310150944.png', speed: 1.2, class: 'level-three-enemy-2' },
                { type: 3, url: 'https://i.postimg.cc/NjKW3q4z/Cattura-removebg-preview-6.png', speed: 1.6, class: 'level-three-enemy-3' },
                { type: 4, url: 'https://i.postimg.cc/bNB5HVhH/2222-removebg-preview.png', speed: 1.4, class: 'level-three-enemy-4' }
            ];
           
            // Array for active demons
            let activeDemons = [];
           
            // Key state tracking
            const keysPressed = {
                ArrowLeft: false,
                ArrowRight: false,
                ArrowUp: false,
                Space: false,
                ShiftLeft: false
            };
           
            // Event listeners for keyboard
            function setupKeyboardListeners() {
                document.addEventListener('keydown', function(event) {
                    if (event.key === 'Enter' && startScreen.style.display !== 'none') {
                        startGame();
                        return;
                    }
                   
                    if (event.key === 'ArrowLeft') keysPressed.ArrowLeft = true;
                    if (event.key === 'ArrowRight') keysPressed.ArrowRight = true;
                    if (event.key === 'ArrowUp') keysPressed.ArrowUp = true;
                    if (event.key === ' ' || event.key === 'Space') keysPressed.Space = true;
                    if (event.key === 'Shift') keysPressed.ShiftLeft = true;
                });
               
                document.addEventListener('keyup', function(event) {
                    if (event.key === 'ArrowLeft') keysPressed.ArrowLeft = false;
                    if (event.key === 'ArrowRight') keysPressed.ArrowRight = false;
                    if (event.key === 'ArrowUp') keysPressed.ArrowUp = false;
                    if (event.key === ' ' || event.key === 'Space') keysPressed.Space = false;
                    if (event.key === 'Shift') keysPressed.ShiftLeft = false;
                });
            }
           
            // Initialize lives display
            function initLives() {
                livesDisplay.innerHTML = '';
                for (let i = 0; i < lives; i++) {
                    const lifeIcon = document.createElement('span');
                    lifeIcon.className = 'life-icon';
                    livesDisplay.appendChild(lifeIcon);
                }
            }
           
            // Update lives display
            function updateLives() {
                livesDisplay.innerHTML = '';
                for (let i = 0; i < lives; i++) {
                    const lifeIcon = document.createElement('span');
                    lifeIcon.className = 'life-icon';
                    livesDisplay.appendChild(lifeIcon);
                }
               
                if (lives <= 0) {
                    gameOver();
                }
            }
           
            // Create a demon with specific type
            function createDemon(typeIndex, xPosition) {
                if (currentLevel === 1) {
                    // Limit to max 3 demons (one of each type)
                    if (activeDemons.some(demon => demon.type === demonTypes[typeIndex].type && demon.active)) {
                        return null;
                    }
                   
                    const demon = document.createElement('div');
                    demon.className = 'enemy';
                    demon.classList.add(`demon-type-${demonTypes[typeIndex].type}`);
                   
                    demon.style.left = xPosition + 'px';
                    demon.style.bottom = '100px';
                    gameContainer.appendChild(demon);
                    gameElements.push(demon);
                   
                    // Create demon object with reduced hitbox (75% of visual size)
                    let width = typeIndex === 0 ? 90 : (typeIndex === 1 ? 100 : 95);
                    let height = typeIndex === 0 ? 90 : (typeIndex === 1 ? 100 : 95);
                   
                    return {
                        element: demon,
                        x: xPosition,
                        y: 100,
                        width: width * 0.75,
                        height: height * 0.75,
                        type: demonTypes[typeIndex].type,
                        speed: demonTypes[typeIndex].speed,
                        active: true,
                        relativeX: xPosition - backgroundPos
                    };
                } else if (currentLevel === 2) {
                    // For level 2, use the new demon types
                    if (activeDemons.some(demon => demon.type === level2DemonTypes[typeIndex].type && demon.active)) {
                        return null;
                    }
                   
                    const demon = document.createElement('div');
                    demon.className = 'enemy';
                    demon.classList.add(level2DemonTypes[typeIndex].class);
                   
                    demon.style.left = xPosition + 'px';
                    demon.style.bottom = '100px';
                    gameContainer.appendChild(demon);
                    gameElements.push(demon);
                   
                    // All level 2 enemies have the same size
                    let width = 120;
                    let height = 120;
                   
                    return {
                        element: demon,
                        x: xPosition,
                        y: 100,
                        width: width * 0.75,
                        height: height * 0.75,
                        type: level2DemonTypes[typeIndex].type,
                        speed: level2DemonTypes[typeIndex].speed,
                        active: true,
                        relativeX: xPosition - backgroundPos
                    };
                } else if (currentLevel === 3) {
                    // For level 3, use the new demon types
                    if (activeDemons.some(demon => demon.type === level3DemonTypes[typeIndex].type && demon.active)) {
                        return null;
                    }
                   
                    const demon = document.createElement('div');
                    demon.className = 'enemy';
                    demon.classList.add(level3DemonTypes[typeIndex].class);
                   
                    demon.style.left = xPosition + 'px';
                    demon.style.bottom = '100px';
                    gameContainer.appendChild(demon);
                    gameElements.push(demon);
                   
                    // All level 3 enemies have the same size
                    let width = 120;
                    let height = 120;
                   
                    return {
                        element: demon,
                        x: xPosition,
                        y: 100,
                        width: width * 0.75,
                        height: height * 0.75,
                        type: level3DemonTypes[typeIndex].type,
                        speed: level3DemonTypes[typeIndex].speed,
                        active: true,
                        relativeX: xPosition - backgroundPos
                    };
                }
            }
           
            // Check for demons based on screen position
            function manageDemons() {
                // Level 1 demons
                if (currentLevel === 1) {
                    if (currentScreen === 4 && !activeDemons.some(demon => demon.type === 1 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(0, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 6 && !activeDemons.some(demon => demon.type === 2 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(1, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 8 && !activeDemons.some(demon => demon.type === 3 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(2, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                }
                // Level 2 demons
                else if (currentLevel === 2) {
                    if (currentScreen === 0 && !activeDemons.some(demon => demon.type === 1 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(0, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 6 && !activeDemons.some(demon => demon.type === 2 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(1, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 12 && !activeDemons.some(demon => demon.type === 3 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(2, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 18 && !activeDemons.some(demon => demon.type === 4 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(3, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                }
                // Level 3 demons with updated positions and images
                else if (currentLevel === 3) {
                    // New demons on specific screens for level 3
                    if (currentScreen === 6 && !activeDemons.some(demon => demon.type === 1 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(0, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 2 && !activeDemons.some(demon => demon.type === 2 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(1, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 4 && !activeDemons.some(demon => demon.type === 3 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(2, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                   
                    if (currentScreen === 8 && !activeDemons.some(demon => demon.type === 4 && demon.active)) {
                        const demonXPosition = backgroundPos + viewportWidth * 0.8;
                        const newDemon = createDemon(3, demonXPosition);
                        if (newDemon) activeDemons.push(newDemon);
                    }
                }
               
                // Update existing demons
                updateDemons();
            }
           
            // Update demon positions
            function updateDemons() {
                activeDemons.forEach(demon => {
                    if (!demon.active) return;
                   
                    const relativeX = demon.x - backgroundPos;
                    const moveTowardsPlayer = playerX > relativeX ? 1 : -1;
                   
                    demon.x += moveTowardsPlayer * demon.speed;
                    demon.element.style.left = (demon.x - backgroundPos) + 'px';
                    demon.relativeX = demon.x - backgroundPos;
                   
                    // Flip demon based on direction
                    if (moveTowardsPlayer > 0) {
                        demon.element.style.transform = 'scaleX(1)';
                    } else {
                        demon.element.style.transform = 'scaleX(-1)';
                    }
                   
                    // Check for collision with player if player is not invincible
                    if (!isInvincible && isColliding(playerX, playerY, 80, 120, demon.relativeX, demon.y, demon.width, demon.height)) {
                        handlePlayerHit();
                    }
                });
            }
           
            // Collision detection function
            function isColliding(x1, y1, width1, height1, x2, y2, width2, height2) {
                return (
                    x1 < x2 + width2 &&
                    x1 + width1 > x2 &&
                    y1 < y2 + height2 &&
                    y1 + height1 > y2
                );
            }
           
            // Handle player being hit
            function handlePlayerHit() {
                if (isInvincible) return;
               
                lives--;
                updateLives();
               
                // Make player invincible temporarily
                isInvincible = true;
                player.classList.add('player-invincible');
               
                setTimeout(() => {
                    isInvincible = false;
                    player.classList.remove('player-invincible');
                }, invincibleDuration);
            }
           
           // Create a portal at specific screen positions based on level
function createPortal() {
    // Only create portal if it doesn't exist
    if (portalObject) return;
   
    const portal = document.createElement('div');
    portal.className = 'portal';
   
    // Different portal for level 1
    if (currentLevel === 1) {
        portal.classList.add('level-one-portal');
    } else {
        portal.classList.add('other-level-portal');
    }
   
    // Set different positions based on the current level
    let portalXPosition;
   
    if (currentLevel === 1) {
        // Portal at screen 12 for level 1
        portalXPosition = 12 * screenWidth + (screenWidth / 2);
    } else if (currentLevel === 2) {
        // Portal at screen 20 for level 2
        portalXPosition = 20 * screenWidth + (screenWidth / 2);
    } else if (currentLevel === 3) {
        // Portal at screen 12 for level 3
        portalXPosition = 12 * screenWidth + (screenWidth / 2);
    }
   
    portal.style.left = (portalXPosition - backgroundPos) + 'px';
    portal.style.bottom = '100px';
    gameContainer.appendChild(portal);
    gameElements.push(portal);
   
    portalObject = {
        element: portal,
        x: portalXPosition,
        active: true
    };
}
           
            // Move to the next level when player reaches portal
            function nextLevel() {
                clearInterval(timerInterval);
               
                // Remove all game elements
                activeDemons.forEach(demon => {
                    if (demon.element) {
                        gameContainer.removeChild(demon.element);
                    }
                });
               
                if (portalObject && portalObject.element) {
                    gameContainer.removeChild(portalObject.element);
                }
               
                activeDemons = [];
                portalObject = null;
               
                // Create and show level complete screen
                const levelComplete = document.createElement('div');
                levelComplete.className = 'level-complete';
                levelComplete.innerHTML = `
                    <h2>Level ${currentLevel} Complete!</h2>
                    <p>Score: ${score}</p>
                    <p>Time Bonus: ${timeRemaining * 10}</p>
                    <p>Total Score: ${score + (timeRemaining * 10)}</p>
                `;
                gameContainer.appendChild(levelComplete);
               
                // Update score with time bonus
                score += timeRemaining * 10;
                scoreDisplay.innerText = score;
               
                // Reset game state for next level
                setTimeout(() => {
                    gameContainer.removeChild(levelComplete);
                    currentLevel++;
                   
                    // If reached final level, show game won screen
                    if (currentLevel > 3) {
                        showGameWon();
                        return;
                    }
                   
                    // Reset player and game state
                    playerX = 100;
                    backgroundPos = 0;
                    isJumping = false;
                    isFalling = false;
                    jumpHeight = 0;
                    timeRemaining = 400;
                   
                    // Update level display
                    worldDisplay.innerText = `${currentLevel}-1`;
                   
                    // Change background for level 2
                    if (currentLevel === 2) {
                        backgroundLayer.classList.add('level-two-background');
                    }
                    // Change background for level 3
                    else if (currentLevel === 3) {
                        backgroundLayer.classList.remove('level-two-background');
                        backgroundLayer.classList.add('level-three-background');
                    }
                   
                    updateBackground();
                    createPortal();
                   
                    // Restart timer
                    startTimer();
                }, 3000);
            }
           
           // Show game won screen with full-screen image
function showGameWon() {
    // Create or get the final image div
    let finalImage = document.getElementById('finalImage');
    if (!finalImage) {
        finalImage = document.createElement('div');
        finalImage.id = 'finalImage';
        gameContainer.appendChild(finalImage);
    }
   
    // Display the final image
    finalImage.style.display = 'block';
   
    // Add a restart button on top of the image
    const restartButton = document.createElement('button');
    restartButton.id = 'restartButton';
    restartButton.innerText = 'Play Again';
    restartButton.style.position = 'fixed';
    restartButton.style.bottom = '50px';
    restartButton.style.left = '50%';
    restartButton.style.transform = 'translateX(-50%)';
    restartButton.style.padding = '15px 30px';
    restartButton.style.fontSize = '20px';
    restartButton.style.backgroundColor = '#4CAF50';
    restartButton.style.color = 'white';
    restartButton.style.border = 'none';
    restartButton.style.borderRadius = '5px';
    restartButton.style.cursor = 'pointer';
    restartButton.style.zIndex = '200';
   
    finalImage.appendChild(restartButton);
   
    // Add event listener to restart button
    restartButton.addEventListener('click', function() {
        location.reload();
    });
   
    gameLoopRunning = false;
}
           
            // Game over function
            function gameOver() {
                clearInterval(timerInterval);
               
                const gameOverScreen = document.createElement('div');
                gameOverScreen.className = 'game-over';
                gameOverScreen.innerHTML = `
                    <h2>Game Over</h2>
                    <p>Your Score: ${score}</p>
                    <button id="restartButton">Try Again</button>
                `;
                gameContainer.appendChild(gameOverScreen);
               
                document.getElementById('restartButton').addEventListener('click', function() {
                    location.reload();
                });
               
                gameLoopRunning = false;
            }
           
            // Start the game timer
            function startTimer() {
                if (timerInterval) {
                    clearInterval(timerInterval);
                }
               
                timerInterval = setInterval(() => {
                    timeRemaining--;
                    timeDisplay.innerText = timeRemaining;
                   
                    if (timeRemaining <= 0) {
                        clearInterval(timerInterval);
                        gameOver();
                    }
                }, 1000);
            }
           
            // Create player character
            function createPlayer() {
                player = document.createElement('div');
                player.id = 'player';
                player.style.backgroundImage = "url('https://i.postimg.cc/NfDdpJvP/Senza-titolo-3-20250310144624.png')";
                player.style.left = playerX + 'px';
                player.style.bottom = playerY + 'px';
                gameContainer.appendChild(player);
                gameElements.push(player);
            }
           
            // Handle player jumping
            function handleJump() {
                if (isJumping && !isFalling) {
                    jumpHeight += 5;
                    playerY += 5;
                   
                    if (jumpHeight >= jumpMax) {
                        isJumping = false;
                        isFalling = true;
                    }
                } else if (isFalling) {
                    jumpHeight -= 5;
                    playerY -= 5;
                   
                    if (jumpHeight <= 0) {
                        isFalling = false;
                        playerY = 100; // Reset to ground level
                    }
                }
               
                // Update player position
                player.style.bottom = playerY + 'px';
            }
           
            // Update background position based on player movement
            function updateBackground() {
                // Update screen position counter
                currentScreen = Math.floor(backgroundPos / screenWidth);
                screenPositionDisplay.innerText = currentScreen;
               
                // Update background position
                backgroundLayer.style.transform = `translateX(${-backgroundPos}px)`;
               
                // Update portal position if it exists
                if (portalObject && portalObject.active) {
                    portalObject.element.style.left = (portalObject.x - backgroundPos) + 'px';
                   
                    // Check for portal collision
                    if (Math.abs((portalObject.x - backgroundPos) - playerX) < 50) {
                        portalObject.active = false;
                        nextLevel();
                    }
                }
            }
           
            // Handle player movement
            function handleMovement() {
                // Initialize sprint modifier
                let sprintModifier = keysPressed.ShiftLeft ? 1.5 : 1;
               
                // Fix the movement issue by making background scrolling and player movement consistent
                if (keysPressed.ArrowLeft) {
                    facingRight = false;
                    player.style.transform = 'scaleX(-1)';
                   
                    // Move player left only if not at minimum position
                    if (playerX > playerMinX) {
                        playerX -= moveSpeed * sprintModifier;
                    } else if (backgroundPos > 0) {
                        // Scroll background right instead
                        backgroundPos -= moveSpeed * sprintModifier;
                    }
                }
               
                if (keysPressed.ArrowRight) {
                    facingRight = true;
                    player.style.transform = 'scaleX(1)';
                   
                    // Only move player right until reaching the center of the screen
                    if (playerX < playerMaxX) {
                        playerX += moveSpeed * sprintModifier;
                    } else if (backgroundPos < maxBackgroundPos) {
                        // Scroll background left instead
                        backgroundPos += moveSpeed * sprintModifier;
                    } else if (playerX < viewportWidth - 80) {
                        // If at the end of level, allow player to move further right
                        playerX += moveSpeed * sprintModifier;
                    }
                }
               
                // Update player position
                player.style.left = playerX + 'px';
               
                // Handle jump with Space bar
                if ((keysPressed.Space || keysPressed.ArrowUp) && !isJumping && !isFalling) {
                    isJumping = true;
                }
               
                // Update background based on player movement
                updateBackground();
            }
           
            // Main game loop
            function gameLoop() {
                if (!gameLoopRunning) return;
               
                handleMovement();
                handleJump();
                manageDemons();
               
                requestAnimationFrame(gameLoop);
            }
           
            // Start the game
            function startGame() {
                startScreen.style.display = 'none';
               
                // Initialize game elements
                createPlayer();
                initLives();
                createPortal();
               
                // Set world display
                worldDisplay.innerText = '1-1';
               
                // Start timer
                startTimer();
               
                // Start game loop
                gameLoopRunning = true;
                gameLoop();
            }
           
            // Handle sound toggle
            soundButton.addEventListener('click', function() {
                const isMuted = soundButton.textContent === '🔇';
                soundButton.textContent = isMuted ? '🔊' : '🔇';
                // Add actual sound control here
            });
           
            // Initialize keyboard listeners
            setupKeyboardListeners();
        });
    </script>
</body>
</html>
