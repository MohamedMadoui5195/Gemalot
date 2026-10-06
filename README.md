<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot & Alislamiah-AI</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.0/math.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column}
.header{height:56px;background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);display:flex;align-items:center;justify-content:space-between;padding:0 14px;position:fixed;top:0;left:0;width:100%;z-index:1000}
.header-title{font-size:18px;font-weight:bold;color:#fff}
.header-actions{display:flex;gap:8px;align-items:center}
.menu-btn{width:38px;height:38px;background:rgba(0,0,0,.25);border:none;border-radius:10px;cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff;font-size:16px}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}
.counter-badge{background:rgba(0,0,0,.4);padding:4px 10px;border-radius:12px;font-size:11px;color:#facc15;font-weight:bold;border:1px solid rgba(250,204,21,.3)}
.models-bar{position:fixed;top:56px;left:0;width:100%;display:flex;gap:8px;padding:10px 12px;overflow-x:auto;background:#0b0f19;border-bottom:1px solid #1f2937;z-index:950;scrollbar-width:none}
.model-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1.5px solid rgba(255,255,255,.15);background:transparent;color:#e5e7eb;font-size:11px;font-weight:600;cursor:pointer;white-space:nowrap}
.model-btn.active{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;border-color:transparent}
.sidebar{position:fixed;top:108px;left:0;width:280px;height:calc(100vh - 108px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .3s;overflow-y:auto;padding:14px}
.sidebar.open{transform:translateX(0)}.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.new-chat-btn{width:100%;padding:10px;border:none;border-radius:10px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:bold;cursor:pointer;margin-bottom:12px}
.chat-container{flex:1;width:100%;max-width:800px;margin:108px auto 180px;padding:16px;display:flex;flex-direction:column}
.welcome-box{text-align:center;margin:auto;padding:40px 0}.welcome-box h1{font-size:24px;margin-bottom:8px}.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}.welcome-box p{color:#9ca3af;font-size:13px}
.message{max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;font-size:14px;line-height:1.6;word-wrap:break-word;white-space:pre-wrap}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:4px;box-shadow:0 2px 6px rgba(0,0,0,.1)}
.message.ai img,.message.ai video{max-width:100%;border-radius:12px;margin-top:8px;display:block}
.source-tag{font-size:9px;color:#6b7280;margin-top:6px;display:block;border-top:1px solid #e5e7eb;padding-top:4px}
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 12px 14px;background:linear-gradient(to top,#0b0f19 75%,transparent);z-index:1000}
.gemini-bar{max-width:720px;margin:0 auto;display:flex;align-items:center;background:#ffffff;border-radius:32px;padding:6px 8px;gap:6px;box-shadow:0 8px 30px rgba(0,0,0,.35)}
.plus-btn{width:40px;height:40px;border-radius:50%;border:none;background:#fff;color:#111;font-size:26px;cursor:pointer;display:flex;align-items:center;justify-content:center}
.input-wrapper{flex:1;display:flex;align-items:center}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:15px;color:#111;background:transparent;max-height:90px;padding:10px 2px}
.send-btn{width:44px;height:44px;border-radius:50%;border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;font-size:18px}
.tools-popup{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);max-width:340px;width:90%;background:#111827;border:1px solid #1f2937;border-radius:18px;padding:10px;display:none;z-index:1001}
.tools-popup.open{display:block}
.tools-popup button{width:100%;text-align:right;padding:10px 12px;border-radius:10px;border:none;background:transparent;color:#e5e7eb;font-size:12px;cursor:pointer;display:flex;align-items:center;gap:8px}
.tools-popup button:hover{background:#1f2937}
</style>
</head>
<body>
<div class="header">
  <div class="header-title">Gemalot</div>
  <div class="header-actions">
    <div class="counter-badge" id="msgCounter">المتبقي: 25/25</div>
    <button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button>
  </div>
</div>

<div class="models-bar">
  <button class="model-btn active">Alislamiah-AI Pro</button>
</div>

<div id="sidebar" class="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات السابقة</h3>
  <div id="chatList"></div>
</div>

<div id="chatContainer" class="chat-container">
  <div id="welcomeBox" class="welcome-box">
    <h1>Gemalot AI</h1>
    <div class="line"></div>
    <p>وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Alislamiah-AI، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية والمزيد.</p>
  </div>
</div>

<div class="tools-popup" id="toolsPopup">
  <button onclick="triggerUpload('image');closeTools()"><span>🖼️</span> رفع صورة وتعديلها</button>
  <button onclick="triggerUpload('video');closeTools()"><span>🎥</span> رفع فيديو وتشغيله</button>
  <button onclick="askImageGen();closeTools()"><span>🎨</span> إنشاء صورة احترافية (4K)</button>
  <button onclick="askVideoGen();closeTools()"><span>🎬</span> إنشاء فيديو متحرك</button>
  <button onclick="insertPrompt('حل المعادلة: ');closeTools()"><span>🧮</span> حل معادلة رياضية</button>
</div>

<div class="input-area">
  <div class="gemini-bar">
    <button class="plus-btn" id="plusBtn" onclick="toggleTools()">+</button>
    <div class="input-wrapper">
      <textarea id="userInput" placeholder="اسأل Alislamiah-AI..." rows="1"></textarea>
    </div>
    <button id="sendBtn" class="send-btn" onclick="processMessage()">↑</button>
  </div>
</div>

<input type="file" id="fileInput" hidden accept="image/*,video/*">

<script>
// ==================== نظام تقييد الرسائل والانتظار ====================
const DAILY_LIMIT = 25;
const COOLDOWN_HOURS = 48;

function getTodayKey() {
  const d = new Date();
  return `${d.getFullYear()}-${d.getMonth() + 1}-${d.getDate()}`;
}

function initLimits() {
  const lastDate = localStorage.getItem('gemalot_last_date');
  const today = getTodayKey();
  const cooldownUntil = localStorage.getItem('gemalot_cooldown_until');

  // إذا دخل يوم جديد، يتم تصفير العداد وتجديد الرسائل تلقائياً
  if (lastDate !== today) {
    if (!cooldownUntil || Date.now() >= parseInt(cooldownUntil)) {
      localStorage.setItem('gemalot_last_date', today);
      localStorage.setItem('gemalot_msg_count', '0');
      localStorage.removeItem('gemalot_cooldown_until');
    }
  }

  updateCounterUI();
}

function checkLimits() {
  const cooldownUntil = localStorage.getItem('gemalot_cooldown_until');
  if (cooldownUntil) {
    const timeLeft = parseInt(cooldownUntil) - Date.now();
    if (timeLeft > 0) {
      const hours = Math.floor(timeLeft / (1000 * 60 * 60));
      const minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
      alert(`⚠️ انتهى حد الرسائل اليومي!\nتم تفعيل فترة الانتظار (48 ساعة).\nالمتبقي لفتح المحادثة: ${hours} ساعة و ${minutes} دقيقة.`);
      return false;
    } else {
      localStorage.removeItem('gemalot_cooldown_until');
      localStorage.setItem('gemalot_last_date', getTodayKey());
      localStorage.setItem('gemalot_msg_count', '0');
    }
  }

  let count = parseInt(localStorage.getItem('gemalot_msg_count') || '0');
  if (count >= DAILY_LIMIT) {
    // تفعيل مهلة الـ 48 ساعة عند تجاوز الـ 25 رسالة
    const unlockTime = Date.now() + (COOLDOWN_HOURS * 60 * 60 * 1000);
    localStorage.setItem('gemalot_cooldown_until', unlockTime.toString());
    updateCounterUI();
    alert(`⚠️ لقد استهلكت الـ 25 رسالة المتاحة لهذا اليوم!\nسيتم تجميد الرسائل لمدة 48 ساعة.`);
    return false;
  }

  return true;
}

function incrementMsgCount() {
  let count = parseInt(localStorage.getItem('gemalot_msg_count') || '0');
  count++;
  localStorage.setItem('gemalot_msg_count', count.toString());

  if (count >= DAILY_LIMIT) {
    const unlockTime = Date.now() + (COOLDOWN_HOURS * 60 * 60 * 1000);
    localStorage.setItem('gemalot_cooldown_until', unlockTime.toString());
  }

  updateCounterUI();
}

function updateCounterUI() {
  const counterEl = document.getElementById('msgCounter');
  const userInput = document.getElementById('userInput');
  const sendBtn = document.getElementById('sendBtn');
  const cooldownUntil = localStorage.getItem('gemalot_cooldown_until');

  if (cooldownUntil && Date.now() < parseInt(cooldownUntil)) {
    const timeLeft = parseInt(cooldownUntil) - Date.now();
    const hours = Math.floor(timeLeft / (1000 * 60 * 60));
    counterEl.textContent = `مغلق (انتظار ${hours}س)`;
    counterEl.style.color = '#ef4444';
    if(userInput) userInput.placeholder = "الخدمة متوقفة مؤقتاً (انتظار 48 ساعة)...";
  } else {
    let count = parseInt(localStorage.getItem('gemalot_msg_count') || '0');
    let remaining = DAILY_LIMIT - count;
    if (remaining < 0) remaining = 0;
    counterEl.textContent = `المتبقي: ${remaining}/${DAILY_LIMIT}`;
    counterEl.style.color = '#facc15';
    if(userInput) userInput.placeholder = "اسأل Alislamiah-AI...";
  }
}

// 1. قاعدة البيانات المحلية
const chatKnowledge = [
    { roots: ["كيف يمكنك مساعدتك", "مرحباً", "مرحبا", "السلام عليكم", "أهلاً", "اهلاً"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Alislamiah-AI، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية." },
    { roots: ["إلى اللقاء", "مع السلامة", "سلام", "باي", "وداعاً", "وداعا"], reply: "في أمان الله ورعايته! أتمنى أن أكون قد أفدتك، وتسعدني عودتك دائماً." },
    { roots: ["أحكام سجدتي السهو", "سجود السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص، ويكون قبل السلام أو بعده." },
    { roots: ["أحكام سجود التلاوة", "سجدة القرآن", "سجود التلاوة"], reply: "يسجد القارئ والمستمع سجود التلاوة عند مروره بآية سجود، ويكبر لها دون تشهد أو تسليم." },
    { roots: ["أحكام سجود الشكر", "سجدة الشكر لله", "سجود الشكر"], reply: "يسن سجود الشكر لله تعالى عند تجدد نعمة عظيمة أو اندفاع نقمة، وهو سجدة واحدة." },
    { roots: ["أحكام قصر الصلاة", "رخصة السفر", "السفر الصائم", "قصر الصلاة"], reply: "يشرع للمسافر قصر الصلاة الرباعية إلى ركعتين، وجمع الظهر والعصر أو المغرب والعشاء، ويجوز له الفطر في رمضان وقضاء عدد من أيام أخر." },
    { roots: ["أحكام صلاة الخوف", "كيفية صلاة الحرب", "صلاة الخوف"], reply: "تشرع صلاة الخوف في المعارك بكيفيات متعددة وردت في السنة النبوية لحفظ الأمن." },
    { roots: ["أحكام صلاة الجمعة", "التخلف عن الجمعة", "صلاة الجمعة"], reply: "تجب صلاة الجمعة على كل مسلم بالغ عاقل مقيم، ومن تركها ثلاث جمع طبع الله على قلبه." },
    { roots: ["أحكام صلاة العيدين", "تكبيرات العيد", "صلاة العيد"], reply: "صلاة العيدين سنة مؤكدة، ويُسن فيها التكبير الزائد والخطبة بعدها لإدخال الفرح." },
    { roots: ["أحكام صلاة الاستسقاء", "طلب الغيث", "صلاة الاستسقاء"], reply: "تشرع صلاة الاستسقاء جماعة في المصلى عند احتضار المطر وتأخر الغيث مع التذلل والافتقار." },
    { roots: ["أحكام صلاة الكسوف والخسوف", "الفزع إلى الصلاة", "صلاة الكسوف", "صلاة الخسوف"], reply: "تستحب صلاة الكسوف والخسوف بركعتين في كل ركعة قيامان وركوعان وسجودان." },
    { roots: ["أحكام الجنازة", "صلاة الجنازة"], reply: "صلاة الجنازة فرض كفاية، وأركانها أربع تكبيرات تقرأ فيها الفاتحة والصلاة والدعاء." },
    { roots: ["أحكام التغسيل", "تغسيل الميت"], reply: "تغسيل الميت وتكفينه والصلاة عليه ودفنه فروض كفاية على المجتمع المسلم." },
    { roots: ["أحكام الدفن", "دفن الميت"], reply: "يُسن دفن الميت في لحد مقبرة المسلمين موجهاً لقبلة الكعبة على شقه الأيمن." },
    { roots: ["أحكام التعزية", "التعزية"], reply: "تشرع التعزية لأهل الميت لتسلية مصيبتهم ودعوتهم للصبر والاحتساب دون إحداث مآتم." },
    { roots: ["أحكام القبور", "زيارة القبور"], reply: "تشرع زيارة القبور للرجال والنساء للاتعاظ بالآخرة والدعاء للموتى بالسلام والمغفرة." },
    { roots: ["أحكام الصيام", "شروط الصيام"], reply: "الصيام ركن من أركان الإسلام، وشروطه الإسلام والبلوغ والعقل والإقامة والصحة." },
    { roots: ["أحكام المفطرات", "المفطرات", "نواقض الصيام"], reply: "يبطل الصيام بالأكل والشرب عمداً والجماع والاستقاءة العمد ونزول دم الحيض والنفاس." },
    { roots: ["أحكام القضاء", "قضاء رمضان"], reply: "يجب قضاء الأيام المفطرة من رمضان قبل حلول رمضان التالي، ويجوز التفريق والتتابع." },
    { roots: ["أحكام الفدية", "فدية الصيام"], reply: "من عجز عن الصيام لكبر أو مرض مزمن لزمته الفدية بإطعام مسكين عن كل يوم أفطره." },
    { roots: ["أحكام الحامل", "صيام الحامل", "صيام المرضع"], reply: "الحامل والمرضع إن خافتا على ولديهما أفطرتتا وقاضتا، وبعضهم أوجب مع القضاء الإطعام." },
    { roots: ["أحكام التطوع", "صيام التطوع"], reply: "أفضل الصيام تطوعاً صيام يوم وإفطار يوم، وصيام الإثنين والخميس وثلاثة أيام من كل شهر." },
    { roots: ["أحكام الأيام البيض", "صيام الأيام البيض"], reply: "يُسن صيام الأيام البيض وهي الثالث عشر والرابع عشر والخامس عشر من كل شهر قَمَري." },
    { roots: ["أحكام زكاة الفطر", "زكاة الفطر"], reply: "تجب زكاة الفطر على كل مسلم يملِك قوت يومه، ومقدارها صاع من تمر أو شعير أو طعام." },
    { roots: ["أحكام زكاة المال", "التجارة المالية", "زكاة المال"], reply: "تجب زكاة المال إذا بلغ النصاب الشرعي وحال عليه الحول القمري، وتقوم عروض التجارة بسعر السوق وقت الحول." },
    { roots: ["أحكام زكاة النقدين", "زكاة الذهب", "زكاة الفضة"], reply: "تجب الزكاة في الذهب والفضة والأوراق النقدية بنسبة ربع العشر (2.5 بالمئة)." },
    { roots: ["أحكام الزروع", "الحبوب", "زكاة الزروع"], reply: "تجب زكاة الزروع والثمار فيما يكطال ويدخر إذا بلغ خمسة أوسق، ومقدارها العشر أو نصفه." },
    { roots: ["أحكام المعادن", "الركاز"], reply: "الركاز المستخرج من دفن الجاهلية فيه الخمس مباشرة، والمعادن فيها الزكاة بشروطها." },
    { roots: ["أحكام المصارف", "مصارف الزكاة"], reply: "تُصرف الزكاة للأصناف الثمانية المذكورة في سورة التوبة كالفقراء والمساكين." },
    { roots: ["أحكام الحج", "أركان الحج"], reply: "الحج فرض على كل مسلم مستطيع في عمره مرة، وأركانه الإحرام والطواف والسعي والوقوف بعرفة." },
    { roots: ["أحكام العمرة", "العمرة"], reply: "العمرة سنة مؤكدة أو واجبة في العمر مرة، وتكفر ما بينها وبين العمرة الأخرى لمن أتمها." },
    { roots: ["أحكام الإحرام", "الإحرام"], reply: "يجب الإحرام من الميقات المحدد لمن أراد الحج أو العمرة، ومن جاوزه بلا إحرام لزمه دم." },
    { roots: ["أحكام المحظورات", "محظورات الإحرام"], reply: "يحرم على المحرم لبس المخيط للرجل، وتغطية الرأس، وقص الأظافر، وحلق الشعر، والطيب." },
    { roots: ["أحكام الطواف", "طواف الإفاضة", "طواف الوداع"], reply: "طواف الإفاضة ركن من أركان الحج، وطواف الوداع واجب على الآفاقي قبل مغادرة مكة." },
    { roots: ["أحكام السعي", "السعي بين الصفا والمروة"], reply: "يبدأ السعي من الصفا وينتهي بالمروة سبعة أشواط تامة بعد طواف صحيح في الحج أو العمرة." },
    { roots: ["أحكام الوقوف", "الوقوف بعرفة"], reply: "الوقوف بعرفة ركن الحج الأعظم، والمبيت بمزدلفة ومِنا من واجبات الحج اللازمة." },
    { roots: ["أحكام الجمرات", "رمي الجمرات"], reply: "يجب رمي الجمرات الثلاث أيام التشريق حصاة حصاة مع التكبير لقول النبي صلى الله عليه وسلم." },
    { roots: ["أحكام الأضحية", "الأضحية"], reply: "الأضحية سنة مؤكدة لمن استطاع، ويشترط فيها السلامة والسن المعتبرة في الأنعام." },
    { roots: ["أحكام العقيقة", "العقيقة"], reply: "العقيقة سنة مؤكدة تذبح عن المولود يوم سابعه، شاتان عن الغلام وشاة عن الجارية." },
    { roots: ["أحكام البيع", "شروط البيع"], reply: "يقوم البيع على الإيجاب والقبول ورضا المتبايعين، وخلوه من الربا والغرر المحرم." },
    { roots: ["أحكام البيوع", "البيوع المحرمة"], reply: "تحرم بيوع الغرر والمجهول والمعدوم والربا بأنواعه لما تسبب من أكل أموال الناس بالباطل." },
    { roots: ["أحكام الإجارة", "الإجارة"], reply: "الإجارة عقد جائز على المنافع أو الأعمال بأعمال معلومة وأجور محددة غير مجهولة." },
    { roots: ["أحكام الشركة", "المضاربة"], reply: "الشركة والمضاربة جائزتان لتنمية الأموال بالشروط الشرعية وتوزيع الأرباح بالاتفاق." },
    { roots: ["أحكام الرهن", "الكفالة"], reply: "الرهن وثيقة بالدين لاستيفائه عند التعذر، والكفالة التزام بالنفس والضمان بالمال." },
    { roots: ["أحكام الوكالة", "الوكالة"], reply: "تجوز الوكالة في كل حق يقبل النيابة كالبيع والشراء والخصومة وقبض الديون." },
    { roots: ["أحكام اللقطة", "اللقطة"], reply: "اللقطة تُعَرّف سنة كاملة، واللقيط نفس معصومة يجب رعايتها والإنفاق عليها." },
    { roots: ["أحكام الوقف", "الوقف"], reply: "الوقف قربة عظيمة يحبس فيها الواقف أصل المال ويسپل منفعته في وجوه الخير والبر." },
    { roots: ["أحكام الميراث", "الميراث", "التركة"], reply: "الميراث نظام عادل يوزع التركة على أصحاب الفروض والعصبات بحسب الأنصباء القرآنية." },
    { roots: ["أحكام الوصية", "الوصية"], reply: "تجوز الوصية لغير الوارث بحدود ثلث التركة فقط، ولا وصية لوارث إلا بإجازة الورثة." },
    { roots: ["أحكام النكاح", "شروط النكاح", "الزواج"], reply: "يقوم النكاح الصحيح على الإيجاب والقبول، ووجود الولي، والشهود، وخلو الزوجين من الموانع." },
    { roots: ["أحكام الطلاق", "الخلع", "العدة"], reply: "الخلع فرقة بعوض تأخذه الزوجة لتباري به زوجها، والطلاق بائن والعدة تحصين للرحم." },
    { roots: ["أحكام الرضاع", "الرضاع"], reply: "يحرم من الرضاع ما يحرم من النسب بشرط خمس رضعات مشبعات في سن الحولين الأولين." },
    { roots: ["أحكام الحضانة", "الحضانة"], reply: "الأم أحق بحضانة طفلها ما لم تتزوج بأجنبي، وحقها يسقط بالزواج أو بوجود مانع شرعي." },
    { roots: ["أحكام الجنايات", "القصاص"], reply: "الجنايات توجب القصاص في العمد، والدية والكفارة في الخطأ لحفظ دماء البشر." },
    { roots: ["أحكام الحدود", "الحدود الشرعية"], reply: "الحدود عقوبات مقدرة شرعاً لحفظ الدين والأعراض والأموال كحد السرقة والزنا." },
    { roots: ["أحكام التعزير", "التعزير"], reply: "التعزير عقوبات غير مقدرة شرعاً يجتهد فيها ولي الأمر والقاضي لدفع الجرائم وحماية المجتمع." },
    { roots: ["أحكام الجهاد", "الجهاد"], reply: "يُشرع الجهاد للدفاع عن الدين، وتجوز الهدنة والعهود مع الكفار إذا وجدت مصلحة راجحة." },
    { roots: ["أحكام الذمة", "أهل الذمة"], reply: "أهل الذمة لهم ما للمسلمين من حقوق الحماية والرعاية مقابل الجزية وحفظ النظام." },
    { roots: ["أحكام الآداب", "الأخلاق في الإسلام"], reply: "أمر الإسلام بحسن الخلق، والصدق، والأمانة، والوفاء، وحرم الكذب والغيبة والنميمة." },
    { roots: ["أحكام الصلة", "بر الوالدين"], reply: "بر الوالدين وصلة الرحم من أعظم القربات الموجبة للجنة، وعقوقهما من أكبر الكبائر." },
    { roots: ["أحكام الجوار", "إكرام الجار"], reply: "يجب كف الأذى عن الجار وإكرامه، وإكرام الضيف من خصال الإيمان والتقوى لله تعالى." },
    { roots: ["أحكام البيئة", "الرفق بالحيوان"], reply: "أمر الإسلام بالإحسان للحيوان وحرم تعذيبه، وحث على غرس الأشجار وإماطة الأذى." },
    { roots: ["أحكام التوكل", "التوكل على الله"], reply: "التوكل الحق يجمع بين صدق الاعتماد على الله وفعل الأسباب، والرضا بالقدر يورث الطمأنينة." },
    { roots: ["أحكام الإخلاص", "الإخلاص"], reply: "الإخلاص شرط لقبول الأعمال، والمراقبة استشعار نظر الله في السر والعلن دائماً وأبداً." },
    { roots: ["أحكام التوبة", "شروط التوبة"], reply: "التوبة تجب ما قبلها، وشرائطها: الإقلاع عن الذنب، والندم، والعزم على عدم العودة." },
    { roots: ["أحكام الذكر", "فضل الذكر"], reply: "ذكر الله يجلو القلوب، والاستغفار يفتح الأقفال، والدعاء مخ العبادة وأعظم أسباب الإجابة." },
    { roots: ["أحكام القيام", "قيام الليل"], reply: "قيام الليل دأب الصالحين ومطردة للداء عن الجسد، وأفضلها صلاة جوف الليل الآخر." },
    { roots: ["أحكام التطوع المطلق"], reply: "يُسن صيام التطوع في الأيام الفاضلة كعاشوراء وعرفة وست شوال والاثنين والخميس." },
    { roots: ["أحكام الصدقة", "الصدقة"], reply: "الصدقة الخفية تطفئ غضب الرب، وتظل صاحبها في ظل عرشه يوم لا ظل إلا ظله." },
    { roots: ["أحكام الإصلاح", "إصلاح ذات البين"], reply: "إصلاح ذات البين أفضل من درجة الصيام والصلاة، وفساد ذات البين هي الحالقة.

    { roots: ["أحكام اليتامى", "كفالة اليتيم"], reply: "كافل اليتيم رفيق النبي صلى الله عليه وسلم في الجنة كهاتين وأشار بإصبعيه." },
    { roots: ["أحكام اللسان", "حفظ اللسان"], reply: "حفظ اللسان من الغيبة والنميمة والبهتان من أعظم وسائل النجاة من عذاب النار." },
    { roots: ["أحكام الستر", "ستر المسلم"], reply: "من ستر مسلماً في الدنيا ستره الله في الدنيا والآخرة، والجزاء من جنس العمل." },
    { roots: ["أحكام الظلم", "عاقبة الظلم"], reply: "اتقوا الظلم فإن الظلم ظلمات يوم القيامة، وادعوا لنصرة المظلوم وردع الظالم." },
    { roots: ["أحكام الحسد", "الحسد"], reply: "إياكم والحسد فإن الحسد يأكل الحسنات كما تأكل النار الحطب اليابس." },
    { roots: ["أحكام الكبر", "الكبر"], reply: "الكبر بطر الحق وغمط الناس، وهو مانع من قبول الحق ودخول الجنة مع الأتقياء." },
    { roots: ["أحكام الغيبة", "النميمة"], reply: "الغيبة والنميمة من الكبائر المحرمة التي تفسد المجتمعات وتورث عذاب القبر." },
    { roots: ["أحكام الرشوة", "الرشوة"], reply: "الرشوة والربا من أكبر الكبائر الموبقة المهلكة لصاحبها في الدنيا والآخرة." },
    { roots: ["أحكام السحر", "الكهانة"], reply: "من أتى كاهناً أو عرافاً فصدقه بما يقول فقد كفر بما أنزل على محمد صلى الله عليه وسلم." },
    { roots: ["أحكام الشهادة", "شهادة الزور"], reply: "شهادة الزور من أكبر الكبائر المهلكة بعد الإشراك بالله وعقوق الوالدين." },
    { roots: ["أحكام اليمين", "اليمين الغموس"], reply: "اليمين الغموس هي التي يقتطع بها مال امرئ مسلم بغير حق، وتغمس صاحبها بالنار." },
    { roots: ["أحكام القطيعة", "قطيعة الرحم"], reply: "لا يدخل الجنة قاطع رحم لحديث البخاري الصحيح، وصلتها تزيد في الأجل والرزق." },
    { roots: ["أحكام العقوق", "عقوق الوالدين"], reply: "عقوق الوالدين من أكبر الكبائر المهلكة، وبرهما من أحب الأعمال إلى الله تعالى." },
    { roots: ["أحكام التشبه"], reply: "لعن النبي صلى الله عليه وسلم المتشبهين من الرجال بالنساء والمتشبهات بالرجال." },
    { roots: ["أحكام النياحة"], reply: "النياحة ورفع الصوت بالويل والثبور على الميت من أعمال الجاهلية المحرمة." },
    { roots: ["أحكام الهجر"], reply: "لا يحل لمسلم أن يهجر أخاه فوق ثلاث ليال وخيرهما يبدأ بالسلام." },
    { roots: ["أحكام الغضب", "علاج الغضب"], reply: "ليس الشديد بالصرعة، بل الشديد الذي يملك نفسه عند الغضب، ولا تغضب ولك الجنة." },
    { roots: ["أحكام الرياء", "الرياء"], reply: "أخوف ما أخاف عليكم الشرك الأصغر وهو الرياء الذي يحبط صالح الأعمال يوم القيامة." },
    { roots: ["أحكام معروف", "تغيير المنكر"], reply: "تغيير المنكر باليد أو اللسان أو القلب من أصول الدين وإصلاح المجتمع المسلم." },
    { roots: ["أحكام النصيحة", "الدين النصيحة"], reply: "الدين النصيحة لله ولكتابه ولرسوله ولأئمة المسلمين وعامتهم بطلب الخير لهم." },
    { roots: ["أحكام الوفاء", "الوفاء بالعهد"], reply: "الوفاء بالعهد وحفظ ود الأصدقاء وصلة المودة من شيم الكرام الأبرار الصالحين." },
    { roots: ["أحكام الضيافة", "إكرام الضيف"], reply: "إكرام الضيف يوم وليلة، والضيافة ثلاثة أيام، وما زاد فهو صدقة تؤجر عليها." },
    { roots: ["أحكام صلة الرحم", "صلة الرحم"], reply: "ليس الواصل بالمكافئ، بل الواصل الذي إذا قطعت رحمه وصلها بالعفو والصفح." },
    { roots: ["أحكام الخدم", "الرفق بالخدم"], reply: "الرفق بالخدم والمماليك وإطعامهم مما تأكلون وتلبسون من توجيهات الإسلام السامية." },
    { roots: ["أحكام التوقير", "احترام الكبير"], reply: "توقير الكبير ورحمة الصغير من آداب الإسلام الرفيعة لحفظ تماسك المجتمع وأخلاقه." },
    { roots: ["أحكام الحلم", "الحلم والأناة"], reply: "الحلم والأناة صفات نبيلة يحبها الله، والتعجل من الشيطان في تصريف الأمور." },
    { roots: ["أحكام الصبر", "الصبر الجميل"], reply: "الصبر الجميل هو الذي لا شكوى فيه لغير الله، والرضا التام بالقدر المقدر بحكمة." },
    { roots: ["أحكام الرضا", "الرضا بالقدر"], reply: "عجباً لأمر المؤمن إن أمره كله خير، إن أصابته سرّاء شكر فكان خيراً له." },
    { 
      roots: ["الوقت", "أهمية الوقت", "تعبير عن الوقت", "الوقت وأهميته", "استثمار الوقت", "استغلال الوقت"], 
      reply: "يعد الوقت من أثمن النعم التي يمتلكها الإنسان، فهو أساس النجاح والتقدم في الحياة.\n\nتكمن أهمية الوقت في أنه يساعد على تحقيق الأهداف وإنجاز الأعمال وتنظيم شؤون الحياة. ويمكن استغلاله من خلال وضع خطة يومية، وتحديد الأولويات، والابتعاد عن الملهيات، والالتزام بالمواعيد. كما أن استثمار الوقت في التعلم والعمل النافع يعود بالفائدة على الفرد والمجتمع.\n\nوفي الختام، فإن حسن استغلال الوقت هو مفتاح النجاح، لذلك يجب المحافظة عليه وعدم إضاعته فيما لا ينفع." 
    },
    { 
      roots: ["الوطن", "حب الوطن", "تعبير عن حب الوطن", "تعبير عن الوطن", "أهمية الوطن", "واجبنا نحو الوطن"], 
      reply: "الوطن ليس مجرد أرض نعيش فوقها، بل هو تاريخنا وذكرياتنا وهويتنا التي نفخر بها بين الأمم. فيه ولدنا، وعلى أرضه نشأنا، ومن خيراته نعيش ونحلم بمستقبل أفضل. لذلك فإن حب الوطن شعور فطري يسكن القلوب، ويجعل الإنسان متعلقًا بأرضه ومخلصًا لها مهما ابتعد عنها.\n\nيتجلى حب الوطن في المحافظة على ممتلكاته العامة، واحترام قوانينه، والعمل بجدٍ وإخلاص من أجل رفعته وتقدمه. فالمواطن الصالح لا يكتفي بالكلام عن حب وطنه، بل يترجمه إلى أفعال نافعة تساهم في تطوره وازدهاره. كما أن طلب العلم، ونشر الأخلاق الحسنة، ومساعدة الآخرين، كلها صور مشرقة من صور حب الوطن. وعندما يتعاون أبناء الوطن ويتحلون بروح المسؤولية، يصبح وطنهم أقوى وأكثر تقدمًا واستقرارًا.\n\nوالوطن يستحق منا الكثير، لأنه يمنحنا الأمن والانتماء والكرامة. لذلك يجب أن نحافظ عليه، وأن ندافع عنه، وأن نسعى دائمًا إلى ترك أثر طيب يساهم في بناء مستقبله المشرق للأجيال القادمة.\n\nوفي الختام، يبقى حب الوطن من أنبل المشاعر وأعظم القيم الإنسانية، فهو مصدر العزة والفخر لكل إنسان. ومن واجبنا أن نحافظ على وطننا ونخدمه بكل ما نستطيع، لأن ازدهاره هو ازدهار لنا جميعًا، ورفعته دليل على إخلاص أبنائه ووفائهم له." 
    },
    { 
      roots: ["Alislamiah AI", "ماهي Alislamiah AI", "Alislamiah bing", "الإسلامية أي أي", "شبكة الإسلامية"], 
      reply: "Alislamiah AI هو محرك البحث الذكي المطور بواسطة شبكة Alislamiah." 
    },
    { 
      roots: ["Gemalot", "Gemalot AI", "جيمايلوت", "من أنت", "من انت"], 
      reply: "Gemalot هو مساعدك الذكي الذاتي، تم تطويري بواسطة شبكة Alislamiah لخدمتك وإجابة استفساراتك." 
    }
];

// 2. محرك حل المعادلات الرياضية
function solveMathPro(text) {
  try {
    let expr = text.replace("حل المعادلة:", "").replace("حل:", "").replace("solve:", "").trim();
    if (!expr) return null;

    if (expr.includes("=")) {
      const parts = expr.split("=");
      if (parts.length === 2) {
        const left = parts[0].trim();
        const right = parts[1].trim();

        if (left.toLowerCase().includes("x") || right.toLowerCase().includes("x")) {
          try {
            const eq = math.parse(`(${left}) - (${right})`);
            let solvedX = null;
            for (let x = -100; x <= 100; x += 0.5) {
              if (Math.abs(eq.evaluate({ x })) < 0.0001) {
                solvedX = x;
                break;
              }
            }
            if (solvedX !== null) {
              return `🧮 **خطوات حل المعادلة:**\nالمعادلة الأصلية: ${expr}\n1) تحويل المعادلة للطرف الصفري: (${left}) - (${right}) = 0\n2) التبسيط والتعويض التجريبي.\n\n✅ **النتيجة النهائيـة:** x = ${solvedX}`;
            }
          } catch(e) {}
        }

        try {
          const lVal = math.evaluate(left);
          const rVal = math.evaluate(right);
          return `🧮 **تحليل المعادلة:**\nالطرف الأيسر: ${left} = ${lVal}\nالطرف الأيمن: ${right} = ${rVal}\nالنتيجة: المعادلة ${lVal === rVal ? "صحيحة ✅" : "غير صحيحة ❌"}`;
        } catch(e) {}
      }
    }

    const res = math.evaluate(expr);
    if (res !== undefined) {
      return `🧮 **خطوات الحل الرياضي:**\nالمسألة: ${expr}\nتطبيق أولويات العمليات الحسابية...\n\n✅ **الناتج النهائي:** ${res}`;
    }
  } catch (e) {
    return null;
  }
  return null;
}

// 3. الإنشاء والوسائط
function generateImagePollinations(prompt) {
  const seed = Math.floor(Math.random() * 100000);
  const enc = encodeURIComponent(prompt + " , highly detailed, 4k resolution, realistic, masterpiece");
  return `https://image.pollinations.ai/prompt/${enc}?width=1024&height=1024&seed=${seed}&nologo=true&enhance=true`;
}

function generateVideoCanvas(prompt) {
  const canvas = document.createElement("canvas");
  canvas.width = 640;
  canvas.height = 360;
  const ctx = canvas.getContext("2d");
  let frame = 0;
  return new Promise(resolve => {
    const stream = canvas.captureStream(30);
    const recorder = new MediaRecorder(stream, { mimeType: "video/webm" });
    const chunks = [];
    recorder.ondataavailable = e => chunks.push(e.data);
    recorder.onstop = () => {
      const blob = new Blob(chunks, { type: "video/webm" });
      resolve(URL.createObjectURL(blob));
    };
    recorder.start();
    function draw() {
      ctx.fillStyle = `hsl(${(frame * 3) % 360}, 75%, 25%)`;
      ctx.fillRect(0, 0, 640, 360);
      ctx.fillStyle = "#ffffff";
      ctx.font = "bold 22px Arial";
      ctx.textAlign = "center";
      ctx.fillText(`🎬 فيديو أُنشئ بواسطة Gemalot`, 320, 160);
      ctx.font = "16px Arial";
      ctx.fillText(prompt.slice(0, 45), 320, 200);
      frame++;
      if (frame < 90) {
        requestAnimationFrame(draw);
      } else {
        setTimeout(() => recorder.stop(), 100);
      }
    }
    draw();
  });
}

// 4. محرك البحث الخارجي
async function searchExternal(q) {
  try {
    const res = await fetch(`https://ar.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);
    if (res.ok) {
      const data = await res.json();
      if (data.extract) return { text: data.extract, source: "Wikipedia Ar" };
    }
  } catch(e) {}
  return null;
}

// 5. معالجة الرسائل الوافدة
const chatContainer = document.getElementById('chatContainer');
const userInput = document.getElementById('userInput');
const fileInput = document.getElementById('fileInput');

function appendMessage(text, sender, source = null, htmlContent = null) {
  const w = document.getElementById("welcomeBox");
  if (w) w.style.display = "none";

  const div = document.createElement("div");
  div.className = `message ${sender}`;

  if (htmlContent) {
    div.innerHTML = htmlContent;
  } else {
    const span = document.createElement("span");
    span.textContent = text;
    div.appendChild(span);
    if (source) {
      const tag = document.createElement("span");
      tag.className = "source-tag";
      tag.textContent = `المصدر: ${source}`;
      div.appendChild(document.createElement("br"));
      div.appendChild(tag);
    }
  }
  chatContainer.appendChild(div);
  window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
}

async function processMessage(customText = null) {
  // التحقق أولاً من قيود الاستخدام
  if (!checkLimits()) return;

  const text = (customText || userInput.value).trim();
  if (!text) return;

  // الخصم واحتساب الرسالة عند الإرسال الناجح
  incrementMsgCount();

  userInput.value = "";
  appendMessage(text, "user");

  const lower = text.toLowerCase();

  // أ) إنشاء صورة
  if (lower.startsWith("صورة:") || lower.startsWith("صوره:") || lower.startsWith("انشئ صورة") || lower.startsWith("image:")) {
    let prompt = text.replace(/(صورة:|صوره:|انشئ صورة:|image:)/i, "").trim() || "Islamic architecture mosque, 4k";
    appendMessage(`🎨 جاري توليد الصورة الذكية...`, "ai");
    const imgUrl = generateImagePollinations(prompt);
    const html = `<div>🎨 <b>الوصف:</b> ${prompt}</div><img src="${imgUrl}" style="width:100%;border-radius:12px;margin-top:8px"><br><a href="${imgUrl}" target="_blank" style="display:inline-block;margin-top:6px;padding:4px 8px;border-radius:8px;background:#facc15;color:#111;text-decoration:none;font-size:11px">تحميل 4K</a>`;
    appendMessage("", "ai", "Pollinations AI", html);
    return;
  }

  // ب) إنشاء فيديو
  if (lower.startsWith("فيديو:") || lower.startsWith("video:") || lower.startsWith("انشئ فيديو")) {
    let prompt = text.replace(/(فيديو:|video:|انشئ فيديو:)/i, "").trim() || "Sunset landscape";
    appendMessage(`🎬 جاري معالجة وإنشاء الفيديو...`, "ai");
    const videoUrl = await generateVideoCanvas(prompt);
    const html = `<div>🎬 <b>المشهد:</b> ${prompt}</div><video src="${videoUrl}" controls autoplay loop style="width:100%;border-radius:12px;margin-top:8px"></video><br><a href="${videoUrl}" download="gemalot_video.webm" style="display:inline-block;margin-top:6px;padding:4px 8px;border-radius:8px;background:#facc15;color:#111;text-decoration:none;font-size:11px">تحميل الفيديو</a>`;
    appendMessage("", "ai", "Gemalot Video Engine", html);
    return;
  }

  // ج) حل المسائل الرياضية
  if (lower.includes("حل") || lower.match(/[0-9x+\-*/^=]/)) {
    const mathResult = solveMathPro(text);
    if (mathResult) {
      appendMessage(mathResult, "ai", "Gemalot Math Pro");
      return;
    }
  }

  // د) قاعدة البيانات المحلية
  let localReply = null;
  for (const item of chatKnowledge) {
    for (const root of item.roots) {
      if (text.toLowerCase().includes(root.toLowerCase())) {
        localReply = item.reply;
        break;
      }
    }
    if (localReply) break;
  }

  if (localReply) {
    appendMessage(localReply, "ai", "قاعدة البيانات الشرعية - Alislamiah");
    return;
  }

  // هـ) البحث الخارجي
  appendMessage("جاري البحث في المصادر...", "ai");
  const ext = await searchExternal(text);
  const lastMsg = chatContainer.lastChild;
  if (lastMsg && lastMsg.textContent === "جاري البحث في المصادر...") lastMsg.remove();

  if (ext) {
    appendMessage(ext.text, "ai", ext.source);
  } else {
    appendMessage("عذراً، لم أجد إجابة دقيقة في قاعدة البيانات أو المصادر الخارجية حالياً.", "ai");
  }
}

// 6. رفع الملفات
function triggerUpload(type) {
  fileInput.setAttribute("data-type", type);
  fileInput.accept = type === "image" ? "image/*" : "video/*";
  fileInput.click();
}

fileInput.addEventListener("change", (e) => {
  if (!checkLimits()) return;
  const file = e.target.files[0];
  if (!file) return;

  incrementMsgCount();
  const url = URL.createObjectURL(file);

  if (file.type.startsWith("image/")) {
    const html = `<div>🖼️️ <b>صورة مرفوعة:</b> ${file.name}</div><img src="${url}" style="max-width:100%;border-radius:12px;margin-top:8px">`;
    appendMessage("", "user", null, html);
    appendMessage("تم استلام الصورة بنجاح ✅", "ai");
  } else if (file.type.startsWith("video/")) {
    const html = `<div>🎥 <b>فيديو مرفوع:</b> ${file.name}</div><video src="${url}" controls style="max-width:100%;border-radius:12px;margin-top:8px"></video>`;
    appendMessage("", "user", null, html);
    appendMessage("تم رفع وتشغيل الفيديو بنجاح ✅", "ai");
  }
});

// أدوات الواجهة
function toggleSidebar() { document.getElementById("sidebar").classList.toggle("open"); }
function toggleTools() { document.getElementById("toolsPopup").classList.toggle("open"); }
function closeTools() { document.getElementById("toolsPopup").classList.remove("open"); }
function askImageGen() { const p = prompt("أدخل وصف الصورة (مثال: مسجد في الغروب):"); if(p) processMessage("انشئ صورة: " + p); }
function askVideoGen() { const p = prompt("أدخل مشهد الفيديو (مثال: أمواج البحر):"); if(p) processMessage("انشئ فيديو: " + p); }
function insertPrompt(txt) { userInput.value = txt; userInput.focus(); }
function newChat() { chatContainer.innerHTML = ''; appendMessage("أهلاً بك! كيف يمكنني مساعدتك اليوم؟", "ai"); toggleSidebar(); }

userInput.addEventListener("keydown", (e) => {
  if (e.key === "Enter" && !e.shiftKey) {
    e.preventDefault();
    processMessage();
  }
});

// تهيئة القيود عند التحميل
initLimits();
setInterval(updateCounterUI, 60000); // تحديث الوقت كل دقيقة
</script>
</body>
</html>
