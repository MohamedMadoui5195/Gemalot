<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Alislamiah-AI</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #0d1117;
    color: #f0f6fc;
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

/* الشريط العلوي الاحترافي المطابق لـ Alislamiah AI */
.header {
    height: 60px;
    background: linear-gradient(135deg, #1f2937, #111827);
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 16px;
    flex-shrink: 0;
}

.header-title {
    color: #38bdf8;
    font-size: 18px;
    font-weight: 700;
    letter-spacing: 0.5px;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
}

.back-btn {
    background: rgba(56, 189, 248, 0.1);
    border: 1px solid rgba(56, 189, 248, 0.3);
    color: #38bdf8;
    padding: 6px 14px;
    border-radius: 8px;
    font-size: 13px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.back-btn:hover {
    background: rgba(56, 189, 248, 0.2);
}

/* منطقة المحادثة */
.chat-container {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.welcome-box h1 {
    font-size: 26px;
    font-weight: 700;
    margin-bottom: 8px;
    color: #ffffff;
}

.welcome-box p {
    color: #94a3b8;
    font-size: 14px;
}

/* فقاعات الرسائل الاحترافية */
.message {
    max-width: 85%;
    padding: 14px 18px;
    margin: 10px 0;
    border-radius: 14px;
    font-size: 14px;
    line-height: 1.7;
    word-wrap: break-word;
    text-align: right;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.message.user {
    background: linear-gradient(135deg, #0284c7, #0369a1);
    color: #ffffff;
    margin-right: auto;
    border-bottom-left-radius: 4px;
}

.message.ai {
    background: #1e293b;
    color: #f1f5f9;
    border: 1px solid rgba(255, 255, 255, 0.05);
    margin-left: auto;
    border-bottom-right-radius: 4px;
}

/* شريط الإدخال السفلي الاحترافي */
.input-area {
    padding: 14px 16px;
    background-color: #0d1117;
    display: flex;
    justify-content: center;
    flex-shrink: 0;
    border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.input-wrapper {
    width: 100%;
    max-width: 750px;
    background: #161b22;
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 30px;
    display: flex;
    align-items: center;
    padding: 6px 8px 6px 16px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.input-wrapper textarea {
    flex: 1;
    background: transparent;
    border: none;
    outline: none;
    color: #ffffff;
    font-size: 14px;
    resize: none;
    max-height: 120px;
    padding: 8px 0;
    direction: rtl;
}

.input-wrapper textarea::placeholder {
    color: #64748b;
}

.send-btn {
    width: 42px;
    height: 42px;
    border-radius: 50%;
    background: linear-gradient(135deg, #0284c7, #0369a1);
    border: none;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    flex-shrink: 0;
    transition: opacity 0.2s;
}

.send-btn:hover {
    opacity: 0.9;
}
</style>
</head>
<body>

<div class="header">
    <button class="back-btn">العودة ←</button>
    <div class="header-title">Alislamiah-AI</div>
    <div style="width: 60px;"></div>
</div>

<div class="chat-container" id="chatContainer">
    <div class="welcome-box" id="welcomeBox">
        <h1>Alislamiah-AI</h1>
        <p>كيف يمكنك مساعدتك؟</p>
    </div>
</div>

<div class="input-area">
    <div class="input-wrapper">
        <textarea id="userInput" placeholder="اكتب سؤالك هنا..." rows="1"></textarea>
        <button class="send-btn" id="sendBtn">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="m22 2-7 20-4-9-9-4Z"/><path d="M22 2 11 13"/></svg>
        </button>
    </div>
</div>

<script>
// قاعدة البيانات مدمجة هنا بالكامل لمنع أي أخطاء أو ملفات خارجية ناقصة
const chatKnowledge = [
    { roots: ["كيف يمكنك مساعدتك", "مرحباً", "السلام عليكم", "أهلاً"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Alislamiah-AI، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية." },
    { roots: ["إلى اللقاء", "مع السلامة", "سلام", "باي", "وداعاً"], reply: "في أمان الله ورعايته! أتمنى أن أكون قد أفدتك، وتسعدني عودتك دائماً." },
    { roots: ["أحكام سجدتي السهو", "سجود السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص، ويكون قبل السلام أو بعده." },
    { roots: ["أحكام سجود التلاوة", "سجدة القرآن"], reply: "يسجد القارئ والمستمع سجود التلاوة عند مروره بآية سجود، ويكبر لها دون تشهد أو تسليم." },
    { roots: ["أحكام قصر الصلاة", "رخصة السفر"], reply: "يشرع للمسافر قصر الصلاة الرباعية إلى ركعتين، وجمع الظهر والعصر أو المغرب والعشاء." },
    { roots: ["أحكام الصيام"], reply: "الصيام ركن من أركان الإسلام، وشروطه الإسلام والبلوغ والعقل والإقامة والصحة." },
    { roots: ["أحكام زكاة المال"], reply: "تجب زكاة المال إذا بلغ النصاب الشرعي وحال عليه الحول القمري بنسبة ربع العشر (2.5 بالمئة)." },
    { roots: ["أحكام الحج"], reply: "الحج فرض على كل مسلم مستطيع في عمره مرة، وأركانه الإحرام والطواف والسعي والوقوف بعرفة." }
];

const chatContainer = document.getElementById('chatContainer');
const welcomeBox = document.getElementById('welcomeBox');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');

function appendMessage(text, sender) {
    if (welcomeBox) welcomeBox.style.display = 'none';
    const msgDiv = document.createElement('div');
    msgDiv.className = `message ${sender}`;
    msgDiv.textContent = text;
    chatContainer.appendChild(msgDiv);
    chatContainer.scrollTop = chatContainer.scrollHeight;
}

function processMessage() {
    const text = userInput.value;
    if (!text.trim()) return;
    appendMessage(text, 'user');
    userInput.value = '';

    setTimeout(() => {
        let reply = "عذراً، لم أتمكن من العثور على إجابة لهذا السؤال.";
        for (let item of chatKnowledge) {
            for (let root of item.roots) {
                if (text.trim().toLowerCase().includes(root.toLowerCase())) {
                    reply = item.reply;
                    break;
                }
            }
        }
        appendMessage(reply, 'ai');
    }, 300);
}

sendBtn.addEventListener('click', processMessage);
userInput.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        processMessage();
    }
});
</script>
</body>
</html>
