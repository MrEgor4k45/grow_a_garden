<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Магазин Grow a Garden</title>
  <style>
    body {
      font-family: Arial;
      background: #101010;
      color: white;
      text-align: center;
      padding-bottom: 100px;
    }
    .form-box {
      background: #1e1e1e;
      margin: 30px auto;
      padding: 20px;
      border-radius: 10px;
      max-width: 500px;
    }
    input, button {
      width: 90%;
      margin: 10px 0;
      padding: 10px;
      border-radius: 5px;
      border: none;
    }
    button {
      background: #4CAF50;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background: #3a9640;
    }
    .entry {
      background: #2a2a2a;
      margin: 10px auto;
      padding: 10px;
      max-width: 500px;
      border-radius: 5px;
      text-align: left;
    }
  </style>
</head>
<body>

  <h1>🌱 Магазин Grow a Garden 🌻</h1>

  <div class="form-box">
    <h2>Купить</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input placeholder="Что хотите купить?" required>
      <input placeholder="Ваш Roblox ник" required>
      <input placeholder="Контакт (Discord и т.п.)">
      <button type="submit">Отправить</button>
    </form>
    <div id="buy-entries"></div>
  </div>

  <div class="form-box">
    <h2>Продать</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input placeholder="Что хотите продать?" required>
      <input placeholder="Цена (по желанию)">
      <input placeholder="Ваш Roblox ник" required>
      <input placeholder="Контакт (Discord и т.п.)">
      <button type="submit">Отправить</button>
    </form>
    <div id="sell-entries"></div>
  </div>

  <div class="form-box">
    <h2>Обмен</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input placeholder="Что вы даёте?" required>
      <input placeholder="Что хотите взамен?" required>
      <input placeholder="Ваш Roblox ник" required>
      <input placeholder="Контакт (Discord и т.п.)">
      <button type="submit">Отправить</button>
    </form>
    <div id="trade-entries"></div>
  </div>

  <script>
    const webhookUrl = "ТУТ_ТВОЙ_WEBHOOK";

    function sendForm(event, type) {
      event.preventDefault();
      const form = event.target;
      const inputs = form.querySelectorAll("input");
      const data = {};
      let text = "";

      inputs.forEach(input => {
        data[input.placeholder] = input.value.trim();
        text += `**${input.placeholder}:** ${input.value.trim()}\n`;
        input.value = "";
      });

      // Показ на сайте
      const preview = Object.entries(data)
        .map(([key, val]) => `<strong>${key}</strong>: ${val}`)
        .join("<br>");
      const div = document.createElement("div");
      div.className = "entry";
      div.innerHTML = preview;
      document.getElementById(`${type}-entries`).prepend(div);

      // Отправка в Discord
      fetch(webhookUrl, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: `📝 Новая заявка (${type}):\n${text}` })
      }).then(() => {
        alert("✅ Отправлено в Discord!");
      }).catch(() => {
        alert("❌ Ошибка отправки в Discord.");
      });
    }
  </script>

</body>
</html>
<head>
  <meta charset="UTF-8">
  <title>Магазин Grow a Garden</title>
  <link rel="icon" type="image/png" href="https://static.wikia.nocookie.net/logopedia/images/8/8a/Roblox_icon_2022.png">
  <!-- другие стили и скрипты -->
</head>
