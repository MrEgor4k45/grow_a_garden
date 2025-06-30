<!-- buy.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Заявки | Grow a Garden</title>
  <link rel="icon" href="https://upload.wikimedia.org/wikipedia/commons/6/6b/Roblox_Logo_2022.svg">
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
    section {
      background-color: rgba(0, 0, 0, 0.8);
      padding: 30px;
      margin: 30px auto;
      max-width: 600px;
      border-radius: 15px;
    }
    input, button {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
    }
    button {
      background-color: #4CAF50;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #3e8e41;
    }
    .entry {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 10px;
      border-radius: 10px;
      margin-top: 15px;
      text-align: left;
    }
    .lang-switch {
      position: fixed;
      top: 10px;
      right: 10px;
    }
    select {
      background: rgba(255,255,255,0.1);
      color: white;
      border: 1px solid white;
      border-radius: 6px;
      padding: 5px;
    }
    select option {
      background: black;
      color: white;
    }
  </style>
</head>
<body>
  <header>🌱 Добро пожаловать на сайт Grow a Garden! 🌻<br><small>Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.</small></header>

  <div class="lang-switch">
    <label for="lang-select">🌐</label>
    <select id="lang-select" onchange="switchLang(this.value)">
      <option value="ru">🇷🇺 Русский</option>
      <option value="uk">🇺🇦 Українська</option>
      <option value="en">🇬🇧 English</option>
    </select>
  </div>

  <section>
    <h2 id="title">📥 Купить</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-placeholder="item" placeholder="Что хотите купить?" required>
      <input type="text" data-placeholder="nick" placeholder="Ваш Roblox ник" required>
      <input type="text" data-placeholder="contact" placeholder="Контакт (Discord и т.п.)">
      <button type="submit" id="submit-buy">Отправить</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2 id="title-sell">📤 Продать</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-placeholder="item" placeholder="Что хотите продать?" required>
      <input type="text" data-placeholder="price" placeholder="Цена (по желанию)">
      <input type="text" data-placeholder="nick" placeholder="Ваш Roblox ник" required>
      <input type="text" data-placeholder="contact" placeholder="Контакт (Discord и т.п.)">
      <button type="submit" id="submit-sell">Отправить</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2 id="title-trade">🔁 Обмен</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-placeholder="give" placeholder="Что вы даёте?" required>
      <input type="text" data-placeholder="want" placeholder="Что хотите взамен?" required>
      <input type="text" data-placeholder="nick" placeholder="Ваш Roblox ник" required>
      <input type="text" data-placeholder="contact" placeholder="Контакт (Discord и т.п.)">
      <button type="submit" id="submit-trade">Отправить</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <script>
    const webhook = "https://discord.com/api/webhooks/1389234189504745675/kUOWAgPGTDDVmsuRdFMpp28aX8t8-ow7HNcumMAsYnMuJYOQFyEEtBRGag0iIZDXndDB";

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll('input');
      let message = `Заявка: ${type.toUpperCase()}\n`;

      inputs.forEach(input => {
        message += `**${input.placeholder}**: ${input.value}\n`;
      });

      document.getElementById(`entries-${type}`).innerHTML = message.replaceAll("\n", "<br>");

      fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message })
      });

      inputs.forEach(input => input.value = "");
    }

    function switchLang(lang) {
      const translations = {
        ru: {
          buy: "📥 Купить",
          sell: "📤 Продать",
          trade: "🔁 Обмен",
          submit: "Отправить",
          item: "Что хотите купить?",
          nick: "Ваш Roblox ник",
          contact: "Контакт (Discord и т.п.)",
          price: "Цена (по желанию)",
          give: "Что вы даёте?",
          want: "Что хотите взамен?"
        },
        uk: {
          buy: "📥 Купити",
          sell: "📤 Продати",
          trade: "🔁 Обмін",
          submit: "Надіслати",
          item: "Що бажаєте купити?",
          nick: "Ваш Roblox нік",
          contact: "Контакт (Discord тощо)",
          price: "Ціна (за бажанням)",
          give: "Що ви віддаєте?",
          want: "Що хочете натомість?"
        },
        en: {
          buy: "📥 Buy",
          sell: "📤 Sell",
          trade: "🔁 Trade",
          submit: "Submit",
          item: "What do you want to buy?",
          nick: "Your Roblox nickname",
          contact: "Contact (Discord etc.)",
          price: "Price (optional)",
          give: "What are you giving?",
          want: "What do you want in return?"
        }
      };

      document.getElementById("title").innerText = translations[lang].buy;
      document.getElementById("title-sell").innerText = translations[lang].sell;
      document.getElementById("title-trade").innerText = translations[lang].trade;
      document.getElementById("submit-buy").innerText = translations[lang].submit;
      document.getElementById("submit-sell").innerText = translations[lang].submit;
      document.getElementById("submit-trade").innerText = translations[lang].submit;

      document.querySelectorAll('input').forEach(input => {
        const key = input.dataset.placeholder;
        if (translations[lang][key]) {
          input.placeholder = translations[lang][key];
        }
      });
    }
  </script>
</body>
</html>
