<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <!-- 关键：移动端适配标签 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>强制双休制度调查</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 15px;
            /* 修复iOS橡皮筋效果 */
            overscroll-behavior: none;
        }
        
        .container {
            background: white;
            border-radius: 16px;
            padding: 20px;
            max-width: 500px;
            width: 100%;
            margin: 0 auto;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 8px;
            font-size: 20px;
            line-height: 1.4;
        }
        
        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 20px;
            font-size: 13px;
        }
        
        .question {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 20px;
            border-left: 3px solid #667eea;
        }
        
        .question-text {
            font-size: 15px;
            color: #333;
            line-height: 1.6;
        }
        
        .options {
            display: grid;
            gap: 12px;
            margin-bottom: 20px;
        }
        
        /* 移动端优化：更大的点击区域 */
        .option-btn {
            padding: 18px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            background: white;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            justify-content: space-between;
            /* 移动端优化 */
            min-height: 60px;
            -webkit-touch-callout: none;
            user-select: none;
            touch-action: manipulation;
        }
        
        .option-btn:active {
            transform: scale(0.98);
            opacity: 0.9;
        }
        
        .option-btn:hover {
            border-color: #667eea;
        }
        
        .option-btn.voted {
            pointer-events: none;
            opacity: 0.8;
        }
        
        .option-btn.agree {
            border-color: #4CAF50;
            background: #f1f8f4;
        }
        
        .option-btn.disagree {
            border-color: #f44336;
            background: #fef5f5;
        }
        
        .option-label {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 15px;
            font-weight: 500;
        }
        
        .icon {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            flex-shrink: 0;
        }
        
        .agree .icon {
            background: #4CAF50;
            color: white;
        }
        
        .disagree .icon {
            background: #f44336;
            color: white;
        }
        
        .vote-count {
            font-size: 18px;
            font-weight: bold;
            color: #333;
        }
        
        .progress-section {
            margin-top: 20px;
        }
        
        .stat-row {
            margin-bottom: 15px;
        }
        
        .stat-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px;
            font-size: 13px;
            color: #666;
        }
        
        .progress-bar {
            height: 10px;
            background: #e0e0e0;
            border-radius: 5px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            border-radius: 5px;
            transition: width 0.5s ease;
            position: relative;
        }
        
        .progress-fill.agree {
            background: linear-gradient(90deg, #4CAF50, #81C784);
        }
        
        .progress-fill.disagree {
            background: linear-gradient(90deg, #f44336, #ef5350);
        }
        
        .percentage {
            position: absolute;
            right: 8px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 11px;
            font-weight: bold;
            color: white;
            text-shadow: 0 1px 2px rgba(0,0,0,0.3);
        }
        
        .total-votes {
            text-align: center;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #e0e0e0;
            color: #666;
            font-size: 13px;
        }
        
        .total-number {
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
            display: block;
            margin-top: 4px;
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        .real-time-badge {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            background: #e8f5e9;
            color: #4CAF50;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 11px;
            margin-left: 6px;
        }
        
        .dot {
            width: 6px;
            height: 6px;
            background: #4CAF50;
            border-radius: 50%;
            animation: blink 1s infinite;
        }
        
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }
        
        .voted-message {
            text-align: center;
            padding: 12px;
            background: #e3f2fd;
            border-radius: 8px;
            color: #1976d2;
            margin-top: 15px;
            display: none;
            font-size: 14px;
        }
        
        /* 移动端横屏优化 */
        @media (max-height: 500px) and (orientation: landscape) {
            .container {
                padding: 15px;
            }
            .option-btn {
                padding: 12px;
                min-height: 50px;
            }
        }
        
        /* 小屏幕手机优化 */
        @media (max-width: 360px) {
            h1 {
                font-size: 18px;
            }
            .question-text {
                font-size: 14px;
            }
            .option-label {
                font-size: 14px;
            }
        }
        
        /* 防止iOS缩放 */
        @supports (-webkit-touch-callout: none) {
            body {
                touch-action: pan-y;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📊 强制双休制度调查</h1>
        <p class="subtitle">实时统计 · 匿名投票 · 即时显示结果</p>
        
        <div class="question">
            <p class="question-text">
                <strong>您是否同意在全国强制实施双休制度（每周工作5天，休息2天）？</strong>
            </p>
        </div>
        
        <div class="options">
            <button class="option-btn agree" onclick="vote('agree')" ontouchstart="">
                <span class="option-label">
                    <span class="icon">✓</span>
                    同意实施
                </span>
                <span class="vote-count" id="agree-count">0</span>
            </button>
            
            <button class="option-btn disagree" onclick="vote('disagree')" ontouchstart="">
                <span class="option-label">
                    <span class="icon">✕</span>
                    不同意
                </span>
                <span class="vote-count" id="disagree-count">0</span>
            </button>
        </div>
        
        <div class="progress-section">
            <div class="stat-row">
                <div class="stat-label">
                    <span>👍 同意比例 <span class="real-time-badge"><span class="dot"></span>实时</span></span>
                    <span id="agree-percent">0%</span>
                </div>
                <div class="progress-bar">
                    <div class="progress-fill agree" id="agree-bar" style="width: 0%">
                        <span class="percentage" id="agree-bar-percent">0%</span>
                    </div>
                </div>
            </div>
            
            <div class="stat-row">
                <div class="stat-label">
                    <span>👎 不同意比例</span>
                    <span id="disagree-percent">0%</span>
                </div>
                <div class="progress-bar">
                    <div class="progress-fill disagree" id="disagree-bar" style="width: 0%">
                        <span class="percentage" id="disagree-bar-percent">0%</span>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="total-votes">
            总投票人数
            <span class="total-number pulse" id="total-count">0</span>
        </div>
        
        <div class="voted-message" id="voted-msg">
            ✅ 您已完成投票，结果已实时更新
        </div>
    </div>

    <script>
        // 初始化数据
        let votes = {
            agree: 3428,
            disagree: 856
        };
        
        let hasVoted = false;
        
        // 检查本地存储（防止重复投票）
        function checkLocalVote() {
            const voted = localStorage.getItem('hasVoted_doubleRest');
            if (voted) {
                hasVoted = true;
                document.querySelectorAll('.option-btn').forEach(btn => {
                    btn.classList.add('voted');
                });
                document.getElementById('voted-msg').style.display = 'block';
            }
        }
        
        // 投票功能
        function vote(type) {
            if (hasVoted) {
                alert('您已经投过票了');
                return;
            }
            
            // 增加票数
            votes[type]++;
            
            // 标记已投票
            hasVoted = true;
            localStorage.setItem('hasVoted_doubleRest', 'true');
            
            // 更新按钮状态
            document.querySelectorAll('.option-btn').forEach(btn => {
                btn.classList.add('voted');
            });
            
            // 高亮选中
            if (type === 'agree') {
                document.querySelector('.option-btn.agree').style.borderWidth = '3px';
            } else {
                document.querySelector('.option-btn.disagree').style.borderWidth = '3px';
            }
            
            // 显示投票成功消息
            document.getElementById('voted-msg').style.display = 'block';
            
            // 更新显示
            updateDisplay();
            
            // 模拟实时同步
            simulateRealTimeUpdate();
        }
        
        // 更新显示
        function updateDisplay() {
            const total = votes.agree + votes.disagree;
            
            // 更新计数
            document.getElementById('agree-count').textContent = votes.agree.toLocaleString();
            document.getElementById('disagree-count').textContent = votes.disagree.toLocaleString();
            document.getElementById('total-count').textContent = total.toLocaleString();
            
            // 计算百分比
            const agreePercent = total > 0 ? (votes.agree / total * 100).toFixed(1) : 0;
            const disagreePercent = total > 0 ? (votes.disagree / total * 100).toFixed(1) : 0;
            
            // 更新百分比文本
            document.getElementById('agree-percent').textContent = agreePercent + '%';
            document.getElementById('disagree-percent').textContent = disagreePercent + '%';
            
            // 更新进度条
            document.getElementById('agree-bar').style.width = agreePercent + '%';
            document.getElementById('disagree-bar').style.width = disagreePercent + '%';
            
            // 更新进度条内百分比
            document.getElementById('agree-bar-percent').textContent = agreePercent + '%';
            document.getElementById('disagree-bar-percent').textContent = disagreePercent + '%';
        }
        
        // 模拟实时更新
        function simulateRealTimeUpdate() {
            setInterval(() => {
                if (Math.random() > 0.5) {
                    votes.agree++;
                } else {
                    votes.disagree++;
                }
                updateDisplay();
            }, 3000 + Math.random() * 4000);
        }
        
        // 页面加载时初始化
        window.onload = function() {
            checkLocalVote();
            updateDisplay();
        };
        
        // 防止双击缩放（iOS）
        let lastTouchEnd = 0;
        document.addEventListener('touchend', function(event) {
            const now = Date.now();
            if (now - lastTouchEnd <= 300) {
                event.preventDefault();
            }
            lastTouchEnd = now;
        }, false);
    </script>
</body>
</html>
