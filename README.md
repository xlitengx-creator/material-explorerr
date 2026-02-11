<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Material Explorer – Interactive Lab</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
  font-family: Arial;
  background:#eef6ff;
  max-width:1000px;
  margin:auto;
  padding:20px;
}

h1 { text-align:center; color:#1e3a8a; }

.section {
  background:white;
  padding:20px;
  border-radius:12px;
  margin-top:20px;
  box-shadow:0 4px 8px rgba(0,0,0,0.1);
}

.materials {
  display:flex;
  gap:15px;
  flex-wrap:wrap;
}

.material-card {
  width:120px;
  padding:15px;
  text-align:center;
  border-radius:10px;
  cursor:pointer;
  color:white;
  font-weight:bold;
  transition:0.3s;
}

.material-card:hover { transform:scale(1.1); }

.wood { background:#92400e; }
.plastic { background:#16a34a; }
.sponge { background:#eab308; }
.metal { background:#64748b; }
.cloth { background:#db2777; }

button {
  padding:10px;
  margin:6px;
  border:none;
  border-radius:8px;
  cursor:pointer;
  font-size:15px;
  background:#2563eb;
  color:white;
}

button:hover { background:#1d4ed8; }

button:disabled { background:#9ca3af; }

.tank {
  position:relative;
  width:350px;
  height:260px;
  margin:auto;
  border:4px solid #60a5fa;
  border-radius:12px;
  background:linear-gradient(#bfdbfe 60%, #3b82f6 60%);
  overflow:hidden;
}

.object {
  position:absolute;
  width:100px;
  height:50px;
  left:125px;
  top:20px;
  border-radius:8px;
  display:flex;
  align-items:center;
  justify-content:center;
  font-weight:bold;
  color:white;
  transition:all 1.2s ease;
}

/* Animations */
.float { top:95px; animation:bob 2s infinite; }
@keyframes bob {
  0%{transform:translateY(0)}
  50%{transform:translateY(-6px)}
  100%{transform:translateY(0)}
}

.sink { top:210px; animation:bubble 1.2s; }

@keyframes bubble {
  0%{transform:translateY(0)}
  100%{transform:translateY(10px)}
}

.wet {
  animation:fill 1.2s forwards;
}

@keyframes fill {
  0%{background:inherit;}
  100%{background:linear-gradient(#60a5fa,#2563eb);}
}

.flex { animation:bend 1s infinite alternate; }
@keyframes bend {
  from{transform:skewX(0)}
  to{transform:skewX(15deg)}
}

.rigidShake { animation:shake 0.5s; }
@keyframes shake {
  0%{transform:translateX(0)}
  25%{transform:translateX(-4px)}
  50%{transform:translateX(4px)}
  75%{transform:translateX(-4px)}
  100%{transform:translateX(0)}
}

.feedback {
  font-weight:bold;
  margin-top:10px;
  font-size:18px;
}
</style>
</head>

<body>

<h1>🔬 Material Explorer Lab</h1>

<div class="section">
  <h3>Step 1: Click a material to investigate</h3>
  <div class="materials">
    <div class="material-card wood" onclick="selectMaterial('wood')">Wood</div>
    <div class="material-card plastic" onclick="selectMaterial('plastic')">Plastic</div>
    <div class="material-card sponge" onclick="selectMaterial('sponge')">Sponge</div>
    <div class="material-card metal" onclick="selectMaterial('metal')">Metal</div>
    <div class="material-card cloth" onclick="selectMaterial('cloth')">Cloth</div>
  </div>
</div>

<div class="section" id="visual" style="display:none;">
  <div class="tank">
    <div id="obj" class="object">Item</div>
  </div>
</div>

<div class="section" id="mcq" style="display:none;">
  <h3>Step 2: What do you think will happen in water?</h3>
  <button onclick="predict('float')">A. It will float</button>
  <button onclick="predict('sink')">B. It will sink</button>
  <div class="feedback" id="feedback"></div>
</div>

<div class="section" id="testSection" style="display:none;">
  <h3>Step 3: Test the material</h3>
  <button onclick="testFloat()">🌊 Test in Water</button>
  <button onclick="testWaterproof()">💧 Test Waterproof</button>
  <button onclick="testRigid()">🧱 Test Rigid/Flexible</button>
</div>

<audio id="splash" src="https://assets.mixkit.co/sfx/preview/mixkit-water-splash-1311.mp3"></audio>
<audio id="correctSound" src="https://assets.mixkit.co/sfx/preview/mixkit-correct-answer-tone-2870.mp3"></audio>
<audio id="wrongSound" src="https://assets.mixkit.co/sfx/preview/mixkit-wrong-answer-fail-notification-946.mp3"></audio>

<script>
const materials = {
  wood:{float:true, waterproof:false, rigid:true, cls:"wood"},
  plastic:{float:true, waterproof:true, rigid:true, cls:"plastic"},
  sponge:{float:true, waterproof:false, rigid:false, cls:"sponge"},
  metal:{float:false, waterproof:true, rigid:true, cls:"metal"},
  cloth:{float:false, waterproof:false, rigid:false, cls:"cloth"}
};

let current;

function selectMaterial(type){
  current = materials[type];
  const obj = document.getElementById("obj");
  obj.className = "object "+current.cls;
  obj.style.top="20px";
  obj.textContent=type.charAt(0).toUpperCase()+type.slice(1);

  visual.style.display="block";
  mcq.style.display="block";
  feedback.innerHTML="";
}

function predict(answer){
  if((answer==="float" && current.float) ||
     (answer==="sink" && !current.float)){
    feedback.innerHTML="✅ Good prediction!";
    correctSound.play();
  } else {
    feedback.innerHTML="❌ Let's test and see!";
    wrongSound.play();
  }
  testSection.style.display="block";
}

function testFloat(){
  const obj=document.getElementById("obj");
  obj.classList.remove("float","sink");
  splash.play();
  if(current.float){
    obj.classList.add("float");
  } else {
    obj.classList.add("sink");
  }
}

function testWaterproof(){
  const obj=document.getElementById("obj");
  obj.classList.remove("wet");
  splash.play();
  if(!current.waterproof){
    obj.classList.add("wet");
  }
}

function testRigid(){
  const obj=document.getElementById("obj");
  obj.classList.remove("flex","rigidShake");
  if(current.rigid){
    obj.classList.add("rigidShake");
  } else {
    obj.classList.add("flex");
  }
}
</script>

</body>
</html>
