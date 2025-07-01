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
      position: relative;
    }
    .delete-btn {
      position: absolute;
      top: 5px;
      right: 10px;
      background: #c33;
      border: none;
      padding: 3px 6px;
      border-radius: 5px;
      cursor: pointer;
      color: white;
      font-weight: bold;
      font-size: 12px;
      display: none;
      min-width: auto;
      width: auto;
    }
    .entry.admin .delete-btn {
      display: block;
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
    #admin-token-box {
      position: fixed;
      top: 10px;
      left: 10px;
      background: rgba(0,0,0,0.6);
      padding: 6px 10px;
      border-radius: 8px;
      z-index: 1000;
      color: white;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    #admin-token-input {
      width: 150px;
      padding: 5px;
      border-radius: 5px;
      border: 1px solid white;
      background: rgba(255,255,255,0.1);
      color: white;
      font-weight: normal;
    }
  </style>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
</head>
<body>

  <div id="admin-token-box">
    <label for="admin-token-input">Админ токен:</label>
    <input type="password" id="admin-token-input" placeholder="Введите токен" />
  </div>

  <div class="lang-switch">
    <select id="lang-select">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en">en English</option>
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
    <div id="entries-buy"></div>
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
    <div id="entries-sell"></div>
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
    <div id="entries-trade"></div>
  </section>

  <script>
    const translations = {
      ru: {
        welcomeTitle: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
        welcomeDesc: "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
        buyTitle: "📥 Купить",
        sellTitle: "📤 Продать",
        tradeTitle: "🔁 Обмен",
        placeholders: {
          buy: ["Что вы хотите купить?", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
          sell: ["Что вы продаёте?", "Цена (необязательно)", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
          trade: ["Что вы отдаёте?", "Что хотите взамен?", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
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
          buy: ["Що ви хочете купити?", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
          sell: ["Що ви продаєте?", "Ціна (необов'язково)", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
          trade: ["Що ви віддаєте?", "Що хочете натомість?", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
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
          buy: ["What do you want to buy?", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
          sell: ["What do you want to sell?", "Price (optional)", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
          trade: ["What are you giving?", "What do you want in return?", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
        },
        sendBtn: "Send"
      }
    };

    let currentLang = "ru";
    const ADMIN_TOKEN = "Admin-gag-shop";
    let currentAdminToken = "";

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

    // Firebase config и инициализация
    const firebaseConfig = {
      apiKey: "AIzaSyCohztyLEbSq2HH4IiMfjnb_UMB2-zwoyw",
      authDomain: "gag-4a6bd.firebaseapp.com",
      databaseURL: "https://gag-4a6bd-default-rtdb.europe-west1.firebasedatabase.app",
      projectId: "gag-4a6bd",
      storageBucket: "gag-4a6bd.appspot.com",
      messagingSenderId: "355235183308",
      appId: "1:355235183308:web:a9b50b7e31e2a276502069"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    const discordWebhook = "https://discord.com/api/webhooks/1389489483812175892/xVBCE4BDw5JzAtuOx3NmJ-gj7FpaYdFykNlcifVugL-Sax88lAN_mFcD6qI-DPCx81jG";

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
          Object.entries(val).forEach(([key, entry]) => {
            let text = '';
            for (const k in entry) {
              text += `${k}: ${entry[k]}\n`;
            }

            const div = document.createElement('div');
            div.classList.add('entry');
            div.textContent = text;

            // Добавляем кнопку удаления, если токен верный
            if(currentAdminToken === ADMIN_TOKEN) {
              div.classList.add('admin');
              const delBtn = document.createElement('button');
              delBtn.textContent = 'Удалить';
              delBtn.className = 'delete-btn';
              delBtn.onclick = () => {
                if(confirm('Удалить эту заявку?')) {
                  db.ref(type + '/' + key).remove();
                }
              };
              div.appendChild(delBtn);
            }

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

    document.getElementById("admin-token-input").addEventListener("input", e => {
      currentAdminToken = e.target.value.trim();
      listenEntries('buy', 'entries-buy');
      listenEntries('sell', 'entries-sell');
      listenEntries('trade', 'entries-trade');
    });

    listenEntries('buy', 'entries-buy');
    listenEntries('sell', 'entries-sell');
    listenEntries('trade', 'entries-trade');

    updateTexts();
  </script>
  <footer style="background: rgba(0,0,0,0.6); padding: 15px; text-align: center; margin-top: 30px;">
  <p style="font-size: 14px; color: white;">
    ❓ Появились проблемы? Пишите: 
    <br>Discord: <b>na_testosterone5x30</b> 
    <br>Telegram: <b>@grow_a_garden_shop</b>
  </p>
</footer>

</body>
</html>
