[index.html](https://github.com/user-attachments/files/28684752/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CoinStack - Currency Block Game</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Space+Grotesk:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- Background Canvas for ambient neon particles -->
    <canvas id="bg-canvas"></canvas>
    <div class="app-container">
        <!-- Live Currency Ticker Header -->
        <header class="ticker-header">
            <div class="ticker-title">LIVE MARKET</div>
            <div class="ticker-wrap">
                <div class="ticker" id="market-ticker">
                    <div class="ticker__item"><span class="symbol usd">😊</span> USD/G: <span id="rate-usd">1.00</span> <span class="trend up">▲</span></div>
                    <div class="ticker__item"><span class="symbol eur">😂</span> EUR/G: <span id="rate-eur">1.25</span> <span class="trend up">▲</span></div>
                    <div class="ticker__item"><span class="symbol gbp">😘</span> GBP/G: <span id="rate-gbp">1.50</span> <span class="trend down">▼</span></div>
                    <div class="ticker__item"><span class="symbol btc">😍</span> BTC/G: <span id="rate-btc">5.00</span> <span class="trend up">▲</span></div>
                    <div class="ticker__item"><span class="symbol usd">$</span> USD/G: <span id="rate-usd2">1.00</span> <span class="trend up">▲</span></div>
                    <div class="ticker__item"><span class="symbol eur">€</span> EUR/G: <span id="rate-eur2">1.25</span> <span class="trend up">▲</span></div>
                    <div class="ticker__item"><span class="symbol gbp">£</span> GBP/G: <span id="rate-gbp2">1.50</span> <span class="trend down">▼</span></div>
                    <div class="ticker__item"><span class="symbol btc">₿</span> BTC/G: <span id="rate-btc2">5.00</span> <span class="trend up">▲</span></div>
                </div>
            </div>
        </header>
        <main class="game-layout">
            <!-- Left Panel: Stats and Controls -->
            <div class="panel side-panel left-panel">
                <!-- Hold Panel -->
                <div class="glass-card hold-card">
                    <div class="card-header">HOLD</div>
                    <div class="hold-preview-container">
                        <canvas id="hold-canvas" width="120" height="120"></canvas>
                    </div>
                </div>
                <!-- Stats Panel -->
                <div class="glass-card stats-card">
                    <div class="card-header">STATISTICS</div>
                    <div class="stat-group">
                        <div class="stat-label">SCORE</div>
                        <div class="stat-value" id="score-val">000,000</div>
                    </div>
                    <div class="stat-group">
                        <div class="stat-label">LINES</div>
                        <div class="stat-value" id="lines-val">0</div>
                    </div>
                    <div class="stat-group">
                        <div class="stat-label">LEVEL</div>
                        <div class="stat-value" id="level-val">1</div>
                    </div>
                </div>
                <!-- Instructions Panel -->
                <div class="glass-card instructions-card">
                    <div class="card-header">CONTROLS</div>
                    <div class="control-list">
                        <div class="control-row"><span class="key">←</span> <span class="key">→</span> <span>Move Block</span></div>
                        <div class="control-row"><span class="key">↑</span> <span>Rotate</span></div>
                        <div class="control-row"><span class="key">↓</span> <span>Soft Drop</span></div>
                        <div class="control-row"><span class="key">Space</span> <span>Hard Drop</span></div>
                        <div class="control-row"><span class="key">C</span> <span>Hold Piece</span></div>
                        <div class="control-row"><span class="key">1</span> <span class="key">2</span> <span class="key">3</span> <span>Power-ups</span></div>
                    </div>
                </div>
            </div>
            <!-- Center Panel: The Game Screen -->
            <div class="center-panel">
                <div class="game-container-wrapper">
                    <!-- Overlay screens (Start, Pause, Game Over) -->
                    <div class="grid-overlay" id="overlay-screen">
                        <div class="overlay-content" id="start-content">
                            <h1 class="glow-title">COINSTACK</h1>
                            <p class="tagline">Stack Currencies. Earn Gold. Beat the Market.</p>
                            <button class="btn btn-primary" id="start-btn">START MINTING</button>
                        </div>
                        <div class="overlay-content hidden" id="pause-content">
                            <h2 class="glow-title">PAUSED</h2>
                            <p class="tagline">Market trading is temporarily suspended.</p>
                            <button class="btn btn-primary" id="resume-btn">RESUME TRADING</button>
                        </div>
                        <div class="overlay-content hidden" id="gameover-content">
                            <h2 class="glow-title-loss">BANKRUPT</h2>
                            <p class="tagline">Your blocks hit the ceiling. High inflation!</p>
                            <div class="final-stats">
                                <div>Final Score: <span id="final-score">0</span></div>
                                <div>Gold Minted: <span id="final-gold">0</span></div>
                            </div>
                            <button class="btn btn-secondary" id="restart-btn">START NEW FIRM</button>
                        </div>
                    </div>
                    <!-- Main Canvas for rendering Tetris grid -->
                    <canvas id="game-canvas" width="320" height="640"></canvas>
                    
                    <!-- Separate overlays for floating gold coins (to allow canvas rendering isolation) -->
                    <div class="coin-animation-container" id="coin-anim-layer"></div>
                </div>
            </div>
            <!-- Right Panel: Shop, Treasury and Next Piece -->
            <div class="panel side-panel right-panel">
                <!-- Next Panel -->
                <div class="glass-card next-card">
                    <div class="card-header">NEXT BLOCK</div>
                    <div class="next-preview-container">
                        <canvas id="next-canvas" width="120" height="120"></canvas>
                    </div>
                </div>
                <!-- Treasury (Gold Coins) -->
                <div class="glass-card treasury-card">
                    <div class="card-header">TREASURY</div>
                    <div class="gold-counter">
                        <div class="gold-coin-icon">
                            <svg viewBox="0 0 24 24" width="36" height="36" class="coin-svg-glow">
                                <circle cx="12" cy="12" r="10" fill="url(#gold-grad)" stroke="#ffd700" stroke-width="1"/>
                                <text x="12" y="16" font-family="'Space Grotesk', sans-serif" font-size="12" font-weight="bold" fill="#7a5c00" text-anchor="middle">G</text>
                                <defs>
                                    <linearGradient id="gold-grad" x1="0%" y1="0%" x2="100%" y2="100%">
                                        <stop offset="0%" stop-color="#fff2a3" />
                                        <stop offset="50%" stop-color="#ffd700" />
                                        <stop offset="100%" stop-color="#b8860b" />
                                    </linearGradient>
                                </defs>
                            </svg>
                        </div>
                        <div class="gold-val-display" id="gold-val">0</div>
                    </div>
                </div>
                <!-- Power-up Shop -->
                <div class="glass-card shop-card">
                    <div class="card-header">POWER-UP SHOP</div>
                    <div class="shop-items">
                        <!-- Power-up 1: Bomb -->
                        <button class="shop-item btn-shop" id="shop-bomb" data-cost="50" disabled>
                            <div class="shop-item-icon">💣</div>
                            <div class="shop-item-details">
                                <div class="shop-item-name">BOMB BLOCK</div>
                                <div class="shop-item-desc">Blows up 3x3 surrounding grid [Key: 1]</div>
                            </div>
                            <div class="shop-item-cost">50G</div>
                        </button>
                        
                        <!-- Power-up 2: Clear Line -->
                        <button class="shop-item btn-shop" id="shop-clear" data-cost="80" disabled>
                            <div class="shop-item-icon">⚡</div>
                            <div class="shop-item-details">
                                <div class="shop-item-name">LASER WIPE</div>
                                <div class="shop-item-desc">Clears bottom 2 grid rows [Key: 2]</div>
                            </div>
                            <div class="shop-item-cost">80G</div>
                        </button>
                        <!-- Power-up 3: Freeze Time -->
                        <button class="shop-item btn-shop" id="shop-freeze" data-cost="100" disabled>
                            <div class="shop-item-icon">❄️</div>
                            <div class="shop-item-details">
                                <div class="shop-item-name">TIME FREEZE</div>
                                <div class="shop-item-desc">Halves drop speed for 15s [Key: 3]</div>
                            </div>
                            <div class="shop-item-cost">100G</div>
                        </button>
                    </div>
                </div>
            </div>
        </main>
        <!-- Mobile Touch Controls overlay -->
        <div class="mobile-controls hidden" id="mobile-control-pad">
            <div class="mobile-row main-buttons">
                <button class="mobile-btn" id="m-hold">HOLD</button>
                <div class="mobile-dpad">
                    <button class="mobile-btn dpad-btn" id="m-left">◀</button>
                    <div class="dpad-vertical">
                        <button class="mobile-btn dpad-btn" id="m-rotate">▲</button>
                        <button class="mobile-btn dpad-btn" id="m-down">▼</button>
                    </div>
                    <button class="mobile-btn dpad-btn" id="m-right">▶</button>
                </div>
                <button class="mobile-btn" id="m-drop">DROP</button>
            </div>
            </div>
                    <!-- <button class="btn btn-secondary" id="restart-game-btn" style="width: 100%; margin-top: 5px; font-size: 0.8rem; padding: 10px;">RESTART GAME</button>
                </div> -->
            <div class="mobile-row power-buttons">
                <button class="mobile-btn power-btn" id="m-bomb" disabled>💣 Bomb</button>
                <button class="mobile-btn power-btn" id="m-clear" disabled>⚡ Laser</button>
                <button class="mobile-btn power-btn" id="m-freeze" disabled>❄️ Freeze</button>
            </div>
        </div>
    </div>
    <script src="game.js"></script>
</body>
</html>
[style.css](https://github.com/user-attachments/files/28684763/style.css)

:root {
    --bg-dark: #070913;
    --card-bg: rgba(15, 19, 42, 0.45);
    --card-border: rgba(255, 255, 255, 0.08);
    --card-glow: rgba(0, 176, 255, 0.03);
    
    --text-primary: #f0f3fa;
    --text-secondary: #8a96b7;
    
    /* Currency Colors */
    --color-usd: #00e676; /* Emerald Green */
    --color-eur: #00b0ff; /* Bright Blue */
    --color-gbp: #a020f0; /* Intense Purple */
    --color-btc: #ff9100; /* Neon Orange-Gold */
    --color-gold: #ffd700; /* Pure Gold */
    --color-loss: #ff1744; /* Bankrupt Red */
    
    --font-heading: 'Space Grotesk', sans-serif;
    --font-body: 'Outfit', sans-serif;
}
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    user-select: none;
    -webkit-user-select: none;
}
body {
    background-color: var(--bg-dark);
    color: var(--text-primary);
    font-family: var(--font-body);
    overflow: hidden;
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
}
/* Background Canvas for Ambient Particles */
#bg-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
    opacity: 0.6;
}
.app-container {
    position: relative;
    z-index: 2;
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 1200px;
    height: 96vh;
    padding: 10px;
    gap: 15px;
}
/* Live Market Ticker */
.ticker-header {
    background: rgba(10, 14, 33, 0.7);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(10px);
    border-radius: 12px;
    display: flex;
    align-items: center;
    overflow: hidden;
    height: 48px;
    padding: 0 15px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}
