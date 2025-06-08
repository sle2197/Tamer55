# Tamer55<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>جدول مواعيد صالون الحلاقة</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1 id="title">جدول مواعيد الصالون</h1>

  <div id="lang-switch">
    <button onclick="setLanguage('ar')">🇸🇦 عربي</button>
    <button onclick="setLanguage('de')">🇩🇪 Deutsch</button>
  </div>

  <div id="login">
    <input type="password" id="password" placeholder="كلمة مرور المدير">
    <button onclick="login()">تسجيل الدخول</button>
  </div>

  <div id="schedule-container"></div>

  <script src="script.js"></script>
</body>
</html>
   


body {
  font-family: sans-serif;
  direction: rtl;
  padding: 20px;
  background-color: #f9f9f9;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

td, th {
  border: 1px solid #aaa;
  padding: 5px;
  text-align: center;
}

#lang-switch {
  margin-bottom: 10px;
}

button {
  margin: 5px;
  padding: 5px 10px;
  cursor: pointer;
}

input {
  padding: 5px;
}




const employees = ["تمر", "سليمان", "سرخو", "غمكين", "عبود", "فرهاد", "رنكين"];
const startHour = 9;
const endHour = 19;
const intervalMinutes = 15;
let isAdmin = false;
let currentLang = "ar";

const translations = {
  ar: {
    title: "جدول مواعيد الصالون",
    passwordPlaceholder: "كلمة مرور المدير",
    loginButton: "تسجيل الدخول",
    addAppointment: "إضافة موعد"
  },
  de: {
    title: "Friseur-Terminplan",
    passwordPlaceholder: "Manager-Passwort",
    loginButton: "Anmelden",
    addAppointment: "Termin hinzufügen"
  }
};

function generateSchedule() {
  const container = document.getElementById("schedule-container");
  container.innerHTML = "";

  const table = document.createElement("table");

  // Create header row
  const headerRow = document.createElement("tr");
  const timeHeader = document.createElement("th");
  timeHeader.textContent = currentLang === "ar" ? "الوقت" : "Uhrzeit";
  headerRow.appendChild(timeHeader);

  for (const emp of employees) {
    const th = document.createElement("th");
    th.textContent = emp;
    headerRow.appendChild(th);
  }

  table.appendChild(headerRow);

  for (let hour = startHour; hour < endHour; hour++) {
    for (let min = 0; min < 60; min += intervalMinutes) {
      const timeLabel = `${hour.toString().padStart(2, '0')}:${min.toString().padStart(2, '0')}`;
      const row = document.createElement("tr");
      const timeCell = document.createElement("td");
      timeCell.textContent = timeLabel;
      row.appendChild(timeCell);

      for (let i = 0; i < employees.length; i++) {
        const cell = document.createElement("td");
        if (isAdmin) {
          cell.addEventListener("click", () => {
            const current = cell.textContent;
            cell.textContent = current ? "" : "✓";
          });
        }
        row.appendChild(cell);
      }

      table.appendChild(row);
    }
  }

  container.appendChild(table);
}

function login() {
  const password = document.getElementById("password").value;
  if (password === "admin123") {
    isAdmin = true;
    alert(currentLang === "ar" ? "تم الدخول كمدير" : "Als Manager angemeldet");
    generateSchedule();
  } else {
    alert(currentLang === "ar" ? "كلمة مرور غير صحيحة" : "Falsches Passwort");
  }
}

function setLanguage(lang) {
  currentLang = lang;
  document.documentElement.lang = lang;
  document.documentElement.dir = lang === "ar" ? "rtl" : "ltr";

  document.getElementById("title").textContent = translations[lang].title;
  document.getElementById("password").placeholder = translations[lang].passwordPlaceholder;
  document.querySelector("#login button").textContent = translations[lang].loginButton;

  generateSchedule();
}

window.onload = () => {
  setLanguage("ar");
};

