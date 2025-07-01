<script>
  const translations = {
    ru: {
      welcomeTitle: "🌱 Добро пожаловать на сайт Grow a Garden! 🌻",
      welcomeDesc: "Здесь вы можете подать заявки на покупку, продажу и обмен предметов из игры Grow a Garden.",
      buyTitle: "📥 Купить",
      sellTitle: "📤 Продать",
      tradeTitle: "🔁 Обмен",
      placeholders: {
        buy: ["Что вы хотите купить?", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
        sell: ["Что вы продаёте?", "Цена (необязательно)", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
        trade: ["Что вы отдаёте?", "Что хотите взамен?", "Ваш ник в Roblox", "Контакт (Пример DS: Nick TG: Nick)"],
      },
      sendBtn: "Отправить"
    },
    uk: {
      welcomeTitle: "🌱 Ласкаво просимо на сайт Grow a Garden! 🌻",
      welcomeDesc: "Тут ви можете подати заявки на купівлю, продаж і обмін предметів із гри Grow a Garden.",
      buyTitle: "📥 Купити",
      sellTitle: "📤 Продати",
      tradeTitle: "🔁 Обмін",
      placeholders: {
        buy: ["Що ви хочете купити?", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
        sell: ["Що ви продаєте?", "Ціна (необов'язково)", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
        trade: ["Що ви віддаєте?", "Що хочете натомість?", "Ваш нік в Roblox", "Контакт (Приклад DS: Nick TG: Nick)"],
      },
      sendBtn: "Відправити"
    },
    en: {
      welcomeTitle: "🌱 Welcome to the Grow a Garden website! 🌻",
      welcomeDesc: "Here you can submit requests to buy, sell, and trade items from the Grow a Garden game.",
      buyTitle: "📥 Buy",
      sellTitle: "📤 Sell",
      tradeTitle: "🔁 Trade",
      placeholders: {
        buy: ["What do you want to buy?", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
        sell: ["What do you want to sell?", "Price (optional)", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
        trade: ["What are you giving?", "What do you want in return?", "Your Roblox nickname", "Contact (Example DS: Nick TG: Nick)"],
      },
      sendBtn: "Send"
    }
  };

  let currentLang = "ru";
  const ADMIN_TOKEN = "Admin-gag-shop";
  let currentAdminToken = "";

  function updateTexts() {
    const t = translations[currentLang];

    document.getElementById("welcome-title").innerText = t.welcomeTitle;
    document.getElementById("welcome-desc").innerText = t.welcomeDesc;

    document.getElementById("title-buy").innerText = t.buyTitle;
    document.getElementById("title-sell").innerText = t.sellTitle;
    document.getElementById("title-trade").innerText = t.tradeTitle;

    // placeholders
    const formBuyInputs = document.querySelectorAll("#form-buy input");
    t.placeholders.buy.forEach((ph, i) => {
      if(formBuyInputs[i]) formBuyInputs[i].placeholder = ph;
    });

    const formSellInputs = document.querySelectorAll("#form-sell input");
    t.placeholders.sell.forEach((ph, i) => {
      if(formSellInputs[i]) formSellInputs[i].placeholder = ph;
    });

    const formTradeInputs = document.querySelectorAll("#form-trade input");
    t.placeholders.trade.forEach((ph, i) => {
      if(formTradeInputs[i]) formTradeInputs[i].placeholder = ph;
    });

    // buttons
    document.getElementById("btn-buy").innerText = t.sendBtn;
    document.getElementById("btn-sell").innerText = t.sendBtn;
    document.getElementById("btn-trade").innerText = t.sendBtn;
  }

  document.getElementById("lang-select").addEventListener("change", e => {
    currentLang = e.target.value;
    updateTexts();
  });

  // Firebase config и инициализация
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
  function addEntry(type, data) {
    const newRef = db.ref(type).push();
    newRef.set(data);

    let discordMessage = `📝 Заявка: ${type.toUpperCase()}\n`;
    for (const key in data) {
      discordMessage += `**${key}**: ${data[key]}\n`;
    }
    fetch(discordWebhook, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ content: discordMessage }),
    });
  }
  function listenEntries(type, containerId) {
    const container = document.getElementById(containerId);
    const ref = db.ref(type);
    ref.on('value', (snapshot) => 
      const val = snapshot.val();
      container.innerHTML = '';
      if (val) {
        Object.entries(val).forEach(([key, entry]) => {
          let text = '';
          for (const k in entry) {
            text += `${k}: ${entry[k]}\n`;
          }
          const div = document.createElement('div');
          div.classList.add('entry');
          div.textContent = text;
          // Добавляем кнопку удаления, если токен верный
          if(currentAdminToken === ADMIN_TOKEN) {
            div.classList.add('admin');
            const delBtn = document.createElement('button');
            delBtn.textContent = 'Удалить';
            delBtn.className = 'delete-btn';
            delBtn.onclick = () => {
              if(confirm('Удалить эту заявку?')) {
                db.ref(type + '/' + key).remove();
              }
            };
            div.appendChild(delBtn);
          }
          container.appendChild(div);
        });
      } else {
        container.textContent = {
          ru: 'Заявок пока нет.',
          uk: 'Заявок поки немає.',
          en: 'No requests yet.'
        }[currentLang];
      }
    });
  }
  const translationsCaptchaAlert = {
    ru: 'Пожалуйста, подтвердите капчу.',
    uk: 'Будь ласка, підтвердіть капчу.',
    en: 'Please complete the captcha.'
  };
  function getRecaptchaResponse(form) {
    const widget = form.querySelector('.g-recaptcha');
    if (!widget) return null;
    const widgets = document.querySelectorAll('.g-recaptcha');
    const index = Array.from(widgets).indexOf(widget);
    return grecaptcha.getResponse(index);
  }
  // Обработчики форм с проверкой капчи
  ['buy', 'sell', 'trade'].forEach(type => {
    document.getElementById(`form-${type}`).addEventListener('submit', e => {
      e.preventDefault();
      const form = e.target;
      const response = getRecaptchaResponse(form);
      if (!response || response.length === 0) {
        alert(translationsCaptchaAlert[currentLang] || 'Please complete the captcha.');
        return;
      }
      const inputs = form.querySelectorAll('input');
      let data = { time: new Date().toLocaleString() };
      if (type === 'buy') {
        data.item = inputs[0].value.trim();
        data.nick = inputs[1].value.trim();
        data.contact = inputs[2].value.trim() || '-';
      } else if (type === 'sell') {
        data.item = inputs[0].value.trim();
        data.price = inputs[1].value.trim() || '-';
        data.nick = inputs[2].value.trim();
        data.contact = inputs[3].value.trim() || '-';
      } else if (type === 'trade') {
        data.give = inputs[0].value.trim();
        data.want = inputs[1].value.trim();
        data.nick = inputs[2].value.trim();
        data.contact = inputs[3].value.trim() || '-';
      }
      addEntry(type, data);
      form.reset();
      grecaptcha.reset();
    });
  });
  document.getElementById("admin-token-input").addEventListener("input", e => {
    currentAdminToken = e.target.value.trim();
    listenEntries('buy', 'entries-buy');
    listenEntries('sell', 'entries-sell');
    listenEntries('trade', 'entries-trade');
  });
  listenEntries('buy', 'entries-buy');
  listenEntries('sell', 'entries-sell');
  listenEntries('trade', 'entries-trade');
  updateTexts();
</script>
