prompt 1

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NutriScope</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
    --bg:#0c1210;
    --panel:#121b18;
    --panel2:#17221e;
    --line:#243530;
    --text:#eaf3ef;
    --muted:#8aa39b;
    --mint:#3ddc97;
    --mint-dim:#2a9d6f;
    --amber:#f0b35a;
    --red:#e8665a;
    --blue:#5ac8fa;
    --font-display: 'Space Grotesk', 'Trebuchet MS', sans-serif;
    --font-body: 'Inter', -apple-system, sans-serif;
    --font-mono: 'IBM Plex Mono', monospace;
  }
  @import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap');

  *{box-sizing:border-box; margin:0; padding:0;}
  body{
    background: radial-gradient(circle at 20% 0%, #15241f 0%, var(--bg) 55%);
    color:var(--text);
    font-family:var(--font-body);
    line-height:1.5;
    min-height:100vh;
    padding-bottom:60px;
  }
  ::selection{ background:var(--mint); color:#04140f;}

  /* ===== Header ===== */
  header{
    padding:28px 24px 18px;
    max-width:1200px;
    margin:0 auto;
    display:flex;
    justify-content:space-between;
    align-items:flex-start;
    flex-wrap:wrap;
    gap:14px;
    border-bottom:1px solid var(--line);
  }
  .brand{display:flex; align-items:center; gap:12px;}
  .brand-mark{
    width:38px; height:38px; border-radius:10px;
    background:linear-gradient(135deg, var(--mint), var(--blue));
    display:flex; align-items:center; justify-content:center;
    font-family:var(--font-display); font-weight:700; color:#04140f; font-size:18px;
    flex-shrink:0;
  }
  .brand h1{ font-family:var(--font-display); font-size:22px; letter-spacing:0.5px;}
  .brand p{ color:var(--muted); font-size:12.5px; font-family:var(--font-mono); margin-top:2px;}
  .header-status{font-family:var(--font-mono); font-size:11px; color:var(--muted); text-align:right;}
  .header-status .pulse{display:inline-block; width:7px; height:7px; border-radius:50%; background:var(--mint); margin-right:6px; box-shadow:0 0 0 0 rgba(61,220,151,.6); animation:pulse 2s infinite;}
  @keyframes pulse{0%{box-shadow:0 0 0 0 rgba(61,220,151,.5)}70%{box-shadow:0 0 0 6px rgba(61,220,151,0)}100%{box-shadow:0 0 0 0 rgba(61,220,151,0)}}

  main{ max-width:1200px; margin:0 auto; padding:24px;}

  /* ===== Section / Card ===== */
  .section-title{
    font-family:var(--font-display); font-size:13px; letter-spacing:2.5px; text-transform:uppercase;
    color:var(--mint); margin:36px 0 14px; display:flex; align-items:center; gap:10px;
  }
  .section-title::after{ content:''; flex:1; height:1px; background:var(--line);}
  .section-title:first-of-type{margin-top:0;}

  .card{
    background: linear-gradient(160deg, var(--panel) 0%, var(--panel2) 100%);
    border:1px solid var(--line);
    border-radius:14px;
    padding:20px;
  }
  .grid{ display:grid; gap:16px; }
  .grid-profile{ grid-template-columns:repeat(auto-fit, minmax(150px,1fr)); }
  .grid-log{ grid-template-columns: 2fr 1fr 1fr auto; }
  .grid-dash{ grid-template-columns: repeat(auto-fit, minmax(280px,1fr)); }
  .grid-2{ grid-template-columns: 1.3fr 1fr; }

  @media (max-width:760px){
    .grid-log{ grid-template-columns: 1fr 1fr; }
    .grid-log .full-row{ grid-column: 1/-1; }
    .grid-2{ grid-template-columns:1fr; }
  }

  /* ===== Form fields ===== */
  label{ display:block; font-size:11px; font-family:var(--font-mono); color:var(--muted); text-transform:uppercase; letter-spacing:1.2px; margin-bottom:6px;}
  input, select{
    width:100%; background:#0c1614; border:1px solid var(--line); color:var(--text);
    padding:10px 12px; border-radius:8px; font-size:14px; font-family:var(--font-body);
    outline:none; transition:border-color .15s, box-shadow .15s;
  }
  input:focus, select:focus{ border-color:var(--mint); box-shadow:0 0 0 3px rgba(61,220,151,0.12);}
  select{ appearance:none; cursor:pointer;
    background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='6'><path d='M0 0L5 6L10 0' fill='%238aa39b'/></svg>");
    background-repeat:no-repeat; background-position:right 12px center; padding-right:30px;
  }
  .field{ display:flex; flex-direction:column; }

  /* ===== Buttons ===== */
  .btn{
    font-family:var(--font-display); font-weight:600; font-size:13.5px; letter-spacing:.5px;
    border:none; border-radius:8px; cursor:pointer; padding:11px 18px;
    transition:transform .12s, filter .15s; white-space:nowrap;
  }
  .btn:active{ transform:scale(0.97); }
  .btn-primary{ background:linear-gradient(135deg, var(--mint), #29b87c); color:#04140f; }
  .btn-primary:hover{ filter:brightness(1.1); }
  .btn-ghost{ background:transparent; border:1px solid var(--line); color:var(--muted); padding:8px 12px; font-size:12px;}
  .btn-ghost:hover{ color:var(--red); border-color:var(--red); }

  /* ===== Table ===== */
  table{ width:100%; border-collapse:collapse; font-size:13.5px; }
  th{ text-align:left; font-family:var(--font-mono); font-size:10.5px; letter-spacing:1.2px; text-transform:uppercase; color:var(--muted); padding:10px 12px; border-bottom:1px solid var(--line);}
  td{ padding:10px 12px; border-bottom:1px solid var(--line); vertical-align:middle;}
  tr:last-child td{ border-bottom:none; }
  td input, td select{ padding:7px 9px; font-size:13px; }
  .empty-row td{ text-align:center; color:var(--muted); font-style:italic; padding:24px; font-family:var(--font-mono); font-size:12px;}

  /* ===== Dashboard widgets ===== */
  .stat-label{ font-family:var(--font-mono); font-size:11px; letter-spacing:1.5px; text-transform:uppercase; color:var(--muted); margin-bottom:10px;}
  .big-number{ font-family:var(--font-display); font-size:38px; font-weight:700; line-height:1;}
  .big-number small{ font-size:15px; color:var(--muted); font-weight:500;}
  .progress-track{ height:8px; border-radius:6px; background:#0c1614; border:1px solid var(--line); overflow:hidden; margin-top:14px;}
  .progress-fill{ height:100%; border-radius:6px; background:linear-gradient(90deg, var(--mint-dim), var(--mint)); transition:width .5s ease;}
  .sub-row{ display:flex; justify-content:space-between; font-size:12px; color:var(--muted); margin-top:8px; font-family:var(--font-mono);}

  .nutrient-row{ display:flex; align-items:center; gap:12px; padding:9px 0; border-bottom:1px solid var(--line);}
  .nutrient-row:last-child{border-bottom:none;}
  .nutrient-name{ width:90px; font-size:12.5px; font-weight:500; flex-shrink:0;}
  .nutrient-bar-track{ flex:1; height:6px; border-radius:4px; background:#0c1614; border:1px solid var(--line); overflow:hidden;}
  .nutrient-bar-fill{ height:100%; border-radius:4px;}
  .nutrient-pct{ width:48px; text-align:right; font-family:var(--font-mono); font-size:12px; color:var(--muted); flex-shrink:0;}

  .pill{ display:inline-block; padding:3px 9px; border-radius:20px; font-size:10.5px; font-family:var(--font-mono); letter-spacing:0.5px; }
  .pill-low{ background:rgba(232,102,90,0.15); color:var(--red); }
  .pill-high{ background:rgba(240,179,90,0.15); color:var(--amber); }
  .pill-ok{ background:rgba(61,220,151,0.12); color:var(--mint); }

  .deficiency-item, .excess-item{
    display:flex; justify-content:space-between; align-items:center;
    padding:11px 0; border-bottom:1px dashed var(--line); font-size:13.5px;
  }
  .deficiency-item:last-child, .excess-item:last-child{ border-bottom:none; }
  .deficiency-item span:first-child, .excess-item span:first-child{ font-weight:500; }

  /* ===== Recommendations ===== */
  .rec-card{
    border:1px solid var(--line); border-radius:10px; padding:14px 16px; margin-bottom:10px;
    background:#0e1715; display:flex; gap:12px; align-items:flex-start;
  }
  .rec-card .tag{
    font-family:var(--font-mono); font-size:10px; letter-spacing:1.5px; text-transform:uppercase;
    padding:4px 8px; border-radius:6px; flex-shrink:0; margin-top:1px;
  }
  .tag-add{ background:rgba(61,220,151,0.15); color:var(--mint);}
  .tag-swap{ background:rgba(90,200,250,0.15); color:var(--blue);}
  .tag-portion{ background:rgba(240,179,90,0.15); color:var(--amber);}
  .rec-card p{ font-size:13.5px; color:#cfe6df; }
  .rec-card strong{ color:var(--text); }

  .empty-state{ text-align:center; padding:40px 20px; color:var(--muted); font-family:var(--font-mono); font-size:12.5px; }

  canvas{ max-height:280px; }

  .table-wrap{ overflow-x:auto; }
  .card-title{ font-family:var(--font-display); font-size:15px; margin-bottom:14px; font-weight:600;}
  footer{ text-align:center; color:var(--muted); font-family:var(--font-mono); font-size:11px; margin-top:50px; padding:20px; border-top:1px solid var(--line);}
</style>
</head>
<body>

<header>
  <div class="brand">
    <div class="brand-mark">N</div>
    <div>
      <h1>NutriScope</h1>
      <p>// personal nutrient analysis engine</p>
    </div>
  </div>
  <div class="header-status"><span class="pulse"></span>local session — no data leaves this device</div>
</header>

<main>

  <!-- ===================== PROFILE ===================== -->
  <div class="section-title">Profile</div>
  <div class="card grid grid-profile">
    <div class="field">
      <label>Age (years)</label>
      <input type="number" id="age" min="1" max="120" value="28">
    </div>
    <div class="field">
      <label>Gender</label>
      <select id="gender">
        <option value="male">Male</option>
        <option value="female">Female</option>
      </select>
    </div>
    <div class="field">
      <label>Height (cm)</label>
      <input type="number" id="height" min="50" max="250" value="170">
    </div>
    <div class="field">
      <label>Weight (kg)</label>
      <input type="number" id="weight" min="10" max="300" value="65">
    </div>
    <div class="field">
      <label>Activity Level</label>
      <select id="activity">
        <option value="1.2">Sedentary (little/no exercise)</option>
        <option value="1.375" selected>Light (1-3 days/week)</option>
        <option value="1.55">Moderate (3-5 days/week)</option>
        <option value="1.725">Active (6-7 days/week)</option>
        <option value="1.9">Very Active (athlete)</option>
      </select>
    </div>
    <div class="field">
      <label>Dietary Preference</label>
      <select id="dietPref">
        <option value="veg">Vegetarian</option>
        <option value="egg">Eggetarian</option>
        <option value="nonveg" selected>Non-Vegetarian</option>
      </select>
    </div>
  </div>

  <!-- ===================== FOOD LOG ===================== -->
  <div class="section-title">Food Log</div>
  <div class="card">
    <div class="grid grid-log" style="margin-bottom:18px;">
      <div class="field">
        <label>Food Item</label>
        <select id="foodSelect"></select>
      </div>
      <div class="field">
        <label>Quantity</label>
        <input type="number" id="qtyInput" value="1" min="0" step="0.1">
      </div>
      <div class="field">
        <label>Unit</label>
        <select id="unitSelect">
          <option value="serving">Serving</option>
          <option value="grams">Grams</option>
          <option value="ml">ml</option>
          <option value="piece">Piece</option>
          <option value="cup">Cup</option>
          <option value="bowl">Bowl</option>
        </select>
      </div>
      <div class="field" style="justify-content:flex-end;">
        <label style="visibility:hidden;">Add</label>
        <button class="btn btn-primary" onclick="addFoodEntry()">+ Add to Log</button>
      </div>
    </div>

    <div class="table-wrap">
      <table id="logTable">
        <thead>
          <tr>
            <th>Food</th><th>Qty</th><th>Unit</th><th>Calories</th><th>Protein</th><th>Carbs</th><th>Fat</th><th></th>
          </tr>
        </thead>
        <tbody id="logTableBody">
          <tr class="empty-row"><td colspan="8">No food logged yet — add your first item above.</td></tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- ===================== DASHBOARD ===================== -->
  <div class="section-title">Dashboard</div>
  <div class="grid grid-dash">

    <div class="card">
      <div class="stat-label">Energy Progress</div>
      <div class="big-number"><span id="kcalConsumed">0</span> <small>/ <span id="kcalTarget">0</span> kcal</small></div>
      <div class="progress-track"><div class="progress-fill" id="energyFill" style="width:0%"></div></div>
      <div class="sub-row"><span id="energyPct">0% of target</span><span id="kcalRemaining">— kcal remaining</span></div>
    </div>

    <div class="card">
      <div class="stat-label">Macro Breakdown</div>
      <canvas id="macroChart"></canvas>
    </div>

    <div class="card">
      <div class="stat-label">Top Deficiencies</div>
      <div id="deficiencyList"><div class="empty-state">Log food to see analysis</div></div>
    </div>

    <div class="card">
      <div class="stat-label">Top Excesses</div>
      <div id="excessList"><div class="empty-state">Log food to see analysis</div></div>
    </div>

  </div>

  <!-- ===================== NUTRIENT TABLE ===================== -->
  <div class="section-title">Nutrient Breakdown</div>
  <div class="card">
    <div id="nutrientTable"></div>
  </div>

  <!-- ===================== RECOMMENDATIONS ===================== -->
  <div class="section-title">Recommendations</div>
  <div class="card">
    <div id="recommendations"><div class="empty-state">Log food to receive personalized recommendations</div></div>
  </div>

</main>

<footer>NutriScope — all calculations run locally in your browser. Estimates only, not medical advice.</footer>

<script>
/* ============================================================
   FOOD DATABASE
   Values per "1 serving" (default serving size noted in gramsPerServing)
   Nutrients: calories, protein(g), carbs(g), fat(g), fiber(g), iron(mg), calcium(mg), vitC(mg), vitD(mcg), b12(mcg)
============================================================ */
const FOOD_DB = {
  "Rice (cooked)":  { gramsPerServing:150, calories:200, protein:4.2, carbs:44, fat:0.4, fiber:0.6, iron:0.5, calcium:10, vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Roti (wheat)":   { gramsPerServing:40,  calories:120, protein:3.1, carbs:18, fat:3.7, fiber:2.2, iron:1.2, calcium:14, vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Dal (cooked)":   { gramsPerServing:150, calories:180, protein:9,   carbs:24, fat:5,   fiber:6,   iron:2.5, calcium:40, vitC:1,   vitD:0,   b12:0,    type:"veg" },
  "Paneer":         { gramsPerServing:100, calories:265, protein:18,  carbs:3.6,fat:20,  fiber:0,   iron:0.5, calcium:480,vitC:0,   vitD:0,   b12:0.6,  type:"veg" },
  "Curd (yogurt)":  { gramsPerServing:100, calories:60,  protein:3.5, carbs:4.7,fat:3.3, fiber:0,   iron:0.1, calcium:120,vitC:1,   vitD:0,   b12:0.3,  type:"veg" },
  "Chana (chickpeas)": { gramsPerServing:150, calories:210, protein:11, carbs:35, fat:3.5, fiber:9, iron:3.4, calcium:60, vitC:2,  vitD:0,  b12:0,    type:"veg" },
  "Rajma (kidney beans)": { gramsPerServing:150, calories:195, protein:10, carbs:30, fat:1.5, fiber:9.5, iron:3.2, calcium:50, vitC:1, vitD:0, b12:0, type:"veg" },
  "Banana":         { gramsPerServing:120, calories:105, protein:1.3, carbs:27, fat:0.4, fiber:3.1, iron:0.3, calcium:6,  vitC:10,  vitD:0,   b12:0,    type:"veg" },
  "Apple":          { gramsPerServing:150, calories:80,  protein:0.4, carbs:21, fat:0.3, fiber:3.6, iron:0.2, calcium:8,  vitC:6,   vitD:0,   b12:0,    type:"veg" },
  "Milk":           { gramsPerServing:240, calories:150, protein:8,   carbs:12, fat:8,   fiber:0,   iron:0.1, calcium:300,vitC:0,   vitD:2.5, b12:1.1,  type:"veg" },
  "Oats":           { gramsPerServing:40,  calories:150, protein:5.3, carbs:27, fat:2.6, fiber:4,   iron:1.7, calcium:20, vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Bread (slice)":  { gramsPerServing:30,  calories:80,  protein:2.7, carbs:14, fat:1,   fiber:1.2, iron:0.8, calcium:30, vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Egg (boiled)":   { gramsPerServing:50,  calories:78,  protein:6.3, carbs:0.6,fat:5.3, fiber:0,   iron:0.9, calcium:25, vitC:0,   vitD:1.1, b12:0.6,  type:"egg" },
  "Chicken (cooked)": { gramsPerServing:100, calories:165, protein:31, carbs:0, fat:3.6, fiber:0, iron:1,   calcium:11, vitC:0,   vitD:0.1, b12:0.3,  type:"nonveg" },
  "Fish (cooked)":  { gramsPerServing:100, calories:140, protein:22,  carbs:0,  fat:5,   fiber:0,   iron:0.6, calcium:15, vitC:0,   vitD:8,   b12:2.5,  type:"nonveg" },
  "Potato (cooked)":{ gramsPerServing:150, calories:130, protein:2.7, carbs:30, fat:0.2, fiber:2.7, iron:0.6, calcium:12, vitC:13,  vitD:0,   b12:0,    type:"veg" },
  "Poha":           { gramsPerServing:150, calories:250, protein:4,   carbs:48, fat:5,   fiber:1.5, iron:1.8, calcium:15, vitC:5,   vitD:0,   b12:0,    type:"veg" },
  "Idli (piece)":   { gramsPerServing:40,  calories:60,  protein:2,   carbs:12, fat:0.3, fiber:0.5, iron:0.4, calcium:5,  vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Dosa":           { gramsPerServing:80,  calories:130, protein:3,   carbs:20, fat:4,   fiber:0.8, iron:0.6, calcium:10, vitC:0,   vitD:0,   b12:0,    type:"veg" },
  "Spinach (cooked)": { gramsPerServing:100, calories:23, protein:2.9, carbs:3.6, fat:0.4, fiber:2.2, iron:3.6, calcium:99, vitC:9.8, vitD:0,  b12:0,   type:"veg" },
};

/* Unit -> gram multiplier relative to a "serving" (approx, generic mapping) */
const UNIT_GRAMS = {
  serving: null, // uses gramsPerServing of food
  grams: 1,
  ml: 1,       // approx 1ml = 1g for liquids
  piece: null, // uses gramsPerServing (treat piece ~ serving)
  cup: 240,
  bowl: 200,
};

/* Daily targets for micronutrients (general adult RDA approximations) */
const MICRO_TARGETS = {
  fiber: 30,    // g
  iron: 18,     // mg (using higher female-leaning value as general baseline, adjusted below)
  calcium: 1000,// mg
  vitC: 75,     // mg
  vitD: 15,     // mcg
  b12: 2.4,     // mcg
};

const NUTRIENT_LABELS = {
  calories:"Calories", protein:"Protein", carbs:"Carbs", fat:"Fat", fiber:"Fiber",
  iron:"Iron", calcium:"Calcium", vitC:"Vitamin C", vitD:"Vitamin D", b12:"Vitamin B12"
};
const NUTRIENT_UNITS = {
  calories:"kcal", protein:"g", carbs:"g", fat:"g", fiber:"g",
  iron:"mg", calcium:"mg", vitC:"mg", vitD:"mcg", b12:"mcg"
};

/* ============================================================
   STATE
============================================================ */
let log = []; // {food, qty, unit}
let macroChart = null;

/* ============================================================
   INIT
============================================================ */
function init(){
  const sel = document.getElementById('foodSelect');
  Object.keys(FOOD_DB).forEach(f=>{
    const opt = document.createElement('option');
    opt.value = f; opt.textContent = f;
    sel.appendChild(opt);
  });

  document.querySelectorAll('#age,#gender,#height,#weight,#activity,#dietPref').forEach(el=>{
    el.addEventListener('input', recalcAll);
    el.addEventListener('change', recalcAll);
  });

  recalcAll();
}

/* ============================================================
   GRAM CONVERSION
============================================================ */
function gramsForEntry(foodName, qty, unit){
  const food = FOOD_DB[foodName];
  let perUnitGrams;
  if(unit === 'serving' || unit === 'piece'){
    perUnitGrams = food.gramsPerServing;
  } else {
    perUnitGrams = UNIT_GRAMS[unit];
  }
  return perUnitGrams * qty;
}

/* multiplier relative to one serving (gramsPerServing) */
function servingMultiplier(foodName, qty, unit){
  const food = FOOD_DB[foodName];
  const grams = gramsForEntry(foodName, qty, unit);
  return grams / food.gramsPerServing;
}

/* ============================================================
   FOOD LOG ACTIONS
============================================================ */
function addFoodEntry(){
  const food = document.getElementById('foodSelect').value;
  const qty = parseFloat(document.getElementById('qtyInput').value) || 0;
  const unit = document.getElementById('unitSelect').value;
  if(qty <= 0) return;
  log.push({food, qty, unit});
  renderLogTable();
  recalcAll();
}

function removeEntry(idx){
  log.splice(idx,1);
  renderLogTable();
  recalcAll();
}

function updateEntry(idx, field, value){
  if(field === 'qty'){
    log[idx].qty = parseFloat(value) || 0;
  } else if(field === 'unit'){
    log[idx].unit = value;
  } else if(field === 'food'){
    log[idx].food = value;
  }
  renderLogTable();
  recalcAll();
}

function renderLogTable(){
  const tbody = document.getElementById('logTableBody');
  if(log.length === 0){
    tbody.innerHTML = '<tr class="empty-row"><td colspan="8">No food logged yet — add your first item above.</td></tr>';
    return;
  }
  tbody.innerHTML = '';
  log.forEach((entry, idx)=>{
    const mult = servingMultiplier(entry.food, entry.qty, entry.unit);
    const f = FOOD_DB[entry.food];
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>
        <select onchange="updateEntry(${idx},'food',this.value)">
          ${Object.keys(FOOD_DB).map(name=>`<option value="${name}" ${name===entry.food?'selected':''}>${name}</option>`).join('')}
        </select>
      </td>
      <td><input type="number" min="0" step="0.1" value="${entry.qty}" onchange="updateEntry(${idx},'qty',this.value)"></td>
      <td>
        <select onchange="updateEntry(${idx},'unit',this.value)">
          ${Object.keys(UNIT_GRAMS).map(u=>`<option value="${u}" ${u===entry.unit?'selected':''}>${u.charAt(0).toUpperCase()+u.slice(1)}</option>`).join('')}
        </select>
      </td>
      <td>${Math.round(f.calories*mult)}</td>
      <td>${(f.protein*mult).toFixed(1)}g</td>
      <td>${(f.carbs*mult).toFixed(1)}g</td>
      <td>${(f.fat*mult).toFixed(1)}g</td>
      <td><button class="btn btn-ghost" onclick="removeEntry(${idx})">Remove</button></td>
    `;
    tbody.appendChild(tr);
  });
}

/* ============================================================
   CALCULATIONS
============================================================ */
function calcBMR(age, gender, height, weight){
  // Mifflin-St Jeor
  if(gender === 'male'){
    return 10*weight + 6.25*height - 5*age + 5;
  } else {
    return 10*weight + 6.25*height - 5*age - 161;
  }
}

function getTargets(){
  const age = parseFloat(document.getElementById('age').value) || 25;
  const gender = document.getElementById('gender').value;
  const height = parseFloat(document.getElementById('height').value) || 170;
  const weight = parseFloat(document.getElementById('weight').value) || 65;
  const activity = parseFloat(document.getElementById('activity').value);

  const bmr = calcBMR(age, gender, height, weight);
  const tdee = bmr * activity;

  // Macro split: 50% carbs, 20% protein (min 0.8g/kg, scale w/ tdee), 30% fat
  const proteinG = Math.max(weight * 0.8, (tdee*0.2)/4);
  const fatG = (tdee * 0.28) / 9;
  const carbsKcalRemainder = tdee - (proteinG*4) - (fatG*9);
  const carbsG = Math.max(carbsKcalRemainder,0) / 4;

  // Micronutrient targets, adjust iron for gender
  const micro = {...MICRO_TARGETS};
  micro.iron = gender === 'female' ? 18 : 8;
  micro.fiber = gender === 'female' ? 25 : 30;

  return {
    calories: tdee,
    protein: proteinG,
    carbs: carbsG,
    fat: fatG,
    fiber: micro.fiber,
    iron: micro.iron,
    calcium: micro.calcium,
    vitC: micro.vitC,
    vitD: micro.vitD,
    b12: micro.b12,
  };
}

function getConsumedTotals(){
  const totals = {calories:0,protein:0,carbs:0,fat:0,fiber:0,iron:0,calcium:0,vitC:0,vitD:0,b12:0};
  log.forEach(entry=>{
    const mult = servingMultiplier(entry.food, entry.qty, entry.unit);
    const f = FOOD_DB[entry.food];
    Object.keys(totals).forEach(key=>{
      totals[key] += f[key] * mult;
    });
  });
  return totals;
}

/* ============================================================
   MAIN RECALC / RENDER
============================================================ */
function recalcAll(){
  const targets = getTargets();
  const consumed = getConsumedTotals();

  renderEnergy(consumed, targets);
  renderMacroChart(consumed, targets);
  renderNutrientTable(consumed, targets);
  renderDeficienciesExcesses(consumed, targets);
  renderRecommendations(consumed, targets);
}

function renderEnergy(consumed, targets){
  const pct = targets.calories > 0 ? (consumed.calories / targets.calories) * 100 : 0;
  const clamped = Math.min(pct, 100);
  document.getElementById('kcalConsumed').textContent = Math.round(consumed.calories);
  document.getElementById('kcalTarget').textContent = Math.round(targets.calories);
  document.getElementById('energyFill').style.width = clamped + '%';
  document.getElementById('energyPct').textContent = Math.round(pct) + '% of target';
  const remaining = targets.calories - consumed.calories;
  document.getElementById('kcalRemaining').textContent = remaining >= 0
    ? Math.round(remaining) + ' kcal remaining'
    : Math.round(Math.abs(remaining)) + ' kcal over';

  // color shift if over
  const fill = document.getElementById('energyFill');
  if(pct > 100){
    fill.style.background = 'linear-gradient(90deg, var(--amber), var(--red))';
  } else {
    fill.style.background = 'linear-gradient(90deg, var(--mint-dim), var(--mint))';
  }
}

function renderMacroChart(consumed, targets){
  const ctx = document.getElementById('macroChart');
  const data = {
    labels: ['Protein','Carbs','Fat'],
    consumed: [
      Math.round(consumed.protein*4),
      Math.round(consumed.carbs*4),
      Math.round(consumed.fat*9)
    ],
    target: [
      Math.round(targets.protein*4),
      Math.round(targets.carbs*4),
      Math.round(targets.fat*9)
    ]
  };

  if(macroChart){
    macroChart.data.datasets[0].data = data.consumed;
    macroChart.data.datasets[1].data = data.target;
    macroChart.update();
    return;
  }

  macroChart = new Chart(ctx, {
    type:'bar',
    data:{
      labels:data.labels,
      datasets:[
        {
          label:'Consumed (kcal)',
          data:data.consumed,
          backgroundColor:'#3ddc97',
          borderRadius:6,
          maxBarThickness:34
        },
        {
          label:'Target (kcal)',
          data:data.target,
          backgroundColor:'rgba(138,163,155,0.25)',
          borderRadius:6,
          maxBarThickness:34
        }
      ]
    },
    options:{
      responsive:true,
      plugins:{
        legend:{ labels:{ color:'#8aa39b', font:{family:"'IBM Plex Mono', monospace", size:11} } }
      },
      scales:{
        x:{ ticks:{ color:'#eaf3ef', font:{family:"'Inter', sans-serif", size:12} }, grid:{ color:'#243530' } },
        y:{ ticks:{ color:'#8aa39b', font:{family:"'IBM Plex Mono', monospace", size:10} }, grid:{ color:'#243530' } }
      }
    }
  });
}

function renderNutrientTable(consumed, targets){
  const container = document.getElementById('nutrientTable');
  if(log.length === 0){
    container.innerHTML = '<div class="empty-state">Log food to see your full nutrient breakdown</div>';
    return;
  }
  let html = '';
  Object.keys(NUTRIENT_LABELS).forEach(key=>{
    const cVal = consumed[key];
    const tVal = targets[key];
    const pct = tVal > 0 ? (cVal/tVal)*100 : 0;
    const clamped = Math.min(pct, 100);

    let barColor = 'var(--mint)';
    let pillHtml = '<span class="pill pill-ok">on track</span>';
    if(pct < 70){ barColor = 'var(--red)'; pillHtml = '<span class="pill pill-low">low</span>'; }
    else if(pct > 130){ barColor = 'var(--amber)'; pillHtml = '<span class="pill pill-high">high</span>'; }

    html += `
      <div class="nutrient-row">
        <div class="nutrient-name">${NUTRIENT_LABELS[key]}</div>
        <div class="nutrient-bar-track"><div class="nutrient-bar-fill" style="width:${clamped}%; background:${barColor};"></div></div>
        <div class="nutrient-pct">${Math.round(pct)}%</div>
        <div style="width:150px; text-align:right; font-family:var(--font-mono); font-size:12px; color:var(--muted);">
          ${formatNum(cVal)} / ${formatNum(tVal)} ${NUTRIENT_UNITS[key]}
        </div>
        <div style="width:80px; text-align:right;">${pillHtml}</div>
      </div>
    `;
  });
  container.innerHTML = html;
}

function formatNum(n){
  if(n >= 100) return Math.round(n).toString();
  return n.toFixed(1);
}

function renderDeficienciesExcesses(consumed, targets){
  const defContainer = document.getElementById('deficiencyList');
  const excContainer = document.getElementById('excessList');

  if(log.length === 0){
    defContainer.innerHTML = '<div class="empty-state">Log food to see analysis</div>';
    excContainer.innerHTML = '<div class="empty-state">Log food to see analysis</div>';
    return;
  }

  const microKeys = ['fiber','iron','calcium','vitC','vitD','b12','protein'];
  const ratios = microKeys.map(key=>{
    return { key, pct: targets[key] > 0 ? (consumed[key]/targets[key])*100 : 0 };
  });

  const deficiencies = ratios.filter(r=>r.pct < 70).sort((a,b)=>a.pct-b.pct).slice(0,5);
  const excesses = ratios.filter(r=>r.pct > 130).sort((a,b)=>b.pct-a.pct).slice(0,5);

  defContainer.innerHTML = deficiencies.length ? deficiencies.map(r=>`
    <div class="deficiency-item">
      <span>${NUTRIENT_LABELS[r.key]}</span>
      <span class="pill pill-low">${Math.round(r.pct)}% of target</span>
    </div>
  `).join('') : '<div class="empty-state">No major deficiencies detected 🎯</div>';

  excContainer.innerHTML = excesses.length ? excesses.map(r=>`
    <div class="excess-item">
      <span>${NUTRIENT_LABELS[r.key]}</span>
      <span class="pill pill-high">${Math.round(r.pct)}% of target</span>
    </div>
  `).join('') : '<div class="empty-state">Nothing in excess — well balanced</div>';
}

/* ============================================================
   RECOMMENDATIONS ENGINE
============================================================ */
function renderRecommendations(consumed, targets){
  const container = document.getElementById('recommendations');
  if(log.length === 0){
    container.innerHTML = '<div class="empty-state">Log food to receive personalized recommendations</div>';
    return;
  }

  const dietPref = document.getElementById('dietPref').value; // veg | egg | nonveg
  const recs = [];

  const ratio = key => targets[key] > 0 ? (consumed[key]/targets[key])*100 : 0;

  // --- Protein ---
  if(ratio('protein') < 80){
    let suggestion;
    if(dietPref === 'nonveg') suggestion = '100g cooked chicken or fish';
    else if(dietPref === 'egg') suggestion = '2 boiled eggs or 100g paneer';
    else suggestion = '100g paneer, a bowl of dal, or a cup of curd';
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Your protein intake is at <strong>${Math.round(ratio('protein'))}%</strong> of target. Consider adding <strong>${suggestion}</strong> to your next meal.`});
  }

  // --- Iron ---
  if(ratio('iron') < 70){
    let suggestion = dietPref === 'nonveg' ? 'fish or chicken liver, or pair Spinach with Apple/Banana (vitamin C boosts iron absorption)'
      : 'Spinach, Rajma, or Chana — combine with Apple or Banana for better absorption';
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Iron is low at <strong>${Math.round(ratio('iron'))}%</strong> of target. Add <strong>${suggestion}</strong>.`});
  }

  // --- Calcium ---
  if(ratio('calcium') < 70){
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Calcium is at <strong>${Math.round(ratio('calcium'))}%</strong> of target. Add a glass of <strong>Milk</strong>, a bowl of <strong>Curd</strong>, or <strong>Paneer</strong> to boost it.`});
  }

  // --- Vitamin C ---
  if(ratio('vitC') < 70){
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Vitamin C is low at <strong>${Math.round(ratio('vitC'))}%</strong>. Add an <strong>Apple</strong>, <strong>Banana</strong>, or <strong>Potato</strong> — or fresh citrus fruit (not in database) to your day.`});
  }

  // --- Vitamin D ---
  if(ratio('vitD') < 50){
    let suggestion = (dietPref === 'nonveg') ? 'Fish (rich in Vitamin D) and get 15-20 minutes of sunlight exposure'
      : 'fortified Milk and 15-20 minutes of sunlight exposure, since plant foods are naturally low in Vitamin D';
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Vitamin D is very low at <strong>${Math.round(ratio('vitD'))}%</strong>. Try <strong>${suggestion}</strong>.`});
  }

  // --- Vitamin B12 ---
  if(ratio('b12') < 50){
    let suggestion;
    if(dietPref === 'nonveg') suggestion = 'Fish, Chicken, or Egg';
    else if(dietPref === 'egg') suggestion = 'Egg and Milk/Curd daily';
    else suggestion = 'Milk, Curd, and Paneer regularly — B12 is hard to get from plant foods alone, consider discussing supplementation with a doctor';
    recs.push({tag:'add', tagLabel:'food addition',
      html:`Vitamin B12 is at <strong>${Math.round(ratio('b12'))}%</strong> of target. Include <strong>${suggestion}</strong>.`});
  }

  // --- Fiber ---
  if(ratio('fiber') < 70){
    recs.push({tag:'swap', tagLabel:'food swap',
      html:`Fiber is at <strong>${Math.round(ratio('fiber'))}%</strong> of target. Swap <strong>Rice for Oats or Roti</strong>, and add <strong>Rajma or Chana</strong> for a fiber boost.`});
  }

  // --- Excess calories ---
  if(ratio('calories') > 110){
    recs.push({tag:'portion', tagLabel:'portion adjustment',
      html:`You're at <strong>${Math.round(ratio('calories'))}%</strong> of your calorie target. Consider reducing portions of high-calorie items like <strong>Rice, Poha, or Paneer</strong> by 20-30%.`});
  }

  // --- Excess fat ---
  if(ratio('fat') > 130){
    let swap = dietPref === 'veg' ? 'Curd instead of Paneer' : 'Fish or Chicken (lean) instead of Paneer';
    recs.push({tag:'swap', tagLabel:'food swap',
      html:`Fat intake is high at <strong>${Math.round(ratio('fat'))}%</strong> of target. Try <strong>${swap}</strong> in your next meal, or reduce oil-heavy items like Dosa.`});
  }

  // --- Excess carbs ---
  if(ratio('carbs') > 130){
    recs.push({tag:'portion', tagLabel:'portion adjustment',
      html:`Carbohydrate intake is at <strong>${Math.round(ratio('carbs'))}%</strong> of target. Reduce portions of <strong>Rice, Roti, Poha, or Bread</strong> and balance with more protein-rich foods.`});
  }

  // --- Excess calcium (rare but possible) ---
  if(ratio('calcium') > 200){
    recs.push({tag:'portion', tagLabel:'portion adjustment',
      html:`Calcium intake is unusually high at <strong>${Math.round(ratio('calcium'))}%</strong>. No action needed unless using supplements — just keep an eye on total intake.`});
  }

  if(recs.length === 0){
    recs.push({tag:'add', tagLabel:'looking good',
      html:`Your nutrient profile looks well-balanced today. Keep up a varied diet across grains, proteins, and vegetables to maintain this.`});
  }

  const tagClassMap = {add:'tag-add', swap:'tag-swap', portion:'tag-portion'};
  container.innerHTML = recs.map(r=>`
    <div class="rec-card">
      <span class="tag ${tagClassMap[r.tag]}">${r.tagLabel}</span>
      <p>${r.html}</p>
    </div>
  `).join('');
}

init();
</script>

</body>
</html>

prompt 2

<img width="985" height="726" alt="image" src="https://github.com/user-attachments/assets/f9e7e4c4-168f-4c44-8213-a6d9ae9f0c91" />

<img width="992" height="457" alt="image" src="https://github.com/user-attachments/assets/49c41032-c97f-4220-9cf4-300ea8db88dd" />

