# material-explorerr
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Material Explorer – Inquiry Lab (P3)</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
  body {
    font-family: Arial, sans-serif;
    background: #eef6ff;
    max-width: 900px;
    margin: auto;
    padding: 20px;
  }

  h1 {
    text-align: center;
    color: #1e3a8a;
  }

  .card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    margin-top: 20px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  }

  select, button {
    padding: 10px;
    font-size: 16px;
    margin: 6px 0;
  }

  button {
    border: none;
    border-radius: 8px;
    background-color: #2563eb;
    color: white;
    cursor: pointer;
  }

  button:disabled {
    background-color: #9ca3af;
    cursor: not-allowed;
  }

  /* Tank visuals */
  .tank {
    position: relative;
    width: 320px;
    height: 260px;
    border: 4px solid #60a5fa;
    border-radius: 12px;
    margin: 20px auto;
    background: linear-gradient(#bfdbfe 60%, #3b82f6 60%);
    overflow: hidden;
  }

  .object {
    position: absolute;
    width: 90px;
    height: 45px;
    left: 115px;
    top: 20px;
    border-radius: 8px;
    color: white;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 1.2s ease;
  }

  .wood { background: #92400e; }
  .plastic { background: #16a34a; }
  .sponge { background: #eab308; }
  .metal { background: #64748b; }
  .cloth { background: #db2777; }

  .float { top: 90px; }
  .sink { top: 200px; }
  .wet { background: linear-gradient(#60a5fa, #2563eb); }
  .flex { transform: skewX(15deg); }

  .prompt {
    font-size: 18px;
    font-weight: bold;
    color: #1f2937;
  }

  textarea {
    width: 100%;
    height: 60px;
    font-size: 14px;
  }
</style>
</head>

<body>

<h1>🔍 Material Explorer – Inquiry Lab</h1>

<!-- STEP 0 -->
<div class="card">
  <p class="prompt">Select a material to investigate:</p>
  <select id="materialSelect">
    <option value="">-- Select --</option>
    <option value="wood">Wood</option>
    <option value="plastic">Plastic</option>
    <option value="sponge">Sponge</option>
    <option value="metal">Metal</option>
    <option value="cloth">Cloth</option>
  </select>
  <br>
  <button onclick="start()">Start Investigation</button>
</div>

<!-- VISUAL -->
<div class="card" id="visualCard" style="display:none;">
  <div class="tank">
    <div id="obj" class="object">Item</div>
  </div>
</div>

<!-- STEP 1 -->
<div class="card" id="step1" style="display:none;">
  <p class="prompt">Step 1: What do you think will happen?</p>
  <textarea placeholder="I think the material will..."></textarea>
  <br>
  <button onclick="unlockTest()">Next</button>
</div>

<!-- STEP 2 -->
<div class="card" id="step2" style="display:none;">
  <p class="prompt">Step 2: Carry out the test</p>
  <button onclick="testFloat()">🌊 Float / Sink</button>
  <button onclick="testWater()">💧 Waterproof</button>
  <button onclick="testRigid()">🧱 Rigid / Flexible</button>
</div>

<!-- STEP 3 -->
<div class="card" id="step3" style="display:none;">
  <p class="prompt">Step 3: What do you observe?</p>
  <textarea placeholder="I observe that..."></textarea>
  <br>
  <button onclick="unlockExplain()">Next</button>
</div>

<!-- STEP 4 -->
<div class="card" id="step4" style="display:none;">
  <p class="prompt">Step 4: Why did this happen?</p>
  <textarea placeholder="This happens because..."></textarea>
</div>

<!-- SOUNDS -->
<audio id="splash" src="https://assets.mixkit.co/sfx/preview/mixkit-water-splash-1311.mp3"></audio>
<audio id="success" src="https://assets.mixkit.co/sfx/preview/mixkit-game-click-1114.mp3"></audio>

<script>
const materials = {
  wood: { float: true, waterproof: false, rigid: true, cls: "wood", name: "Wood" },
  plastic: { float: true, waterproof: true, rigid: true, cls: "plastic", name: "Plastic" },
  sponge: { float: true, waterproof: false, rigid: false, cls: "sponge", name: "Sponge" },
  metal: { float: false, waterproof: true, rigid: true, cls: "metal", name: "Metal" },
  cloth: { float: false, waterproof: false, rigid: false, cls: "cloth", name: "Cloth" }
};

let current;

function start() {
  const choice = materialSelect.value;
  if (!choice) {
    alert("Please select a material.");
    return;
  }

  current = materials[choice];
  const obj = document.getElementById("obj");

  obj.className = "object " + current.cls;
  obj.textContent = current.name;
  obj.style.top = "20px";

  document.getElementById("visualCard").style.display = "block";
  document.getElementById("step1").style.display = "block";
}

function unlockTest() {
  document.getElementById("step2").style.display = "block";
  document.getElementById("success").play();
}

function testFloat() {
  const o = document.getElementById("obj");
  o.classList.remove("float", "sink");
  o.classList.add(current.float ? "float" : "sink");
  document.getElementById("splash").play();
  document.getElementById("step3").style.display = "block";
}

function testWater() {
  const o = document.getElementById("obj");
  o.classList.toggle("wet", !current.waterproof);
  document.getElementById("splash").play();
}

function testRigid() {
  const o = document.getElementById("obj");
  o.classList.toggle("flex", !current.rigid);
  document.getElementById("success").play();
}

function unlockExplain() {
  document.getElementById("step4").style.display = "block";
  document.getElementById("success").play();
}
</script>

</body>
</html>
