<!DOCTYPE html>
<html lang="am">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#080d0a">
  <title>TaskCoin Ethiopia</title>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>

  <style>
    :root {
      --bg: #080d0a;
      --card: #121a14;
      --green: #00d995;
      --text: #f5f7f2;
      --muted: #9aaa9c;
      --border: #27342a;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font-family: Arial, sans-serif;
    }

    button, input { font: inherit; }

    button {
      cursor: pointer;
      border: 0;
      border-radius: 12px;
      padding: 14px;
      font-weight: bold;
    }

    input {
      width: 100%;
      padding: 14px;
      margin: 8px 0 14px;
      color: white;
      background: #080d0a;
      border: 1px solid var(--border);
      border-radius: 10px;
    }

    .wrap {
      width: min(100%, 520px);
      margin: auto;
      padding: 20px 16px 100px;
    }

    header {
      text-align: center;
      padding: 18px 0;
      border-bottom: 1px solid var(--border);
    }

    header h1 {
      color: var(--green);
      font-size: 25px;
      margin: 0;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: 20px;
      margin-top: 18px;
    }

    h2 { font-size: 21px; }
    .muted { color: var(--muted); line-height: 1.6; }
    .green { color: var(--green); }

    .primary {
      width: 100%;
      color: #00170e;
      background: var(--green);
      margin-top: 8px;
    }

    .secondary {
      width: 100%;
      color: var(--text);
      background: #26352b;
      margin-top: 8px;
    }

    .hidden { display: none !important; }

    .tabs {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      display: flex;
      justify-content: space-around;
      gap: 4px;
      padding: 10px 3px;
      background: #101711;
      border-top: 1px solid var(--border);
      z-index: 5;
    }

    .tabs button {
      flex: 1;
      padding: 10px 2px;
      color: var(--muted);
      background: transparent;
      font-size: 12px;
    }

    .tabs button.active { color: var(--green); }

    .task {
      padding: 15px 0;
      border-bottom: 1px solid var(--border);
    }

    .notice {
      font-size: 13px;
      color: #f3c66b;
      line-height: 1.6;
      margin-top: 12px;
    }

    #message {
      margin-top: 12px;
      line-height: 1.5;
      overflow-wrap: anywhere;
    }

    .stat {
      font-size: 30px;
      font-weight: bold;
      color: var(--green);
    }
  </style>
</head>

