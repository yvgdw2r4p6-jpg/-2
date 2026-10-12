# -2
Курс кс2 дешево
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Курсы CS2 - Регистрация</title>
  <style>
    :root {
      --primary: #667eea;
      --secondary: #764ba2;
      --accent: #2ecc71;
      --bg-gradient: linear-gradient(135deg, var(--primary), var(--secondary));
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      background: var(--bg-gradient);
      color: #fff;
      margin: 0;
      padding: 0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    /* --- Экран регистрации --- */
    #auth-overlay {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0, 0, 0, 0.95);
      z-index: 1000;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 20px;
      box-sizing: border-box;
      transition: opacity 0.3s;
    }

    .auth-box {
      background: #fff;
      padding: 30px;
      border-radius: 16px;
      width: 100%;
      max-width: 400px;
      color: #333;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    }

    .auth-box h2 { margin-top: 0; color: var(--primary); font-size: 24px; }
    .auth-box p { color: #666; margin-bottom: 20px; font-size: 15px; }
    .auth-box input {
      width: 100%;
      padding: 12px;
      margin-bottom: 12px;
      border: 1px solid #ddd;
      border-radius: 8px;
      box-sizing: border-box;
      font-size: 16px;
    }
    .auth-box button {
      width: 100%;
      padding: 14px;
      background: var(--bg-gradient);
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
    }
    .error-msg { color: #e74c3c; font-size: 13px; margin-bottom: 10px; display: none; }
    .success-msg { color: #2ecc71; font-size: 13px; margin-bottom: 10px; display: none; }

    /* --- Модальное окно подтверждения (имитация письма) --- */
    #verify-modal {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0,0,0,0.8);
      z-index: 2000;
      display: none;
      justify-content: center;
      align-items: center;
      padding: 20px;
      box-sizing: border-box;
    }
    .modal-content {
      background: #fff;
      padding: 30px;
      border-radius: 16px;
      text-align: center;
      max-width: 400px;
      width: 100%;
    }
    .modal-content h3 { color: var(--primary); margin-top: 0; }
    .modal-code {
      font-family: monospace;
      font-size: 24px;
      font-weight: bold;
      color: #333;
      letter-spacing: 5px;
      margin: 20px 0;
      text-align: center;
    }

    /* --- Основной сайт --- */
    #site-content {
      display: none;
      flex: 1;
      opacity: 0;
      transform: translateY(10px);
      transition: all 0.3s ease;
    }
    #site-content.visible {
      display: block;
      opacity: 1;
      transform: translateY(0);
    }

    header { padding: 20px; text-align: center; background: rgba(0,0,0,0.3); }
    header h1 { margin: 0; font-size: 28px; }
    
    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 0 15px;
      flex: 1;
      display: flex;
      flex-direction: column;
    }

    .card {
      background: rgba(255,255,255,0.1);
      padding: 25px;
      border-radius: 12px;
      margin-bottom: 20px;
    }
    .buy-section { margin-top: auto; text-align: center; padding: 20px 0; }
    .price-tag {
      display: inline-block;
      background: rgba(0,0,0,0.3);
      padding: 10px 20px;
      border-radius: 50px;
      font-size: 18px;
      font-weight: bold;
      color: #ffd76e;
      margin-bottom: 20px;
    }
    .buy-btn {
      display: inline-block;
      background: var(--accent);
      color: white;
      padding: 15px 40px;
      text-decoration: none;
      border-radius: 50px;
      font-weight: bold;
      font-size: 18px;
    }
    footer {
      text-align: center;
      padding: 15px;
      background: rgba(0,0,0,0.3);
      font-size: 13px;
    }
  </style>
