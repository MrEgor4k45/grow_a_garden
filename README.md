<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests</title>
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
  </style>
</head>
<body>

  <header>
    <h1>🌱 Grow a Garden Requests 🌻</h1>
    <p>Submit your Buy, Sell or Trade requests</p>
  </header>

  <section>
    <h2>📥 Buy</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-placeholder="item" placeholder="What do you want to buy?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Sell</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-placeholder="item" placeholder="What do you want to sell?" required />
      <input type="text" data-placeholder="price" placeholder="Price (optional)" />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Trade</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-placeholder="give" placeholder="What are you giving?" required />
      <input type="text" data-placeholder="want" placeholder="What do you want in return?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    // ⚠️ ЗАМЕНИ НА СВОЙ config Firebase
    const firebaseConfig = {
      apiKey: "ВАШ_API_KEY",
      authDomain: "ВАШ_ПРОЕКТ.firebaseapp.com",
      databaseURL: "https://ВАШ_ПРОЕКТ.firebaseio.com",
      projectId: "ВАШ_ПРОЕКТ",
      storageBucket: "ВАШ_ПРОЕКТ.appspot.com",
      messagingSenderId: "ВАШ_ID",
      appId: "ВАШ_APP_ID",
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let data = {};

      inputs.forEach(input => {
        data[input.dataset.placeholder] = input.value.trim();
      });

      data.type = type;
      data.timestamp = Date.now();

      db.ref('requests').push(data).then(() => {
        alert("Request sent!");
        inputs.forEach(input => input.value = "");
      }).catch(err => {
        alert("Error: " + err.message);
      });
    }

    function displayRequests() {
      db.ref('requests').on('value', (snapshot) => {
        const all = snapshot.val();
        document.getElementById('entries-buy').innerHTML = '';
        document.getElementById('entries-sell').innerHTML = '';
        document.getElementById('entries-trade').innerHTML = '';

        for (let key in all) {
          const entry = all[key];
          const box = document.createElement('div');
          box.className = 'entry';
          box.innerText = Object.entries(entry).filter(([k]) => k !== 'type' && k !== 'timestamp').map(([k, v]) => `${k}: ${v}`).join('\n');
          const containerId = `entries-${entry.type}`;
          const container = document.getElementById(containerId);
          if (container) container.appendChild(box);
        }
      });
    }

    // Загружаем все заявки при старте
    displayRequests();
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests</title>
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
  </style>
</head>
<body>

  <header>
    <h1>🌱 Grow a Garden Requests 🌻</h1>
    <p>Submit your Buy, Sell or Trade requests</p>
  </header>

  <section>
    <h2>📥 Buy</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-placeholder="item" placeholder="What do you want to buy?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Sell</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-placeholder="item" placeholder="What do you want to sell?" required />
      <input type="text" data-placeholder="price" placeholder="Price (optional)" />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Trade</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-placeholder="give" placeholder="What are you giving?" required />
      <input type="text" data-placeholder="want" placeholder="What do you want in return?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    // ⚠️ ЗАМЕНИ НА СВОЙ config Firebase
    const firebaseConfig = {
      apiKey: "ВАШ_API_KEY",
      authDomain: "ВАШ_ПРОЕКТ.firebaseapp.com",
      databaseURL: "https://ВАШ_ПРОЕКТ.firebaseio.com",
      projectId: "ВАШ_ПРОЕКТ",
      storageBucket: "ВАШ_ПРОЕКТ.appspot.com",
      messagingSenderId: "ВАШ_ID",
      appId: "ВАШ_APP_ID",
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let data = {};

      inputs.forEach(input => {
        data[input.dataset.placeholder] = input.value.trim();
      });

      data.type = type;
      data.timestamp = Date.now();

      db.ref('requests').push(data).then(() => {
        alert("Request sent!");
        inputs.forEach(input => input.value = "");
      }).catch(err => {
        alert("Error: " + err.message);
      });
    }

    function displayRequests() {
      db.ref('requests').on('value', (snapshot) => {
        const all = snapshot.val();
        document.getElementById('entries-buy').innerHTML = '';
        document.getElementById('entries-sell').innerHTML = '';
        document.getElementById('entries-trade').innerHTML = '';

        for (let key in all) {
          const entry = all[key];
          const box = document.createElement('div');
          box.className = 'entry';
          box.innerText = Object.entries(entry).filter(([k]) => k !== 'type' && k !== 'timestamp').map(([k, v]) => `${k}: ${v}`).join('\n');
          const containerId = `entries-${entry.type}`;
          const container = document.getElementById(containerId);
          if (container) container.appendChild(box);
        }
      });
    }

    // Загружаем все заявки при старте
    displayRequests();
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests</title>
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
  </style>
