<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests</title>
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
      transition: transform 0.2s ease;
    }
    button {
      background-color: #4caf50;
      color: white;
      cursor: pointer;
      font-weight: bold;
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
  </style>
</head>
<body>

  <!-- Language switcher -->
  <div class="lang-switch">
    <label for="lang-select">🌐</label>
    <select id="lang-select" onchange="switchLang(this.value)">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en" selected>🇬🇧 English</option>
    </select>
  </div>

  <!-- Main content -->
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

  <script>
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

    let currentLang = 'en';

    function switchLang(lang) {
      currentLang = lang;
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

    // Инициализируем язык при загрузке
    switchLang(currentLang);
  </script>
</body>
</html>
