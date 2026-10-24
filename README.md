<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LapTech — Выборы</title>
<style>
:root {
  --bg: #100d18;
  --card: #20182e;
  --accent: #a855f7;
  --text: #f7f2ff;
  --muted: #b9accb;
  --border: #3c2b50;
}
* { box-sizing: border-box; }
body {
  margin: 0;
  background: radial-gradient(ellipse at top, #32194d, var(--bg) 65%);
  color: var(--text);
  font-family: Arial, sans-serif;
  min-height: 100vh;
}
header {
  text-align: center;
  padding: 28px 15px;
  border-bottom: 1px solid var(--border);
}
.logo { color: #c084fc; letter-spacing: 5px; font-weight: bold; }
h1 { margin: 12px 0 6px; }
.sub, .small { color: var(--muted); }
main { max-width: 1100px; padding: 20px 14px; margin: auto; }
.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-bottom: 20px;
}
.stat, .panel, .party {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 15px;
}
.stat { padding: 15px 5px; text-align: center; min-width: 0; }
.stat strong { display: block; color: #d8b4fe; font-size: 23px; overflow-wrap: anywhere; }
.stat span { color: var(--muted); font-size: 12px; }
.panel { padding: 17px; margin-bottom: 20px; }
h2 { font-size: 20px; margin-top: 0; }
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 270px), 1fr));
  gap: 14px;
}
.party { padding: 16px; min-width: 0; }
.party-head { display: flex; align-items: center; gap: 12px; }
.symbol {
  display: grid;
  place-items: center;
  width: 55px;
  height: 55px;
  flex-shrink: 0;
  border-radius: 13px;
  background: #38234e;
  font-size: 26px;
}
.party h3 { overflow-wrap: anywhere; }
.tag { color: #d8b4fe; font-size: 12px; }
.blocked { color: #ff7272; font-weight: bold; font-size: 12px; }
.count { font-size: 26px; font-weight: bold; margin-top: 14px; overflow-wrap: anywhere; }
input[type=text], input[type=number] {
  width: 100%;
  padding: 12px;
  margin: 7px 0;
  border: 1px solid #59436d;
  border-radius: 9px;
  background: #110d19;
  color: white;
}
button {
  cursor: pointer;
  border: 0;
  border-radius: 9px;
  padding: 11px 13px;
  font-weight: bold;
}
button:disabled { opacity: .4; cursor: not-allowed; }
.primary {
  width: 100%;
  margin-top: 8px;
  background: linear-gradient(100deg, #9333ea, #c084fc);
  color: white;
}
.secondary { background: #39264e; color: white; }
.danger { background: #4b202d; color: #ffc2cb; }
.actions { display: flex; gap: 7px; margin-top: 10px; }
.actions button { flex: 1; font-size: 12px; }
.progress {
  height: 7px;
  background: #110d19;
  border-radius: 10px;
  overflow: hidden;
  margin: 9px 0;
}
.progress div {
  height: 100%;
  background: linear-gradient(90deg, #9333ea, #d8b4fe);
}
.barline { display: flex; justify-content: space-between; gap: 8px; font-size: 13px; margin-top: 13px; }
.notice { background: #302039; border-radius: 9px; padding: 12px; font-size: 13px; }
.tablewrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; font-size: 13px; }
th, td { text-align: left; padding: 11px 7px; border-bottom: 1px solid var(--border); }
th { color: #d8b4fe; }
td:last-child, th:last-child { text-align: right; }
footer { text-align: center; color: var(--muted); padding: 25px; font-size: 12px; }
@media(max-width:450px) {
  h1 { font-size: 27px; }
  .panel { padding: 13px; }
  .stats { gap: 6px; }
  .stat strong { font-size: 18px; }
}
</style>
</head>
<body>

<header>
  <div class="logo">LAPTECH</div>
  <h1>🗳️ Выборы</h1>
  <p class="sub">Симулятор голосования за политические партии</p>
</header>

<main>
  <div class="stats">
    <div class="stat">
      <strong id="total">0</strong>
      <span>Всего голосов</span>
    </div>
    <div class="stat">
      <strong id="partyCount">8</strong>
      <span>Партий</span>
    </div>
    <div class="stat">
      <strong id="leader">—</strong>
      <span>Лидер</span>
    </div>
  </div>

  <section class="panel">
    <h2>📊 Результаты выборов</h2>
    <div id="results"></div>
    <p class="small">Демонстрационный подсчёт в этом браузере.</p>
  </section>

  <section class="panel">
    <h2>➕ Добавить свою партию</h2>
    <p class="small">До 10 собственных партий. Названия не должны повторяться.</p>
    <input id="newName" type="text" maxlength="60" placeholder="Название партии">
    <input id="newSymbol" type="text" maxlength="4" placeholder="Эмодзи, например ⭐">
    <button class="primary" onclick="addParty()">Создать партию</button>
    <p class="small" id="customCount">0 из 10 собственных партий</p>
    <p id="message" class="small" role="status"></p>
  </section>

  <h2>🏛️ Политические партии</h2>
  <div id="parties" class="grid"></div>

  <section class="panel" style="margin-top:22px">
    <h2>📋 Сводка голосов</h2>
    <div class="tablewrap">
      <table>
        <thead>
          <tr><th>Партия</th><th>Голоса</th><th>Доля</th></tr>
        </thead>
        <tbody id="summary"></tbody>
      </table>
    </div>
  </section>

  <section class="panel">
    <h2>⚙️ Управление</h2>
    <p class="small">Сбросит результаты и созданные партии в этом браузере.</p>
    <button class="danger" onclick="resetAll()">Сбросить данные</button>
  </section>
</main>

<footer>LapTech · Простые сайты, интересные игры</footer>

<script>
const MAX_VOTES = 100000000;
const MAX_CUSTOM = 10;

const defaults = [
  {id:"er",name:"Единая Россия",symbol:"🐻",votes:0},
  {id:"kprf",name:"КПРФ",symbol:"⭐",votes:0},
  {id:"sr",name:"Справедливая Россия",symbol:"⚖️",votes:0},
  {id:"ldpr",name:"ЛДПР",symbol:"🔷",votes:0},
  {id:"yabloko",name:"Яблоко",symbol:"🍏",votes:0,blocked:true},
  {id:"newpeople",name:"Новые Люди",symbol:"🟣",votes:0},
  {id:"green",name:"Зелёные",symbol:"🌿",votes:0},
  {id:"other",name:"Другая партия",symbol:"🏛️",votes:0}
];

function loadParties() {
  try {
    const data = JSON.parse(localStorage.getItem("laptechElection"));
    if (Array.isArray(data) && data.length) return data;
  } catch (e) {}
  return defaults.map(p => ({...p}));
}

let parties = loadParties();

function save() {
  try {
    localStorage.setItem("laptechElection", JSON.stringify(parties));
  } catch (e) {
    showMessage("Не удалось сохранить данные браузера.");
  }
}

function esc(value) {
  return String(value).replace(/[&<>"']/g, c => ({
    "&":"&amp;", "<":"&lt;", ">":"&gt;",
    '"':"&quot;", "'":"&#39;"
  })[c]);
}

function showMessage(text) {
  document.getElementById("message").textContent = text;
}

function totalVotes() {
  return parties.reduce((sum, p) => sum + (Number(p.votes) || 0), 0);
}

function addParty() {
  const name = document.getElementById("newName").value.trim();
  const symbol = document.getElementById("newSymbol").value.trim() || "🗳️";

  if (!name) return showMessage("Введи название партии.");

  if (parties.some(p => p.name.trim().toLocaleLowerCase("ru") === name.toLocaleLowerCase("ru"))) {
    return showMessage("Партия с таким названием уже существует.");
  }

  if (parties.filter(p => p.custom).length >= MAX_CUSTOM) {
    return showMessage("Максимум 10 собственных партий.");
  }

  parties.push({
    id: "custom_" + Date.now(),
    name,
    symbol,
    votes: 0,
    custom: true
  });

  document.getElementById("newName").value = "";
  document.getElementById("newSymbol").value = "";
  showMessage("Партия создана!");
  save();
  render();
}

function vote(id) {
  const party = parties.find(p => p.id === id);
  if (!party || party.blocked) return;

  const input = document.getElementById("amount_" + id);
  const amount = Number(input.value);

  if (!Number.isSafeInteger(amount) || amount < 1 || amount > MAX_VOTES) {
    alert("Введи целое число от 1 до 100 000 000.");
    return;
  }

  if (party.votes + amount > MAX_VOTES) {
    alert("Достигнут лимит 100 000 000 голосов для этой партии.");
    return;
  }

  party.votes += amount;
  save();
  render();
}

function renameParty(id) {
  const party = parties.find(p => p.id === id);
  if (!party || !party.custom) return;

  const result = prompt("Новое название партии:", party.name);
  if (result === null) return;

  const name = result.trim();
  if (!name) return alert("Название не может быть пустым.");

  if (parties.some(p => p.id !== id &&
      p.name.trim().toLocaleLowerCase("ru") === name.toLocaleLowerCase("ru"))) {
    return alert("Такое название уже существует.");
  }

  party.name = name;
  save();
  render();
}

function deleteParty(id) {
  const party = parties.find(p => p.id === id);
  if (!party || !party.custom) return;

  if (!confirm('Удалить партию "' + party.name + '"?')) return;

  parties = parties.filter(p => p.id !== id);
  save();
  render();
}

function resetAll() {
  if (!confirm("Сбросить голоса и удалить собственные партии?")) return;
  parties = defaults.map(p => ({...p}));
  save();
  render();
  showMessage("Данные сброшены.");
}

function render() {
  const total = totalVotes();

  document.getElementById("total").textContent = total.toLocaleString("ru-RU");
  document.getElementById("partyCount").textContent = parties.length;

  const eligible = parties.filter(p => !p.blocked);
  const leader = [...eligible].sort((a,b) => b.votes - a.votes)[0];
  document.getElementById("leader").textContent =
    leader && leader.votes ? leader.name : "—";

  document.getElementById("customCount").textContent =
    "Собственных партий: " + parties.filter(p => p.custom).length + " из 10";

  document.getElementById("parties").innerHTML = parties.map(p => {
    const share = total ? p.votes / total * 100 : 0;

    return `
      <article class="party">
        <div class="party-head">
          <div class="symbol">${esc(p.symbol || "🗳️")}</div>
          <div>
            <h3>${esc(p.name)}</h3>
            <div class="tag">${p.custom ? "Собственная партия" : "Политическая партия"}</div>
            ${p.blocked ? '<div class="blocked">❌ Партия не прошла</div>' : ""}
          </div>
        </div>

        <div class="count">${Number(p.votes).toLocaleString("ru-RU")}</div>
        <div class="small">голосов · ${share.toFixed(2)}%</div>
        <div class="progress"><div style="width:${Math.min(share,100)}%"></div></div>

        ${p.blocked ? `
          <div class="notice">Голосование за эту партию заблокировано.</div>
        ` : `
          <label class="small" for="amount_${p.id}">Количество голосов</label>
          <input type="number" id="amount_${p.id}" min="1" max="${MAX_VOTES}" step="1" value="1">
          <button class="primary" onclick="vote('${p.id}')">🗳️ Проголосовать</button>
        `}

        ${p.custom ? `
          <div class="actions">
            <button class="secondary" onclick="renameParty('${p.id}')">✏️ Переименовать</button>
            <button class="danger" onclick="deleteParty('${p.id}')">🗑️ Удалить</button>
          </div>
        ` : ""}
      </article>
    `;
  }).join("");

  document.getElementById("results").innerHTML =
    [...parties].sort((a,b) => b.votes - a.votes).map(p => {
      const share = total ? p.votes / total * 100 : 0;
      return `
        <div class="barline">
          <span>${esc(p.symbol || "")} ${esc(p.name)}</span>
          <strong>${share.toFixed(2)}%</strong>
        </div>
        <div class="progress"><div style="width:${Math.min(share,100)}%"></div></div>
      `;
    }).join("");

  document.getElementById("summary").innerHTML =
    [...parties].sort((a,b) => b.votes - a.votes).map(p => {
      const share = total ? p.votes / total * 100 : 0;
      return `
        <tr>
          <td>${esc(p.symbol || "")} ${esc(p.name)}${p.blocked ? " ❌" : ""}</td>
          <td>${Number(p.votes).toLocaleString("ru-RU")}</td>
          <td>${share.toFixed(2)}%</td>
        </tr>
      `;
    }).join("");
}

render();
</script>
</body>
</html>
