<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FIND X</title>

<style>
:root{
  --bg:#ffffff;
  --text:#000;
  --card:#f1f1f1;
}
.dark{
  --bg:#121212;
  --text:#fff;
  --card:#1e1e1e;
}
body{
  margin:0;
  font-family:Arial;
  background:var(--bg);
  color:var(--text);
}
header{
  padding:12px;
  display:flex;
  justify-content:space-between;
  background:#2e7d32;
  color:white;
}
button{
  padding:8px;
  margin-top:10px;
  width:100%;
}
.card{
  background:var(--card);
  padding:15px;
  margin:10px;
  border-radius:8px;
}
.tab{
  display:none;
}
.tab.active{
  display:block;
}
nav{
  position:fixed;
  bottom:0;
  width:100%;
  display:flex;
  background:#2e7d32;
}
nav button{
  flex:1;
  border:none;
  background:none;
  color:white;
}
input[type=range]{
  width:100%;
}
</style>
</head>

<body>

<header>
  <b>FIND X</b>
  <button onclick="toggleDark()">🌙</button>
</header>

<!-- BASIC -->
<div id="basic" class="tab active card">
<h3>BASIC CALCULATOR</h3>
<input id="expr" placeholder="25*4+10">
<button onclick="basicCalc()">CALCULATE</button>
<p id="basicOut"></p>
</div>

<!-- SIP -->
<div id="sip" class="tab card">
<h3>SIP CALCULATOR</h3>

<label>Monthly SIP: ₹<span id="sipV">10000</span></label>
<input type="range" min="500" max="100000" value="10000" oninput="sipV.innerText=this.value">

<label>Return %: <span id="rateV">12</span>%</label>
<input type="range" min="1" max="20" value="12" oninput="rateV.innerText=this.value">

<label>Years: <span id="yearV">10</span></label>
<input type="range" min="1" max="30" value="10" oninput="yearV.innerText=this.value">

<label>Step-Up %: <span id="stepV">10</span>%</label>
<input type="range" min="0" max="20" value="10" oninput="stepV.innerText=this.value">

<button onclick="sipCalc()">CALCULATE</button>
<p id="sipOut"></p>
</div>

<!-- STOCK -->
<div id="stock" class="tab card">
<h3>STOCK CALCULATOR</h3>
<input id="buy" placeholder="Buy Price">
<input id="sell" placeholder="Sell Price">
<input id="qty" placeholder="Quantity">
<button onclick="stockCalc()">CALCULATE</button>
<p id="stockOut"></p>
</div>

<nav>
<button onclick="showTab('basic')">BASIC</button>
<button onclick="showTab('sip')">SIP</button>
<button onclick="showTab('stock')">STOCK</button>
</nav>

<script>
function showTab(id){
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function toggleDark(){
  document.body.classList.toggle('dark');
}

function basicCalc(){
  try{
    document.getElementById('basicOut').innerText=
      eval(document.getElementById('expr').value);
  }catch{
    basicOut.innerText="Invalid";
  }
}

// VERSION-1 SAFE SIP LOGIC
function sipCalc(){
  let sip=+sipV.innerText;
  let rate=+rateV.innerText/100/12;
  let years=+yearV.innerText;
  let step=+stepV.innerText;

  let invested=sip*12*years;
  let total=0;
  let s=sip;

  for(let y=0;y<years;y++){
    total+= s*((Math.pow(1+rate,12)-1)/rate)*(1+rate);
    s+=s*step/100;
  }

  sipOut.innerText=
    `Invested: ₹${invested.toFixed(0)}
Value: ₹${total.toFixed(0)}
Returns: ₹${(total-invested).toFixed(0)}`;
}

function stockCalc(){
  let p=(sell.value-buy.value)*qty.value;
  stockOut.innerText="Profit / Loss: ₹"+p;
}
</script>

</body>
</html>
