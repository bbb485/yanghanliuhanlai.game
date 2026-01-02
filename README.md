
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>养汉大作战 - GitHub版</title>
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🐕</text></svg>">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        
        body {
            font-family: 'Courier New', 'Microsoft YaHei', sans-serif;
            background: #000;
            color: #0f0;
            overflow-x: hidden;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .header {
            text-align: center;
            margin-bottom: 20px;
            width: 100%;
            max-width: 800px;
        }
        
        .title {
            font-size: 2.5rem;
            color: #0f0;
            text-shadow: 0 0 10px #0f0;
            margin-bottom: 10px;
            letter-spacing: 2px;
        }
        
        .subtitle {
            color: #8f8;
            font-size: 1.1rem;
            margin-bottom: 15px;
        }
        
        .github-badge {
            display: inline-block;
            background: #333;
            color: #fff;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
            margin-top: 10px;
            text-decoration: none;
        }
        
        .game-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            border: 4px solid #0f0;
            border-radius: 10px;
            box-shadow: 0 0 30px rgba(0, 255, 0, 0.3);
            margin-bottom: 20px;
            overflow: hidden;
        }
        
        #gameCanvas {
            display: block;
            width: 100%;
            background: #000;
        }
        
        .ui-overlay {
            position: absolute;
            top: 15px;
            left: 15px;
            background: rgba(0, 0, 0, 0.7);
            padding: 12px;
            border-radius: 8px;
            border: 2px solid #0f0;
            min-width: 180px;
        }
        
        .ui-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 1rem;
        }
        
        .ui-label {
            color: #8f8;
        }
        
        .ui-value {
            color: #0f0;
            font-weight: bold;
        }
        
        .controls {
            display: flex;
            gap: 20px;
            margin: 20px 0;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .control-btn {
            background: linear-gradient(145deg, #0a0, #0f0);
            border: none;
            color: #000;
            padding: 15px 30px;
            font-size: 1.2rem;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            min-width: 140px;
            transition: all 0.2s;
            box-shadow: 0 5px 0 #080;
        }
        
        .control-btn:active {
            transform: translateY(5px);
            box-shadow: 0 0 0 #080;
        }
        
        .attack-btn {
            background: linear-gradient(145deg, #a00, #f00);
            box-shadow: 0 5px 0 #800;
        }
        
        .share-section {
            background: rgba(0, 30, 0, 0.5);
            padding: 25px;
            border-radius: 15px;
            border: 2px solid #0f0;
            margin-top: 30px;
            width: 100%;
            max-width: 800px;
            text-align: center;
        }
        
        .share-title {
            color: #0f0;
            font-size: 1.5rem;
            margin-bottom: 15px;
        }
        
        .share-link {
            background: #111;
            border: 2px solid #0f0;
            color: #0ff;
            padding: 15px;
            border-radius: 8px;
            word-break: break-all;
            margin: 15px 0;
            font-family: monospace;
            font-size: 1.1rem;
        }
        
        .copy-btn {
            background: #0f0;
            color: #000;
            border: none;
            padding: 12px 25px;
            font-size: 1.1rem;
            border-radius: 8px;
            cursor: pointer;
            margin: 10px;
            font-weight: bold;
        }
        
        .instructions {
            background: rgba(0, 40, 0, 0.3);
            padding: 20px;
            border-radius: 10px;
            margin-top: 25px;
            width: 100%;
            max-width: 800px;
        }
        
        .instructions h3 {
            color: #0f0;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .instructions-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 15px;
        }
        
        .instruction-item {
            background: rgba(0, 20, 0, 0.5);
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #0f0;
        }
        
        @media (max-width: 768px) {
            .title {
                font-size: 2rem;
            }
            
            .control-btn {
                padding: 12px 20px;
                min-width: 120px;
                font-size: 1.1rem;
            }
            
            .ui-overlay {
                position: relative;
                top: 0;
                left: 0;
                margin: 10px;
                width: calc(100% - 20px);
            }
            
            .game-container {
                border-width: 3px;
            }
        }
        
        @media (max-width: 480px) {
            .title {
                font-size: 1.8rem;
            }
            
            .controls {
                gap: 10px;
            }
            
            .control-btn {
                padding: 10px 15px;
                min-width: 100px;
                font-size: 1rem;
            }
            
            .share-link {
                font-size: 0.9rem;
                padding: 10px;
            }
        }
        
        .health-bar-container {
            position: absolute;
            bottom: 15px;
            left: 15px;
            width: 200px;
            height: 20px;
            background: rgba(255, 0, 0, 0.3);
            border: 2px solid #f00;
            border-radius: 10px;
            overflow: hidden;
        }
        
        .health-bar {
            height: 100%;
            background: linear-gradient(90deg, #f00, #0f0);
            transition: width 0.3s;
        }
        
        .wave-indicator {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
            border-radius: 8px;
            border: 2px solid #0f0;
            font-size: 1.2rem;
            font-weight: bold;
        }
        
        .mobile-warning {
            display: none;
            background: #ff0;
            color: #000;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
            text-align: center;
        }
        
        @media (hover: none) and (pointer: coarse) {
            .mobile-warning {
                display: block;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1 class="title">🐕 养汉大作战 🎮</h1>
        <p class="subtitle">控制刘汉来抵御源源不断的狗群进攻！</p>
        <a href="https://github.com" class="github-badge" target="_blank">
            🌐 GitHub Pages 托管版
        </a>
    </div>
    
    <div class="mobile-warning">
        📱 手机用户：建议横屏游戏，体验更佳！
    </div>
    
    <div class="game-container">
        <canvas id="gameCanvas" width="800" height="500"></canvas>
        
        <div class="ui-overlay">
            <div class="ui-row">
                <span class="ui-label">玩家:</span>
                <span class="ui-value" id="playerName">刘汉来</span>
            </div>
            <div class="ui-row">
                <span class="ui-label">得分:</span>
                <span class="ui-value" id="score">0</span>
            </div>
            <div class="ui-row">
                <span class="ui-label">击杀:</span>
                <span class="ui-value" id="kills">0</span>
            </div>
            <div class="ui-row">
                <span class="ui-label">时间:</span>
                <span class="ui-value" id="time">30s</span>
            </div>
        </div>
        
        <div class="health-bar-container">
            <div class="health-bar" id="healthBar"></div>
        </div>
        
        <div class="wave-indicator">
            波次: <span id="wave">1</span>
        </div>
    </div>
    
    <div class="controls">
        <button class="control-btn" id="leftBtn" 
                ontouchstart="game.keys.left = true" 
                ontouchend="game.keys.left = false"
                onmousedown="game.keys.left = true"
                onmouseup="game.keys.left = false"
                onmouseleave="game.keys.left = false">
            ← 左移 (A)
        </button>
        
        <button class="control-btn attack-btn" id="attackBtn"
                onclick="game.playerAttack()">
            ⚔️ 攻击 (空格)
        </button>
        
        <button class="control-btn" id="rightBtn"
                ontouchstart="game.keys.right = true"
                ontouchend="game.keys.right = false"
                onmousedown="game.keys.right = true"
                onmouseup="game.keys.right = false"
                onmouseleave="game.key# yanghanliuhanlai.game
```javascript
// 养汉大作战 - 游戏核心逻辑
// 游戏常量定义
const GameConstants = {
    PLAYER: {
        NAME: "刘汉来",
        WIDTH: 32,
        HEIGHT: 48,
        SPEED: 5,
        MAX_HEALTH: 100,
        BASE_DAMAGE: 15,
        ATTACK_COOLDOWN: 400, // 毫秒
        ATTACK_RANGE: 60
    },
    
    ENEMIES: {
        DOG: {
            name: "土狗",
            width: 28,
            height: 28,
            health: 20,
            damage: 8,
            speed: 2.0,
            score: 10,
            color: "#8B4513",
            spawnWeight: 60
        },
        WOLF_DOG: {
            name: "狼狗",
            width: 32,
            height: 32,
            health: 35,
            damage: 12,
            speed: 2.5,
            score: 20,
            color: "#666666",
            spawnWeight: 30
        },
        MAD_DOG: {
            name: "疯狗",
            width: 36,
            height: 36,
            health: 50,
            damage: 18,
            speed: 3.0,
            score: 35,
            color: "#FF3300",
            spawnWeight: 10
        }
    },
    
    GAME: {
        WAVE_DURATION: 30000, // 30秒
        WAVE_BONUS: 50,
        MAX_ENEMIES: 20,
        SPAWN_INTERVAL: 1000, // 毫秒
        DIFFICULTY_INCREASE: 0.1 // 每波难度增加10%
    }
};

// 游戏主类
class YangHanGame {
    constructor() {
        this.canvas = document.getElementById('gameCanvas');
        this.ctx = this.canvas.getContext('2d');
        this.gameTime = 0;
        this.lastTime = 0;
        this.isRunning = false;
        this.gameOver = false;
        this.score = 0;
        this.highScore = localStorage.getItem('yanghan_highscore') || 0;
        this.kills = 0;
        this.wave = 1;
        this.waveTimeLeft = GameConstants.GAME.WAVE_DURATION;
        this.spawnTimer = 0;
        this.enemies = [];
        this.particles = [];
        this.keys = {
            left: false,
            right: false
        };
        
        // 初始化玩家
        this.player = {
            x: this.canvas.width * 0.2,
            y: this.canvas.height / 2 - GameConstants.PLAYER.HEIGHT / 2,
            width: GameConstants.PLAYER.WIDTH,
            height: GameConstants.PLAYER.HEIGHT,
            health: GameConstants.PLAYER.MAX_HEALTH,
            maxHealth: GameConstants.PLAYER.MAX_HEALTH,
            damage: GameConstants.PLAYER.BASE_DAMAGE,
            speed: GameConstants.PLAYER.SPEED,
            attackCooldown: 0,
            facing: 'right',
            isAttacking: false,
            attackAnimation: 0
        };
        
        this.init();
    }
    
    init() {
        // 初始化UI
        this.updateUI();
        
        // 绑定键盘事件
        this.bindEvents();
        
        // 开始游戏循环
        this.isRunning = true;
        this.lastTime = performance.now();
        this.gameLoop();
        
        console.log('🎮 养汉大作战初始化完成！');
    }
    
    bindEvents() {
        // 键盘控制
        document.addEventListener('keydown', (e) => {
            switch(e.key.toLowerCase()) {
                case 'a':
                case 'arrowleft':
                    this.keys.left = true;
                    this.player.facing = 'left';
                    break;
                case 'd':
                case 'arrowright':
                    this.keys.right = true;
                    this.player.facing = 'right';
                    break;
                case ' ':
                case 'spacebar':
                    this.playerAttack();
                    break;
                case 'r':
                    if (this.gameOver) this.restart();
                    break;
            }
        });
        
        document.addEventListener('keyup', (e) => {
            switch(e.key.toLowerCase()) {
                case 'a':
                case 'arrowleft':
                    this.keys.left = false;
                    break;
                case 'd':
                case 'arrowright':
                    this.keys.right = false;
                    break;
            }
        });
        
        // 防止方向键滚动页面
        window.addEventListener('keydown', (e) => {
            if(['Space','ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].indexOf(e.code) > -1) {
                e.preventDefault();
            }
        }, false);
    }
    
    gameLoop(currentTime) {
        if (!this.isRunning) return;
        
        // 计算时间差
        const deltaTime = currentTime - this.lastTime;
        this.lastTime = currentTime;
        this.gameTime += deltaTime;
        
        // 更新游戏状态
        this.update(deltaTime);
        
        // 渲染游戏
        this.render();
        
        // 继续循环
        requestAnimationFrame((time) => this.gameLoop(time));
    }
    
    update(deltaTime) {
        if (this.gameOver) return;
        
        // 更新波次时间
        this.waveTimeLeft -= deltaTime;
        if (this.waveTimeLeft <= 0) {
            this.nextWave();
        }
        
        // 更新玩家
        this.updatePlayer(deltaTime);
        
        // 生成敌人
        this.spawnTimer -= deltaTime;
        if (this.spawnTimer <= 0) {
            this.spawnEnemy();
            this.spawnTimer = GameConstants.GAME.SPAWN_INTERVAL / (1 + (this.wave - 1) * 0.2);
        }
        
        // 更新敌人
        this.updateEnemies(deltaTime);
        
        // 更新粒子效果
        this.updateParticles(deltaTime);
        
        // 更新攻击冷却
        if (this.player.attackCooldown > 0) {
            this.player.attackCooldown -= deltaTime;
        }
        
        // 更新攻击动画
        if (this.player.isAttacking) {
            this.player.attackAnimation += deltaTime * 0.01;
            if (this.player.attackAnimation >= 1) {
                this.player.isAttacking = false;
                this.player.attackAnimation = 0;
            }
        }
        
        // 更新UI
        this.updateUI();
    }
    
    updatePlayer(deltaTime) {
        // 移动玩家
        if (this.keys.left) {
            this.player.x -= this.player.speed * (deltaTime / 16);
        }
        if (this.keys.right) {
            this.player.x += this.player.speed * (deltaTime / 16);
        }
        
        // 边界检查
        this.player.x = Math.max(20, Math.min(this.player.x, this.canvas.width - this.player.width - 20));
        
        // 检查玩家生命值
        if (this.player.health <= 0) {
            this.gameOver = true;
            if (this.score > this.highScore) {
                this.highScore = this.score;
                localStorage.setItem('yanghan_highscore', this.highScore);
            }
        }
    }
    
    spawnEnemy() {
        if (this.enemies.length >= GameConstants.GAME.MAX_ENEMIES) return;
        
        // 根据权重随机选择敌人类型
        const enemies = [
            GameConstants.ENEMIES.DOG,
            GameConstants.ENEMIES.WOLF_DOG,
            GameConstants.ENEMIES.MAD_DOG
        ];
        
        let totalWeight = enemies.reduce((sum, enemy) => sum + enemy.spawnWeight, 0);
        let random = Math.random() * totalWeight;
        let selectedEnemy = enemies[0];
        
        for (let enemy of enemies) {
            if (random < enemy.spawnWeight) {
                selectedEnemy = enemy;
                break;
            }
            random -= enemy.spawnWeight;
        }
        
        // 调整难度
        const difficulty = 1 + (this.wave - 1) * GameConstants.GAME.DIFFICULTY_INCREASE;
        
        this.enemies.push({
            type: selectedEnemy.name,
            x: this.canvas.width + 50,
            y: 100 + Math.random() * (this.canvas.height - 200),
            width: selectedEnemy.width,
            height: selectedEnemy.height,
            health: selectedEnemy.health * difficulty,
            maxHealth: selectedEnemy.health * difficulty,
            damage: selectedEnemy.damage * difficulty,
            speed: selectedEnemy.speed * difficulty,
            score: Math.floor(selectedEnemy.score * difficulty),
            color: selectedEnemy.color,
            originalColor: selectedEnemy.color
        });
    }
    
    updateEnemies(deltaTime) {
        for (let i = this.enemies.length - 1; i >= 0; i--) {
            const enemy = this.enemies[i];
            
            // 向玩家移动
            const dx = this.player.x - enemy.x;
            const dy = (this.player.y + this.player.height / 2) - (enemy.y + enemy.height / 2);
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance > 10) {
                enemy.x += (dx / distance) * enemy.speed * (deltaTime / 16);
                enemy.y += (dy / distance) * enemy.speed * 0.5 * (deltaTime / 16);
            }
            
            // 边界检查
            enemy.y = Math.max(50, Math.min(enemy.y, this.canvas.height - enemy.height - 50));
            
            // 碰撞检测（敌人攻击玩家）
            if (this.checkCollision(this.player, enemy)) {
                this.player.health -= enemy.damage * 0.1;
                enemy.x += 20; // 击退效果
                
                // 受伤效果
                this.createParticles(enemy.x + enemy.width / 2, enemy.y + enemy.height / 2, 5, '#ff0000');
                
                // 更新生命条
                this.updateHealthBar();
            }
            
            // 移除屏幕外的敌人
            if (enemy.x < -100 || enemy.x > this.canvas.width + 100) {
                this.enemies.splice(i, 1);
            }
        }
    }
    
    playerAttack() {
        if (this.player.attackCooldown > 0 || this.gameOver) return;
        
        this.player.isAttacking = true;
        this.player.attackAnimation = 0;
        this.player.attackCooldown = GameConstants.PLAYER.ATTACK_COOLDOWN;
        
        // 攻击范围
        const attackRange = {
            x: this.player.facing === 'right' 
                ? this.player.x + this.player.width 
                : this.player.x - GameConstants.PLAYER.ATTACK_RANGE,
            y: this.player.y + this.player.height / 4,
            width: GameConstants.PLAYER.ATTACK_RANGE,
            height: this.player.height / 2
        };
        
        // 检测攻击命中的敌人
        let hitCount
```markdown
# 🐕 养汉大作战 🎮

一个基于HTML5 Canvas的FC风格生存射击游戏，使用GitHub Pages免费托管。

## 🎯 游戏简介
控制主角**刘汉来**，使用简单的移动和攻击操作，击败源源不断的狗群敌人。生存越久，得分越高！

## 🕹️ 操作方法
### 电脑端：
- **A / ←** : 向左移动
- **D / →** : 向右移动  
- **空格键** : 攻击
- **R键** : 游戏结束后重新开始

### 手机/平板：
- 点击屏幕上的**左右按钮**移动
- 点击**攻击按钮**进行攻击

## 🐶 敌人类型
1. **土狗** - 基础敌人，10分
2. **狼狗** - 速度较快，20分  
3. **疯狗** - 生命值高，攻击力强，35分

## ⭐ 游戏特色
- 🎮 复古FC像素风格画面
- 📱 完全响应式设计，支持手机/电脑
- 🌊 波次制敌人进攻，难度递增
- 💾 自动保存最高分记录
- 🎨 粒子特效系统
- 🔗 一键分享游戏链接

## 🚀 如何部署
1. Fork或下载本仓库
2. 进入仓库Settings → Pages
3. Source选择main分支
4. 保存后等待部署完成
5. 访问 `https://你的用户名.github.io/仓库名/`

## 📁 文件结构
```

├── index.html      # 主游戏页面
├──game.js         # 游戏核心逻辑
├──README.md       # 说明文档
└──.nojekyll       # GitHub Pages配置

```

## 🔧 技术栈
- HTML5 Canvas
- 原生JavaScript
- CSS3动画
- GitHub Pages托管

## 📄 许可证
MIT License - 可自由修改和分发

## 🤝 贡献
欢迎提交Issue和Pull Request！

## 📞 联系
如有问题，请在GitHub仓库中提交Issue。

---
**游戏愉快！** 🎮
```
