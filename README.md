<!DOCTYPE html>
<html lang="am">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#080808">
  <title>TaskCoin Ethiopia</title>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: Arial, sans-serif;
      background: #080808;
      color: white;
      min-height: 100vh;
      padding-bottom: 85px;
    }

    .container {
      width: 100%;
      max-width: 480px;
      margin: auto;
      padding: 20px 16px;
    }

    .logo {
      text-align: center;
      color: #00e676;
      font-size: 27px;
      font-weight: bold;
      margin: 18px 0 8px;
    }

    .subtitle {
      text-align: center;
      color: #aaa;
      margin-bottom: 25px;
    }

    .card {
      background: #151515;
      border: 1px solid #292929;
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 16px;
    }

    h2 {
      font-size: 21px;
      margin-bottom: 16px;
    }

    h3 {
      margin-bottom: 12px;
    }

    p {
      line-height: 1.6;
    }

    label {
      display: block;
      color: #ddd;
      margin: 14px 0 8px;
    }

    input {
      width: 100%;
      padding: 14px;
      border-radius: 10px;
      border: 1px solid #383838;
      background: #0d0d0d;
      color: white;
      font-size: 16px;
      outline: none;
    }

    input:focus {
      border-color: #00e676;
    }

    button {
      width: 100%;
      padding: 14px;
      border: none;
      border-radius: 10px;
      background: #00e676;
      color: #001b0d;
      font-weight: bold;
      font-size: 16px;
      margin-top: 14px;
      cursor: pointer;
    }

    button:disabled {
      opacity: 0.6;
    }

    .secondary {
      background: #292929;
      color: white;
    }

    .small {
      color: #aaa;
      font-size: 13px;
      margin-top: 12px;
    }

    .message {
      margin-top: 14px;
      font-size: 14px;
      overflow-wrap: anywhere;
    }

    .success {
      color: #00e676;
    }

    .error {
      color: #ff7373;
    }

    .hidden {
      display: none !important;
    }

    .balance {
      font-size: 32px;
      color: #00e676;
      font-weight: bold;
      margin: 10px 0;
    }

    .coin {
      font-size: 42px;
      margin-bottom: 8px;
    }

    .task-button {
      text-align: left;
      background: #202020;
      color: white;
      border: 1px solid #333;
    }

    .task-button span {
      display: block;
      color: #aaa;
      font-size: 13px;
      margin-top: 6px;
      font-weight: normal;
    }

    .status {
      display: inline-block;
      padding: 6px 10px;
      border-radius: 20px;
      background: #183d2a;
      color: #00e676;
      font-size: 12px;
      margin-bottom: 12px;
    }

    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: #111;
      border-top: 1px solid #292929;
      display: flex;
      justify-content: space-around;
      padding: 10px 2px;
      z-index: 10;
    }

    .bottom-nav button {
      background: transparent;
      color: #aaa;
      padding: 7px 2px;
      margin: 0;
      font-size: 11px;
      width: 20%;
    }

    .bottom-nav button.active {
      color: #00e676;
    }

    .bottom-nav .nav-icon {
      display: block;
      font-size: 20px;
      margin-bottom: 5px;
    }

    #recaptcha-container {
      margin-top: 15px;
      overflow: hidden;
    }

    .green {
      color: #00e676;
    }

    a {
      color: #00e676;
      overflow-wrap: anywhere;
    }
  </style>
</head>

<body>

  <!-- LOGIN / REGISTER -->
  <section id="authScreen" class="container">
    <div class="logo">🪙 TaskCoin Ethiopia</div>
    <p class="subtitle">ስራ ስራ፤ ኮይን ሰብስብ!</p>

    <div class="card">
      <h2 id="authTitle">አካውንት ፍጠር</h2>

      <p class="small">
        ስልክ ቁጥርህን አስገባ። በSMS የሚላክልህን
        የማረጋገጫ ኮድ በመጠቀም ግባ።
      </p>

      <label for="phone">የስልክ ቁጥር</label>
      <input
        id="phone"
        type="tel"
        inputmode="tel"
        autocomplete="tel"
        placeholder="+2519XXXXXXXX"
      >

      <div id="recaptcha-container"></div>

      <button id="sendCodeBtn" type="button">
        የSMS ኮድ ላክ
      </button>

      <div id="otpArea" class="hidden">
        <label for="otp">የማረጋገጫ ኮድ (OTP)</label>
        <input
          id="otp"
          type="text"
          inputmode="numeric"
          autocomplete="one-time-code"
          maxlength="6"
          placeholder="6 አሃዝ ኮድ"
        >

        <button id="verifyBtn" type="button">
          አረጋግጥ እና ግባ
        </button>

        <button id="resendBtn" class="secondary" type="button">
          ኮዱን እንደገና ላክ
        </button>
      </div>

      <div id="authMessage" class="message" aria-live="polite"></div>

      <p class="small">
        ስልክ ቁጥርህ በአገር ኮድ መጀመር አለበት።
        ለኢትዮጵያ +251 ተጠቀም።
      </p>
    </div>
  </section>

  <!-- MAIN APP -->
  <main id="appScreen" class="container hidden">

    <div class="logo">🪙 TaskCoin Ethiopia</div>
    <p class="subtitle">እንኳን ደህና መጣህ!</p>

    <!-- HOME -->
    <section id="homePage" class="page">
      <div class="card">
        <span class="status">● አካውንትህ ተገናኝቷል</span>
        <h2>ሰላም! 👋</h2>
        <p>በTaskCoin Ethiopia ወደ የተግባር ጉዞህ በደህና ግባ።</p>
      </div>

      <div class="card">
        <div