.ticker-title {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 2px;
    color: var(--color-btc);
    background: rgba(255, 145, 0, 0.1);
    padding: 6px 12px;
    border-radius: 6px;
    margin-right: 15px;
    white-space: nowrap;
    border: 1px solid rgba(255, 145, 0, 0.2);
    box-shadow: 0 0 10px rgba(255, 145, 0, 0.15);
}
.ticker-wrap {
    width: 100%;
    overflow: hidden;
}
.ticker {
    display: flex;
    white-space: nowrap;
    animation: ticker-slide 30s linear infinite;
}
.ticker__item {
    display: inline-block;
    padding: 0 2rem;
    font-family: var(--font-heading);
    font-size: 0.95rem;
    color: var(--text-primary);
}
.ticker__item .symbol {
    font-weight: bold;
    margin-right: 4px;
}
.ticker__item .symbol.usd { color: var(--color-usd); }
.ticker__item .symbol.eur { color: var(--color-eur); }
.ticker__item .symbol.gbp { color: var(--color-gbp); }
.ticker__item .symbol.btc { color: var(--color-btc); }
.trend {
    font-size: 0.75rem;
    margin-left: 4px;
}
.trend.up { color: #00e676; }
.trend.down { color: #ff1744; }
@keyframes ticker-slide {
    0% { transform: translate3d(0, 0, 0); }
    100% { transform: translate3d(-50%, 0, 0); }
}
/* Main Layout Grid */
.game-layout {
    display: grid;
    grid-template-columns: 280px 1fr 280px;
    gap: 15px;
    flex-grow: 1;
    height: calc(100% - 65px);
    overflow: hidden;
}
/* Glassmorphic Side Panels */
.panel {
    display: flex;
    flex-direction: column;
    gap: 15px;
    height: 100%;
    overflow-y: auto;
}
/* Hide scrollbar for sidebars */
.panel::-webkit-scrollbar {
    width: 0px;
}
.glass-card {
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border-radius: 16px;
    padding: 16px;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}
.glass-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at top left, var(--card-glow), transparent 60%);
    pointer-events: none;
}
.card-header {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.85rem;
    letter-spacing: 1.5px;
    color: var(--text-secondary);
    margin-bottom: 12px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    padding-bottom: 8px;
}
/* Left Panel Specifics */
.hold-card, .next-card {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.hold-preview-container, .next-preview-container {
    background: rgba(0, 0, 0, 0.35);
    border: 1px dashed rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    width: 120px;
    height: 120px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.stats-card {
    display: flex;
    flex-direction: column;
    gap: 12px;
}
.stat-group {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    padding: 10px 14px;
    border: 1px solid rgba(255, 255, 255, 0.02);
}
.stat-label {
    font-family: var(--font-heading);
    font-size: 0.75rem;
    letter-spacing: 1px;
    color: var(--text-secondary);
    margin-bottom: 4px;
}
.stat-value {
    font-family: var(--font-heading);
    font-size: 1.6rem;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: 1px;
}
.instructions-card .control-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
    font-size: 0.85rem;
    color: var(--text-secondary);
}
.control-row {
    display: flex;
    align-items: center;
    gap: 8px;
}
.key {
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.15);
    border-radius: 4px;
    padding: 2px 6px;
    font-size: 0.75rem;
    font-family: var(--font-heading);
    color: var(--text-primary);
    min-width: 24px;
    text-align: center;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
/* Center Game Screen */
.center-panel {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
.game-container-wrapper {
    position: relative;
    aspect-ratio: 1 / 2;
    height: 100%;
    max-height: 700px;
    background: rgba(10, 14, 30, 0.65);
    border: 2px solid rgba(0, 176, 255, 0.15);
    box-shadow: 0 0 40px rgba(0, 176, 255, 0.1);
    border-radius: 20px;
    overflow: hidden;
}
#game-canvas {
    width: 100%;
    height: 100%;
    display: block;
}
/* Interactive Grid Overlay (Screens) */
.grid-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(6, 8, 20, 0.85);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 24px;
    text-align: center;
    transition: opacity 0.3s ease, visibility 0.3s ease;
}
.grid-overlay.hidden {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
}
.overlay-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    max-width: 280px;
}
.overlay-content.hidden {
    display: none;
}
.glow-title {
    font-family: var(--font-heading);
    font-size: 2.2rem;
    font-weight: 800;
    letter-spacing: 3px;
    background: linear-gradient(135deg, var(--color-usd), var(--color-eur));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 15px rgba(0, 230, 118, 0.4));
    animation: glow-pulse 2s infinite ease-in-out;
}
.glow-title-loss {
    font-family: var(--font-heading);
    font-size: 2.2rem;
    font-weight: 800;
    letter-spacing: 3px;
    background: linear-gradient(135deg, var(--color-loss), #ff5252);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 15px rgba(255, 23, 68, 0.4));
}
.tagline {
    font-size: 0.85rem;
    color: var(--text-secondary);
    line-height: 1.4;
}
.btn {
    font-family: var(--font-heading);
    font-weight: 700;
    padding: 12px 24px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.2s ease;
    font-size: 0.9rem;
    letter-spacing: 1px;
}
.btn-primary {
    background: linear-gradient(135deg, #00b0ff, #00e676);
    color: #030a16;
    box-shadow: 0 4px 15px rgba(0, 230, 118, 0.3);
}
.btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 230, 118, 0.5);
}
.btn-secondary {
    background: rgba(255, 255, 255, 0.1);
    color: var(--text-primary);
    border: 1px solid rgba(255, 255, 255, 0.15);
}
.btn-secondary:hover {
    background: rgba(255, 255, 255, 0.18);
    transform: translateY(-2px);
}
.final-stats {
    background: rgba(0,0,0,0.3);
    border-radius: 10px;
    padding: 12px;
    width: 100%;
    font-family: var(--font-heading);
    font-size: 0.95rem;
    display: flex;
    flex-direction: column;
    gap: 8px;
    border: 1px solid rgba(255,255,255,0.05);
}
.final-stats span {
    font-weight: bold;
    color: var(--color-gold);
}
/* Treasury (Gold Coins counter) */
.treasury-card {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.gold-counter {
    display: flex;
    align-items: center;
    gap: 15px;
    background: rgba(0, 0, 0, 0.25);
    border-radius: 12px;
    padding: 10px 20px;
    width: 100%;
    justify-content: center;
    border: 1px solid rgba(255, 215, 0, 0.1);
}
.gold-coin-icon {
    display: flex;
    justify-content: center;
    align-items: center;
    animation: gold-bounce 2s infinite ease-in-out;
}
.coin-svg-glow {
    filter: drop-shadow(0 0 8px rgba(255, 215, 0, 0.6));
}
.gold-val-display {
    font-family: var(--font-heading);
    font-size: 2rem;
    font-weight: 800;
    color: var(--color-gold);
    text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
}
/* Power-up Shop UI */
.shop-card {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
}
.shop-items {
    display: flex;
    flex-direction: column;
    gap: 10px;
    flex-grow: 1;
}
.btn-shop {
    display: flex;
    align-items: center;
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid var(--card-border);
    border-radius: 12px;
    padding: 10px;
    color: var(--text-primary);
    cursor: pointer;
    text-align: left;
    transition: all 0.2s ease;
    width: 100%;
    gap: 12px;
}
.btn-shop:hover:not(:disabled) {
    background: rgba(255, 255, 255, 0.07);
    border-color: rgba(255, 215, 0, 0.25);
    transform: translateX(4px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}
.btn-shop:disabled {
    opacity: 0.4;
    cursor: not-allowed;
}
.shop-item-icon {
    font-size: 1.5rem;
    background: rgba(255, 255, 255, 0.05);
    width: 38px;
    height: 38px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 8px;
    border: 1px solid rgba(255, 255, 255, 0.05);
}
.shop-item-details {
    flex-grow: 1;
}
.shop-item-name {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.5px;
    margin-bottom: 2px;
}
.shop-item-desc {
    font-size: 0.65rem;
    color: var(--text-secondary);
}
.shop-item-cost {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.9rem;
    color: var(--color-gold);
    background: rgba(255, 215, 0, 0.1);
    padding: 4px 8px;
    border-radius: 6px;
    border: 1px solid rgba(255, 215, 0, 0.15);
}
/* Floating Gold Coin Animation Layer */
.coin-animation-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 15;
    overflow: hidden;
}
/* Individual Physical Coin */
.flying-coin {
    position: fixed;
    width: 24px;
    height: 24px;
    background: radial-gradient(circle at 35% 35%, #fff2a3, #ffd700 60%, #b8860b);
    border-radius: 50%;
    border: 1.5px solid #fff;
    box-shadow: 0 0 8px rgba(255, 215, 0, 0.8), inset 0 0 4px rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: var(--font-heading);
    font-size: 11px;
    font-weight: 800;
    color: #7a5c00;
    z-index: 20;
    transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1), opacity 0.8s ease;
    animation: coin-spin 0.4s linear infinite;
}
/* Mobile Controls Layout */
.mobile-controls {
    display: none; /* Controlled via JS or Media Queries */
    flex-direction: column;
    gap: 8px;
    padding: 10px;
    background: rgba(10, 14, 30, 0.8);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    z-index: 20;
}
.mobile-row {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    gap: 8px;
}
.mobile-btn {
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.15);
    color: var(--text-primary);
    padding: 12px;
    border-radius: 10px;
    font-family: var(--font-heading);
    font-weight: bold;
    cursor: pointer;
    font-size: 0.9rem;
    flex-grow: 1;
    min-height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.mobile-btn:active {
    background: rgba(255, 255, 255, 0.18);
}
.mobile-dpad {
    display: flex;
    align-items: center;
    gap: 6px;
}
.dpad-vertical {
    display: flex;
    flex-direction: column;
    gap: 6px;
}
.dpad-btn {
    width: 44px;
    height: 44px;
    padding: 0;
}
.power-btn {
    font-size: 0.75rem;
    padding: 8px;
}
.power-btn:disabled {
    opacity: 0.3;
}
/* Animation Keyframes */
@keyframes glow-pulse {
    0%, 100% { filter: drop-shadow(0 0 10px rgba(0, 230, 118, 0.3)); }
    50% { filter: drop-shadow(0 0 20px rgba(0, 230, 118, 0.6)); }
}
@keyframes gold-bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-4px); }
}
@keyframes coin-spin {
    0% { transform: rotateY(0deg); }
    100% { transform: rotateY(360deg); }
}
/* Media Queries for Responsiveness */
@media (max-width: 1024px) {
    .game-layout {
        grid-template-columns: 220px 1fr 220px;
    }
}
@media (max-width: 768px) {
    body {
        overflow-y: auto;
        height: auto;
    }
    .app-container {
        height: auto;
        min-height: 100vh;
        overflow: visible;
        padding: 5px;
    }
    .game-layout {
        grid-template-columns: 1fr;
        height: auto;
        overflow: visible;
    }
    
    .panel {
        height: auto;
        overflow: visible;
    }
    
    .left-panel {
        order: 2;
        display: grid;
        grid-template-columns: 1fr 1fr;
    }
    
    .instructions-card {
        grid-column: span 2;
    }
    .right-panel {
        order: 3;
    }
    
    .center-panel {
        order: 1;
        width: 100%;
        max-width: 380px;
        margin: 0 auto;
    }
    .mobile-controls {
        display: flex;
        order: 4;
        width: 100%;
        max-width: 380px;
        margin: 0 auto;
    }
}

