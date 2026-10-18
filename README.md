<!DOCTYPE html>
<html lang="am">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>TaskCoin Ethiopia</title>

  <script src="https://telegram.org/js/telegram-web-app.js"></script>

  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 20px;
      background: #0b0b0b;
      color: white;
      font-family: Arial, sans-serif;
    }
    .container { max-width: 450px; margin: auto; }
    .card {
      background: #171717;
      border: 1px solid #303030;
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 16px;
    }
    h1 { color: #35d07f; text-align: center; }
    h2 { margin-top: 0; }
    button {
      width: 100%;
      padding: 14px;
      margin-top: 10px;
      border: 0;
      border-radius: 10px;
      background: #35d07f;
      color: #07140c;
      font-size: 16px;
      font-weight: bold;
    }
    .secondary { background: #292929; color: white; }
    .muted { color: #b8b8b8; line-height: 1.5; }
    .balance { font-size: 30px; color: #35d07f; font-weight: bold; }
    .hidden { display: none; }
    nav {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
      margin-bottom: 16px;
    }
    nav button { margin: 0; padding: 12px 4px; font-size: 14px; }
    #status { color: #ffcc66; overflow-wrap: anywhere; }
  </style>
</head>

<body>
<div class="container">
  <h1>🪙 TaskCoin Ethiopia</h1>

  <div class="card" id="login">
    <h2>በTelegram ግባ</h2>
    <p class="muted">
      ይህን Mini App ከTelegram ውስጥ ክፈት።
      የTelegram ፓስወርድህን ወይም SMS ኮድህን አናስገባም።
    </p>
    <p id="userInfo">የTelegram መረጃ እየተፈተሸ ነው...</p>
    <button id="continueBtn">ቀጥል</button>
    <p class="muted" id="status" role="status"></p>
  </div>

  <div id="app" class="hidden">
    <div class="card">
      <p class="muted">የእርስዎ ቀሪ ሳንቲም</p>
      <div class="balance"><span id="coins">0</span> 🪙</div>
      <p class="muted">ማስታወሻ፦ ይህ የማሳያ ቀሪ ነው፤ እውነተኛ ክፍያ አይደለም።</p>
    </div>

    <nav>
      <button data-page="home">Home</button>
      <button data-page="tasks">Tasks</button>
      <button data-page="invite">Invite</button>
    </nav>
    <nav>
      <button data-page="wallet">Wallet</button>
      <button data-page="profile">Profile</button>
    </nav>

    <div class="card" id="page">
      <h2>እንኳን ደህና መጣህ!</h2>
      <p class="muted">TaskCoin Ethiopia አፕህ ተከፍቷል።</p>
    </div>
  </div>
</div>

<script>
(() => {
  const tg = window.Telegram?.WebApp;
  const $ = id => document.getElementById(id);
  let telegramUser = null;

  if (tg) {
    tg.ready();
    tg.expand();
    telegramUser = tg.initDataUnsafe?.user || null;
  }

  if (telegramUser) {
    $("userInfo").textContent =
      "ሰላም " + (telegramUser.first_name || "ተጠቃሚ") +
      " 👋 የTelegram መረጃ ተገኝቷል።";
    $("status").textContent =
      "ማስታወሻ፦ ይህ መረጃ በserver እስካልተረጋገጠ ድረስ የተረጋገጠ መግቢያ አይደለም።";
  } else {
    $("userInfo").textContent =
      "የTelegram መረጃ አልተገኘም። አፑን ከTelegram Bot ውስጥ ክፈት።";
    $("status").textContent =
      "በChrome ውስጥ ብቻ ከከፈትከው የTelegram መረጃ አይታይም።";
  }

  $("continueBtn").addEventListener("click", () => {
    if (!tg || !telegramUser) {
      $("status").textContent =
        "እባክህ በTelegram ውስጥ ያለውን Bot ክፈትና Mini App አስነሳ።";
      return;
    }

    $("login").classList.add("hidden");
    $("app").classList.remove("hidden");
  });

  const pages = {
    home: ["እንኳን ደህና መጣህ!", "TaskCoin Ethiopia አፕህ ተከፍቷል።"],
    tasks: ["Tasks", "የሚገኙ ስራዎች በቅርቡ ይጨመራሉ። እዚህ ላይ እስካሁን ሽልማት አይሰጥም።"],
    invite: ["Invite Friends", "የግብዣ ሽልማት ለመስራት በserver የሚረጋገጥ የግብዣ ስርዓት ያስፈልጋል።"],
    wallet: ["Wallet", "የክፍያ ስርዓት ገና አልተገናኘም። ይህ ገጽ ገንዘብ አያወጣም።"],
    profile: ["Profile", "የTelegram ስም: " + (telegramUser?.first_name || "አልተገኘም")]
  };

  document.querySelectorAll("[data-page]").forEach(button => {
    button.addEventListener("click", () => {
      const [title, description] = pages[button.dataset.page];
      $("page").innerHTML = "<h2></h2><p class='muted'></p>";
      $("page").querySelector("h2").textContent = title;
      $("page").querySelector("p").textContent = description;
    });
  });
})();
</script>
</body>
</html>