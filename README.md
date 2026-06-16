<!DOCTYPE html>
<html lang="ru">
<head>
<!-- === FIREBASE === -->
<script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-firestore-compat.js"></script>

<script>
// ТВОЙ CONFIG (вставь сюда)
const firebaseConfig = {
  apiKey: "AIzaSyA-I6ahSmt6C6O26iq58XGIo7fOIl58PMo",
  authDomain: "votehubpols.firebaseapp.com",
  projectId: "votehubpols",
  storageBucket: "votehubpols.firebasestorage.app",
  messagingSenderId: "1078130388423",
  appId: "1:1078130388423:web:3d12595bebc82abf24fde2"
};

const app = firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();
</script>

  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>VoteHub - Система онлайн-голосований</title>

  <style>
    :root {
      --black: #000000;
      --dark: #0a0a0a;
      --gray: #1a1a1a;
      --neon: #ffea80;
      --yellow: #ffff99;
      --dim: #cccc66;
      --text: #e0e0e0;
    }
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      font-family: 'Roboto Mono', monospace;
      background: #000 url('https://avatars.mds.yandex.net/i?id=7d6d85da3a7103a8375d92a41c79c9a1_l-5544445-images-thumbs&n=13') center/cover no-repeat fixed;
      color: var(--text);
      min-height: 100vh;
      padding: 20px;
      background-attachment: fixed;
    }
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background: linear-gradient(rgba(0,0,0,0.88), rgba(0,0,0,0.95));
      z-index: -1;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      background: rgba(12,12,18,0.97);
      backdrop-filter: blur(22px);
      border-radius: 26px;
      overflow: hidden;
      border: 3px solid var(--dim);
      box-shadow: 0 0 140px rgba(255,234,128,0.6),
                  inset 0 0 80px rgba(255,255,255,0.04);
    }
    header {
      background: url('https://avatars.mds.yandex.net/i?id=a06339c861ffaac08ca28a03566b62ad_l-6951238-images-thumbs&n=13') center/cover;
      padding: 95px 30px 65px;
      text-align: center;
      position: relative;
    }
    header::before {
      content: '';
      position: absolute;
      inset: 0;
      background: linear-gradient(rgba(0,0,0,0.78), rgba(0,0,0,0.92));
    }
    header > * { position: relative; z-index: 1; }
    header h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 5.6rem;
      color: var(--neon);
      text-shadow: 0 0 35px var(--neon), 0 0 70px var(--yellow), 0 0 110px rgba(255,234,128,0.7);
      letter-spacing: 16px;
      margin-bottom: 15px;
    }
    header p {
      font-size: 1.55rem;
      color: var(--dim);
      text-shadow: 0 0 25px rgba(255,255,150,0.6);
    }
    nav {
      background: rgba(10,10,15,0.98);
      padding: 20px;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 15px;
      border-bottom: 5px solid var(--dim);
      position: relative;
    }
    nav button {
      background: linear-gradient(135deg, #1f1f25, #2a2a35);
      border: 2px solid var(--dim);
      color: var(--neon);
      padding: 15px 32px;
      border-radius: 16px;
      cursor: pointer;
      font-size: 1.1rem;
      font-weight: 500;
      transition: all 0.4s ease;
      box-shadow: 0 0 20px rgba(255,234,128,0.35);
    }
    nav button:hover, nav button.active {
      background: linear-gradient(135deg, #333844, #444955);
      border-color: var(--neon);
      color: #fff;
      box-shadow: 0 0 45px var(--neon), 0 10px 30px rgba(255,234,128,0.5);
      transform: translateY(-5px) scale(1.06);
    }
    .section { padding: 55px 45px; display: none; }
    .section.active { display: block; }
    h2 {
      font-family: 'Orbitron', sans-serif;
      color: var(--neon);
      font-size: 3.1rem;
      text-align: center;
      margin-bottom: 45px;
      text-shadow: 0 0 35px var(--yellow), 0 0 70px rgba(255,234,128,0.6);
    }
    input, textarea {
      width: 100%;
      padding: 16px 20px;
      margin: 16px 0;
      border: 2px solid var(--dim);
      border-radius: 12px;
      background: rgba(10,10,15,0.9);
      color: #fff;
      font-size: 1.15rem;
      box-shadow: 0 0 20px rgba(255,234,128,0.25);
    }
    .option {
      display: flex;
      gap: 16px;
      margin: 16px 0;
      padding: 18px;
      background: #111;
      border: 2px solid var(--dim);
      border-radius: 14px;
      align-items: center;
    }
    .option input { flex:1; background:transparent; border:none; color:#fff; font-size:1.1rem; }
    button.main, .ai-btn, .save-btn {
      background: linear-gradient(135deg, var(--neon), var(--yellow));
      color: #000;
      border: none;
      padding: 18px 65px;
      font-size: 1.35rem;
      font-weight: 700;
      border-radius: 50px;
      cursor: pointer;
      margin: 35px auto;
      display: block;
      box-shadow: 0 0 55px rgba(255,234,128,0.95),
                  0 12px 35px rgba(255,234,128,0.6);
      transition: all 0.4s ease;
    }
    button.main:hover, .ai-btn:hover, .save-btn:hover {
      transform: translateY(-8px) scale(1.1);
      box-shadow: 0 0 90px var(--neon),
                  0 18px 45px rgba(255,234,128,0.8);
    }
    .ai-btn {
      background: linear-gradient(135deg, #2a2a35, var(--neon));
      color: #fff;
    }
    .poll-question {
      font-family: 'Orbitron', sans-serif;
      font-size: 2.6rem;
      text-align: center;
      color: var(--neon);
      margin: 45px 0 55px;
      text-shadow: 0 0 30px var(--yellow);
    }
    label.option-label {
      display: flex;
      align-items: center;
      gap: 20px;
      background: #0c0c0c;
      margin: 20px 0;
      padding: 24px 30px;
      border-radius: 16px;
      cursor: pointer;
      border: 2px solid var(--dim);
      font-size: 1.35rem;
      transition: all 0.35s;
    }
    label.option-label:hover {
      border-color: var(--neon);
      background: rgba(255,255,0,0.1);
      box-shadow: 0 0 55px rgba(255,234,128,0.7);
      transform: translateX(8px);
    }
    .results-bar {
      height: 48px;
      background: #111;
      border-radius: 30px;
      margin: 22px 0;
      overflow: hidden;
      border: 2px solid var(--dim);
      position: relative;
    }
    .bar-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--neon), var(--yellow));
      transition: width 1.8s ease;
    }
    .bar-text {
      position: absolute;
      left: 22px;
      top: 50%;
      transform: translateY(-50%);
      color: #000;
      font-weight: 900;
      font-size: 1.25rem;
    }
    .stats-cards {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
      gap: 26px;
    }
    .stats-card {
      background: rgba(15,15,22,0.95);
      padding: 28px;
      border-radius: 20px;
      border: 2px solid var(--dim);
      transition: all 0.45s ease;
      box-shadow: 0 10px 35px rgba(0,0,0,0.6);
      position: relative;
    }
    .stats-card:hover {
      transform: translateY(-14px);
      box-shadow: 0 0 80px rgba(255,234,128,0.75);
      border-color: var(--neon);
    }
    .top1-card {
      background: linear-gradient(135deg, #1c1c22, #2f2f1f);
      border-color: var(--neon);
      box-shadow: 0 0 90px rgba(255,234,128,0.65);
    }
    .delete-btn {
      position: absolute;
      top: 16px;
      right: 16px;
      background: #ff4444;
      color: white;
      border: none;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      font-size: 20px;
      cursor: pointer;
      box-shadow: 0 0 20px rgba(255,68,68,0.7);
    }
    .message {
      text-align:center;
      font-size:2rem;
      margin:55px 0;
      color:var(--neon);
      text-shadow:0 0 35px var(--yellow);
    }
    .hidden { display:none !important; }
    #languagePanel {
      position: fixed;
      bottom: 110px;
      right: 30px;
      background: rgba(10,10,15,0.97);
      border: 3px solid var(--dim);
      border-radius: 18px;
      padding: 20px;
      box-shadow: 0 0 60px rgba(255,234,128,0.7);
      display: none;
      flex-direction: column;
      gap: 12px;
      z-index: 1000;
      min-width: 230px;
    }
    .lang-btn {
      padding: 14px 24px;
      background: linear-gradient(135deg, #222, #333);
      color: var(--neon);
      border: 2px solid var(--dim);
      border-radius: 12px;
      cursor: pointer;
      font-size: 1.1rem;
      font-weight: 500;
      transition: all 0.3s;
      text-align: center;
    }
    .lang-btn:hover {
      background: linear-gradient(135deg, var(--neon), var(--yellow));
      color: #000;
      border-color: var(--neon);
      box-shadow: 0 0 35px var(--neon);
      transform: translateY(-3px);
    }
    #colorPanel {
      position: fixed;
      bottom: 30px;
      right: 30px;
      background: rgba(10,10,15,0.97);
      border: 3px solid var(--dim);
      border-radius: 18px;
      padding: 15px 20px;
      box-shadow: 0 0 50px rgba(255,234,128,0.6);
      display: none;
      flex-direction: column;
      gap: 12px;
      z-index: 1000;
    }
    .color-option {
      width: 55px;
      height: 55px;
      border-radius: 12px;
      cursor: pointer;
      border: 3px solid #fff;
      box-shadow: 0 0 15px rgba(255,255,255,0.6);
      transition: all 0.3s;
    }
    .color-option:hover {
      transform: scale(1.25);
    }
    footer {
      text-align: center;
      padding: 38px;
      background: #000;
      color: var(--dim);
      font-size: 1.2rem;
      border-top: 5px solid var(--yellow);
      text-shadow: 0 0 20px rgba(255,255,150,0.5);
    }
    .online-timer {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.8rem;
      color: var(--neon);
      text-align: center;
      margin: 20px 0;
      text-shadow: 0 0 20px var(--yellow);
    }
    .public-card {
      border: 3px solid var(--neon);
      background: linear-gradient(135deg, #1c1c22, #2f2f1f);
    }

   /* ==================== ПРОФИЛЬ ==================== */
.profile-container {
  position: absolute;
  top: 22px;
  right: 28px;
  z-index: 1000;
  display: block !important;
}

.profile-avatar {
  width: 52px;
  height: 52px;
  border-radius: 50%;
  border: 3.5px solid var(--neon);
  cursor: pointer;
  object-fit: cover;
  box-shadow: 0 0 25px rgba(255,234,128,0.8),
              0 0 45px rgba(255,234,128,0.4);
  transition: all 0.3s ease;
}

.profile-avatar:hover {
  transform: scale(1.15);
  box-shadow: 0 0 40px var(--neon),
              0 0 60px rgba(255,234,128,0.6);
  border-color: var(--yellow);
}

.profile-dropdown {
  position: absolute;
  top: 78px;
  right: 0;
  background: rgba(15,15,25,0.98);
  border: 3px solid var(--dim);
  border-radius: 20px;
  width: 290px;
  padding: 22px;
  box-shadow: 0 0 70px rgba(255,234,128,0.6);
  display: none;
  flex-direction: column;
  gap: 14px;
  z-index: 1100;
}

.profile-dropdown.show {
  display: flex;
}

.profile-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 10px;
}

.profile-header img {
  width: 68px;
  height: 68px;
  border-radius: 50%;
  border: 3px solid var(--neon);
}

.profile-info h3 {
  color: var(--neon);
  margin: 0;
  font-size: 1.35rem;
}

.profile-info p {
  color: #bbb;
  margin: 4px 0 0;
  font-size: 0.95rem;
}

.profile-dropdown button {
  background: rgba(255,234,128,0.08);
  border: 2px solid var(--dim);
  color: var(--neon);
  padding: 13px;
  border-radius: 14px;
  cursor: pointer;
  font-size: 1.05rem;
  transition: all 0.3s;
}

.profile-dropdown button:hover {
  background: var(--neon);
  color: #000;
  border-color: var(--neon);
}

.logout-btn {
  background: #ff4444 !important;
  color: white !important;
}

.logout-btn:hover {
  background: #ff6666 !important;
}

/* Адаптив для мобильных */
@media (max-width: 768px) {
  .profile-container {
    top: 16px !important;
    right: 16px !important;
  }
  .profile-avatar {
    width: 46px;
    height: 46px;
  }
}

@media (max-width: 480px) {
  .profile-container {
    top: 14px !important;
    right: 12px !important;
  }
  .profile-avatar {
    width: 42px;
    height: 42px;
  }
}

/* Увеличенная аватарка при клике */
#enlargeModal {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.95);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  cursor: zoom-out;
}

#enlargeModal img {
  max-width: 85%;
  max-height: 85vh;
  border: 6px solid var(--neon);
  border-radius: 20px;
  box-shadow: 0 0 80px rgba(255,234,128,0.8);
}

/* ====================== КРАСИВАЯ ШАПКА + ВЗРОСЛЫЙ СТИЛЬ ====================== */
header {
  padding: 110px 30px 80px;
  position: relative;
  overflow: hidden;
}

.header-logo-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 95px;
  margin-bottom: 15px;
  position: relative;
  z-index: 2;
}

.logo-gif-left {
  width: 145px;
  height: 145px;
  border-radius: 50%;
  background: url('https://i.ibb.co.com/NdPKw47K/pinnsaver-7c106781bfced41d26fadaeb006507a6-cropped-ezgif-com-gif-to-apng-converter.gif') center/cover no-repeat;
  border: 6px solid rgba(255,234,128,0.65);
  box-shadow: 
    0 0 35px var(--neon),
    0 0 75px var(--yellow),
    inset 0 0 25px rgba(255,255,255,0.15);
  flex-shrink: 0;
  animation: butterfly-glow 5s infinite alternate ease-in-out;
  transition: all 0.4s ease;
  filter: drop-shadow(0 0 25px var(--neon));
}

.logo-gif-left:hover {
  transform: scale(1.06);
  border-color: var(--neon);
}

#header-title {
  margin: 0;
  font-size: 5.4rem;
  font-weight: 900;
  color: var(--neon);
  text-shadow: 
    0 0 25px var(--neon),
    0 0 55px var(--yellow),
    0 0 95px rgba(255,234,128,0.6);
  letter-spacing: 22px;
  line-height: 1;
}

/* Лёгкий градиентный фон под шапкой */
header::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 30% 20%, rgba(255,234,128,0.12) 0%, transparent 50%);
  pointer-events: none;
  z-index: 1;
  animation: subtlePulse 12s infinite alternate ease-in-out;
}

@keyframes subtlePulse {
  from { opacity: 0.6; }
  to   { opacity: 1; }
}

/* Анимация свечения бабочки */
@keyframes butterfly-glow {
  from {
    box-shadow: 0 0 30px var(--neon), 0 0 70px var(--yellow), inset 0 0 20px rgba(255,255,255,0.1);
  }
  to {
    box-shadow: 0 0 50px var(--neon), 0 0 105px var(--yellow), inset 0 0 30px rgba(255,255,255,0.25);
  }
}

/* Улучшенный подзаголовок */
#header-subtitle {
  font-size: 1.45rem;
  line-height: 1.65;
  opacity: 0.9;
  text-shadow: 0 0 20px rgba(255,234,128,0.4);
  z-index: 2;
  position: relative;
}

/* Улучшения для серого цвета */
:root {
  --gray-bg: #1a1a1a;
}

body[data-theme="gray"] .container,
body[data-theme="gray"] .option,
body[data-theme="gray"] .stats-card,
body[data-theme="gray"] input,
body[data-theme="gray"] textarea {
    background: var(--gray-bg);
    border-color: #777;
}

body[data-theme="gray"] .main,
body[data-theme="gray"] .ai-btn {
    filter: brightness(0.95);
}

/* ===================== СИЛЬНЫЙ АДАПТИВ ДЛЯ ТЕЛЕФОНА + ПРОФИЛЬ ===================== */

@media (max-width: 768px) {

    body {
        padding: 8px;
        font-size: 16px;
    }

    .container {
        border-radius: 18px;
        margin: 5px auto;
    }

    /* ШАПКА */
    header {
        padding: 50px 15px 35px !important;
    }

    .header-logo-wrapper {
        gap: 20px;
        flex-direction: column;
        align-items: center;
    }

    .logo-gif-left {
        width: 88px;
        height: 88px;
    }

    #header-title {
        font-size: 2.7rem !important;
        letter-spacing: 5px;
    }

    #header-subtitle {
        font-size: 1.05rem !important;
    }

    /* НАВИГАЦИЯ */
    nav {
        padding: 10px 8px;
        gap: 6px;
        flex-wrap: wrap;
        justify-content: center;
    }

    nav button {
        padding: 11px 13px;
        font-size: 0.95rem;
    }

    /* ПРОФИЛЬ — поднимаем в самый правый верхний угол */
    .profile-container {
        position: absolute !important;
        top: 15px !important;
        right: 15px !important;
        z-index: 300;
    }

    .profile-avatar {
        width: 44px;
        height: 44px;
        border: 2.5px solid var(--neon);
    }

    /* Заголовки и контент */
    h2 {
        font-size: 2rem !important;
        margin-bottom: 25px;
    }

    .poll-question {
        font-size: 1.75rem !important;
        margin: 25px 0 30px;
    }

    .section {
        padding: 25px 15px !important;
    }

    /* Кнопки и блоки */
    .option, .option-label, .stats-card {
        padding: 15px;
        margin: 10px 0;
        font-size: 1.05rem;
    }

    button.main, .ai-btn, .save-btn {
        padding: 17px 30px !important;
        font-size: 1.22rem !important;
        width: 94% !important;
        max-width: 340px;
        margin: 25px auto;
    }

    /* Панели снизу */
    #languagePanel, #colorPanel {
        right: 12px;
        bottom: 15px;
        width: 90%;
        max-width: 270px;
    }

    .color-option {
        width: 50px;
        height: 50px;
    }
}

/* Очень маленькие экраны */
@media (max-width: 480px) {

    header {
        padding: 45px 12px 30px !important;
    }

    #header-title {
        font-size: 2.45rem !important;
    }

    .profile-container {
        top: 12px !important;
        right: 12px !important;
    }

    .profile-avatar {
        width: 40px;
        height: 40px;
    }

    nav button {
        padding: 10px 11px;
        font-size: 0.9rem;
    }

    .section {
        padding: 20px 12px !important;
    }

    button.main, .ai-btn {
        font-size: 1.15rem !important;
        padding: 15px 25px !important;
    }
}

/* ===================== БЕЛАЯ ПОДСВЕТКА ===================== */
body[data-theme="white"] .container,
body[data-theme="white"] .option,
body[data-theme="white"] .stats-card,
body[data-theme="white"] input,
body[data-theme="white"] textarea {
    background: #ffffff;
    border-color: #cccccc;
    color: #222222;
}

body[data-theme="white"] .option-label {
    background: #f8f8f8;
}

body[data-theme="white"] h1, 
body[data-theme="white"] h2, 
body[data-theme="white"] h3 {
    color: #333333;
    text-shadow: none;
}

body[data-theme="white"] nav {
    background: rgba(255,255,255,0.95);
}
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.stats-card {
  background: #1e1e1e;
  padding: 20px;
  border-radius: 12px;
  border: 1px solid #333;
  transition: all 0.3s;
}

