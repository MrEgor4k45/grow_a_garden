<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Магазин Grow a Garden</title>
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-app.js";
    import { getDatabase, ref, push, onChildAdded } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyCohztyLEbSq2HH4IiMfjnb_UMB2-zwoyw",
      authDomain: "gag-4a6bd.firebaseapp.com",
      databaseURL: "https://gag-4a6bd-default-rtdb.europe-west1.firebasedatabase.app/",
      projectId: "gag-4a6bd",
      storageBucket: "gag-4a6bd.firebasestorage.app",
      messagingSenderId: "355235183308",
      appId: "1:355235183308:web:a9b50b7e31e2a276502069"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    window.addEntry = (event, type) => {
      event.preventDefault();
      const form = event.target;
      const inputs = form.querySelectorAll('input');
      const data = {};
      inputs.forEach(input => {
        data[input.placeholder] = input.value.trim();
        input.value = "";
      });

      push(ref(db, type), data);
    };

    // Показывать заявки в реальном времени
    ["buy", "sell", "trade"].forEach(type => {
      const container = document.getElementById(`${type}-entries`);
      onChildAdded(ref(db, type), snapshot => {
        const data = snapshot.val();
        const html = Object.entries(data)
          .map(([k, v]) => `<strong>${k}</strong>: ${v}`)
          .join("<br>");
        const div = document.createElement("div");
        div.className = "entry";
        div.innerHTML = html;
        container.prepend(div);
      });
    });
  </script>
  <style>
    html { scroll-behavior: smooth; }
    body {
      margin: 0; padding: 0;
      background: url('https://insider-gaming.com/wp-content/uploads/2025/05/grow-a-garden-update.png') no-repeat center center fixed;
      background-size: cover;
      font-family: Arial, sans-serif;
      color: white; text-align: center;
    }
    header { background: rgba(0,0,0,0.7); padding: 20px; font-size: 24px; }
    .container { display: flex; justify-content: center; flex-wrap: wrap; padding: 30px; }
    .card {
      background: rgba(0,0,0,0.6); margin: 20px; padding: 20px;
      width: 250px; border-radius: 15px; box-shadow: 0 0 10px #222;
    }
    .card img { width: 100%; border-radius: 10px; }
    .card button {
      margin-top: 10px; padding: 10px; width: 100%;
      background-color: #4CAF50; color: white;
      border: none; border-radius: 8px; cursor: pointer;
    }
    .card button:hover { background-color: #3e8e41; }
    section {
      background: rgba(0, 0, 0, 0.85); margin: 40px auto; padding: 30px;
      width: 90%; max-width: 600px; border-radius: 15px;
    }
    input {
      width: 90%; padding: 10px; margin: 10px 0;
      border-radius: 8px; border: none;
    }
    .form-btn {
      background: #2196F3; color: white; padding: 10px 20px;
      border-radius: 8px; border: none; cursor: pointer;
    }
    .form-btn:hover { background-color: #0b7dda; }
    .entries { text-align: left; margin-top: 20px; }
    .entry {
      background: rgba(255,255,255,0.1); padding: 10px;
      margin: 10px 0; border-radius: 10px;
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
</body>
</html>