</head>
<body>

  <!-- 1. ОКНО РЕГИСТРАЦИИ -->
  <div id="auth-overlay">
    <div class="auth-box">
      <h2>Курсы CS2</h2>
      <p>Доступ к материалам только после регистрации</p>
      
      <form id="reg-form">
        <input type="text" id="name" placeholder="Ваше имя" required>
        <!-- type="email" включает проверку браузера + JS ниже -->
        <input type="email" id="email" placeholder="Ваш Email" required>
        <input type="password" id="password" placeholder="Пароль (мин. 6 символов)" required minlength="6">
        
        <div class="error-msg" id="error-msg"></div>
        <div class="success-msg" id="success-msg"></div>
        
        <button type="submit">Получить доступ</button>
      </form>
    </div>
  </div>

  <!-- 2. МОДАЛЬНОЕ ОКНО ПОДТВЕРЖДЕНИЯ (ИМИТАЦИЯ ПИСЬМА) -->
  <div id="verify-modal">
    <div class="modal-content">
      <h3>Письмо отправлено!</h3>
      <p style="color:#666; margin-bottom:20px;">Мы отправили код подтверждения на адрес:<br>
         <strong id="modal-email-display"></strong></p>
      <div class="modal-code" id="fake-code">123456</div>
      <p style="margin-bottom:15px; font-size:14px;">Введите код ниже (в реальности здесь было бы поле ввода)</p>
      <button onclick="confirmCode()" class="buy-btn" style="width:100%; padding:12px;">Подтвердить код</button>
    </div>
  </div>

  <!-- 3. ОСНОВНОЙ КОНТЕНТ -->
  <div id="site-content">
    <header>
      <h1 id="welcome-text">Привет, друг! 🎉</h1>
      <p>Ты внутри. Вот твой курс по CS2.</p>
    </header>

    <div class="container">
      <div class="card">
        <h3>Что ты получишь?</h3>
        <p>Мы разберем механику стрельбы, раскидки гранат на всех картах и тактику для победы в клатчах.</p>
      </div>
      <div class="card">
        <h3>Преимущества:</h3>
        <ul style="list-style: none; padding: 0;">
          <li style="margin-bottom: 10px;">✅ Механика: спреи, контр-спреи.</li>
          <li style="margin-bottom: 10px;">✅ Тактика: раскидки, тайминги.</li>
          <li style="margin-bottom: 10px;">✅ Разбор: личный разбор демо-записей.</li>
        </ul>
      </div>

      <div class="buy-section">
        <div class="price-tag">Стоимость: 2990 ₽</div>
        <a href="tel:+79164259834" class="buy-btn">Оплатить по телефону</a>
        <p style="margin-top: 15px; font-size: 14px; opacity: 0.8;">
          После оплаты отправьте чек на почту: <a href="mailto:mail@example.com" style="color:#ffd76e;">mail@example.com</a>
        </p>
      </div>
    </div>

    <footer>
      <p>© 2026 CS2 Academy. Все права защищены.</p>
    </footer>
  </div>

  <script>
    const form = document.getElementById('reg-form');
    const overlay = document.getElementById('auth-overlay');
    const site = document.getElementById('site-content');
    const verifyModal = document.getElementById('verify-modal');
    const errorMsg = document.getElementById('error-msg');
    const successMsg = document.getElementById('success-msg');
    const modalEmailDisplay = document.getElementById('modal-email-display');

    // Разрешенные домены (защита от фейковых ящиков)
    const allowedDomains = ['gmail.com', 'yandex.ru', 'mail.ru', 'yahoo.com', 'outlook.com'];

    // Проверка при загрузке
    if (localStorage.getItem('cs2_course_registered') === 'true') {
      overlay.style.display = 'none';
      site.classList.add('visible');
      const savedName = localStorage.getItem('cs2_user_name') || 'друг';
      document.getElementById('welcome-text').textContent = `Привет, \${savedName}! 🎉`;
    }

    // Обработка формы
    form.addEventListener('submit', function(e) {
      e.preventDefault();
      errorMsg.style.display = 'none';
      successMsg.style.display = 'none';

      const name = document.getElementById('name').value.trim();
      const email = document.getElementById('email').value.trim().toLowerCase();
      const password = document.getElementById('password').value;

      // 1. Проверка имени
      if (name.length < 2) {
        errorMsg.textContent = 'Имя должно быть длиннее 1 символа';
        errorMsg.style.display = 'block';
        return;
      }

      // 2. Проверка пароля
      if (password.length < 6) {
        errorMsg.textContent = 'Пароль должен быть минимум 6 символов';
        errorMsg.style.display = 'block';
        return;
      }

      // 3. Проверка формата Email (Regex)
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+\$/;
      if (!emailRegex.test(email)) {
        errorMsg.textContent = 'Некорректный формат Email';
        errorMsg.style.display = 'block';
        return;
      }

      // 4. Проверка домена (чтобы не вводили spam@test.com)
      const domain = email.split('@'); [1](https://w3resource.com/javascript/form/email-validation.php)
      if (!allowedDomains.includes(domain)) {
        errorMsg.textContent = `Мы принимаем email только из надежных доменов (\${allowedDomains.join(', ')})`;
        errorMsg.style.display = 'block';
        return;
      }

      // Если все проверки пройдены -> показываем "Письмо отправлено"
      modalEmailDisplay.textContent = email;
      verifyModal.style.display = 'flex';
    });

    // Функция подтверждения кода (имитация)
    function confirmCode() {
      verifyModal.style.display = 'none';
      
      // Сохраняем данные
      localStorage.setItem('cs2_course_registered', 'true');
      localStorage.setItem('cs2_user_name', document.getElementById('name').value.trim());

      // Показываем сайт
      overlay.style.opacity = '0';
      setTimeout(() => {
        overlay.style.display = 'none';
        site.classList.add('visible');
        document.getElementById('welcome-text').textContent = `Привет, \${document.getElementById('name').value.trim()}! 🎉`;
      }, 300);
    }
  </script>
</body>
</html>