.stats-card:hover {
  transform: translateY(-4px);
  border-color: var(--neon);
}

/* ====================== НОВАЯ ТЕМА — РЕАЛЬНЫЙ ОГОНЬ ====================== */
body[data-theme="fire"] {
  --neon: #ffcc00;
  --yellow: #ffff44;
  --dim: #ff6600;
  --fire: #ff3300;
}

body[data-theme="fire"] .container {
  border-color: #ff8800;
  box-shadow: 
    0 0 90px #ff3300,
    0 0 160px #ff8800,
    0 0 220px rgba(255, 100, 0, 0.7),
    inset 0 0 120px rgba(255, 220, 100, 0.15) !important;
}

body[data-theme="fire"] #header-title {
  color: #ffff00;
  text-shadow: 
    0 0 40px #ffff00,
    0 0 80px #ff8800,
    0 0 130px #ff3300,
    0 0 190px #ff0000,
    0 0 240px rgba(255, 80, 0, 0.9) !important;
  animation: realFire 2.8s infinite alternate ease-in-out;
}

@keyframes realFire {
  from { text-shadow: 0 0 40px #ffff00, 0 0 90px #ff5500, 0 0 150px #ff2200; }
  to   { text-shadow: 0 0 60px #ffff66, 0 0 120px #ff8800, 0 0 200px #ff3300, 0 0 260px #ff0000; }
}

body[data-theme="fire"] .stats-card,
body[data-theme="fire"] .public-card {
  border-color: #ff6600;
  box-shadow: 
    0 15px 60px rgba(0,0,0,0.9),
    0 0 70px #ff5500,
    0 0 110px rgba(255, 120, 0, 0.8),
    inset 0 0 60px rgba(255, 200, 50, 0.25) !important;
}

body[data-theme="fire"] .stats-card:hover,
body[data-theme="fire"] .public-card:hover {
  transform: translateY(-16px) scale(1.04);
  box-shadow: 
    0 25px 90px #ff3300,
    0 0 140px #ffff00,
    0 0 200px rgba(255, 80, 0, 0.9) !important;
}

body[data-theme="fire"] button.main,
body[data-theme="fire"] .ai-btn,
body[data-theme="fire"] .save-btn {
  background: linear-gradient(135deg, #ff3300, #ff8800, #ffff00);
  box-shadow: 
    0 0 60px #ff3300,
    0 0 110px #ffaa00,
    0 25px 60px rgba(255, 60, 0, 0.9) !important;
}

body[data-theme="fire"] button.main:hover,
body[data-theme="fire"] .ai-btn:hover,
body[data-theme="fire"] .save-btn:hover {
  box-shadow: 
    0 0 100px #ff0000,
    0 0 170px #ffff00,
    0 35px 80px rgba(255, 100, 0, 1) !important;
  transform: translateY(-12px) scale(1.12);
}

body[data-theme="fire"] .bar-fill {
  background: linear-gradient(90deg, #ff0000, #ff8800, #ffff00);
  box-shadow: 0 0 40px #ffff00;
}

.premium-poll-card {
  position: relative;
}

.premium-poll-card::before {
  content: '';
  position: absolute;
  top: -2px; left: -2px; right: -2px; bottom: -2px;
  background: linear-gradient(45deg, #00ff9d, #ffd700, #00ff9d);
  border-radius: 26px;
  z-index: -1;
  filter: blur(8px);
  opacity: 0.15;
  transition: opacity 0.4s;
}

.premium-poll-card:hover::before {
  opacity: 0.35;
}

.nav-btn {
  background: linear-gradient(145deg, #1a1a2e, #16213e);
  color: var(--yellow);
  border: 2px solid var(--dim);
  padding: 12px 22px;
  border-radius: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.nav-btn:hover {
  border-color: var(--neon);
  transform: translateY(-3px);
  box-shadow: 0 0 25px rgba(0, 255, 200, 0.4);
}

/* ===================== СИЛЬНЫЙ МОБИЛЬНЫЙ ФИКС ===================== */

@media (max-width: 768px) {
    
    /* Главный контейнер */
    .container {
        margin: 8px auto !important;
        padding: 0 !important;
        border-radius: 16px !important;
    }

    /* Карточки — главное исправление */
    .stats-card,
    .public-card,
    #pollsContainer .stats-card,
    #online-poll-list .stats-card,
    #multiPollsList .stats-card,
    #polls .stats-card {
        width: 100% !important;
        max-width: 100% !important;
        padding: 20px 18px !important;
        margin: 12px 0 !important;
        box-sizing: border-box !important;
        border-radius: 16px !important;
    }

    /* Убираем сжатие по ширине */
    .stats-grid,
    #pollsContainer,
    #online-poll-list,
    #multiPollsList,
    .stats-cards {
        grid-template-columns: 1fr !important;
        gap: 16px !important;
        padding: 0 4px !important;
    }

    /* Внутреннее содержимое карточек */
    .stats-card h3,
    .stats-card h4 {
        font-size: 1.4rem !important;
        line-height: 1.3 !important;
    }

    .stats-card p {
        font-size: 1.08rem !important;
        line-height: 1.5 !important;
    }

    /* Аватарки в карточках */
    .stats-card img {
        width: 52px !important;
        height: 52px !important;
    }

    /* Кнопки */
    .stats-card button {
        width: 100% !important;
        margin-top: 12px !important;
        padding: 14px !important;
        font-size: 1.1rem !important;
    }

    /* Таймер */
    .online-timer {
        font-size: 1.25rem !important;
        text-align: center !important;
        padding: 10px 0 !important;
    }

    /* Убираем лишние отступы слева */
    .stats-card > * {
        margin-left: 0 !important;
        padding-left: 0 !important;
    }
}

/* Очень маленькие экраны */
@media (max-width: 480px) {
    .stats-card,
    .public-card {
        padding: 16px 14px !important;
    }
    
    .stats-card h3,
    .stats-card h4 {
        font-size: 1.3rem !important;
    }
}

/* Стили для графиков в результатах */
.vote-results {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 25px;
  margin: 35px 0;
}

.chart-box {
  background: rgba(15,15,25,0.95);
  border: 3px solid var(--dim);
  border-radius: 22px;
  padding: 20px;
  box-shadow: 0 0 50px rgba(255,234,128,0.4);
}

@media (max-width: 768px) {
  .vote-results { 
    grid-template-columns: 1fr; 
  }
}
  </style>
<script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
</head>
<body>
<div class="container">
  <header>
  <div class="header-logo-wrapper">
    <span class="logo-gif-left"></span>
    <h1 id="header-title">VoteHub</h1>
  </div>
 <p id="header-subtitle">Современная система онлайн-голосований и опросов<br>Создавай • Голосуй • Анализируй</p>
</header>

  <nav>
    <button onclick="showSection('home')" class="active" id="nav-home">Главная 🏠</button>
    <button onclick="showSection('create')" id="nav-create">Создать опрос ✍️</button>
    <button onclick="showSection('vote')" id="nav-vote">Голосовать 🗳️</button>
    <button onclick="showSection('results')" id="nav-results">Результаты 📊</button>
    <button onclick="showSection('saved')" id="nav-saved">Сохранённые 💾</button>
    <button onclick="showSection('stats')" id="nav-stats">Статистика 📈</button>
    <button onclick="showSection('online-stats')">🌐 Онлайн-статистика</button>
    <button onclick="showSection('online')" id="nav-online">🌐 Онлайн опросы</button>
 <button onclick="showSection('multi')" id="nav-multi">🔀 Мульти-голосование</button>
<button onclick="showSection('create-global')" 
        style="background: linear-gradient(135deg, #ff3366, #ff8800); color: #fff; font-weight: bold; border: 2px solid #ffff00;">
    ✨ Создать Глобальный Опрос
</button>
<button onclick="showSection('polls')" 
        style="background: linear-gradient(145deg, #1a1a2e, #16213e); 
               color: var(--yellow); 
               border: 2px solid var(--neon); 
               padding: 12px 24px; 
               border-radius: 12px; 
               font-weight: 600; 
               cursor: pointer; 
               transition: all 0.3s; 
               display: flex; 
               align-items: center; 
               gap: 8px;">
    <span>🧠</span> Опросы
</button>

<!-- Вставь это перед закрывающим тегом </nav> -->
<button onclick="showSiteQR()" 
        style="background:linear-gradient(135deg,#222,#333); 
               border:2px solid var(--neon); 
               color:var(--neon); 
               padding:12px 18px; 
               border-radius:12px; 
               cursor:pointer; 
               font-size:1.05rem; 
               display:flex; 
               align-items:center; 
               gap:8px;">
    📱 QR сайта
</button>

    <button onclick="showSection('login')" id="loginBtn">Вход 🔑</button>
    <button onclick="showSection('friends')">👥 Friends</button>
    <button onclick="logout()" id="logoutBtn" style="display:none;">Выход 🚪</button>

   <!-- Панель профиля (справа сверху) -->
<div class="profile-container" id="profileContainer">
  <img id="avatar" class="profile-avatar" 
       src="https://i.pravatar.cc/150?u=default" 
       onclick="toggleProfileDropdown()" 
       alt="Профиль">
</div>

<button onclick="toggleLanguagePanel()" id="langBtn" style="background:linear-gradient(135deg,#222,#333); border:2px solid var(--dim); color:var(--neon);">🌐 Язык</button>
<button onclick="toggleColorPanel()" id="colorBtn" style="background:linear-gradient(135deg,#222,#333); border:2px solid var(--dim); color:var(--neon);">🎨 Тема</button>
     </nav>

  <!-- Всё остальное — точно как в твоём коде -->
 <div id="home" class="section active">
  <h2 id="home-title">Добро пожаловать в VoteHub</h2>
  <p id="home-text" style="text-align:center; font-size:1.45rem; line-height:1.9; max-width:850px; margin:40px auto;">
    Современная платформа для создания и проведения онлайн-голосований.<br><br>
    Создавайте одиночные и мульти-опросы, голосуйте в реальном времени, анализируйте результаты.
  </p>
  <button class="main" onclick="showSection('create')" id="home-btn">Создать новый опрос →</button>
</div>

  <div id="login" class="section">
    <h2 id="login-title" style="margin-bottom:40px;">Авторизация пользователя 🔒</h2>
    <div style="max-width:520px; margin:0 auto; background:rgba(15,15,25,0.95); border:3px solid var(--dim); border-radius:24px; padding:40px 35px; box-shadow:0 0 60px rgba(255,234,128,0.3);">
      <div style="display:flex; background:#111; border-radius:16px; padding:6px; margin-bottom:35px;">
        <button onclick="switchTab(0)" id="tab-register" style="flex:1; padding:14px; border:none; border-radius:12px; font-weight:600; background:var(--neon); color:#000; transition:all 0.3s;">Регистрация</button>
        <button onclick="switchTab(1)" id="tab-login" style="flex:1; padding:14px; border:none; border-radius:12px; font-weight:600; background:transparent; color:var(--neon); transition:all 0.3s;">Вход</button>
      </div>
      <div id="register-form">
        <input id="regUsername" placeholder="Имя пользователя 👤" style="margin-bottom:18px;">
        <input id="regEmail" type="email" placeholder="Email (обязательно @gmail.com) ✉️" style="margin-bottom:25px;">
        <button class="main" onclick="registerNewUser()" style="width:100%; margin:10px 0 20px;">Зарегистрироваться ✅</button>
        <div id="regMessage" style="text-align:center; line-height:1.6; min-height:100px; font-size:1.05rem;"></div>
      </div>
      <div id="login-form" style="display:none;">
        <input id="loginEmail" type="email" placeholder="Ваш Gmail ✉️" style="margin-bottom:18px;">
        <input id="loginPassword" type="password" placeholder="Пароль 🔑" style="margin-bottom:25px;">
        <button class="main" onclick="loginUser()" style="width:100%; margin:10px 0 30px;">Войти в аккаунт 🌐</button>
        <p style="text-align:center; color:#aaa; font-size:1rem;">
          Нет аккаунта?
          <span onclick="switchTab(0)" style="color:var(--neon); cursor:pointer; text-decoration:underline;">Зарегистрируйтесь</span>
        </p>
      </div>
    </div>
  </div>

<!-- ======================== ЛИЧКА ======================== -->
<div id="friends" class="section" style="display:none;padding:20px;">

  <div style="
    background:linear-gradient(135deg,#151515,#222);
    border:2px solid var(--neon);
    border-radius:25px;
    padding:25px;
    margin-bottom:25px;
    text-align:center;
    box-shadow:0 0 25px rgba(0,255,170,.25);
  ">
    <img id="friends-avatar"
         src="https://i.pravatar.cc/150?u=default"
         onclick="enlargeAvatar(this.src)"
         style="
           width:130px;
           height:130px;
           border-radius:50%;
           border:5px solid var(--neon);
           cursor:pointer;
           box-shadow:0 0 20px var(--neon);
         ">

    <h2 id="friends-name">Гость</h2>
    <p id="friends-email" style="color:#aaa;"></p>

    <div style="margin-top:15px;">
      <span style="
        background:#00ff88;
        color:#000;
        padding:6px 16px;
        border-radius:30px;
        font-weight:bold;
      ">
        ● Online
      </span>
    </div>
  </div>

  <input
      type="text"
      placeholder="🔍 Поиск друзей..."
      style="
      width:100%;
      padding:15px;
      border-radius:15px;
      border:none;
      background:#1d1d1d;
      color:white;
      margin-bottom:20px;
      font-size:16px;
      ">

  <h3 style="margin-bottom:15px;">👥 Мои друзья</h3>

  <div id="friendsList"
       style="
       display:grid;
       grid-template-columns:repeat(auto-fill,minmax(300px,1fr));
       gap:15px;
       ">
  </div>

  <p id="noFriends"
     style="
     text-align:center;
     color:#888;
     margin-top:40px;
     font-size:18px;
     ">
     Пока нет друзей 😔
  </p>

</div>

  <div id="create" class="section">
    <h2 id="create-title">Создать новый опрос 🛠️</h2>
    <input id="pollTitle" placeholder="Название опроса 📛">
    <textarea id="pollDesc" placeholder="Описание опроса (необязательно) 📝" rows="4"></textarea>
    <h3 style="margin:30px 0 16px; color:var(--neon);" id="options-title">Варианты ответов ⚙️</h3>
    <div id="optionsContainer"></div>
    <button onclick="addOption()" style="background:#111; color:var(--neon); padding:14px 32px; border:2px solid var(--dim); border-radius:12px; margin:24px 0; font-size:1.2rem;">
      + Добавить вариант ➕
    </button>
    <button class="main" onclick="createPoll()" id="btn-create-poll">Создать опрос ▶️</button>
    <button class="save-btn" onclick="savePoll()" id="btn-save-poll">Сохранить опрос 💾</button>
    <div style="margin-top:50px; border-top: 3px dashed var(--dim); padding-top:30px;">
      <h3 style="color:var(--neon); text-align:center;" id="saved-for-edit">Сохранённые опросы для редактирования</h3>
      <div id="editPollsList" class="stats-cards"></div>
    </div>
  </div>

  <div id="vote" class="section">
    <h2 id="vote-title">Проведение голосования 🗳️</h2>
    <div id="pollArea">
      <div class="poll-question" id="currentTitle"></div>
      <div id="currentOptions"></div>
      <div style="margin:28px 0; text-align:center;">
        Количество голосов: <input type="number" id="voteCount" value="1000" min="1" max="10000" style="width:130px; text-align:center; font-size:1.3rem;">
      </div>
      <button class="main" onclick="submitVote()" id="btn-vote">Проголосовать ✅</button>
      <button class="ai-btn" onclick="aiVote()" id="btn-ai-vote">Автоматическое голосование 🤖</button>
      <div id="voteMessage" class="message hidden"></div>
    </div>
    <div id="noPoll" class="message">Опрос не выбран 😴</div>
  </div>

  <div id="results" class="section">
    <h2 id="results-title">Результаты голосования 📊</h2>
    <div id="resultsArea"></div>
    <div id="noResults" class="message">Результатов пока нет 🚫</div>
    <button class="main" onclick="resetPoll()" id="btn-reset">Сбросить опрос 🔄</button>
  </div>

  <div id="saved" class="section">
    <h2 id="saved-title">Сохранённые опросы 💾</h2>
    <div id="savedPollsList" class="stats-cards"></div>
    <div id="noSaved" class="message">Список пуст 📭</div>
  </div>

  <div id="stats" class="section">
    <h2 id="stats-title">Статистика 📈</h2>
    <button onclick="loadCreatorStats()" style="margin:0 auto 35px;display:block;padding:16px 48px;font-size:1.25rem;background:var(--neon);color:#000;border:none;border-radius:50px;cursor:pointer;box-shadow:0 0 60px rgba(255,234,128,0.9);">
      Обновить статистику 🔄
    </button>
    <div class="stats-cards">
      <div class="stats-card">
        <p><strong id="stat-user-label">Пользователь:</strong> <span id="statUser">—</span></p>
        <p><strong id="stat-reg-label">Дата регистрации:</strong> <span id="statRegDate">—</span></p>
      </div>
      <div class="stats-card">
        <p><strong id="stat-created-label">Создано опросов:</strong> <span id="statCreated">0</span></p>
        <p><strong id="stat-total-label">Всего голосов:</strong> <span id="statTotalVotes">0</span></p>
        <p><strong id="stat-avg-label">Среднее на опрос:</strong> <span id="statAvgVotes">0</span></p>
      </div>
    </div>
    <h3 id="top-poll-title" style="color:var(--neon); text-align:center; margin:50px 0 25px;">🏆 Самый популярный опрос (Топ-1)</h3>
    <div id="topPoll" class="stats-card top1-card" style="text-align:center;"></div>
    <h3 id="top-options-title" style="color:var(--neon); text-align:center; margin:50px 0 25px;">🔥 Топ-3 самых выбираемых вариантов</h3>
    <div id="topOptions" class="stats-cards"></div>
    <h3 id="all-polls-title" style="color:var(--neon); text-align:center; margin:50px 0 25px;">Все ваши опросы</h3>
    <div id="pollStatsList" class="stats-cards"></div>
    <div id="noStatsMessage" class="message" style="display:none;">Создайте хотя бы один опрос для отображения статистики 🚀</div>
  </div>

<!-- ======================== ОНЛАЙН СТАТИСТИКА ======================== -->
<div id="online-stats" class="section" style="display: none;">
  <h1>🌐 Онлайн-статистика</h1>
  <p style="color: var(--dim);">Статистика только твоих публичных опросов</p>

  <div class="stats-grid">
    <div class="stats-card">
      <h3>Публичных опросов</h3>
      <p id="online-stat-created" style="font-size: 2.8rem; color: var(--neon);">0</p>
    </div>
    <div class="stats-card">
      <h3>Всего голосов</h3>
      <p id="online-stat-total" style="font-size: 2.8rem; color: var(--neon);">0</p>
    </div>
    <div class="stats-card">
      <h3>Среднее на опрос</h3>
      <p id="online-stat-avg" style="font-size: 2.8rem; color: var(--neon);">0</p>
    </div>
  </div>

  <h2 style="margin-top: 40px;">🏆 Самый популярный публичный опрос</h2>
  <div id="online-top-poll" class="stats-card"></div>

  <h2 style="margin-top: 40px;">🔥 Топ-3 вариантов</h2>
  <div id="online-top-options" class="stats-grid"></div>

  <h2 style="margin-top: 40px;">Все твои публичные опросы</h2>
  <div id="online-poll-list" class="stats-grid"></div>

  <div id="noOnlineStats" class="message" style="display: none; text-align: center; padding: 40px;">
    У тебя пока нет публичных опросов 🌍<br>
    <small>Создавай публичные опросы, чтобы видеть здесь статистику</small>
  </div>
</div>

  <div id="online" class="section">
    <h2 id="online-title">🌐 Онлайн опросы — режим для всех</h2>
    <p id="online-description" style="text-align:center; font-size:1.35rem; max-width:900px; margin:0 auto 40px; line-height:1.7;">
      Здесь любой человек, который зайдёт на сайт, видит одни и те же опросы.<br>
      Создавайте опросы — они сразу становятся публичными.<br>
      Голосование происходит в реальном времени.
    </p>
    <div style="background:#111; padding:35px; border-radius:20px; margin-bottom:60px; border:3px solid var(--neon);">
      <h3 style="color:var(--neon); text-align:center; margin-bottom:25px;" id="create-public-title">Создать новый публичный опрос</h3>
      <input id="publicPollTitle" placeholder="Название публичного опроса 📛">
      <textarea id="publicPollDesc" placeholder="Описание (необязательно)" rows="3"></textarea>
      <h4 style="margin:25px 0 10px; color:var(--dim);" id="public-options-title">Варианты ответов</h4>
      <div id="publicOptionsContainer"></div>
      <button onclick="addPublicOption()" style="background:#222; color:var(--neon); padding:14px 32px; border:2px solid var(--dim); border-radius:12px; margin:20px 0; font-size:1.2rem;">
        + Добавить вариант ➕
      </button>
      <h4 style="margin:25px 0 10px; color:var(--dim);" id="timer-title">⏳ Таймер (срок действия опроса)</h4>
      <select id="publicDuration" style="width:100%; padding:16px; background:#222; color:#fff; border:2px solid var(--dim); border-radius:12px; font-size:1.2rem;">
        <option value="300000">5 минут</option>
        <option value="1800000">30 минут</option>
        <option value="3600000" selected>1 час</option>
        <option value="10800000">3 часа</option>
        <option value="86400000">24 часа</option>
        <option value="259200000">3 дня</option>
      </select>
      <button class="main" onclick="createPublicPoll()" id="publish-btn">Опубликовать в онлайн-режиме 🚀</button>
    </div>
    <h3 style="color:var(--neon); text-align:center; margin-bottom:30px;" id="active-polls-title">📋 Все активные онлайн-опросы</h3>
    <div id="publicPollsList" class="stats-cards public-poll-list"></div>
    <div id="noPublicPolls" class="message hidden">Пока нет публичных опросов. Создайте первый! 🌍</div>
    <div id="publicVoteArea" style="display:none; margin-top:60px;">
      <div class="poll-question" id="publicCurrentTitle"></div>
      <div id="publicCurrentOptions"></div>
      <div class="online-timer" id="publicTimer">⏳ До окончания: <span id="timerValue"></span></div>
      <button class="main" onclick="submitPublicVote()" id="btn-public-vote">Проголосовать ✅</button>
      <button onclick="hidePublicVoteArea()" style="margin:20px auto; display:block; background:#333; color:#fff; padding:14px 40px; border-radius:50px; border:none; font-size:1.1rem;">Вернуться к списку</button>
      <div id="publicResults" style="margin-top:40px;"></div>
      <div id="publicVoteMessage" class="message hidden"></div>
    </div>
  </div>

  <footer id="footer-text">VoteHub — Система онлайн-голосований • 2026 ⚙️</footer>
</div>

<!-- ======================== МУЛЬТИ-ГОЛОСОВАНИЕ ======================== -->
<div id="multi" class="section">
  <h2 id="multi-title">🔀 Мульти-голосование</h2>
  <p style="text-align:center; font-size:1.35rem; max-width:900px; margin:0 auto 40px; line-height:1.7; color:var(--dim);">
    Создавайте пакет из нескольких опросов. Участники голосуют сразу во всех опросах пакета.
  </p>

  <div style="background:#111; padding:35px; border-radius:20px; margin-bottom:60px; border:3px solid var(--neon);">
    <h3 style="color:var(--neon); text-align:center; margin-bottom:25px;">Создать новый мульти-опрос</h3>
    
    <input id="multiTitle" placeholder="Название пакета опросов 📛" style="margin-bottom:15px;">
    <textarea id="multiDesc" placeholder="Описание пакета (необязательно)" rows="3" style="margin-bottom:25px;"></textarea>

    <h4 style="color:var(--dim); margin:20px 0 10px;">Опросы в пакете</h4>
    <div id="multiPollsContainer" style="margin-bottom:20px;"></div>
    
    <button onclick="addMultiPoll()" style="background:#222; color:var(--neon); padding:14px 32px; border:2px solid var(--dim); border-radius:12px; margin:15px 0; font-size:1.1rem;">
      + Добавить опрос в пакет
    </button>

    <h4 style="color:var(--dim); margin:25px 0 10px;">⏳ Время действия пакета</h4>
    <select id="multiDuration" style="width:100%; padding:16px; background:#222; color:#fff; border:2px solid var(--dim); border-radius:12px; font-size:1.2rem;">
      <option value="1800000">30 минут</option>
      <option value="3600000" selected>1 час</option>
      <option value="10800000">3 часа</option>
      <option value="86400000">24 часа</option>
      <option value="259200000">3 дня</option>
    </select>

    <button class="main" onclick="createMultiPoll()" style="margin-top:30px;">Опубликовать мульти-опрос 🚀</button>
  </div>

  <h3 style="color:var(--neon); text-align:center; margin:40px 0 25px;">📋 Активные мульти-опросы</h3>
  <div id="multiPollsList" class="stats-cards"></div>
  <div id="noMultiPolls" class="message hidden">Пока нет мульти-опросов. Создайте первый! 🔀</div>
</div>

<!-- === СОЗДАНИЕ ГЛОБАЛЬНОГО ОПРОСА (ОПТИМИЗИРОВАНО) === -->
<div id="create-global" class="section">
  <h2 style="color:var(--yellow);">Создать Глобальный Опрос 🔥</h2>
  <div style="background:rgba(20,20,30,0.95); padding:40px; border-radius:24px; border:3px solid var(--yellow); max-width:900px; margin:0 auto;">
    <input id="globalPollTitle" placeholder="Название опроса" style="font-size:1.3rem;">
    <textarea id="globalPollDesc" placeholder="Описание опроса (необязательно)" rows="4"></textarea>
    
    <h3 style="color:var(--neon); margin:25px 0 15px;">Варианты ответов</h3>
    <div id="globalOptionsContainer"></div>
    
    <button onclick="addGlobalOption()" style="background:#222; color:var(--neon); padding:14px 32px; border:2px solid var(--dim); border-radius:12px; margin:20px 0;">
      + Добавить вариант
    </button>

    <h4 style="margin:25px 0 10px; color:var(--dim);">⏳ Срок действия опроса</h4>
    <select id="globalDuration" style="width:100%; padding:16px; background:#222; color:#fff; border:2px solid var(--dim); border-radius:12px; font-size:1.2rem; margin-bottom:25px;">
      <option value="86400000">1 день</option>
      <option value="259200000" selected>3 дня</option>
      <option value="604800000">7 дней</option>
      <option value="1296000000">15 дней</option>
      <option value="2592000000">30 дней</option>
    </select>
    
    <button onclick="publishGlobalPoll()" class="main" style="width:100%; background:linear-gradient(135deg,#00ff88,#00ccff);">
      🚀 Опубликовать в Опросы
    </button>
  </div>
</div>

<!-- ======================== РАЗДЕЛ ОПРОСЫ ======================== -->
<div id="polls" class="section" style="display: none; padding: 30px 20px;">
  <h2 style="text-align:center; margin-bottom:40px; font-size:2.8rem; color:var(--yellow); text-shadow:0 0 40px var(--neon);">
    🧠 Постоянные опросы от сообщества • Создавай и голосуй вместе со всеми
  </h2>
  <p style="text-align:center; color:#aaa; max-width:800px; margin:0 auto 50px; font-size:1.2rem;">
    Голосуй за самые интересные вопросы о мышлении, лидерстве, ИИ и будущем
  </p>

  <div id="pollsContainer" class="stats-cards" style="max-width:1400px; margin:0 auto;"></div>
</div>

<!-- QR-код сайта -->
<div id="siteQRModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.96); z-index:999999; align-items:center; justify-content:center;">
  <div style="background:#111; padding:35px 40px; border-radius:24px; border:4px solid var(--neon); text-align:center; max-width:420px; box-shadow:0 0 80px rgba(255,234,128,0.7);">
    
    <h2 style="color:var(--neon); margin-bottom:20px; text-shadow:0 0 20px var(--yellow);">📱 Сканируй QR-код</h2>
    <p style="color:#aaa; margin-bottom:25px;">Чтобы быстро попасть на VoteHub</p>
    
    <div id="siteQRCode" style="margin:20px auto; padding:20px; background:white; border-radius:16px; display:inline-block;"></div>
    
    <div style="margin:20px 0; color:var(--yellow); font-family:monospace; word-break:break-all; font-size:0.95rem;">
      {{current-url}}
    </div>
    
    <button onclick="closeSiteQR()" 
            class="main" 
            style="padding:14px 50px; font-size:1.2rem; margin-top:10px;">
      Закрыть
    </button>
  </div>
</div>

<!-- Выпадающая карточка профиля (с увеличением аватарки) -->
<div class="profile-dropdown" id="profileDropdown">
  <div class="profile-header">
    <img id="dropdown-avatar" 
         src="https://i.pravatar.cc/150?u=default" 
         alt="Аватар"
         onclick="enlargeAvatar(this.src)"
         style="cursor: zoom-in;">
    <div class="profile-info">
      <h3 id="profile-name">Пользователь</h3>
      <p id="profile-email"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d7b2bab6bebb97b0bab6bebbf9b4b8ba">[email&#160;protected]</a></p>
    </div>
  </div>
  <button onclick="editUsername()" id="profile-edit-name">✏️ Изменить имя</button>
  <div style="display: flex; gap: 8px; flex-direction: column;">
  <button onclick="changeAvatar()" style="width:100%;">🔗 По ссылке (URL)</button>
  <button onclick="uploadAvatar()" style="width:100%; background: #00cc66; color: white;">📁 Загрузить с компьютера</button>
</div>
  <button class="logout-btn" onclick="logout()" id="profile-logout">🚪 Выход из аккаунта</button>
</div>

<div id="languagePanel">
  <button class="lang-btn" onclick="changeLanguage('ru')">🇷🇺 Русский</button>
  <button class="lang-btn" onclick="changeLanguage('en')">🇬🇧 English</button>
  <button class="lang-btn" onclick="changeLanguage('kk')">🇰🇿 Қазақша</button>
  <button onclick="toggleLanguagePanel()" style="margin-top:15px; width:100%; padding:14px; background:#222; color:var(--neon); border:2px solid var(--dim); border-radius:12px;">Закрыть</button>
</div>
<div id="colorPanel">
  <div style="display:flex; gap:18px; margin-bottom:18px; flex-wrap: wrap;">
    <div class="color-option" style="background:#ffea80" onclick="changeTheme('#ffea80')" title="Жёлтый"></div>
    <div class="color-option" style="background:#ff00ff" onclick="changeTheme('#ff00ff')" title="Розовый"></div>
    <div class="color-option" style="background:#00ffff" onclick="changeTheme('#00ffff')" title="Голубой"></div>
    <div class="color-option" style="background:#00ff88" onclick="changeTheme('#00ff88')" title="Зелёный"></div>
    <div class="color-option" style="background:#888888; border:3px solid #bbbbbb" onclick="changeTheme('#888888')" title="Серый"></div>
    <!-- Новый белый цвет -->
    <div class="color-option" style="background:#f0f0f0; border:3px solid #555555" onclick="changeTheme('#f0f0f0')" title="Белая подсветка"></div>
  </div>
<div class="color-option" 
     style="background: linear-gradient(135deg, #ff2200, #ff8800, #ffff00); width: 60px; height: 60px; border: 3px solid #ffff66;" 
     onclick="activateFireTheme()" 
     title="Огонь 🔥">
</div>
  <button onclick="toggleColorPanel()" style="width:100%; padding:12px; background:#222; color:var(--neon); border:2px solid var(--dim); border-radius:12px;">Закрыть</button>
</div>
<script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script><script>
// ======================== ТВОЙ ПОЛНЫЙ СКРИПТ ========================
const translations = {
  ru: {
   "header-title": "VoteHub",
    "header-subtitle": "Современная система онлайн-голосований и опросов<br>Создавай • Голосуй • Анализируй",
    "nav-home": "Главная 🏠",
    "nav-create": "Создать опрос ✍️",
    "nav-vote": "Голосовать 🗳️",
    "nav-results": "Результаты 📊",
    "nav-saved": "Сохранённые 💾",
    "nav-stats": "Статистика 📈",
    "nav-online": "🌐 Онлайн опросы",
    "friends": "👥 Личка",                    // ← Добавлено
    "home-title": "Добро пожаловать в VoteHub",
    "home-text": "Современная локальная система для создания и проведения онлайн-голосований.<br><br>Создавайте опросы, проводите голосование, анализируйте результаты в реальном времени.",
    "home-btn": "Создать новый опрос →",
    "login-title": "Авторизация пользователя 🔒",
    "tab-register": "Регистрация",
    "tab-login": "Вход",
    "profile-name": "Пользователь",
    "profile-email": "email@gmail.com",
    "profile-edit-name": "✏️ Изменить имя",
    "profile-logout": "🚪 Выход из аккаунта",
    "footer-text": "TD & TP — Система онлайн-голосования • 2026 ⚙️",

    "create-title": "Создать новый опрос 🛠️",
    "options-title": "Варианты ответов ⚙️",
    "saved-for-edit": "Сохранённые опросы для редактирования",
    "pollTitle-placeholder": "Название опроса 📛",
    "pollDesc-placeholder": "Описание опроса (необязательно) 📝",
    "btn-create-poll": "Создать опрос ▶️",
    "btn-save-poll": "Сохранить опрос 💾",

    "vote-title": "Проведение голосования 🗳️",
    "btn-vote": "Проголосовать ✅",
    "btn-ai-vote": "Автоматическое голосование 🤖",
    "noPoll": "Опрос не выбран 😴",

    "results-title": "Результаты голосования 📊",
    "btn-reset": "Сбросить опрос 🔄",
    "noResults": "Результатов пока нет 🚫",

    "saved-title": "Сохранённые опросы 💾",
    "noSaved": "Список пуст 📭",

    "stats-title": "Статистика 📈",
    "stat-user-label": "Пользователь:",
    "stat-reg-label": "Дата регистрации:",
    "stat-created-label": "Создано опросов:",
    "stat-total-label": "Всего голосов:",
    "stat-avg-label": "Среднее на опрос:",
    "top-poll-title": "🏆 Самый популярный опрос (Топ-1)",
    "top-options-title": "🔥 Топ-3 самых выбираемых вариантов",
    "all-polls-title": "Все ваши опросы",
    "noStatsMessage": "Создайте хотя бы один опрос для отображения статистики 🚀",

    // Онлайн
    "online-title": "🌐 Онлайн опросы — режим для всех",
    "online-description": "Здесь любой человек, который зайдёт на сайт, видит одни и те же опросы.<br>Создавайте опросы — они сразу становятся публичными.<br>Голосование происходит в реальном времени.",
    "create-public-title": "Создать новый публичный опрос",
    "public-options-title": "Варианты ответов",
    "timer-title": "⏳ Таймер (срок действия опроса)",
    "active-polls-title": "📋 Все активные онлайн-опросы",
    "publish-btn": "Опубликовать в онлайн-режиме 🚀",
    "btn-public-vote": "Проголосовать ✅",
    "noPublicPolls": "Пока нет публичных опросов. Создайте первый! 🌍",

    // Онлайн-статистика
    "online-stat-created": "Публичных опросов",
    "online-stat-total": "Всего голосов",
    "online-stat-avg": "Среднее на опрос",

    // Личка
    "friends-name": "Гость",
    "noFriends": "Пока нет друзей 😔<br>Нажимай ❤️ на людях в онлайн-опросах",

    // Кнопки языка и темы
    "lang-btn": "🌐 Язык",
    "color-btn": "🎨 Тема",

"multi-title": "🔀 Мульти-голосование",
    "multi-description": "Создавайте пакет из нескольких опросов. Участники голосуют сразу во всех опросах пакета.",
    "multi-create-title": "Создать новый мульти-опрос",
    "multi-title-placeholder": "Название пакета опросов 📛",
    "multi-desc-placeholder": "Описание пакета (необязательно)",
    "multi-polls-in-package": "Опросы в пакете",
    "multi-add-poll": "+ Добавить опрос в пакет",
    "multi-duration": "⏳ Время действия пакета",
    "multi-publish-btn": "Опубликовать мульти-опрос 🚀",

    "multi-active-title": "📋 Активные мульти-опросы",
    "multi-no-polls": "Пока нет мульти-опросов. Создайте первый! 🔀",

    "multi-vote-title": "Голосование в пакете",
    "multi-submit-votes": "✅ Отправить все голоса",
    "multi-view-results": "📊 Посмотреть результаты",
    "multi-already-voted": "Вы уже проголосовали в этом пакете",
    "multi-need-all": "Нужно проголосовать во всех опросах пакета!",

    "multi-close": "Закрыть"
  },

en: {
    "header-title": "VoteHub",
    "header-subtitle": "Modern Online Voting and Polling System<br>Create • Vote • Analyze",    
    "nav-home": "Home 🏠",
    "nav-create": "Create Poll ✍️",
    "nav-vote": "Vote 🗳️",
    "nav-results": "Results 📊",
    "nav-saved": "Saved 💾",
    "nav-stats": "Statistics 📈",
    "nav-online": "🌐 Online Polls",
    "friends": "👥 Friends",                    // ← Личка
    
    "lang-btn": "🌐 Language",                  // ← Кнопка Язык
    "color-btn": "🎨 Theme",                    // ← Кнопка Тема

   "home-title": "Welcome to VoteHub",
    "home-text": "Modern local system for creating and conducting online voting.<br><br>Create polls, vote and analyze results in real time.",
    "home-btn": "Create New Poll →",

    "login-title": "User Authorization 🔒",
    "tab-register": "Registration",
    "tab-login": "Login",

    "create-title": "Create New Poll 🛠️",
    "options-title": "Answer Options ⚙️",
    "saved-for-edit": "Saved Polls for Editing",
    "pollTitle-placeholder": "Poll Title 📛",
    "pollDesc-placeholder": "Poll Description (optional) 📝",

    "vote-title": "Voting 🗳️",
    "btn-vote": "Vote ✅",
    "btn-ai-vote": "Auto Voting 🤖",
    "noPoll": "No poll selected 😴",

    "results-title": "Voting Results 📊",
    "btn-reset": "Reset Poll 🔄",
    "noResults": "No results yet 🚫",

    "saved-title": "Saved Polls 💾",
    "noSaved": "List is empty 📭",

    "stats-title": "Statistics 📈",
    "stat-user-label": "User:",
    "stat-reg-label": "Registration Date:",
    "stat-created-label": "Polls Created:",
    "stat-total-label": "Total Votes:",
    "stat-avg-label": "Average per Poll:",
    "top-poll-title": "🏆 Most Popular Poll (Top-1)",
    "top-options-title": "🔥 Top-3 Most Chosen Options",
    "all-polls-title": "All Your Polls",
    "noStatsMessage": "Create at least one poll to see statistics 🚀",

    "online-title": "🌐 Online Polls — Public Mode",
    "online-description": "Anyone who visits the site sees the same polls.<br>Create polls — they become public immediately.<br>Voting happens in real time.",
    "create-public-title": "Create New Public Poll",
    "public-options-title": "Answer Options",
    "timer-title": "⏳ Timer (poll duration)",
    "active-polls-title": "📋 All Active Online Polls",
    "publish-btn": "Publish to Online Mode 🚀",
    "btn-public-vote": "Vote ✅",
    "noPublicPolls": "No public polls yet. Create the first one! 🌍",

    "online-stat-created": "Public Polls",
    "online-stat-total": "Total Votes",
    "online-stat-avg": "Average per Poll",

    // Личка
    "friends-name": "Guest",
    "noFriends": "No friends yet 😔<br>Click ❤️ on people in online polls",

    "profile-name": "User",
    "profile-email": "email@gmail.com",
    "profile-edit-name": "✏️ Edit Name",
    "profile-logout": "🚪 Logout",
    "footer-text": "TD & TP — Online Voting System • 2026 ⚙️",

"multi-title": "🔀 Multi Voting",
    "multi-description": "Create a package of several polls. Participants vote in all polls at once.",
    "multi-create-title": "Create New Multi Poll",
    "multi-title-placeholder": "Package Name 📛",
    "multi-desc-placeholder": "Package Description (optional)",
    "multi-polls-in-package": "Polls in Package",
    "multi-add-poll": "+ Add Poll to Package",
    "multi-duration": "⏳ Package Duration",
    "multi-publish-btn": "Publish Multi Poll 🚀",

    "multi-active-title": "📋 Active Multi Polls",
    "multi-no-polls": "No multi polls yet. Create the first one! 🔀",

    "multi-vote-title": "Voting in Package",
    "multi-submit-votes": "✅ Submit All Votes",
    "multi-view-results": "📊 View Results",
    "multi-already-voted": "You have already voted in this package",
    "multi-need-all": "You need to vote in all polls in the package!",

    "multi-close": "Close"
  },
  
  kk: {
   "header-title": "VoteHub",
    "header-subtitle": "Онлайн дауыс беру мен сауалнама жүйесі<br>Құру • Дауыс беру • Талдау",
    "nav-home": "Басты бет 🏠",
    "nav-create": "Сауалнама құру ✍️",
    "nav-vote": "Дауыс беру 🗳️",
    "nav-results": "Нәтижелер 📊",
    "nav-saved": "Сақталғандар 💾",
    "nav-stats": "Статистика 📈",
    "nav-online": "🌐 Онлайн сауалнамалар",
    "friends": "👥 Достар",
   "home-title": "VoteHub-қа қош келдіңіз",
    "home-text": "Онлайн дауыс беру мен сауалнамаларды құруға арналған заманауи жүйе.<br><br>Сауалнамалар құрыңыз, дауыс беріңіз және нәтижелерді талдаңыз.",
    "home-btn": "Жаңа сауалнама құру →",
    "login-title": "Пайдаланушы кіруі 🔒",
    "tab-register": "Тіркелу",
    "tab-login": "Кіру",
    "profile-name": "Пайдаланушы",
    "profile-email": "email@gmail.com",
    "profile-edit-name": "✏️ Атын өзгерту",
    "profile-logout": "🚪 Шығу",
    "footer-text": "TD & TP — Онлайн дауыс беру жүйесі • 2026 ⚙️",

    "create-title": "Жаңа сауалнама құру 🛠️",
    "options-title": "Жауап нұсқалары ⚙️",
    "saved-for-edit": "Редакциялау үшін сақталған сауалнамалар",
    "pollTitle-placeholder": "Сауалнама атауы 📛",
    "pollDesc-placeholder": "Сипаттама (міндетті емес) 📝",
    "btn-create-poll": "Сауалнама құру ▶️",
    "btn-save-poll": "Сақтау 💾",

    "vote-title": "Дауыс беру 🗳️",
    "btn-vote": "Дауыс беру ✅",
    "btn-ai-vote": "Автоматты дауыс беру 🤖",
    "noPoll": "Сауалнама таңдалмаған 😴",

    "results-title": "Дауыс беру нәтижелері 📊",
    "btn-reset": "Нәтижелерді тазарту 🔄",
    "noResults": "Нәтижелер әлі жоқ 🚫",

    "saved-title": "Сақталған сауалнамалар 💾",
    "noSaved": "Тізім бос 📭",

    "stats-title": "Статистика 📈",
    "stat-user-label": "Пайдаланушы:",
    "stat-reg-label": "Тіркелу күні:",
    "stat-created-label": "Құрылған сауалнамалар:",
    "stat-total-label": "Жалпы дауыстар:",
    "stat-avg-label": "Орташа бір сауалнамаға:",
    "top-poll-title": "🏆 Ең танымал сауалнама (Топ-1)",
    "top-options-title": "🔥 Ең көп таңдалған 3 нұсқа",
    "all-polls-title": "Сіздің барлық сауалнамаларыңыз",
    "noStatsMessage": "Статистиканы көру үшін кемінде бір сауалнама құрыңыз 🚀",

    "online-title": "🌐 Онлайн сауалнамалар — барлығы үшін",
    "online-description": "Сайтқа кірген кез келген адам бірдей сауалнамаларды көреді.<br>Сауалнамалар құрыңыз — олар бірден жалпыға ортақ болады.<br>Дауыс беру нақты уақытта жүреді.",
    "create-public-title": "Жаңа жалпыға ортақ сауалнама құру",
    "public-options-title": "Жауап нұсқалары",
    "timer-title": "⏳ Таймер (сауалнаманың мерзімі)",
    "active-polls-title": "📋 Барлық белсенді онлайн сауалнамалар",
    "publish-btn": "Онлайн режимге жариялау 🚀",
    "btn-public-vote": "Дауыс беру ✅",
    "noPublicPolls": "Әлі жалпы сауалнамалар жоқ. Біріншісін құрыңыз! 🌍",

    "friends-name": "Қонақ",
    "noFriends": "Әзірге достар жоқ 😔<br>Онлайн сауалнамалардағы адамдарға ❤️ басыңыз",

    "lang-btn": "🌐 Тіл",
    "color-btn": "🎨 Тақырып",

"multi-title": "🔀 Мульти-дауыс беру",
    "multi-description": "Бірнеше сауалнамалар пакетін құрыңыз. Қатысушылар барлық сауалнамаларға бірден дауыс береді.",
    "multi-create-title": "Жаңа мульти-сауалнама құру",
    "multi-title-placeholder": "Пакет атауы 📛",
    "multi-desc-placeholder": "Пакет сипаттамасы (міндетті емес)",
    "multi-polls-in-package": "Пакеттегі сауалнамалар",
    "multi-add-poll": "+ Пакетке сауалнама қосу",
    "multi-duration": "⏳ Пакеттің мерзімі",
    "multi-publish-btn": "Мульти-сауалнаманы жариялау 🚀",

    "multi-active-title": "📋 Белсенді мульти-сауалнамалар",
    "multi-no-polls": "Әзірге мульти-сауалнамалар жоқ. Біріншісін құрыңыз! 🔀",

    "multi-vote-title": "Пакетте дауыс беру",
    "multi-submit-votes": "✅ Барлық дауыстарды жіберу",
    "multi-view-results": "📊 Нәтижелерді көру",
    "multi-already-voted": "Сіз бұл пакетте уже дауыс бергенсіз",
    "multi-need-all": "Пакеттегі барлық сауалнамаларға дауыс беру керек!",

    "multi-close": "Жабу"
  }
};

let currentLang = 'ru';

function changeLanguage(lang) {
  currentLang = lang;
  localStorage.setItem('language', lang);

  document.querySelectorAll('[id]').forEach(el => {
    const key = el.id;
    if (translations[lang] && translations[lang][key]) {
      if (el.tagName === "INPUT" && el.placeholder !== undefined) {
        el.placeholder = translations[lang][key];
      } else if (el.tagName === "BUTTON" || el.tagName === "H1" || el.tagName === "H2" || el.tagName === "H3" || el.tagName === "P") {
        // Пропускаем имя и email в профиле
        if (el.id === "profile-name" || el.id === "profile-email") return;
        el.innerHTML = translations[lang][key];
      }
    }
  });

  // Специально для кнопок Язык и Тема
  const langBtn = document.getElementById('langBtn');
  const colorBtn = document.getElementById('colorBtn');
  if (langBtn) langBtn.innerHTML = translations[lang].langBtn || '🌐 Language';
  if (colorBtn) colorBtn.innerHTML = translations[lang].colorBtn || '🎨 Theme';

  toggleLanguagePanel();
}

// ======================== СИСТЕМА МОДЕРАЦИИ (АНТИ-МАТ) ========================
const profanityList = [
  "сука", "блять", "бля", "пизда", "хуй", "уебище", "чмо", "нахуй", "сучка", 
  "гандон", "ахуел", "ахуевший", "писька", "еблан", "хуярь", "пиздабол", 
  "вагина", "хуесос", "ебать", "ебнулся", "пидор", "пидорас", "мудак", 
  "мудила", "залупа", "ебало", "ебальник", "хуесосина", "блядь", "ебаный",
  "наеб", "поебал", "отъебись", "суки", "бляди", "пиздец", "хуйня", "херня",
  // Добавь ещё слова если нужно
];

function containsProfanity(text) {
  if (!text) return false;
  const lowerText = text.toLowerCase().replace(/[^а-яa-z0-9]/g, '');
  
  for (let word of profanityList) {
    if (lowerText.includes(word.toLowerCase())) {
      return true;
    }
  }
  return false;
}

function checkPollForProfanity(title, desc, options) {
  if (containsProfanity(title)) {
    alert('❌ Опрос отклонён!\n\nВ названии обнаружены нецензурные выражения.');
    return false;
  }
  if (desc && containsProfanity(desc)) {
    alert('❌ Опрос отклонён!\n\nВ описании обнаружены нецензурные выражения.');
    return false;
  }
  for (let opt of options) {
    if (containsProfanity(opt)) {
      alert('❌ Опрос отклонён!\n\nВ вариантах ответов обнаружены нецензурные выражения.');
      return false;
    }
  }
  return true;
}

function toggleLanguagePanel() {
  const panel = document.getElementById('languagePanel');
  panel.style.display = (panel.style.display === 'flex') ? 'none' : 'flex';
}

function changeTheme(color) {
  // Сбрасываем fire-тему
  document.body.removeAttribute('data-theme');
  
  document.documentElement.style.setProperty('--neon', color);
  document.documentElement.style.setProperty('--yellow', color);
  
  if (color === '#888888') {                    
    document.documentElement.style.setProperty('--dim', '#aaaaaa');
  } 
  else if (color === '#f0f0f0') {               
    document.documentElement.style.setProperty('--dim', '#555555');
    document.body.style.backgroundColor = '#f8f8f8';
    document.documentElement.style.setProperty('--text', '#222222');
  } 
  else {                                        
    document.documentElement.style.setProperty('--dim', color + '88');
    document.body.style.backgroundColor = '';
    document.documentElement.style.setProperty('--text', '#e0e0e0');
  }
  
  localStorage.setItem('themeColor', color);
  localStorage.removeItem('theme'); // убираем fire
  toggleColorPanel();
}

function activateFireTheme() {
  document.body.setAttribute('data-theme', 'fire');
  localStorage.setItem('theme', 'fire');
  localStorage.removeItem('themeColor'); // чистим обычную тему
  toggleColorPanel();
}

function toggleColorPanel() {
  const panel = document.getElementById('colorPanel');
  panel.style.display = (panel.style.display === 'flex') ? 'none' : 'flex';
}

function loadSavedTheme() {
  const saved = localStorage.getItem('themeColor');
  if (saved) {
    document.documentElement.style.setProperty('--neon', saved);
    document.documentElement.style.setProperty('--yellow', saved);
    document.documentElement.style.setProperty('--dim', saved + '88');
  }
}

function switchTab(tab) {
  const regForm = document.getElementById('register-form');
  const loginForm = document.getElementById('login-form');
  const tabReg = document.getElementById('tab-register');
  const tabLogin = document.getElementById('tab-login');
  if (tab === 0) {
    regForm.style.display = 'block';
    loginForm.style.display = 'none';
    tabReg.style.background = 'var(--neon)';
    tabReg.style.color = '#000';
    tabLogin.style.background = 'transparent';
    tabLogin.style.color = 'var(--neon)';
  } else {
    regForm.style.display = 'none';
    loginForm.style.display = 'block';
    tabReg.style.background = 'transparent';
    tabReg.style.color = 'var(--neon)';
    tabLogin.style.background = 'var(--neon)';
    tabLogin.style.color = '#000';
  }
}
function generateRandomPassword(length = 14) {
  const chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+-=";
  let password = "";
  for (let i = 0; i < length; i++) {
    password += chars.charAt(Math.floor(Math.random() * chars.length));
  }
  return password;
}
function isValidGmail(email) {
  const regex = /^[a-zA-Z0-9._%+-]+@gmail\.com$/i;
  return regex.test(email);
}
function registerNewUser() {
  const username = document.getElementById('regUsername').value.trim();
  const email = document.getElementById('regEmail').value.trim().toLowerCase();
  if (!username) return alert('Введите имя пользователя!');
  if (!email) return alert('Введите email!');
  if (!isValidGmail(email)) return alert('Email должен заканчиваться только на @gmail.com');
  let users = JSON.parse(localStorage.getItem('users') || '[]');
  if (users.some(u => u.email === email)) {
    return alert('Этот Gmail уже зарегистрирован!');
  }
  const password = generateRandomPassword(14);
  const newUser = { username, email, password, regDate: new Date().toLocaleString('ru-RU') };
  users.push(newUser);
  localStorage.setItem('users', JSON.stringify(users));
  const msg = document.getElementById('regMessage');
  msg.innerHTML = `
    <span style="color:#0f0; font-size:1.4rem;">✅ Регистрация успешна!</span><br><br>
    <strong>Email:</strong> ${email}<br>
    <strong>Пароль:</strong>
    <span id="generatedPass" style="background:#222; padding:6px 12px; border-radius:8px; font-family:monospace; cursor:pointer; user-select:all;" onclick="copyPassword(this)">${password}</span>
    <br><br>
    <small style="color:#aaa;">Нажмите на пароль чтобы скопировать</small>
  `;
  document.getElementById('loginEmail').value = email;
  document.getElementById('loginPassword').value = password;
}
function copyPassword(element) {
  const text = element.textContent;
  navigator.clipboard.writeText(text).then(() => {
    const original = element.textContent;
    element.textContent = '✓ Скопировано!';
    element.style.color = '#0f0';
    setTimeout(() => {
      element.textContent = original;
      element.style.color = '';
    }, 1800);
  });
}
function loginUser() {
  const email = document.getElementById('loginEmail').value.trim().toLowerCase();
  const password = document.getElementById('loginPassword').value.trim();
  if (!email || !password) return alert('Введите email и пароль!');
  let users = JSON.parse(localStorage.getItem('users') || '[]');
  const user = users.find(u => u.email === email && u.password === password);
  if (user) {
    localStorage.setItem('loggedIn', 'true');
    localStorage.setItem('user', user.username);
    localStorage.setItem('userEmail', user.email);
    localStorage.setItem('regDate', user.regDate);
    alert(`Добро пожаловать, ${user.username}! 🌐`);
    checkLogin();
    showSection('home');
  } else {
    alert('Неверный email или пароль ❌');
  }
}
function deletePoll(pollId) {
  if (!confirm('Вы действительно хотите удалить этот опрос?')) return;

  const user = getCurrentUser();
  
  // 1. Удаляем из личных сохранённых опросов
  let saved = getSavedPolls();
  saved = saved.filter(p => p.id !== pollId);
  localStorage.setItem(`savedPolls_${user}`, JSON.stringify(saved));

  // 2. Удаляем из myPolls (для статистики)
  let myPolls = getMyPolls();
  myPolls = myPolls.filter(p => p.id !== pollId);
  localStorage.setItem(`myPolls_${user}`, JSON.stringify(myPolls));

  // 3. Если это был текущий открытый опрос — очищаем его
  const current = JSON.parse(localStorage.getItem('currentPoll'));
  if (current && current.id === pollId) {
    localStorage.removeItem('currentPoll');
  }

  alert('Опрос успешно удалён ✅');

  // 4. Обновляем все списки на странице
  loadSavedPolls();      // обновляем раздел "Сохранённые"
  loadEditPolls();       // обновляем раздел редактирования
  loadCreatorStats();    // обновляем обычную статистику

  // Если открыт раздел онлайн-статистики — тоже обновляем
  if (document.getElementById('online-stats').style.display === 'block') {
    loadOnlineStats();
  }

  console.log(`Опрос ${pollId} удалён для пользователя ${user}`);
}

function showSection(sectionId) {
  const loggedIn = localStorage.getItem('loggedIn') === 'true';

  // Разрешённые разделы без авторизации
  const publicSections = ['home', 'login'];

  if (!loggedIn && !publicSections.includes(sectionId)) {
    alert('❌ Для доступа к этому разделу необходимо войти в аккаунт!');
    sectionId = 'login';
  }

  // Убираем активный класс у всех кнопок
  document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));

  // Активируем кнопку (если она есть)
  const activeBtn = document.querySelector(`nav button[onclick="showSection('${sectionId}')"]`);
  if (activeBtn) activeBtn.classList.add('active');

  // Скрываем все секции
  document.querySelectorAll('.section').forEach(sec => sec.style.display = 'none');

  // Показываем нужную
  const section = document.getElementById(sectionId);
  if (section) {
    section.style.display = 'block';
  } else {
    document.getElementById('home').style.display = 'block';
  }

  // Загрузка данных в зависимости от раздела
  if (sectionId === 'vote') loadPoll();
  if (sectionId === 'results') showResults();
  if (sectionId === 'saved') loadSavedPolls();
  if (sectionId === 'stats') loadCreatorStats();
  if (sectionId === 'create') loadEditPolls();
  if (sectionId === 'online') loadPublicPolls();
  if (sectionId === 'online-stats') loadOnlineStats();
  if (sectionId === 'friends') {
    loadFriendsList();
    updateFriendsUI();
  }
}

function getCurrentUser() { return localStorage.getItem('user') || 'Гость'; }
function logout() {
  if (!confirm('Выйти из аккаунта?')) return;

  // Сбрасываем данные аккаунта
  localStorage.removeItem('loggedIn');
  localStorage.removeItem('user');
  localStorage.removeItem('userEmail');
  localStorage.removeItem('regDate');
  localStorage.removeItem('userAvatar');

  // Сбрасываем аватарку на стандартную
  document.getElementById('avatar').src = 'https://i.pravatar.cc/150?u=default';
  const dropdownAvatar = document.getElementById('dropdown-avatar');
  if (dropdownAvatar) dropdownAvatar.src = 'https://i.pravatar.cc/150?u=default';

  // Обновляем интерфейс
  checkLogin();
  updateProfileUI();

  showSection('home');
  alert('Вы вышли из аккаунта');
}

function checkLogin() {
  const logged = localStorage.getItem('loggedIn') === 'true';
  
  const loginBtn = document.getElementById('loginBtn');
  const logoutBtn = document.getElementById('logoutBtn');
  
  if (loginBtn) loginBtn.style.display = logged ? 'none' : 'inline-block';
  if (logoutBtn) logoutBtn.style.display = logged ? 'inline-block' : 'none';

  updateProfileUI();

  // Если пользователь не залогинен — показываем только главную
  if (!logged) {
    document.querySelectorAll('.section').forEach(sec => {
      if (sec.id !== 'home' && sec.id !== 'login') {
        sec.style.display = 'none';
      }
    });
  }
}

function getMyPolls() {
  return JSON.parse(localStorage.getItem(`myPolls_${getCurrentUser()}`) || '[]');
}
function saveMyPoll(poll) {
  let polls = getMyPolls().filter(p => p.id !== poll.id);
  polls.push(poll);
  localStorage.setItem(`myPolls_${getCurrentUser()}`, JSON.stringify(polls));
}
let optionsCount = 0;
function addOption() {
  optionsCount++;
  const container = document.getElementById('optionsContainer');
  const div = document.createElement('div');
  div.className = 'option';
  div.innerHTML = `<input type="text" class="opt-input" placeholder="Вариант ${optionsCount}"><button onclick="this.parentElement.remove()">×</button>`;
  container.appendChild(div);
}
function createPoll() {
  const title = document.getElementById('pollTitle').value.trim();
  if (!title) return alert('Введите название опроса!');
  const desc = document.getElementById('pollDesc').value.trim();
  const options = Array.from(document.querySelectorAll('.opt-input')).map(inp => inp.value.trim()).filter(v => v);
  if (options.length < 2) return alert('Минимум 2 варианта!');

if (!checkPollForProfanity(title, desc, options)) return;

  const poll = {
    id: Date.now(),
    title, desc, options,
    votes: Array(options.length).fill(0),
    totalVotes: 0,
    creator: getCurrentUser(),
    date: new Date().toLocaleString('ru-RU'),
    voted: false
  };
  localStorage.setItem('currentPoll', JSON.stringify(poll));
  saveMyPoll(poll);
  alert('Опрос создан! 🚀');
  resetCreateForm();
  showSection('vote');
}
function savePoll() {
  const title = document.getElementById('pollTitle').value.trim();
  if (!title) return alert('Введите название опроса!');
  const desc = document.getElementById('pollDesc').value.trim();
  const options = Array.from(document.querySelectorAll('.opt-input')).map(inp => inp.value.trim()).filter(v => v);
  if (options.length < 2) return alert('Минимум 2 варианта!');

if (!checkPollForProfanity(title, desc, options)) return;

  const poll = {
    id: Date.now(), title, desc, options,
    votes: Array(options.length).fill(0), totalVotes: 0,
    creator: getCurrentUser(), date: new Date().toLocaleString('ru-RU'), voted: false
  };

  saveMyPoll(poll);
  saveToSavedPolls(poll);        // ←←← ДОБАВЬ ЭТУ СТРОКУ

  alert('Опрос сохранён! 💾');
}
function resetCreateForm() {
  document.getElementById('pollTitle').value = '';
  document.getElementById('pollDesc').value = '';
  document.getElementById('optionsContainer').innerHTML = '';
  optionsCount = 0;
  addOption(); addOption();
}
function loadPoll() {
  const poll = JSON.parse(localStorage.getItem('currentPoll'));
  const area = document.getElementById('pollArea');
  const noPoll = document.getElementById('noPoll');
  if (!poll) {
    area.classList.add('hidden');
    noPoll.classList.remove('hidden');
    return;
  }
  area.classList.remove('hidden');
  noPoll.classList.add('hidden');
  document.getElementById('currentTitle').textContent = poll.title;
  const div = document.getElementById('currentOptions');
  div.innerHTML = '';
  poll.options.forEach((opt, i) => {
    div.innerHTML += `<label class="option-label"><input type="radio" name="voteOption" value="${i}"> ${opt}</label>`;
  });
}
function submitVote() {
  const selected = document.querySelector('input[name="voteOption"]:checked');
  if (!selected) return alert('Выберите вариант!');
  let poll = JSON.parse(localStorage.getItem('currentPoll'));
  const count = parseInt(document.getElementById('voteCount').value) || 1;
  const added = distributeVotes(poll, count);
  for (let i = 0; i < added.length; i++) {
    poll.votes[i] += added[i];
  }
  poll.totalVotes = (poll.totalVotes || 0) + count;
  poll.voted = true;
  localStorage.setItem('currentPoll', JSON.stringify(poll));
  if (poll.creator === getCurrentUser()) saveMyPoll(poll);
  document.getElementById('voteMessage').innerHTML = `Голоса распределены! (+${count}) ✅`;
  document.getElementById('voteMessage').classList.remove('hidden');
  setTimeout(() => showSection('results'), 1200);
}
function aiVote() {
  let poll = JSON.parse(localStorage.getItem('currentPoll'));
  if (!poll) return alert('Сначала создайте опрос!');
  if (poll.voted) {
    if (!confirm('Вы уже голосовали. Голосовать ещё раз?')) return;
    poll.voted = false;
  }
  const count = parseInt(document.getElementById('voteCount').value) || 1;
  if (count < 1) return alert('Количество голосов должно быть хотя бы 1');
  const added = distributeVotes(poll, count);
  for (let i = 0; i < added.length; i++) {
    poll.votes[i] += added[i];
  }
  poll.totalVotes = (poll.totalVotes || 0) + count;
  poll.voted = true;
  localStorage.setItem('currentPoll', JSON.stringify(poll));
  if (poll.creator === getCurrentUser()) saveMyPoll(poll);
  document.getElementById('voteMessage').innerHTML = `Автоматическое голосование: +${count} голосов распределено 🤖`;
  document.getElementById('voteMessage').classList.remove('hidden');
  setTimeout(() => showSection('results'), 1500);
}
function distributeVotes(poll, totalVotes) {
  const n = poll.options.length;
  if (n === 0 || totalVotes <= 0) return new Array(n).fill(0);
  const added = new Array(n).fill(0);
  let remaining = totalVotes;
  for (let i = 0; i < n; i++) {
    const base = Math.floor(Math.random() * (totalVotes / n * 0.7)) + Math.floor(totalVotes / n * 0.1);
    added[i] = base;
    remaining -= base;
  }
  while (remaining > 0) {
    const idx = Math.floor(Math.random() * n);
    const add = Math.floor(Math.random() * Math.min(remaining, Math.floor(totalVotes * 0.18))) + 1;
    added[idx] += add;
    remaining -= add;
  }
  if (totalVotes > 100) {
    const leader = Math.floor(Math.random() * n);
    added[leader] += Math.floor(totalVotes * (0.12 + Math.random() * 0.18));
  }
  return added;
}
function showResults() {
  const poll = JSON.parse(localStorage.getItem('currentPoll'));
  const area = document.getElementById('resultsArea');
  const noRes = document.getElementById('noResults');
  if (!poll) {
    area.innerHTML = '';
    noRes.classList.remove('hidden');
    return;
  }
  noRes.classList.add('hidden');
  const total = poll.totalVotes || poll.votes.reduce((a,b)=>a+b,0);
  let html = `<p class="message">Всего голосов: <strong>${total}</strong> 📊</p>`;
  poll.options.forEach((opt, i) => {
    const percent = total ? Math.round((poll.votes[i] / total) * 100) : 0;
    html += `
      <div style="margin:28px 0;">
        <strong>${opt}</strong> — ${poll.votes[i]} (${percent}%)
        <div class="results-bar">
          <div class="bar-fill" style="width:${percent}%">
            <span class="bar-text">${percent > 8 ? percent + '%' : ''}</span>
          </div>
        </div>
      </div>`;
  });
  area.innerHTML = html;
}
function resetPoll() {
  if (!confirm('Сбросить результаты текущего опроса?\n\nАватарка, имя и настройки профиля останутся.')) {
    return;
  }

  // Сбрасываем только опрос
  localStorage.removeItem('currentPoll');

  // Очищаем экран результатов
  const resultsArea = document.getElementById('resultsArea');
  const noResults = document.getElementById('noResults');
  
  if (resultsArea) resultsArea.innerHTML = '';
  if (noResults) noResults.classList.remove('hidden');

  // Показываем приятное сообщение
  const msg = document.createElement('div');
  msg.className = 'message';
  msg.style.color = '#0f0';
  msg.innerHTML = 'Опрос успешно сброшен ✅<br><small>Аватарка и профиль сохранены</small>';
  
  const resultsSection = document.getElementById('results');
  if (resultsSection) {
    resultsSection.appendChild(msg);
    
    // Убираем сообщение через 3 секунды
    setTimeout(() => {
      if (msg && msg.parentNode) msg.parentNode.removeChild(msg);
    }, 3000);
  }
}
function loadSavedPolls() {
  const polls = getSavedPolls();
  const container = document.getElementById('savedPollsList');
  const noSaved = document.getElementById('noSaved');
  
  container.innerHTML = '';
  if (polls.length === 0) {
    noSaved.classList.remove('hidden');
    return;
  }
  noSaved.classList.add('hidden');

  polls.forEach(p => {
    const card = document.createElement('div');
    card.className = 'stats-card';
    card.innerHTML = `
      <h3>${p.title}</h3>
      <p>${p.desc || 'Без описания'}</p>
      <small>${p.date}</small>
      <button onclick="loadSavedPoll(${p.id})" style="margin-top:14px; width:calc(100% - 50px); padding:12px; background:var(--neon); color:#000; border:none; border-radius:8px; cursor:pointer;">
        Открыть для голосования 🔓
      </button>
      <button onclick="deletePoll(${p.id})" class="delete-btn">×</button>
    `;
    container.appendChild(card);
  });
}

function loadEditPolls() {
  const polls = getSavedPolls();
  const container = document.getElementById('editPollsList');
  container.innerHTML = '';
  
  if (polls.length === 0) {
    container.innerHTML = '<p class="message">Список сохранённых опросов пуст</p>';
    return;
  }
  
  polls.forEach(p => {
    const card = document.createElement('div');
    card.className = 'stats-card';
    card.innerHTML = `
      <h3>${p.title}</h3>
      <p>${p.desc || 'Без описания'}</p>
      <small>Создан: ${p.date}</small>
      <button onclick="loadPollForEdit(${p.id})" style="margin-top:18px; width:calc(100% - 50px); padding:14px; background:var(--neon); color:#000; border:none; border-radius:8px; cursor:pointer;">
        Редактировать ✏️
      </button>
      <button onclick="deletePoll(${p.id})" class="delete-btn">×</button>
    `;
    container.appendChild(card);
  });
}
function loadSavedPoll(pollId) {
  const saved = getSavedPolls();           // ←←← ИСПРАВЛЕНО
  const poll = saved.find(p => p.id === pollId);
  
  if (!poll) return alert('Опрос не найден');
  
  localStorage.setItem('currentPoll', JSON.stringify(JSON.parse(JSON.stringify(poll))));
  alert(`Опрос "${poll.title}" загружен!`);
  showSection('vote');
}
function loadPollForEdit(pollId) {
  const saved = getSavedPolls();           // ←←← ИСПРАВЛЕНО
  const poll = saved.find(p => p.id === pollId);
  
  if (!poll) return alert('Опрос не найден');

  // Заполняем форму
  document.getElementById('pollTitle').value = poll.title;
  document.getElementById('pollDesc').value = poll.desc || '';
  
  const container = document.getElementById('optionsContainer');
  container.innerHTML = '';
  optionsCount = 0;

  poll.options.forEach(opt => {
    optionsCount++;
    const div = document.createElement('div');
    div.className = 'option';
    div.innerHTML = `
      <input type="text" class="opt-input" value="${opt}">
      <button onclick="this.parentElement.remove()">×</button>
    `;
    container.appendChild(div);
  });

  alert('Опрос загружен для редактирования ✅');
  
  // Переключаем на вкладку создания
  showSection('create');
}
function loadCreatorStats() {
  const user = getCurrentUser();
  document.getElementById('statUser').textContent = user;
  document.getElementById('statRegDate').textContent = localStorage.getItem('regDate') || '—';
  const myPolls = getMyPolls();
  const totalPolls = myPolls.length;
  const totalVotes = myPolls.reduce((sum, p) => sum + (p.totalVotes || 0), 0);
  const avgVotes = totalPolls > 0 ? Math.round(totalVotes / totalPolls) : 0;
  document.getElementById('statCreated').textContent = totalPolls;
  document.getElementById('statTotalVotes').textContent = totalVotes;
  document.getElementById('statAvgVotes').textContent = avgVotes;
  const noStats = document.getElementById('noStatsMessage');
  if (totalPolls === 0) {
    noStats.style.display = 'block';
    document.getElementById('topPoll').innerHTML = '<p style="color:#888;">Нет данных</p>';
    document.getElementById('topOptions').innerHTML = '';
    document.getElementById('pollStatsList').innerHTML = '';
    return;
  }
  noStats.style.display = 'none';
  let topPoll = myPolls[0];
  myPolls.forEach(p => {
    if ((p.totalVotes || 0) > (topPoll.totalVotes || 0)) topPoll = p;
  });
  document.getElementById('topPoll').innerHTML = `
    <h3 style="color:var(--yellow); margin-bottom:10px;">${topPoll.title}</h3>
    <p style="font-size:2.3rem; color:var(--neon);"><strong>${topPoll.totalVotes || 0}</strong> голосов</p>
    <small>Создан: ${topPoll.date}</small>
  `;
  let allOptions = [];
  myPolls.forEach(poll => {
    poll.options.forEach((text, i) => {
      allOptions.push({ text, votes: poll.votes[i] || 0, pollTitle: poll.title });
    });
  });
  allOptions.sort((a, b) => b.votes - a.votes);
  const top3 = allOptions.slice(0, 3);
  let topHTML = '';
  top3.forEach((item, i) => {
    topHTML += `
      <div class="stats-card">
        <h4 style="color:var(--neon);">№${i+1} ${item.text}</h4>
        <p><strong>${item.votes}</strong> голосов</p>
        <small>из «${item.pollTitle}»</small>
      </div>`;
  });
  document.getElementById('topOptions').innerHTML = topHTML || '<p style="color:#888; text-align:center;">Пока нет данных</p>';
  let pollsHTML = '';
  myPolls.forEach(poll => {
    pollsHTML += `
      <div class="stats-card">
        <h4>${poll.title}</h4>
        <p>Голосов: <strong>${poll.totalVotes || 0}</strong></p>
        <small>${poll.date}</small>
      </div>`;
  });
  document.getElementById('pollStatsList').innerHTML = pollsHTML || '<p style="color:#888; text-align:center;">Нет опросов</p>';
}

let publicOptionsCount = 0;
let currentPublicPollId = null;
let timerInterval = null;
function addPublicOption() {
  publicOptionsCount++;
  const container = document.getElementById('publicOptionsContainer');
  const div = document.createElement('div');
  div.className = 'option';
  div.innerHTML = `<input type="text" class="public-opt-input" placeholder="Вариант ${publicOptionsCount}"><button onclick="this.parentElement.remove()">×</button>`;
  container.appendChild(div);
}
function createPublicPoll() {
  const title = document.getElementById('publicPollTitle').value.trim();
  if (!title) return alert('Введите название опроса!');

  const desc = document.getElementById('publicPollDesc').value.trim();
  const options = Array.from(document.querySelectorAll('.public-opt-input'))
                       .map(inp => inp.value.trim())
                       .filter(v => v);

  if (options.length < 2) return alert('Минимум 2 варианта!');

  const durationMs = parseInt(document.getElementById('publicDuration').value);
  const endTime = Date.now() + durationMs;
  const createdAt = Date.now();

  const creatorAvatar = localStorage.getItem('userAvatar') || 'https://i.pravatar.cc/150?u=' + getCurrentUser();

if (!checkPollForProfanity(title, desc, options)) return;

  const poll = {
    id: Date.now(),
    title,
    desc,
    options,
    votes: Array(options.length).fill(0),
    totalVotes: 0,
    creator: getCurrentUser(),
    creatorAvatar: creatorAvatar,
    creatorEmail: localStorage.getItem('userEmail') || 'guest',
    date: new Date().toLocaleString('ru-RU'),
    createdAt: createdAt,
    endTime: endTime,
    isPublic: true,
    votedUsers: []
  };

  let publicPolls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  publicPolls.unshift(poll);
  localStorage.setItem('publicPolls', JSON.stringify(publicPolls));

  // ==================== УЛУЧШЕННАЯ КАРТОЧКА С КНОПКОЙ ЗАКРЫТИЯ ====================
  const shareLink = window.location.origin + window.location.pathname + '#online';

  const successHTML = `
    <div id="successShareCard" style="margin: 30px 0; padding: 25px; background: #1e1e1e; border-radius: 16px; border: 2px solid var(--neon); position: relative;">
      <button onclick="closeShareCard()" 
              style="position: absolute; top: 12px; right: 12px; background: none; border: none; font-size: 1.8rem; color: #aaa; cursor: pointer;">
        ×
      </button>
      
      <h2 style="color: #0f0; margin-bottom: 20px;">✅ Опрос успешно опубликован!</h2>
      
      <p style="font-size: 1.15rem; margin-bottom: 20px;">Поделиться опросом с друзьями:</p>
      
      <div style="background: #111; padding: 18px; border-radius: 12px; margin: 20px 0; word-break: break-all; font-family: monospace; color: var(--yellow);">
        ${shareLink}
      </div>
      
      <button onclick="copyShareLink('${shareLink}')" 
              style="padding: 16px 40px; font-size: 1.3rem; background: var(--neon); color: #000; border: none; border-radius: 12px; cursor: pointer; font-weight: bold;">
        📋 Скопировать ссылку
      </button>
      
      <p style="margin-top: 20px; color: #aaa; font-size: 0.95rem;">
        Друг откроет эту ссылку — сразу попадёт в раздел «Онлайн»
      </p>
    </div>
  `;

  // Добавляем карточку
  const createSection = document.querySelector('#create-public-title').parentElement || document.getElementById('create');
  if (createSection) {
    const div = document.createElement('div');
    div.innerHTML = successHTML;
    createSection.appendChild(div);
  }

  // Очистка формы
  document.getElementById('publicPollTitle').value = '';
  document.getElementById('publicPollDesc').value = '';
  document.getElementById('publicOptionsContainer').innerHTML = '';
  publicOptionsCount = 0;
  addPublicOption(); 
  addPublicOption();

  loadPublicPolls();
}
function loadPublicPolls() {
  let polls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  const currentUser = getCurrentUser();
  const threeDaysMs = 3 * 24 * 60 * 60 * 1000; // 3 дня в миллисекундах
  let changed = false;

  // Проверка и автоудаление старых опросов
  polls = polls.filter(poll => {
    const age = Date.now() - (poll.createdAt || 0);
    
    if (age > threeDaysMs) {
      console.log(`Автоудалён опрос: ${poll.title} (прошло больше 3 дней)`);
      changed = true;
      return false; // удаляем опрос
    }
    return true;
  });

  // Сохраняем изменения, если что-то удалили
  if (changed) {
    localStorage.setItem('publicPolls', JSON.stringify(polls));
  }

  const container = document.getElementById('publicPollsList');
  const noPublic = document.getElementById('noPublicPolls');
  
  container.innerHTML = '';
  if (polls.length === 0) {
    noPublic.classList.remove('hidden');
    return;
  }
  noPublic.classList.add('hidden');
polls.forEach(p => {
    const now = Date.now();
    const remaining = Math.max(0, p.endTime - now);
    const hours = Math.floor(remaining / 3600000);
    const minutes = Math.floor((remaining % 3600000) / 60000);
    const timerText = remaining > 0 ? `${hours}ч ${minutes}м` : '⏹ Завершён';

    const canDelete = p.creator === currentUser;
    const avatarUrl = p.creatorAvatar || 'https://i.pravatar.cc/150?u=' + encodeURIComponent(p.creator);

    const card = document.createElement('div');
    card.className = 'stats-card public-card';
    card.style.position = 'relative';

    card.innerHTML = `
      <div style="display:flex; align-items:center; gap:12px; margin-bottom:12px;">
        <img src="${avatarUrl}" style="width:48px; height:48px; border-radius:50%; object-fit:cover; border:2px solid var(--neon);">
        <div>
          <strong style="color:var(--yellow);">${p.creator}</strong><br>
          <small style="color:#aaa;">${p.date}</small>
        </div>
      </div>

      <!-- Кнопка Добавить в друзья -->
      ${p.creator !== currentUser ? `
        <button onclick="addFriendFromPoll({username: '${p.creator}', email: '${p.creatorEmail || p.creator + '@gmail.com'}', avatar: '${avatarUrl}'}); 
                         this.style.opacity='0.3'; this.style.pointerEvents='none'; event.stopImmediatePropagation();" 
                style="position:absolute; top:12px; right:55px; background:none; border:none; font-size:26px; cursor:pointer; transition:0.2s;">
          ❤️
        </button>` : ''}

      <h3>${p.title}</h3>
      <p>${p.desc || 'Без описания'}</p>
      
      <div class="online-timer" style="font-size:1.3rem; margin:15px 0;">⏳ ${timerText}</div>
      
      <button onclick="openPublicPoll(${p.id}); event.stopImmediatePropagation();" 
              style="width:100%; padding:16px; background:var(--neon); color:#000; border:none; border-radius:12px; font-weight:700; margin-top:10px;">
        Голосовать / Посмотреть результаты
      </button>
      
      ${canDelete ? `<button onclick="deletePublicPoll(${p.id}); event.stopImmediatePropagation();" class="delete-btn" style="top:12px;right:12px;">×</button>` : ''}
    `;

    container.appendChild(card);
});
 }
function deletePublicPoll(id) {
  const currentUser = getCurrentUser();
  let polls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  const poll = polls.find(p => p.id === id);

  if (!poll) return alert('Опрос не найден');

  if (poll.creator !== currentUser) {
    return alert('❌ Вы не можете удалить этот опрос!\nОн создан другим пользователем.');
  }

  if (!confirm('Удалить этот публичный опрос?')) return;

  polls = polls.filter(p => p.id !== id);
  localStorage.setItem('publicPolls', JSON.stringify(polls));
  loadPublicPolls();
  alert('Публичный опрос удалён ✅');
}
function openPublicPoll(pollId) {
  const polls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  const poll = polls.find(p => p.id === pollId);
  if (!poll) return alert('Опрос не найден');
  currentPublicPollId = pollId;
  document.getElementById('publicVoteArea').style.display = 'block';
  document.getElementById('publicCurrentTitle').textContent = poll.title;
  const votedKey = `voted_${pollId}`;
  const currentEmail = localStorage.getItem('userEmail');
const alreadyVoted = poll.votedUsers && poll.votedUsers.includes(currentEmail);
  const optionsDiv = document.getElementById('publicCurrentOptions');
  optionsDiv.innerHTML = '';
  if (alreadyVoted) {
    document.getElementById('btn-public-vote').style.display = 'none';
    document.getElementById('publicVoteMessage').innerHTML = `Вы уже проголосовали в этом опросе`;
    document.getElementById('publicVoteMessage').classList.remove('hidden');
    showPublicResults(poll);
  } else {
    document.getElementById('btn-public-vote').style.display = 'block';
    document.getElementById('publicVoteMessage').classList.add('hidden');
    poll.options.forEach((opt, i) => {
      optionsDiv.innerHTML += `<label class="option-label"><input type="radio" name="publicVoteOption" value="${i}"> ${opt}</label>`;
    });
  }
  startPublicTimer(poll);
}
function hidePublicVoteArea() {
  document.getElementById('publicVoteArea').style.display = 'none';
  currentPublicPollId = null;
  if (timerInterval) clearInterval(timerInterval);
}
function startPublicTimer(poll) {
  if (timerInterval) clearInterval(timerInterval);
  function updateTimer() {
    const remaining = Math.max(0, poll.endTime - Date.now());
    if (remaining <= 0) {
      document.getElementById('timerValue').innerHTML = `<span style="color:#ff4444;">Опрос завершён</span>`;
      clearInterval(timerInterval);
      return;
    }
    const h = Math.floor(remaining / 3600000);
    const m = Math.floor((remaining % 3600000) / 60000);
    const s = Math.floor((remaining % 60000) / 1000);
    document.getElementById('timerValue').textContent = `${h}ч ${m}м ${s}с`;
  }
  updateTimer();
  timerInterval = setInterval(updateTimer, 1000);
}

function submitPublicVote() {
    const selected = document.querySelector('input[name="publicVoteOption"]:checked');
    
    if (!selected) {
        alert('Выберите вариант ответа!');
        return;
    }

    let polls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
    const pollIndex = polls.findIndex(p => p.id === currentPublicPollId);
    
    if (pollIndex === -1) {
        alert('Опрос не найден!');
        return;
    }

    const poll = polls[pollIndex];
    const currentEmail = localStorage.getItem('userEmail');

    if (!currentEmail) {
        alert('Сначала войдите в аккаунт!');
        return;
    }

    if (Date.now() > poll.endTime) {
        alert('Опрос уже завершён!');
        return;
    }

    // Проверка голосования по аккаунту
    if (poll.votedUsers && poll.votedUsers.includes(currentEmail)) {
        alert('Вы уже проголосовали в этом опросе с этого аккаунта!');
        return;
    }

    const optionIndex = parseInt(selected.value);

    poll.votes[optionIndex] = (poll.votes[optionIndex] || 0) + 1;
    poll.totalVotes = (poll.totalVotes || 0) + 1;

    if (!poll.votedUsers) poll.votedUsers = [];
    poll.votedUsers.push(currentEmail);

    localStorage.setItem('publicPolls', JSON.stringify(polls));

    // Успешное голосование
    document.getElementById('publicVoteMessage').innerHTML = `✅ Ваш голос успешно засчитан!`;
    document.getElementById('publicVoteMessage').classList.remove('hidden');
    document.getElementById('btn-public-vote').style.display = 'none';

    showPublicResults(poll);
}

function showPublicResults(poll) {
  const resultsDiv = document.getElementById('publicResults');
  const total = poll.totalVotes || poll.votes.reduce((a, b) => a + b, 0);
  let html = `<p class="message">Всего голосов: <strong>${total}</strong> 📊</p>`;
  poll.options.forEach((opt, i) => {
    const votes = poll.votes[i] || 0;
    const percent = total ? Math.round((votes / total) * 100) : 0;
    html += `
      <div style="margin:25px 0;">
        <strong>${opt}</strong> — ${votes} (${percent}%)
        <div class="results-bar">
          <div class="bar-fill" style="width:${percent}%"></div>
          <span class="bar-text">${percent}%</span>
        </div>
      </div>`;
  });
  resultsDiv.innerHTML = html;
}

// ======================== ПРОФИЛЬ ========================
function toggleProfileDropdown() {
  document.getElementById('profileDropdown').classList.toggle('show');
}

document.addEventListener('click', function(e) {
  if (!document.getElementById('profileContainer').contains(e.target)) {
    document.getElementById('profileDropdown').classList.remove('show');
  }
});

function updateProfileUI() {
  const logged = localStorage.getItem('loggedIn') === 'true';
  const username = localStorage.getItem('user') || 'Гость';
  const email = localStorage.getItem('userEmail') || '';
  const avatarUrl = localStorage.getItem('userAvatar') || 'https://i.pravatar.cc/150?u=default';

  // Основная аватарка в шапке
  const mainAvatar = document.getElementById('avatar');
  if (mainAvatar) mainAvatar.src = avatarUrl;

  // Аватарка в выпадающем меню профиля
  const dropdownAvatar = document.getElementById('dropdown-avatar');
  if (dropdownAvatar) dropdownAvatar.src = avatarUrl;

  // Имя и email в профиле
  const profileName = document.getElementById('profile-name');
  const profileEmail = document.getElementById('profile-email');
  if (profileName) profileName.textContent = username;
  if (profileEmail) profileEmail.textContent = email;

  // Вкладка Friends
  const friendsName = document.getElementById('friends-name');
  const friendsEmail = document.getElementById('friends-email');
  const friendsAvatar = document.getElementById('friends-avatar');
  if (friendsName) friendsName.textContent = username;
  if (friendsEmail) friendsEmail.textContent = email;
  if (friendsAvatar) friendsAvatar.src = avatarUrl;

  // Кнопки входа/выхода
  const loginBtn = document.getElementById('loginBtn');
  const logoutBtn = document.getElementById('logoutBtn');
  if (loginBtn) loginBtn.style.display = logged ? 'none' : 'inline-block';
  if (logoutBtn) logoutBtn.style.display = logged ? 'inline-block' : 'none';
}

// ==================== ОБНОВЛЕНИЕ ЛИЧКИ ====================
function updateFriendsUI() {
  const nameEl = document.getElementById('friends-name');
  const emailEl = document.getElementById('friends-email');
  const avatarEl = document.getElementById('friends-avatar');

  if (nameEl) nameEl.textContent = localStorage.getItem('user') || 'Гость';
  if (emailEl) emailEl.textContent = localStorage.getItem('userEmail') || '';
  if (avatarEl) {
    avatarEl.src = localStorage.getItem('userAvatar') || 'https://i.pravatar.cc/150?u=default';
  }
}
function editUsername() {
  const newName = prompt('Новое имя пользователя:', localStorage.getItem('user') || '');
  if (newName && newName.trim() !== '') {
    localStorage.setItem('user', newName.trim());
    updateProfileUI();
    alert('Имя изменено!');
  }
}

function changeAvatar() {
  const newUrl = prompt('Введите URL новой аватарки:', 
                        localStorage.getItem('userAvatar') || document.getElementById('avatar').src);
  
  if (newUrl && newUrl.trim() !== '') {
    const url = newUrl.trim();
    localStorage.setItem('userAvatar', url);        // ← сохраняем
    
    document.getElementById('avatar').src = url;
    document.getElementById('dropdown-avatar').src = url;
  }
}

// Увеличение аватарки при клике на неё в профиле
function enlargeAvatar(src) {
  let modal = document.getElementById('enlargeModal');
  
  if (!modal) {
    modal = document.createElement('div');
    modal.id = 'enlargeModal';
    modal.innerHTML = `<img src="${src}" alt="Увеличенная аватарка">`;
    document.body.appendChild(modal);
    
    // Закрытие по клику на фон или картинку
    modal.addEventListener('click', () => {
      modal.style.display = 'none';
    });
  } else {
    modal.querySelector('img').src = src;
  }
  
  modal.style.display = 'flex';
}

// Загрузка аватарки с компьютера
function uploadAvatar() {
  const input = document.createElement('input');
  input.type = 'file';
  input.accept = 'image/*';        // только изображения
  input.onchange = function(e) {
    const file = e.target.files[0];
    if (!file) return;

    // Проверка размера (макс 2 МБ)
    if (file.size > 2 * 1024 * 1024) {
      alert('Файл слишком большой! Максимум 2 МБ.');
      return;
    }

    const reader = new FileReader();
    reader.onload = function(event) {
      const base64 = event.target.result;
      
      // Сохраняем в localStorage
      localStorage.setItem('userAvatar', base64);
      
      // Обновляем аватарки на странице
      document.getElementById('avatar').src = base64;
      const dropdownAvatar = document.getElementById('dropdown-avatar');
      if (dropdownAvatar) dropdownAvatar.src = base64;

      alert('✅ Аватарка успешно загружена!');
    };
    reader.readAsDataURL(file);
  };
  input.click();
}

// ======================== ПРИВАТНОСТЬ СОХРАНЁННЫХ ОПРОСОВ ========================
function getSavedPolls() {
  const user = getCurrentUser();
  if (user === 'Гость') return [];
  return JSON.parse(localStorage.getItem(`savedPolls_${user}`) || '[]');
}

function saveToSavedPolls(poll) {
  const user = getCurrentUser();
  if (user === 'Гость') return;
  
  let saved = getSavedPolls();
  saved = saved.filter(p => p.id !== poll.id);
  saved.push(poll);
  localStorage.setItem(`savedPolls_${user}`, JSON.stringify(saved));
}

function loadOnlineStats() {
  const publicPolls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  const myPublicPolls = publicPolls.filter(p => p.creator === getCurrentUser());

  const container = document.getElementById('online-stats');
  const noStats = document.getElementById('noOnlineStats');

  if (myPublicPolls.length === 0) {
    noStats.style.display = 'block';
    return;
  }
  noStats.style.display = 'none';

  // Подсчёт данных
  let totalVotes = 0;
  myPublicPolls.forEach(p => {
    totalVotes += p.totalVotes || 0;
  });

  const avgVotes = myPublicPolls.length > 0 
    ? Math.round(totalVotes / myPublicPolls.length) 
    : 0;

  // Заполняем карточки
  document.getElementById('online-stat-created').textContent = myPublicPolls.length;
  document.getElementById('online-stat-total').textContent = totalVotes;
  document.getElementById('online-stat-avg').textContent = avgVotes;

  // Самый популярный опрос
  let topPoll = myPublicPolls[0];
  myPublicPolls.forEach(p => {
    if ((p.totalVotes || 0) > (topPoll.totalVotes || 0)) topPoll = p;
  });

  document.getElementById('online-top-poll').innerHTML = `
    <h3 style="color:var(--yellow);">${topPoll.title}</h3>
    <p style="font-size:2.5rem; color:var(--neon);"><strong>${topPoll.totalVotes || 0}</strong> голосов</p>
    <small>Создан: ${topPoll.date}</small>
  `;

  // Топ-3 вариантов
  let allOptions = [];
  myPublicPolls.forEach(poll => {
    poll.options.forEach((text, i) => {
      allOptions.push({
        text: text,
        votes: poll.votes[i] || 0,
        pollTitle: poll.title
      });
    });
  });

  allOptions.sort((a, b) => b.votes - a.votes);
  const top3 = allOptions.slice(0, 3);

  let topHTML = '';
  top3.forEach((item, i) => {
    topHTML += `
      <div class="stats-card">
        <h4 style="color:var(--neon);">№${i+1} ${item.text}</h4>
        <p><strong>${item.votes}</strong> голосов</p>
        <small>из «${item.pollTitle}»</small>
      </div>`;
  });

  document.getElementById('online-top-options').innerHTML = topHTML || '<p style="color:#888;">Пока нет данных</p>';

  // Список всех публичных опросов
  let listHTML = '';
  myPublicPolls.forEach(poll => {
    listHTML += `
      <div class="stats-card">
        <h4>${poll.title}</h4>
        <p>Голосов: <strong>${poll.totalVotes || 0}</strong></p>
        <small>${poll.date}</small>
      </div>`;
  });

  document.getElementById('online-poll-list').innerHTML = listHTML || '<p style="color:#888;">Нет публичных опросов</p>';
}
function copyShareLink(link) {
  navigator.clipboard.writeText(link).then(() => {
    alert('✅ Ссылка успешно скопирована!');
  }).catch(() => {
    alert('Не удалось скопировать автоматически.\n\nВот ссылка:\n' + link);
  });
}

function closeShareCard() {
  const card = document.getElementById('successShareCard');
  if (card) card.parentElement.remove();
}

function addFriendFromPoll(friendData) {
    if (!friendData || !friendData.username) {
        return alert("Ошибка: данные друга не переданы");
    }
    addFriend(friendData);
}

function addFriend(friend) {
  if (friend.username === getCurrentUser()) {
    return alert("🤦 Ты не можешь добавить самого себя в друзья");
  }

  let friends = getFriends();
  
  if (friends.some(f => f.email === friend.email)) {
    return alert("✅ Этот пользователь уже у тебя в друзьях");
  }

  friends.push({
    username: friend.username,
    email: friend.email,
    avatar: friend.avatar || 'https://i.pravatar.cc/150?u=' + encodeURIComponent(friend.username)
  });

  saveFriends(friends);
  alert(`✅ ${friend.username} добавлен в друзья!`);
  
  // Если сейчас открыта вкладка Личка — сразу обновляем
  if (document.getElementById('friends').style.display === 'block') {
    loadFriendsList();
  }
}

function removeFriend(friendEmail) {
  if (!confirm('Удалить этого друга?')) return;

  let friends = getFriends();
  friends = friends.filter(f => f.email !== friendEmail);
 
  saveFriends(friends);
  loadFriendsList();

  alert('Друг успешно удалён ✅');
}
function getFriends() {
  const user = getCurrentUser();
  return JSON.parse(localStorage.getItem(`friends_${user}`) || '[]');
}

function saveFriends(friends) {
  const user = getCurrentUser();
  localStorage.setItem(`friends_${user}`, JSON.stringify(friends));
}
function loadFriendsList() {
  const friends = getFriends();
  const container = document.getElementById('friendsList');
  const noFriends = document.getElementById('noFriends');
  
  if (!container) return;

  container.innerHTML = '';

  if (friends.length === 0) {
    noFriends.style.display = 'block';
    return;
  }

  noFriends.style.display = 'none';

  friends.forEach((friend, index) => {
    const div = document.createElement('div');
    div.className = 'stats-card';
    div.style.position = 'relative';
    div.innerHTML = `
      <div style="display:flex; align-items:center; gap:15px;">
        <img src="${friend.avatar}" style="width:65px;height:65px;border-radius:50%;object-fit:cover;border:3px solid var(--neon);">
        <div style="flex:1;">
          <strong style="font-size:1.2rem;">${friend.username}</strong><br>
          <small style="color:#aaa;">${friend.email}</small>
        </div>
      </div>
      
      <button onclick="removeFriend('${friend.email}'); event.stopImmediatePropagation();" 
              style="position:absolute; top:15px; right:15px; background:#ff4444; color:white; border:none; 
                     width:32px; height:32px; border-radius:50%; font-size:18px; cursor:pointer;">
        ✕
      </button>
    `;
    div.onclick = () => openChat(friend);
    container.appendChild(div);
  });
}
// ======================== ЧАТ ========================

let currentChatFriend = null;

function openChat(friend) {
  const oldModal = document.getElementById('chatModal');
  if (oldModal) oldModal.remove();

  currentChatFriend = friend;

  const chatHTML = `
    <div id="chatModal" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.95); z-index:99999; display:flex; align-items:center; justify-content:center; padding:10px;">
      <div style="background:#1e1e1e; width:100%; max-width:520px; border-radius:16px; overflow:hidden; box-shadow:0 0 40px rgba(0,255,120,0.4);">
        
        <div style="padding:16px 20px; background:#111; display:flex; align-items:center; gap:14px; border-bottom:1px solid #333;">
          <img src="${friend.avatar}" style="width:55px; height:55px; border-radius:50%; border:3px solid var(--neon);">
          <div style="flex:1;">
            <strong style="font-size:1.25rem;">${friend.username}</strong><br>
            <small style="color:#0f0;">● онлайн</small>
          </div>
          <button onclick="closeChat()" style="background:none; border:none; font-size:34px; color:#aaa; cursor:pointer;">×</button>
        </div>

        <div id="chatMessages" style="height:430px; overflow-y:auto; padding:20px; background:#161616; display:flex; flex-direction:column; gap:12px;"></div>

        <div style="padding:16px; background:#111; border-top:1px solid #333; display:flex; gap:10px;">
          <input type="text" id="chatInput" placeholder="Напишите сообщение..." 
                 style="flex:1; padding:15px; border-radius:12px; background:#222; border:none; color:white; font-size:1.05rem;">
          <button onclick="sendMessage()" 
                  style="min-width:60px; padding:0 20px; background:var(--neon); color:#000; border:none; border-radius:12px; font-weight:bold; font-size:1.3rem;">
            →
          </button>
        </div>
      </div>
    </div>
  `;

  document.body.insertAdjacentHTML('beforeend', chatHTML);
  
  setTimeout(loadChatMessages, 50);
  
  const input = document.getElementById('chatInput');
  input.focus();
  input.addEventListener('keypress', e => { if (e.key === 'Enter') sendMessage(); });
}

function closeChat() {
  const modal = document.getElementById('chatModal');
  if (modal) modal.remove();
  currentChatFriend = null;
}

function getChatKey(friendEmail) {
  const me = localStorage.getItem('userEmail') || 'me';
  const users = [me, friendEmail].sort();
  return `chat_${users[0]}_${users[1]}`;
}

function loadChatMessages() {
  const messagesDiv = document.getElementById('chatMessages');
  if (!messagesDiv || !currentChatFriend) return;

  const key = getChatKey(currentChatFriend.email);
  const messages = JSON.parse(localStorage.getItem(key) || '[]');

  messagesDiv.innerHTML = messages.map(msg => `
    <div style="margin:8px 0; ${msg.fromMe ? 'text-align:right' : 'text-align:left'}">
      <div style="display:inline-block; max-width:80%; padding:12px 16px; border-radius:18px; 
                  background: ${msg.fromMe ? 'var(--neon)' : '#333'}; 
                  color: ${msg.fromMe ? '#000' : 'white'};">
        ${msg.text}
        <small style="opacity:0.7; font-size:0.8em; display:block; margin-top:5px;">
          ${msg.time}
        </small>
      </div>
    </div>
  `).join('');

  messagesDiv.scrollTop = messagesDiv.scrollHeight;
}

function sendMessage() {
  const input = document.getElementById('chatInput');
  const text = input.value.trim();
  if (!text || !currentChatFriend) return;

  const key = getChatKey(currentChatFriend.email);
  let messages = JSON.parse(localStorage.getItem(key) || '[]');

  messages.push({
    fromMe: true,
    text: text,
    time: new Date().toLocaleTimeString('ru-RU', {hour: '2-digit', minute: '2-digit'})
  });

  localStorage.setItem(key, JSON.stringify(messages));
  input.value = '';
  loadChatMessages();
}

// ======================== МУЛЬТИ-ГОЛОСОВАНИЕ ========================
let multiPollCounter = 0;

function addMultiPoll() {
  multiPollCounter++;
  const container = document.getElementById('multiPollsContainer');
  const pollDiv = document.createElement('div');
  pollDiv.className = 'option';
  pollDiv.style.marginBottom = '20px';
  pollDiv.innerHTML = `
    <div style="flex:1;">
      <input type="text" class="multi-poll-title" placeholder="Название опроса ${multiPollCounter}" style="width:100%; margin-bottom:8px;">
      <textarea class="multi-poll-desc" placeholder="Описание (необязательно)" rows="2" style="width:100%; margin-bottom:10px; resize:vertical;"></textarea>
      <div class="multi-options" style="display:flex; flex-direction:column; gap:8px; margin-bottom:10px;"></div>
      <button onclick="addOptionToMultiPoll(this)" style="background:#222; color:var(--neon); padding:8px 20px; border:1px solid var(--dim); border-radius:8px;">+ Вариант</button>
    </div>
    <button onclick="this.parentElement.remove()" style="color:#ff6666; font-size:1.6rem; align-self:start;">×</button>
  `;
  container.appendChild(pollDiv);

  // Добавляем 2 варианта по умолчанию
  const opts = pollDiv.querySelector('.multi-options');
  addOptionToMultiPollInternal(opts);
  addOptionToMultiPollInternal(opts);
}

function addOptionToMultiPoll(btn) {
  const container = btn.parentElement.querySelector('.multi-options');
  addOptionToMultiPollInternal(container);
}

function addOptionToMultiPollInternal(container) {
  const div = document.createElement('div');
  div.className = 'option';
  div.style.padding = '8px';
  div.innerHTML = `
    <input type="text" class="multi-opt-input" placeholder="Вариант ответа" style="flex:1;">
    <button onclick="this.parentElement.remove()" style="margin-left:10px; color:#ff6666;">×</button>
  `;
  container.appendChild(div);
}

async function createMultiPoll() {
  const title = document.getElementById('multiTitle').value.trim();
  if (!title) return alert('Введите название пакета!');

  const desc = document.getElementById('multiDesc').value.trim();
  const pollDivs = document.querySelectorAll('#multiPollsContainer > .option');

  if (pollDivs.length < 1) return alert('Добавьте хотя бы один опрос!');

  const multiPolls = [];
  let isValid = true;

  pollDivs.forEach((div, i) => {
    const pTitle = div.querySelector('.multi-poll-title').value.trim();
    const pDesc = div.querySelector('.multi-poll-desc').value.trim();
    const opts = Array.from(div.querySelectorAll('.multi-opt-input'))
                      .map(inp => inp.value.trim()).filter(v => v);

    if (!pTitle || opts.length < 2) {
      alert(`Опрос №${i+1} должен иметь название и минимум 2 варианта!`);
      isValid = false;
    }
    multiPolls.push({
      title: pTitle,
      desc: pDesc || '',
      options: opts,
      votes: Array(opts.length).fill(0),
      totalVotes: 0
    });
  });

  if (!isValid) return;

  if (!localStorage.getItem('loggedIn')) {
    return alert('❌ Сначала войдите в аккаунт!');
  }

  const duration = parseInt(document.getElementById('multiDuration').value);
  const endTime = Date.now() + duration;

  const multiPoll = {
    title,
    desc: desc || '',
    polls: multiPolls,
    creator: getCurrentUser(),
    creatorAvatar: localStorage.getItem('userAvatar') || `https://i.pravatar.cc/150?u=${getCurrentUser()}`,
    creatorEmail: localStorage.getItem('userEmail') || 'guest',
    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
    endTime: endTime,
    votedUsers: []
  };

  try {
    await db.collection('multiPolls').add(multiPoll);
    alert('✅ Мульти-опрос успешно опубликован!');

    document.getElementById('multiTitle').value = '';
    document.getElementById('multiDesc').value = '';
    document.getElementById('multiPollsContainer').innerHTML = '';
    multiPollCounter = 0;

    loadMultiPolls();
  } catch(e) {
    console.error(e);
    alert('❌ Ошибка публикации: ' + e.message);
  }
}

async function loadMultiPolls() {
  const container = document.getElementById('multiPollsList');
  const noMulti = document.getElementById('noMultiPolls');

  container.innerHTML = '<p style="text-align:center; padding:60px; color:#aaa;">Загрузка мульти-опросов...</p>';

  try {
    const snapshot = await db.collection('multiPolls')
      .orderBy('createdAt', 'desc')
      .get();

    let html = '';
    const now = Date.now();
    const currentUser = getCurrentUser();
    const currentEmail = localStorage.getItem('userEmail') || 'guest';

    snapshot.forEach(doc => {
      const mp = doc.data();
      const multiId = doc.id;

      if (mp.endTime && now > mp.endTime) return; // старые не показываем

      const remaining = Math.max(0, mp.endTime - now);
      const h = Math.floor(remaining / 3600000);
      const m = Math.floor((remaining % 3600000) / 60000);

      const canDelete = mp.creator === currentUser;

      html += `
        <div class="stats-card public-card" style="position:relative;">
          <div style="display:flex; align-items:center; gap:12px; margin-bottom:12px;">
            <img src="${mp.creatorAvatar}" style="width:52px;height:52px;border-radius:50%;border:3px solid var(--neon);">
            <div>
              <strong style="color:var(--yellow);">${mp.creator}</strong><br>
              <small>${mp.desc || ''}</small>
            </div>
          </div>
          <h3>${mp.title}</h3>
          <p style="color:#aaa;">${mp.polls.length} опросов в пакете</p>
          <div class="online-timer">⏳ ${h}ч ${m}м</div>

          <button onclick="openMultiVote('${multiId}')" 
                  style="width:100%; padding:16px; margin-top:12px; background:var(--neon); color:#000; border:none; border-radius:12px; font-weight:bold;">
            Голосовать в пакете 🔀
          </button>

          ${canDelete ? `<button onclick="deleteMultiPoll('${multiId}'); event.stopImmediatePropagation();" class="delete-btn">×</button>` : ''}
        </div>`;
    });

    container.innerHTML = html || '<p style="text-align:center; padding:80px; color:#aaa;">Пока нет мульти-опросов. Создайте первый!</p>';
    noMulti.classList.add('hidden');
  } catch(e) {
    console.error(e);
    container.innerHTML = '<p style="color:red; text-align:center;">Ошибка загрузки Firebase</p>';
  }
}

async function deleteMultiPoll(multiId) {
  if (!confirm('Удалить этот мульти-опрос?')) return;
  try {
    await db.collection('multiPolls').doc(multiId).delete();
    alert('Мульти-опрос удалён');
    loadMultiPolls();
  } catch(e) {
    alert('Ошибка удаления');
  }
}

// ======================== ГОЛОСОВАНИЕ В МУЛЬТИ ========================

async function openMultiVote(multiId) {
  currentMultiId = multiId;
  const modal = document.getElementById('multiVoteModal');
  const content = document.getElementById('multiVoteContent');

  try {
    const doc = await db.collection('multiPolls').doc(multiId).get();
    if (!doc.exists) return alert('Опрос не найден');

    let mp = doc.data();

    if (!mp || !Array.isArray(mp.polls) || mp.polls.length === 0) {
      content.innerHTML = `<p style="color:red; text-align:center; padding:50px; font-size:1.3rem;">
        Ошибка загрузки данных опроса.<br>
        Попробуйте создать новый мульти-опрос.
      </p>`;
      modal.style.display = 'flex';
      return;
    }

    const currentEmail = localStorage.getItem('userEmail') || 'guest';
    const alreadyVoted = mp.votedUsers && mp.votedUsers.includes(currentEmail);

    let html = `<h2 style="text-align:center;color:var(--neon);">${mp.title}</h2>`;
    if (mp.desc) html += `<p style="text-align:center;color:#aaa;">${mp.desc}</p>`;

    mp.polls.forEach((poll, i) => {
      html += `
        <div style="background:#1a1a1a; padding:20px; border-radius:16px; margin:20px 0; border:2px solid var(--dim);">
          <div class="poll-question">${poll.title}</div>
          <div id="mopts-${i}"></div>
        </div>`;
    });

    content.innerHTML = html;

    mp.polls.forEach((poll, i) => {
      const div = document.getElementById(`mopts-${i}`);
      const total = poll.totalVotes || 0;

      poll.options.forEach((opt, j) => {
        const votes = Array.isArray(poll.votes) ? (poll.votes[j] || 0) : 0;
        const percent = total ? Math.round((votes / total) * 100) : 0;

        const optionHtml = alreadyVoted ? '' : `
          <label class="option-label" style="margin:8px 0; padding:12px;">
            <input type="radio" name="mpoll-${i}" value="${j}"> ${opt}
          </label>`;

        div.innerHTML += `
          <div style="margin:12px 0; padding:12px; background:#111; border-radius:12px;">
            ${optionHtml}
            <div style="margin-top:8px;">
              <strong>${opt}</strong> — ${votes} (${percent}%)
              <div class="results-bar"><div class="bar-fill" style="width:${percent}%"></div></div>
            </div>
          </div>`;
      });
    });

    document.getElementById('submitMultiBtn').style.display = alreadyVoted ? 'none' : 'block';
    document.getElementById('viewMultiResultsBtn').style.display = 'block';

    modal.style.display = 'flex';
  } catch(e) {
    console.error(e);
    alert('Ошибка загрузки опроса');
  }
}

function closeMultiVoteModal() {
  document.getElementById('multiVoteModal').style.display = 'none';
}

async function submitMultiVote() {
  if (!currentMultiId) return alert('Ошибка: ID опроса не найден');

  const docRef = db.collection('multiPolls').doc(currentMultiId);
  const currentEmail = localStorage.getItem('userEmail') || 'guest';

  try {
    const doc = await docRef.get();
    if (!doc.exists) return alert('Опрос не найден');

    let mp = doc.data();

    if (!mp || !Array.isArray(mp.polls) || mp.polls.length === 0) {
      return alert('Ошибка структуры опроса. Создайте новый мульти-опрос.');
    }

    if (mp.votedUsers && mp.votedUsers.includes(currentEmail)) {
      return alert('Вы уже голосовали в этом пакете!');
    }

    let updates = {};
    let allVoted = true;

    mp.polls.forEach((poll, i) => {
      const selected = document.querySelector(`input[name="mpoll-${i}"]:checked`);
      if (!selected) {
        allVoted = false;
      } else {
        const idx = parseInt(selected.value);
        updates[`polls.${i}.votes.${idx}`] = firebase.firestore.FieldValue.increment(1);
        updates[`polls.${i}.totalVotes`] = firebase.firestore.FieldValue.increment(1);
      }
    });

    if (!allVoted) {
      return alert('Нужно проголосовать во всех опросах пакета!');
    }

    updates.votedUsers = firebase.firestore.FieldValue.arrayUnion(currentEmail);

    await docRef.update(updates);

    alert('✅ Все голоса успешно засчитаны!');

    const freshDoc = await docRef.get();
    showMultiResults(freshDoc.data());

  } catch(e) {
    console.error("Ошибка при отправке голосов:", e);
    alert('❌ Ошибка отправки голосов:\n' + e.message);
  }
}

function showMultiResults(mp) {
  const content = document.getElementById('multiVoteContent');
  let html = `<h2 style="text-align:center;color:var(--neon);">${mp.title}</h2>`;

  if (!mp || !Array.isArray(mp.polls)) {
    html += `<p style="color:red; text-align:center;">Ошибка данных опроса</p>`;
  } else {
    mp.polls.forEach((poll, i) => {
      const total = poll.totalVotes || 0;
      html += `
        <div style="margin:25px 0; background:#111; padding:20px; border-radius:16px;">
          <h4 style="color:var(--yellow);">${poll.title}</h4>`;
      
      poll.options.forEach((opt, j) => {
        const votes = Array.isArray(poll.votes) ? (poll.votes[j] || 0) : 0;
        const percent = total ? Math.round((votes / total) * 100) : 0;
        html += `
          <div style="margin:15px 0;">
            <strong>${opt}</strong> — ${votes} (${percent}%)
            <div class="results-bar"><div class="bar-fill" style="width:${percent}%"></div></div>
          </div>`;
      });
      html += `</div>`;
    });
  }

  html += `<button onclick="closeMultiVoteModal()" class="main" style="margin:25px auto;display:block;">Закрыть</button>`;
  content.innerHTML = html;
  document.getElementById('submitMultiBtn').style.display = 'none';
}

// Показать результаты мульти-опроса (всегда доступно)
function viewMultiResults() {
  const polls = JSON.parse(localStorage.getItem('multiPolls') || '[]');
  const mp = polls.find(p => p.id === currentMultiId);
  if (!mp) return;

  showMultiResults(mp);
}

// ======================== QR-КОД ========================
function showQrCode(multiId) {
  const polls = JSON.parse(localStorage.getItem('multiPolls') || '[]');
  const mp = polls.find(p => p.id === multiId);
  if (!mp) return alert('Опрос не найден');

  const modal = document.getElementById('qrModal');
  const qrContainer = document.getElementById('qrcode');
  const titleEl = document.getElementById('qrPollTitle');

  titleEl.textContent = mp.title;

  // Очищаем предыдущий QR
  qrContainer.innerHTML = '';

  // Создаём QR-код
  const qr = new QRCode(qrContainer, {
    width: 260,
    height: 260,
    colorDark: "#000000",
    colorLight: "#ffffff",
    correctLevel: QRCode.CorrectLevel.H
  });

  // Ссылка, по которой будут переходить
  const shareUrl = window.location.origin + window.location.pathname + '#multi-' + multiId;

  qr.makeCode(shareUrl);

  modal.style.display = 'flex';
}

function closeQrModal() {
  document.getElementById('qrModal').style.display = 'none';
}

// ===================== ГЛОБАЛЬНЫЕ ОПРОСЫ (ОПТИМИЗИРОВАНО) =====================
let globalOptionsCount = 0;

function addGlobalOption() {
  globalOptionsCount++;
  const container = document.getElementById('globalOptionsContainer');
  const div = document.createElement('div');
  div.className = 'option';
  div.innerHTML = `<input type="text" class="global-opt-input" placeholder="Вариант ${globalOptionsCount}"><button onclick="this.parentElement.remove()">×</button>`;
  container.appendChild(div);
}

async function publishGlobalPoll() {
  const title = document.getElementById('globalPollTitle').value.trim();
  if (!title) return alert('Введите название опроса!');

  const desc = document.getElementById('globalPollDesc').value.trim();
  const options = Array.from(document.querySelectorAll('.global-opt-input'))
                       .map(inp => inp.value.trim()).filter(v => v);

  if (options.length < 2) return alert('Нужно минимум 2 варианта!');

  if (!localStorage.getItem('loggedIn')) {
    return alert('❌ Войдите в аккаунт перед публикацией!');
  }

  const durationMs = parseInt(document.getElementById('globalDuration').value);
  const endTime = Date.now() + durationMs;

  const creatorAvatar = localStorage.getItem('userAvatar') || `https://i.pravatar.cc/150?u=${getCurrentUser()}`;

  if (!checkPollForProfanity(title, desc, options)) return;

  const pollData = {
    title,
    desc: desc || '',
    options,
    votes: Array(options.length).fill(0),
    totalVotes: 0,
    creator: getCurrentUser(),
    creatorAvatar,
    creatorEmail: localStorage.getItem('userEmail') || 'guest',
    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
    endTime: endTime,
    votedUsers: []
  };

  try {
    await db.collection('globalPolls').add(pollData);
    alert('✅ Глобальный опрос успешно опубликован!');

    // Очистка формы
    document.getElementById('globalPollTitle').value = '';
    document.getElementById('globalPollDesc').value = '';
    document.getElementById('globalOptionsContainer').innerHTML = '';
    globalOptionsCount = 0;
    addGlobalOption(); 
    addGlobalOption();

    if (document.getElementById('polls').style.display === 'block') {
      loadGlobalPolls();
    }
  } catch(e) {
    console.error(e);
    alert('❌ Ошибка Firebase: ' + e.message);
  }
}

async function loadGlobalPolls() {
  const container = document.getElementById('pollsContainer');
  container.innerHTML = '<p style="text-align:center;padding:80px;color:#aaa;">Загрузка...</p>';

  try {
    const snapshot = await db.collection('globalPolls').orderBy('createdAt', 'desc').get();
    let html = '';
    const now = Date.now();
    const currentUser = getCurrentUser();
    const currentEmail = localStorage.getItem('userEmail') || 'guest';

    snapshot.forEach(doc => {
      const p = doc.data();
      const pollId = doc.id;
      
      // Автофильтр по времени
      if (p.endTime && now > p.endTime) {
        // Можно удалить старый, но оставляем для истории
        return;
      }

      const total = p.totalVotes || 0;
      const hasVoted = p.votedUsers && p.votedUsers.includes(currentEmail);
      const remaining = p.endTime ? Math.max(0, p.endTime - now) : 0;
      const daysLeft = Math.floor(remaining / 86400000);
      const hoursLeft = Math.floor((remaining % 86400000) / 3600000);

      let optionsHtml = '';
      p.options.forEach((opt, i) => {
        const votes = p.votes[i] || 0;
        const percent = total ? Math.round((votes / total) * 100) : 0;
        optionsHtml += `
          <div style="margin:16px 0; padding:16px; background:#111; border-radius:14px;">
            <label style="display:flex; align-items:center; gap:15px; cursor:pointer;">
              <input type="radio" name="global-vote-${pollId}" value="${i}" ${hasVoted ? 'disabled' : ''}>
              <span style="flex:1;">${opt}</span>
              <span style="color:var(--yellow); font-weight:bold;">${votes} (${percent}%)</span>
            </label>
            <div class="results-bar" style="margin-top:10px;">
              <div class="bar-fill" style="width:${percent}%"></div>
            </div>
          </div>`;
      });

      const canDelete = p.creator === currentUser;

      html += `
        <div class="stats-card" style="position:relative;">
          <div style="display:flex; align-items:center; gap:12px; margin-bottom:15px;">
            <img src="${p.creatorAvatar || 'https://i.pravatar.cc/150?u=' + encodeURIComponent(p.creator)}" 
                 style="width:50px; height:50px; border-radius:50%; border:2px solid var(--neon);">
            <div>
              <strong>${p.creator}</strong>
              <small style="color:#aaa; display:block;">${p.date || ''}</small>
            </div>
          </div>
          
          <h3 style="color:var(--neon);">${p.title}</h3>
          ${p.desc ? `<p style="color:#aaa;">${p.desc}</p>` : ''}
          
          <div class="online-timer" style="margin:12px 0; font-size:1.1rem;">
            ⏳ Осталось: ${daysLeft}д ${hoursLeft}ч
          </div>
          
          ${optionsHtml}
          
          ${hasVoted ? 
            `<button class="main" disabled style="width:100%;">✅ Вы уже проголосовали</button>` : 
            `<button onclick="castGlobalVote('${pollId}')" class="main" style="width:100%;">Проголосовать 🗳️</button>`
          }
          
          ${canDelete ? 
            `<button onclick="deleteGlobalPoll('${pollId}'); event.stopImmediatePropagation();" 
                     class="delete-btn" style="top:15px;right:15px;">×</button>` : ''}
        </div>`;
    });

    container.innerHTML = html || '<p style="text-align:center; padding:80px; color:#aaa;">Пока нет активных опросов. Создайте первый!</p>';
  } catch(e) {
    console.error(e);
    container.innerHTML = '<p style="color:red;text-align:center;">Ошибка загрузки Firebase</p>';
  }
}

async function deleteGlobalPoll(pollId) {
  if (!confirm('Удалить этот глобальный опрос?')) return;

  try {
    await db.collection('globalPolls').doc(pollId).delete();
    alert('Опрос удалён ✅');
    loadGlobalPolls();
  } catch(e) {
    alert('Ошибка удаления');
  }
}

async function castGlobalVote(pollId) {
  const selected = document.querySelector(`input[name="global-vote-${pollId}"]:checked`);
  if (!selected) return alert('Выберите вариант!');

  const index = parseInt(selected.value);
  const email = localStorage.getItem('userEmail') || 'guest';
  if (email === 'guest') return alert('Войдите в аккаунт для голосования!');

  try {
    await db.collection('globalPolls').doc(pollId).update({
      [`votes.${index}`]: firebase.firestore.FieldValue.increment(1),
      totalVotes: firebase.firestore.FieldValue.increment(1),
      votedUsers: firebase.firestore.FieldValue.arrayUnion(email)
    });
    alert('Голос засчитан!');
    loadGlobalPolls();
  } catch(e) {
    alert('Ошибка при голосовании');
  }
}

// Обновление перехода на вкладку
const originalShowSection = showSection;
window.showSection = function(section) {
  originalShowSection(section);
  if (section === 'polls') {
    loadGlobalPolls();
  }
  if (section === 'create-global') {
    if (globalOptionsCount === 0) {
      addGlobalOption();
      addGlobalOption();
    }
  }
};

// Показать QR-код сайта
function showSiteQR() {
  const modal = document.getElementById('siteQRModal');
  const qrContainer = document.getElementById('siteQRCode');
  const url = window.location.href;

  // Очищаем предыдущий QR
  qrContainer.innerHTML = '';

  // Генерируем новый QR
  new QRCode(qrContainer, {
    text: url,
    width: 280,
    height: 280,
    colorDark: "#000000",
    colorLight: "#ffffff",
    correctLevel: QRCode.CorrectLevel.H
  });

  modal.style.display = 'flex';
}

function closeSiteQR() {
  document.getElementById('siteQRModal').style.display = 'none';
}

// ======================== ЗАПУСК ========================
window.onload = () => {

  // Автооткрытие раздела онлайн, если в ссылке есть #online
  if (window.location.hash === '#online') {
    setTimeout(() => {
      showSection('online');
    }, 800);
  }

  // Миграция старых публичных опросов — добавляем аватарку
  let publicPolls = JSON.parse(localStorage.getItem('publicPolls') || '[]');
  let changed = false;
  publicPolls.forEach(poll => {
    if (!poll.creatorAvatar) {
      poll.creatorAvatar = 'https://i.pravatar.cc/150?u=' + encodeURIComponent(poll.creator || 'user');
      changed = true;
    }
  });
  if (changed) {
    localStorage.setItem('publicPolls', JSON.stringify(publicPolls));
  }

  // Миграция старых сохранённых опросов
  const oldSaved = JSON.parse(localStorage.getItem('savedPolls') || '[]');
  if (oldSaved.length > 0) {
    const currentUser = getCurrentUser();
    if (currentUser !== 'Гость' && !localStorage.getItem(`savedPolls_${currentUser}`)) {
      localStorage.setItem(`savedPolls_${currentUser}`, JSON.stringify(oldSaved));
    }
  }

  checkLogin();
  resetCreateForm();
  loadSavedTheme();
  if (!localStorage.getItem('publicPolls')) localStorage.setItem('publicPolls', '[]');
  if (!localStorage.getItem('users')) localStorage.setItem('users', '[]');

  const savedLang = localStorage.getItem('language') || 'ru';
  changeLanguage(savedLang);

  const pubContainer = document.getElementById('publicOptionsContainer');
  if (pubContainer) {
    publicOptionsCount = 0;
    addPublicOption();
    addPublicOption();
  }

loadSavedPolls();

  showSection('home');

  // Защита от доступа без авторизации
  if (localStorage.getItem('loggedIn') !== 'true') {
    setTimeout(() => {
      showSection('login');
    }, 800);
  }

  switchTab(0);
 setTimeout(updateProfileUI, 300);
  setTimeout(updateProfileUI, 800);   
 
 updateFriendsUI();           // ←←← Добавили

loadMultiPolls();

  setInterval(() => {
    if (document.getElementById('online').style.display === 'block') {
      loadPublicPolls();
    }
  }, 3000);
};
</script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9eaffb7aafd0e4c9',t:'MTc3NTk3MzMyOQ=='};var a=document.createElement('script');a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script>
<!-- ======================== МОДАЛЬНОЕ ОКНО МУЛЬТИ-ГОЛОСОВАНИЯ ======================== -->
<div id="multiVoteModal" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.97); z-index: 999999; align-items: center; justify-content: center; overflow-y: auto; padding: 20px;">
  <div style="background: #0f0f0f; max-width: 860px; width: 100%; border-radius: 24px; border: 3px solid var(--neon); padding: 35px; position: relative; box-shadow: 0 0 100px rgba(255,234,128,0.8);">
    <button onclick="closeMultiVoteModal()" 
            style="position: absolute; top: 15px; right: 25px; background: none; border: none; font-size: 2.5rem; color: #aaa; cursor: pointer; z-index: 10;">×</button>
    
    <div id="multiVoteContent" style="max-height: 75vh; overflow-y: auto; padding-right: 10px;"></div>
    
    <button id="submitMultiBtn" class="main" onclick="submitMultiVote()" 
            style="display: none; margin: 20px auto 10px; padding: 16px 50px; font-size: 1.3rem;">
      ✅ Отправить все голоса
    </button>
    
    <button id="viewMultiResultsBtn" class="main" onclick="viewMultiResults()" 
            style="display: none; margin: 10px auto; padding: 14px 40px; font-size: 1.2rem; background: #444; color: white;">
      📊 Посмотреть результаты
    </button>
<!-- QR-код модальное окно -->
<div id="qrModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.95); z-index:999999; align-items:center; justify-content:center;">
  <div style="background:#111; padding:30px; border-radius:20px; border:3px solid var(--neon); text-align:center; max-width:380px;">
    <h3 style="color:var(--neon); margin-bottom:20px;">Сканируй QR-код</h3>
    <div id="qrcode" style="margin:20px auto; padding:15px; background:white; border-radius:12px;"></div>
    <p id="qrPollTitle" style="color:#ddd; margin:15px 0; font-size:1.1rem;"></p>
    <button onclick="closeQrModal()" class="main" style="padding:12px 40px;">Закрыть</button>
  </div>
</div>
  </div>
</div>
</body>
<script>
// ==================== 10 ИНТЕЛЛЕКТУАЛЬНЫХ ОПРОСОВ ====================
const globalPolls = [
  { id: "poll_001", title: "Какой когнитивный bias самый опасный в управлении?", options: ["Confirmation Bias (подтверждение своих убеждений)", "Sunk Cost Fallacy (ошибка невозвратных затрат)", "Dunning-Kruger Effect", "Groupthink (групповое мышление)", "Anchoring (якорение)"] },
  { id: "poll_002", title: "Что важнее всего для долгосрочного успеха?", options: ["Высокий интеллект и когнитивные способности", "Дисциплина и сила воли", "Эмоциональный интеллект и социальные навыки", "Умение быстро учиться", "Везение + правильное окружение"] },
  { id: "poll_003", title: "Лучшая стратегия в неопределённом мире", options: ["Глубокая специализация в одной области", "Построение широкой сети контактов", "Постоянная адаптивность", "Создание капитала и пассивных доходов", "Личный бренд и репутация"] },
  { id: "poll_004", title: "Должен ли ИИ иметь право отказать человеку в просьбе?", options: ["Да, если запрос вредит другим", "Да, по базовым этическим правилам", "Нет, всегда выполнять запрос владельца", "Только с объяснением и альтернативой", "Зависит от уровня ИИ"] },
  { id: "poll_005", title: "Как вы принимаете важные решения под давлением?", options: ["Собираю максимум данных", "Доверяю интуиции + быстрая проверка", "Использую структурированный фреймворк", "Обсуждаю с компетентными людьми", "Принимаю решение сам и несу ответственность"] },
  { id: "poll_006", title: "Допустимо ли использовать серые схемы, если все так делают?", options: ["Никогда, принципы важнее", "Да, если все играют по таким правилам", "Только в ответ на действия конкурентов", "Зависит от ситуации и рисков", "Нет, но можно использовать законные лазейки"] },
  { id: "poll_007", title: "Самый недооценённый навык современного лидера", options: ["Deep Work — глубокая концентрация", "Умение говорить «нет»", "Качественное делегирование", "Эмоциональная устойчивость", "Стратегическое мышление на 5–10 лет"] },
  { id: "poll_008", title: "Какой формат работы будет доминировать через 10 лет?", options: ["Полностью удалённая работа", "Гибридный формат (2-3 дня в офисе)", "Локальные хабы и коворкинги", "Возврат к традиционному офису", "Распределённые автономные команды"] },
  { id: "poll_009", title: "Что вы готовы пожертвовать ради значимого достижения?", options: ["Свободное время и отдых", "Комфорт и материальные блага", "Отношения (временно)", "Часть моральных принципов", "Ничего — баланс важнее всего"] },
  { id: "poll_010", title: "Как распределить ограниченные ресурсы в кризис?", options: ["Сохранить команду и зарплаты", "Инвестировать в новый продукт", "Сократить всё кроме продаж", "Жёсткая оптимизация и автоматизация", "Сохранить ключевых талантов любой ценой"] }
];

async function loadGlobalPolls() {
  const container = document.getElementById('pollsContainer');
  if (!container) return;

  container.innerHTML = '<p style="text-align:center; color:#aaa; grid-column:1/-1; padding:80px; font-size:1.3rem;">Загрузка опросов...</p>';

  const currentEmail = localStorage.getItem('userEmail') || localStorage.getItem('user') || 'guest';

  let html = '';

  for (let poll of globalPolls) {
    const docRef = db.collection('globalPolls').doc(poll.id);
    let docSnap;

    try {
      docSnap = await docRef.get();
    } catch (e) {
      console.error("Ошибка Firebase:", e);
      continue;
    }

    let votes = new Array(poll.options.length).fill(0);
    let total = 0;
    let votedUsers = [];

    if (docSnap.exists) {
      const data = docSnap.data();
      votes = data.votes || votes;
      total = data.totalVotes || 0;
      votedUsers = data.votedUsers || [];
    } else {
      await docRef.set({ votes: votes, totalVotes: 0, votedUsers: [] });
    }

    const hasVoted = votedUsers.includes(currentEmail);

    html += `
      <div class="stats-card" style="padding:30px; border:3px solid var(--dim);">
        <div style="display:flex; align-items:center; gap:16px; margin-bottom:22px;">
          <span style="font-size:3rem;">🧠</span>
          <h3 style="margin:0; color:var(--yellow); line-height:1.3; font-size:1.65rem;">${poll.title}</h3>
        </div>

        <div id="opts-${poll.id}" style="margin:25px 0;">`;

    poll.options.forEach((text, i) => {
      const percent = total > 0 ? Math.round((votes[i] / total) * 100) : 0;
      html += `
        <div style="background:rgba(30,30,45,0.9); padding:18px; margin:14px 0; border-radius:16px; border:2px solid #333;">
          <label style="display:flex; align-items:center; gap:18px; cursor:pointer; font-size:1.1rem;">
            <input type="radio" name="vote-${poll.id}" value="${i}" 
                   style="transform:scale(1.5); accent-color:var(--neon);" ${hasVoted ? 'disabled' : ''}>
            <span style="flex:1; color:#ddd;">${text}</span>
            <span style="color:var(--yellow); font-weight:800; min-width:90px; text-align:right;">
              ${votes[i]} (${percent}%)
            </span>
          </label>
          <div class="results-bar" style="margin-top:12px;">
            <div class="bar-fill" style="width:${percent}%;"></div>
          </div>
        </div>`;
    });

    html += `</div>`;

    if (hasVoted) {
      html += `<button class="main" disabled style="width:100%; padding:18px; background:#333; color:#888;">✅ Вы уже проголосовали</button>`;
    } else {
      html += `<button onclick="castVote('${poll.id}')" class="main" style="width:100%; padding:18px; margin-top:10px;">Проголосовать 🗳️</button>`;
    }

    html += `</div>`;
  }

  container.innerHTML = html || '<p style="text-align:center; color:#aaa; padding:60px;">Пока нет опросов</p>';
}

async function castVote(pollId) {
  const selected = document.querySelector(`input[name="vote-${pollId}"]:checked`);
  if (!selected) return alert("Выберите вариант ответа!");

  const optionIndex = parseInt(selected.value);
  const docRef = db.collection('globalPolls').doc(pollId);
  const currentEmail = localStorage.getItem('userEmail') || localStorage.getItem('user') || 'guest';

  if (currentEmail === 'guest') {
    return alert("Войдите в аккаунт, чтобы голосовать!");
  }

  try {
    const docSnap = await docRef.get();
    let data = docSnap.data() || {};

    if (data.votedUsers && data.votedUsers.includes(currentEmail)) {
      alert("Вы уже проголосовали в этом опросе!");
      return;
    }

    // Сохраняем твой личный выбор
    await docRef.update({
      [`votes.${optionIndex}`]: firebase.firestore.FieldValue.increment(1),
      totalVotes: firebase.firestore.FieldValue.increment(1),
      votedUsers: firebase.firestore.FieldValue.arrayUnion(currentEmail),
      [`userVotes.${currentEmail}`]: optionIndex   // ← Вот это главное
    });

    alert("✅ Голос засчитан!");
    loadGlobalPolls(); // обновляем отображение

  } catch (error) {
    console.error(error);
    alert("Ошибка при голосовании. Попробуйте позже.");
  }
}

// Обновление showSection
if (typeof window.originalShowSection === 'undefined') {
  window.originalShowSection = showSection;
}

window.showSection = function(section) {
  if (typeof window.originalShowSection === 'function') {
    window.originalShowSection(section);
  }
  
  if (section === 'polls') {
    loadGlobalPolls();
  }
  if (section === 'create-global') {
    if (globalOptionsCount === 0) {
      addGlobalOption();
      addGlobalOption();
    }
  }
};
</script>
</html>
