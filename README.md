<html>
<head>
<meta charset="UTF-8">
<title>NovaMarket</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

  * { box-sizing: border-box; margin:0; padding:0; font-family: 'Roboto', sans-serif; }
  body { background: linear-gradient(to bottom, #1a1a2e, #162447); color: #fff; }
  
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 50px;
    background: #0f3460;
    border-bottom: 2px solid #e94560;
  }
  header h1 { font-family: 'Roboto', sans-serif; color: #e94560; }
  nav a {
    color: #fff;
    text-decoration: none;
    margin-left: 20px;
    font-weight: bold;
  }
  nav a:hover { color: #e94560; }

  .balance {
    background: #162447;
    padding: 8px 15px;
    border-radius: 12px;
    border: 1px solid #e94560;
  }

  .hero {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 50px;
  }
  .hero-text h2 { font-size: 36px; color: #f9ed69; }
  .hero-text p { margin-top: 10px; font-size: 18px; color: #fff; }
  .hero img { width: 300px; border-radius: 20px; border: 2px solid #e94560; }

  .shop {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
    padding: 50px;
  }

  .card {
    background: #0f3460;
    border: 2px solid #e94560;
    border-radius: 20px;
    overflow: hidden;
    text-align: center;
    transition: transform 0.2s;
  }
  .card:hover { transform: scale(1.05); }
  .card img { width: 100%; height: 200px; object-fit: cover; }
  .card h3 { margin: 10px 0; color: #f9ed69; }
  .card p { margin-bottom: 15px; }
  .card button {
    background: #e94560;
    border: none;
    padding: 10px 20px;
    border-radius: 12px;
    color: #fff;
    cursor: pointer;
    margin-bottom: 15px;
  }
  .card button:hover { background: #f9ed69; color:#0f3460; }

  footer {
    text-align: center;
    padding: 20px;
    background: #0f3460;
    border-top: 2px solid #e94560;
    margin-top: 50px;
  }

</style>
</head>
<body>

<header>
  <h1>NovaMarket</h1>
  <nav>
    <a href="#">Главная</a>
    <a href="#">Магазин</a>
    <a href="#">Профиль</a>
  </nav>
  <div class="balance">Баланс: <span id="balance">100</span> ₦</div>
</header>

<section class="hero">
  <div class="hero-text">
    <h2>Добро пожаловать в NovaMarket!</h2>
    <p>Купи любимый контент в стиле аниме прямо сейчас!</p>
  </div>
  <img src="https://i.imgur.com/5bU1t7T.png" alt="Аниме персонаж">
</section>

<section class="shop">
  <div class="card">
    <img src="https://i.imgur.com/q1Y6gZB.jpg" alt="Товар 1">
    <h3>Эротический арт #1</h3>
    <p>Цена: 50 ₦</p>
    <button onclick="buyItem(50)">Купить</button>
  </div>
  <div class="card">
    <img src="https://i.imgur.com/0Q1M6Pe.jpg" alt="Товар 2">
    <h3>Эротический арт #2</h3>
    <p>Цена: 70 ₦</p>
    <button onclick="buyItem(70)">Купить</button>
  </div>
  <div class="card">
    <img src="https://i.imgur.com/jk9eE6k.jpg" alt="Товар 3">
    <h3>Эротический арт #3</h3>
    <p>Цена: 100 ₦</p>
    <button onclick="buyItem(100)">Купить</button>
  </div>
</section>

<footer>
  NovaMarket &copy; 2025
</footer>

<script>
  let balance = 100;
  function buyItem(price) {
    if(balance >= price){
      balance -= price;
      document.getElementById('balance').innerText = balance;
      alert('Покупка успешна!');
    } else {
      alert('Недостаточно средств!');
    }
  }
</script>

</body>
</html>
