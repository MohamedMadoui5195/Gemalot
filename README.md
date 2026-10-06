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
  height:56px;
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  display:flex;align-items:center;justify-content:space-between;
  padding:0 14px;position:fixed;top:0;left:0;width:100%;z-index:1000;
}
.header-title{font-size:18px;font-weight:bold;color:#fff}
.header-actions{display:flex;gap:8px}
.menu-btn{
  width:38px;height:38px;background:rgba(0,0,0,.25);border:none;border-radius:10px;
  cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff;font-size:16px;
}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}

.models-bar{
  position:fixed;top:56px;left:0;width:100%;
  display:flex;gap:8px;padding:10px 12px;overflow-x:auto;
  background:#0b0f19;border-bottom:1px solid #1f2937;z-index:950;
}
.model-btn{
  flex-shrink:0;padding:8px 14px;border-radius:20px;
  border:1.5px solid rgba(255,255,255,.15);background:transparent;
  color:#e5e7eb;font-size:12px;font-weight:600;cursor:pointer;white-space:nowrap;
}
.model-btn.active{
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  color:#111;border-color:transparent;
}
.model-btn.locked{opacity:.7}

.sidebar{
  position:fixed;top:108px;left:0;width:280px;height:calc(100vh - 108px);
  background:#111827;border-right:1px solid #1f2937;z-index:900;
  transform:translateX(-100%);transition:transform .3s;overflow-y:auto;padding:14px;
}
.sidebar.open{transform:translateX(0)}
.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{
  display:flex;align-items:center;justify-content:space-between;
  padding:10px;border-radius:10px;background:#1f2937;margin-bottom:6px;font-size:13px;
}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer;padding:2px 6px}
.new-chat-btn{
  width:100%;padding:10px;border:none;border-radius:10px;
  background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:bold;cursor:pointer;margin-bottom:12px;
}

.chat-container{
  flex:1;width:100%;max-width:800px;margin:108px auto 140px;padding:16px;
  display:flex;flex-direction:column;
}
.welcome-box{text-align:center;margin:auto;padding:40px 0}
.welcome-box h1{font-size:24px;margin-bottom:8px}
.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}
.welcome-box p{color:#9ca3af;font-size:13px}

.message{
  max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;
  font-size:14px;line-height:1.6;word-wrap:break-word;
}
.message.user{
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);
  color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px;
}
.message.ai{
  background:#fff;color:#111;margin-left:auto;
  border-bottom-right-radius:4px;box-shadow:0 2px 6px rgba(0,0,0,.1);
}
.source-logo{
  display:block;margin-top:10px;height:20px;opacity:.75;
}

