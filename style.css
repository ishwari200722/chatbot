<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MIT VPU CS Chatbot</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Exo+2:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  *{margin:0;padding:0;box-sizing:border-box}
  :root{
    --purple:#6C2BD9;
    --purple-dark:#4A1A9E;
    --purple-deep:#1A0540;
    --purple-mid:#2D1066;
    --cyan:#00F5FF;
    --cyan-dim:#00B8C4;
    --cyan-glow:rgba(0,245,255,0.15);
    --bg:#0A0018;
    --bg2:#110028;
    --card:#160033;
    --text:#E8D5FF;
    --text-dim:#9E8FBB;
    --border:rgba(0,245,255,0.2);
    --bot-bubble:#1E0047;
    --user-bubble:linear-gradient(135deg,#6C2BD9,#4A1A9E);
  }

  body{
    font-family:'Exo 2',sans-serif;
    background:var(--bg);
    color:var(--text);
    height:100vh;
    display:flex;
    flex-direction:column;
    overflow:hidden;
    position:relative;
  }

  /* Animated starfield background */
  .stars{
    position:fixed;
    inset:0;
    pointer-events:none;
    z-index:0;
    overflow:hidden;
  }
  .star{
    position:absolute;
    border-radius:50%;
    background:#fff;
    animation:twinkle var(--d,2s) ease-in-out infinite alternate;
  }
  @keyframes twinkle{0%{opacity:.1;transform:scale(0.8)}100%{opacity:.9;transform:scale(1.2)}}

  /* Floating orbs */
  .orb{
    position:fixed;
    border-radius:50%;
    filter:blur(80px);
    pointer-events:none;
    z-index:0;
    animation:floatOrb var(--od,8s) ease-in-out infinite alternate;
  }
  @keyframes floatOrb{0%{transform:translate(0,0)}100%{transform:translate(var(--ox,20px),var(--oy,-20px))}}

  /* Grid lines */
  .grid{
    position:fixed;
    inset:0;
    background-image:
      linear-gradient(rgba(0,245,255,0.03) 1px,transparent 1px),
      linear-gradient(90deg,rgba(0,245,255,0.03) 1px,transparent 1px);
    background-size:40px 40px;
    pointer-events:none;
    z-index:0;
  }

  /* Header */
  header{
    position:relative;
    z-index:10;
    display:flex;
    align-items:center;
    gap:16px;
    padding:16px 24px;
    background:rgba(10,0,24,0.85);
    border-bottom:1px solid var(--border);
    backdrop-filter:blur(20px);
    box-shadow:0 0 30px rgba(0,245,255,0.08);
  }
  .logo-wrap{
    width:54px;height:54px;
    border-radius:14px;
    border:2px solid var(--cyan);
    display:flex;align-items:center;justify-content:center;
    background:var(--purple-deep);
    box-shadow:0 0 20px rgba(0,245,255,0.3),inset 0 0 15px rgba(108,43,217,0.4);
    animation:pulse-logo 3s ease-in-out infinite;
    overflow:hidden;
    flex-shrink:0;
  }
  @keyframes pulse-logo{0%,100%{box-shadow:0 0 20px rgba(0,245,255,0.3),inset 0 0 15px rgba(108,43,217,0.4)}50%{box-shadow:0 0 40px rgba(0,245,255,0.6),inset 0 0 25px rgba(108,43,217,0.6)}}
  .logo-wrap img{width:100%;height:100%;object-fit:cover;border-radius:12px}
  .logo-fallback{font-family:'Orbitron',sans-serif;font-size:14px;font-weight:900;color:var(--cyan);text-align:center;line-height:1.1}
  .header-info h1{
    font-family:'Orbitron',sans-serif;
    font-size:15px;font-weight:700;
    color:var(--cyan);
    letter-spacing:1px;
    text-shadow:0 0 15px rgba(0,245,255,0.5);
  }
  .header-info p{font-size:12px;color:var(--text-dim);letter-spacing:.5px}
  .status-dot{
    margin-left:auto;
    display:flex;align-items:center;gap:8px;
    font-size:12px;color:var(--cyan-dim);
  }
  .dot{
    width:8px;height:8px;border-radius:50%;
    background:var(--cyan);
    box-shadow:0 0 8px var(--cyan);
    animation:blink 1.5s ease-in-out infinite;
  }
  @keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}

  /* Chat area */
  .chat-wrap{
    position:relative;z-index:5;
    flex:1;overflow-y:auto;
    padding:24px 20px;
    display:flex;flex-direction:column;gap:16px;
    scrollbar-width:thin;
    scrollbar-color:var(--purple) transparent;
  }
  .chat-wrap::-webkit-scrollbar{width:4px}
  .chat-wrap::-webkit-scrollbar-track{background:transparent}
  .chat-wrap::-webkit-scrollbar-thumb{background:var(--purple);border-radius:4px}

  /* Messages */
  .msg{
    display:flex;gap:10px;
    animation:msgIn .4s cubic-bezier(0.175,0.885,0.32,1.275);
    max-width:85%;
  }
  @keyframes msgIn{from{opacity:0;transform:translateY(20px) scale(.95)}to{opacity:1;transform:translateY(0) scale(1)}}
  .msg.bot{align-self:flex-start}
  .msg.user{align-self:flex-end;flex-direction:row-reverse}

  .avatar{
    width:34px;height:34px;border-radius:10px;
    display:flex;align-items:center;justify-content:center;
    flex-shrink:0;font-size:14px;font-weight:700;
    font-family:'Orbitron',sans-serif;
  }
  .bot .avatar{
    background:var(--purple-deep);
    border:1.5px solid var(--cyan);
    color:var(--cyan);
    box-shadow:0 0 10px rgba(0,245,255,0.2);
  }
  .user .avatar{
    background:linear-gradient(135deg,var(--purple),var(--purple-dark));
    border:1.5px solid rgba(108,43,217,0.5);
    color:#fff;
  }

  .bubble{
    padding:12px 16px;
    border-radius:16px;
    font-size:14px;line-height:1.6;
    max-width:100%;
  }
  .bot .bubble{
    background:var(--bot-bubble);
    border:1px solid rgba(0,245,255,0.12);
    color:var(--text);
    border-radius:4px 16px 16px 16px;
    box-shadow:0 4px 20px rgba(0,0,0,0.4);
  }
  .user .bubble{
    background:var(--user-bubble);
    border:1px solid rgba(108,43,217,0.3);
    color:#fff;
    border-radius:16px 4px 16px 16px;
    box-shadow:0 4px 20px rgba(108,43,217,0.3);
  }

  .bubble strong{color:var(--cyan);font-weight:600}
  .bubble .highlight{
    display:inline-block;
    background:rgba(0,245,255,0.08);
    border:1px solid rgba(0,245,255,0.2);
    border-radius:6px;padding:2px 8px;
    color:var(--cyan);font-size:13px;margin:2px 2px;
  }

  /* Fee card */
  .fee-card{
    background:rgba(108,43,217,0.08);
    border:1px solid rgba(0,245,255,0.15);
    border-radius:10px;
    padding:10px 12px;margin:8px 0;
    display:flex;justify-content:space-between;align-items:center;
    transition:all .2s;
  }
  .fee-card:hover{background:rgba(0,245,255,0.06);border-color:rgba(0,245,255,0.3);transform:translateX(3px)}
  .fee-label{font-size:13px;color:var(--text-dim)}
  .fee-amount{font-family:'Orbitron',sans-serif;font-size:14px;color:var(--cyan);font-weight:700}

  /* Quick buttons */
  .quick-btns{
    display:flex;flex-wrap:wrap;gap:8px;margin-top:12px;
  }
  .qbtn{
    font-family:'Exo 2',sans-serif;
    font-size:12px;padding:7px 14px;
    border:1px solid rgba(0,245,255,0.25);
    border-radius:20px;
    background:rgba(0,245,255,0.04);
    color:var(--cyan-dim);
    cursor:pointer;
    transition:all .2s;
    white-space:nowrap;
  }
  .qbtn:hover{
    background:rgba(0,245,255,0.12);
    border-color:var(--cyan);
    color:var(--cyan);
    box-shadow:0 0 12px rgba(0,245,255,0.15);
    transform:translateY(-1px);
  }

  /* Typing indicator */
  .typing{
    display:flex;align-items:center;gap:5px;
    padding:12px 16px;
    background:var(--bot-bubble);
    border:1px solid rgba(0,245,255,0.12);
    border-radius:4px 16px 16px 16px;
    width:fit-content;
  }
  .typing span{
    width:7px;height:7px;border-radius:50%;
    background:var(--cyan-dim);
    animation:bounce .8s ease-in-out infinite;
  }
  .typing span:nth-child(2){animation-delay:.15s}
  .typing span:nth-child(3){animation-delay:.3s}
  @keyframes bounce{0%,80%,100%{transform:translateY(0);opacity:.4}40%{transform:translateY(-6px);opacity:1}}

  /* Input area */
  .input-wrap{
    position:relative;z-index:10;
    padding:16px 20px;
    background:rgba(10,0,24,0.9);
    border-top:1px solid var(--border);
    backdrop-filter:blur(20px);
  }
  .input-row{
    display:flex;gap:10px;align-items:center;
    background:var(--bg2);
    border:1px solid var(--border);
    border-radius:14px;
    padding:8px 8px 8px 16px;
    transition:border-color .2s, box-shadow .2s;
  }
  .input-row:focus-within{
    border-color:rgba(0,245,255,0.5);
    box-shadow:0 0 20px rgba(0,245,255,0.1);
  }
  #msgInput{
    flex:1;background:transparent;border:none;outline:none;
    color:var(--text);font-family:'Exo 2',sans-serif;font-size:14px;
    caret-color:var(--cyan);
  }
  #msgInput::placeholder{color:var(--text-dim);opacity:.7}
  #sendBtn{
    width:38px;height:38px;border-radius:10px;
    background:linear-gradient(135deg,var(--purple),var(--purple-dark));
    border:none;cursor:pointer;
    display:flex;align-items:center;justify-content:center;
    transition:all .2s;
    flex-shrink:0;
    box-shadow:0 0 15px rgba(108,43,217,0.4);
  }
  #sendBtn:hover{transform:scale(1.08);box-shadow:0 0 25px rgba(108,43,217,0.6)}
  #sendBtn svg{width:18px;height:18px;fill:none;stroke:#fff;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
  .hint{text-align:center;font-size:11px;color:var(--text-dim);margin-top:8px;opacity:.5;letter-spacing:.5px}

  /* Scanline effect */
  .scanline{
    position:fixed;inset:0;pointer-events:none;z-index:1;
    background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,0.03) 2px,rgba(0,0,0,0.03) 4px);
    animation:scan 8s linear infinite;
  }
  @keyframes scan{0%{background-position:0 0}100%{background-position:0 100%}}

  /* Glitch text */
  @keyframes glitch{
    0%,90%,100%{text-shadow:0 0 15px rgba(0,245,255,0.5)}
    92%{text-shadow:-2px 0 #ff00ff,2px 0 #00ffff}
    94%{text-shadow:2px 0 #ff00ff,-2px 0 #00ffff}
    96%{text-shadow:0 0 15px rgba(0,245,255,0.5)}
  }
  header h1{animation:glitch 6s ease-in-out infinite}

  /* Welcome banner */
  .welcome-banner{
    text-align:center;padding:20px;
    animation:fadeUp .8s ease;
  }
  @keyframes fadeUp{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}
  .welcome-banner .icon{
    font-size:40px;margin-bottom:12px;
    display:block;
    filter:drop-shadow(0 0 15px var(--cyan));
    animation:floatIcon 3s ease-in-out infinite;
  }
  @keyframes floatIcon{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
  .welcome-banner h2{
    font-family:'Orbitron',sans-serif;
    font-size:18px;color:var(--cyan);
    text-shadow:0 0 20px rgba(0,245,255,0.4);
    margin-bottom:6px;
  }
  .welcome-banner p{font-size:13px;color:var(--text-dim)}

  /* Circuit decoration */
  .circuit{position:fixed;bottom:0;right:0;opacity:.05;pointer-events:none;z-index:0}
</style>
</head>
<body>

<!-- Background effects -->
<div class="stars" id="stars"></div>
<div class="orb" style="width:400px;height:400px;background:rgba(108,43,217,0.15);top:-100px;left:-100px;--od:10s;--ox:30px;--oy:20px"></div>
<div class="orb" style="width:300px;height:300px;background:rgba(0,245,255,0.06);bottom:-50px;right:-50px;--od:7s;--ox:-20px;--oy:-30px"></div>
<div class="grid"></div>
<div class="scanline"></div>

<!-- SVG circuit decoration -->
<svg class="circuit" width="300" height="300" viewBox="0 0 300 300" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M 50 0 L 50 50 L 100 50 L 100 100 L 200 100 L 200 50 L 250 50 L 250 0" stroke="#00F5FF" stroke-width="1"/>
  <path d="M 0 150 L 100 150 L 100 200 L 200 200 L 200 150 L 300 150" stroke="#00F5FF" stroke-width="1"/>
  <circle cx="50" cy="50" r="4" fill="#00F5FF"/>
  <circle cx="100" cy="100" r="4" fill="#00F5FF"/>
  <circle cx="200" cy="100" r="4" fill="#00F5FF"/>
  <circle cx="100" cy="150" r="4" fill="#00F5FF"/>
  <circle cx="200" cy="200" r="4" fill="#6C2BD9"/>
</svg>

<!-- Header -->
<header>
  <div class="logo-wrap" id="logoWrap">
    <img id="logoImg" src="https://www.mitvpu.ac.in/wp-content/uploads/2022/10/MIT-VPU-Logo.png"
      onerror="this.style.display='none';document.getElementById('logoFb').style.display='flex'"
      alt="MIT VPU Logo">
    <div id="logoFb" class="logo-fallback" style="display:none">MIT<br>VPU</div>
  </div>
  <div class="header-info">
    <h1>MIT VPU CS ADVISOR</h1>
    <p>Department of Computer Science &amp; Engineering</p>
  </div>
  <div class="status-dot">
    <div class="dot"></div>
    Online
  </div>
</header>

<!-- Chat area -->
<div class="chat-wrap" id="chatWrap">
  <div class="welcome-banner">
    <span class="icon">🎓</span>
    <h2>Welcome to MIT VPU!</h2>
    <p>Ask me about courses, fees, admissions &amp; eligibility</p>
  </div>
</div>

<!-- Input -->
<div class="input-wrap">
  <div class="input-row">
    <input type="text" id="msgInput" placeholder="Ask about fees, admissions, eligibility..." autocomplete="off"/>
    <button id="sendBtn" aria-label="Send">
      <svg viewBox="0 0 24 24"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
    </button>
  </div>
  <p class="hint">MIT VPU · Solapur · Computer Science Department</p>
</div>

<script>
// Stars
const starsEl = document.getElementById('stars');
for(let i=0;i<120;i++){
  const s=document.createElement('div');
  s.className='star';
  const size=Math.random()*2.5+0.5;
  s.style.cssText=`width:${size}px;height:${size}px;top:${Math.random()*100}%;left:${Math.random()*100}%;--d:${(Math.random()*3+1).toFixed(1)}s;animation-delay:${(Math.random()*3).toFixed(1)}s`;
  starsEl.appendChild(s);
}

// Knowledge base
const KB = {
  fees:{
    'btech cse':175000,'b tech cse':175000,'btech computer science':175000,
    'bca':90000,'bachelor of computer applications':90000,
    'mca':175000,'master of computer applications':175000,
    'it':175000,'information technology':175000,'btech it':175000,
    'ai ml':175000,'aiml':175000,'artificial intelligence':175000,'machine learning':175000,'ai/ml':175000,
  },
  allFees:[
    {name:'B.Tech CSE',amount:'₹1,75,000 / year'},
    {name:'BCA',amount:'₹90,000 / year'},
    {name:'MCA',amount:'₹1,75,000 / year'},
    {name:'B.Tech IT',amount:'₹1,75,000 / year'},
    {name:'B.Tech AI/ML',amount:'₹1,75,000 / year'},
  ],
  eligibility:{
    'btech':['Completed 10+2 / HSC with Physics, Chemistry & Maths','Minimum 45% aggregate (40% for reserved categories)','Valid MHT-CET / JEE Main score required','Maharashtra domicile preferred'],
    'bca':['Completed 10+2 / HSC in any stream','Minimum 45% aggregate marks','No entrance exam mandatory','Direct admission on merit basis'],
    'mca':['Bachelor\'s degree with Maths at 10+2 or graduation level','Minimum 50% in graduation','Valid PERA CET / MHT-CET PG score','Lateral entry available for BCA/BSc-CS graduates'],
    'it':['Same as B.Tech CSE — PCM in 12th','MHT-CET / JEE Main score','45% minimum aggregate'],
    'aiml':['Same as B.Tech — PCM in 12th','MHT-CET / JEE Main score','45% minimum aggregate','Interest in data & programming preferred'],
  },
  admissionProcess:[
    'Visit the official MIT VPU website: www.mitvpu.ac.in',
    'Click on "Admissions" and select your desired program',
    'Fill the online application form with required details',
    'Upload 10th, 12th marksheets & entrance score card',
    'Pay application fee online',
    'Appear for counselling / merit-based selection',
    'Complete document verification & fee payment to confirm seat',
  ]
};

function detectIntent(msg){
  const m=msg.toLowerCase();
  if(m.includes('hello')||m.includes('hi ')||m.includes('hey')||m.trim()==='hi')return 'greet';
  if(m.includes('all fee')||m.includes('all course')||(m.includes('fee')&&m.includes('list')))return 'allFees';
  if(m.includes('fee')||m.includes('cost')||m.includes('price')||m.includes('how much')){
    if(m.includes('bca'))return 'fee_bca';
    if(m.includes('mca'))return 'fee_mca';
    if(m.includes('ai')||m.includes('ml'))return 'fee_aiml';
    if(m.includes('it')&&!m.includes('mit'))return 'fee_it';
    if(m.includes('cse')||m.includes('computer science')||m.includes('btech')||m.includes('b.tech'))return 'fee_btech';
    return 'allFees';
  }
  if(m.includes('eligib')||m.includes('qualify')||m.includes('criteria')||m.includes('requirement')){
    if(m.includes('bca'))return 'elig_bca';
    if(m.includes('mca'))return 'elig_mca';
    if(m.includes('ai')||m.includes('ml'))return 'elig_aiml';
    if(m.includes('it'))return 'elig_it';
    if(m.includes('cse')||m.includes('btech')||m.includes('b.tech'))return 'elig_btech';
    return 'elig_general';
  }
  if(m.includes('admission')||m.includes('apply')||m.includes('how to join')||m.includes('enroll'))return 'admission';
  if(m.includes('course')||m.includes('program')||m.includes('offer')||m.includes('available'))return 'courses';
  if(m.includes('contact')||m.includes('address')||m.includes('phone')||m.includes('email')||m.includes('website'))return 'contact';
  if(m.includes('about')||m.includes('college')||m.includes('university')||m.includes('vpu')||m.includes('mit'))return 'about';
  if(m.includes('thank'))return 'thanks';
  return 'unknown';
}

function buildFeeCard(name,amount){
  return `<div class="fee-card"><span class="fee-label">${name}</span><span class="fee-amount">${amount}</span></div>`;
}

function buildEligList(items){
  return items.map(i=>`• ${i}`).join('<br>');
}

function getResponse(intent){
  const quickAll='<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">All Fees</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button><button class="qbtn" onclick="sendQ(this)">B.Tech Eligibility</button><button class="qbtn" onclick="sendQ(this)">Contact Info</button></div>';
  const feeQuick='<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">BCA Fee</button><button class="qbtn" onclick="sendQ(this)">MCA Fee</button><button class="qbtn" onclick="sendQ(this)">AI/ML Fee</button><button class="qbtn" onclick="sendQ(this)">IT Fee</button></div>';

  switch(intent){
    case 'greet':
      return `Hello! 👋 Welcome to <strong>MIT VPU's CS Advisor</strong>!<br><br>I can help you with:<br>• 💰 Course fees<br>• 📋 Admission eligibility<br>• 🎓 Programs offered<br>• 📞 Contact details<br><br>What would you like to know?${quickAll}`;

    case 'allFees':
      return `Here are all CS program fees at <strong>MIT VPU</strong>:<br><br>${KB.allFees.map(f=>buildFeeCard(f.name,f.amount)).join('')}<br><span style="font-size:12px;color:var(--text-dim)">* Fees are per academic year. Subject to revision.</span>${feeQuick}`;

    case 'fee_btech':
      return `<strong>B.Tech CSE</strong> Annual Fee:<br>${buildFeeCard('B.Tech Computer Science & Engineering','₹1,75,000 / year')}<br>Hostel & other charges may apply separately.<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">B.Tech Eligibility</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'fee_bca':
      return `<strong>BCA</strong> Annual Fee:<br>${buildFeeCard('Bachelor of Computer Applications','₹90,000 / year')}<br>BCA is one of the most affordable programs at MIT VPU!<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">BCA Eligibility</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'fee_mca':
      return `<strong>MCA</strong> Annual Fee:<br>${buildFeeCard('Master of Computer Applications','₹1,75,000 / year')}<br>MCA is a 2-year PG program.<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">MCA Eligibility</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'fee_it':
      return `<strong>B.Tech IT</strong> Annual Fee:<br>${buildFeeCard('B.Tech Information Technology','₹1,75,000 / year')}<br>4-year undergraduate program.<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">IT Eligibility</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'fee_aiml':
      return `<strong>B.Tech AI/ML</strong> Annual Fee:<br>${buildFeeCard('B.Tech Artificial Intelligence & Machine Learning','₹1,75,000 / year')}<br>One of the most in-demand programs!<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">AI/ML Eligibility</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'elig_btech':
      return `<strong>B.Tech CSE</strong> Eligibility:<br><br>${buildEligList(KB.eligibility['btech'])}<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">B.Tech Fee</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'elig_bca':
      return `<strong>BCA</strong> Eligibility:<br><br>${buildEligList(KB.eligibility['bca'])}<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">BCA Fee</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'elig_mca':
      return `<strong>MCA</strong> Eligibility:<br><br>${buildEligList(KB.eligibility['mca'])}<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">MCA Fee</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'elig_it':
      return `<strong>B.Tech IT</strong> Eligibility:<br><br>${buildEligList(KB.eligibility['it'])}<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">IT Fee</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'elig_aiml':
      return `<strong>AI/ML</strong> Eligibility:<br><br>${buildEligList(KB.eligibility['aiml'])}<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">AI/ML Fee</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'elig_general':
      return `<strong>General Eligibility Overview:</strong><br><br>🎓 <strong>UG Programs (B.Tech/BCA):</strong><br>• 10+2 / HSC completion<br>• Min 45% aggregate<br>• Entrance exam for B.Tech (MHT-CET/JEE)<br><br>🏆 <strong>PG Programs (MCA):</strong><br>• Bachelor's degree with 50%+<br>• Maths background required<br>• PERA CET / MHT-CET PG<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">B.Tech Eligibility</button><button class="qbtn" onclick="sendQ(this)">BCA Eligibility</button><button class="qbtn" onclick="sendQ(this)">MCA Eligibility</button></div>`;

    case 'admission':
      return `<strong>Admission Process at MIT VPU:</strong><br><br>${KB.admissionProcess.map((s,i)=>`<span class="highlight">Step ${i+1}</span> ${s}`).join('<br><br>')}<br><br>🌐 Apply at: <strong>www.mitvpu.ac.in</strong><div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">All Fees</button><button class="qbtn" onclick="sendQ(this)">B.Tech Eligibility</button><button class="qbtn" onclick="sendQ(this)">Contact Info</button></div>`;

    case 'courses':
      return `<strong>CS Programs at MIT VPU:</strong><br><br>${[
        ['🖥️','B.Tech CSE','4 years · UG'],
        ['💻','BCA','3 years · UG'],
        ['🎓','MCA','2 years · PG'],
        ['🌐','B.Tech IT','4 years · UG'],
        ['🤖','B.Tech AI/ML','4 years · UG'],
      ].map(([ico,n,d])=>`<div class="fee-card"><span style="display:flex;align-items:center;gap:8px">${ico} <span>${n}</span></span><span class="fee-label">${d}</span></div>`).join('')}
      <div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">All Fees</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'contact':
      return `<strong>Contact MIT VPU:</strong><br><br>🌐 Website: <strong>www.mitvpu.ac.in</strong><br>📍 Location: <strong>Solapur, Maharashtra</strong><br>📧 Email: <strong>info@mitvpu.ac.in</strong><br>📱 Instagram: <strong>@mitvpu</strong><br><br><span style="font-size:12px;color:var(--text-dim)">Visit the official website for updated phone numbers & office hours.</span><div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">Admission Process</button><button class="qbtn" onclick="sendQ(this)">All Fees</button></div>`;

    case 'about':
      return `<strong>About MIT VPU</strong> — MIT Vishwaprayag University:<br><br>🏛️ Part of the prestigious <strong>MAEER's MIT Group</strong> with 40+ years of excellence<br><br>🌱 100-acre green campus in Solapur, Maharashtra<br><br>🏆 Schools of Computing, Business, Design, Sciences & Pharmacy<br><br>🤝 Strong industry collaborations & placement support<br><br>🔬 Modern labs, high-tech studios & collaborative workspaces<div class="quick-btns"><button class="qbtn" onclick="sendQ(this)">All Courses</button><button class="qbtn" onclick="sendQ(this)">All Fees</button><button class="qbtn" onclick="sendQ(this)">Admission Process</button></div>`;

    case 'thanks':
      return `You're welcome! 😊 Feel free to ask anything else about MIT VPU admissions, fees, or eligibility. Good luck with your future! 🚀${quickAll}`;

    default:
      return `I'm not sure about that, but I can help you with:<br><br>• 💰 <strong>Course Fees</strong> — B.Tech, BCA, MCA, IT, AI/ML<br>• 📋 <strong>Eligibility Criteria</strong> — for all programs<br>• 🎓 <strong>Admission Process</strong><br>• 🏛️ <strong>About MIT VPU</strong><br>• 📞 <strong>Contact Info</strong><br><br>Try asking something like <em>"What is the BCA fee?"</em> or <em>"How to apply for B.Tech?"</em>${quickAll}`;
  }
}

window.sendQ=function(btn){
  const txt=btn.textContent.trim();
  processInput(txt);
};

function appendMsg(who,html){
  const wrap=document.getElementById('chatWrap');
  const div=document.createElement('div');
  div.className=`msg ${who}`;
  const av=document.createElement('div');
  av.className='avatar';
  av.textContent=who==='bot'?'AI':'U';
  const bub=document.createElement('div');
  bub.className='bubble';
  bub.innerHTML=html;
  div.appendChild(av);div.appendChild(bub);
  wrap.appendChild(div);
  wrap.scrollTop=wrap.scrollHeight;
  return div;
}

function showTyping(){
  const wrap=document.getElementById('chatWrap');
  const div=document.createElement('div');
  div.className='msg bot';div.id='typing-ind';
  const av=document.createElement('div');av.className='avatar';av.textContent='AI';
  const bub=document.createElement('div');bub.className='typing';
  bub.innerHTML='<span></span><span></span><span></span>';
  div.appendChild(av);div.appendChild(bub);
  wrap.appendChild(div);wrap.scrollTop=wrap.scrollHeight;
}

function removeTyping(){
  const t=document.getElementById('typing-ind');if(t)t.remove();
}

function processInput(txt){
  if(!txt.trim())return;
  const welcome=document.querySelector('.welcome-banner');
  if(welcome)welcome.remove();
  appendMsg('user',txt);
  const inp=document.getElementById('msgInput');
  inp.value='';
  showTyping();
  const delay=600+Math.random()*600;
  setTimeout(()=>{
    removeTyping();
    const intent=detectIntent(txt);
    const resp=getResponse(intent);
    appendMsg('bot',resp);
  },delay);
}

document.getElementById('sendBtn').onclick=()=>processInput(document.getElementById('msgInput').value);
document.getElementById('msgInput').addEventListener('keydown',e=>{if(e.key==='Enter')processInput(e.target.value)});

// Auto-greet
setTimeout(()=>{
  const intent=detectIntent('hello');
  appendMsg('bot',getResponse(intent));
},600);
</script>
</body>
</html>
