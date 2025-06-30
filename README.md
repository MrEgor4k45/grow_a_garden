<!-- buy.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Купить | Grow a Garden</title>
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
    section {
      background-color: rgba(0, 0, 0, 0.8);
      padding: 30px;
      margin: 50px auto;
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
  </style>
</head>
<body>
  <div class="lang-switch">
    <select onchange="switchLang(this.value)">
      <option value="ru">Русский</option>
      <option value="uk">Українська</option>
      <option value="en">English</option>
    </select>
  </div>

  <section>
    <h2 id="title">📥 Купить</h2>
    <form onsubmit="sendForm(event)">
      <input type="text" placeholder="Что хотите купить?" required>
      <input type="text" placeholder="Ваш Roblox ник" required>
      <input type="text" placeholder="Контакт (Discord и т.п.)">
      <button type="submit">Отправить</button>
    </form>
    <div class="entry" id="entries"></div>
  </section>

  <script>
    const webhook = "https://discord.com/api/webhooks/1389234189504745675/kUOWAgPGTDDVmsuRdFMpp28aX8t8-ow7HNcumMAsYnMuJYOQFyEEtBRGag0iIZDXndDB";

    function sendForm(e) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll('input');
      let message = "";

      inputs.forEach(input => {
        message += `**${input.placeholder}**: ${input.value}\n`;
      });

      // Показать на сайте
      document.getElementById("entries").innerHTML = message.replaceAll("\n", "<br>");

      // Отправить в Discord
      fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message })
      });

      inputs.forEach(input => input.value = "");
    }

    function switchLang(lang) {
      const titles = {
        ru: "\uD83D\uDCE5 Купить",
        uk: "\uD83D\uDCE5 Купити",
        en: "\uD83D\uDCE5 Buy"
      };
      document.getElementById("title").innerText = titles[lang];
    }
  </script>
</body>
</html>
