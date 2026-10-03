 <!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
    background-color: #0b0f19;
    color: #ffffff;
    font-family: Arial, sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

.header {
    height: 65px;
    background: linear-gradient(90deg, #facc15, #f97316, #22c55e, #3b82f6);
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding: 0 20px;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 1000;
}

.header-title {
    color: #ffffff;
    font-size: 22px;
    font-weight: bold;
}

.chat-container {
    flex: 1;
    width: 100%;
    max-width: 800px;
    margin: 65px auto 80px auto;
    padding: 20px;
    display: flex;
    flex-direction: column;
    overflow-y: visible;
}

.welcome-box {
    text-align: center;
    margin: auto;
    padding: 40px 0;
}

.welcome-box h1 {
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 6px;
    color: #ffffff;
}

.welcome-box p {
    color: #9ca3af;
    font-size: 13px;
}

.message {
    max-width: 85%;
    padding: 12px 16px;
    margin: 8px 0;
    border-radius: 16px;
    font-size: 14px;
    line-height: 1.6;
    word-wrap: break-word;
    text-align: right;
}

.message.user {
    background: linear-gradient(90deg, #facc15, #f97316, #22c55e, #3b82f6);
    color: #ffffff;
    font-weight: bold;
    margin-right: auto;
    border-bottom-left-radius: 4px;
}

.message.ai {
    background: #ffffff;
    color: #111827;
    margin-left: auto;
    border-bottom-right-radius: 4px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.input-area {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 12px 16px;
    background-color: #0b0f19;
    display: flex;
    justify-content: center;
    z-index: 1000;
}

.input-wrapper {
    width: 100%;
    max-width: 700px;
    background: #ffffff;
    border-radius: 35px;
    display: flex;
    align-items: center;
    padding: 4px 6px 4px 14px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

.input-wrapper textarea {
    flex: 1;
    background: transparent;
    border: none;
    outline: none;
    color: #111827;
    font-size: 14px;
    resize: none;
    max-height: 100px;
    padding: 8px 0;
    direction: rtl;
}

.input-wrapper textarea::placeholder {
    color: #9ca3af;
}

.send-btn {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: linear-gradient(90deg, #facc15, #f97316, #22c55e);
    border: none;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    flex-shrink: 0;
    font-size: 13px;
    font-weight: bold;
}

/* ===== توهج الواجهة كلها - ألوان أوضح وأقوى ===== */
.full-glow {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
    opacity: 0;
    background: linear-gradient(
        120deg,
        #ffeb3b 0%,
        #ff9800 16%,
        #f44336 33%,
        #9c27b0 50%,
        #2196f3 66%,
        #4caf50 83%,
        #ffeb3b 100%
    );
    background-size: 400% 400%;
    mix-blend-mode: soft-light;
}

.full-glow.active {
    animation: fullGlowFlow 2s ease-in-out forwards;
}

@keyframes fullGlowFlow {
    0% {
        background-position: 0% 50%;
        opacity: 0.75;
    }
    30% {
        background-position: 50% 50%;
        opacity: 0.85;
    }
    60% {
        background-position: 100% 50%;
        opacity: 0.7;
    }
    100% {
        background-position: 0% 50%;
        opacity: 0;
    }
}
</style>
</head>
<body>

<!-- طبقة التوهج -->
<div class="full-glow" id="fullGlow"></div>

<div class="header">
    <div class="header-title">Gemalot</div>
</div>

<div class="chat-container" id="chatContainer">
    <div class="welcome-box" id="welcomeBox">
        <h1>Gemalot</h1>
        <p>كيف يمكنك مساعدتك؟</p>
    </div>
</div>

<div class="input-area">
    <div class="input-wrapper">
        <textarea id="userInput" placeholder="اكتب سؤالك هنا..." rows="1"></textarea>
        <button class="send-btn" id="sendBtn">إرسال</button>
    </div>
</div>

<script>
const chatKnowledge = [
    { roots: ["كيف يمكنك مساعدتك", "مرحباً", "السلام عليكم", "أهلاً", "مرحبا", "هلا", "السلآم"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Gemalot، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية." },
    { roots: ["الوطن", "حب الوطن", "تعبير عن حب الوطن", "تعبير عن الوطن", "أهمية الوطن", "واجبنا نحو الوطن"], reply: "الوطن ليس مجرد أرض نعيش فوقها، بل هو تاريخنا وذكرياتنا وهويتنا التي نفخر بها بين الأمم. فيه ولدنا، وعلى أرضه نشأنا، ومن خيراته نعيش ونحلم بمستقبل أفضل. لذلك فإن حب الوطن شعور فطري يسكن القلوب، ويجعل الإنسان متعلقًا بأرضه ومخلصًا لها مهما ابتعد عنها.\n\nيتجلى حب الوطن في المحافظة على ممتلكاته العامة، واحترام قوانينه، والعمل بجدٍ وإخلاص من أجل رفعته وتقدمه. فالمواطن الصالح لا يكتفي بالكلام عن حب وطنه، بل يترجمه إلى أفعال نافعة تساهم في تطوره وازدهاره. كما أن طلب العلم، ونشر الأخلاق الحسنة، ومساعدة الآخرين، كلها صور مشرقة من صور حب الوطن. وعندما يتعاون أبناء الوطن ويتحلون بروح المسؤولية، يصبح وطنهم أقوى وأكثر تقدمًا واستقرارًا.\n\nوالوطن يستحق منا الكثير، لأنه يمنحنا الأمن والانتماء والكرامة. لذلك يجب أن نحافظ عليه، وأن ندافع عنه، وأن نسعى دائمًا إلى ترك أثر طيب يساهم في بناء مستقبله المشرق للأجيال القادمة.\n\nوفي الختام، يبقى حب الوطن من أنبل المشاعر وأعظم القيم الإنسانية، فهو مصدر العزة والفخر لكل إنسان. ومن واجبنا أن نحافظ على وطننا ونخدمه بكل ما نستطيع، لأن ازدهاره هو ازدهار لنا جميعًا، ورفعته دليل على إخلاص أبنائه ووفائهم له." },
    { roots: ["الوقت", "أهمية الوقت", "تعبير عن الوقت", "الوقت وأهميته", "استثمار الوقت", "استغلال الوقت"], reply: "يعد الوقت من أثمن النعم التي يمتلكها الإنسان، فهو أساس النجاح والتقدم في الحياة.\n\nتكمن أهمية الوقت في أنه يساعد على تحقيق الأهداف وإنجاز الأعمال وتنظيم شؤون الحياة. ويمكن استغلاله من خلال وضع خطة يومية، وتحديد الأولويات، والابتعاد عن الملهيات، والالتزام بالمواعيد. كما أن استثمار الوقت في التعلم والعمل النافع يعود بالفائدة على الفرد والمجتمع.\n\nوفي الختام، فإن حسن استغلال الوقت هو مفتاح النجاح، لذلك يجب المحافظة عليه وعدم إضاعته فيما لا ينفع." },
    { roots: ["إلى اللقاء", "مع السلامة", "سلام", "وداعاً", "باي"], reply: "في أمان الله ورعايته! أتمنى أن أكون قد أفدتك، وتسعدني عودتك دائماً." },
    { roots: ["الوضوء", "نواقض الوضوء", "طهارة"], reply: "نواقض الوضوء هي: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، وأكل لحم الإبل." },
    { roots: ["الغسل", "الجنب", "الجنابة"], reply: "يجب الغسل بالجنابة، الحيض، النفاس، وكيفيته تعميم الجسد بالماء مع النية." },
    { roots: ["التيمم"], reply: "يُشرع التيمم بفاقد الماء أو العاجز عنه لاستعماله بضرب الصعيد الطاهر ضربة واحدة للوجه والكفين." },
    { roots: ["الصلاة", "شروط الصلاة", "أركان الصلاة"], reply: "الصلاة عماد الدين، وشروطها الطهارة ودخول الوقت ستر العورة واستقبال القبلة، وأركانها تكبيرة الإحرام والقرآن والركوع والسجود." },
    { roots: ["سجود السهو", "سجدتي السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص، ويكون قبل السلام أو بعده." },
    { roots: ["سجود التلاوة", "سجدة القرآن"], reply: "يسجد القارئ والمستمع سجود التلاوة عند مروره بآية سجود، ويكبر لها دون تشهد أو تسليم." },
    { roots: ["سجود الشكر"], reply: "يسن سجود الشكر لله تعالى عند تجدد نعمة عظيمة أو اندفاع نقمة، وهو سجدة واحدة." },
    { roots: ["قصر الصلاة", "رخصة السفر", "صلاة المسافر"], reply: "يشرع للمسافر قصر الصلاة الرباعية إلى ركعتين، وجمع الظهر والعصر أو المغرب والعشاء، ويجوز له الفطر في رمضان." },
    { roots: ["صلاة الجمعة"], reply: "تجب صلاة الجمعة على كل مسلم بالغ عاقل مقيم، ومن تركها ثلاث جمع طبع الله على قلبه." },
    { roots: ["صلاة العيدين"], reply: "صلاة العيدين سنة مؤكدة، ويُسن فيها التكبير الزائد والخطبة بعدها لإدخال الفرح." },
    { roots: ["صلاة الاستسقاء"], reply: "تشرع صلاة الاستسقاء جماعة في المصلى عند انقطاع المطر وتأخر الغيث مع التذلل والافتقار لله." },
    { roots: ["صلاة الكسوف", "صلاة الخسوف"], reply: "تستحب صلاة الكسوف والخسوف بركعتين في كل ركعة قيامان وركوعان وسجودان." },
    { roots: ["صلاة الجنازة"], reply: "صلاة الجنازة فرض كفاية، وأركانها أربع تكبيرات تقرأ فيها الفاتحة والصلاة الإبراهيمية والدعاء للميت." },
    { roots: ["الصيام", "احكام الصيام", "رمضان"], reply: "الصيام ركن من أركان الإسلام، وشروطه الإسلام والبلوغ والعقل والإقامة والصحة." },
    { roots: ["المفطرات", "ما يبطل الصيام"], reply: "يبطل الصيام بالأكل والشرب عمداً والجماع والاستقاءة العمد ونزول دم الحيض والنفاس." },
    { roots: ["القضاء"], reply: "يجب قضاء الأيام المفطرة من رمضان قبل حلول رمضان التالي، ويجوز التفريق والتتابع." },
    { roots: ["الفدية"], reply: "من عجز عن الصيام لكبر أو مرض مزمن لزمته الفدية بإطعام مسكين عن كل يوم أفطره." },
    { roots: ["زكاة المال", "الزكاة"], reply: "تجب زكاة المال إذا بلغ النصاب الشرعي وحال عليه الحول القمري بنسبة ربع العشر (2.5 بالمئة)." },
    { roots: ["زكاة الفطر"], reply: "تجب زكاة الفطر على كل مسلم يملِك قوت يومه، ومقدارها صاع من تمر أو شعير أو طعام." },
    { roots: ["الحج", "احكام الحج والعمرة"], reply: "الحج فرض على كل مسلم مستطيع في عمره مرة، وأركانه الإحرام والطواف والسعي والوقوف بعرفة." },
    { roots: ["العمرة"], reply: "العمرة سنة مؤكدة أو واجبة في العمر مرة، وتكفر ما بينها وبين العمرة الأخرى لمن أتمها." },
    { roots: ["الإحرام"], reply: "يجب الإحرام من الميقات المحدد لمن أراد الحج أو العمرة، ومن جاوزه بلا إحرام لزمه دم." },
    { roots: ["الأضحية"], reply: "الأضحية سنة مؤكدة لمن استطاع، ويشترط فيها السلامة والسن المعتبرة في الأنعام." },
    { roots: ["البيوع", "التجارة", "البيع"], reply: "يقوم البيع على الإيجاب والقبول ورضا المتبايعين، وخلوه من الربا والغرر المحرم." },
    { roots: ["الربا"], reply: "الربا محرم تحريماً قاطعاً وهو من أكبر الكبائر، سواء كان ربا نسيئة أو ربا فضل." },
    { roots: ["الميراث"], reply: "الميراث نظام عادل يوزع التركة على أصحاب الفروض والعصبات بحسب الأنصباء القرآنية." },
    { roots: ["الوصية"], reply: "تجوز الوصية لغير الوارث بحدود ثلث التركة فقط، ولا وصية لوارث إلا بإجازة الورثة." },
    { roots: ["النكاح", "الزواج"], reply: "يقوم النكاح الصحيح على الإيجاب والقبول، ووجود الولي، والشهود، وخلو الزوجين من الموانع." },
    { roots: ["الطلاق"], reply: "الطلاق حق للزوج بيد، ويدخل في الأحكام الخمسة بحسب سببه، والعدة تحصين للرحم." },
    { roots: ["الرضاع"], reply: "يحرم من الرضاع ما يحرم من النسب بشرط خمس رضعات مشبعات في سن الحولين الأولين." },
    { roots: ["الحضانة"], reply: "الأم أحق بحضانة طفلها ما لم تتزوج بأجنبي، وحقها يسقط بالزواج أو بوجود مانع شرعي." },
    { roots: ["بر الوالدين", "صلة الرحم"], reply: "بر الوالدين وصلة الرحم من أعظم القربات الموجبة للجنة، وعقوقهما من أكبر الكبائر." },
    { roots: ["التوبة"], reply: "التوبة تجب ما قبلها، وشرائطها: الإقلاع عن الذنب، والندم، والعزم على عدم العودة." }
];

const chatContainer = document.getElementById('chatContainer');
const welcomeBox = document.getElementById('welcomeBox');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');
const fullGlow = document.getElementById('fullGlow');

function triggerFullGlow() {
    fullGlow.classList.remove('active');
    void fullGlow.offsetWidth;
    fullGlow.classList.add('active');
}

function appendMessage(text, sender) {
    if (welcomeBox) welcomeBox.style.display = 'none';
    const msgDiv = document.createElement('div');
    msgDiv.className = `message ${sender}`;
    chatContainer.appendChild(msgDiv);
    window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
    
    if (sender === 'ai') {
        let i = 0;
        function typeWriter() {
            if (i < text.length) {
                msgDiv.textContent += text.charAt(i);
                i++;
                window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
                setTimeout(typeWriter, 15);
            }
        }
        typeWriter();
    } else {
        msgDiv.textContent = text;
    }
}

function processMessage() {
    const text = userInput.value;
    if (!text.trim()) return;

    triggerFullGlow();

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

userInput.addEventListener('focus', triggerFullGlow);

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