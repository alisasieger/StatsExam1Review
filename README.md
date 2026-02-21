# StatsExam1Review
<!DOCTYPE html>
<html>
<head>
  <title>Zebra Survival: Stats Edition</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      text-align: center;
      margin: 0;
      padding: 0;
    }

    h1 {
      margin-top: 20px;
    }

    .game-container {
      max-width: 800px;
      margin: auto;
      padding: 20px;
    }

    .zebra {
      font-size: 80px;
    }

    .stats {
      margin: 15px 0;
    }

    .bar-container {
      background: #ddd;
      border-radius: 20px;
      margin: 8px 0;
    }

    .bar {
      height: 20px;
      border-radius: 20px;
    }

    .hunger { background: #4CAF50; }
    .thirst { background: #2196F3; }
    .happy  { background: #FF9800; }

    button {
      padding: 10px 15px;
      margin: 5px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .shop button {
      background-color: #333;
      color: white;
    }

    .question-box {
      margin-top: 20px;
      padding: 15px;
      background: white;
      border-radius: 10px;
    }

    .timer {
      font-size: 20px;
      font-weight: bold;
    }

  </style>
</head>
<body>

<h1>🦓 Zebra Survival: Stats Edition</h1>

<div class="game-container">

  <div class="timer">Time Survived: <span id="time">0</span> / 300 seconds</div>
  <div>Coins: <span id="coins">0</span></div>

  <div class="zebra">🦓</div>

  <div class="stats">
    Hunger
    <div class="bar-container">
      <div id="hungerBar" class="bar hunger"></div>
    </div>

    Thirst
    <div class="bar-container">
      <div id="thirstBar" class="bar thirst"></div>
    </div>

    Happiness
    <div class="bar-container">
      <div id="happyBar" class="bar happy"></div>
    </div>
  </div>

  <div class="shop">
    <h3>Shop</h3>
    <button onclick="buyFood()">🌾 Food (10 coins)</button>
    <button onclick="buyWater()">💧 Water (10 coins)</button>
    <button onclick="buyStable()">🏠 Stable (20 coins)</button>
    <button onclick="buyOutfit()">🎀 Outfit (15 coins)</button>
  </div>

  <div class="question-box">
    <h3 id="question"></h3>
    <div id="answers"></div>
  </div>

</div>

<script>
let hunger = 100;
let thirst = 100;
let happiness = 70;
let coins = 0;
let timeSurvived = 0;
let gameOver = false;

const questions = [
  {q:"What is statistics?", a:["Study of shapes","Data calculations used to understand the world","Animal science","Random guessing"], c:1},
  {q:"Why is randomness important?", a:["Makes data confusing","Selects representative samples","Removes bias completely","Increases sample size"], c:1},
  {q:"What is ordinal categorical data?", a:["Has inherent order","Numerical only","Random values","No order"], c:0},
  {q:"Which is a univariate categorical display?", a:["Histogram","Frequency table","Scatterplot","Boxplot"], c:1},
  {q:"Difference between frequency and relative frequency?", a:["Same thing","Relative gives percentages","Frequency gives percentages","No difference"], c:1},
  {q:"In a bar chart, bars must be same width.", a:["True","False"], c:0},
  {q:"In a Pareto chart, most frequent category is on the ___?", a:["Right","Left"], c:1},
  {q:"Which is a univariate quantitative display?", a:["Pie chart","Dotplot","Mosaic plot","Pareto chart"], c:1},
  {q:"When describing distribution always state:", a:["Color, size, shape","Shape, center, spread","Mean only","Spread only"], c:1},
  {q:"Histograms show:", a:["Only mean","Shape, center, spread","Only outliers","Categories"], c:1},
  {q:"If distribution is skewed use:", a:["Mean & SD","Median & IQR"], c:1},
  {q:"Which is a bivariate categorical display?", a:["Contingency table","Histogram","Dotplot","Stem plot"], c:0},
  {q:"Z-score formula is:", a:["(x-mean)/SD","mean/x","SD/x","x+mean"], c:0},
  {q:"Standardizing data means:", a:["Convert to z-scores","Delete outliers","Square data","Average data"], c:0},
  {q:"Big z-score means:", a:["Close to mean","Far from mean","Equal to mean","Negative value"], c:1},
  {q:"Negative z-score means:", a:["Above mean","Below mean"], c:1},
  {q:"Normal distributions are:", a:["Skewed & bimodal","Unimodal & symmetric"], c:1},
  {q:"Values beyond how many SDs are outliers?", a:["2","3","5"], c:1},
  {q:"Goodness of fit test:", a:["Tests if sample fits distribution","Tests correlation","Finds mean","Measures spread"], c:0}
];

function updateBars(){
  hungerBar.style.width = hunger + "%";
  thirstBar.style.width = thirst + "%";
  happyBar.style.width = happiness + "%";
  coinsDisplay.innerText = coins;
}

function loadQuestion(){
  const q = questions[Math.floor(Math.random()*questions.length)];
  document.getElementById("question").innerText = q.q;
  const answersDiv = document.getElementById("answers");
  answersDiv.innerHTML="";
  q.a.forEach((text,i)=>{
    const btn=document.createElement("button");
    btn.innerText=text;
    btn.onclick=function(){
      if(i===q.c){
        coins+=10;
      }
      updateBars();
      loadQuestion();
    }
    answersDiv.appendChild(btn);
  });
}

function buyFood(){
  if(coins>=10){ coins-=10; hunger=Math.min(100,hunger+20); updateBars();}
}
function buyWater(){
  if(coins>=10){ coins-=10; thirst=Math.min(100,thirst+20); updateBars();}
}
function buyStable(){
  if(coins>=20){ coins-=20; happiness=Math.min(100,happiness+15); updateBars();}
}
function buyOutfit(){
  if(coins>=15){ coins-=15; happiness=Math.min(100,happiness+10); updateBars();}
}

function gameLoop(){
  if(gameOver) return;

  hunger-=1;
  thirst-=1;
  happiness-=0.5;

  timeSurvived++;
  document.getElementById("time").innerText=timeSurvived;

  updateBars();

  if(hunger<=0 || thirst<=0){
    alert("Your zebra didn't survive! Game Over.");
    gameOver=true;
  }

  if(timeSurvived>=300){
    alert("You kept your zebra alive for 5 minutes! You Win!");
    gameOver=true;
  }
}

const hungerBar=document.getElementById("hungerBar");
const thirstBar=document.getElementById("thirstBar");
const happyBar=document.getElementById("happyBar");
const coinsDisplay=document.getElementById("coins");

updateBars();
loadQuestion();
setInterval(gameLoop,1000);

</script>

</body>
</html>
