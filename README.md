<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests & Auth</title>
  <link rel="icon" href="https://upload.wikimedia.org/wikipedia/commons/6/6b/Roblox_Logo_2022.svg" />
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('https://insider-gaming.com/wp-content/uploads/2025/05/grow-a-garden-update.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
    }
    header {
      background-color: rgba(0, 0, 0, 0.7);
      padding: 20px;
      font-size: 24px;
    }
    header small {
      font-size: 14px;
      display: block;
      margin-top: 6px;
    }
    section {
      background-color: rgba(0, 0, 0, 0.8);
      padding: 30px;
      margin: 30px auto;
      max-width: 600px;
      border-radius: 15px;
    }
    input,
    button {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
      font-size: 16px;
    }
    button {
      background-color: #4caf50;
      color: white;
      cursor: pointer;
      font-weight: bold;
      transition: transform 0.2s ease;
    }
    button:hover {
      background-color: #3e8e41;
      transform: scale(1.05);
    }
    button:active {
      transform: scale(1.1);
    }
    .entry {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 10px;
      border-radius: 10px;
      margin-top: 15px;
      text-align: left;
      white-space: pre-line;
    }
    .lang-switch {
      position: fixed;
      top: 10px;
      right: 10px;
    }
    select {
      background: rgba(255, 255, 255, 0.1);
      color: white;
      border: 1px solid white;
      border-radius: 6px;
      padding: 5px;
      font-size: 16px;
    }
    select option {
      background: black;
      color: white;
    }
    /* Auth box styles */
    #auth-box {
      background: rgba(0,0,0,0.7);
      padding: 20px;
      max-width: 400px;
      margin: 20px auto;
      border-radius: 10px;
      color: white;
      text-align: center;
    }
    #auth-box input {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      border: none;
      font-size: 16px;
    }
    #auth-box button {
      width: 94%;
      padding: 10px;
      border: none;
      border-radius: 5px;
      background: #4caf50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: transform 0.2s ease;
    }
    #auth-box button:hover {
      background: #3e8e41;
      transform: scale(1.05);
    }
    #auth-box button:active {
      transform: scale(1.1);
    }
    #auth-toggle {
      margin-top: 15px;
    }
    #auth-toggle a {
      color: #4caf50;
      cursor: pointer;
      text-decoration: none;
      font-weight: bold;
    }
    #auth-message {
      margin-top: 10px;
      min-height: 24px;
      font-weight: bold;
    }
    /* Hide main content if not logged in */
    #main-content.hidden {
      display: none;
    }
  </style>
