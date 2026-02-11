# material-explorerr
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

