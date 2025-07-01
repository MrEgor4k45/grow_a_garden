<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Заявки</title>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url('https://insider-gaming.com/wp-content/uploads/2025/05/grow-a-garden-update.png') no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 10px;
    }
    .overlay {
      background: rgba(0, 0, 0, 0.7);
      padding: 20px;
      margin: 20px auto 10px;
      border-radius: 12px;
      max-width: 700px;
      width: 100%;
    }
    section {
      background-color: rgba(0, 0, 0, 0.75);
      padding: 20px;
      margin: 10px auto;
      max-width: 600px;
      border-radius: 15px;
      width: 100%;
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
    button:disabled {
      background-color: #777;
      cursor: not-allowed;
    }
    button:hover:not(:disabled) {
      background-color: #3e8e41;
      transform: scale(1.05);
    }
    button:active:not(:disabled) {
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
    .g-recaptcha {
      margin: 10px auto;
      display: flex;
      justify-content: center;
    }
    #admin-token-container {
      position: fixed;
      top: 10px;
      right: 10px;
      background: rgba(0,0,0,0.6);
      padding: 8px 12px;
      border-radius: 10px;
      color: white;
      z-index: 9999;
      font-family: Arial, sans-serif;
      user-select: none;
      display: flex;
      align-items: center;
    }
    #admin-token-input {
      padding: 5px 8px;
      border-radius: 6px;
      border: none;
      font-size: 14px;
      width: 140px;
    }
    #admin-token-btn {
      padding: 5px 10px;
      margin-left: 6px;
      border-radius: 6px;
      border: none;
      background-color: #4caf50;
      color: white;
      font-weight: bold;
      cursor: pointer;
      font-size: 14px;
    }
    #admin-token-msg {
      margin-left: 10px;
      font-size: 13px;
      color: #f44336;
      min-width: 130px;
    }
  </style>
</head>
<body>
  <div id="admin-token-container">
    <input id="admin-token-input" type="password" placeholder="Админ токен" />
    <button id="admin-token-btn">Войти</button>
    <div id="admin-token-msg"></div>
  </div>

  <div class="overlay">
    <h1>Grow a Garden Shop</h1>
    <p>Отправьте заявку на покупку, продажу или обмен</p>
  </div>

  <section>
    <h2>📥 Купить</h2>
    <form id="form-buy">
      <input type="text" placeholder="Что вы хотите купить?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (DS, TG и т.п.)" required />
      <div class="g-recaptcha" data-sitekey="6Lfgp3MrAAAAAGiQK_wglmeukAE6HUW3iJGM1TRZ"></div>
      <button type="submit" disabled>Отправить</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Продать</h2>
    <form id="form-sell">
      <input type="text" placeholder="Что вы продаёте?" required />
      <input type="text" placeholder="Цена (необязательно)" />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (DS, TG и т.п.)" required />
      <div class="g-recaptcha" data-sitekey="6Lfgp3MrAAAAAGiQK_wglmeukAE6HUW3iJGM1TRZ"></div>
      <button type="submit" disabled>Отправить</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Обмен</h2>
    <form id="form-trade">
      <input type="text" placeholder="Что вы отдаёте?" required />
      <input type="text" placeholder="Что хотите взамен?" required />
      <input type="text" placeholder="Ваш ник в Roblox" required />
      <input type="text" placeholder="Контакт (DS, TG и т.п.)" required />
      <div class="g-recaptcha" data-sitekey="6Lfgp3MrAAAAAGiQK_wglmeukAE6HUW3iJGM1TRZ"></div>
      <button type="submit" disabled>Отправить</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
  <script>
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

    function sendToDiscord(type, data) {
      let msg = `📝 Заявка: ${type}\n`;
      for (const key in data) msg += `**${key}**: ${data[key]}\n`;
      fetch(discordWebhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: msg })
      });
    }

    function setupForm(formId, dbPath, entryId) {
      const form = document.getElementById(formId);
      const submitBtn = form.querySelector('button[type="submit"]');

      form.addEventListener('submit', async (e) => {
        e.preventDefault();
        if (submitBtn.disabled) {
          alert("Вы не авторизованы как админ.");
          return;
        }

        const token = grecaptcha.getResponse();
        if (!token) {
          alert('Пожалуйста, пройдите капчу.');
          return;
        }

        const inputs = e.target.querySelectorAll('input');
        const data = {};
        inputs.forEach((input, i) => {
          if(input.type !== "submit" && input.type !== "button")
            data[`field${i + 1}`] = input.value.trim();
        });
        data.time = new Date().toLocaleString();

        await db.ref(dbPath).push(data);
        sendToDiscord(dbPath, data);

        const entryDiv = document.createElement('div');
        entryDiv.className = 'entry';
        entryDiv.innerHTML = Object.entries(data).map(([k,v]) => `${k}: ${v}`).join("<br>");
        document.getElementById(entryId).appendChild(entryDiv);

        e.target.reset();
        grecaptcha.reset();
      });
    }

    setupForm('form-buy', 'buy', 'entries-buy');
    setupForm('form-sell', 'sell', 'entries-sell');
    setupForm('form-trade', 'trade', 'entries-trade');

    // --- Админ токен ---
    const adminToken = "Admin-gag-shop";
    let isAdmin = false;

    const tokenInput = document.getElementById("admin-token-input");
    const tokenBtn = document.getElementById("admin-token-btn");
    const tokenMsg = document.getElementById("admin-token-msg");
    const submitButtons = document.querySelectorAll("form button[type='submit']");

    tokenBtn.addEventListener("click", () => {
      if (tokenInput.value === adminToken) {
        isAdmin = true;
        tokenMsg.style.color = "#4caf50";
        tokenMsg.textContent = "Добро пожаловать, админ!";
        tokenInput.value = "";
        submitButtons.forEach(btn => btn.disabled = false);
      } else {
        isAdmin = false;
        tokenMsg.style.color = "#f44336";
        tokenMsg.textContent = "Неверный токен!";
        submitButtons.forEach(btn => btn.disabled = true);
      }
    });
  </script>
</body>
</html>