:root {
    --bg-dark: #070913;
    --card-bg: rgba(15, 19, 42, 0.45);
    --card-border: rgba(255, 255, 255, 0.08);
    --card-glow: rgba(0, 176, 255, 0.03);
    
    --text-primary: #f0f3fa;
    --text-secondary: #8a96b7;
    
    /* Currency Colors */
    --color-usd: #00e676; /* Emerald Green */
    --color-eur: #00b0ff; /* Bright Blue */
    --color-gbp: #a020f0; /* Intense Purple */
    --color-btc: #ff9100; /* Neon Orange-Gold */
    --color-gold: #ffd700; /* Pure Gold */
    --color-loss: #ff1744; /* Bankrupt Red */
    
    --font-heading: 'Space Grotesk', sans-serif;
    --font-body: 'Outfit', sans-serif;
}
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    user-select: none;
    -webkit-user-select: none;
}
body {
    background-color: var(--bg-dark);
    color: var(--text-primary);
    font-family: var(--font-body);
    overflow: hidden;
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
}
/* Background Canvas for Ambient Particles */
#bg-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: none;
    opacity: 0.6;
}
.app-container {
    position: relative;
    z-index: 2;
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 1200px;
    height: 96vh;
    padding: 10px;
    gap: 15px;
}
/* Live Market Ticker */
.ticker-header {
    background: rgba(10, 14, 33, 0.7);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(10px);
    border-radius: 12px;
    display: flex;
    align-items: center;
    overflow: hidden;
    height: 48px;
    padding: 0 15px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}
