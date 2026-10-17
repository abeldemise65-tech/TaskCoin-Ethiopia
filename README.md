 <!DOCTYPE html>
<html lang="am">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#080d0a">
<title>TaskCoin Ethiopia</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
:root{--bg:#080d0a;--card:#121a14;--green:#b7ff4a;--text:#f5f7f2;--muted:#9aa79c;--border:#27342a}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font-family:Arial,sans-serif}
button,input{font:inherit}
button{cursor:pointer}
.app{max-width:520px;min-height:100vh;margin:auto;padding:22px 16px 100px}
.brand{font-size:23px;font-weight:800;margin-bottom:26px}
.brand span{color:var(--green)}
.page{display:none}
.page.active{display:block}
.card{background:var(--card);border:1px solid var(--border);padding:18px;border-radius:18px;margin-bottom:14px}
h2{font-size:23px;margin:8px 0}
p,.muted{color:var(--muted);font-size:14px;line-height:1.6}
input{width:100%;padding:14px;background:#0b110d;color:white;border:1px solid var(--border);border-radius:12px;margin:8px 0 14px}
.btn{border:0;border-radius:12px;padding:13px 16px;font-weight:bold}
.primary{background:var(--green);color:#10170b}
.secondary{background:#202c22;color:white;border:1px solid var(--border)}
.full{width:100%}
.link{color:var(--green);background:none;border:0;padding:12px 0}
.hero{padding:22px;border-radius:22px;background:linear-gradient(140deg,#1d3020,#111812);border:1px solid #30472d;margin-bottom:22px}
.balance{font-size:36px;font-weight:800;margin:12px 0}
.balance small{font-size:14px;color:var(--green)}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.item{background:var(--card);padding:17px;border:1px solid var(--border);border-radius:16px;cursor:pointer}
.item b{display:block;margin:8px 0}
.icon{font-size:25px}
.nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:520px;display:none;grid-template-columns:repeat(5,1fr);background:#101711;border-top:1px solid var(--border);padding:10px 3px calc(10px + env(safe-area-inset-bottom));z-index:5}
.nav button{background:none;border:0;color:var(--muted);font-size:11px}
.nav button span{display:block;font-size:20px;margin-bottom:4px}
.nav button.active{color:var(--green)}
.notice{font-size:12px;color:var(--muted);line-height:1.6}
.task{display:flex;align-items:center;gap:12px}
.task .details{flex:1}
.error{color:#ff9d9d;font-size:13px}
.hidden{display:none!important}
</style>
</head>
<body>
<div class="app">
  <div class="brand">TaskCoin <span>Ethiopia 🇪🇹</span></div>

  <!-- LOGIN / REGISTER -->
  <section id="auth" class="page active">
    <div class="card">
      <h2 id="authTitle">እንኳን ወደ TaskCoin በደህና መጣህ!</h2>
      <p id="authDescription">ግባ ወይም አዲስ መለያ ፍጠር።</p>

      <div id="nameWrap" class="hidden">
        <label for="name">ሙሉ ስም</label>
        <input id="name" autocomplete="name" placeholder="ስምህን አስገባ">
      </div>

      <label for="email">Email</label>
      <input id="email" type="email" autocomplete="email" placeholder="name@example.com">

      <label for="password">Password</label>
      <input id="password" type="password" autocomplete="current-password" placeholder="ቢያንስ 6 ቁምፊ">

      <p id="authError" class="error"></p>
      <button id="submitBtn" class="btn primary full" onclick="submitAuth()">Login</button>
      <button id="switchBtn" class="link full" onclick="switchAuth()">አካውንት የለህም? Register</button>
      <p class="notice">የመለያ መረጃህ በFirebase Authentication ይጠበቃል። የሚያስፈልግ የይለፍ ቃልህን ለማንም አትስጥ።</p>
    </div>
  </section>

  <!-- HOME -->
  <section id="home" class="page">
    <div class="hero">
      <p>እንኳን ደህና መጣህ፣ <b id="welcomeName">User</b>!</p>
      <div class="muted">የእኔ የCoin ሂሳብ</div>
      <div class="balance"><span id="balance">0</span> <small>TC</small></div>
      <button class="btn primary" onclick="showPage('tasks')">Tasks ጀምር →</button>
    </div>
    <h2>ፈጣን መዳረሻ</h2>
    <div class="grid">
      <div class="item" onclick="showPage('tasks')"><span class="icon">📋</span><b>Tasks</b><p>ተግባሮችን ተመልከት።</p></div>
      <div class="item" onclick="showPage('invite')"><span class="icon">👥</span><b>Invite</b><p>ጓደኞችህን ጋብዝ።</p></div>
      <div class="item" onclick="showPage('wallet')"><span class="icon">💰</span><b>Wallet</b><p>የCoin ሂሳብህን ተመልከት።</p></div>
      <div class="item" onclick="showPage('profile')"><span class="icon">👤</span><b>Profile</b><p>የመለያ መረጃ።</p></div>
    </div>
  </section>

  <!-- TASKS -->
  <section id="tasks" class="page">
    <h2>📋 Tasks</h2>
    <div class="card task">
      <div class="icon">📱</div>
      <div class="details"><b>አፑን ተመልከት</b><p>የሙከራ ተግባር ነው።</p></div>
      <span class="muted">Demo</span>
    </div>
    <div class="card task">
      <div class="icon">🎬</div>
      <div class="details"><b>Video Task</b><p>በቅርቡ ይጨመራል።</p></div>
      <span class="muted">Soon</span>
    </div>
    <p class="notice">እውነተኛ Tasks እና ሽልማቶች ገና አልተገናኙም።</p>
  </section>

  <!-- INVITE -->
  <section id="invite" class="page">
    <h2>👥 Invite Friends</h2>
    <div class="card">
      <p>የግብዣ ሊንክህን አጋራ።</p>
      <input id="inviteLink" readonly>
      <button class="btn primary full" onclick="copyInvite()">ሊንኩን Copy አድርግ</button>
      <p id="inviteMessage" class="notice"></p>
    </div>
    <p class="notice">የግብዣ ሽልማት እስካሁን አልተነቃም።</p>
  </section>

  <!-- WALLET -->
  <section id="wallet" class="page">
    <h2>💰 Wallet</h2>
    <div class="hero">
      <div class="muted">የCoin ሂሳብ</div>
      <div class="balance"><span id="walletBalance">0</span> <small>TC</small></div>
      <p>ይህ ሂሳብ በሙከራ ላይ ነው። TC እውነተኛ ገንዘብ አይደለም።</p>
    </div>
    <div class="card">
      <h3>Withdraw</h3>
      <p>የገንዘብ ማውጣት ስርዓት ገና አልተገናኘም።</p>
      <button class="btn secondary full" onclick="alert('የክፍያ ስርዓቱ ገና አልተገናኘም።')">Withdraw — Soon</button>
    </div>
  </section>

  <!-- PROFILE -->
  <section id="profile" class="page">
    <h2>👤 Profile</h2>
    <div class="card">
      <div class="icon">🧑‍💻</div>
      <h3 id="profileName">User</h3>
      <p id="profileEmail" class="muted"></p>
      <p>TaskCoin Ethiopia Member</p>
      <button class="btn secondary full" onclick="logout()">Logout</button>
    </div>
  </section>
</div>

<nav id="nav" class="nav">
  <button class="active" data-page="home" onclick="showPage('home',this)"><span>🏠</span>Home</button>
  <button data-page="tasks" onclick="showPage('tasks',this)"><span>📋</span>Tasks</button>
  <button data-page="invite" onclick="showPage('invite',this)"><span>👥</span>Invite</button>
  <button data-page="wallet" onclick="showPage('wallet',this)"><span>💰</span>Wallet</button>
  <button data-page="profile" onclick="showPage('profile',this)"><span>👤</span>Profile</button>
</nav>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
import {
  getAuth, onAuthStateChanged, createUserWithEmailAndPassword,
  signInWithEmailAndPassword, signOut
} from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
import {
  getFirestore, doc, setDoc, getDoc, serverTimestamp
} from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

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
let mode = "login";
let currentUser = null;

const $ = id => document.getElementById(id);

window.switchAuth = function() {
  mode = mode === "login" ? "register" : "login";
  $("authTitle").textContent = mode === "login" ? "እንኳን ደህና መጣህ!" : "አዲስ መለያ ፍጠር";
  $("authDescription").textContent = mode === "login" ? "በመለያህ ግባ።" : "መረጃህን ሙላና ተመዝገብ።";
  $("nameWrap").classList.toggle("hidden", mode === "login");
  $("submitBtn").textContent = mode === "login" ? "Login" : "Register";
  $("switchBtn").textContent = mode === "login" ? "አካውንት የለህም? Register" : "አካውንት አለህ? Login";
  $("password").autocomplete = mode === "login" ? "current-password" : "new-password";
  $("authError").textContent = "";
};

window.submitAuth = async function() {
  const email = $("email").value.trim();
  const password = $("password").value;
  const name = $("name").value.trim();
  $("authError").textContent = "";

  if (!email || !password) {
    $("authError").textContent = "Email እና Password አስገባ።";
    return;
  }
  if (password.length < 6) {
    $("authError").textContent = "Password ቢያንስ 6 ቁምፊ መሆን አለበት።";
    return;
  }
  if (mode === "register" && !name) {
    $("authError").textContent = "ስምህን አስገባ።";
    return;
  }

  $("submitBtn").disabled = true;
  $("submitBtn").textContent = "ቆይ...";
  try {
    if (mode === "register") {
      const result = await createUserWithEmailAndPassword(auth, email, password);
      await setDoc(doc(db, "users", result.user.uid), {
        name, email, coins: 0, createdAt: serverTimestamp()
      });
    } else {
      await signInWithEmailAndPassword(auth, email, password);
    }
  } catch (error) {
    $("authError").textContent = friendlyError(error.code);
  } finally {
    $("submitBtn").disabled = false;
    $("submitBtn").textContent = mode === "login" ? "Login" : "Register";
  }
};

function friendlyError(code) {
  const errors = {
    "auth/email-already-in-use": "ይህ Email ቀድሞ ተጠቅሟል። Login ሞክር።",
    "auth/invalid-email": "Email ትክክል አይደለም።",
    "auth/weak-password": "ጠንካራ Password አስገባ።",
    "auth/invalid-credential": "Email ወይም Password ትክክል አይደለም።",
    "auth/network-request-failed": "Internet ግንኙነትህን አረጋግጥ።",
    "permission-denied": "Firestore Rules ማስተካከል ያስፈልጋል።"
  };
  return errors[code] || ("ስህተት ተፈጥሯል። (" + code + ")");
}

window.showPage = function(id, button) {
  if (!currentUser) return;
  document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
  $(id).classList.add("active");
  document.querySelectorAll(".nav button").forEach(b => b.classList.remove("active"));
  const navButton = button || document.querySelector('.nav button[data-page="' + id + '"]');
  if (navButton) navButton.classList.add("active");
  window.scrollTo({top: 0, behavior: "smooth"});
};

window.logout = async function() {
  await signOut(auth);
};

window.copyInvite = async function() {
  const field = $("inviteLink");
  try {
    await navigator.clipboard.writeText(field.value);
    $("inviteMessage").textContent = "ሊንኩ ተኮፒ ተደርጓል።";
  } catch {
    field.select();
    $("inviteMessage").textContent = "ሊንኩን ምረጥና Copy አድርግ።";
  }
};

onAuthStateChanged(auth, async user => {
  currentUser = user;
  if (!user) {
    $("nav").style.display = "none";
    document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
    $("auth").classList.add("active");
    return;
  }

  let name = user.displayName || user.email.split("@")[0];
  let coins = 0;
  try {
    const snap = await getDoc(doc(db, "users", user.uid));
    if (snap.exists()) {
      const data = snap.data();
      name = data.name || name;
      coins = Number(data.coins) || 0;
    } else {
      await setDoc(doc(db, "users", user.uid), {
        name, email: user.email, coins: 0, createdAt: serverTimestamp()
      });
    }
  } catch (error) {
    console.error("Profile loading error:", error);
  }

  $("welcomeName").textContent = name;
  $("profileName").textContent = name;
  $("profileEmail").textContent = user.email || "";
  $("balance").textContent = coins;
  $("walletBalance").textContent = coins;
  $("inviteLink").value = "https://t.me/taskcoin_ethiopia_bot/TaskCoinEthiopia";
  $("nav").style.display = "grid";

  document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
  $("home").classList.add("active");
  document.querySelectorAll(".nav button").forEach(b => b.classList.remove("active"));
  document.querySelector('.nav button[data-page="home"]').classList.add("active");
});

const tg = window.Telegram?.WebApp;
if (tg) {
  tg.ready();
  tg.expand();
}
</script>
</body>
</html>