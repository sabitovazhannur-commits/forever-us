<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Forever Keyed Memories</title>
<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #ffb6d5, #ff7eb9);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.page {
  display: none;
  text-align: center;
  width: 90%;
  max-width: 400px;
  animation: fade 0.5s ease;
}

.page.active {
  display: block;
}

@keyframes fade {
  from {opacity: 0; transform: translateY(10px);}
  to {opacity: 1; transform: translateY(0);}
}

input, textarea {
  width: 100%;
  padding: 12px;
  margin-top: 10px;
  border-radius: 12px;
  border: none;
  outline: none;
  font-size: 16px;
}

button {
  margin-top: 20px;
  padding: 12px 25px;
  background: #ff5fa2;
  border: none;
  border-radius: 25px;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #ff3b8d;
}

.code-inputs {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.code-inputs input {
  width: 60px;
  text-align: center;
  font-size: 24px;
}

img {
  max-width: 100%;
  margin-top: 15px;
  border-radius: 15px;
}
</style>
</head>
<body>

<!-- СОЗДАНИЕ КОДА -->
<div class="page active" id="createPage">
  <h2>Создай код 💖</h2>
  <input id="newCode" maxlength="4" placeholder="Придумай 4 цифры">
  <button onclick="saveCode()">Сохранить код</button>
</div>

<!-- ВВОД КОДА -->
<div class="page" id="loginPage">
  <h2>Введите код 💗</h2>
  <div class="code-inputs">
    <input maxlength="1">
    <input maxlength="1">
    <input maxlength="1">
    <input maxlength="1">
  </div>
  <button onclick="checkCode()">Далее</button>
</div>

<!-- 2 СТРАНИЦА -->
<div class="page" id="page2">
  <h2>Одно слово ✨</h2>
  <input id="oneWord" placeholder="Напиши одно слово">
  <button onclick="goToPage('page3')">Продолжить</button>
</div>

<!-- 3 СТРАНИЦА -->
<div class="page" id="page3">
  <h2>Твое сообщение 💖</h2>
  <textarea id="fullMessage" rows="5" placeholder="Напиши полный текст..."></textarea>
  <input type="file" accept="image/*" onchange="previewImage(event)">
  <img id="preview">
  <button onclick="submitMessage()">Отправить 💌</button>
</div>

<!-- ФИНАЛ -->
<div class="page" id="page4">
  <h2>Спасибо 💞</h2>
  <p id="finalText"></p>
</div>

<script>
const codeInputs = document.querySelectorAll(".code-inputs input");

window.onload = function() {
  const savedCode = localStorage.getItem("secretCode");
  if (savedCode) {
    goToPage("loginPage");
  }
};

function saveCode() {
  const newCode = document.getElementById("newCode").value;

  if (newCode.length !== 4 || isNaN(newCode)) {
    alert("Код должен состоять из 4 цифр 💕");
    return;
  }

  localStorage.setItem("secretCode", newCode);
  alert("Код сохранен 💖");
  goToPage("loginPage");
}

codeInputs.forEach((input, index) => {
  input.addEventListener("input", () => {
    if (input.value.length === 1 && index < codeInputs.length - 1) {
      codeInputs[index + 1].focus();
    }
  });
});

function checkCode() {
  let enteredCode = "";
  codeInputs.forEach(input => enteredCode += input.value);

  const savedCode = localStorage.getItem("secretCode");

  if (enteredCode === savedCode) {
    goToPage("page2");
  } else {
    alert("Неверный код 💔");
  }
}

function goToPage(pageId) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById(pageId).classList.add('active');
}

function previewImage(event) {
  const img = document.getElementById('preview');
  img.src = URL.createObjectURL(event.target.files[0]);
}

function submitMessage() {
  const word = document.getElementById("oneWord").value;
  const message = document.getElementById("fullMessage").value;

  if (word.trim() === "" || message.trim() === "") {
    alert("Заполни все поля 💕");
    return;
  }

  document.getElementById("finalText").innerHTML =
    "Твое слово: <b>" + word + "</b><br><br>Сообщение:<br>" + message;

  goToPage("page4");
}
</script>

</body>
</html>