.suggestions{
  position:fixed;bottom:70px;left:0;width:100%;
  display:flex;gap:8px;padding:8px 12px;overflow-x:auto;
  background:#0b0f19;z-index:999;
}
.sug-btn{
  flex-shrink:0;padding:8px 14px;border-radius:20px;
  border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.06);
  color:#e5e7eb;font-size:12px;cursor:pointer;white-space:nowrap;
}
.sug-btn:hover{background:rgba(250,204,21,.15);border-color:#facc15}

.input-area{
  position:fixed;bottom:0;left:0;width:100%;
  padding:10px 14px;background:#0b0f19;z-index:1000;
}
.input-wrapper{
  max-width:700px;margin:0 auto;display:flex;align-items:center;
  background:#fff;border-radius:30px;padding:4px 6px 4px 14px;
}
.input-wrapper textarea{
  flex:1;border:none;outline:none;resize:none;font-size:14px;
  color:#111;max-height:80px;padding:8px 0;direction:rtl;background:transparent;
}
.send-btn{
  width:46px;height:46px;border-radius:50%;border:none;
  background:linear-gradient(90deg,#facc15,#f97316,#22c55e);
  color:#fff;font-size:12px;font-weight:bold;cursor:pointer;flex-shrink:0;
}

.full-glow{
  position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;
  background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);
  background-size:400% 400%;mix-blend-mode:screen;
}
.full-glow.active{animation:fullGlowFlow 2s ease-in-out forwards}
@keyframes fullGlowFlow{
  0%{background-position:0% 50%;opacity:.75}
  25%{background-position:40% 50%;opacity:.9}
  50%{background-position:80% 50%;opacity:.8}
  100%{background-position:0% 50%;opacity:0}
}
</style>
</head>
<body>

<div class="full-glow" id="fullGlow"></div>

<div class="header">
  <div class="header-title">Gemalot</div>
  <div class="header-actions">
    <button class="menu-btn" onclick="location.href='Settings.html'">⚙️</button>
    <button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button>
  </div>
</div>

<div class="models-bar">
  <button class="model-btn active" onclick="selectModel(this)">Gemalot Normal</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Gemalot Plus 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Gemalot Bronze 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Gemalot Silver 🔒</button>
  <button class="model-btn locked" onclick="location.href='subscription.html'">Gemalot Gold 🔒</button>
</div>

<div class="sidebar" id="sidebar">
  <button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button>
  <h3>المحادثات السابقة</h3>
  <div id="chatList"></div>
</div>

<div class="chat-container" id="chatContainer">
  <div class="welcome-box" id="welcomeBox">
    <h1>Gemalot</h1>
    <div class="line"></div>
    <p>كيف يمكنني مساعدتك؟</p>
  </div>
</div>

<div class="suggestions">
  <button class="sug-btn" onclick="sendSuggestion('ما هي نواقض الوضوء؟')">نواقض الوضوء</button>
  <button class="sug-btn" onclick="sendSuggestion('أحكام الصيام')">أحكام الصيام</button>
  <button class="sug-btn" onclick="sendSuggestion('أركان الصلاة')">أركان الصلاة</button>
  <button class="sug-btn" onclick="sendSuggestion('ما هي الزكاة؟')">الزكاة</button>
  <button class="sug-btn" onclick="sendSuggestion('السلام عليكم')">السلام عليكم</button>
</div>

<div class="input-area">
  <div class="input-wrapper">
    <textarea id="userInput" placeholder="اكتب سؤالك هنا..." rows="1"></textarea>
    <button class="send-btn" id="sendBtn">إرسال</button>
  </div>
</div>

<script>
if (!localStorage.getItem("alislamiah_current_user")) location.href = "signin.html";

const chatKnowledge = [
  { roots: ["كيف يمكنك مساعدتك","مرحباً","السلام عليكم","أهلاً","مرحبا","هلا"], reply: "وعليكم السلام ورحمة الله وبركاته! أنا مساعدك الذكي Gemalot، جاهز لإجابتك." },
  { roots: ["الوضوء","نواقض الوضوء","طهارة"], reply: "نواقض الوضوء هي: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، وأكل لحم الإبل." },
  { roots: ["الغسل","الجنب","الجنابة"], reply: "يجب الغسل بالجنابة، الحيض، النفاس، وكيفيته تعميم الجسد بالماء مع النية." },
  { roots: ["التيمم"], reply: "يُشرع التيمم بفاقد الماء أو العاجز عنه بضرب الصعيد الطاهر ضربة واحدة للوجه والكفين." },
  { roots: ["الصلاة","شروط الصلاة","أركان الصلاة"], reply: "الصلاة عماد الدين، وشروطها الطهارة ودخول الوقت وستر العورة واستقبال القبلة." },
  { roots: ["الصيام","احكام الصيام","رمضان"], reply: "الصيام ركن من أركان الإسلام، وشروطه الإسلام والبلوغ والعقل والإقامة والصحة." },
  { roots: ["المفطرات","ما يبطل الصيام"], reply: "يبطل الصيام بالأكل والشرب عمداً والجماع والاستقاءة العمد ونزول دم الحيض والنفاس." },
  { roots: ["زكاة","الزكاة"], reply: "تجب زكاة المال إذا بلغ النصاب وحال عليه الحول بنسبة ربع العشر (2.5%)." },
  { roots: ["الحج","العمرة"], reply: "الحج فرض على المستطيع مرة في العمر، وأركانه الإحرام والطواف والسعي والوقوف بعرفة." },
  { roots: ["الربا"], reply: "الربا محرم تحريماً قاطعاً وهو من أكبر الكبائر." },
  { roots: ["التوبة"], reply: "التوبة تجب ما قبلها: الإقلاع والندم والعزم على عدم العودة." },
  { roots: ["إلى اللقاء","مع السلامة","وداعاً","باي"], reply: "في أمان الله! تسعدني عودتك دائماً." }
];

const chatContainer = document.getElementById('chatContainer');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');
const fullGlow = document.getElementById('fullGlow');
const chatList = document.getElementById('chatList');
let currentChatId = null, messages = [];

function selectModel(btn) {
  document.querySelectorAll('.model-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
}
function triggerFullGlow() {
  fullGlow.classList.remove('active');
  void fullGlow.offsetWidth;
  fullGlow.classList.add('active');
}
function getChats(){ return JSON.parse(localStorage.getItem("gemalot_chats")||"[]"); }
function saveChats(c){ localStorage.setItem("gemalot_chats", JSON.stringify(c)); }

function renderChatList() {
  chatList.innerHTML = "";
  getChats().forEach(c => {
    const div = document.createElement("div");
    div.className = "chat-item";
    div.innerHTML = `<span class="chat-title">${c.title||"محادثة"}</span><button class="delete-btn">🗑️</button>`;
    div.querySelector('.chat-title').onclick = () => loadChat(c.id);
    div.querySelector('.delete-btn').onclick = e => { e.stopPropagation(); deleteChat(c.id); };
    chatList.appendChild(div);
  });
}
function deleteChat(id) {
  saveChats(getChats().filter(c => c.id !== id));
  if (currentChatId === id) newChat();
  renderChatList();
}
function newChat() {
  currentChatId = Date.now().toString();
  messages = [];
  chatContainer.innerHTML = `<div class="welcome-box" id="welcomeBox"><h1>Gemalot</h1><div class="line"></div><p>كيف يمكنني مساعدتك؟</p></div>`;
  document.getElementById("sidebar").classList.remove("open");
}
function loadChat(id) {
  const chat = getChats().find(c => c.id === id);
  if (!chat) return;
  currentChatId = id; messages = chat.messages || [];
  chatContainer.innerHTML = "";
  messages.forEach(m => appendMessage(m.text, m.sender, false, m.source));
  document.getElementById("sidebar").classList.remove("open");
}
function saveCurrentChat() {
  if (!messages.length) return;
  const chats = getChats();
  const title = messages[0]?.text?.slice(0,30) || "محادثة";
  const data = { id: currentChatId||Date.now().toString(), title, messages, updated: Date.now() };
  const i = chats.findIndex(c => c.id === currentChatId);
  if (i > -1) chats[i] = data; else chats.unshift(data);
  saveChats(chats); renderChatList();
}
function toggleSidebar(){ document.getElementById("sidebar").classList.toggle("open"); }

function appendMessage(text, sender, save=true, source) {
  const w = document.getElementById("welcomeBox");
  if (w) w.style.display = "none";
  const msgDiv = document.createElement("div");
  msgDiv.className = `message ${sender}`;
  chatContainer.appendChild(msgDiv);
  window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });

  if (sender === "ai") {
    let i = 0;
    (function type() {
      if (i < text.length) {
        msgDiv.textContent += text.charAt(i++);
        window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
        setTimeout(type, 10);
      } else if (source === "wiki") {
        // لوجو ويكيبيديا فقط — بدون رابط نصي
        const img = document.createElement("img");
        img.className = "source-logo";
        img.src = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/Wikipedia-logo-v2.svg/48px-Wikipedia-logo-v2.svg.png";
        img.alt = "";
        msgDiv.appendChild(document.createElement("br"));
        msgDiv.appendChild(img);
      }
    })();
  } else {
    msgDiv.textContent = text;
  }

  if (save) {
    messages.push({ text, sender, source });
    if (!currentChatId) currentChatId = Date.now().toString();
    saveCurrentChat();
  }
}

async function searchWikipedia(q) {
  try {
    const res = await fetch(`https://ar.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);
    if (!res.ok) return null;
    const d = await res.json();
    return d.extract || null;
  } catch { return null; }
}

async function processMessage(text) {
  if (!text) text = userInput.value.trim();
  if (!text) return;
  triggerFullGlow();
  appendMessage(text, "user");
  userInput.value = "";

  let reply = null, source = null;
  for (const item of chatKnowledge) {
    for (const root of item.roots) {
      if (text.toLowerCase().includes(root.toLowerCase())) { reply = item.reply; break; }
    }
    if (reply) break;
  }

  if (!reply) {
    const wiki = await searchWikipedia(text);
    if (wiki) {
      reply = wiki;
      source = "wiki";
    } else {
      reply = "تعذر تحميل الإجابة";
    }
  }

  appendMessage(reply, "ai", true, source);
}

function sendSuggestion(t) { processMessage(t); }

userInput.addEventListener("focus", triggerFullGlow);
sendBtn.addEventListener("click", () => processMessage());
userInput.addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); processMessage(); }
});
renderChatList();
</script>
</body>
</html>