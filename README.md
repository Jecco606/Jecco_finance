<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<title>Jecco Finance</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {font-family: Arial; background:#f5f5f5; margin:0;}
.container {max-width:900px; margin:auto; padding:20px;}
.card {background:white; padding:15px; margin:10px 0; border-radius:10px;}
button {padding:10px; background:orange; border:none; color:white; cursor:pointer;}
input {padding:8px; margin:5px;}
</style>
</head>

<body>

<div class="container" id="login">
  <h2>🔐 Jecco Finance Login</h2>
  <input type="text" id="user" placeholder="Username"><br>
  <input type="password" id="pass" placeholder="Parol"><br>
  <button onclick="login()">Kirish</button>
</div>

<div class="container" id="app" style="display:none;">
  <h2>💰 Jecco Finance</h2>

  <div class="card">
    <input type="number" id="amount" placeholder="Summa">
    <input type="text" id="desc" placeholder="Izoh">
    <button onclick="addIncome()">➕ Kirim</button>
    <button onclick="addExpense()">➖ Chiqim</button>
  </div>

  <div class="card">
    <h3>Balans: <span id="balance">0</span> so‘m</h3>
  </div>

  <div class="card">
    <canvas id="chart"></canvas>
  </div>

  <div class="card">
    <h3>Tarix</h3>
    <ul id="list"></ul>
  </div>
</div>

<script>
let data = JSON.parse(localStorage.getItem("data")) || [];

function login() {
  let u = document.getElementById("user").value;
  let p = document.getElementById("pass").value;

  if (u === "Jecco_1" && p === "Javohir123") {
    document.getElementById("login").style.display = "none";
    document.getElementById("app").style.display = "block";
    render();
  } else {
    alert("Xato login!");
  }
}

function addIncome() {
  let amount = Number(document.getElementById("amount").value);
  let desc = document.getElementById("desc").value;
  data.push({amount, desc, type:"income"});
  save();
}

function addExpense() {
  let amount = Number(document.getElementById("amount").value);
  let desc = document.getElementById("desc").value;
  data.push({amount, desc, type:"expense"});
  save();
}

function save() {
  localStorage.setItem("data", JSON.stringify(data));
  render();
}

function render() {
  let list = document.getElementById("list");
  list.innerHTML = "";

  let balance = 0, income = 0, expense = 0;

  data.forEach(item => {
    let li = document.createElement("li");
    li.textContent = item.desc + " - " + item.amount;
    list.appendChild(li);

    if (item.type === "income") {
      balance += item.amount;
      income += item.amount;
    } else {
      balance -= item.amount;
      expense += item.amount;
    }
  });

  document.getElementById("balance").textContent = balance;

  drawChart(income, expense);
}

function drawChart(income, expense) {
  let ctx = document.getElementById("chart").getContext("2d");

  new Chart(ctx, {
    type: "pie",
    data: {
      labels: ["Kirim", "Chiqim"],
      datasets: [{
        data: [income, expense]
      }]
    }
  });
}
</script>

</body>
</html>
