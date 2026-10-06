<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head><style>
@font-face {
  font-family: "Optimistic";
  font-style: normal;
  font-weight: 400 600;
  font-display: swap;
  src: url("/fonts/OptimisticAI_VF_Optimized.woff2") format("woff2");
}
@font-face {
  font-family: "Optimistic Mono";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("/fonts/OptimisticMono_W_TextRegular.woff2") format("woff2");
}
:where(html) {
  font-family: "Optimistic", system-ui, sans-serif;
}
:where(code, pre, kbd, samp) {
  font-family: "Optimistic Mono", ui-monospace, monospace;
}
</style>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>Gemalot</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0b0f19;color:#fff;font-family:Arial,sans-serif;min-height:100vh;display:flex;flex-direction:column}
.header{height:56px;background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);display:flex;align-items:center;justify-content:space-between;padding:0 14px;position:fixed;top:0;left:0;width:100%;z-index:1000}
.header-title{font-size:18px;font-weight:bold}
.header-actions{display:flex;gap:8px;align-items:center}
.menu-btn{width:38px;height:38px;background:rgba(0,0,0,.25);border:none;border-radius:10px;cursor:pointer;display:flex;flex-direction:column;justify-content:center;align-items:center;gap:4px;color:#fff;font-size:16px}
.menu-btn span{display:block;width:18px;height:2.5px;background:#fff;border-radius:2px}
.counter-badge{background:rgba(0,0,0,.4);padding:4px 10px;border-radius:12px;font-size:12px;color:#facc15;font-weight:bold;border:1px solid rgba(250,204,21,.3)}
.models-bar{position:fixed;top:56px;left:0;width:100%;display:flex;gap:8px;padding:10px 12px;overflow-x:auto;background:#0b0f19;border-bottom:1px solid #1f2937;z-index:950}
.model-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1.5px solid rgba(255,255,255,.15);background:transparent;color:#e5e7eb;font-size:12px;font-weight:600;cursor:pointer;white-space:nowrap}
.model-btn.active{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#111;border-color:transparent}
.model-btn.premium{background:#1f2937;color:#facc15;border-color:#facc15}
.sidebar{position:fixed;top:108px;left:0;width:280px;height:calc(100vh - 108px);background:#111827;border-right:1px solid #1f2937;z-index:900;transform:translateX(-100%);transition:transform .3s;overflow-y:auto;padding:14px}
.sidebar.open{transform:translateX(0)}.sidebar h3{font-size:14px;margin-bottom:10px;color:#facc15}
.chat-item{display:flex;align-items:center;justify-content:space-between;padding:10px;border-radius:10px;background:#1f2937;margin-bottom:6px;font-size:13px}
.chat-item .chat-title{flex:1;cursor:pointer;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.delete-btn{background:none;border:none;color:#ef4444;font-size:15px;cursor:pointer;padding:2px 6px}
.new-chat-btn{width:100%;padding:10px;border:none;border-radius:10px;background:linear-gradient(90deg,#facc15,#f97316);color:#111;font-weight:bold;cursor:pointer;margin-bottom:12px}
.chat-container{flex:1;width:100%;max-width:800px;margin:108px auto 140px;padding:16px;display:flex;flex-direction:column}
.welcome-box{text-align:center;margin:auto;padding:40px 0}.welcome-box h1{font-size:26px;margin-bottom:8px}.welcome-box .line{width:40px;height:2px;background:#4b5563;margin:0 auto 10px}.welcome-box p{color:#9ca3af;font-size:13px}.welcome-box .limit-info{margin-top:16px;padding:12px;background:#111827;border-radius:12px;border:1px solid #1f2937;font-size:12px;line-height:1.6}
.message{max-width:85%;padding:12px 14px;margin:8px 0;border-radius:16px;font-size:14px;line-height:1.6;word-wrap:break-word;white-space:pre-wrap}
.message.user{background:linear-gradient(90deg,#facc15,#f97316,#22c55e,#3b82f6);color:#fff;font-weight:bold;margin-right:auto;border-bottom-left-radius:4px}
.message.ai{background:#fff;color:#111;margin-left:auto;border-bottom-right-radius:4px}
.message.blocked{background:#7f1d1d;color:#fff;margin:0 auto;max-width:95%;text-align:center;border-radius:12px}
.source-tag{font-size:10px;color:#6b7280;margin-top:8px;display:block;border-top:1px solid #e5e7eb;padding-top:4px}
.suggestions{position:fixed;bottom:70px;left:0;width:100%;display:flex;gap:8px;padding:8px 12px;overflow-x:auto;background:#0b0f19;z-index:999}
.sug-btn{flex-shrink:0;padding:8px 14px;border-radius:20px;border:1px solid rgba(255,255,255,.15);background:rgba(255,255,255,.06);color:#e5e7eb;font-size:12px;cursor:pointer;white-space:nowrap}
.input-area{position:fixed;bottom:0;left:0;width:100%;padding:10px 14px;background:#0b0f19;z-index:1000}
.input-wrapper{max-width:700px;margin:0 auto;display:flex;align-items:center;background:#fff;border-radius:30px;padding:4px 6px 4px 14px}
.input-wrapper.disabled{opacity:.4;pointer-events:none}
.input-wrapper textarea{flex:1;border:none;outline:none;resize:none;font-size:14px;color:#111;max-height:80px;padding:8px 0;background:transparent}
.send-btn{width:46px;height:46px;border-radius:50%;border:none;background:linear-gradient(90deg,#facc15,#f97316,#22c55e);color:#fff;font-size:12px;font-weight:bold;cursor:pointer;flex-shrink:0}
.full-glow{position:fixed;inset:0;pointer-events:none;z-index:9999;opacity:0;background:linear-gradient(120deg,#facc15 0%,#f97316 25%,#22c55e 50%,#3b82f6 75%,#facc15 100%);background-size:400% 400%;mix-blend-mode:screen}
.full-glow.active{animation:fullGlowFlow 2s ease-in-out forwards}
@keyframes fullGlowFlow{0%{background-position:0% 50%;opacity:.75}25%{background-position:40% 50%;opacity:.9}50%{background-position:80% 50%;opacity:.8}100%{background-position:0% 50%;opacity:0}}
</style>
</head>
<body>
<div class="header"><div class="header-title">Gemalot</div><div class="header-actions"><div id="msgCounter" class="counter-badge">5/5</div><button id="settingsBtn" class="menu-btn">⚙️</button><button class="menu-btn" onclick="toggleSidebar()"><span></span><span></span><span></span></button></div></div>
<div class="models-bar"><button class="model-btn active" onclick="selectModel(this)">Gemalot Normal</button><button class="model-btn premium" onclick="location.href='subscription.html'">Gemalot Plus 🔒</button><button class="model-btn premium" onclick="location.href='subscription.html'">Gemalot Bronze 🔒</button><button class="model-btn premium" onclick="location.href='subscription.html'">Gemalot Silver 🔒</button><button class="model-btn premium" onclick="location.href='subscription.html'">Gemalot Gold 🔒</button></div>
<div id="sidebar" class="sidebar"><button id="newChatBtn" class="new-chat-btn" onclick="newChat()">+ محادثة جديدة</button><h3 id="prevTitle">المحادثات السابقة</h3><div id="chatList"></div><div style="margin-top:20px;padding:10px;background:#0b0f19;border-radius:10px;font-size:11px;color:#9ca3af"><div id="subStatus">الوضع: عادي (5 رسائل/يوم)</div><div id="subExpiry" style="margin-top:4px"></div><div style="margin-top:8px;font-size:10px;color:#6b7280">التفعيل يتم من طرف الإدارة فقط</div></div></div>
<div id="chatContainer" class="chat-container"><div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p id="welcomeText">كيف يمكنني مساعدتك؟</p><div class="limit-info" id="limitInfo"></div></div></div>
<div id="suggestionsBar" class="suggestions"></div>
<div class="input-area"><div id="inputWrapper" class="input-wrapper"><textarea id="userInput" placeholder="اكتب سؤالك هنا..." rows="1"></textarea><button id="sendBtn" class="send-btn">إرسال</button></div></div>
<div id="fullGlow" class="full-glow"></div>
<script>
const translations={ar:{dir:"rtl",welcome:"كيف يمكنني مساعدتك؟",placeholder:"اكتب سؤالك هنا...",newChat:"+ محادثة جديدة",prevChats:"المحادثات السابقة",send:"إرسال",suggestions:["نواقض الوضوء","أحكام الصيام","أركان الصلاة","الزكاة","السلام عليكم"],noAnswer:"عذراً، لم أجد إجابة.",searching:"جاري البحث في 10 مصادر...",normalMode:"الوضع: عادي (5 رسائل/يوم)",premiumMode:"الوضع: مميز ∞",remaining:"متبقي: {n}/5 اليوم",blockedTitle:"انتهت رسائلك اليومية",blockedMsg:"استهلكت 5 رسائل اليوم. انتظر 48 ساعة أو تواصل مع الإدارة للتفعيل.",subscribeNow:"طلب اشتراك",waitTime:"الوقت المتبقي: {h} ساعة و {m} دقيقة"},en:{dir:"ltr",welcome:"How can I help you?",placeholder:"Type your question...",newChat:"+ New Chat",prevChats:"Previous Chats",send:"Send",suggestions:["Pillars of Islam","Fasting rules","Prayer pillars","Zakat","Peace"],noAnswer:"Sorry, no answer found.",searching:"Searching 10 sources...",normalMode:"Mode: Normal (5/day)",premiumMode:"Mode: Premium ∞",remaining:"Remaining: {n}/5 today",blockedTitle:"Daily limit reached",blockedMsg:"You used 5 messages today. Wait 48h or contact admin.",subscribeNow:"Request",waitTime:"Time left: {h}h {m}m"}};
let currentLang=localStorage.getItem("gemalot_language")||"ar";let t=translations[currentLang]||translations.ar;
function applyLanguage(lang){currentLang=lang;t=translations[lang]||translations.ar;localStorage.setItem("gemalot_language",lang);document.documentElement.lang=lang;document.documentElement.dir=t.dir;document.body.dir=t.dir;const wt=document.getElementById("welcomeText");if(wt)wt.textContent=t.welcome;const inp=document.getElementById("userInput");if(inp){inp.placeholder=t.placeholder;inp.style.direction=t.dir;}const ncb=document.getElementById("newChatBtn");if(ncb)ncb.textContent=t.newChat;const pt=document.getElementById("prevTitle");if(pt)pt.textContent=t.prevChats;const sb=document.getElementById("sendBtn");if(sb)sb.textContent=t.send;updateLimitUI();renderSuggestions();}
function renderSuggestions(){const bar=document.getElementById("suggestionsBar");if(!bar)return;bar.innerHTML="";(t.suggestions||[]).forEach(s=>{const b=document.createElement("button");b.className="sug-btn";b.textContent=s;b.onclick=()=>sendSuggestion(s);bar.appendChild(b);});}
function getTodayStr(){return new Date().toISOString().split('T')[0];}
function checkAndResetDaily(){const d=localStorage.getItem("gemalot_daily_date");const today=getTodayStr();if(d!==today){localStorage.setItem("gemalot_daily_date",today);localStorage.setItem("gemalot_daily_count","0");localStorage.removeItem("gemalot_block_until");}}
function isSubscribed(){const a=localStorage.getItem("gemalot_subscription_active")==="true";const e=localStorage.getItem("gemalot_subscription_expiry");const currentUser=localStorage.getItem("alislamiah_current_user");const allSubs=JSON.parse(localStorage.getItem("gemalot_all_subscriptions")||"{}");if(currentUser&&allSubs[currentUser]){const sub=allSubs[currentUser];if(sub.expiry&&Date.now()<parseInt(sub.expiry)){localStorage.setItem("gemalot_subscription_active","true");localStorage.setItem("gemalot_subscription_expiry",sub.expiry);return true;}}if(!a)return false;if(!e)return true;return Date.now()<parseInt(e);}
function getBlockUntil(){return parseInt(localStorage.getItem("gemalot_block_until")||"0");}
function canSendMessage(){checkAndResetDaily();if(isSubscribed())return{allowed:true};const b=getBlockUntil();if(b&&Date.now()<b){return{allowed:false,until:b};}const c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");if(c>=5){const nb=Date.now()+(48*60*60*1000);localStorage.setItem("gemalot_block_until",nb.toString());return{allowed:false,until:nb};}return{allowed:true};}
function incrementCount(){if(isSubscribed())return;let c=parseInt(localStorage.getItem("gemalot_daily_count")||"0");c++;localStorage.setItem("gemalot_daily_count",c.toString());}
function updateLimitUI(){checkAndResetDaily();const counter=document.getElementById("msgCounter");const subStatus=document.getElementById("subStatus");const subExpiry=document.getElementById("subExpiry");const limitInfo=document.getElementById("limitInfo");const wrapper=document.getElementById("inputWrapper");if(!counter)return;if(isSubscribed()){counter.textContent="∞ Premium";counter.style.background="linear-gradient(90deg,#facc15,#f97316)";counter.style.color="#111";if(subStatus)subStatus.textContent=t.premiumMode;const exp=localStorage.getItem("gemalot_subscription_expiry");if(subExpiry){if(exp&&parseInt(exp)>9999999999999)subExpiry.textContent="مدى الحياة";else if(exp)subExpiry.textContent="ينتهي: "+new Date(parseInt(exp)).toLocaleDateString();else subExpiry.textContent="مدى الحياة";}if(limitInfo)limitInfo.innerHTML=`<span style="color:#22c55e">✓ ${t.premiumMode} - غير محدود<br><small>تم التفعيل من طرف الإدارة</small></span>`;if(wrapper)wrapper.classList.remove("disabled");}else{const count=parseInt(localStorage.getItem("gemalot_daily_count")||"0");const remaining=Math.max(0,5-count);counter.textContent=`${remaining}/5`;counter.style.background="rgba(0,0,0,.4)";counter.style.color=remaining>0?"#facc15":"#ef4444";if(subStatus)subStatus.textContent=t.normalMode;const blockUntil=getBlockUntil();if(blockUntil&&Date.now()<blockUntil){const diff=blockUntil-Date.now();const h=Math.floor(diff/3600000);const m=Math.floor((diff%3600000)/60000);if(subExpiry)subExpiry.textContent=t.waitTime.replace("{h}",h).replace("{m}",m);if(limitInfo)limitInfo.innerHTML=`<span style="color:#ef4444">⛔ ${t.blockedTitle}<br>${t.waitTime.replace("{h}",h).replace("{m}",m)}<br><a href="subscription.html" style="color:#facc15">${t.subscribeNow}</a></span>`;if(wrapper)wrapper.classList.add("disabled");}else{if(subExpiry)subExpiry.textContent=t.remaining.replace("{n}",remaining);if(limitInfo)limitInfo.textContent=t.remaining.replace("{n}",remaining);if(wrapper)wrapper.classList.remove("disabled");}}}
async function safeFetch(url){try{const r=await fetch(url);if(!r.ok)return null;return r;}catch{return null;}}
async function searchWikipediaLang(q,lang){try{let res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(q)}`);if(res){const d=await res.json();if(d.extract&&d.extract.length>40)return{text:d.extract,source:`Wikipedia ${lang.toUpperCase()}`};}res=await safeFetch(`https://${lang}.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(q)}&format=json&origin=*`);if(res){const d=await res.json();const first=d.query?.search?.[0]?.title;if(first){res=await safeFetch(`https://${lang}.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(first)}`);if(res){const dd=await res.json();if(dd.extract)return{text:dd.extract,source:`Wikipedia ${lang.toUpperCase()}`};}}}}catch{}return null;}
async function searchDuckDuckGo(q){try{const url=`https://api.duckduckgo.com/?q=${encodeURIComponent(q)}&format=json&pretty=1&no_html=1&skip_disambig=1`;const proxy=`https://api.allorigins.win/get?url=${encodeURIComponent(url)}`;const res=await safeFetch(proxy);if(!res)return null;const wrapper=await res.json();const data=JSON.parse(wrapper.contents);let text=data.AbstractText||data.Abstract||data.RelatedTopics?.[0]?.Text||"";if(text.length>40)return{text,source:"DuckDuckGo"};}catch{}return null;}
async function searchWikidata(q){try{const url=`https://www.wikidata.org/w/api.php?action=wbsearchentities&search=${encodeURIComponent(q)}&language=${currentLang}&format=json&origin=*`;const res=await safeFetch(url);if(res){const d=await res.json();const first=d.search?.[0];if(first?.description)return{text:`${first.label}: ${first.description}`,source:"Wikidata"};}}catch{}return null;}
async function searchQuran(q){try{if(/قرآن|آية|سورة|quran|ayah/i.test(q)||q.length<40){const res=await safeFetch(`https://api.alquran.cloud/v1/search/${encodeURIComponent(q)}/all/ar`);if(res){const d=await res.json();const ayah=d.data?.matches?.[0]?.text;if(ayah)return{text:ayah,source:"Quran.com"};}}}catch{}return null;}
async function searchOpenSearch(q,lang){try{const url=`https://${lang}.wikipedia.org/w/api.php?action=opensearch&search=${encodeURIComponent(q)}&limit=1&format=json&origin=*`;const res=await safeFetch(url);if(res){const d=await res.json();if(d[2]?.[0])return{text:d[2][0],source:`OpenSearch ${lang.toUpperCase()}`};}}catch{}return null;}
async function searchAllSources(q){const sources=[()=>searchWikipediaLang(q,currentLang),()=>searchWikipediaLang(q,"ar"),()=>searchWikipediaLang(q,"en"),()=>searchWikipediaLang(q,"fr"),()=>searchWikipediaLang(q,"es"),()=>searchDuckDuckGo(q),()=>searchWikidata(q),()=>searchOpenSearch(q,currentLang),()=>searchOpenSearch(q,"en"),()=>searchQuran(q)];for(let batch of[sources.slice(0,5),sources.slice(5)]){const results=await Promise.all(batch.map(fn=>fn()));const found=results.find(r=>r&&r.text&&r.text.length>30);if(found)return found;}return null;}
const chatKnowledge=[{roots:["الوضوء","طهارة"],reply:{ar:"نواقض الوضوء: الخارج من السبيلين، النوم العميق، زوال العقل، مس الفرج بشهوة، وأكل لحم الإبل."}},{roots:["الصلاة"],reply:{ar:"الصلاة عماد الدين وشروطها الطهارة ودخول الوقت وستر العورة واستقبال القبلة."}},{roots:["الصيام","رمضان"],reply:{ar:"الصيام ركن وشروطه الإسلام والبلوغ والعقل والإقامة والصحة."}},{roots:["زكاة"],reply:{ar:"تجب الزكاة إذا بلغ النصاب وحال الحول بنسبة 2.5%."}}];
const chatContainer=document.getElementById('chatContainer');const userInput=document.getElementById('userInput');const sendBtn=document.getElementById('sendBtn');const fullGlow=document.getElementById('fullGlow');const chatList=document.getElementById('chatList');let currentChatId=null,messages=[];
function selectModel(btn){if(btn.classList.contains('premium')){location.href='subscription.html';return;}document.querySelectorAll('.model-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');}
function triggerFullGlow(){fullGlow.classList.remove('active');void fullGlow.offsetWidth;fullGlow.classList.add('active');}
function getChats(){return JSON.parse(localStorage.getItem("gemalot_chats")||"[]");}
function saveChats(c){localStorage.setItem("gemalot_chats",JSON.stringify(c));}
function renderChatList(){chatList.innerHTML="";getChats().forEach(c=>{const div=document.createElement("div");div.className="chat-item";div.innerHTML=`<span class="chat-title">${c.title||"محادثة"}</span><button class="delete-btn">🗑️</button>`;div.querySelector('.chat-title').onclick=()=>loadChat(c.id);div.querySelector('.delete-btn').onclick=e=>{e.stopPropagation();deleteChat(c.id);};chatList.appendChild(div);});}
function deleteChat(id){saveChats(getChats().filter(c=>c.id!==id));if(currentChatId===id)newChat();renderChatList();}
function newChat(){currentChatId=Date.now().toString();messages=[];chatContainer.innerHTML=`<div id="welcomeBox" class="welcome-box"><h1>Gemalot</h1><div class="line"></div><p id="welcomeText">${t.welcome}</p><div class="limit-info" id="limitInfo"></div></div>`;const sb=document.getElementById("sidebar");if(sb)sb.classList.remove("open");updateLimitUI();renderSuggestions();}
function loadChat(id){const chat=getChats().find(c=>c.id===id);if(!chat)return;currentChatId=id;messages=chat.messages||[];chatContainer.innerHTML="";messages.forEach(m=>appendMessage(m.text,m.sender,false,m.source));document.getElementById("sidebar").classList.remove("open");}
function saveCurrentChat(){if(!messages.length)return;const chats=getChats();const title=messages[0]?.text?.slice(0,30)||"محادثة";const data={id:currentChatId||Date.now().toString(),title,messages,updated:Date.now()};const i=chats.findIndex(c=>c.id===currentChatId);if(i>-1)chats[i]=data;else chats.unshift(data);saveChats(chats);renderChatList();}
function toggleSidebar(){document.getElementById("sidebar").classList.toggle("open");}
function appendMessage(text,sender,save=true,source){const w=document.getElementById("welcomeBox");if(w)w.style.display="none";const div=document.createElement("div");div.className=`message ${sender}`;if(sender==="blocked"){div.innerHTML=text;}else{div.textContent=text;if(sender==="ai"&&source){const tag=document.createElement("span");tag.className="source-tag";tag.textContent=`المصدر: ${source}`;div.appendChild(document.createElement("br"));div.appendChild(tag);}}chatContainer.appendChild(div);window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});if(save&&sender!=="blocked"){messages.push({text,sender,source});if(!currentChatId)currentChatId=Date.now().toString();saveCurrentChat();}}
async function processMessage(text){if(!text)text=userInput.value.trim();if(!text)return;const check=canSendMessage();if(!check.allowed){const until=check.until;const diff=until-Date.now();const h=Math.floor(diff/3600000);const m=Math.floor((diff%3600000)/60000);const msg=`<b>${t.blockedTitle}</b><br><br>${t.blockedMsg}<br><br>${t.waitTime.replace("{h}",h).replace("{m}",m)}<br><br><a href="subscription.html" style="color:#facc15;font-weight:bold">${t.subscribeNow}</a>`;appendMessage(msg,"blocked",false);updateLimitUI();return;}triggerFullGlow();appendMessage(text,"user");userInput.value="";incrementCount();updateLimitUI();let reply=null,source=null;for(const item of chatKnowledge){for(const root of item.roots){if(text.toLowerCase().includes(root.toLowerCase())){const r=item.reply;reply=typeof r==="string"?r:(r[currentLang]||r.ar);source="Gemalot DB";break;}}if(reply)break;}if(!reply){appendMessage(t.searching,"ai",false);const web=await searchAllSources(text);const last=chatContainer.lastChild;if(last&&last.textContent===t.searching)last.remove();if(web){reply=web.text;source=web.source;}else{reply=t.noAnswer;}}appendMessage(reply,"ai",true,source);updateLimitUI();}
function sendSuggestion(txt){processMessage(txt);}
if(userInput){userInput.addEventListener("focus",triggerFullGlow);userInput.addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();processMessage();}});}
if(sendBtn)sendBtn.addEventListener("click",()=>processMessage());
const settingsBtn=document.getElementById('settingsBtn');if(settingsBtn)settingsBtn.addEventListener('click',()=>{location.href='settings.html';});
applyLanguage(currentLang);renderChatList();updateLimitUI();renderSuggestions();setInterval(updateLimitUI,60000);
</script>
</body>
</html>