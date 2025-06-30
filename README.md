
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Магазин Grow a Garden</title>
  <style>
    html {
      scroll-behavior: smooth;
    }

    body {
      margin: 0;
      padding: 0;
      background-image: url('https://insider-gaming.com/wp-content/uploads/2025/05/grow-a-garden-update.png');
      background-size: cover;
      font-family: Arial, sans-serif;
      color: white;
      text-align: center;
    }

    header {
      background-color: rgba(0, 0, 0, 0.7);
      padding: 20px;
      font-size: 24px;
    }

    .container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 30px;
    }

    .card {
      background: rgba(0, 0, 0, 0.6);
      margin: 20px;
      padding: 20px;
      width: 250px;
      border-radius: 15px;
      box-shadow: 0 0 10px #222;
    }

    .card img {
      width: 100%;
      border-radius: 10px;
    }

    .card button {
      margin-top: 10px;
      padding: 10px;
      width: 100%;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }

    .card button:hover {
      background-color: #3e8e41;
    }

    section {
      background-color: rgba(0, 0, 0, 0.85);
      margin: 40px auto;
      padding: 30px;
      width: 90%;
      max-width: 600px;
      border-radius: 15px;
    }

    input, textarea {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
    }

    .form-btn {
      background-color: #2196F3;
      color: white;
      padding: 10px 20px;
      border-radius: 8px;
      border: none;
      cursor: pointer;
    }

    .form-btn:hover {
      background-color: #0b7dda;
    }

    .entries {
      text-align: left;
      margin-top: 20px;
    }

    .entry {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 10px;
      margin: 10px 0;
      border-radius: 10px;
    }

    a {
      text-decoration: none;
    }
  </style>
</head>
<body>
  <header>🌱 Магазин Grow a Garden 🌻</header>

  <div class="container">
    <div class="card">
      <img src="https://files.bo3.gg/uploads/image/82204/image/webp-94d4d63b82d3ee25e1de3f08ff3f9e93.webp">
      <h3>Питомцы</h3>
      <a href="#buy"><button>Купить</button></a>
      <a href="#sell"><button>Продать</button></a>
      <a href="#trade"><button>Обмен</button></a>
    </div>
  </div>

  <!-- Купить -->
  <section id="buy">
    <h2>📥 Купить</h2>
    <form onsubmit="addEntry(event, 'buy')">
      <input type="text" placeholder="Что хотите купить?" required><br>
      <input type="text" placeholder="Ваш Roblox ник" required><br>
      <input type="text" placeholder="Контакт (Discord и т.п.)"><br>
      <button class="form-btn" type="submit">Отправить</button>
    </form>
    <div class="entries" id="buy-entries"></div>
  </section>

  <!-- Продать -->
  <section id="sell">
    <h2>📤 Продать</h2>
    <form onsubmit="addEntry(event, 'sell')">
      <input type="text" placeholder="Что хотите продать?" required><br>
      <input type="text" placeholder="Цена (по желанию)"><br>
      <input type="text" placeholder="Ваш Roblox ник" required><br>
      <input type="text" placeholder="Контакт (Discord и т.п.)"><br>
      <button class="form-btn" type="submit">Отправить</button>
    </form>
    <div class="entries" id="sell-entries"></div>
  </section>

  <!-- Обмен -->
  <section id="trade">
    <h2>🔁 Обмен</h2>
    <form onsubmit="addEntry(event, 'trade')">
      <input type="text" placeholder="Что вы даёте?" required><br>
      <input type="text" placeholder="Что хотите взамен?" required><br>
      <input type="text" placeholder="Ваш Roblox ник" required><br>
      <input type="text" placeholder="Контакт (Discord и т.п.)"><br>
      <button class="form-btn" type="submit">Отправить</button>
    </form>
    <div class="entries" id="trade-entries"></div>
  </section>

  <script>
    const endpoint = "https://script.google.com/macros/s/AKfycbxRPv_2wa6-FdHkOwihGblH-erTVc1-VfzGwUctml8yLimUUTiVrFQLFLPVZWspYRKB1A/exec";

    function addEntry(event, type) {
      event.preventDefault();
      const form = event.target;
      const inputs = form.querySelectorAll('input');
      let text = "";
      let previewHTML = "";

      inputs.forEach(input => {
        if (input.value.trim()) {
          text += `${input.placeholder}: ${input.value}\n`;
          previewHTML += `<strong>${input.placeholder}</strong>: ${input.value}<br>`;
        }
      });

      // Показать на сайте
      const container = document.getElementById(`${type}-entries`);
      const div = document.createElement("div");
      div.className = "entry";
      div.innerHTML = previewHTML;
      container.prepend(div);

      // Отправить на Google Таблицу
      fetch(endpoint, {
        method: "POST",
        body: JSON.stringify({ type: type, content: text }),
        headers: {
          "Content-Type": "application/json"
        }
      }).then(res => {
        alert("✅ Отправлено и сохранено!");
      }).catch(err => {
        alert("❌ Ошибка отправки");
      });

      inputs.forEach(input => input.value = ""); // очистить поля
    }
  </script>
</body>
</html>
