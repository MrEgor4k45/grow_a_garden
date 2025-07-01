<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Заявки</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('https://insider-gaming.com/wp-content/uploads/2025/05/grow-a-garden-update.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 10px;
    }
    .overlay {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      margin: 20px auto 10px;
      border-radius: 12px;
      max-width: 700px;
      width: 100%;
    }
    section {
      background-color: rgba(0, 0, 0, 0.75);
      padding: 20px;
      margin: 10px auto;
      max-width: 600px;
      border-radius: 15px;
      width: 100%;
    }
    input, button, select {
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
      max-height: 200px;
      overflow-y: auto;
    }
    .lang-switch {
      position: fixed;
      top: 10px;
      right: 10px;
      background: rgba(0,0,0,0.6);
      border-radius: 8px;
      padding: 5px 10px;
      z-index: 999;
    }
    select {
      background: rgba(255,255,255,0.1);
      color: white;
      border: 1px solid white;
    }
    select option {
      background: black;
      color: white;
    }
    #auth-section {
      margin-bottom: 20px;
    }
    #balance {
      font-weight: bold;
      margin-top: 10px;
      color: #4caf50;
    }
    #logout-btn {
      background-color: #f44336;
      margin-top: 5px;
      width: auto;
      padding: 5px 15px;
    }
  </style>
