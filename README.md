# TK-
Тут можно написать жалобу или отзыв
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Good Hub — Отзывы и жалобы</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Arial, sans-serif; }
  body {
    min-height: 100vh;
    background: linear-gradient(135deg, #1a1a2e, #16213e, #0f3460);
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
  }
  .card {
    background: rgba(255,255,255,0.08);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 20px;
    padding: 40px;
    max-width: 520px;
    width: 100%;
    box-shadow: 0 15px 40px rgba(0,0,0,0.4);
  }
  h1 { font-size: 28px; margin-bottom: 10px; }
  h1 span { color: #4fc3f7; }
  .sub { color: #b0b8c9; margin-bottom: 25px; font-size: 15px; }
  label { display: block; margin: 15px 0 6px; font-size: 14px; color: #cfd6e4; }
  input[type="text"], textarea {
    width: 100%;
    padding: 12px 14px;
    border-radius: 10px;
    border: 1px solid rgba(255,255,255,0.2);
    background: rgba(0,0,0,0.3);
    color: #fff;
    font-size: 15px;
    outline: none;
    transition: border 0.2s;
  }
  input:focus, textarea:focus { border-color: #4fc3f7; }
  textarea { min-height: 110px; resize: vertical; }
  .stars { display: flex; gap: 6px; margin-top: 5px; }
  .star {
    font-size: 34px;
    cursor: pointer;
    color: #555;
    transition: transform 0.15s, color 0.15s;
    user-select: none;
  }
  .star:hover { transform: scale(1.2); }
  .star.active { color: #ffc107; }
  .hint { font-size: 13px; color: #8fa3b8; margin-top: 5px; min-height: 18px; }
  button {
    width: 100%;
    margin-top: 25px;
    padding: 15px;
    border: none;
    border-radius: 12px;
    background: linear-gradient(90deg, #4fc3f7, #2196f3);
    color: #fff;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.1s;
  }
  button:hover { opacity: 0.9; }
  button:active { transform: scale(0.98); }
  button:disabled { opacity: 0.5; cursor: not-allowed; }
  #status { text-align: center; margin-top: 15px; font-size: 15px; min-height: 20px; }
  .ok { color: #66ff99; }
  .err { color: #ff6b6b; }
</style>
</head>
<body>

<div class="card">
  <h1>👋 Привет! Это <span>Good Hub</span></h1>
  <p class="sub">Тут вы можете написать жалобу на нас!! Или положительный отзыв 😊</p>

  <form id="review-form" action="https://formspree.io/f/ВАШ_ID" method="POST">

    <label for="name">Ваше имя (можно анонимно):</label>
    <input type="text" id="name" name="Имя" placeholder="Например: Аноним">

    <label>Оценка:</label>
    <div class="stars" id="stars">
      <span class="star" data-value="1">★</span>
      <span class="star" data-value="2">★</span>
      <span class="star" data-value="3">★</span>
      <span class="star" data-value="4">★</span>
      <span class="star" data-value="5">★</span>
    </div>
    <div class="hint" id="hint">Поставьте звёзды</div>
    <input type="hidden" name="Оценка" id="rating" required>

    <label for="message">Ваш отзыв или жалоба:</label>
    <textarea id="message" name="Отзыв" placeholder="Напишите всё, что думаете о нас..." required></textarea>

    <button type="submit" id="btn">Отправить 🚀</button>
    <div id="status"></div>
  </form>
</div>

<script>
  const stars = document.querySelectorAll('.star');
  const ratingInput = document.getElementById('rating');
  const hint = document.getElementById('hint');
  const phrases = [
    'Поставьте звёзды',
    'Ужасно 😡',
    'Плохо 😕',
    'Нормально 🙂',
    'Хорошо 😊',
    'Отлично! 😍'
  ];

  stars.forEach(star => {
    star.addEventListener('click', () => {
      const value = +star.dataset.value;
      ratingInput.value = value;
      hint.textContent = phrases[value];
      stars.forEach(s => s.classList.toggle('active', +s.dataset.value <= value));
    });
  });

  const form = document.getElementById('review-form');
  const btn = document.getElementById('btn');
  const status = document.getElementById('status');

  form.addEventListener('submit', async (e) => {
    e.preventDefault();
    if (!ratingInput.value) {
      status.textContent = 'Поставьте звёзды, пожалуйста!';
      status.className = 'err';
      return;
    }
    btn.disabled = true;
    btn.textContent = 'Отправляем...';
    try {
      const resp = await fetch(form.action, {
        method: 'POST',
        body: new FormData(form),
        headers: { 'Accept': 'application/json' }
      });
      if (resp.ok) {
        status.textContent = 'Спасибо! Ваш отзыв отправлен 💙';
        status.className = 'ok';
        form.reset();
        stars.forEach(s => s.classList.remove('active'));
        hint.textContent = phrases[0];
      } else {
        throw new Error('Ошибка');
      }
    } catch {
      status.textContent = 'Не получилось отправить. Попробуйте ещё раз.';
      status.className = 'err';
    }
    btn.disabled = false;
    btn.textContent = 'Отправить 🚀';
  });
</script>

</body>
</html>

