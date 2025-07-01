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
    <div id="entri
