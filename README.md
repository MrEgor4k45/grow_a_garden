<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
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
      padding: 10px 10px 200px; /* Отступ снизу, чтобы формы не были зафиксированы под заявками */
      box-sizing: border-box;
    }
    .overlay {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      margin: 20px auto 10px;
      border-radius: 12px;
      max-width: 700px;
      width: 100%;
      box-sizing: border-box;
      position: relative;
      z-index: 1;
    }
    section {
      background-color: rgba(0, 0, 0, 0.75);
      padding: 20px;
      margin: 15px auto;
      max-width: 600px;
      width: 100%;
      border-radius: 15px;
      box-sizing: border-box;
      position: relative;
      z-index: 1;
    }
    input, button {
      width: 100%;
      padding: 14px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
      font-size: 18px;
      box-sizing: border-box;
      background: rgba(255,255,255,0.1);
      color: white;
    }
    input::placeholder {
      color: #ddd;
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

    /* Блоки заявок фиксируем внизу, растягиваем на всю ширину */
    .entries-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0,0,0,0.85);
      border-top: 3px solid #4caf50;
      display: flex;
      justify-content: space-around;
      padding: 10px 0;
      box-sizing: border-box;
      z-index: 10;
      height: 180px;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
    }
    /* Каждый блок заявок (buy, sell, trade) */
    .entries-section {
      flex: 1 0 33%;
      max-width: 33%;
      margin: 0 5px;
      background-color: rgba(0, 0, 0, 0.6);
      border-radius: 12px;
      display: flex;
      flex-direction: column;
      color: #a5d6a7;
      font-size: 14px;
      box-sizing: border-box;
      overflow-y: auto;
      padding: 6px;
      /* Чтобы не занимал весь контент и был прокрутка */
      max-height: 100%;
    }
    .entries-section h3 {
      margin: 5px 0 10px;
      font-weight: bold;
      color: #81c784;
      font-size: 16px;
      text-align: center;
      border-bottom: 1px solid #4caf50;
      padding-bottom: 4px;
    }
    /* Карточка заявки квадратная */
    .entry-card {
      background-color: rgba(76, 175, 80, 0.2);
      border: 1px solid #4caf50;
      border-radius: 12px;
      margin-bottom: 10px;
      padding: 10px;
      box-shadow: 0 0 8px rgba(76, 175, 80, 0.5);
      white-space: pre-line;
      flex-shrink: 0;
      aspect-ratio: 1 / 1; /* квадрат */
      display: flex;
      flex-direction: column;
      justify-content: center;
      font-size: 13px;
      overflow-wrap: break-word;
    }
    /* Стиль для выбора языка */
    .lang-switch {
      position: fixed;
      top: 12px;
      right: 12px;
      background: rgba(0,0,0,0.7);
      border-radius: 12px;
      padding: 6px 12px;
      z-index: 1000;
      user-select: none;
      font-size: 18px;
      color: white;
    }
    select {
      background: transparent;
      border: none;
      color: white;
      font-weight: bold;
      font-size: 18px;
      cursor: pointer;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      padding-right: 24px;
      text-align-last: center;
    }
    /* Стрелка для селекта */
    .lang-switch select {
      background-image:
        linear-gradient(45deg, transparent 50%, white 50%),
        linear-gradient(135deg, white 50%, transparent 50%),
        linear-gradient(to right, #555, #555);
      background-position:
        calc(100% - 20px) calc(1em + 2px),
        calc(100% - 15px) calc(1em + 2px),
        calc(100% - 25px) 0.5em;
      background-size: 5px 5px, 5px 5px, 1px 1.5em;
      background-repeat: no-repeat;
    }
    /* Адаптив */
    @media (max-width: 480px) {
      input, button {
        font-size: 16px;
        padding: 12px;
      }
      .lang-switch {
        font-size: 16px;
        padding: 5px 10px;
      }
      .lang-switch select {
        font-size: 16px;
        padding-right: 20px;
      }
      .entries-container {
        height: 140px;
      }
      .entries-section {
        font-size: 12px;
      }
      .entry-card {
        font-size: 11px;
        padding: 8px;
      }
    }
  </style>
</head>
<body>

  <div class="lang-switch" title="Выберите язык">
    <select id="lang-select" aria-label="Выбор языка">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en">🇬🇧 English</option>
    </select>
  </div>

  <div class="overlay">
    <h1 id="welcome-title">🌱 Добро пожаловать на сайт Grow a Garden! 🌻</h1>
    <p id="welcome-desc">Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.</p>
  </div>

  <section>
    <h2 id="title-buy">📥 Купить</h2>
    <form id="form-buy">
      <input type="text" placeholder="Что вы хотите купить?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit" id="btn-buy">Отправить</button>
    </form>
  </section>

  <section>
    <h2 id="title-sell">📤 Продать</h2>
    <form id="form-sell">
      <input type="text" placeholder="Что вы продаёте?" required />
      <input type="text" placeholder="Цена (необязательно)" />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit" id="btn-sell">Отправить</button>
    </form>
  </section>

  <section>
    <h2 id="title-trade">🔁 Обмен</h2>
    <form id="form-trade">
      <input type="text" placeholder="Что вы отдаёте?" required />
      <input type="text" placeholder="Что хотите взамен?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit" id="btn-trade">Отправить</button>
    </form>
  </section>

  <!-- Контейнер для заявок -->
  <div class="entries-container" aria-label="Список заявок">
    <div class="entries-section" id="entries-buy-section" aria-live="polite" aria-label="Заявки на покупку">
      <h3>📥 Купить</h3>
      <div id="entries-buy"></div>
    </div>
    <div class="entries-section" id="entries-sell-section" aria-live="polite" aria-label="Заявки на продажу">
      <h3>📤 Продать</h3>
      <div id="entries-sell"></div>
    </div>
    <div class="entries-section" id="entries-trade-section" aria-live="polite" aria-label="Заявки на обмен">
      <h3>🔁 Обмен</h3>
      <div id="entries-trade"></div>
    </div>
  </div>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    const translations = {
      ru: {
        welcomeTitle: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
        welcomeDesc: "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
        buyTitle: "📥 Купить",
        sellTitle: "📤 Продать",
        tradeTitle: "🔁 Обмен",
        placeholders: {
          buy: ["Что вы хотите купить?", "Ваш ник в Roblox", "Контакт (Discord и т.п.)"],
          sell: ["Что вы продаёте?", "Цена (необязательно)", "Ваш ник в Roblox", "Контакт (Discord и т.п.)"],
          trade: ["Что вы отдаёте?", "Что хотите взамен?", "Ваш ник в Roblox", "Контакт (Discord и т.п.)"],
        },
        sendBtn: "Отправить",
        noRequests: "Заявок пока нет."
      },
      uk: {
        welcomeTitle: "🌱 Ласкаво просимо на сайт Grow a Garden! 🌻",
        welcomeDesc: "Тут ви можете подати заявки на купівлю, продаж і обмін предметів із гри Grow a Garden.",
        buyTitle: "📥 Купити",
        sellTitle: "📤 Продати",
        tradeTitle: "🔁 Обмін",
        placeholders: {
          buy: ["Що ви хочете купити?", "Ваш нік в Roblox", "Контакт (Discord тощо)"],
          sell: ["Що ви продаєте?", "Ціна (необов'язково)", "Ваш нік в Roblox", "Контакт (Discord тощо)"],
          trade: ["Що ви віддаєте?", "Що хочете натомість?", "Ваш нік в Roblox", "Контакт (Discord тощо)"],
        },
        sendBtn: "Відправити",
        noRequests: "Заявок поки немає."
      },
      en: {
        welcomeTitle: "🌱 Welcome to the Grow a Garden website! 🌻",
        welcomeDesc: "Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.",
        buyTitle: "📥 Buy",
        sellTitle: "📤 Sell",
        tradeTitle: "🔁 Trade",
        placeholders: {
          buy: ["What do you want to buy?", "Your Roblox nickname", "Contact (Discord etc.)"],
          sell: ["What do you want to sell?", "Price (optional)", "Your Roblox nickname", "Contact (Discord etc.)"],
          trade: ["What are you giving?", "What do you want in return?", "Your Roblox nickname", "Contact (Discord etc.)"],
        },
        sendBtn: "Send",
        noRequests: "No requests yet."
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

      // Обновим тексты "нет заявок"
      updateNoRequestsText();
    }

    function updateNoRequestsText() {
      const containers = ['entries-buy', 'entries-sell', 'entries-trade'];
      containers.forEach(id => {
        const container = document.getElementById(id);
        if(container.innerHTML.trim() === '') {
          container.textContent = translations[currentLang].noRequests;
        }
      });
    }

    document.getElementById("lang-select").addEventListener("change", e => {
      currentLang = e.target.value;
      updateTexts();
    });

    // Firebase
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
    const db = firebase.database();

    const discordWebhook = "https://discord.com/api/webhooks/1389234189504745675/kUOWAgPGTDDVmsuRdFMpp28aX8t8-ow7HNcumMAsYnMuJYOQFyEEtBRGag0iIZDXndDB";

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
            div.classList.add('entry-card');
            div.textContent = text;
            container.appendChild(div);
          });
        } else {
          container.textContent = translations[currentLang].noRequests;
        }
      });
    }

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
        nick: inputs[2].
