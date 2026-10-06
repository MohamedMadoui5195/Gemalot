<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column}

.header{
  height:60px;
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:0 16px;
  position:fixed;
  top:0;left:0;width:100%;
  z-index:1000;
}
.header-title{font-size:20px;font-weight:bold;color:#fff}
.header-actions{display:flex;gap:10px;align-items:center}

.menu-btn{
  width:40px;height:40px;
  background:rgba(0,0,0,.25);
  border:none;border-radius:10px;
  cursor:pointer;
  display:flex;flex-direction:column;
  justify-content:center;align-items:center;gap:5px;
  color:#fff;font-size:18px;
}
.menu-btn span{
  display:block;width:20px;height:2.5px;
  background:#fff;border-radius:2px;
}

/* شريط النماذج */
.models-bar{
  position:fixed;
  top:60px;
  left:0;
  width:100%;
  background:#0b0f19;
  padding:10px 16px;
  display:flex;
  gap:8px;
  overflow-x:auto;
  z-index:950;
  border-bottom:1px solid #1f2937;
}
.model-btn{
  flex-shrink:0;
  padding:8px 16px;
  border-radius:20px;
  border:1.5px solid rgba(255,255,255,.15);
  background:transparent;
  color:#e5e7eb;
  font-size:13px;
  font-weight:600;
  cursor:pointer;
  white-space:nowrap;
  transition:.2s;
}
.model-btn.active{
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  color:#111;
  border-color:transparent;
}
.model-btn.locked{
  opacity:0.7;
}
.model-btn:hover:not(.active){
  background:rgba(255,255,255,.08);
}

/* القائمة من اليسار */
.sidebar{
  position:fixed;top:110px;left:0;
  width:280px;height:calc(100vh - 110px);
  background:#111827;
  border-right:1px solid #1f2937;
  z-index:900;
  transform:translateX(-100%);
  transition:transform .3s;
  overflow-y:auto;padding:16px;
}
.sidebar.open{transform:translateX(0)}
.sidebar h3{font-size:15px;margin-bottom:12px;color:#facc15}
.chat-item{
  display:flex;
  align-items:center;
  justify-content:space-between;
  padding:12px;
  border-radius:12px;
  background:#1f2937;
  margin-bottom:8px;
  font-size:13px;
  color:#e5e7eb;
}
.chat-item .chat-title{
  flex:1;
  cursor:pointer;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  margin-left:8px;
}
.chat-item .chat-title:hover{color:#facc15}
.delete-btn{
  background:none;
  border:none;
  color:#ef4444;
  font-size:16px;
  cursor:pointer;
  padding:4px 8px;
  border-radius:6px;
}
.delete-btn:hover{background:rgba(239,68,68,.15)}
.new-chat-btn{
  width:100%;padding:11px;border:none;border-radius:12px;
  background:linear-gradient(90deg,#facc15,#f97316);
  color:#111;font-weight:bold;cursor:pointer;margin-bottom:16px;
}

.chat-container{
  flex:1;width:100%;max-width:800px;
  margin:110px auto 90px;padding:20px;
  display:flex;flex-direction:column;
}
.welcome-box{text-align:center;margin:auto;padding:40px 0}
.welcome-box h1{font-size:24px;margin-bottom:6px}
.welcome-box p{color:#9ca3af;font-size:13px}

.message{
  max-width:85%;padding:12px 16px;margin:8px 0;
  border-radius:16px;font-size:14px;line-height:1.6;
  word-wrap:break-word;text-align:right;
}
.message.user{
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  color:#fff;font-weight:bold;margin-right:auto;
  border-bottom-left-radius:4px;
}
.message.ai{
  background:#fff;color:#111827;margin-left:auto;
  border-bottom-right-radius:4px;
  box-shadow:0 4px 6px rgba(0,0,0,.1);
}

.input-area{
  position:fixed;bottom:0;left:0;width:100%;
  padding:12px 16px;background:#0b0f19;
  display:flex;justify-content:center;z-index:1000;
}
.input-wrapper{
  width:100%;max-width:700px;background:#fff;
  border-radius:35px;display:flex;align-items:center;
  padding:4px 6px 4px 14px;box-shadow:0 4px 10px rgba(0,0,0,.2);
}
.input-wrapper textarea{
  flex:1;background:transparent;border:none;outline:none;
  color:#111827;font-size:14px;resize:none;max-height:100px;
  padding:8px 0;direction:rtl;
}
.input-wrapper textarea::placeholder{color:#9ca3af}
.send-btn{
  width:50px;height:50px;border-radius:50%;
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e);
  border:none;color:#fff;display:flex;align-items:center;
  justify-content:center;cursor:pointer;flex-shrink:0;
  font-size:13px;font-weight:bold;
}

.full-glow{
  position:fixed;top:0;left:0;width:100%;height:100%;
  pointer-events:none;z-index:9999;opacity:0;
  background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);
  background-size:400% 400%;
  mix-blend-mode:screen;
}
.full-glow.active{animation:fullGlowFlow 2s ease-in-out forwards}
@keyframes fullGlowFlow{
  0%{background-position:0% 50%;opacity:0.75}
  25%{background-position:40% 50%;opacity:0.9}
  50%{background-position:80% 50%;opacity:0.8}
  100%{background-position:0% 50%;opacity:0}
}

@media(max-width:768px){.sidebar{width:100%}}
</style>
</head>
<body>

<div class="full-glow" id="fullGlow"></div>

<div class="header">
  <div class="header-title">Gemalot</div>
  <div class="header-actions">
    <button class="menu-btn" onclick="window.location.href='Settings.html'" title="Settings">⚙️</button>
    <button class="menu-btn" onclick="toggleSidebar()">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </div>
</div>

<!-- النماذج: Normal مجاني | الباقي اشتراك -->
<div class="models-bar">
  <button class="model-btn active" data-model="normal" onclick="selectModel(this)">Gemalot Normal</button>
  <button class="model-btn locked" data-model="plus" onclick="goSubscription()">Gemalot Plus 🔒</button>
  <button class="model-btn locked" data-model="bronze" onclick="goSubscription()">Gemalot Bronze 🔒</button>
  <button class="model-btn locked" data-model="silver" onclick="goSubscription()">Gemalot Silver 🔒</button>
  <button class="model-btn locked" data-model="gold" onclick="goSubscription()">Gemalot Gold 🔒</button>
</div>

<!-- القائمة من اليسار -->
<div class="sidebar" id="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات السابقة</h3>
  <div id="chatList"></div>
</div>

<div class="chat-container" id="chatContainer">
  <div class="welcome-box" id="welcomeBox">
    <h1>Gemalot</h1>
    <p>كيف يمكنني مساعدتك؟</p>
  </div>
</div>

<div class="input-area">
  <div class="input-wrapper">
    <textarea id="userInput" placeholder="اكتب سؤالك هنا..." rows="1"></textarea>
    <button class="send-btn" id="sendBtn">إرسال</button>
  </div>
</div>

<script>
if (!localStorage.getItem("alislamiah_current_user")) {
  window.location.href = "signin.html";
}

const chatKnowledge = [
  { roots: ["كيف يمكنك مساعدتك","مرحباً","السلام عليكم","أهلاً","مرحبا","هلا","السلآم"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Gemalot، جاهز لإجابتك عن الأسئلة والأحكام الفقهية الشرعية." },
  { roots: ["الوطن","حب الوطن","تعبير عن حب الوطن","تعبير عن الوطن","أهمية الوطن","واجبنا نحو الوطن"], reply: "الوطن ليس مجرد أرض نعيش فوقها، بل هو تاريخنا وذكرياتنا وهويتنا التي نفخر بها بين الأمم. فيه ولدنا، وعلى أرضه نشأنا، ومن خيراته نعيش ونحلم بمستقبل أفضل. لذلك فإن حب الوطن شعور فطري يسكن القلوب، ويجعل الإنسان متعلقًا بأرضه ومخلصًا لها مهما ابتعد عنها.\n\nيتجلى حب الوطن في المحافظة على ممتلكاته العامة، واحترام قوانينه، والعمل بجدٍ وإخلاص من أجل رفعته وتقدمه. فالمواطن الصالح لا يكتفي بالكلام عن حب وطنه، بل يترجمه إلى أفعال نافعة تساهم في تطوره وازدهاره. كما أن طلب العلم، ونشر الأخلاق الحسنة، ومساعدة الآخرين، كلها صور مشرقة من صور حب الوطن. وعندما يتعاون أبناء الوطن ويتحلون بروح المسؤولية، يصبح وطنهم أقوى وأكثر تقدمًا واستقرارًا.\n\nوالوطن يستحق منا الكثير، لأنه يمنحنا الأمن والانتماء والكرامة. لذلك يجب أن نحافظ عليه، وأن ندافع عنه، وأن نسعى دائمًا إلى ترك أثر طيب يساهم في بناء مستقبله المشرق للأجيال القادمة.\n\nوفي الختام، يبقى حب الوطن من أنبل المشاعر وأعظم القيم الإنسانية، فهو مصدر العزة والفخر لكل إنسان. ومن واجبنا أن نحافظ على وطننا ونخدمه بكل ما نستطيع، لأن ازدهاره هو ازدهار لنا جميعًا، ورفعته دليل على إخلاص أبنائه ووفائهم له." },
  { roots: ["الوقت","أهمية الوقت","تعبير عن الوقت","الوقت وأهميته","استثمار الوقت","استغلال الوقت"], reply: "يعد الوقت من أثمن النعم التي يمتلكها الإنسان، فهو أساس النجاح والتقدم في الحياة.\n\nتكمن أهمية الوقت في أنه يساعد على تحقيق الأهداف وإنجاز الأعمال وتنظيم شؤون الحياة. ويمكن استغلاله من خلال وضع خطة يومية، وتحديد الأولويات، والابتعاد عن الملهيات، والالتزام بالمواعيد. كما أن استثمار الوقت في التعلم والعمل النافع يعود بالفائدة على الفرد والمجتمع.\n\nوفي الختام، فإن حسن استغلال الوقت هو مفتاح النجاح، لذلك يجب المحافظة عليه وعدم إضاعته فيما لا ينفع." },
  { roots: ["إلى اللقاء","مع السلامة","سلام","وداعاً","باي"], reply: "في أمان الله ورعايته! أتمنى أن أكون قد أفدتك، وتسعدني عودتك دائماً." },
  { roots: ["الوضوء","نواقض الوضوء","طهارة"], reply: "نواقض الوضوء هي: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، وأكل لحم الإبل." },
  { roots: ["الغسل","الجنب","الجنابة"], reply: "يجب الغسل بالجنابة، الحيض، النفاس، وكيفيته تعميم الجسد بالماء مع النية." },
  { roots: ["التيمم"], reply: "يُشرع التيمم بفاقد الماء أو العاجز عنه لاستعماله بضرب الصعيد الطاهر ضربة واحدة للوجه والكفين." },
  { roots: ["الصلاة","شروط الصلاة","أركان الصلاة"], reply: "الصلاة عماد الدين، وشروطها الطهارة ودخول الوقت ستر العورة واستقبال القبلة، وأركانها تكبيرة الإحرام والقرآن والركوع والسجود." },
  { roots: ["سجود السهو","سجدتي السهو"], reply: "سجود السهو مشروع لجبر ما حصل في الصلاة من زيادة أو نقص، ويكون قبل السلام أو بعده." },
  { roots: ["سجود التلاوة","سجدة القرآن"], reply: "يسجد القارئ والمستمع سجود التلاوة عند مروره بآية سجود، ويكبر لها دون تشهد أو تسليم." },
  { roots: ["سجود الشكر"], reply: "يسن سجود الشكر لله تعالى عند تجدد نعمة عظيمة أو اندفاع نقمة، وهو سجدة واحدة." },
  { roots: ["قصر الصلاة","رخصة السفر","صلاة المسافر"], reply: "يشرع للمسافر قصر الصلاة الرباعية إلى ركعتين، وجمع الظهر والعصر أو المغرب والعشاء، ويجوز له الفطر في رمضان." },
  { roots: ["صلاة الجمعة"], reply: "تجب صلاة الجمعة على كل مسلم بالغ عاقل مقيم، ومن تركها ثلاث جمع طبع الله على قلبه." },
  { roots: ["صلاة العيدين"], reply: "صلاة العيدين سنة مؤكدة، ويُسن فيها التكبير الزائد والخطبة بعدها لإدخال الفرح." },
  { roots: ["صلاة الاستسقاء"], reply: "تشرع صلاة الاستسقاء جماعة في المصلى عند انقطاع المطر وتأخر الغيث مع التذلل والافتقار لله." },
  { roots: ["صلاة الكسوف","صلاة الخسوف"], reply: "تستحب صلاة الكسوف والخسوف بركعتين في كل ركعة قيامان وركوعان وسجودان." },
  { roots: ["صلاة الجنازة"], reply: "صلاة الجنازة فرض كفاية، وأركانها أربع تكبيرات تقرأ فيها الفاتحة والصلاة الإبراهيمية والدعاء للميت." },
  { roots: ["الصيام","احكام الصيام","رمضان"], reply: "الصيام ركن من أركان الإسلام، وشروطه الإسلام والبلوغ والعقل والإقامة والصحة." },
  { roots: ["المفطرات","ما يبطل الصيام"], reply: "يبطل الصيام بالأكل والشرب عمداً والجماع والاستقاءة العمد ونزول دم الحيض والنفاس." },
  { roots: ["القضاء"], reply: "يجب قضاء الأيام المفطرة من رمضان قبل حلول رمضان التالي، ويجوز التفريق والتتابع." },
  { roots: ["الفدية"], reply: "من عجز عن الصيام لكبر أو مرض مزمن لزمته الفدية بإطعام مسكين عن كل يوم أفطره." },
  { roots: ["زكاة المال","الزكاة"], reply: "تجب زكاة المال إذا بلغ النصاب الشرعي وحال عليه الحول القمري بنسبة ربع العشر (2.5 بالمئة)." },
  { roots: ["زكاة الفطر"], reply: "تجب زكاة الفطر على كل مسلم يملِك قوت يومه، ومقدارها صاع من تمر أو شعير أو طعام." },
  { roots: ["الحج","احكام الحج والعمرة"], reply: "الحج فرض على كل مسلم مستطيع في عمره مرة، وأركانه الإحرام والطواف والسعي والوقوف بعرفة." },
  { roots: ["العمرة"], reply: "العمرة سنة مؤكدة أو واجبة في العمر مرة، وتكفر ما بينها وبين العمرة الأخرى لمن أتمها." },
  { roots: ["الإحرام"], reply: "يجب الإحرام من الميقات المحدد لمن أراد الحج أو العمرة، ومن جاوزه بلا إحرام لزمه دم." },
  { roots: ["الأضحية"], reply: "الأضحية سنة مؤكدة لمن استطاع، ويشترط فيها السلامة والسن المعتبرة في الأنعام." },
  { roots: ["البيوع","التجارة","البيع"], reply: "يقوم البيع على الإيجاب والقبول ورضا المتبايعين، وخلوه من الربا والغرر المحرم." },
  { roots: ["الربا"], reply: "الربا محرم تحريماً قاطعاً وهو من أكبر الكبائر، سواء كان ربا نسيئة أو ربا فضل." },
  { roots: ["الميراث"], reply: "الميراث نظام عادل يوزع التركة على أصحاب الفروض والعصبات بحسب الأنصباء القرآنية." },
  { roots: ["الوصية"], reply: "تجوز الوصية لغير الوارث بحدود ثلث التركة فقط، ولا وصية لوارث إلا بإجازة الورثة." },
  { roots: ["النكاح","الزواج"], reply: "يقوم النكاح الصحيح على الإيجاب والقبول، ووجود الولي، والشهود، وخلو الزوجين من الموانع." },
  { roots: ["الطلاق"], reply: "الطلاق حق للزوج بيد، ويدخل في الأحكام الخمسة بحسب سببه، والعدة تحصين للرحم." },
  { roots: ["الرضاع"], reply: "يحرم من الرضاع ما يحرم من النسب بشرط خمس رضعات مشبعات في سن الحولين الأولين." },
  { roots: ["الحضانة"], reply: "الأم أحق بحضانة طفلها ما لم تتزوج بأجنبي، وحقها يسقط بالزواج أو بوجود مانع شرعي." },
  { roots: ["بر الوالدين","صلة الرحم"], reply: "بر الوالدين وصلة الرحم من أعظم القربات الموجبة للجنة، وعقوقهما من أكبر الكبائر." },
  { roots: ["التوبة"], reply: "التوبة تجب ما قبلها، وشرائطها: الإقلاع عن الذنب، والندم، والعزم على عدم العودة." }
];

const chatContainer = document.getElementById('chatContainer');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');
const fullGlow = document.getElementById('fullGlow');
const chatList = document.getElementById('chatList');

let currentChatId = null;
let messages = [];

function selectModel(btn) {
  document.querySelectorAll('.model-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  localStorage.setItem('gemalot_model', 'normal');
}

function goSubscription() {
  window.location.href = 'subscription.html';
}

function triggerFullGlow() {
  fullGlow.classList.remove('active');
  void fullGlow.offsetWidth;
  fullGlow.classList.add('active');
}

function getChats() {
  return JSON.parse(localStorage.getItem("gemalot_chats") || "[]");
}
function saveChats(chats) {
  localStorage.setItem("gemalot_chats", JSON.stringify(chats));
}

function renderChatList() {
  const chats = getChats();
  chatList.innerHTML = "";
  chats.forEach(c => {
    const div = document.createElement("div");
    div.className = "chat-item";
    div.innerHTML = `
      <span class="chat-title">${c.title || "محادثة بدون عنوان"}</span>
      <button class="delete-btn" title="حذف">🗑️</button>
    `;
    div.querySelector('.chat-title').onclick = () => loadChat(c.id);
    div.querySelector('.delete-btn').onclick = (e) => {
      e.stopPropagation();
      deleteChat(c.id);
    };
    chatList.appendChild(div);
  });
}

function deleteChat(id) {
  let chats = getChats();
  chats = chats.filter(c => c.id !== id);
  saveChats(chats);
  if (currentChatId === id) {
    newChat();
  }
  renderChatList();
}

function newChat() {
  currentChatId = Date.now().toString();
  messages = [];
  chatContainer.innerHTML = `<div class="welcome-box" id="welcomeBox"><h1>Gemalot</h1><p>كيف يمكنني مساعدتك؟</p></div>`;
  toggleSidebar();
}

function loadChat(id) {
  const chats = getChats();
  const chat = chats.find(c => c.id === id);
  if (!chat) return;
  currentChatId = id;
  messages = chat.messages || [];
  chatContainer.innerHTML = "";
  messages.forEach(m => appendMessage(m.text, m.sender, false));
  toggleSidebar();
}

function saveCurrentChat() {
  if (!messages.length) return;
  const chats = getChats();
  const title = messages[0]?.text?.slice(0, 30) || "محادثة جديدة";
  const existing = chats.findIndex(c => c.id === currentChatId);
  const data = { id: currentChatId || Date.now().toString(), title, messages, updated: Date.now() };
  if (existing > -1) chats[existing] = data;
  else chats.unshift(data);
  saveChats(chats);
  renderChatList();
}

function toggleSidebar() {
  document.getElementById("sidebar").classList.toggle("open");
}

function appendMessage(text, sender, save = true) {
  const welcome = document.getElementById("welcomeBox");
  if (welcome) welcome.style.display = "none";
  const msgDiv = document.createElement("div");
  msgDiv.className = `message ${sender}`;
  chatContainer.appendChild(msgDiv);
  window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });

  if (sender === "ai") {
    let i = 0;
    function typeWriter() {
      if (i < text.length) {
        msgDiv.textContent += text.charAt(i);
        i++;
        window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
        setTimeout(typeWriter, 12);
      }
    }
    typeWriter();
  } else {
    msgDiv.textContent = text;
  }

  if (save) {
    messages.push({ text, sender });
    if (!currentChatId) currentChatId = Date.now().toString();
    saveCurrentChat();
  }
}

/* بحث ويكيبيديا عند عدم وجود إجابة محلية */
async function searchWikipedia(query) {
  try {
    const url = `https://ar.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(query)}`;
    const res = await fetch(url);
    if (!res.ok) return null;
    const data = await res.json();
    if (data.extract) {
      return data.extract + (data.content_urls?.desktop?.page ? "\n\nالمصدر: " + data.content_urls.desktop.page : "");
    }
    return null;
  } catch (e) {
    return null;
  }
}

async function processMessage() {
  const text = userInput.value.trim();
  if (!text) return;

  triggerFullGlow();
  appendMessage(text, "user");
  userInput.value = "";

  // 1) البحث في قاعدة المعرفة المحلية أولاً
  let reply = null;
  for (let item of chatKnowledge) {
    for (let root of item.roots) {
      if (text.toLowerCase().includes(root.toLowerCase())) {
        reply = item.reply;
        break;
      }
    }
    if (reply) break;
  }

  // 2) إذا لم يجد → يبحث في الإنترنت (ويكيبيديا)
  if (!reply) {
    appendMessage("جاري البحث في الإنترنت...", "ai", false);
    const wiki = await searchWikipedia(text);
    // حذف رسالة "جاري البحث"
    const lastMsg = chatContainer.querySelector('.message.ai:last-child');
    if (lastMsg && lastMsg.textContent.includes("جاري البحث")) {
      lastMsg.remove();
    }
    reply = wiki || "عذراً، لم أتمكن من العثور على إجابة لهذا السؤال في قاعدة المعرفة ولا على الإنترنت.";
  }

  appendMessage(reply, "ai");
}

userInput.addEventListener("focus", triggerFullGlow);
sendBtn.addEventListener("click", processMessage);
userInput.addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) {
    e.preventDefault();
    processMessage();
  }
});

renderChatList();
</script>
</body>
</html>