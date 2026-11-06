<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXUS-V // Authentication Portal</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --bg-deep: #05050a;
            --panel-bg: rgba(13, 15, 28, 0.85);
            --neon-cyan: #00f0ff;
            --neon-magenta: #ff007f;
            --neon-purple: #9d00ff;
            --text-main: #f0f4f8;
            --text-muted: #8a99ad;
            --border-glow: rgba(0, 240, 255, 0.3);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Rajdhani', sans-serif;
        }

        body {
            background-color: var(--bg-deep);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            position: relative;
        }

        /* Animated Cyberpunk Background */
        .cyber-grid {
            position: absolute;
            width: 200vw;
            height: 200vh;
            background-image: 
                linear-gradient(rgba(0, 240, 255, 0.05) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 240, 255, 0.05) 1px, transparent 1px);
            background-size: 40px 40px;
            transform: perspective(500px) rotateX(60deg);
            animation: gridMove 20s linear infinite;
            z-index: 0;
        }

        @keyframes gridMove {
            0% { transform: perspective(500px) rotateX(60deg) translateY(0); }
            100% { transform: perspective(500px) rotateX(60deg) translateY(40px); }
        }

        .ambient-glow {
            position: absolute;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(157, 0, 255, 0.15) 0%, rgba(0, 0, 0, 0) 70%);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1;
            pointer-events: none;
        }

        /* Login Container */
        .login-wrapper {
            position: relative;
            z-index: 2;
            width: 100%;
            max-width: 440px;
            padding: 20px;
        }

        .login-panel {
            background: var(--panel-bg);
            border: 1px solid var(--border-glow);
            backdrop-filter: blur(16px);
            padding: 40px 30px;
            position: relative;
            clip-path: polygon(
                0 0, 
                calc(100% - 20px) 0, 
                100% 20px, 
                100% 100%, 
                20px 100%, 
                0 calc(100% - 20px)
            );
            box-shadow: 0 0 30px rgba(0, 240, 255, 0.1), inset 0 0 15px rgba(0, 240, 255, 0.05);
        }

        .login-panel::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, transparent 50%, var(--neon-cyan) 50%);
            opacity: 0.8;
        }

        /* Header */
        .brand-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .brand-header h1 {
            font-family: 'Orbitron', sans-serif;
            font-size: 1.8rem;
            font-weight: 900;
            letter-spacing: 2px;
            color: #fff;
            text-shadow: 0 0 10px rgba(0, 240, 255, 0.5);
            text-transform: uppercase;
        }

        .brand-header p {
            color: var(--text-muted);
            font-size: 0.95rem;
            letter-spacing: 1px;
            margin-top: 5px;
            text-transform: uppercase;
        }

        /* Mode Switcher Tabs */
        .auth-tabs {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 25px;
            background: rgba(0, 0, 0, 0.3);
            padding: 4px;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        .tab-btn {
            background: transparent;
            border: none;
            color: var(--text-muted);
            font-family: 'Orbitron', sans-serif;
            font-size: 0.75rem;
            font-weight: 700;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .tab-btn.active {
            background: rgba(0, 240, 255, 0.15);
            color: var(--neon-cyan);
            box-shadow: inset 0 0 10px rgba(0, 240, 255, 0.2);
        }

        /* Social Auth (Google) */
        .social-auth {
            margin-bottom: 25px;
        }

        .google-btn {
            width: 100%;
            background: #ffffff;
            color: #1a1a1a;
            border: none;
            padding: 12px;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.85rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.2);
        }

        .google-btn:hover {
            background: #f0f0f0;
            box-shadow: 0 0 25px rgba(255, 255, 255, 0.4);
            transform: translateY(-1px);
        }

        .google-btn i {
            font-size: 1.1rem;
            color: #ea4335;
        }

        /* Divider */
        .divider {
            display: flex;
            align-items: center;
            text-align: center;
            color: var(--text-muted);
            font-size: 0.8rem;
            font-family: 'Orbitron', sans-serif;
            letter-spacing: 2px;
            margin-bottom: 25px;
            text-transform: uppercase;
        }

        .divider::before, .divider::after {
            content: '';
            flex: 1;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .divider::before { margin-right: 15px; }
        .divider::after { margin-left: 15px; }

        /* Form Controls */
        .input-group {
            margin-bottom: 20px;
            position: relative;
        }

        .input-group label {
            display: block;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.7rem;
            letter-spacing: 1px;
            color: var(--text-muted);
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-wrapper i {
            position: absolute;
            left: 15px;
            color: var(--text-muted);
            font-size: 0.9rem;
            transition: color 0.3s ease;
        }

        .input-field {
            width: 100%;
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 12px 15px 12px 42px;
            color: #fff;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            outline: none;
        }

        .input-field:focus {
            border-color: var(--neon-cyan);
            background: rgba(0, 240, 255, 0.03);
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.15);
        }

        .input-field:focus + i, .input-wrapper:focus-within i {
            color: var(--neon-cyan);
        }

        .toggle-password {
            position: absolute;
            right: 15px;
            cursor: pointer;
            color: var(--text-muted);
            transition: color 0.3s ease;
        }

        .toggle-password:hover {
            color: var(--text-main);
        }

        /* Action Button */
        .action-btn {
            width: 100%;
            background: linear-gradient(135deg, var(--neon-cyan), var(--neon-purple));
            color: #fff;
            border: none;
            padding: 14px;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.9rem;
            font-weight: 700;
            letter-spacing: 2px;
            cursor: pointer;
            text-transform: uppercase;
            transition: all 0.3s ease;
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
            margin-top: 10px;
        }

        .action-btn:hover {
            background: linear-gradient(135deg, #00c2cc, #8200d6);
            box-shadow: 0 0 30px rgba(0, 240, 255, 0.5);
            transform: translateY(-1px);
        }

        /* Status Toast Notification */
        .toast {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: rgba(13, 15, 28, 0.95);
            border: 1px solid var(--neon-cyan);
            color: #fff;
            padding: 12px 24px;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.8rem;
            letter-spacing: 1px;
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.3);
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 10;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .toast.show {
            transform: translateX(-50%) translateY(0);
        }

        .toast i {
            color: var(--neon-cyan);
        }
    </style>
</head>
<body>

    <div class="cyber-grid"></div>
    <div class="ambient-glow"></div>

    <div class="login-wrapper">
        <div class="login-panel">
            <div class="brand-header">
                <h1>NEXUS-V</h1>
                <p id="panel-subtitle">Access Terminal v4.2</p>
            </div>

            <!-- Tabs -->
            <div class="auth-tabs">
                <button class="tab-btn active" id="signin-tab" onclick="switchMode('signin')">Sign In</button>
                <button class="tab-btn" id="signup-tab" onclick="switchMode('signup')">Create Account</button>
            </div>

            <!-- Google Login Option -->
            <div class="social-auth">
                <button class="google-btn" onclick="handleGoogleLogin()">
                    <i class="fab fa-google"></i> Continue with Google
                </button>
            </div>

            <div class="divider">OR GAME ID</div>

            <!-- Account Form -->
            <form id="auth-form" onsubmit="handleFormSubmit(event)">
                <div class="input-group">
                    <label>Player Handle / Email</label>
                    <div class="input-wrapper">
                        <input type="text" class="input-field" id="username" placeholder="Enter handle or email..." required>
                        <i class="fas fa-user"></i>
                    </div>
                </div>

                <div class="input-group">
                    <label>Password</label>
                    <div class="input-wrapper">
                        <input type="password" class="input-field" id="password" placeholder="••••••••••••" required>
                        <i class="fas fa-lock"></i>
                        <i class="fas fa-eye toggle-password" onclick="togglePasswordVisibility()"></i>
                    </div>
                </div>

                <button type="submit" class="action-btn" id="submit-btn">Initialize Login</button>
            </form>
        </div>
    </div>

    <!-- Toast Notification -->
    <div class="toast" id="notification-toast">
        <i class="fas fa-terminal"></i>
        <span id="toast-message">Authenticating credentials...</span>
    </div>

    <script>
        let currentMode = 'signin';

        function switchMode(mode) {
            currentMode = mode;
            const signinTab = document.getElementById('signin-tab');
            const signupTab = document.getElementById('signup-tab');
            const submitBtn = document.getElementById('submit-btn');
            const subtitle = document.getElementById('panel-subtitle');

            if (mode === 'signin') {
                signinTab.classList.add('active');
                signupTab.classList.remove('active');
                submitBtn.innerText = 'Initialize Login';
                subtitle.innerText = 'Access Terminal v4.2';
                showToast('Switched to Sign In mode');
            } else {
                signupTab.classList.add('active');
                signinTab.classList.remove('active');
                submitBtn.innerText = 'Register Account';
                subtitle.innerText = 'New Operative Registration';
                showToast('Switched to Registration mode');
            }
        }

        function togglePasswordVisibility() {
            const passwordInput = document.getElementById('password');
            const icon = document.querySelector('.toggle-password');
            if (passwordInput.type === 'password') {
                passwordInput.type = 'text';
                icon.classList.remove('fa-eye');
                icon.classList.add('fa-eye-slash');
            } else {
                passwordInput.type = 'password';
                icon.classList.remove('fa-eye-slash');
                icon.classList.add('fa-eye');
            }
        }

        function handleGoogleLogin() {
            showToast('Connecting to Google OAuth Gateway...');
            setTimeout(() => {
                showToast('Google Authentication successful! Launching...');
            }, 1500);
        }

        function handleFormSubmit(event) {
            event.preventDefault();
            const user = document.getElementById('username').value;
            if (currentMode === 'signin') {
                showToast(`Authenticating player: ${user}...`);
                setTimeout(() => {
                    showToast('Access Granted. Welcome back, Operative.');
                }, 1500);
            } else {
                showToast(`Creating account for: ${user}...`);
                setTimeout(() => {
                    showToast('Account registered successfully!');
                }, 1500);
            }
        }

        function showToast(message) {
            const toast = document.getElementById('notification-toast');
            const msgSpan = document.getElementById('toast-message');
            msgSpan.innerText = message;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }
    </script>
</body>
</html>
