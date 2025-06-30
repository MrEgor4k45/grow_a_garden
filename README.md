<!DOCTYPE html>
<html lang="en">
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
      padding: 20px;
    }
    section {
      background: rgba(0,0,0,0.8);
      margin: 20px auto;
      padding: 20px;
      border-radius: 10px;
      max-width: 500px;
    }
    input, button {
      width: 90%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 6px;
      border: none;
      font-size: 16px;
    }
    button {
      background: #4caf50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: 0.2s;
    }
    button:hover {
      background: #3e8e41;
      transform: scale(1.05);
    }
    .entry {
      background: rgba(255,255,255,0.1);
      margin-top: 10px;
      padding: 10px;
      border-radius: 8px;
      text-align: left;
      white-space: pre-line;
    }
    .lang-switch {
      position: fixed;
      top: 10px;
      right: 10px;
    }
    select {
      background: #333;
      color: white;
      border: 1px solid white;
      padding: 5px;
      border-radius: 5px;
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

  <h1 id="title">🌱 Добро пожаловать на сайт Grow a Garden! 🌻</h1>
  <p id="subtitle">Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.</p>

  <section>
    <h2 id="buy-title">📥 Купить</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-id="item" required />
      <input type="text" data-id="nick" required />
      <input type="text" data-id="contact" />
      <button type="submit" id="buy-submit">Отправить</button>
    </form>
    <div id="entries-buy"></div>
  </section>

  <section>
    <h2 id="sell-title">📤 Продать</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-id="item" required />
      <input type="text" data-id="price" />
      <input type="text" data-id="nick" required />
      <input type="text" data-id="contact" />
      <button type="submit" id="sell-submit">Отправить</button>
    </form>
    <div id="entries-sell"></div>
  </section>

  <section>
    <h2 id="trade-title">🔁 Обмен</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-id="give" required />
      <input type="text" data-id="want" required />
      <input type="text" data-id="nick" required />
      <input type="text" data-id="contact" />
      <button type="submit" id="trade-submit">Отправить</button>
    </form>
    <div id="entries-trade"></div>
  </section>

  <script>
    const webhook = "https://discord.com/api/webhooks/ВАШ_ID/ВАШ_КЛЮЧ"; // вставьте свой Webhook

    let currentLang = "ru";

    const translations = {
      ru: {
        title: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
        subtitle: "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
        buy: "📥 Купить",
        sell: "📤 Продать",
        trade: "🔁 Обмен",
        submit: "Отправить",
        item: "Что хотите купить / продать?",
        nick: "Ваш ник в Roblox",
        contact: "Контакт (Discord и т.п.)",
        price: "Цена (если есть)",
        give: "Что вы отдаёте?",
        want: "Что хотите взамен?"
      },
      uk: {
        title: "🌱 Ласкаво просимо на сайт Grow a Garden! 🌻",
        subtitle: "Тут ви можете подати заявки на купівлю, продаж і обмін предметів із гри Grow a Garden.",
        buy: "📥 Купити",
        sell: "📤 Продати",
        trade: "🔁 Обмін",
        submit: "Надіслати",
        item: "Що бажаєте купити / продати?",
        nick: "Ваш нік у Roblox",
        contact: "Контакт (Discord тощо)",
        price: "Ціна (за бажанням)",
        give: "Що ви віддаєте?",
        want: "Що хочете натомість?"
      },
      en: {
        title: "🌱 Welcome to the Grow a Garden website! 🌻",
        subtitle: "Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.",
        buy: "📥 Buy",
        sell: "📤 Sell",
        trade: "🔁 Trade",
        submit: "Submit",
        item: "What do you want to buy / sell?",
        nick: "Your Roblox nickname",
        contact: "Contact (Discord etc.)",
        price: "Price (optional)",
        give: "What are you giving?",
        want: "What do you want in return?"
      }
    };

    function switchLang(lang) {
      currentLang = lang;
      const t = translations[lang];

      document.getElementById("title").innerText = t.title;
      document.getElementById("subtitle").innerText = t.subtitle;
      document.getElementById("buy-title").innerText = t.buy;
      document.getElementById("sell-title").innerText = t.sell;
      document.getElementById("trade-title").innerText = t.trade;
      document.getElementById("buy-submit").innerText =
        document.getElementById("sell-submit").innerText =
        document.getElementById("trade-submit").innerText = t.submit;

      document.querySelectorAll("form").forEach(form => {
        form.querySelectorAll("input").forEach(input => {
          const id = input.dataset.id;
          if (t[id]) input.placeholder = t[id];
        });
      });

      displayRequests();
    }

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      const data = { type, time: Date.now() };

      let message = `Request: ${type.toUpperCase()}\n`;

      inputs.forEach(input => {
        const key = input.dataset.id;
        const label = translations[currentLang][key] || key;
        const value = input.value;
        data[key] = value;
        message += `**${label}**: ${value}\n`;
      });

      // отправка в Discord
      fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message })
      });

      // сохранение в localStorage
      const all = JSON.parse(localStorage.getItem("requests") || "[]");
      all.push(data);
      localStorage.setItem("requests", JSON.stringify(all));

      inputs.forEach(input => input.value = "");
      displayRequests();
    }

    function displayRequests() {
      const all = JSON.parse(localStorage.getItem("requests") || "[]");
      ["buy", "sell", "trade"].forEach(type => {
        const container = document.getElementById(`entries-${type}`);
        container.innerHTML = "";
        all.filter(r => r.type === type).forEach(entry => {
          const div = document.createElement("div");
          div.className = "entry";
          div.innerText = Object.entries(entry)
            .filter(([k]) => k !== "type" && k !== "time")
            .map(([k, v]) => `${translations[currentLang][k] || k}: ${v}`)
            .join("\n");
          container.appendChild(div);
        });
      });
    }

    switchLang("ru");
  </script>
</body>
</html>