</head>
<body>

  <!-- Языковой переключатель -->
  <div class="lang-switch">
    <label for="lang-select">🌐</label>
    <select id="lang-select" onchange="switchLang(this.value)">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en" selected>🇬🇧 English</option>
    </select>
  </div>

  <!-- Блок авторизации -->
  <div id="auth-box">
    <h3 id="auth-title">Register</h3>
    <input id="email" type="email" placeholder="Email" required />
    <input id="password" type="password" placeholder="Password" required />
    <button onclick="authAction()">Register</button>
    <div id="auth-toggle">
      <span id="toggle-text">Already have an account?</span>
      <a onclick="toggleAuth()">Sign In</a>
    </div>
    <div id="auth-message"></div>
  </div>

  <!-- Основной контент (показывается только после входа) -->
  <div id="main-content" class="hidden">

    <header>
      <span id="welcome-title">🌱 Welcome to the Grow a Garden website! 🌻</span>
      <small id="welcome-subtitle">Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.</small>
    </header>

    <section>
      <h2 id="title-buy">📥 Buy</h2>
      <form onsubmit="sendForm(event, 'buy')">
        <input
          type="text"
          data-placeholder="item"
          placeholder="What do you want to buy?"
          required
        />
        <input
          type="text"
          data-placeholder="nick"
          placeholder="Your Roblox nickname"
          required
        />
        <input
          type="text"
          data-placeholder="contact"
          placeholder="Contact (Discord etc.)"
        />
        <button type="submit" id="submit-buy">Submit</button>
      </form>
      <div class="entry" id="entries-buy"></div>
    </section>

    <section>
      <h2 id="title-sell">📤 Sell</h2>
      <form onsubmit="sendForm(event, 'sell')">
        <input
          type="text"
          data-placeholder="item"
          placeholder="What do you want to sell?"
          required
        />
        <input
          type="text"
          data-placeholder="price"
          placeholder="Price (optional)"
        />
        <input
          type="text"
          data-placeholder="nick"
          placeholder="Your Roblox nickname"
          required
        />
        <input
          type="text"
          data-placeholder="contact"
          placeholder="Contact (Discord etc.)"
        />
        <button type="submit" id="submit-sell">Submit</button>
      </form>
      <div class="entry" id="entries-sell"></div>
    </section>

    <section>
      <h2 id="title-trade">🔁 Trade</h2>
      <form onsubmit="sendForm(event, 'trade')">
        <input
          type="text"
          data-placeholder="give"
          placeholder="What are you giving?"
          required
        />
        <input
          type="text"
          data-placeholder="want"
          placeholder="What do you want in return?"
          required
        />
        <input
          type="text"
          data-placeholder="nick"
          placeholder="Your Roblox nickname"
          required
        />
        <input
          type="text"
          data-placeholder="contact"
          placeholder="Contact (Discord etc.)"
        />
        <button type="submit" id="submit-trade">Submit</button>
      </form>
      <div class="entry" id="entries-trade"></div>
    </section>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>

  <script>
    // Замените на свои параметры Firebase!
    const firebaseConfig = {
      apiKey: "ВАШ_API_КЛЮЧ",
      authDomain: "ВАШ_ДОМЕН.firebaseapp.com",
      projectId: "ВАШ_ID_ПРОЕКТА",
    };

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    let isRegister = true;

    function authAction() {
      const email = document.getElementById('email').value.trim();
      const password = document.getElementById('password').value.trim();
      const messageEl = document.getElementById('auth-message');

      messageEl.style.color = '#f44336';
      messageEl.textContent = '';

      if (!email || !password) {
        messageEl.textContent = 'Please enter email and password.';
        return;
      }

      if (isRegister) {
        auth.createUserWithEmailAndPassword(email, password)
          .then(() => {
            messageEl.style.color = 'lime';
            messageEl.textContent = 'Registration successful! You can now sign in.';
          })
          .catch(e => {
            messageEl.textContent = e.message;
          });
      } else {
        auth.signInWithEmailAndPassword(email, password)
          .then(() => {
            messageEl.style.color = 'lime';
            messageEl.textContent = 'Signed in successfully!';
            showMainContent();
          })
          .catch(e => {
            messageEl.textContent = e.message;
          });
      }
    }

    function toggleAuth() {
      isRegister = !isRegister;
      document.getElementById('auth-title').textContent = isRegister ? 'Register' : 'Sign In';
      document.querySelector('#auth-box button').textContent = isRegister ? 'Register' : 'Sign In';
      document.getElementById('toggle-text').textContent = isRegister ? 'Already have an account?' : "Don't have an account?";
      document.getElementById('auth-message').textContent = '';
    }

    function showMainContent() {
      document.getElementById('auth-box').style.display = 'none';
      document.getElementById('main-content').classList.remove('hidden');
    }

    // Автоматически проверяем, если пользователь уже вошёл
    auth.onAuthStateChanged(user => {
      if (user) {
        showMainContent();
      } else {
        document.getElementById('auth-box').style.display = 'block';
        document.getElementById('main-content').classList.add('hidden');
      }
    });

    // Заявки в Discord webhook
    const webhook =
      "https://discord.com/api/webhooks/1389234189504745675/kUOWAgPGTDDVmsuRdFMpp28aX8t8-ow7HNcumMAsYnMuJYOQFyEEtBRGag0iIZDXndDB";

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let message = `Request: ${type.toUpperCase()}\n`;

      inputs.forEach((input) => {
        message += `**${input.placeholder}**: ${input.value}\n`;
      });

      document.getElementById(`entries-${type}`).innerText = message;

      fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message }),
      });

      inputs.forEach((input) => (input.value = ""));
    }

    // Переводы и переключение языка
    function switchLang(lang) {
      const translations = {
        ru: {
          welcomeTitle: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
          welcomeSubtitle:
            "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
          buy: "📥 Купить",
          sell: "📤 Продать",
          trade: "🔁 Обмен",
          submit: "Отправить",
          item: "Что хотите купить?",
          nick: "Ваш Roblox ник",
          contact: "Контакт (Discord и т.п.)",
          price: "Цена (по желанию)",
          give: "Что вы даёте?",
          want: "Что хотите взамен?",
        },
        uk: {
          welcomeTitle: "🌱 Ласкаво просимо на сайт Grow a Garden! 🌻",
          welcomeSubtitle:
            "Тут ви можете подати заявки на купівлю, продаж і обмін предметів із гри Grow a Garden.",
          buy: "📥 Купити",
          sell: "📤 Продати",
          trade: "🔁 Обмін",
          submit: "Надіслати",
          item: "Що бажаєте купити?",
          nick: "Ваш Roblox нік",
          contact: "Контакт (Discord тощо)",
          price: "Ціна (за бажанням)",
          give: "Що ви віддаєте?",
          want: "Що хочете натомість?",
        },
        en: {
          welcomeTitle: "🌱 Welcome to the Grow a Garden website! 🌻",
          welcomeSubtitle:
            "Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.",
          buy: "📥 Buy",
          sell: "📤 Sell",
          trade: "🔁 Trade",
          submit: "Submit",
          item: "What do you want to buy?",
          nick: "Your Roblox nickname",
          contact: "Contact (Discord etc.)",
          price: "Price (optional)",
          give: "What are you giving?",
          want: "What do you want in return?",
        },
      };

      const t = translations[lang];

      document.getElementById("welcome-title").innerText = t.welcomeTitle;
      document.getElementById("welcome-subtitle").innerText = t.welcomeSubtitle;

      document.getElementById("title-buy").innerText = t.buy;
      document.getElementById("title-sell").innerText = t.sell;
      document.getElementById("title-trade").innerText = t.trade;

      document.getElementById("submit-buy").innerText = t.submit;
      document.getElementById("submit-sell").innerText = t.submit;
      document.getElementById("submit-trade").innerText = t.submit;

      document.querySelectorAll("input").forEach((input) => {
        const key = input.dataset.placeholder;
        if (t[key]) {
          input.placeholder = t[key];
        }
      });
    }

    // Устанавливаем язык по умолчанию
    switchLang("en");
  </script>
</body>
</html>
