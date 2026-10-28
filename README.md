   
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>TaskCoin Ethiopia</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
:root{color-scheme:dark;--bg:#080b0a;--card:#121916;--green:#20d98b;--muted:#9aaba3}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:#fff;font-family:Arial,sans-serif;padding:18px 15px 90px}
header{display:flex;align-items:center;gap:12px;margin:8px 0 22px}
.logo{width:48px;height:48px;border-radius:15px;background:var(--green);color:#06150e;display:grid;place-items:center;font-weight:900;font-size:23px}
h1{font-size:21px;margin:0}h2{font-size:19px;margin:22px 0 12px}
p{color:var(--muted);font-size:14px;line-height:1.5}
.card{background:var(--card);border:1px solid #26332d;border-radius:17px;padding:16px;margin:12px 0}
.balance{background:linear-gradient(130deg,#123e2b,#10221a);border:1px solid #206844}
.balance strong{font-size:32px;color:var(--green);display:block;margin:10px 0}
button,.btn{border:0;border-radius:11px;padding:12px 14px;background:var(--green);color:#04160d;font-weight:bold;cursor:pointer}
button:disabled{opacity:.5}
input,textarea{width:100%;padding:12px;margin:7px 0 12px;border:1px solid #34443b;border-radius:10px;background:#090e0b;color:white}
nav{position:fixed;bottom:0;left:0;right:0;background:#101612;border-top:1px solid #29342d;display:flex;justify-content:space-around;padding:10px 2px calc(10px + env(safe-area-inset-bottom));z-index:5}
nav button{background:transparent;color:#b8c7be;padding:8px 5px;font-size:11px}
nav button.active{color:var(--green)}
.page{display:none}.page.active{display:block}
.small{font-size:12px;color:var(--muted)}
.task-title{font-weight:bold;font-size:16px}
.row{display:flex;justify-content:space-between;align-items:center;gap:10px}
button.secondary{background:#25342b;color:#fff}
.notice{padding:12px;border-radius:10px;background:#17241c;color:#c8f6dc;font-size:13px}
a{color:var(--green)}
.hidden{display:none!important}
</style>
</head>
<body>
<header>
  <div class="logo">T</div>
  <div><h1>TaskCoin Ethiopia</h1><p style="margin:5px 0">Tasks • Coins • Rewards</p></div>
</header>

<section id="auth" class="card">
  <h2 id="authTitle">Welcome back</h2>
  <p>Sign in or create your account.</p>
  <input id="email" type="email" placeholder="Email address" autocomplete="email">
  <input id="password" type="password" placeholder="Password" autocomplete="current-password">
  <button id="authBtn" style="width:100%">Login</button>
  <button id="switchAuth" class="secondary" style="width:100%;margin-top:9px">Create account</button>
  <p id="authMsg" role="status"></p>
</section>

<main id="app" class="hidden">
  <section id="home" class="page active">
    <div class="card balance">
      <p>Total Points</p><strong id="homeCoins">0</strong>
      <span class="small">Your approved TaskCoin rewards</span>
    </div>
    <div class="card"><div class="row"><span>Available tasks</span><strong>3</strong></div>
      <p>Complete a task and submit evidence for review.</p>
      <button onclick="showPage('tasks')">Explore Tasks</button>
    </div>
    <div class="card"><h3>How it works</h3>
      <p>1. Choose a task.<br>2. Complete it and submit proof.<br>3. Wait for an admin to review it.<br>4. Approved rewards are added to your balance.</p>
    </div>
  </section>

  <section id="tasks" class="page">
    <h2>Available Tasks</h2>
    <p>Follow the public account and submit your proof. Do not submit the same task twice.</p>
    <div id="taskList"></div>
  </section>

  <section id="wallet" class="page">
    <h2>My Wallet</h2>
    <div class="card balance"><p>Total Points</p><strong id="walletCoins">0</strong></div>
    <div class="notice">Only rewards approved by an authorized admin should appear here.</div>
    <p>Withdrawals are not enabled in this version.</p>
  </section>

  <section id="invite" class="page">
    <h2>Invite Friends</h2>
    <div class="card"><p>Share TaskCoin Ethiopia with your friends.</p>
    <button id="shareBtn">Share App</button></div>
  </section>

  <section id="profile" class="page">
    <h2>My Profile</h2>
    <div class="card"><p>Email</p><strong id="profileEmail"></strong>
    <p>Account ID</p><span id="profileId" class="small"></span>
    <button id="logoutBtn" class="secondary" style="width:100%;margin-top:16px">Logout</button></div>
  </section>

  <section id="admin" class="page">
    <h2>Admin Review</h2>
    <div class="notice">Authorized admin accounts only.</div>
    <div id="adminList"><p>No submissions loaded.</p></div>
  </section>
</main>

<nav id="nav" class="hidden">
  <button class="active" data-page="home">🏠<br>Home</button>
  <button data-page="tasks">📋<br>Tasks</button>
  <button data-page="wallet">💰<br>Wallet</button>
  <button data-page="invite">👥<br>Invite</button>
  <button data-page="profile">👤<br>Profile</button>
  <button id="adminNav" class="hidden" data-page="admin">🛡️<br>Admin</button>
</nav>

<script type="module">
import {initializeApp} from
'https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js';
import {getAuth,onAuthStateChanged,createUserWithEmailAndPassword,
signInWithEmailAndPassword,signOut} from
'https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js';
import {getFirestore,doc,getDoc,setDoc,collection,addDoc,
query,where,getDocs,runTransaction,serverTimestamp}
from 'https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js';

const firebaseConfig = {
  apiKey: "AIzaSyCMa8PltC1GF_fn8VWV2w8Un-7zzRzpZOc",
  authDomain: "taskcoin-ethiopia.firebaseapp.com",
  projectId: "taskcoin-ethiopia",
  storageBucket: "taskcoin-ethiopia.firebasestorage.app",
  messagingSenderId: "371761604216",
  appId: "1:371761604216:web:18609aa65ec95f5800ef8d",
  measurementId: "G-7133MYN4HV"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const tg = window.Telegram?.WebApp;
tg?.ready();
tg?.expand();

const tasks = [
 {id:'follow_herfinsta',name:'Follow herfinsta_2',
  url:'https://www.tiktok.com/@herfinsta_2',coins:10},
 {id:'follow_taskcoin',name:'Follow taskcoinethiopia',
  url:'https://www.tiktok.com/@taskcoinethiopia',coins:10},
 {id:'follow_vynaro',name:'Follow vynarostudio',
  url:'https://www.tiktok.com/@vynarostudio',coins:10}
];

let registerMode=false;
let currentUser=null;
let isAdmin=false;
const $=id=>document.getElementById(id);
const msg=(id,text)=>$(id).textContent=text;

$('switchAuth').onclick=()=>{
 registerMode=!registerMode;
 $('authTitle').textContent=registerMode?'Create account':'Welcome back';
 $('authBtn').textContent=registerMode?'Register':'Login';
 $('switchAuth').textContent=registerMode?'Back to login':'Create account';
 msg('authMsg','');
};

$('authBtn').onclick=async()=>{
 const email=$('email').value.trim();
 const password=$('password').value;
 if(!email||password.length<6){
  msg('authMsg','Enter an email and a password of at least 6 characters.');
  return;
 }
 $('authBtn').disabled=true;
 try{
  if(registerMode){
   const result=await createUserWithEmailAndPassword(auth,email,password);
   await setDoc(doc(db,'users',result.user.uid),{
    email,coins:0,createdAt:serverTimestamp()
   });
  }else{
   await signInWithEmailAndPassword(auth,email,password);
  }
 }catch(e){msg('authMsg',e.message);}
 finally{$('authBtn').disabled=false;}
};

function showPage(page){
 document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
 $(page)?.classList.add('active');
 document.querySelectorAll('nav [data-page]').forEach(b=>
  b.classList.toggle('active',b.dataset.page===page));
 if(page==='tasks')loadTasks();
 if(page==='admin'&&isAdmin)loadAdmin();
}
window.showPage=showPage;
document.querySelectorAll('nav [data-page]').forEach(b=>
 b.onclick=()=>showPage(b.dataset.page));

async function loadTasks(){
 const box=$('taskList');box.replaceChildren();
 for(const t of tasks){
  const card=document.createElement('div');card.className='card';
  const title=document.createElement('div');title.className='task-title';
  title.textContent=t.name;
  const reward=document.createElement('p');reward.textContent=`Reward: ${t.coins} coins after approval`;
  const link=document.createElement('a');link.href=t.url;link.target='_blank';
  link.rel='noopener noreferrer';link.textContent='Open TikTok account';
  const proof=document.createElement('textarea');
  proof.placeholder='Paste your public proof link or describe the evidence';
  proof.maxLength=1000;
  const button=document.createElement('button');button.textContent='Submit for review';
  const status=document.createElement('p');status.className='small';
  try{
   const old=await getDoc(doc(db,'submissions',`${currentUser.uid}_${t.id}`));
   if(old.exists()){
    status.textContent='Status: '+(old.data().status||'submitted');
    proof.disabled=true;button.disabled=true;
   }
  }catch(e){status.textContent='Submission status unavailable.';}
  button.onclick=async()=>{
   const value=proof.value.trim();
   if(value.length<5){status.textContent='Add your proof before submitting.';return;}
   button.disabled=true;
   try{
    const ref=doc(db,'submissions',`${currentUser.uid}_${t.id}`);
    await runTransaction(db,async tx=>{
     const old=await tx.get(ref);
     if(old.exists())throw Error('This task has already been submitted.');
     tx.set(ref,{
      uid:currentUser.uid,taskId:t.id,taskName:t.name,
      reward:t.coins,proof:value,status:'pending',
      createdAt:serverTimestamp()
     });
    });
    status.textContent='Submitted. Waiting for admin review.';
    proof.disabled=true;
   }catch(e){status.textContent=e.message;button.disabled=false;}
  };
  card.append(title,reward,link,proof,button,status);
  box.append(card);
 }
}

async function refreshBalance(){
 const snap=await getDoc(doc(db,'users',currentUser.uid));
 const coins=snap.exists()?Number(snap.data().coins||0):0;
 $('homeCoins').textContent=coins;
 $('walletCoins').textContent=coins;
}

async function loadAdmin(){
 const box=$('adminList');box.replaceChildren();
 try{
  const result=await getDocs(query(collection(db,'submissions'),
   where('status','==','pending')));
  if(result.empty){box.textContent='No pending submissions.';return;}
  result.forEach(snap=>{
   const d=snap.data(),card=document.createElement('div');card.className='card';
   const title=document.createElement('strong');title.textContent=d.taskName||d.taskId;
   const details=document.createElement('p');
   details.textContent=`User: ${d.uid} • Reward: ${d.reward} coins`;
   const proof=document.createElement('p');proof.textContent='Proof: '+d.proof;
   const approve=document.createElement('button');approve.textContent='Approve';
   const reject=document.createElement('button');reject.textContent='Reject';
   reject.className='secondary';
   const status=document.createElement('p');
   approve.onclick=()=>review(snap.ref,d,true,status,approve,reject);
   reject.onclick=()=>review(snap.ref,d,false,status,approve,reject);
   card.append(title,details,proof,approve,reject,status);box.append(card);
  });
 }catch(e){box.textContent='Cannot load reviews: '+e.message;}
}

async function review(ref,data,approved,status,approve,reject){
 approve.disabled=true;reject.disabled=true;
 try{
  await runTransaction(db,async tx=>{
   const sub=await tx.get(ref);
   if(!sub.exists()||sub.data().status!=='pending')
    throw Error('This submission has already been reviewed.');
   if(approved){
    const userRef=doc(db,'users',data.uid);
    const user=await tx.get(userRef);
    if(!user.exists())throw Error('User profile not found.');
    tx.update(userRef,{coins:Number(user.data().coins||0)+Number(data.reward||0)});
   }
   tx.update(ref,{status:approved?'approved':'rejected',
    reviewedBy:currentUser.uid,reviewedAt:serverTimestamp()});
  });
  status.textContent=approved?'Approved.':'Rejected.';
  await refreshBalance();await loadAdmin();
 }catch(e){
  status.textContent=e.message;approve.disabled=false;reject.disabled=false;
 }
}

$('logoutBtn').onclick=()=>signOut(auth);
$('shareBtn').onclick=()=>{
 const url='https://t.me/taskcoin_ethiopia_bot/TaskCoinEthiopia';
 if(tg?.openTelegramLink)tg.openTelegramLink(
  'https://t.me/share/url?url='+encodeURIComponent(url));
 else if(navigator.share)navigator.share({title:'TaskCoin Ethiopia',url});
 else alert(url);
};

onAuthStateChanged(auth,async user=>{
 currentUser=user;
 $('auth').classList.toggle('hidden',!!user);
 $('app').classList.toggle('hidden',!user);
 $('nav').classList.toggle('hidden',!user);
 if(!user){isAdmin=false;return;}
 $('profileEmail').textContent=user.email||'';
 $('profileId').textContent=user.uid;
 try{
  const profile=await getDoc(doc(db,'users',user.uid));
  if(!profile.exists())await setDoc(doc(db,'users',user.uid),{
   email:user.email||'',coins:0,createdAt:serverTimestamp()
  });
  const admin=await getDoc(doc(db,'admins',user.uid));
  isAdmin=admin.exists()&&admin.data().enabled===true;
  $('adminNav').classList.toggle('hidden',!isAdmin);
  await refreshBalance();
  showPage('home');
 }catch(e){alert('Firebase setup or permissions need attention: '+e.message);}
});
</script>
</body>
</html>
     