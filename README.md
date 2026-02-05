  <!DOCTYPE html>
  <html lang="ar" dir="rtl">
  <head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>🏕️ البحث عن الكنز الكشفي - تحدي صعب جدًا</title>

  <style>
    :root{
      --bg1:#0f1f12;
      --bg2:#1e3d25;
      --card:#ffffff10;
      --accent:#ffca28;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      color:#fff;
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      animation:bgAnimate 10s infinite alternate;
    }

    @keyframes bgAnimate{
      0%{background:linear-gradient(135deg,#0f1f12,#1e3d25);}
      50%{background:linear-gradient(135deg,#1e3d25,#0f2a15);}
      100%{background:linear-gradient(135deg,#0f1f12,#1e3d25);}
    }

    .app{
      width:95%;
      max-width:950px;
      padding:35px;
      border-radius:25px;
      background:var(--card);
      backdrop-filter: blur(18px);
      box-shadow:0 0 50px #00000080;
      border:2px solid rgba(255,255,255,0.2);
    }

    h1{margin:0 0 12px 0;font-size:36px;text-align:center;text-shadow:1px 1px 6px #000}
    p{font-size:18px;text-shadow:1px 1px 4px #000}

    .panel{
      margin-top:25px;
      padding:25px;
      border-radius:20px;
      background:#ffffff15;
      box-shadow:0 0 25px #00000060;
      transition:transform 0.3s;
    }

    .panel:hover{transform:scale(1.02)}

    input{
      width:85%;
      padding:16px;
      border-radius:14px;
      border:none;
      font-size:18px;
      text-align:center;
      margin-top:18px;
    }

    button{
      margin-top:18px;
      padding:16px 35px;
      font-size:18px;
      border:none;
      border-radius:14px;
      cursor:pointer;
      background:var(--accent);
      font-weight:bold;
      box-shadow:0 0 12px #00000050;
      transition:transform 0.2s;
    }

    button:hover{transform:scale(1.05)}
    .hidden{display:none}
    .msg{margin-top:14px;font-weight:bold;font-size:18px;text-align:center}
    .ok{color:#00ffb3}
    .bad{color:#ff9e9e}
    .hint{
      margin-top:12px;
      opacity:0.9;
      font-size:17px;
      text-align:center;
    }

    footer{
      text-align:center;
      margin-top:30px;
      opacity:0.85;
      font-size:15px;
      text-shadow:1px 1px 3px #000;
    }
  </style>
  </head>
  <body>

  <div class="app">
    <h1>لعبة البحث عن الكنز الكشفي - تحدي صعب جدًا</h1>

    <div id="start" class="panel">
      <p>اكتب رقم فريقك (1 → 4)</p>
      <input type="number" id="team" min="1" max="4" />
      <br/>
      <button onclick="startGame()">ابدأ المغامرة</button>
      <div class="hint">ابحث عن الورقة الخاصة بك بعناية… كل كود فريد وصعب!</div>
    </div>

    <div id="game" class="panel hidden">
      <h2 id="question"></h2>
      <div class="hint" id="goTo"></div>
      <input type="text" id="codeInput" placeholder="اكتب الكود هنا" />
      <br/>
      <button onclick="checkCode()">تأكيد الكود</button>
      <div id="message" class="msg"></div>
    </div>

    <div id="end" class="panel hidden">
      <h2>🏆 مبروك!</h2>
      <p>لقد وصلت للمرحلة الأخيرة… الكنز مدفون في منطقة الرمل!</p>
    </div>

    <footer>
      تحدي الكشافة - كل مرحلة تحتاج تركيز عالي 🧭
    </footer>
  </div>

<script>
const paths = {
  1: ["الفيلا", "المياه", "العلم", "ركن العربيات", "المطعم", "الكوبري", "غرفة 207", "الرمل"],
  2: ["ركن العربيات", "المطعم", "الكوبري", "غرفة 207", "المياه", "الفيلا", "العلم", "الرمل"],
  3: ["غرفة 207", "الفيلا", "ركن العربيات", "المطعم", "العلم", "المياه", "الكوبري", "الرمل"],
  4: ["المياه", "العلم", "الكوبري", "الفيلا", "ركن العربيات", "المطعم", "غرفة 207", "الرمل"]
};

const questions = {
  "العلم":{q:"ابدأوا يومكم هنا، المكان الذي يرفع فيه شيء ما كل صباح. ابحثوا عن الكود."},
  "الكوبري":{q:"مكان به ماء لكن لا تتبلل الكود مخفي في هذا المكان ."},
  "الفيلا":{q:"مبنى مهجور مليء بالغموض… اكتشفوا الكود."},
  "المطعم":{q:"لابد  ان تفعله كل يوم و الا سوف تموت اذهب للمكان الذي تفعل هذا فيه."},
  "المياه":{q:"مصدر الحياة شفاف…  يفتح المرحلة التالية."},
  "غرفة 207":{q:"اذهب لغرفه من برمجوا هذه اللعبه غرفه  ..... ."},
  "ركن العربيات":{q:"في مكان يوجد بع ثلاث اشياء ضخمه و يدخل بها الانسان كي يفعل شيء ما اذهب سريعا."},
  "الرمل":{q:"المرحلة الأخيرة… اكتب الكود الموجود عند الرمل لتنال الكنز."}
};

const codes = {
  "العلم":{1:"X7F3Q",2:"M8H2L",3:"T9Z1K",4:"B4R8W"},
  "الكوبري":{1:"J2L9P",2:"K7X5Q",3:"N1C6V",4:"F8D3R"},
  "الفيلا":{1:"Q4W8Z",2:"H3J9M",3:"L5T2B",4:"S7K1X"},
  "المطعم":{1:"V9N3C",2:"P8Q4J",3:"D2M7R",4:"A6F5L"},
  "المياه":{1:"G3X9T",2:"K4L7P",3:"R8D1B",4:"M6C2Q"},
  "غرفة 207":{1:"B5F8J",2:"L1Q3X",3:"T7C4V",4:"D9K2M"},
  "ركن العربيات":{1:"H2M9R",2:"F4V8P",3:"C1X6T",4:"J7L3Q"},
  "الرمل":{1:"Z9K2W",2:"X7Q4V",3:"P8C5R",4:"T1L9B"}
};

let step=0;
let path=[];
let currentTeam=1;

function startGame(){
  currentTeam=parseInt(document.getElementById('team').value);
  if(!paths[currentTeam]) return alert('ادخل رقم فريق صحيح (1-4)');
  path=paths[currentTeam];
  step = 0; // reset step
  document.getElementById('start').classList.add('hidden');
  document.getElementById('game').classList.remove('hidden');
  showQuestion();
}

function showQuestion(){
  const place=path[step];
  document.getElementById('question').innerText=questions[place].q;
  document.getElementById('goTo').innerText="ابحث عن الورقة الخاصة بفريقك في هذا المكان.";
  document.getElementById('codeInput').value='';
  document.getElementById('message').innerText='';
}

function normalize(t){return t.trim().toUpperCase();}

function checkCode(){
  const place=path[step];
  const user=normalize(document.getElementById('codeInput').value);
  if(user===normalize(codes[place][currentTeam])){
    step++;
    const msg=document.getElementById('message');
    msg.innerText='كود صحيح! المرحلة التالية مفتوحة';
    msg.className='msg ok';

    if(step<path.length){
      setTimeout(showQuestion,1200);
    }else{
      document.getElementById('game').classList.add('hidden');
      document.getElementById('end').classList.remove('hidden');
    }
  }else{
    const msg=document.getElementById('message');
    msg.innerText='الكود خاطئ… راجع الورقة الخاصة بك';
    msg.className='msg bad';
  }
}
</script>



  </body>
  </html>
