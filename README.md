<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemalot</title>
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
.model-btn.locked{opacity:.7}
.sidebar{position:fixed;top:108px;left:0;width:280px;height:calc(100vh - 108px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .3s;overflow-y:auto;padding:14px}
.sidebar.open{transform:translateX(0)}.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{display:flex;align-items:center;justify-content:space-between;padding:10px;border-radius:10px;background:#1f2937;margin-bottom:6px;font-size:13px}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer;padding:2px 6px}
.new-chat-btn{width:100%;padding:10px;border:none;border-radius:10px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:bold;cursor:pointer;margin-bottom:12px}
.chat-container{flex:1;width:100%;max-width:800px;margin:108px auto 180px;padding:16px;display:flex;flex-direction:column}
.welcome-box{text-align:center;margin:auto;padding:40px 0}.welcome-box h1{font-size:24px;margin-bottom:8px}.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}.welcome-box p{color:#9ca3af;font-size:13px}.welcome-box .limit-info{margin-top:16px;padding:12px;background:#111827;border-radius:12px;border:1px solid #1f2937;font-size:11px;line-height:1.6}
.message{max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;font-size:14px;line-height:1.6;word-wrap:break-word;white-space:pre-wrap}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:4px;box-shadow:0 2px 6px rgba(0,0,0,.1)}
.message.ai img,.message.ai video{max-width:100%;border-radius:12px;margin-top:8px;display:block}
.message.blocked{background:#7f1d1d;color:#fff;margin:0 auto;max-width:95%;text-align:center;border-radius:12px}
.source-tag{font-size:9px;color:#6b7280;margin-top:6px;display:block;border-top:1px solid #e5e7eb;padding-top:4px}
.suggestions{position:fixed;bottom:86px;left:0;width:100%;display:flex;gap:8px;padding:8px 12px;overflow-x:auto;background:transparent;z-index:999;scrollbar-width:none}
.sug-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.08);color:#e5e7eb;font-size:11px;cursor:pointer;white-space:nowrap;backdrop-filter:blur(6px)}
.sug-btn:hover{background:rgba(250,204,21,.15);border-color:#facc15}
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 12px 14px;background:linear-gradient(to top,#0b0f19 75%,transparent);z-index:1000}
.gemini-bar{max-width:720px;margin:0 auto;display:flex;align-items:center;background:#ffffff;border-radius:32px;padding:6px 8px;gap:6px;box-shadow:0 8px 30px rgba(0,0,0,.35),0 0 0 1px rgba(0,0,0,.05)}
.gemini-bar:focus-within{box-shadow:0 10px 36px rgba(0,0,0,.45),0 0 0 2px rgba(250,204,21,.35)}
.plus-btn{width:40px;height:40px;border-radius:50%;border:none;background:#fff;color:#111;font-size:26px;font-weight:300;cursor:pointer;flex-shrink:0;display:flex;align-items:center;justify-content:center}
.plus-btn:hover{background:#f3f4f6}
.input-wrapper{flex:1;display:flex;align-items:center;min-width:0}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:15px;color:#111;background:transparent;max-height:90px;padding:10px 2px;line-height:1.4}
.input-wrapper textarea::placeholder{color:#6b7280}
.mic-btn{width:40px;height:40px;border-radius:50%;border:none;background:transparent;color:#111;font-size:20px;cursor:pointer;flex-shrink:0;display:flex;align-items:center;justify-content:center}
.mic-btn:hover{background:#f3f4f6}
.send-btn{width:44px;height:44px;border-radius:50%;border:none;cursor:pointer;flex-shrink:0;display:flex;align-items:center;justify-content:center;transition:.2s}
.send-btn.wave{background:#e0f2fe;color:#111;font-size:16px}
.send-btn.active{background:linear-gradient(135deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;font-size:18px;box-shadow:0 2px 10px rgba(250,204,21,.35)}
.tools-popup{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);max-width:340px;width:90%;background:#111827;border:1px solid #1f2937;border-radius:18px;padding:10px;display:none;z-index:1001;box-shadow:0 20px 40px rgba(0,0,0,.5)}
.tools-popup.open{display:block}
.tools-popup button{width:100%;text-align:right;padding:10px 12px;border-radius:10px;border:none;background:transparent;color:#e5e7eb;font-size:12px;cursor:pointer;display:flex;align-items:center;gap:8px}
.tools-popup button:hover{background:#1f2937}
.full-glow{position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);background-size:400% 400%;mix-blend-mode:screen}
.full-glow.active{animation:fullGlowFlow 1.8s ease-in-out forwards}
@keyframes fullGlowFlow{0%{background-position:0% 50%;opacity:.7}50%{background-position:80% 50%;opacity:.85}100%{background-position:0% 50%;opacity:0}}
.edit-modal{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:2000;display:none;align-items:center;justify-content:center;padding:12px}
.edit-box{background:#111827;border-radius:16px;padding:16px;width:100%;max-width:500px;max-height:90vh;overflow:auto}
.edit-box canvas{width:100%;border-radius:12px;background:#000;margin:10px 0}
.edit-tools{display:flex;flex-wrap:wrap;gap:6px}
.edit-tools button{padding:6px 10px;border-radius:8px;border:1px solid #374151;background:#1f2937;color:#fff;font-size:11px;cursor:pointer}
</style>
</head>
<body>
<div class="header"><div class="header-title">Gemalot</div><div class="header-actions"><div id="msgCounter" class="counter-badge">25/25</div><button id="settingsBtn" class="menu-btn">⚙️️</button><button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button></div></div>
<div class="models-bar">
<button class="model-btn active" onclick="selectModel(this)">Gemalot Normal</button>
<button class="model-btn locked" onclick="location.href='subscription.html'">Plus 45 🔒</button>
<button class="model-btn locked" onclick="location.href='subscription.html'">Bronze 65 🔒</button>
<button class="model-btn locked" onclick="location.href='subscription.html'">Silver 75 🔒</button>
<button class="model-btn locked" onclick="location.href='subscription.html'">Gold 100 🔒</button>
</div>
<div id="sidebar" class="sidebar"><button class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button><h3>المحادثات السابقة</h3><div id="chatList"></div><div style="margin-top:20px;padding:10px;background:#0b0f19;border-radius:10px;font-size:11px;color:#9ca3af"><div id="subStatus">الوضع: عادي (25 رسالة/يوم)</div><div id="subExpiry" style="margin-top:4px"></div></div></div>
<div id="chatContainer" class="chat-container"><div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p id="welcomeText">كيف يمكنني مساعدتك؟</p><div class="limit-info" id="limitInfo"></div></div></div>
<div id="suggestionsBar" class="suggestions"></div>
<div class="tools-popup" id="toolsPopup">
<button onclick="triggerUpload('image');closeTools()"><span>🖼️</span> رفع صورة وتعديلها</button>
<button onclick="triggerUpload('video');closeTools()"><span>🎥</span> رفع فيديو</button>
<button onclick="askImageGen();closeTools()"><span>🎨</span> إنشاء صورة 4K</button>
<button onclick="askVideoGen();closeTools()"><span>🎬</span> إنشاء فيديو</button>
<button onclick="insertPrompt('حل المعادلة: ');closeTools()"><span>🧮</span> حل معادلة</button>
</div>
<div class="input-area">
<div class="gemini-bar" id="geminiBar">
<button class="plus-btn" id="plusBtn" onclick="toggleTools()">+</button>
<div id="inputWrapper" class="input-wrapper"><textarea id="userInput" placeholder="Ask Gemalot" rows="1"></textarea></div>
<button class="mic-btn" id="micBtn" onclick="toggleMic()">🎙️</button>
<button id="sendBtn" class="send-btn wave"><span id="sendIcon">◍</span></button>
</div>
</div>
<input type="file" id="fileInput" hidden accept="image/*,video/*">
<div id="fullGlow" class="full-glow"></div>
<div id="editModal" class="edit-modal"><div class="edit-box"><h3 style="color:#facc15;font-size:14px">محرر احترافي - بدون سيرفر</h3><canvas id="editCanvas"></canvas><div class="edit-tools"><button onclick="applyFilter('grayscale')">أبيض وأسود</button><button onclick="applyFilter('sepia')">Sepia</button><button onclick="applyFilter('invert')">عكسي</button><button onclick="applyFilter('bright')">تفتيح</button><button onclick="rotateCanvas(90)">دوران</button><button onclick="addTextOverlay()">نص</button><button onclick="downloadCanvas()">تحميل</button><button onclick="closeEditor()">إغلاق</button></div></div></div>
<script>
const planLimits={normal:25, plus:45, bronze:65, silver:75, gold:100};
const translations={ar:{dir:"rtl",welcome:"كيف يمكنني مساعدتك؟",placeholder:"Ask Gemalot",placeholderAr:"اسأل Gemalot",newChat:"+ محادثة جديدة",prevChats:"المحادثات السابقة",send:"إرسال",suggestions:["أنشئ صورة: مسجد مستقبلي","أنشئ فيديو: محيط","حل المعادلة: 2x+3=11","نواقض الوضوء","أحكام الصيام"],noAnswer:"عذراً، لم أجد إجابة دقيقة.",searching:"جاري البحث في 35 مصدر...",normalMode:"الوضع: عادي",premiumMode:"الوضع: مميز",remaining:"متبقي: {n}/{t} اليوم",blockedTitle:"انتهت رسائلك",blockedMsg:"استهلكت {t} رسالة اليوم.",subscribeNow:"الاشتراك",waitTime:"المتبقي: {h}س {m}د"},en:{dir:"ltr",welcome:"How can I help you?",placeholder:"Ask Gemalot",placeholderAr:"Ask Gemalot",newChat:"+ New Chat",prevChats:"Previous",send:"Send",suggestions:["generate image: futuristic mosque","generate video: ocean","solve: 2x+3=11"],noAnswer:"No exact answer.",searching:"Searching 35 sources...",normalMode:"Mode: Normal",premiumMode:"Premium",remaining:"Remaining: {n}/{t}",blockedTitle:"Limit reached",blockedMsg:"You used {t} messages.",subscribeNow:"Subscribe",waitTime:"{h}h {m}m"}};
let currentLang=localStorage.getItem("gemalot_language")||"ar";let t=translations[currentLang]||translations.ar;
function parseCurrentUser(){const raw=localStorage.getItem("alislamiah_current_user")||"";if(!raw)return {raw:"",name:"",email:"",id:""};try{const j=JSON.parse(raw);if(j&&typeof j==="object")return {raw,name:j.name||j.username||"",email:j.email||"",id:String(j.id||"")};}catch{}return {raw,name:raw,email:"",id:""};}
function getSubscription(){const allSubs=JSON.parse(localStorage.getItem("gemalot_all_subscriptions")||"{}");const cu=parseCurrentUser();if(allSubs[cu.raw])return allSubs[cu.raw];if(cu.name&&allSubs[cu.name])return allSubs[cu.name];if(cu.email&&allSubs[cu.email])return allSubs[cu.email];for(let k in allSubs){const s=allSubs[k];if(!s)continue;if(cu.email&&s.email&&s.email.toLowerCase()===cu.email.toLowerCase())return s;if(cu.name&&k.toLowerCase()===cu.name.toLowerCase())return s;}return null;}
function getDailyLimit(){const sub=getSubscription();if(sub&&sub.dailyLimit)return parseInt(sub.dailyLimit);if(sub&&sub.plan&&planLimits[sub.plan])return planLimits[sub.plan];return planLimits.normal;}
function isSubscribed(){const sub=getSubscription();if(sub&&sub.expiry){if(Date.now()<parseInt(sub.expiry)){localStorage.setItem("gemalot_subscription_active","true");localStorage.setItem("gemalot_subscription_expiry",sub.expiry);return true;}else{localStorage.setItem("gemalot_subscription_active","false");return false;}}const a=localStorage.getItem("gemalot_subscription_active")==="true";const e=localStorage.getItem("gemalot_subscription_expiry");if(!a)return false;if(!e)return true;return Date.now()<parseInt(e);}
function getTodayStr(){return new Date().toISOString().split('T')[0];}
function checkAndResetDaily(){const d=localStorage.getItem("gemalot_daily_date");const today=getTodayStr();if(d!==today){localStorage.setItem("gemalot_daily_date",today);localStorage.setItem("gemalot_daily_count","0");localStorage.removeItem("gemalot_block_until");}}
function getBlockUntil(){return parseInt(localStorage.getItem("gemalot_block_until")||"0");}
function canSendMessage(){checkAndResetDaily();if(isSubscribed())return {allowed:true,limit:getDailyLimit()};const b=getBlockUntil();if(b&&Date.now()<b)return {allowed:false,until:b,limit:getDailyLimit()};const c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");const lim=getDailyLimit();if(c>=lim){const nb=Date.now()+48*60*60*1000;localStorage.setItem("gemalot_block_until",nb.toString());return {allowed:false,until:nb,limit:lim};}return {allowed:true,limit:lim};}
function incrementCount(){if(isSubscribed())return;let c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");c++;localStorage.setItem("gemalot_daily_count",c.toString());}
function applyLanguage(lang){currentLang=lang;t=translations[lang]||translations.ar;localStorage.setItem("gemalot_language",lang);document.documentElement.lang=lang;document.documentElement.dir=t.dir;document.body.dir=t.dir;const wt=document.getElementById("welcomeText");if(wt)wt.textContent=t.welcome;updatePlaceholder();updateLimitUI();renderSuggestions();}
function renderSuggestions(){const bar=document.getElementById("suggestionsBar");if(!bar)return;bar.innerHTML="";(t.suggestions||[]).forEach(s=>{const b=document.createElement("button");b.className="sug-btn";b.textContent=s;b.onclick=()=>sendSuggestion(s);bar.appendChild(b);});}
function triggerFullGlow(){const g=document.getElementById("fullGlow");g.classList.remove("active");void g.offsetWidth;g.classList.add("active");}
function getChats(){return JSON.parse(localStorage.getItem("gemalot_chats")||"[]");}
function saveChats(c){localStorage.setItem("gemalot_chats",JSON.stringify(c));}
function renderChatList(){const cl=document.getElementById("chatList");cl.innerHTML="";getChats().forEach(c=>{const div=document.createElement("div");div.className="chat-item";div.innerHTML=`<span class="chat-title">${c.title||"محادثة"}</span><button class="delete-btn">🗑️</button>`;div.querySelector('.chat-title').onclick=()=>loadChat(c.id);div.querySelector('.delete-btn').onclick=e=>{e.stopPropagation();deleteChat(c.id);};cl.appendChild(div);});}
function deleteChat(id){saveChats(getChats().filter(c=>c.id!==id));if(currentChatId===id)newChat();renderChatList();}
function newChat(){currentChatId=Date.now().toString();messages=[];document.getElementById("chatContainer").innerHTML=`<div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p id="welcomeText">${t.welcome}</p><div class="limit-info" id="limitInfo"></div></div>`;document.getElementById("sidebar").classList.remove("open");updateLimitUI();renderSuggestions();}
function loadChat(id){const chat=getChats().find(c=>c.id===id);if(!chat)return;currentChatId=id;messages=chat.messages||[];document.getElementById("chatContainer").innerHTML="";messages.forEach(m=>appendMessage(m.text,m.sender,false,m.source,m.html));document.getElementById("sidebar").classList.remove("open");}
function saveCurrentChat(){if(!messages.length)return;const chats=getChats();const title=messages[0]?.text?.slice(0,30)||"محادثة";const data={id:currentChatId||Date.now().toString(),title,messages,updated:Date.now()};const i=chats.findIndex(c=>c.id===currentChatId);if(i>-1)chats[i]=data;else chats.unshift(data);saveChats(chats);renderChatList();}
function toggleSidebar(){document.getElementById("sidebar").classList.toggle("open");}
function updateLimitUI(){checkAndResetDaily();const counter=document.getElementById("msgCounter");const subStatus=document.getElementById("subStatus");const subExpiry=document.getElementById("subExpiry");const limitInfo=document.getElementById("limitInfo");if(!counter)return;const lim=getDailyLimit();const sub=getSubscription();const count=parseInt(localStorage.getItem("gemalot_daily_count")||"0");const remaining=Math.max(0,lim-count);if(isSubscribed()){counter.textContent=`∞ ${sub?sub.plan.toUpperCase():"Premium"} (${sub?sub.dailyLimit:lim})`;if(subStatus)subStatus.textContent=`${t.premiumMode} - ${sub?sub.plan.toUpperCase():""} - ${sub?sub.dailyLimit:lim}/يوم`;const exp=localStorage.getItem("gemalot_subscription_expiry");if(subExpiry){if(exp&&parseInt(exp)>9999999999999)subExpiry.textContent="مدى الحياة";else if(exp)subExpiry.textContent="ينتهي: "+new Date(parseInt(exp)).toLocaleDateString();}if(limitInfo)limitInfo.innerHTML=`<span style="color:#22c55e">✓ ${t.premiumMode} - ${sub?sub.plan.toUpperCase():""}<br>${sub?sub.dailyLimit:lim} رسالة/يوم<br><small>بدون سيرفر - صور وفيديو باحترافية</small></span>`;}else{counter.textContent=`${remaining}/${lim}`;if(subStatus)subStatus.textContent=`${t.normalMode} - ${lim}/يوم`;const blockUntil=getBlockUntil();if(blockUntil&&Date.now()<blockUntil){const diff=blockUntil-Date.now();const h=Math.floor(diff/3600000);const m=Math.floor((diff%3600000)/60000);if(subExpiry)subExpiry.textContent=t.waitTime.replace("{h}",h).replace("{m}",m);if(limitInfo)limitInfo.innerHTML=`<span style="color:#ef4444">⛔ ${t.blockedTitle}<br>${t.waitTime.replace("{h}",h).replace("{m}",m)}</span>`;}else{if(subExpiry)subExpiry.textContent=t.remaining.replace("{n}",remaining).replace("{t}",lim);if(limitInfo)limitInfo.textContent=`${t.remaining.replace("{n}",remaining).replace("{t}",lim)} - بدون سيرفر`}}}
function selectModel(btn){document.querySelectorAll('.model-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');}
let messages=[];let currentChatId=null;
const chatContainer=document.getElementById('chatContainer');const userInput=document.getElementById('userInput');const sendBtn=document.getElementById('sendBtn');const fileInput=document.getElementById('fileInput');
function appendMessage(text,sender,save=true,source,htmlContent){
  const w=document.getElementById("welcomeBox");if(w)w.style.display="none";
  const div=document.createElement("div");div.className=`message ${sender}`;
  if(htmlContent){div.innerHTML=htmlContent;}
  else if(sender==="blocked"){div.innerHTML=text;}
  else{div.innerHTML="";const span=document.createElement("span");span.textContent=text;div.appendChild(span);if(source){const tag=document.createElement("span");tag.className="source-tag";tag.textContent=`المصدر: ${source}`;div.appendChild(document.createElement("br"));div.appendChild(tag);}}
  chatContainer.appendChild(div);window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  if(save&&sender!=="blocked"){messages.push({text,sender,source,html:htmlContent||null});if(!currentChatId)currentChatId=Date.now().toString();saveCurrentChat();}
}
function triggerUpload(type){fileInput.setAttribute("data-type",type);fileInput.accept=type==="image"?"image/*":type==="video"?"video/*":"image/*,video/*";fileInput.click();}
fileInput.addEventListener("change", async (e)=>{
  const file=e.target.files[0];if(!file)return;const url=URL.createObjectURL(file);
  if(file.type.startsWith("image/")){
    const html=`<div>🖼️ صورة: ${file.name}</div><img src="${url}" style="max-width:100%;border-radius:12px;margin-top:8px" onclick="openEditor(this.src)"><br><button onclick="openEditor('${url}')" style="margin-top:6px;padding:4px 8px;border-radius:8px;border:1px solid #facc15;background:transparent;color:#facc15;font-size:11px">تعديل احترافي</button>`;
    appendMessage(file.name,"user",true,null,html);
    appendMessage("تم رفع الصورة ✅\nتعديل: فلتر، دوران، نص، تحميل - بدون سيرفر","ai",true,"Image Upload");
  }else{
    const html=`<div>🎥 فيديو: ${file.name}</div><video src="${url}" controls style="max-width:100%;border-radius:12px;margin-top:8px"></video>`;
    appendMessage(file.name,"user",true,null,html);
    appendMessage("تم رفع الفيديو ✅ بدون سيرفر","ai",true,"Video Upload");
  }
});
function askImageGen(){const p=prompt("وصف الصورة:");if(p)processMessage("صورة: "+p);}
function askVideoGen(){const p=prompt("وصف الفيديو:");if(p)processMessage("فيديو: "+p);}
function insertPrompt(txt){userInput.value=txt;userInput.focus();updateSendIcon();}
function generateImagePollinations(prompt){const seed=Math.floor(Math.random()*100000);const enc=encodeURIComponent(prompt+" , ultra detailed, 4k, professional");return `https://image.pollinations.ai/prompt/${enc}?width=1024&height=1024&seed=${seed}&nologo=true&enhance=true`;}
function generateVideoCanvas(prompt){const canvas=document.createElement("canvas");canvas.width=640;canvas.height=360;const ctx=canvas.getContext("2d");let frame=0;return new Promise(resolve=>{const stream=canvas.captureStream(30);const recorder=new MediaRecorder(stream,{mimeType:"video/webm"});const chunks=[];recorder.ondataavailable=e=>chunks.push(e.data);recorder.onstop=()=>{const blob=new Blob(chunks,{type:"video/webm"});resolve(URL.createObjectURL(blob));};recorder.start();function draw(){ctx.fillStyle=`hsl(${(frame*2)%360},70%,30%)`;ctx.fillRect(0,0,640,360);ctx.fillStyle="#fff";ctx.font="bold 24px Arial";ctx.textAlign="center";ctx.fillText(prompt.slice(0,40),320,180);ctx.font="14px Arial";ctx.fillText(`Gemalot Video - ${frame}`,320,210);frame++;if(frame<60){requestAnimationFrame(draw);}else{setTimeout(()=>recorder.stop(),200);}}draw();});}
function solveMathPro(text){try{let expr=text.replace("حل المعادلة:","").replace("حل:","").replace("solve:","").trim();if(!expr)return null;if(expr.includes("=")){const parts=expr.split("=");if(parts.length===2){const left=parts[0].trim();const right=parts[1].trim();if(left.toLowerCase().includes("x")){try{const eq=math.parse(left+" - ("+right+")");for(let x=-100;x<=100;x+=0.5){const v=eq.evaluate({x});if(Math.abs(v)<0.001){return `🧮 الحل العفوي:\n${expr}\n\nالخطوات:\n${left} = ${right}\n${left} - ${right} = 0\nنجرب x=${x}\nالتحقق: ${math.evaluate(left,{x})} ≈ ${right}\n\nالجواب: x = ${x} ✅`;}}}catch{}}try{const r=math.evaluate(right);const l=math.evaluate(left);return `اليمين = ${r}\nاليسار = ${l}`;}catch{}}}const res=math.evaluate(expr);return `🧮 حل عفوي:\n${expr} = ${res}\nالخطوات: طبقت الأولويات → الناتج ${res} ✅`;}catch{return null;}}
async function safeFetch(url){try{const r=await fetch(url);if(!r.ok)return null;return r;}catch{return null;}}
async function searchWikipediaLang(q,lang){try{let res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);if(res){const d=await res.json();if(d.extract&&d.extract.length>40)return {text:d.extract, source:`Wikipedia ${lang}`};}res=await safeFetch(`https://${lang}.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(q)}&format=json&origin=*`);if(res){const d=await res.json();const first=d.query?.search?.[0]?.title;if(first){res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(first)}`);if(res){const dd=await res.json();if(dd.extract)return {text:dd.extract, source:`Wikipedia ${lang}`};}}}}catch{}return null;}
async function searchWiktionary(q,lang){try{const res=await safeFetch(`https://${lang}.wiktionary.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(q)}&format=json&origin=*`);if(res){const d=await res.json();const f=d.query?.search?.[0];if(f?.snippet){let t=f.snippet.replace(/<[^>]*>/g,"");if(t.length>30)return {text:t, source:`Wiktionary ${lang}`};}}}catch{}return null;}
async function searchDuckDuckGo(q){try{const url=`https://api.duckduckgo.com/?q=${encodeURIComponent(q)}&format=json&pretty=1&no_html=1`;const proxy=`https://api.allorigins.win/get?url=${encodeURIComponent(url)}`;const res=await safeFetch(proxy);if(!res)return null;const w=await res.json();const data=JSON.parse(w.contents);let text=data.AbstractText||data.Abstract||data.RelatedTopics?.[0]?.Text||"";if(text.length>40)return {text, source:"DuckDuckGo"};}catch{}return null;}
async function searchWikidata(q){try{const url=`https://www.wikidata.org/w/api.php?action=wbsearchentities&search=${encodeURIComponent(q)}&language=${currentLang}&format=json&origin=*`;const res=await safeFetch(url);if(res){const d=await res.json();const f=d.search?.[0];if(f?.description)return {text:`${f.label}: ${f.description}`, source:"Wikidata"};}}catch{}return null;}
async function searchQuran(q){try{if(q.length<100){const res=await safeFetch(`https://api.alquran.cloud/v1/search/${encodeURIComponent(q)}/all/ar`);if(res){const d=await res.json();const a=d.data?.matches?.[0]?.text;if(a)return {text:a, source:"Quran.com"};}}}catch{}return null;}
async function searchOpenSearch(q,lang){try{const url=`https://${lang}.wikipedia.org/w/api.php?action=opensearch&search=${encodeURIComponent(q)}&limit=1&format=json&origin=*`;const res=await safeFetch(url);if(res){const d=await res.json();if(d[2]?.[0])return {text:d[2][0], source:`OpenSearch ${lang}`};}}catch{}return null;}
async function searchAllSources(q){
  const sources=[()=>searchWikipediaLang(q,"ar"),()=>searchWikipediaLang(q,"en"),()=>searchWikipediaLang(q,"fr"),()=>searchWikipediaLang(q,"es"),()=>searchWikipediaLang(q,"tr"),()=>searchWikipediaLang(q,"de"),()=>searchWikipediaLang(q,"ru"),()=>searchWikipediaLang(q,"id"),()=>searchWikipediaLang(q,"ur"),()=>searchWikipediaLang(q,"fa"),()=>searchWiktionary(q,"ar"),()=>searchWiktionary(q,"en"),()=>searchDuckDuckGo(q),()=>searchWikidata(q),()=>searchQuran(q),()=>searchOpenSearch(q,"ar"),()=>searchOpenSearch(q,"en"),()=>searchOpenSearch(q,"fr"),()=>searchWikipediaLang(q,"ms"),()=>searchWikipediaLang(q,"bn"),()=>searchWikipediaLang(q,"hi"),()=>searchWikipediaLang(q,"ja"),()=>searchWikipediaLang(q,"zh"),()=>searchWiktionary(q,"de"),()=>searchDuckDuckGo(q+" islam"),()=>searchDuckDuckGo(q+" معنى"),()=>searchQuran(q+" الله")];
  for(let batch of [sources.slice(0,7),sources.slice(7,14),sources.slice(14,21),sources.slice(21)]){
    const results=await Promise.all(batch.map(fn=>fn()));
    const found=results.find(r=>r&&r.text&&r.text.length>30);
    if(found)return found;
  }
  return null;
}
const chatKnowledge=[{roots:["الوضوء","طهارة"],reply:"نواقض الوضوء: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، أكل لحم الإبل."},{roots:["الصلاة"],reply:"الصلاة عماد الدين وشروطها الطهارة ودخول الوقت وستر العورة واستقبال القبلة."},{roots:["الصيام","رمضان"],reply:"الصيام ركن وشروطه الإسلام والبلوغ والعقل والإقامة والصحة."},{roots:["زكاة"],reply:"تجب الزكاة إذا بلغ النصاب وحال الحول 2.5%."}];
async function processMessage(text){
  if(!text)text=userInput.value.trim();if(!text)return;
  const check=canSendMessage();if(!check.allowed){const until=check.until;const diff=until-Date.now();const h=Math.floor(diff/3600000);const m=Math.floor((diff%3600000)/60000);const msg=`<b>${t.blockedTitle}</b><br><br>${t.blockedMsg.replace("{t}",check.limit)}<br><br>${t.waitTime.replace("{h}",h).replace("{m}",m)}<br><br><a href="subscription.html" style="color:#facc15">${t.subscribeNow}</a>`;appendMessage(msg,"blocked",false);updateLimitUI();return;}
  triggerFullGlow();
  const lower=text.toLowerCase();
  if(lower.startsWith("صورة:")||lower.startsWith("صوره:")||lower.startsWith("image:")||lower.startsWith("انشئ صورة")){
    let prompt=text.replace(/صورة:|صوره:|image:|انشئ صورة:/i,"").trim();if(!prompt)prompt="beautiful landscape";
    appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();
    appendMessage(`🎨 جاري إنشاء صورة: "${prompt}"`,"ai",false);
    const imgUrl=generateImagePollinations(prompt);
    const html=`<div>🎨 ${prompt}</div><img src="${imgUrl}" loading="lazy" style="width:100%;border-radius:12px;margin-top:8px" onclick="openEditor(this.src)"><br><div style="margin-top:6px;display:flex;gap:6px"><button onclick="openEditor('${imgUrl}')" style="padding:4px 8px;border-radius:8px;border:1px solid #facc15;background:transparent;color:#facc15;font-size:11px">تعديل</button><a href="${imgUrl}" target="_blank" style="padding:4px 8px;border-radius:8px;background:#facc15;color:#111;text-decoration:none;font-size:11px">تحميل 4K</a></div>`;
    appendMessage("", "ai", true, "Pollinations AI", html);
    if(lastVoiceInput){speakText("تم إنشاء الصورة "+prompt);lastVoiceInput=false;}
    return;
  }
  if(lower.startsWith("فيديو:")||lower.startsWith("video:")||lower.startsWith("انشئ فيديو")){
    let prompt=text.replace(/فيديو:|video:|انشئ فيديو:/i,"").trim();if(!prompt)prompt="sunset";
    appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();
    appendMessage(`🎬 جاري إنشاء فيديو: "${prompt}"`,"ai",false);
    const videoUrl=await generateVideoCanvas(prompt);
    const html=`<div>🎬 ${prompt}</div><video src="${videoUrl}" controls autoplay loop style="width:100%;border-radius:12px;margin-top:8px"></video><br><a href="${videoUrl}" download="gemalot_video.webm" style="display:inline-block;margin-top:6px;padding:4px 8px;border-radius:8px;background:#facc15;color:#111;text-decoration:none;font-size:11px">تحميل</a>`;
    appendMessage("", "ai", true, "Canvas Video", html);
    if(lastVoiceInput){speakText("تم إنشاء الفيديو "+prompt);lastVoiceInput=false;}
    return;
  }
  if(lower.includes("حل")||lower.match(/[0-9x+\-*/^=]/)){
    const mr=solveMathPro(text);if(mr){appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();appendMessage(mr,"ai",true,"Math.js");
    if(lastVoiceInput){speakText(mr);lastVoiceInput=false;}
    return;}
  }
  appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();updateSendIcon();
  let reply=null,source=null;for(const item of chatKnowledge){for(const root of item.roots){if(text.toLowerCase().includes(root.toLowerCase())){reply=item.reply;source="Gemalot DB";break;}}if(reply)break;}
  if(!reply){appendMessage(t.searching,"ai",false);const web=await searchAllSources(text);const last=chatContainer.lastChild;if(last&&last.textContent===t.searching)last.remove();if(web){reply=web.text;source=web.source;}else{reply=t.noAnswer;}}
  appendMessage(reply,"ai",true,source);
  if(lastVoiceInput){speakText(reply);lastVoiceInput=false;}
  updateLimitUI();
}
function sendSuggestion(txt){processMessage(txt);}
function toggleTools(){document.getElementById('toolsPopup').classList.toggle('open');}
function closeTools(){document.getElementById('toolsPopup').classList.remove('open');}
document.addEventListener('click',e=>{const pop=document.getElementById('toolsPopup');const plus=document.getElementById('plusBtn');if(pop&&plus&&!pop.contains(e.target)&&e.target!==plus)pop.classList.remove('open');});
function updateSendIcon(){
  const val=userInput.value.trim();
  const icon=document.getElementById('sendIcon');const mic=document.getElementById('micBtn');const btn=document.getElementById('sendBtn');
  if(val.length>0){icon.textContent='↑';btn.classList.remove('wave');btn.classList.add('active');mic.style.display='none';}
  else{icon.textContent='◍';btn.classList.remove('active');btn.classList.add('wave');mic.style.display='flex';}
}
function updatePlaceholder(){if(currentLang==='ar'){userInput.placeholder=t.placeholderAr||'اسأل Gemalot';}else{userInput.placeholder=t.placeholder||'Ask Gemalot';}}
let recognition=null;let voiceMode=false;let lastVoiceInput=false;
function speakText(text){
  if(!('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  let clean=text.replace(/<[^>]*>/g,'').replace(/[🎨🎬🧮✅🔍]/g,'').slice(0,300);
  const utter=new SpeechSynthesisUtterance(clean);
  utter.lang=currentLang==='ar'?'ar-SA':'en-US';
  utter.rate=1;utter.pitch=1;
  window.speechSynthesis.speak(utter);
}
function toggleMic(){
  if(!('webkitSpeechRecognition' in window)&&!('SpeechRecognition' in window)){alert('المتصفح لا يدعم المايك - جرب Chrome');return;}
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(recognition){recognition.stop();recognition=null;document.getElementById('micBtn').textContent='🎙️';document.getElementById('micBtn').style.background='transparent';return;}
  recognition=new SR();recognition.lang=currentLang==='ar'?'ar-SA':'en-US';recognition.interimResults=false;recognition.maxAlternatives=1;
  recognition.start();
  voiceMode=true;lastVoiceInput=true;
  document.getElementById('micBtn').textContent='🔴';document.getElementById('micBtn').style.background='#fee2e2';
  recognition.onresult=e=>{
    const transcript=e.results[0][0].transcript;
    userInput.value=transcript;
    updateSendIcon();
    setTimeout(()=>{processMessage(transcript);},600);
  };
  recognition.onerror=e=>{console.log(e);document.getElementById('micBtn').textContent='🎙️';document.getElementById('micBtn').style.background='transparent';recognition=null;};
  recognition.onend=()=>{if(recognition){recognition=null;document.getElementById('micBtn').textContent='🎙️';document.getElementById('micBtn').style.background='transparent';}};
}
if(userInput){
  userInput.addEventListener("focus",()=>{triggerFullGlow();closeTools();});
  userInput.addEventListener("input",updateSendIcon);
  userInput.addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();processMessage();}});
}
if(sendBtn)sendBtn.addEventListener("click",()=>processMessage());
document.getElementById('settingsBtn').addEventListener('click',()=>{location.href='settings.html';});
applyLanguage(currentLang);renderChatList();updateLimitUI();renderSuggestions();updatePlaceholder();updateSendIcon();setInterval(updateLimitUI,60000);
let currentImage=null;const editModal=document.getElementById("editModal");const editCanvas=document.getElementById("editCanvas");const ctx=editCanvas.getContext("2d");
function openEditor(src){editModal.style.display="flex";currentImage=new Image();currentImage.crossOrigin="anonymous";currentImage.onload=()=>{editCanvas.width=currentImage.width;editCanvas.height=currentImage.height;ctx.drawImage(currentImage,0,0);};currentImage.src=src;}
function closeEditor(){editModal.style.display="none";}
function applyFilter(type){const imageData=ctx.getImageData(0,0,editCanvas.width,editCanvas.height);const data=imageData.data;for(let i=0;i<data.length;i+=4){if(type==="grayscale"){const avg=(data[i]+data[i+1]+data[i+2])/3;data[i]=data[i+1]=data[i+2]=avg;}else if(type==="sepia"){data[i]=Math.min(255,data[i]*0.393+data[i+1]*0.769+data[i+2]*0.189);data[i+1]=Math.min(255,data[i]*0.349+data[i+1]*0.686+data[i+2]*0.168);data[i+2]=Math.min(255,data[i]*0.272+data[i+1]*0.534+data[i+2]*0.131);}else if(type==="invert"){data[i]=255-data[i];data[i+1]=255-data[i+1];data[i+2]=255-data[i+2];}else if(type==="bright"){data[i]=Math.min(255,data[i]+30);data[i+1]=Math.min(255,data[i+1]+30);data[i+2]=Math.min(255,data[i+2]+30);}}ctx.putImageData(imageData,0,0);}
function rotateCanvas(deg){const tmp=document.createElement("canvas");tmp.width=editCanvas.height;tmp.height=editCanvas.width;const tctx=tmp.getContext("2d");tctx.translate(tmp.width/2,tmp.height/2);tctx.rotate(deg*Math.PI/180);tctx.drawImage(editCanvas,-editCanvas.width/2,-editCanvas.height/2);editCanvas.width=tmp.width;editCanvas.height=tmp.height;ctx.drawImage(tmp,0,0);}
function addTextOverlay(){const txt=prompt("اكتب النص:");if(!txt)return;ctx.fillStyle="#facc15";ctx.font="bold 40px Arial";ctx.fillText(txt,30,50);}
function downloadCanvas(){const link=document.createElement("a");link.download="gemalot_edited.png";link.href=editCanvas.toDataURL();link.click();}
</script>
</body>
</html>
