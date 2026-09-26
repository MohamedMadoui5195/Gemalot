<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gemalot</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background-color: #0b0f19;
            color: #ffffff;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow: hidden;
            transition: background 0.5s ease;
        }

        /* تأثير لمعان الشاشة مثل جيميناي عند الإرسال */
        body.gemini-glow::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, rgba(59, 130, 246, 0.25), rgba(16, 185, 129, 0.2), rgba(245, 158, 11, 0.15), rgba(236, 72, 153, 0.2), transparent 80%);
            animation: geminiPulse 1.8s ease-in-out infinite;
            z-index: 1;
            pointer-events: none;
        }

        @keyframes geminiPulse {
            0% { opacity: 0.3; transform: scale(0.95); }
            50% { opacity: 1; transform: scale(1.05); filter: hue-rotate(20deg); }
            100% { opacity: 0.3; transform: scale(0.95); }
        }

        .header, .main-content, .footer {
            position: relative;
            z-index: 2;
        }

        .header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #1f2937;
        }

        .app-title {
            font-size: 24px;
            font-weight: bold;
            /* ألوان الصورة الخاصة بك بدقة */
            background: linear-gradient(135deg, #2b7de9, #12bc8e, #e8b31a, #ea580c, #db2777);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .back-btn {
            background: linear-gradient(135deg, #2b7de9, #ea580c);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
            text-decoration: none;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        .main-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            width: 100%;
            max-width: 600px;
            margin: auto;
            gap: 15px;
        }

        .main-heading {
            font-size: 28px;
            font-weight: 600;
            color: #f3f4f6;
        }

        .sub-text {
            font-size: 16px;
            color: #9ca3af;
            margin-bottom: 20px;
        }

        .chat-container {
            width: 100%;
            background-color: #111827;
            border: 1px solid #374151;
            border-radius: 30px;
            display: flex;
            align-items: center;
            padding: 8px 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.6);
            transition: border-color 0.3s;
        }

        .chat-container:focus-within {
            border-color: #12bc8e;
        }

        .chat-input {
            flex: 1;
            background: transparent;
            border: none;
            color: white;
            font-size: 16px;
            padding: 10px;
            outline: none;
            text-align: right;
        }

        .chat-input::placeholder {
            color: #6b7280;
        }

        .send-btn {
            /* ألوان التدرج المستخرجة من صورتك */
            background: linear-gradient(135deg, #2b7de9, #12bc8e, #e8b31a, #ea580c, #db2777);
            color: white;
            border: none;
            padding: 10px 22px;
            border-radius: 25px;
            font-size: 15px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
        }

        .send-btn:active {
            transform: scale(0.95);
        }

        .footer {
            font-size: 12px;
            color: #4b5563;
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <div class="header">
        <a href="#" class="back-btn">العودة ⬅</a>
        <div class="app-title">Gemalot</div>
    </div>

    <div class="main-content">
        <h1 class="main-heading">Gemalot</h1>
        <p class="sub-text">كيف يمكنك مساعدةتك؟</p>

        <div class="chat-container">
            <button class="send-btn" id="sendBtn">إرسال</button>
            <input type="text" class="chat-input" id="chatInput" placeholder="اكتب سؤالك هنا...">
        </div>
    </div>

    <div class="footer">
        Gemalot &copy; 2026
    </div>

    <script>
        const sendBtn = document.getElementById('sendBtn');
        const chatInput = document.getElementById('chatInput');
        const body = document.body;

        sendBtn.addEventListener('click', () => {
            if (chatInput.value.trim() !== "" || true) { // مفعل حتى لو الحقل فارغ للتجربة
                // تفعيل تأثير اللمعان المتحرك مثل جيميناي
                body.classList.add('gemini-glow');
                
                // إيقاف اللمعان تلقائياً بعد 4 ثوانٍ (أو يمكنك جعلها مستمرة حتى يتلقى رداً)
                setTimeout(() => {
                    body.classList.remove('gemini-glow');
                }, 4000);

                // تفريغ الحقل بعد الإرسال
                chatInput.value = "";
            }
        });
    </script>

</body>
</html>
