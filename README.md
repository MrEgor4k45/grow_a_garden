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
    }
    .overlay {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      margin: 30px auto 10px;
      border-radius: 12px;
      max-width: 700px;
    }
    section {
      background-color: rgba(0, 0, 0, 0.75);
      padding: 20px;
      margin: 20px auto;
      max-width: 600px;
      border-radius: 15px;
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
    <div class="entry" id="entries-buy"></div>
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
    <div class="entry" id="entries-sell"></div>
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
    <div class="entry" id="entries-trade"></div>
  </section>

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
        sendBtn: "Отправить"
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
        sendBtn: "Відправити"
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
        sendBtn: "Send"
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
    }

    document.getElementById("lang-select").addEventListener("change", e => {
      currentLang = e.target.value;
      updateTexts();
    });

    // Инициализация Firebase
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
        contact: inputs[3].value.trim() || '-',
        time: new Date().toLocaleString()
      };
      addEntry('sell', data);
      e.target.reset();
    });

    document.getElementById('form-trade').addEventListener('submit', e => {
      e.preventDefault();
      const inputs = e.target.querySelectorAll('input');
      const data = {
        give: inputs[0].value.trim(),
        want: inputs[1].value.trim(),
        nick: inputs[2].value.trim(),
        contact: inputs[3].value.trim() || '-',
        time: new Date().toLocaleString()
      };
      addEntry('trade', data);
      e.target.reset();
    });

    listenEntries('buy', 'entries-buy');
    listenEntries('sell', 'entries-sell');
    listenEntries('trade', 'entries-trade');

    // Устанавливаем изначальный язык и тексты
    updateTexts(ru);
  </script>
</body>
</html>