</head>
<body>

  <div class="lang-switch">
    <select id="lang-select">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en">🇬🇧 English</option>
    </select>
  </div>

  <div id="auth-section" class="overlay">
    <div id="user-info" style="display:none;">
      <div id="user-email"></div>
      <div id="balance">Баланс: <span id="user-balance">0</span></div>
      <button id="logout-btn">Выйти</button>
    </div>
    <div id="login-section">
      <button id="google-signin-btn">Войти через Google</button>
    </div>
  </div>

  <div class="overlay" id="welcome-section" style="display:none;">
    <h1 id="welcome-title">🌱 Добро пожаловать на сайт Grow a Garden Shop! 🌻</h1>
    <p id="welcome-desc">Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden Shop.</p>
  </div>

  <section id="buy-section" style="display:none;">
    <h2 id="title-buy">📥 Купить</h2>
    <form id="form-buy">
      <input type="text" placeholder="Что вы хотите купить?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (пожалуйста пишите в начале DS, TG и т. п.)" />
      <button type="submit" id="btn-buy">Отправить</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section id="sell-section" style="display:none;">
    <h2 id="title-sell">📤 Продать</h2>
    <form id="form-sell">
      <input type="text" placeholder="Что вы продаёте?" required />
      <input type="text" placeholder="Цена (необязательно)" />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (пожалуйста пишите в начале DS, TG и т. п.)" />
      <button type="submit" id="btn-sell">Отправить</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section id="trade-section" style="display:none;">
    <h2 id="title-trade">🔁 Обмен</h2>
    <form id="form-trade">
      <input type="text" placeholder="Что вы отдаёте?" required />
      <input type="text" placeholder="Что хотите взамен?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (пожалуйста пишите в начале DS, TG и т. п.)" />
      <button type="submit" id="btn-trade">Отправить</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    // --- Переключение языков ---
    const translations = {
      ru: {
        welcomeTitle: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
        welcomeDesc: "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
        buyTitle: "📥 Купить",
        sellTitle: "📤 Продать",
        tradeTitle: "🔁 Обмен",
        placeholders: {
          buy: ["Что вы хотите купить?", "Ваш ник в Roblox", "Контакт (пожалуйста пишите в начале DS, TG и т. п.)"],
          sell: ["Что вы продаёте?", "Цена (необязательно)", "Ваш ник в Roblox", "Контакт (пожалуйста пишите в начале DS, TG и т. п.)"],
          trade: ["Что вы отдаёте?", "Что хотите взамен?", "Ваш ник в Roblox", "Контакт (пожалуйста пишите в начале DS, TG и т. п.)"],
        },
        sendBtn: "Отправить",
        logoutBtn: "Выйти",
        loginGoogleBtn: "Войти через Google",
        balanceText: "Баланс",
      },
      uk: {
        welcomeTitle: "🌱 Ласкаво просимо на сайт Grow a Garden! 🌻",
        welcomeDesc: "Тут ви можете подати заявки на купівлю, продаж і обмін предметів із гри Grow a Garden.",
        buyTitle: "📥 Купити",
        sellTitle: "📤 Продати",
        tradeTitle: "🔁 Обмін",
        placeholders: {
          buy: ["Що ви хочете купити?", "Ваш нік в Roblox", "Контакт (Будь ласка, напишіть на початку DS, TG тощо)"],
          sell: ["Що ви продаєте?", "Ціна (необов'язково)", "Ваш нік в Roblox", "Контакт (Будь ласка, напишіть на початку DS, TG тощо)"],
          trade: ["Що ви віддаєте?", "Що хочете натомість?", "Ваш нік в Roblox", "Контакт (Будь ласка, напишіть на початку DS, TG тощо)"],
        },
        sendBtn: "Відправити",
        logoutBtn: "Вийти",
        loginGoogleBtn: "Увійти через Google",
        balanceText: "Баланс",
      },
      en: {
        welcomeTitle: "🌱 Welcome to the Grow a Garden website! 🌻",
        welcomeDesc: "Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.",
        buyTitle: "📥 Buy",
        sellTitle: "📤 Sell",
        tradeTitle: "🔁 Trade",
        placeholders: {
          buy: ["What do you want to buy?", "Your Roblox nickname", "Contact (Please write at the beginning of DS, TG etc.)"],
          sell: ["What do you want to sell?", "Price (optional)", "Your Roblox nickname", "Contact (Please write at the beginning of DS, TG etc.)"],
          trade: ["What are you giving?", "What do you want in return?", "Your Roblox nickname", "Contact (Please write at the beginning of DS, TG etc.)"],
        },
        sendBtn: "Send",
        logoutBtn: "Logout",
        loginGoogleBtn: "Sign in with Google",
        balanceText: "Balance",
      }
    };
    let currentLang = "ru";

    function updateTexts() {
      const t = translations[currentLang];

      document.getElementById("welcome-title").innerText = t.welcomeTitle;
      document.getElementById("welcome-desc").innerText = t.welcomeDesc;

      document.getElementById("title-buy").innerText = t.buyTitle;
      document.getElementById("title-sell").innerText = t.sellTitle;
      document.getElementById("title-trade").innerText = t.tradeTitle;

      // placeholders
      const formBuyInputs = document.querySelectorAll("#form-buy input");
      t.placeholders.buy.forEach((ph, i) => {
        if(formBuyInputs[i]) formBuyInputs[i].placeholder = ph;
      });

      const formSellInputs = document.querySelectorAll("#form-sell input");
      t.placeholders.sell.forEach((ph, i) => {
        if(formSellInputs[i]) formSellInputs[i].placeholder = ph;
      });

      const formTradeInputs = document.querySelectorAll("#form-trade input");
      t.placeholders.trade.forEach((ph, i) => {
        if(formTradeInputs[i]) formTradeInputs[i].placeholder = ph;
      });

      // buttons
      document.getElementById("btn-buy").innerText = t.sendBtn;
      document.getElementById("btn-sell").innerText = t.sendBtn;
      document.getElementById("btn-trade").innerText = t.sendBtn;

      document.getElementById("logout-btn").innerText = t.logoutBtn;
      document.getElementById("google-signin-btn").innerText = t.loginGoogleBtn;

      document.getElementById("balance").childNodes[0].nodeValue = t.balanceText + ": ";
    }

    document.getElementById("lang-select").addEventListener("change", e => {
      currentLang = e.target.value;
      updateTexts();
    });

    // --- Firebase ---
    const firebaseConfig = {
      apiKey: "AIzaSyCohztyLEbSq2HH4IiMfjnb_UMB2-zwoyw",
      authDomain: "gag-4a6bd.firebaseapp.com",
      databaseURL: "https://gag-4a6bd-default-rtdb.europe-west1.firebasedatabase.app",
      projectId: "gag-4a6bd",
      storageBucket: "gag-4a6bd.firebasestorage.app",
      messagingSenderId: "355235183308",
      appId: "1:355235183308:web:a9b50b7e31e2a276502069"
    };
    firebase.initializeApp(firebaseConfig);

    const auth = firebase.auth();
    const db = firebase.database();

    // Discord Webhook URL
    const discordWebhook = "https://discord.com/api/webhooks/1389489483812175892/xVBCE4BDw5JzAtuOx3NmJ-gj7FpaYdFykNlcifVugL-Sax88lAN_mFcD6qI-DPCx81jG";

    // --- Авторизация ---
    const userInfo = document.getElementById("user-info");
    const loginSection = document.getElementById("login-section");
    const userEmail = document.getElementById("user-email");
    const userBalanceEl = document.getElementById("user-balance");
    const logoutBtn = document.getElementById("logout-btn");
    const welcomeSection = document.getElementById("welcome-section");

    const buySection = document.getElementById("buy-section");
    const sellSection = document.getElementById("sell-section");
    const tradeSection = document.getElementById("trade-section");

    function showAppForUser(user) {
      userEmail.textContent = `Пользователь: ${user.email}`;
      userInfo.style.display = "block";
      loginSection.style.display = "none";
      welcomeSection.style.display = "block";
      buySection.style.display = "block";
      sellSection.style.display = "block";
      tradeSection.style.display = "block";
      loadUserBalance(user.uid);
    }

    function hideApp() {
      userInfo.style.display = "none";
      loginSection.style.display = "block";
      welcomeSection.style.display = "none";
      buySection.style.display = "none";
      sellSection.style.display = "none";
      tradeSection.style.display = "none";
    }

    // Загрузка баланса из БД или установка 0
    function loadUserBalance(uid) {
      const balanceRef = db.ref('users/' + uid + '/balance');
      balanceRef.once('value').then(snapshot => {
        if(snapshot.exists()) {
          userBalanceEl.textContent = snapshot.val();
        } else {
          balanceRef.set(0);
          userBalanceEl.textContent = '0';
        }
      });
    }

    // Обработчик входа через Google
    document.getElementById("google-signin-btn").addEventListener("click", () => {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider).catch(err => alert("Ошибка входа: " + err.message));
    });

    // Обработчик выхода
    logoutBtn.addEventListener("click", () => {
      auth.signOut();
    });

    // Отслеживание состояния авторизации
    auth.onAuthStateChanged(user => {
      if(user) {
        showAppForUser(user);
      } else {
        hideApp();
      }
      updateTexts();
    });

    // Добавление заявки + отправка в Discord
    function addEntry(type, data) {
      const newRef = db.ref(type).push();
      newRef.set(data);

      let discordMessage = `📝 Заявка: ${type.toUpperCase()}\n`;
      for (const key in data) {
        discordMessage += `**${key}**: ${data[key]}\n`;
      }

      fetch(discordWebhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: discordMessage }),
      });
    }

    // Прослушка заявок и отображение
    function listenEntries(type, containerId) {
      const container = document.getElementById(containerId);
      const ref = db.ref(type);
      ref.on('value', (snapshot) => {
        const val = snapshot.val();
        container.innerHTML = '';
        if (val) {
          Object.values(val).forEach(entry => {
            let text = '';
            for (const key in entry) {
              text += `${key}: ${entry[key]}\n`;
            }
            const div = document.createElement('div');
            div.style.border = '1px solid #4caf50';
            div.style.marginBottom = '10px';
            div.style.padding = '8px';
            div.style.borderRadius = '6px';
            div.style.backgroundColor = 'rgba(76, 175, 80, 0.1)';
            div.textContent = text;
            container.appendChild(div);
          });
        } else {
          container.textContent = {
            ru: 'Заявок пока нет.',
            uk: 'Заявок поки немає.',
            en: 'No requests yet.'
          }[currentLang];
        }
      });
    }

    // Обработчики форм
    document.getElementById('form-buy').addEventListener('submit', e => {
      e.preventDefault();
      const inputs = e.target.querySelectorAll('input');
      const data = {
        item: inputs[0].value.trim(),
        nick: inputs[1].value.trim(),
        contact: inputs[2].value.trim() || '-',
        time: new Date().toLocaleString()
      };
      addEntry('buy', data);
      e.target.reset();
    });

    document.getElementById('form-sell').addEventListener('submit', e => {
      e.preventDefault();
      const inputs = e.target.querySelectorAll('input');
      const data = {
        item: inputs[0].value.trim(),
        price: inputs[1].value.trim() || '-',
        nick: inputs[2].value.trim(),
        contact: inputs[3].value.trim