.ticker-title {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 2px;
    color: var(--color-btc);
    background: rgba(255, 145, 0, 0.1);
    padding: 6px 12px;
    border-radius: 6px;
    margin-right: 15px;
    white-space: nowrap;
    border: 1px solid rgba(255, 145, 0, 0.2);
    box-shadow: 0 0 10px rgba(255, 145, 0, 0.15);
}
.ticker-wrap {
    width: 100%;
    overflow: hidden;
}
.ticker {
    display: flex;
    white-space: nowrap;
    animation: ticker-slide 30s linear infinite;
}
.ticker__item {
    display: inline-block;
    padding: 0 2rem;
    font-family: var(--font-heading);
    font-size: 0.95rem;
    color: var(--text-primary);
}
.ticker__item .symbol {
    font-weight: bold;
    margin-right: 4px;
}
.ticker__item .symbol.usd { color: var(--color-usd); }
.ticker__item .symbol.eur { color: var(--color-eur); }
.ticker__item .symbol.gbp { color: var(--color-gbp); }
.ticker__item .symbol.btc { color: var(--color-btc); }
.trend {
    font-size: 0.75rem;
    margin-left: 4px;
}
.trend.up { color: #00e676; }
.trend.down { color: #ff1744; }
@keyframes ticker-slide {
    0% { transform: translate3d(0, 0, 0); }
    100% { transform: translate3d(-50%, 0, 0); }
}
/* Main Layout Grid */
.game-layout {
    display: grid;
    grid-template-columns: 280px 1fr 280px;
    gap: 15px;
    flex-grow: 1;
    height: calc(100% - 65px);
    overflow: hidden;
}
/* Glassmorphic Side Panels */
.panel {
    display: flex;
    flex-direction: column;
    gap: 15px;
    height: 100%;
    overflow-y: auto;
}
/* Custom scrollbar for sidebars */
.panel::-webkit-scrollbar {
    width: 6px;
}
.panel::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.01);
    border-radius: 3px;
}
.panel::-webkit-scrollbar-thumb {
    background: rgba(0, 176, 255, 0.25);
    border-radius: 3px;
}
.panel::-webkit-scrollbar-thumb:hover {
    background: rgba(0, 176, 255, 0.5);
}
.glass-card {
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border-radius: 16px;
    padding: 16px;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    position: relative;
    overflow: hidden;
}
.glass-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at top left, var(--card-glow), transparent 60%);
    pointer-events: none;
}
.card-header {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.85rem;
    letter-spacing: 1.5px;
    color: var(--text-secondary);
    margin-bottom: 12px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    padding-bottom: 8px;
}
/* Left Panel Specifics */
.hold-card, .next-card {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.hold-preview-container, .next-preview-container {
    background: rgba(0, 0, 0, 0.35);
    border: 1px dashed rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    width: 120px;
    height: 120px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.stats-card {
    display: flex;
    flex-direction: column;
    gap: 12px;
}
.stat-group {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 10px;
    padding: 10px 14px;
    border: 1px solid rgba(255, 255, 255, 0.02);
}
.stat-label {
    font-family: var(--font-heading);
    font-size: 0.75rem;
    letter-spacing: 1px;
    color: var(--text-secondary);
    margin-bottom: 4px;
}
.stat-value {
    font-family: var(--font-heading);
    font-size: 1.6rem;
    font-weight: 700;
    color: var(--text-primary);
    letter-spacing: 1px;
}
.instructions-card .control-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
    font-size: 0.85rem;
    color: var(--text-secondary);
}
.control-row {
    display: flex;
    align-items: center;
    gap: 8px;
}
.key {
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.15);
    border-radius: 4px;
    padding: 2px 6px;
    font-size: 0.75rem;
    font-family: var(--font-heading);
    color: var(--text-primary);
    min-width: 24px;
    text-align: center;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
}
/* Center Game Screen */
.center-panel {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
.game-container-wrapper {
    position: relative;
    aspect-ratio: 1 / 2;
    height: 100%;
    max-height: 700px;
    background: rgba(10, 14, 30, 0.65);
    border: 2px solid rgba(0, 176, 255, 0.15);
    box-shadow: 0 0 40px rgba(0, 176, 255, 0.1);
    border-radius: 20px;
    overflow: hidden;
}
#game-canvas {
    width: 100%;
    height: 100%;
    display: block;
}
/* Interactive Grid Overlay (Screens) */
.grid-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(6, 8, 20, 0.85);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 24px;
    text-align: center;
    transition: opacity 0.3s ease, visibility 0.3s ease;
}
.grid-overlay.hidden {
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
}
.overlay-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    max-width: 280px;
}
.overlay-content.hidden {
    display: none;
}
.glow-title {
    font-family: var(--font-heading);
    font-size: 2.2rem;
    font-weight: 800;
    letter-spacing: 3px;
    background: linear-gradient(135deg, var(--color-usd), var(--color-eur));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 15px rgba(0, 230, 118, 0.4));
    animation: glow-pulse 2s infinite ease-in-out;
}
.glow-title-loss {
    font-family: var(--font-heading);
    font-size: 2.2rem;
    font-weight: 800;
    letter-spacing: 3px;
    background: linear-gradient(135deg, var(--color-loss), #ff5252);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 15px rgba(255, 23, 68, 0.4));
}
.tagline {
    font-size: 0.85rem;
    color: var(--text-secondary);
    line-height: 1.4;
}
.btn {
    font-family: var(--font-heading);
    font-weight: 700;
    padding: 12px 24px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.2s ease;
    font-size: 0.9rem;
    letter-spacing: 1px;
}
.btn-primary {
    background: linear-gradient(135deg, #00b0ff, #00e676);
    color: #030a16;
    box-shadow: 0 4px 15px rgba(0, 230, 118, 0.3);
}
.btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 230, 118, 0.5);
}
.btn-secondary {
    background: rgba(255, 255, 255, 0.1);
    color: var(--text-primary);
    border: 1px solid rgba(255, 255, 255, 0.15);
}
.btn-secondary:hover {
    background: rgba(255, 255, 255, 0.18);
    transform: translateY(-2px);
}
.final-stats {
    background: rgba(0,0,0,0.3);
    border-radius: 10px;
    padding: 12px;
    width: 100%;
    font-family: var(--font-heading);
    font-size: 0.95rem;
    display: flex;
    flex-direction: column;
    gap: 8px;
    border: 1px solid rgba(255,255,255,0.05);
}
.final-stats span {
    font-weight: bold;
    color: var(--color-gold);
}
/* Treasury (Gold Coins counter) */
.treasury-card {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.gold-counter {
    display: flex;
    align-items: center;
    gap: 15px;
    background: rgba(0, 0, 0, 0.25);
    border-radius: 12px;
    padding: 10px 20px;
    width: 100%;
    justify-content: center;
    border: 1px solid rgba(255, 215, 0, 0.1);
}
.gold-coin-icon {
    display: flex;
    justify-content: center;
    align-items: center;
    animation: gold-bounce 2s infinite ease-in-out;
}
.coin-svg-glow {
    filter: drop-shadow(0 0 8px rgba(255, 215, 0, 0.6));
}
.gold-val-display {
    font-family: var(--font-heading);
    font-size: 2rem;
    font-weight: 800;
    color: var(--color-gold);
    text-shadow: 0 0 10px rgba(255, 215, 0, 0.3);
}
/* Power-up Shop UI */
.shop-card {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
}
.shop-items {
    display: flex;
    flex-direction: column;
    gap: 10px;
    flex-grow: 1;
}
.btn-shop {
    display: flex;
    align-items: center;
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid var(--card-border);
    border-radius: 12px;
    padding: 10px;
    color: var(--text-primary);
    cursor: pointer;
    text-align: left;
    transition: all 0.2s ease;
    width: 100%;
    gap: 12px;
}
.btn-shop:hover:not(:disabled) {
    background: rgba(255, 255, 255, 0.07);
    border-color: rgba(255, 215, 0, 0.25);
    transform: translateX(4px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}
.btn-shop:disabled {
    opacity: 0.4;
    cursor: not-allowed;
}
.shop-item-icon {
    font-size: 1.5rem;
    background: rgba(255, 255, 255, 0.05);
    width: 38px;
    height: 38px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 8px;
    border: 1px solid rgba(255, 255, 255, 0.05);
}
.shop-item-details {
    flex-grow: 1;
}
.shop-item-name {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.8rem;
    letter-spacing: 0.5px;
    margin-bottom: 2px;
}
.shop-item-desc {
    font-size: 0.65rem;
    color: var(--text-secondary);
}
.shop-item-cost {
    font-family: var(--font-heading);
    font-weight: 700;
    font-size: 0.9rem;
    color: var(--color-gold);
    background: rgba(255, 215, 0, 0.1);
    padding: 4px 8px;
    border-radius: 6px;
    border: 1px solid rgba(255, 215, 0, 0.15);
}
/* Floating Gold Coin Animation Layer */
.coin-animation-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 15;
    overflow: hidden;
}
/* Individual Physical Coin */
.flying-coin {
    position: fixed;
    width: 24px;
    height: 24px;
    background: radial-gradient(circle at 35% 35%, #fff2a3, #ffd700 60%, #b8860b);
    border-radius: 50%;
    border: 1.5px solid #fff;
    box-shadow: 0 0 8px rgba(255, 215, 0, 0.8), inset 0 0 4px rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: var(--font-heading);
    font-size: 11px;
    font-weight: 800;
    color: #7a5c00;
    z-index: 20;
    transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1), opacity 0.8s ease;
    animation: coin-spin 0.4s linear infinite;
}
/* Mobile Controls Layout */
.mobile-controls {
    display: none; /* Controlled via JS or Media Queries */
    flex-direction: column;
    gap: 8px;
    padding: 10px;
    background: rgba(10, 14, 30, 0.8);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    z-index: 20;
}
.mobile-row {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    gap: 8px;
}
.mobile-btn {
    background: rgba(255, 255, 255, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.15);
    color: var(--text-primary);
    padding: 12px;
    border-radius: 10px;
    font-family: var(--font-heading);
    font-weight: bold;
    cursor: pointer;
    font-size: 0.9rem;
    flex-grow: 1;
    min-height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.mobile-btn:active {
    background: rgba(255, 255, 255, 0.18);
}
.mobile-dpad {
    display: flex;
    align-items: center;
    gap: 6px;
}
.dpad-vertical {
    display: flex;
    flex-direction: column;
    gap: 6px;
}
.dpad-btn {
    width: 44px;
    height: 44px;
    padding: 0;
}
.power-btn {
    font-size: 0.75rem;
    padding: 8px;
}
.power-btn:disabled {
    opacity: 0.3;
}
/* Animation Keyframes */
@keyframes glow-pulse {
    0%, 100% { filter: drop-shadow(0 0 10px rgba(0, 230, 118, 0.3)); }
    50% { filter: drop-shadow(0 0 20px rgba(0, 230, 118, 0.6)); }
}
@keyframes gold-bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-4px); }
}
@keyframes coin-spin {
    0% { transform: rotateY(0deg); }
    100% { transform: rotateY(360deg); }
}
/* Media Queries for Responsiveness */
@media (max-width: 1024px) {
    .game-layout {
        grid-template-columns: 220px 1fr 220px;
    }
}
@media (max-width: 768px) {
    body {
        overflow-y: auto;
        height: auto;
    }
    .app-container {
        height: auto;
        min-height: 100vh;
        overflow: visible;
        padding: 5px;
    }
    .game-layout {
        grid-template-columns: 1fr;
        height: auto;
        overflow: visible;
    }
    
    .panel {
        height: auto;
        overflow: visible;
    }
    
    .left-panel {
        order: 2;
        display: grid;
        grid-template-columns: 1fr 1fr;
    }
    
    .instructions-card {
        grid-column: span 2;
    }
    .right-panel {
        order: 3;
    }
    
    .center-panel {
        order: 1;
        width: 100%;
        max-width: 380px;
        margin: 0 auto;
    }
    .mobile-controls {
        display: flex;
        order: 4;
        width: 100%;
        max-width: 380px;
        margin: 0 auto;
    }
}
/* Enable scrolling on screens with low height */
@media (max-height: 720px) {
    body {
        overflow-y: auto;
        height: auto;
        padding: 20px 0;
    }
    .app-container {
        height: auto;
        min-height: 96vh;
    }
[game.js](https://github.com/user-attachments/files/28684767/game.js)
// Sound Synthesizer using Web Audio API
class AudioSynth {
    constructor() {
        this.ctx = null;
        this.muted = false;
    }
    init() {
        if (!this.ctx) {
            this.ctx = new (window.AudioContext || window.webkitAudioContext)();
        }
        if (this.ctx.state === 'suspended') {
            this.ctx.resume();
        }
    }
    playMove() {
        this.init();
        if (this.muted) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'triangle';
        osc.frequency.setValueAtTime(180, this.ctx.currentTime);
        gain.gain.setValueAtTime(0.08, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.05);
        
        osc.start();
        osc.stop(this.ctx.currentTime + 0.05);
    }
    playRotate() {
        this.init();
        if (this.muted) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sine';
        osc.frequency.setValueAtTime(220, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(440, this.ctx.currentTime + 0.08);
        gain.gain.setValueAtTime(0.08, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.08);
        
        osc.start();
        osc.stop(this.ctx.currentTime + 0.08);
    }
    playHold() {
        this.init();
        if (this.muted) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(300, this.ctx.currentTime);
        osc.frequency.linearRampToValueAtTime(150, this.ctx.currentTime + 0.12);
        gain.gain.setValueAtTime(0.05, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.12);
        
        osc.start();
        osc.stop(this.ctx.currentTime + 0.12);
    }
    playLand() {
        this.init();
        if (this.muted) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sine';
        osc.frequency.setValueAtTime(120, this.ctx.currentTime);
        osc.frequency.linearRampToValueAtTime(60, this.ctx.currentTime + 0.1);
        gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.1);
        
        osc.start();
        osc.stop(this.ctx.currentTime + 0.1);
    }
    playCoin() {
        this.init();
        if (this.muted) return;
        
        // Classic retro coin sound: two notes (e.g. B5 then E6)
        const now = this.ctx.currentTime;
        
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sine';
        osc.frequency.setValueAtTime(987.77, now); // B5
        osc.frequency.setValueAtTime(1318.51, now + 0.08); // E6
        
        gain.gain.setValueAtTime(0.08, now);
        gain.gain.setValueAtTime(0.08, now + 0.08);
        gain.gain.exponentialRampToValueAtTime(0.001, now + 0.3);
        
        osc.start(now);
        osc.stop(now + 0.3);
    }
    playClear() {
        this.init();
        if (this.muted) return;
        
        const now = this.ctx.currentTime;
        const notes = [261.63, 329.63, 392.00, 523.25, 659.25, 783.99, 1046.50]; // C major arpeggio
        
        notes.forEach((freq, idx) => {
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(freq, now + idx * 0.05);
            gain.gain.setValueAtTime(0.08, now + idx * 0.05);
            gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.05 + 0.15);
            
            osc.start(now + idx * 0.05);
            osc.stop(now + idx * 0.05 + 0.15);
        });
    }
    playLaser() {
        this.init();
        if (this.muted) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(880, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(110, this.ctx.currentTime + 0.4);
        
        gain.gain.setValueAtTime(0.12, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + 0.4);
        
        osc.start();
        osc.stop(this.ctx.currentTime + 0.4);
    }
    playBomb() {
        this.init();
        if (this.muted) return;
        
        // Low boom + White Noise
        const now = this.ctx.currentTime;
        
        // Low boom
        const osc = this.ctx.createOscillator();
        const gainOsc = this.ctx.createGain();
        osc.connect(gainOsc);
        gainOsc.connect(this.ctx.destination);
        osc.frequency.setValueAtTime(100, now);
        osc.frequency.linearRampToValueAtTime(10, now + 0.5);
        gainOsc.gain.setValueAtTime(0.3, now);
        gainOsc.gain.exponentialRampToValueAtTime(0.001, now + 0.5);
        osc.start(now);
        osc.stop(now + 0.5);
        // White noise
        const bufferSize = this.ctx.sampleRate * 0.5; // 0.5 seconds
        const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
        const data = buffer.getChannelData(0);
        for (let i = 0; i < bufferSize; i++) {
            data[i] = Math.random() * 2 - 1;
        }
        
        const noise = this.ctx.createBufferSource();
        noise.buffer = buffer;
        
        const filter = this.ctx.createBiquadFilter();
        filter.type = 'lowpass';
        filter.frequency.setValueAtTime(400, now);
        filter.frequency.exponentialRampToValueAtTime(10, now + 0.5);
        
        const gainNoise = this.ctx.createGain();
        gainNoise.gain.setValueAtTime(0.25, now);
        gainNoise.gain.exponentialRampToValueAtTime(0.001, now + 0.5);
        
        noise.connect(filter);
        filter.connect(gainNoise);
        gainNoise.connect(this.ctx.destination);
        
        noise.start(now);
        noise.stop(now + 0.5);
    }
    playFreeze() {
        this.init();
        if (this.muted) return;
        const now = this.ctx.currentTime;
        const notes = [523.25, 493.88, 440.00, 392.00]; // descending chilly notes
        
        notes.forEach((freq, idx) => {
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            
            osc.type = 'sine';
            osc.frequency.setValueAtTime(freq, now + idx * 0.1);
            gain.gain.setValueAtTime(0.07, now + idx * 0.1);
            gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.1 + 0.25);
            
            osc.start(now + idx * 0.1);
            osc.stop(now + idx * 0.1 + 0.25);
        });
    }
    playGameOver() {
        this.init();
        if (this.muted) return;
        const now = this.ctx.currentTime;
        
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(220, now);
        osc.frequency.linearRampToValueAtTime(55, now + 0.8);
        
        gain.gain.setValueAtTime(0.15, now);
        gain.gain.exponentialRampToValueAtTime(0.001, now + 0.8);
        
        osc.start(now);
        osc.stop(now + 0.8);
    }
    playLevelUp() {
        this.init();
        if (this.muted) return;
        const now = this.ctx.currentTime;
        const notes = [261.63, 329.63, 392.00, 523.25, 392.00, 523.25, 783.99];
        
        notes.forEach((freq, idx) => {
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            
            osc.type = 'sine';
            osc.frequency.setValueAtTime(freq, now + idx * 0.08);
            gain.gain.setValueAtTime(0.08, now + idx * 0.08);
            gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.08 + 0.2);
            
            osc.start(now + idx * 0.08);
            osc.stop(now + idx * 0.08 + 0.2);
        });
    }
}
// Background Ambient Particles
class ParticleField {
    constructor(canvas) {
        this.canvas = canvas;
        this.ctx = canvas.getContext('2d');
        this.particles = [];
        this.resize();
        window.addEventListener('resize', () => this.resize());
        this.initParticles();
    }
    resize() {
        this.canvas.width = window.innerWidth;
        this.canvas.height = window.innerHeight;
    }
    initParticles() {
        const count = 40;
        const symbols = ['$', '€', '£', '₿'];
        for (let i = 0; i < count; i++) {
            this.particles.push({
                x: Math.random() * this.canvas.width,
                y: Math.random() * this.canvas.height,
                vy: -0.2 - Math.random() * 0.5,
                size: 10 + Math.random() * 14,
                opacity: 0.1 + Math.random() * 0.35,
                text: symbols[Math.floor(Math.random() * symbols.length)],
                color: ['#00e676', '#00b0ff', '#d500f9', '#ff9100'][Math.floor(Math.random() * 4)]
            });
        }
    }
    draw() {
        this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
        this.particles.forEach(p => {
            this.ctx.fillStyle = p.color;
            this.ctx.globalAlpha = p.opacity;
            this.ctx.font = `${p.size}px 'Space Grotesk'`;
            this.ctx.fillText(p.text, p.x, p.y);
            
            // Move
            p.y += p.vy;
            
            // Reset if out of bounds
            if (p.y < -30) {
                p.y = this.canvas.height + 30;
                p.x = Math.random() * this.canvas.width;
            }
        });
        this.ctx.globalAlpha = 1.0;
    }
}
// Game Settings & Constants
const COLS = 10;
const ROWS = 20;
// Currencies mapping
const CURRENCIES = {
    USD: { symbol: '$', name: 'USD', color: '#00e676', glow: 'rgba(0, 230, 118, 0.4)', baseRate: 1.00, rate: 1.00, weight: 0.50 },
    EUR: { symbol: '€', name: 'EUR', color: '#00b0ff', glow: 'rgba(0, 176, 255, 0.4)', baseRate: 1.25, rate: 1.25, weight: 0.25 },
    GBP: { symbol: '£', name: 'GBP', color: '#a020f0', glow: 'rgba(160, 32, 240, 0.4)', baseRate: 1.50, rate: 1.50, weight: 0.15 },
    BTC: { symbol: '₿', name: 'BTC', color: '#ff9100', glow: 'rgba(255, 145, 0, 0.4)', baseRate: 5.00, rate: 5.00, weight: 0.10 }
};
// Tetromino definitions
const TETROMINOES = {
    I: [[0,0,0,0],[1,1,1,1],[0,0,0,0],[0,0,0,0]],
    O: [[1,1],[1,1]],
    T: [[0,1,0],[1,1,1],[0,0,0]],
    S: [[0,1,1],[1,1,0],[0,0,0]],
    Z: [[1,1,0],[0,1,1],[0,0,0]],
    J: [[1,0,0],[1,1,1],[0,0,0]],
    L: [[0,0,1],[1,1,1],[0,0,0]]
};
class Game {
    constructor() {
        this.sound = new AudioSynth();
        
        // Canvas Setup
        this.canvas = document.getElementById('game-canvas');
        this.ctx = this.canvas.getContext('2d');
        this.nextCanvas = document.getElementById('next-canvas');
        this.nextCtx = this.nextCanvas.getContext('2d');
        this.holdCanvas = document.getElementById('hold-canvas');
        this.holdCtx = this.holdCanvas.getContext('2d');
        this.bgCanvas = document.getElementById('bg-canvas');
        
        this.particleField = new ParticleField(this.bgCanvas);
        
        // Block Sizes
        this.blockSize = this.canvas.width / COLS;
        
        // Game Grid state: rows contain cell object { currencyKey, symbol, opacity } or null
        this.grid = Array.from({ length: ROWS }, () => Array(COLS).fill(null));
        
        // Play State
        this.score = 0;
        this.gold = 0;
        this.lines = 0;
        this.level = 1;
        this.gameOver = false;
        this.paused = false;
        
        // Falling Piece State
        this.currentPiece = null;
        this.nextPiece = null;
        this.heldPiece = null;
        this.canHold = true;
        
        // Dynamic Timings
        this.baseFallInterval = 1000; // ms
        this.fallInterval = 1000;
        this.lastFallTime = 0;
        this.isTimeFrozen = false;
        this.freezeTimer = null;
        
        // Active Powerups & Specials
        this.nextIsBomb = false;
        this.clearingLines = false;
        
        // Rate Ticker fluctuation loop
        this.tickerInterval = null;
        
        // Setup UI references
        this.scoreVal = document.getElementById('score-val');
        this.linesVal = document.getElementById('lines-val');
        this.levelVal = document.getElementById('level-val');
        this.goldVal = document.getElementById('gold-val');
        
        this.overlay = document.getElementById('overlay-screen');
        this.startContent = document.getElementById('start-content');
        this.pauseContent = document.getElementById('pause-content');
        this.gameoverContent = document.getElementById('gameover-content');
        
        this.initEventListeners();
        this.updateTickerRates();
        this.startTickerFluctuations();
        this.animateBackground();
    }
    // Main Game Loop
    animate(timestamp) {
        if (this.gameOver || this.paused) return;
        
        // Draw Ticker & Ambient background particles
        this.particleField.draw();
        
        // Check for Fall timing
        if (!this.clearingLines) {
            const timeSinceLastFall = timestamp - this.lastFallTime;
            if (timeSinceLastFall > this.fallInterval) {
                this.moveDown();
                this.lastFallTime = timestamp;
            }
        }
        
        this.draw();
        requestAnimationFrame((t) => this.animate(t));
    }
    animateBackground() {
        const loop = () => {
            if (this.gameOver || this.paused) {
                this.particleField.draw();
                requestAnimationFrame(loop);
            }
        };
        requestAnimationFrame(loop);
    }
    // Initialize listeners for game interaction
    initEventListeners() {
        // Keyboard controls
        document.addEventListener('keydown', (e) => {
            if (this.gameOver) return;
            
            // Prevent browser scrolling for control keys
            const controlKeys = ['Space', 'ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight'];
            if (controlKeys.includes(e.code) && !this.paused) {
                e.preventDefault();
            }
            
            // Start or Resume on Enter/Escape or Space if overlays are visible
            if (e.key === 'Enter') {
                if (this.overlay.style.display !== 'none') {
                    if (this.paused) this.resumeGame();
                    else if (this.gameOver === false && !this.currentPiece) this.startGame();
                }
                return;
            }
            
            if (e.key === 'p' || e.key === 'P' || e.key === 'Escape') {
                this.togglePause();
                return;
            }
            
            if (this.paused || this.clearingLines) return;
            
            switch(e.code) {
                case 'ArrowLeft':
                case 'KeyA':
                    this.moveLeft();
                    break;
                case 'ArrowRight':
                case 'KeyD':
                    this.moveRight();
                    break;
                case 'ArrowUp':
                case 'KeyW':
                    this.rotatePiece();
                    break;
                case 'ArrowDown':
                case 'KeyS':
                    this.moveDown();
                    break;
                case 'Space':
                    this.hardDrop();
                    break;
                case 'KeyC':
                    this.holdPiece();
                    break;
                // Shop Keys
                case 'Digit1':
                case 'Numpad1':
                    this.buyPowerup('bomb');
                    break;
                case 'Digit2':
                case 'Numpad2':
                    this.buyPowerup('clear');
                    break;
                case 'Digit3':
                case 'Numpad3':
                    this.buyPowerup('freeze');
                    break;
            }
        });
        // Overlay Screen Buttons
        document.getElementById('start-btn').addEventListener('click', () => this.startGame());
        document.getElementById('resume-btn').addEventListener('click', () => this.resumeGame());
        document.getElementById('restart-btn').addEventListener('click', () => this.startGame());
        document.getElementById('restart-game-btn').addEventListener('click', () => this.startGame());
        
        // Power-up Shop buttons
        document.getElementById('shop-bomb').addEventListener('click', () => this.buyPowerup('bomb'));
        document.getElementById('shop-clear').addEventListener('click', () => this.buyPowerup('clear'));
        document.getElementById('shop-freeze').addEventListener('click', () => this.buyPowerup('freeze'));
        // Mobile Controls
        document.getElementById('m-hold').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.holdPiece(); });
        document.getElementById('m-left').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.moveLeft(); });
        document.getElementById('m-right').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.moveRight(); });
        document.getElementById('m-rotate').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.rotatePiece(); });
        document.getElementById('m-down').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.moveDown(); });
        document.getElementById('m-drop').addEventListener('click', () => { if (!this.paused && !this.clearingLines) this.hardDrop(); });
        document.getElementById('m-bomb').addEventListener('click', () => this.buyPowerup('bomb'));
        document.getElementById('m-clear').addEventListener('click', () => this.buyPowerup('clear'));
        document.getElementById('m-freeze').addEventListener('click', () => this.buyPowerup('freeze'));
        
        
        
        
        // Setup touch detection to show mobile pad

        if ('ontouchstart' in window || navigator.maxTouchPoints > 0) {
            document.getElementById('mobile-control-pad').classList.remove('hidden');
        }
    }
    // Start New Game
    startGame() {
        this.sound.init();
        this.sound.playLevelUp();
        
        this.grid = Array.from({ length: ROWS }, () => Array(COLS).fill(null));
        this.score = 0;
        this.gold = 0;
        this.lines = 0;
        this.level = 1;
        this.gameOver = false;
        this.paused = false;
        this.isTimeFrozen = false;
        this.nextIsBomb = false;
        this.clearingLines = false;
        
        if (this.freezeTimer) {
            clearTimeout(this.freezeTimer);
            this.freezeTimer = null;
        }
        this.updateStatsDisplay();
        this.updateShopButtons();
        
        this.currentPiece = null;
        this.nextPiece = this.generatePiece();
        this.heldPiece = null;
        this.canHold = true;
        
        this.spawnPiece();
        
        // Hide Overlays
        this.overlay.classList.add('hidden');
        this.startContent.classList.add('hidden');
        this.pauseContent.classList.add('hidden');
        this.gameoverContent.classList.add('hidden');
        
        this.lastFallTime = performance.now();
        requestAnimationFrame((t) => this.animate(t));
    }
    
    // Toggle Pause
    togglePause() {
        if (this.gameOver || !this.currentPiece) return;
        
        if (this.paused) {
            this.resumeGame();
        } else {
            this.paused = true;
            this.overlay.classList.remove('hidden');
            this.pauseContent.classList.remove('hidden');
        }
    }
    resumeGame() {
        this.paused = false;
        this.overlay.classList.add('hidden');
        this.pauseContent.classList.add('hidden');
        this.lastFallTime = performance.now();
        requestAnimationFrame((t) => this.animate(t));
    }
    // Trigger Game Over
    triggerGameOver() {
        this.gameOver = true;
        this.sound.playGameOver();
        
        document.getElementById('final-score').innerText = this.formatScore(this.score);
        document.getElementById('final-gold').innerText = this.gold + 'G';
        
        this.overlay.classList.remove('hidden');
        this.gameoverContent.classList.remove('hidden');
    }
    // Select currency type based on probabilities
    getRandomCurrency() {
        const r = Math.random();
        let cumulative = 0;
        for (const [key, details] of Object.entries(CURRENCIES)) {
            cumulative += details.weight;
            if (r <= cumulative) {
                return key;
            }
        }
        return 'USD';
    }
    // Generate random tetromino
    generatePiece() {
        const keys = Object.keys(TETROMINOES);
        const type = keys[Math.floor(Math.random() * keys.length)];
        const matrix = TETROMINOES[type].map(row => [...row]);
        
        // Assign Currency to piece
        const currencyKey = this.getRandomCurrency();
        
        return {
            matrix,
            type,
            currencyKey,
            x: Math.floor((COLS - matrix[0].length) / 2),
            y: -2,
            isBomb: false
        };
    }
    // Spawn a piece from "next" slot
    spawnPiece() {
        this.currentPiece = this.nextPiece;
        
        // Apply Bomb upgrade if bought
        if (this.nextIsBomb) {
            this.currentPiece.isBomb = true;
            this.nextIsBomb = false;
            // Un-press button/visually normalize
            document.getElementById('shop-bomb').classList.remove('active');
        }
        
        this.nextPiece = this.generatePiece();
        this.canHold = true;
        
        // Check if spawn position is colliding (Game Over trigger)
        if (this.checkCollision(this.currentPiece.x, this.currentPiece.y, this.currentPiece.matrix)) {
            this.triggerGameOver();
        }
        
        this.updateFallSpeed();
        this.drawPreviews();
    }
    // Collision Check
    checkCollision(offsetX, offsetY, matrix) {
        for (let r = 0; r < matrix.length; r++) {
            for (let c = 0; c < matrix[r].length; c++) {
                if (matrix[r][c] !== 0) {
                    const gridX = offsetX + c;
                    const gridY = offsetY + r;
                    
                    // Allow pieces to spawn above the ceiling, but block lateral/bottom boundary escapes
                    if (gridX < 0 || gridX >= COLS || gridY >= ROWS) {
                        return true;
                    }
                    
                    if (gridY >= 0 && this.grid[gridY][gridX] !== null) {
                        return true;
                    }
                }
            }
        }
        return false;
    }
    // Piece Movement & Actions
    moveLeft() {
        if (!this.checkCollision(this.currentPiece.x - 1, this.currentPiece.y, this.currentPiece.matrix)) {
            this.currentPiece.x--;
            this.sound.playMove();
        }
    }
    moveRight() {
        if (!this.checkCollision(this.currentPiece.x + 1, this.currentPiece.y, this.currentPiece.matrix)) {
            this.currentPiece.x++;
            this.sound.playMove();
        }
    }
    moveDown() {
        if (!this.checkCollision(this.currentPiece.x, this.currentPiece.y + 1, this.currentPiece.matrix)) {
            this.currentPiece.y++;
            return true;
        }
        this.lockPiece();
        return false;
    }
    hardDrop() {
        let cellsDropped = 0;
        while (!this.checkCollision(this.currentPiece.x, this.currentPiece.y + 1, this.currentPiece.matrix)) {
            this.currentPiece.y++;
            cellsDropped++;
        }
        this.score += cellsDropped * 2;
        this.lockPiece();
    }
    rotatePiece() {
        const matrix = this.currentPiece.matrix;
        const n = matrix.length;
        
        // Create rotated matrix
        const rotated = Array.from({ length: n }, () => Array(n).fill(0));
        for (let r = 0; r < n; r++) {
            for (let c = 0; c < n; c++) {
                rotated[c][n - 1 - r] = matrix[r][c];
            }
        }
        
        // Rotation wall kick (tries to push block horizontally if it collides on boundaries)
        const kicks = [0, -1, 1, -2, 2];
        for (let kick of kicks) {
            if (!this.checkCollision(this.currentPiece.x + kick, this.currentPiece.y, rotated)) {
                this.currentPiece.matrix = rotated;
                this.currentPiece.x += kick;
                this.sound.playRotate();
                return;
            }
        }
    }
    holdPiece() {
        if (!this.canHold) return;
        this.sound.playHold();
        
        const temp = this.heldPiece;
        
        // Store current, reset coordinates, remove upgrades like Bomb from held
        this.heldPiece = {
            matrix: TETROMINOES[this.currentPiece.type].map(row => [...row]),
            type: this.currentPiece.type,
            currencyKey: this.currentPiece.currencyKey,
            x: Math.floor((COLS - TETROMINOES[this.currentPiece.type][0].length) / 2),
            y: -2,
            isBomb: false
        };
        
        if (temp === null) {
            this.spawnPiece();
        } else {
            this.currentPiece = temp;
            this.canHold = false;
        }
        
        this.drawPreviews();
    }
    // Lock piece and handle line cleans or bombs
    lockPiece() {
        this.sound.playLand();
        const matrix = this.currentPiece.matrix;
        const currencyKey = this.currentPiece.currencyKey;
        
        const lockedCells = [];
        
        for (let r = 0; r < matrix.length; r++) {
            for (let c = 0; c < matrix[r].length; c++) {
                if (matrix[r][c] !== 0) {
                    const gridX = this.currentPiece.x + c;
                    const gridY = this.currentPiece.y + r;
                    
                    if (gridY >= 0) {
                        this.grid[gridY][gridX] = {
                            currencyKey: currencyKey,
                            symbol: CURRENCIES[currencyKey].symbol,
                            opacity: 1.0,
                            isBombIndicator: this.currentPiece.isBomb
                        };
                        lockedCells.push({ x: gridX, y: gridY });
                    }
                }
            }
        }
        
        // Handle Bomb Explosion if current piece was a bomb
        if (this.currentPiece.isBomb) {
            this.triggerBombExplosion(lockedCells);
        } else {
            this.checkLineClears();
        }
    }
    // Bomb Explosion Mechanic (3x3 grid cleanup)
    triggerBombExplosion(lockedCells) {
        this.clearingLines = true;
        this.sound.playBomb();
        
        // Find centers of locked cells (just use the average or last cell)
        if (lockedCells.length === 0) {
            this.clearingLines = false;
            this.spawnPiece();
            return;
        }
        
        const midCell = lockedCells[Math.floor(lockedCells.length / 2)];
        const startX = Math.max(0, midCell.x - 1);
        const endX = Math.min(COLS - 1, midCell.x + 1);
        const startY = Math.max(0, midCell.y - 1);
        const endY = Math.min(ROWS - 1, midCell.y + 1);
        
        // Render explosion visual effect on grid cells
        for (let y = startY; y <= endY; y++) {
            for (let x = startX; x <= endX; x++) {
                if (this.grid[y][x]) {
                    this.grid[y][x].exploding = true;
                    // Spawn a minor flying coin for destroyed blocks! (compensation cash)
                    this.spawnGoldCoin(x, y, 0.5); // 0.5G rate
                }
            }
        }
        
        // Wait for visual flash
        setTimeout(() => {
            for (let y = startY; y <= endY; y++) {
                for (let x = startX; x <= endX; x++) {
                    this.grid[y][x] = null;
                }
            }
            
            // Apply gravity to collapse hanging blocks
            this.applyGravity();
            
            this.clearingLines = false;
            this.spawnPiece();
        }, 300);
    }
    // Pull blocks down when spaces clear underneath (due to explosions)
    applyGravity() {
        for (let c = 0; c < COLS; c++) {
            let emptySpaces = 0;
            // Iterate bottom to top
            for (let r = ROWS - 1; r >= 0; r--) {
                if (this.grid[r][c] === null) {
                    emptySpaces++;
                } else if (emptySpaces > 0) {
                    // Pull block down
                    this.grid[r + emptySpaces][c] = this.grid[r][c];
                    this.grid[r][c] = null;
                }
            }
        }
    }
    // Check for completed horizontal rows
    checkLineClears() {
        const completedRows = [];
        
        for (let r = ROWS - 1; r >= 0; r--) {
            let rowFull = true;
            for (let c = 0; c < COLS; c++) {
                if (this.grid[r][c] === null) {
                    rowFull = false;
                    break;
                }
            }
            if (rowFull) {
                completedRows.push(r);
            }
        }
        
        if (completedRows.length > 0) {
            this.clearingLines = true;
            this.sound.playClear();
            
            // Calculate Value of Cleared Lines and Trigger Coin animations
            let totalRowCoins = 0;
            
            completedRows.forEach(row => {
                for (let c = 0; c < COLS; c++) {
                    const cell = this.grid[row][c];
                    if (cell) {
                        const currency = CURRENCIES[cell.currencyKey];
                        // Rate dictates currency value
                        const coinVal = currency.rate;
                        totalRowCoins += coinVal;
                        
                        // Mark cell as cleared for flash effect
                        cell.cleared = true;
                        
                        // Spawn physical gold coin that flies to the scoreboard
                        this.spawnGoldCoin(c, row, coinVal);
                    }
                }
            });
            
            // Wait for coin animation (visual flash & coins launch)
            setTimeout(() => {
                // Remove rows and slide down
                completedRows.forEach(row => {
                    this.grid.splice(row, 1);
                    this.grid.unshift(Array(COLS).fill(null));
                });
                
                // Add Score & Lines cleared
                this.lines += completedRows.length;
                this.score += completedRows.length * 100 * this.level;
                
                // Level Up Check
                const newLevel = Math.floor(this.lines / 10) + 1;
                if (newLevel > this.level) {
                    this.level = newLevel;
                    this.sound.playLevelUp();
                }
                
                this.updateStatsDisplay();
                this.clearingLines = false;
                this.spawnPiece();
            }, 600);
        } else {
            this.spawnPiece();
        }
    }
    // Convert cleared block to gold coin & animate flying to Treasury
    spawnGoldCoin(gridX, gridY, goldWorth) {
        // Calculate cell center on viewport
        const rect = this.canvas.getBoundingClientRect();
        const cellW = rect.width / COLS;
        const cellH = rect.height / ROWS;
        
        const originX = rect.left + gridX * cellW + cellW / 2;
        const originY = rect.top + gridY * cellH + cellH / 2;
        
        // Get Treasury Target coordinates
        const treasuryIcon = document.querySelector('.gold-coin-icon');
        const targetRect = treasuryIcon.getBoundingClientRect();
        const targetX = targetRect.left + targetRect.width / 2;
        const targetY = targetRect.top + targetRect.height / 2;
        
        // Spawn Floating Coin DOM Element
        const coin = document.createElement('div');
        coin.className = 'flying-coin';
        coin.innerText = 'G';
        coin.style.left = `${originX - 12}px`;
        coin.style.top = `${originY - 12}px`;
        
        document.body.appendChild(coin);
        
        // Animate
        setTimeout(() => {
            // Apply fly transition
            coin.style.transform = `translate(${targetX - originX}px, ${targetY - originY}px) scale(0.6)`;
            coin.style.opacity = '0.7';
        }, 50);
        
        // Cleanup and reward Gold
        setTimeout(() => {
            coin.remove();
            this.gold += Math.ceil(goldWorth);
            this.sound.playCoin();
            
            // Pulse treasury display
            const display = document.getElementById('gold-val');
            display.innerText = this.gold;
            display.style.transform = 'scale(1.2)';
            setTimeout(() => display.style.transform = 'scale(1)', 100);
            
            this.updateShopButtons();
        }, 850);
    }
    // Power-up Purchase handler
    buyPowerup(type) {
        let cost = 0;
        let button = null;
        
        if (type === 'bomb') {
            cost = 50;
            button = document.getElementById('shop-bomb');
        } else if (type === 'clear') {
            cost = 80;
            button = document.getElementById('shop-clear');
        } else if (type === 'freeze') {
            cost = 100;
            button = document.getElementById('shop-freeze');
        }
        
        if (this.gold >= cost) {
            this.gold -= cost;
            this.goldVal.innerText = this.gold;
            this.updateShopButtons();
            
            if (type === 'bomb') {
                this.sound.playRotate(); // success boop
                this.nextIsBomb = true;
                button.classList.add('active');
            } else if (type === 'clear') {
                this.sound.playLaser();
                this.laserClearEffect();
            } else if (type === 'freeze') {
                this.sound.playFreeze();
                this.activateTimeFreeze();
            }
        }
    }
    // Laser Powerup: Clears bottom 2 rows
    laserClearEffect() {
        this.clearingLines = true;
        
        // Highlight bottom 2 rows as clearing
        for (let r = ROWS - 2; r < ROWS; r++) {
            for (let c = 0; c < COLS; c++) {
                if (this.grid[r][c]) {
                    this.grid[r][c].cleared = true;
                    // convert blocks to minor coins
                    this.spawnGoldCoin(c, r, 0.5);
                }
            }
        }
        
        setTimeout(() => {
            // Drop rows
            this.grid.splice(ROWS - 2, 2);
            this.grid.unshift(Array(COLS).fill(null), Array(COLS).fill(null));
            
            this.clearingLines = false;
            this.draw();
        }, 500);
    }
    // Freeze Powerup: slows fall speed for 15 seconds
    activateTimeFreeze() {
        if (this.isTimeFrozen) {
            clearTimeout(this.freezeTimer);
        }
        this.isTimeFrozen = true;
        this.updateFallSpeed();
        
        // Add blue ice frame overlay to grid border
        this.canvas.style.borderColor = '#00b0ff';
        this.canvas.style.boxShadow = '0 0 40px rgba(0, 176, 255, 0.4)';
        
        this.freezeTimer = setTimeout(() => {
            this.isTimeFrozen = false;
            this.updateFallSpeed();
            this.canvas.style.borderColor = 'rgba(0, 176, 255, 0.15)';
            this.canvas.style.boxShadow = '0 0 40px rgba(0, 176, 255, 0.1)';
        }, 15000);
    }
    // Speed scaling
    updateFallSpeed() {
        // Decrease interval by 10% per level
        const levelSpeed = Math.max(100, this.baseFallInterval - (this.level - 1) * 80);
        this.fallInterval = this.isTimeFrozen ? levelSpeed * 2.5 : levelSpeed;
    }
    // Refresh UI display values
    updateStatsDisplay() {
        this.scoreVal.innerText = this.formatScore(this.score);
        this.linesVal.innerText = this.lines;
        this.levelVal.innerText = this.level;
        this.goldVal.innerText = this.gold;
    }
    // Enable/Disable shop buttons based on gold balance
    updateShopButtons() {
        const bombBtn = document.getElementById('shop-bomb');
        const clearBtn = document.getElementById('shop-clear');
        const freezeBtn = document.getElementById('shop-freeze');
        
        const mBomb = document.getElementById('m-bomb');
        const mClear = document.getElementById('m-clear');
        const mFreeze = document.getElementById('m-freeze');
        
        bombBtn.disabled = this.gold < 50;
        clearBtn.disabled = this.gold < 80;
        freezeBtn.disabled = this.gold < 100;
        
        mBomb.disabled = this.gold < 50;
        mClear.disabled = this.gold < 80;
        mFreeze.disabled = this.gold < 100;
    }
    formatScore(num) {
        return num.toString().padStart(6, '0').replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
    // Live Currency Fluctuation
    updateTickerRates() {
        const trendIcon = (prev, curr) => curr >= prev ? '<span class="trend up">▲</span>' : '<span class="trend down">▼</span>';
        
        Object.keys(CURRENCIES).forEach(key => {
            const curr = CURRENCIES[key];
            const prevRate = curr.rate;
            
            // Fluctuate rate slightly (up to 12%)
            const fluctuation = (Math.random() - 0.48) * 0.18; // slight bias to climb
            curr.rate = Math.max(0.2, +(curr.rate * (1 + fluctuation)).toFixed(2));
            
            // Cap max rate for balance
            if (key === 'BTC') curr.rate = Math.min(15.00, curr.rate);
            else if (key === 'GBP') curr.rate = Math.min(5.00, curr.rate);
            else if (key === 'EUR') curr.rate = Math.min(3.50, curr.rate);
            else curr.rate = Math.min(2.50, curr.rate);
            
            // Update UI instances
            const rateIds = [`rate-${key.toLowerCase()}`, `rate-${key.toLowerCase()}2`];
            rateIds.forEach(id => {
                const el = document.getElementById(id);
                if (el) {
                    el.innerText = curr.rate.toFixed(2);
                    const parent = el.parentElement;
                    const existingTrend = parent.querySelector('.trend');
                    if (existingTrend) existingTrend.remove();
                    parent.insertAdjacentHTML('beforeend', trendIcon(prevRate, curr.rate));
                }
            });
        });
    }
    startTickerFluctuations() {
        this.tickerInterval = setInterval(() => {
            this.updateTickerRates();
        }, 12000); // fluctuate every 12 seconds
    }
    // Canvas Rendering Methods
    draw() {
        this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
        
        // Draw Grid Lines (thin & futuristic)
        this.ctx.strokeStyle = 'rgba(255, 255, 255, 0.03)';
        this.ctx.lineWidth = 1;
        for (let c = 1; c < COLS; c++) {
            this.ctx.beginPath();
            this.ctx.moveTo(c * this.blockSize, 0);
            this.ctx.lineTo(c * this.blockSize, this.canvas.height);
            this.ctx.stroke();
        }
        for (let r = 1; r < ROWS; r++) {
            this.ctx.beginPath();
            this.ctx.moveTo(0, r * this.blockSize);
            this.ctx.lineTo(this.canvas.width, r * this.blockSize);
            this.ctx.stroke();
        }
        // Draw Locked Grid blocks
        for (let r = 0; r < ROWS; r++) {
            for (let c = 0; c < COLS; c++) {
                if (this.grid[r][c] !== null) {
                    const block = this.grid[r][c];
                    
                    if (block.cleared) {
                        // Cleared animation: Flash bright white
                        this.drawBlock(c, r, 'white', 'G', true);
                    } else if (block.exploding) {
                        // Exploding animation
                        this.drawBlock(c, r, '#ff5252', '💥', true);
                    } else {
                        this.drawBlock(c, r, block.currencyKey, block.symbol, false, block.isBombIndicator);
                    }
                }
            }
        }
        // Draw current falling block
        if (this.currentPiece && !this.clearingLines) {
            // Draw Ghost piece first (projection layout at bottom)
            this.drawGhostPiece();
            
            const matrix = this.currentPiece.matrix;
            for (let r = 0; r < matrix.length; r++) {
                for (let c = 0; c < matrix[r].length; c++) {
                    if (matrix[r][c] !== 0) {
                        const gridX = this.currentPiece.x + c;
                        const gridY = this.currentPiece.y + r;
                        if (gridY >= 0) {
                            this.drawBlock(gridX, gridY, this.currentPiece.currencyKey, this.currentPiece.isBomb ? '💣' : CURRENCIES[this.currentPiece.currencyKey].symbol, false, this.currentPiece.isBomb);
                        }
                    }
                }
            }
        }
    }
    // Drawing a Ghost drop block
    drawGhostPiece() {
        let ghostY = this.currentPiece.y;
        while (!this.checkCollision(this.currentPiece.x, ghostY + 1, this.currentPiece.matrix)) {
            ghostY++;
        }
        
        const matrix = this.currentPiece.matrix;
        const color = CURRENCIES[this.currentPiece.currencyKey].color;
        
        for (let r = 0; r < matrix.length; r++) {
            for (let c = 0; c < matrix[r].length; c++) {
                if (matrix[r][c] !== 0) {
                    const gridX = this.currentPiece.x + c;
                    const gridY = ghostY + r;
                    
                    if (gridY >= 0) {
                        this.ctx.strokeStyle = color;
                        this.ctx.lineWidth = 1.5;
                        this.ctx.globalAlpha = 0.25;
                        // Draw hollow box
                        this.ctx.strokeRect(gridX * this.blockSize + 2, gridY * this.blockSize + 2, this.blockSize - 4, this.blockSize - 4);
                    }
                }
            }
        }
        this.ctx.globalAlpha = 1.0;
    }
    // Draws a beautiful premium block
    drawBlock(gridX, gridY, currencyKey, charSymbol, isFlash = false, isBomb = false) {
        const x = gridX * this.blockSize;
        const y = gridY * this.blockSize;
        const size = this.blockSize;
        const padding = 2.5;
        
        this.ctx.save();
        
        // Set rounded corners path
        const rx = x + padding;
        const ry = y + padding;
        const rw = size - padding * 2;
        const rh = size - padding * 2;
        const radius = 6;
        
        this.ctx.beginPath();
        this.ctx.moveTo(rx + radius, ry);
        this.ctx.lineTo(rx + rw - radius, ry);
        this.ctx.quadraticCurveTo(rx + rw, ry, rx + rw, ry + radius);
        this.ctx.lineTo(rx + rw, ry + rh - radius);
        this.ctx.quadraticCurveTo(rx + rw, ry + rh, rx + rw - radius, ry + rh);
        this.ctx.lineTo(rx + radius, ry + rh);
        this.ctx.quadraticCurveTo(rx, ry + rh, rx, ry + rh - radius);
        this.ctx.lineTo(rx, ry + radius);
        this.ctx.quadraticCurveTo(rx, ry, rx + radius, ry);
        this.ctx.closePath();
        
        if (isFlash) {
            this.ctx.fillStyle = charSymbol === 'G' ? 'rgba(255, 215, 0, 0.95)' : 'rgba(255, 82, 82, 0.95)';
            this.ctx.shadowColor = charSymbol === 'G' ? '#ffd700' : '#ff1744';
            this.ctx.shadowBlur = 15;
            this.ctx.fill();
        } else {
            const currency = CURRENCIES[currencyKey];
            
            // Box shadow neon effect
            this.ctx.shadowColor = currency.color;
            this.ctx.shadowBlur = 8;
            
            // Gradient fill (metallic neon)
            const grad = this.ctx.createLinearGradient(rx, ry, rx + rw, ry + rh);
            grad.addColorStop(0, this.lightenColor(currency.color, 40));
            grad.addColorStop(0.5, currency.color);
            grad.addColorStop(1, this.darkenColor(currency.color, 40));
            
            this.ctx.fillStyle = grad;
            this.ctx.fill();
            
            // Border stroke
            this.ctx.strokeStyle = isBomb ? '#ff1744' : this.lightenColor(currency.color, 20);
            this.ctx.lineWidth = isBomb ? 2.5 : 1.5;
            this.ctx.shadowBlur = 0; // disable shadow for border
            this.ctx.stroke();
            // Inner glass sheen
            this.ctx.fillStyle = 'rgba(255, 255, 255, 0.15)';
            this.ctx.beginPath();
            this.ctx.moveTo(rx, ry);
            this.ctx.lineTo(rx + rw, ry);
            this.ctx.lineTo(rx + rw, ry + rh * 0.3);
            this.ctx.lineTo(rx, ry + rh * 0.45);
            this.ctx.closePath();
            this.ctx.fill();
        }
        
        // Draw Symbol inside block
        this.ctx.fillStyle = isFlash ? '#000' : 'rgba(255, 255, 255, 0.9)';
        this.ctx.font = `bold ${size * 0.45}px 'Space Grotesk'`;
        this.ctx.textAlign = 'center';
        this.ctx.textBaseline = 'middle';
        
        // Add subtle shadow behind text
        this.ctx.shadowColor = 'rgba(0, 0, 0, 0.5)';
        this.ctx.shadowBlur = 3;
        this.ctx.shadowOffsetY = 1;
        
        this.ctx.fillText(charSymbol, rx + rw / 2, ry + rh / 2);
        
        this.ctx.restore();
    }
    // Helper functions to change color brightness
    lightenColor(hex, percent) {
        let num = parseInt(hex.replace("#",""), 16),
            amt = Math.round(2.55 * percent),
            R = (num >> 16) + amt,
            G = (num >> 8 & 0x00FF) + amt,
            B = (num & 0x0000FF) + amt;
        return "#" + (0x1000000 + (R<255?R<0?0:R:255)*0x10000 + (G<255?G<0?0:G:255)*0x100 + (B<255?B<0?0:B:255)).toString(16).slice(1);
    }
    darkenColor(hex, percent) {
        let num = parseInt(hex.replace("#",""), 16),
            amt = Math.round(2.55 * percent),
            R = (num >> 16) - amt,
            G = (num >> 8 & 0x00FF) - amt,
            B = (num & 0x0000FF) - amt;
        return "#" + (0x1000000 + (R<255?R<0?0:R:255)*0x10000 + (G<255?G<0?0:G:255)*0x100 + (B<255?B<0?0:B:255)).toString(16).slice(1);
    }
    // Previews (Next and Hold slots rendering)
    drawPreviews() {
        // Next Preview
        this.nextCtx.clearRect(0, 0, this.nextCanvas.width, this.nextCanvas.height);
        if (this.nextPiece) {
            const matrix = this.nextPiece.matrix;
            const currencyKey = this.nextPiece.currencyKey;
            const size = 22; // custom block size for side boxes
            const cellW = this.nextCanvas.width;
            const cellH = this.nextCanvas.height;
            
            const pieceW = matrix[0].length * size;
            const pieceH = matrix.length * size;
            
            const startX = (cellW - pieceW) / 2;
            const startY = (cellH - pieceH) / 2;
            
            this.drawPreviewGrid(this.nextCtx, matrix, currencyKey, startX, startY, size);
        }
        
        // Hold Preview
        this.holdCtx.clearRect(0, 0, this.holdCanvas.width, this.holdCanvas.height);
        if (this.heldPiece) {
            const matrix = this.heldPiece.matrix;
            const currencyKey = this.heldPiece.currencyKey;
            const size = 22;
            const cellW = this.holdCanvas.width;
            const cellH = this.holdCanvas.height;
            
            const pieceW = matrix[0].length * size;
            const pieceH = matrix.length * size;
            
            const startX = (cellW - pieceW) / 2;
            const startY = (cellH - pieceH) / 2;
            
            this.drawPreviewGrid(this.holdCtx, matrix, currencyKey, startX, startY, size);
        }
    }
    drawPreviewGrid(ctx, matrix, currencyKey, startX, startY, size) {
        const padding = 1.5;
        const radius = 4;
        
        for (let r = 0; r < matrix.length; r++) {
            for (let c = 0; c < matrix[r].length; c++) {
                if (matrix[r][c] !== 0) {
                    const rx = startX + c * size + padding;
                    const ry = startY + r * size + padding;
                    const rw = size - padding * 2;
                    const rh = size - padding * 2;
                    
                    ctx.save();
                    
                    ctx.beginPath();
                    ctx.moveTo(rx + radius, ry);
                    ctx.lineTo(rx + rw - radius, ry);
                    ctx.quadraticCurveTo(rx + rw, ry, rx + rw, ry + radius);
                    ctx.lineTo(rx + rw, ry + rh - radius);
                    ctx.quadraticCurveTo(rx + rw, ry + rh, rx + rw - radius, ry + rh);
                    ctx.lineTo(rx + radius, ry + rh);
                    ctx.quadraticCurveTo(rx, ry + rh, rx, ry + rh - radius);
                    ctx.lineTo(rx, ry + radius);
                    ctx.quadraticCurveTo(rx, ry, rx + radius, ry);
                    ctx.closePath();
                    
                    const currency = CURRENCIES[currencyKey];
                    ctx.shadowColor = currency.color;
                    ctx.shadowBlur = 4;
                    
                    const grad = ctx.createLinearGradient(rx, ry, rx + rw, ry + rh);
                    grad.addColorStop(0, this.lightenColor(currency.color, 40));
                    grad.addColorStop(0.5, currency.color);
                    grad.addColorStop(1, this.darkenColor(currency.color, 40));
                    
                    ctx.fillStyle = grad;
                    ctx.fill();
                    
                    ctx.strokeStyle = this.lightenColor(currency.color, 20);
                    ctx.lineWidth = 1;
                    ctx.shadowBlur = 0;
                    ctx.stroke();
                    
                    // Symbol
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.9)';
                    ctx.font = `bold ${size * 0.5}px 'Space Grotesk'`;
                    ctx.textAlign = 'center';
                    ctx.textBaseline = 'middle';
                    ctx.fillText(currency.symbol, rx + rw / 2, ry + rh / 2);
                    
                    ctx.restore();
                }
            }
        }
    }
    
}
// Initialise Game Engine when page loads
window.addEventListener('DOMContentLoaded', () => {
    window.gameInstance = new Game();
});