</head>
<body>

  <header>
    <h1>🌱 Grow a Garden Requests 🌻</h1>
    <p>Submit your Buy, Sell or Trade requests</p>
  </header>

  <section>
    <h2>📥 Buy</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-placeholder="item" placeholder="What do you want to buy?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Sell</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-placeholder="item" placeholder="What do you want to sell?" required />
      <input type="text" data-placeholder="price" placeholder="Price (optional)" />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Trade</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-placeholder="give" placeholder="What are you giving?" required />
      <input type="text" data-placeholder="want" placeholder="What do you want in return?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    // ⚠️ ЗАМЕНИ НА СВОЙ config Firebase
    const firebaseConfig = {
      apiKey: "ВАШ_API_KEY",
      authDomain: "ВАШ_ПРОЕКТ.firebaseapp.com",
      databaseURL: "https://ВАШ_ПРОЕКТ.firebaseio.com",
      projectId: "ВАШ_ПРОЕКТ",
      storageBucket: "ВАШ_ПРОЕКТ.appspot.com",
      messagingSenderId: "ВАШ_ID",
      appId: "ВАШ_APP_ID",
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let data = {};

      inputs.forEach(input => {
        data[input.dataset.placeholder] = input.value.trim();
      });

      data.type = type;
      data.timestamp = Date.now();

      db.ref('requests').push(data).then(() => {
        alert("Request sent!");
        inputs.forEach(input => input.value = "");
      }).catch(err => {
        alert("Error: " + err.message);
      });
    }

    function displayRequests() {
      db.ref('requests').on('value', (snapshot) => {
        const all = snapshot.val();
        document.getElementById('entries-buy').innerHTML = '';
        document.getElementById('entries-sell').innerHTML = '';
        document.getElementById('entries-trade').innerHTML = '';

        for (let key in all) {
          const entry = all[key];
          const box = document.createElement('div');
          box.className = 'entry';
          box.innerText = Object.entries(entry).filter(([k]) => k !== 'type' && k !== 'timestamp').map(([k, v]) => `${k}: ${v}`).join('\n');
          const containerId = `entries-${entry.type}`;
          const container = document.getElementById(containerId);
          if (container) container.appendChild(box);
        }
      });
    }

    // Загружаем все заявки при старте
    displayRequests();
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Grow a Garden | Requests</title>
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
  </style>
</head>
<body>

  <header>
    <h1>🌱 Grow a Garden Requests 🌻</h1>
    <p>Submit your Buy, Sell or Trade requests</p>
  </header>

  <section>
    <h2>📥 Buy</h2>
    <form onsubmit="sendForm(event, 'buy')">
      <input type="text" data-placeholder="item" placeholder="What do you want to buy?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-buy"></div>
  </section>

  <section>
    <h2>📤 Sell</h2>
    <form onsubmit="sendForm(event, 'sell')">
      <input type="text" data-placeholder="item" placeholder="What do you want to sell?" required />
      <input type="text" data-placeholder="price" placeholder="Price (optional)" />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-sell"></div>
  </section>

  <section>
    <h2>🔁 Trade</h2>
    <form onsubmit="sendForm(event, 'trade')">
      <input type="text" data-placeholder="give" placeholder="What are you giving?" required />
      <input type="text" data-placeholder="want" placeholder="What do you want in return?" required />
      <input type="text" data-placeholder="nick" placeholder="Your Roblox nickname" required />
      <input type="text" data-placeholder="contact" placeholder="Contact (Discord etc.)" />
      <button type="submit">Submit</button>
    </form>
    <div class="entry" id="entries-trade"></div>
  </section>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>

  <script>
    // ⚠️ ЗАМЕНИ НА СВОЙ config Firebase
    const firebaseConfig = {
      apiKey: "ВАШ_API_KEY",
      authDomain: "ВАШ_ПРОЕКТ.firebaseapp.com",
      databaseURL: "https://ВАШ_ПРОЕКТ.firebaseio.com",
      projectId: "ВАШ_ПРОЕКТ",
      storageBucket: "ВАШ_ПРОЕКТ.appspot.com",
      messagingSenderId: "ВАШ_ID",
      appId: "ВАШ_APP_ID",
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    function sendForm(e, type) {
      e.preventDefault();
      const inputs = e.target.querySelectorAll("input");
      let data = {};

      inputs.forEach(input => {
        data[input.dataset.placeholder] = input.value.trim();
      });

      data.type = type;
      data.timestamp = Date.now();

      db.ref('requests').push(data).then(() => {
        alert("Request sent!");
        inputs.forEach(input => input.value = "");
      }).catch(err => {
        alert("Error: " + err.message);
      });
    }

    function displayRequests() {
      db.ref('requests').on('value', (snapshot) => {
        const all = snapshot.val();
        document.getElementById('entries-buy').innerHTML = '';
        document.getElementById('entries-sell').innerHTML = '';
        document.getElementById('entries-trade').innerHTML = '';

        for (let key in all) {
          const entry = all[key];
          const box = document.createElement('div');
          box.className = 'entry';
          box.innerText = Object.entries(entry).filter(([k]) => k !== 'type' && k !== 'timestamp').map(([k, v]) => `${k}: ${v}`).join('\n');
          const containerId = `entries-${entry.type}`;
          const container = document.getElementById(containerId);
          if (container) container.appendChild(box);
        }
      });
    }

    // Загружаем все заявки при старте
    displayRequests();
  </script>
</body>
</html>
