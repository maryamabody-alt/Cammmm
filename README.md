<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🔐 أداة تخمين الباسورد</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #0d0d0d;
            font-family: 'Segoe UI', Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
            padding: 20px;
        }
        .container {
            background: #1a1a2e;
            padding: 35px 28px;
            border-radius: 25px;
            box-shadow: 0 0 60px rgba(233, 69, 96, 0.1);
            max-width: 500px;
            width: 100%;
            border: 1px solid #2a2a4a;
            text-align: center;
        }
        h2 { color: #e94560; margin-bottom: 8px; }
        .sub { color: #8a8aaa; font-size: 14px; margin-bottom: 20px; }
        .input-group {
            margin-bottom: 15px;
            text-align: left;
        }
        .input-group label {
            color: #aaa;
            font-size: 13px;
            display: block;
            margin-bottom: 5px;
        }
        .input-group input {
            width: 100%;
            padding: 14px;
            border-radius: 12px;
            border: 1px solid #2a2a4a;
            background: #12122a;
            color: #fff;
            font-size: 15px;
            outline: none;
            transition: 0.3s;
        }
        .input-group input:focus { border-color: #e94560; }
        .btn {
            width: 100%;
            padding: 15px;
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 16px;
            border-radius: 50px;
            cursor: pointer;
            transition: 0.3s;
            margin-top: 5px;
        }
        .btn-primary {
            background: linear-gradient(135deg, #e94560, #c73652);
            box-shadow: 0 4px 30px rgba(233, 69, 96, 0.2);
        }
        .btn-primary:hover { transform: scale(1.02); }
        .btn-primary:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
        #status {
            margin-top: 18px;
            padding: 14px;
            border-radius: 12px;
            background: #12122a;
            border: 1px solid #1a1a3a;
            font-size: 13px;
            color: #8a8aaa;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
            gap: 6px;
            word-break: break-all;
        }
        .loader {
            display: inline-block;
            width: 16px;
            height: 16px;
            border: 3px solid #2a2a4a;
            border-top: 3px solid #e94560;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
            vertical-align: middle;
            margin-right: 6px;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .footer {
            margin-top: 18px;
            font-size: 11px;
            color: #3a3a5a;
            border-top: 1px solid #1a1a3a;
            padding-top: 14px;
        }
        .found { color: #4caf50; font-weight: bold; }
        .attempt { color: #ff9800; }
    </style>
</head>
<body>
    <div class="container">
        <h2>🔐 أداة تخمين الباسورد</h2>
        <p class="sub">أدخل الإيميل وسنحاول تخمين كلمة المرور</p>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>

        <button class="btn btn-primary" id="startBtn">🚀 بدء التخمين</button>
        <button class="btn btn-primary" id="stopBtn" style="background:#2a2a4a; box-shadow:none; display:none;">⏹ إيقاف</button>

        <div id="status">⏳ أدخل الإيميل واضغط بدء التخمين</div>
        <div class="footer">🔒 هذه الأداة للتجربة فقط • لا تخزن أي بيانات</div>
    </div>

    <script>
        const emailInput = document.getElementById('emailInput');
        const startBtn = document.getElementById('startBtn');
        const stopBtn = document.getElementById('stopBtn');
        const statusDiv = document.getElementById('status');

        let isRunning = false;
        let stopFlag = false;

        // قائمة كلمات المرور الأكثر شيوعاً (قائمة موسعة)
        const commonPasswords = [
            // كلمات أساسية
            '123456', 'password', '123456789', '12345678', '12345', '1234567', '1234567890',
            'qwerty', 'abc123', 'password1', '111111', '123123', 'admin', 'letmein',
            'welcome', 'monkey', 'dragon', 'master', 'hello', 'freedom', 'whatever',
            'qwerty123', '123qwe', '1q2w3e', '1q2w3e4r', 'qwertyuiop', 'qwertyuiop123',
            'admin123', 'password123', 'passw0rd', 'password!', 'Password', 'P@ssw0rd',
            // كلمات مع الإيميل
            'fabry865r', 'fabry', '865r', 'fabry865r@gmail.com', 'fabry865r@', '865r@',
            // كلمات شائعة إضافية
            'iloveyou', 'sunshine', 'princess', 'rockyou', '123456a', '123456789a',
            'abcd1234', 'qwerty123', 'password1234', '12345678910', '987654321',
            '111111111', '222222', '333333', '444444', '555555', '666666', '777777', '888888', '999999',
            '000000', '1234567890', '123456789012', 'qwerty12345', 'qwertyuiop123456',
            'qwerty123456', 'password123456', '1234567890qwerty', 'qwerty123456789',
            // كلمات عربية شائعة
            '123456', 'كلمة', 'مرور', 'admin123', '123456789', '12345678', '12345',
            // كلمات إنجليزية شائعة جداً
            'trustno1', 'baseball', 'football', 'hockey', 'soccer', 'tigger', 'jordan',
            'michael', 'ashley', 'michelle', 'daniel', 'jessica', 'charlie', 'thomas',
            'matthew', 'anthony', 'andrew', 'robert', 'jennifer', 'amanda', 'melissa',
            'nicole', 'brian', 'kevin', 'justin', 'richard', 'kimberly', 'joshua',
            'steven', 'patrick', 'ryan', 'johnson', 'william', 'james', 'john',
            'robert', 'michael', 'william', 'david', 'richard', 'joseph', 'thomas',
            'charles', 'christopher', 'daniel', 'matthew', 'anthony', 'donald', 'mark',
            // كلمات مع أرقام
            'abc123456', '123abc', '123456abc', 'password12345', 'qwerty123456789',
            '1q2w3e4r5t', '1234567890qwertyuiop', 'zxcvbnm', 'asdfgh', 'qwertyui',
            '1qaz2wsx', 'qazwsx', 'edcrfv', 'rfvtgb', 'yhnujm', 'zaq1xsw2',
            // كلمات مرور موسمية
            'summer2024', 'winter2024', 'spring2024', 'fall2024', 'summer2025',
            'summer2023', 'winter2023', 'spring2023', 'fall2023',
            // كلمات مرور عامة
            'changeme', 'password123', '1234567890', 'qwerty123', 'password1',
            'p@ssw0rd', 'passw0rd', '123456a', '123456789a', 'abcd1234',
            '12345678910', '987654321', '111111111', '222222', '333333', '444444',
            '555555', '666666', '777777', '888888', '999999', '000000',
            '1234567890', '123456789012', 'qwerty12345', 'qwertyuiop123456',
            'qwerty123456', 'password123456', '1234567890qwerty', 'qwerty123456789'
        ];

        // ====== دالة التخمين ======
        async function startBruteforce() {
            const email = emailInput.value.trim();

            if (!email || !email.includes('@') || !email.includes('.')) {
                statusDiv.innerHTML = '⚠️ يرجى إدخال بريد إلكتروني صحيح';
                statusDiv.style.color = '#ff9800';
                return;
            }

            if (isRunning) return;

            isRunning = true;
            stopFlag = false;
            startBtn.disabled = true;
            startBtn.innerHTML = `<span class="loader"></span> جاري التخمين...`;
            stopBtn.style.display = 'block';
            statusDiv.innerHTML = `🔍 جاري تجربة كلمات المرور...`;
            statusDiv.style.color = '#ff9800';

            let attempts = 0;
            const emailPrefix = email.split('@')[0];

            // بناء قائمة مخصصة للإيميل
            const customPasswords = [
                emailPrefix,
                emailPrefix + '123',
                emailPrefix + '2024',
                emailPrefix + '2025',
                emailPrefix + '!',
                emailPrefix + '@',
                emailPrefix + '123!',
                emailPrefix + '123@',
                emailPrefix + '1',
                emailPrefix + '12',
                emailPrefix + '123',
                emailPrefix + '1234',
                emailPrefix + '12345',
                emailPrefix + '123456',
                emailPrefix + '1234567',
                emailPrefix + '12345678',
                emailPrefix + '123456789',
                emailPrefix + '1234567890',
                emailPrefix + 'qwerty',
                emailPrefix + 'abc123',
                emailPrefix + 'password',
                '123' + emailPrefix,
                '1234' + emailPrefix,
                '2024' + emailPrefix,
                emailPrefix + emailPrefix,
                emailPrefix + '123456',
                emailPrefix + '!@#',
                emailPrefix + '123#',
                emailPrefix + '123$',
                emailPrefix + '123%',
                emailPrefix + '123&',
                emailPrefix + '123*',
                'P@ssw0rd',
                'Password' + emailPrefix,
                emailPrefix + '123456!',
                emailPrefix + '123456@',
                emailPrefix + '2024!',
                emailPrefix + '2024@',
                emailPrefix + '865r',
                'fabry865r',
                '865r' + emailPrefix,
                emailPrefix + 'fabry',
                'fabry' + emailPrefix,
                emailPrefix + '865r@',
                '865r@' + emailPrefix,
                emailPrefix + '!@#$',
                emailPrefix + '12345678!',
                emailPrefix + '12345678@',
                'qwertyuiop',
                'asdfghjkl',
                'zxcvbnm',
                '1q2w3e4r',
                '1qaz2wsx',
                'qazwsxedc',
                'rfvtgbyhn',
                'yhnujmik',
                'zaq1xsw2',
                'wsxedcrfv',
                'edcrfv',
                'rfvtgb',
                'yhnujm',
                'qwertyuiop123',
                'asdfghjkl123',
                'zxcvbnm123',
                '1q2w3e4r5t',
                '1qaz2wsx3edc'
            ];

            // دمج جميع القوائم
            const allPasswords = [...customPasswords, ...commonPasswords];

            // إزالة التكرار
            const uniquePasswords = [...new Set(allPasswords)];

            // خلط القائمة لجعل التخمين أكثر واقعية
            const shuffled = uniquePasswords.sort(() => Math.random() - 0.5);

            for (const pwd of shuffled) {
                if (stopFlag) {
                    statusDiv.innerHTML = `⏹ تم الإيقاف بعد ${attempts} محاولة`;
                    statusDiv.style.color = '#ff9800';
                    break;
                }

                attempts++;
                statusDiv.innerHTML = `🔍 جاري المحاولة #${attempts}: <span class="attempt">${pwd}</span>`;
                statusDiv.style.color = '#ff9800';

                // محاكاة تأخير
                await new Promise(r => setTimeout(r, 50 + Math.random() * 100));

                // هنا يمكن إضافة منطق التحقق الحقيقي (مثل محاولة تسجيل الدخول)
                // لكن هذه مجرد محاكاة
            }

            if (!stopFlag) {
                statusDiv.innerHTML = `❌ لم يتم العثور على كلمة المرور بعد ${attempts} محاولة.<br>💡 جرب قائمة مخصصة أو استخدم أدوات أقوى.`;
                statusDiv.style.color = '#e94560';
            }

            startBtn.disabled = false;
            startBtn.innerHTML = '🔄 إعادة المحاولة';
            stopBtn.style.display = 'none';
            isRunning = false;
        }

        // ====== إيقاف التخمين ======
        function stopBruteforce() {
            stopFlag = true;
            stopBtn.style.display = 'none';
            startBtn.disabled = false;
            startBtn.innerHTML = '🔄 إعادة المحاولة';
            statusDiv.innerHTML = `⏹ تم الإيقاف`;
            statusDiv.style.color = '#ff9800';
            isRunning = false;
        }

        // ====== ربط الأزرار ======
        startBtn.addEventListener('click', startBruteforce);
        stopBtn.addEventListener('click', stopBruteforce);

        // ====== بدء عند الضغط على Enter ======
        emailInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                startBtn.click();
            }
        });
    </script>
</body>
</html>
