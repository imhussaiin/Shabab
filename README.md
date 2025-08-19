[index.html](https://github.com/user-attachments/files/21868773/index.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>لعبة قروب الشباب</title>
<style>
  body{font-family:system-ui, Arial; background:linear-gradient(135deg,#fce3ec,#d9e7ff); display:flex; justify-content:center; align-items:center; min-height:100vh; margin:0; padding:20px;}
  .card{width:min(720px,100%);background:#fff;border-radius:16px;padding:20px;box-shadow:0 20px 50px rgba(0,0,0,.15);}
  h1{text-align:center;color:#111827;margin:0 0 10px;}
  .sub{text-align:center;color:#4b5563;margin-bottom:14px;}
  .topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
  .badge{background:#111827;color:#fff;padding:6px 12px;border-radius:999px;font-weight:bold;}
  .timer{font-weight:bold;color:#111827;}
  .q{font-size:20px;color:#111827;margin:16px 0;}
  .opts{display:grid;gap:10px;}
  .btn{padding:12px;border:2px solid #e5e7eb;border-radius:12px;background:#f8fafc;font-weight:bold;cursor:pointer;text-align:right;}
  .btn.ok{border-color:green;background:#ecfdf5;}
  .btn.bad{border-color:red;background:#fef2f2;}
  .result{text-align:center;margin-top:20px;}
  .cta{margin-top:12px;padding:12px 18px;border:none;border-radius:10px;background:#6c5ce7;color:#fff;font-weight:bold;cursor:pointer;}
  .prize{background:#ecfdf5;border:2px dashed green;padding:12px;border-radius:12px;margin-top:12px;color:#065f46;}
  .fail{background:#fef2f2;border:2px dashed red;padding:12px;border-radius:12px;margin-top:12px;color:#991b1b;}
</style>
</head>
<body>
  <div class="card">
    <h1>لعبة قروب الشباب</h1>
    <div class="sub">جاوب على 10 أسئلة متتالية صح للفوز!</div>
    <div class="topbar">
      <div class="badge">صحيحة متتالية: <span id="streak">0</span> / 10</div>
      <div class="timer">الوقت: <span id="time">15</span>ث</div>
    </div>
    <div id="game">
      <div id="question" class="q">السؤال سيظهر هنا…</div>
      <div id="options" class="opts"></div>
    </div>
    <div id="result" class="result" style="display:none"></div>
  </div>

<script>
const bank = [
  {q:'متى عيد ميلاد عبدالغفار؟', a:['٤ / يناير','٩ / يناير','٦ / يناير','٨ / يناير'], c:1},
  {q:'كم عمر مشاري؟', a:['47','46','49','48'], c:3},
  {q:'وين يشتغل وهاب؟', a:['وزارة الداخليه','الادلة الجنائية','حفار قبور','كابتن طيارة حربية'], c:1},
  {q:'متى عيد ميلاد ماريا؟', a:['٥ / مايو','٢١ / مايو','٣٠ / مايو','٣١ / مايو'], c:3},
  {q:'كم وزن علاو غفار؟', a:['120 كيلو','118 كيلو','طن','112 كيلو'], c:2},
  {q:'كم يبلغ طول شنب يوسف؟', a:['نص متر','ربع متر','٤ سانتي','1 كيلومتر'], c:3},
  {q:'من هو الملقب بـ \"محقان\" من عيال اخواني؟', a:['عبود وهاب','حسون غفار','رزوق مشاري','عزوز'], c:3},
  {q:'من قائل العبارة ( ابا ابدول )؟', a:['علاو مشاري','علاو وهاب','حمود','حسون'], c:2},
  {q:'شنو سيارة عمكم عبود الثانية؟', a:['كامري','موستنق','وانيت سلفرادو','تاهو'], c:1},
  {q:'شنو اسم كاسكو عمكم عبود؟', a:['بابو','بوبا','باريو','بطل'], c:1}
];

const REQUIRED_STREAK = 10;
const PER_QUESTION_TIME = 15;
let streak=0,timeLeft=PER_QUESTION_TIME,tHandle=null,current=null;
const $=id=>document.getElementById(id);
const qEl=$("question"),optsEl=$("options"),timeEl=$("time"),streakEl=$("streak"),resultEl=$("result"),gameEl=$("game");

function startTimer(){timeLeft=PER_QUESTION_TIME;timeEl.textContent=timeLeft;clearInterval(tHandle);tHandle=setInterval(()=>{timeLeft--;timeEl.textContent=timeLeft;if(timeLeft<=0){clearInterval(tHandle);endGame(false,'انتهى الوقت!');}},1000);}
function showQuestion(){if(streak>=REQUIRED_STREAK){endGame(true,'مبروك 🎉');return;}current=bank[streak];qEl.textContent=current.q;optsEl.innerHTML='';current.a.forEach((text,i)=>{const b=document.createElement('button');b.className='btn';b.textContent=`${['أ','ب','ج','د'][i]}) ${text}`;b.onclick=()=>selectAnswer(i,b);optsEl.appendChild(b);});startTimer();}
function selectAnswer(i,btn){clearInterval(tHandle);[...optsEl.children].forEach(b=>b.disabled=true);if(i===current.c){btn.classList.add('ok');streak++;streakEl.textContent=streak;setTimeout(showQuestion,700);}else{btn.classList.add('bad');endGame(false,'إجابة خاطئة!');}}
function endGame(win,reason){gameEl.style.display='none';resultEl.style.display='block';if(win){resultEl.innerHTML=`<h2 style=\"color:green\">لقد ربحت 🎉</h2><div class=\"prize\">بطل ماي أبراج لتر ونص<br>الرجاء التواصل عبر واتساب:<br><b>94097093</b><br><small>رحم الله والديكم</small></div><button class=\"cta\" onclick=\"restart()\">إعادة المحاولة</button>`;}else{resultEl.innerHTML=`<h2 style=\"color:red\">حاول مرة أخرى يا الهطف 😅</h2><div class=\"fail\">${reason}</div><button class=\"cta\" onclick=\"restart()\">إعادة المحاولة</button>`;}}
function restart(){streak=0;streakEl.textContent=0;resultEl.style.display='none';gameEl.style.display='block';showQuestion();}
showQuestion();
</script>
</body>
</html>