<body>
  <div class="wrap">
    <header>
      <h1>🪙 TaskCoin Ethiopia</h1>
      <p class="muted">Tasks • Coins • Rewards</p>
    </header>

    <section id="auth">
      <div class="card">
        <h2 id="authTitle">አዲስ መለያ ፍጠር</h2>
        <p class="muted">
          በኢሜይልህ ተመዝገብ ወይም ወደ መለያህ ግባ።
        </p>

        <form id="authForm">
          <label for="email">ኢሜይል</label>
          <input id="email" type="email"
                 placeholder="name@example.com"
                 autocomplete="email" required>

          <label for="password">የይለፍ ቃል</label>
          <input id="password" type="password"
                 placeholder="ቢያንስ 6 ቁምፊዎች"
                 minlength="6"
                 autocomplete="current-password" required>

          <button class="primary" id="submitAuth" type="submit">
            ተመዝገብ
          </button>
        </form>

        <button class="secondary" id="toggleAuth" type="button">
          መለያ አለኝ — ግባ
        </button>

        <div id="message" role="status"></div>
      </div>
    </section>

    <main id="app" class="hidden">
      <section id="homePage" class="page">
        <div class="card">
          <p class="muted">እንኳን ደህና መጣህ!</p>
          <h2 id="welcome">TaskCoin Ethiopia</h2>
          <p class="muted">የመለያህ ኢሜይል፦</p>
          <p id="userEmail"></p>
        </div>

        <div class="card">
          <p class="muted">የCoins ሂሳብ</p>
          <div class="stat">0 Coins</div>
          <p class="notice">
            ይህ የመጀመሪያ ስሪት ነው። እውነተኛ ሽልማቶች
            እስካሁን አልተነቃቁም።
          </p>
        </div>

        <div class="card">
          <h2>📋 የTasks ክፍል</h2>
          <p class="muted">
            የሚገኙ Tasks እና የሽልማት ስርዓት በቀጣይ
            ይጨመራሉ።
          </p>
        </div>
      </section>

      <section id="tasksPage" class="page hidden">
        <div class="card">
          <h2>📋 Tasks</h2>
          <div class="task">
            <strong>Tasks በቅርቡ ይጀምራሉ</strong>
            <p class="muted">
              ሽልማት የሚያገኙ ተግባራት እና ማረጋገጫቸው
              እየተዘጋጁ ነው።
            </p>
          </div>
        </div>
      </section>

      <section id="invitePage" class="page hidden">
        <div class="card">
          <h2>👥 ጓደኞችን ጋብዝ</h2>
          <p class="muted">
            የግብዣ ሊንክና የInvite ሽልማት ስርዓት
            በቀጣይ ይጨመራሉ።
          </p>
        </div>
      </section>

      <section id="walletPage" class="page hidden">
        <div class="card">
          <h2>👛 Wallet</h2>
          <p class="muted">የCoins ሂሳብ፦ 0</p>
          <p class="notice">
            ገንዘብ ማውጣት ገና አልተነቃም።
            እውነተኛ ክፍያ እስካሁን አይፈጸምም።
          </p>
        </div>
      </section>

      <section id="profilePage" class="page hidden">
        <div class="card">
          <h2>👤 Profile</h2>
          <p class="muted">ኢሜይል፦</p>
          <p id="profileEmail"></p>
          <button class="secondary" id="logout" type="button">
            ውጣ
          </button>
        </div>
      </section>
    </main>
  </div>

  <nav id="tabs" class="tabs hidden">
    <button class="active" data-page="homePage">🏠<br>ቤት</button>
    <button data-page="tasksPage">📋<br>Tasks</button>
    <button data-page="invitePage">👥<br>Invite</button>
    <button data-page="walletPage">👛<br>Wallet</button>
    <button data-page="profilePage">👤<br>Profile</button>
  </nav>

  <script type="module">
    import { initializeApp }
      from "https://www.gstatic.com/firebasejs/11.10.0/firebase-app.js";

    import {
      getAuth,
      createUserWithEmailAndPassword,
      signInWithEmailAndPassword,
      onAuthStateChanged,
      signOut
    } from "https://www.gstatic.com/firebasejs/11.10.0/firebase-auth.js";

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

    const $ = (id) => document.getElementById(id);

    let isRegister = true;

    function showMessage(text, error = false) {
      $("message").textContent = text;
      $("message").style.color = error ? "#ff8b8b" : "#00d995";
    }

    function firebaseError(error) {
      const messages = {
        "auth/email-already-in-use": "ይህ ኢሜይል ቀድሞ ተመዝግቧል። ግባ።",
        "auth/invalid-email": "ኢሜይሉ ትክክል አይደለም።",
        "auth/weak-password": "የይለፍ ቃሉን አጠናክር።",
        "auth/invalid-credential": "ኢሜይሉ ወይም የይለፍ ቃሉ ትክክል አይደለም።",
        "auth/network-request-failed": "ኢንተርኔትህን አረጋግጥ።",
        "auth/too-many-requests": "ብዙ ሙከራዎች ተደርገዋል። ቆይተህ ሞክር።",
        "auth/operation-not-allowed": "በFirebase ውስጥ Email/Password አልተነቃም።"
      };

      return messages[error.code] ||
        "ስህተት ተፈጥሯል። ኢንተርኔትህንና Firebase ቅንብርህን አረጋግጥ።";
    }

    $("toggleAuth").addEventListener("click", () => {
      isRegister = !isRegister;

      $("authTitle").textContent = isRegister
        ? "አዲስ መለያ ፍጠር"
        : "ወደ መለያህ ግባ";

      $("submitAuth").textContent = isRegister
        ? "ተመዝገብ"
        : "ግባ";

      $("toggleAuth").textContent = isRegister
        ? "መለያ አለኝ — ግባ"
        : "አዲስ ነኝ — ተመዝገብ";

      $("password").autocomplete = isRegister
        ? "new-password"
        : "current-password";

      showMessage("");
    });

    $("authForm").addEventListener("submit", async (event) => {
      event.preventDefault();

      const email = $("email").value.trim();
      const password = $("password").value;

      $("submitAuth").disabled = true;
      $("submitAuth").textContent = "ቆይ...";

      try {
        if (isRegister) {
          await createUserWithEmailAndPassword(auth, email, password);
        } else {
          await signInWithEmailAndPassword(auth, email, password);
        }
      } catch (error) {
        showMessage(firebaseError(error), true);
      } finally {
        $("submitAuth").disabled = false;
        $("submitAuth").textContent = isRegister ? "ተመዝገብ" : "ግባ";
      }
    });

    onAuthStateChanged(auth, (user) => {
      const loggedIn = Boolean(user);

      $("auth").classList.toggle("hidden", loggedIn);
      $("app").classList.toggle("hidden", !loggedIn);
      $("tabs").classList.toggle("hidden", !loggedIn);

      if (user) {
        $("welcome").textContent = "እንኳን ደህና መጣህ!";
        $("userEmail").textContent = user.email || "";
        $("profileEmail").textContent = user.email || "";
      } else {
        showMessage("");
      }
    });

    $("logout").addEventListener("click", async () => {
      try {
        await signOut(auth);
      } catch (error) {
        alert(firebaseError(error));
      }
    });

    document.querySelectorAll(".tabs button").forEach((button) => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".page").forEach((page) => {
          page.classList.add("hidden");
        });

        $(button.dataset.page).classList.remove("hidden");

        document.querySelectorAll(".tabs button").forEach((tab) => {
          tab.classList.remove("active");
        });

        button.classList.add("active");
      });
    });

    if (window.Telegram?.WebApp) {
      window.Telegram.WebApp.ready();
      window.Telegram.WebApp.expand();
    }
  </script>
</body>
</html>