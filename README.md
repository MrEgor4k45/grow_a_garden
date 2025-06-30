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
    input, button {
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
    .entry-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 15px;
    }
    .entry {
      background-color: rgba(255, 255, 255, 0.15);
      padding: 12px;
      border-radius: 10px;
      text-align: left;
      white-space: pre-line;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }
  </style>
</head>
<body>

  <div class="overlay">
    <h1>🌱 Добро пожаловать на сайт Grow a Garden! 🌻</h1>
    <p>Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.</p>
  </div>

  <section>
    <h2>📥 Купить</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" placeholder="Что вы хотите купить?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit">Отправить</button>
    </form>
    <div class="entry-list" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Продать</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" placeholder="Что вы продаёте?" required />
      <input type="text" placeholder="Цена (необязательно)" />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit">Отправить</button>
    </form>
    <div class="entry-list" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Обмен</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" placeholder="Что вы отдаёте?" required />
      <input type="text" placeholder="Что хотите взамен?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (Discord и т.п.)" />
      <button type="submit">Отправить</button>
    </form>
    <div class="entry-list" id="entries-trade"></div>
  </section>

  <script>
    const webhook = "https://discord.com/api/webhooks/1389234189504745675/kUOWAgPGTDDVmsuRdFMpp28aX8t8-ow7HNcumMAsYnMuJYOQFyEEtBRGag0iIZDXndDB";

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let message = `📝 Заявка: ${type.toUpperCase()}\n`;

      inputs.forEach(input => {
        message += `**${input.placeholder}**: ${input.value}\n`;
      });

      // Отображение карточки на сайте
      const entryDiv = document.createElement('div');
      entryDiv.className = 'entry';
      entryDiv.innerText = message;
      document.getElementById(`entries-${type}`).appendChild(entryDiv);

      // Отправка в Discord
      fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message }),
      });

      inputs.forEach(input => input.value = "");
    }
  </script>

</body>
</html>
