# project1
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>DiaLab: Virtual Shortwave Diathermy Lab</title>
<meta name="description" content="Interactive virtual lab on shortwave diathermy (SWD). Physiotherapy / Biomedical Equipment Lab.">
<style>
:root{
  --bg:#fbf7f2; --ink:#1d2433; --muted:#5d6578; --card:#ffffff; --line:#e6dfd5;
  --navy:#16203a; --accent:#e8590c; --accent2:#0b7285; --good:#2b8a3e; --warn:#f08c00; --bad:#c92a2a;
}
*{box-sizing:border-box}
html{scroll-behavior:smooth}
body{margin:0;font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;background:var(--bg);color:var(--ink);line-height:1.55}
a{color:inherit}
nav{position:sticky;top:0;z-index:20;background:var(--navy);color:#fff;display:flex;align-items:center;justify-content:space-between;padding:12px 5vw;flex-wrap:wrap;gap:8px}
nav .logo{font-weight:800;font-size:1.25rem;text-decoration:none;letter-spacing:.3px}
nav .logo span{color:#ffa94d}
nav ul{list-style:none;display:flex;gap:20px;margin:0;padding:0;flex-wrap:wrap}
nav ul a{text-decoration:none;color:#d0d6e6;font-size:.95rem}
nav ul a:hover{color:#fff}
.hero{padding:70px 5vw 60px;background:linear-gradient(135deg,#16203a 0%,#2b1d3a 55%,#7a2e0e 100%);color:#fff}
.hero h1{font-size:clamp(2rem,5vw,3.4rem);margin:0 0 14px;line-height:1.1;max-width:14em}
.hero h1 em{font-style:normal;color:#ffa94d}
.hero p{max-width:40em;color:#dfe3f0;font-size:1.08rem}
.btn{display:inline-block;border:0;cursor:pointer;font:inherit;font-weight:600;padding:10px 20px;border-radius:8px;text-decoration:none;background:var(--accent);color:#fff}
.btn:hover{filter:brightness(1.08)}
.btn.ghost{background:transparent;border:1.5px solid #ffffff88;color:#fff}
.btn.sec{background:var(--accent2)}
.btn.light{background:#eee7dc;color:var(--ink)}
.hero .credit{margin-top:34px;font-size:.9rem;color:#aab2c8}
section{padding:56px 5vw;max-width:1250px;margin:auto}
section h2{font-size:1.9rem;margin:0 0 6px}
section .sub{color:var(--muted);margin:0 0 26px}
.grid{display:grid;gap:18px}
.g2{grid-template-columns:repeat(auto-fit,minmax(300px,1fr))}
.g3{grid-template-columns:repeat(auto-fit,minmax(260px,1fr))}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:20px 22px}
.card h3{margin:0 0 8px;font-size:1.1rem}
.card ul,.card ol{margin:6px 0 0;padding-left:20px}
.card li{margin:4px 0}
.callout{background:#fff4e6;border-left:5px solid var(--accent);padding:14px 18px;border-radius:8px;margin-top:18px}
table{width:100%;border-collapse:collapse;font-size:.92rem}
th,td{border-bottom:1px solid var(--line);padding:8px 8px;text-align:left}
th{background:#f4ede3;font-weight:600}
.tablewrap{overflow-x:auto}
.formula{background:#f1ebe1;padding:2px 7px;border-radius:5px;font-family:ui-monospace,Menlo,Consolas,monospace;font-size:.92em;display:inline-block;margin:4px 0}
hr.sp{border:0;height:1px;background:var(--line);margin:0 5vw}

/* simulator */
.simwrap{display:grid;grid-template-columns:minmax(0,1.5fr) minmax(300px,1fr);gap:18px}
@media(max-width:900px){.simwrap{grid-template-columns:1fr}}
canvas{width:100%;height:auto;display:block;background:#fff;border:1px solid var(--line);border-radius:12px}
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:10px;margin:14px 0}
.stat{background:var(--card);border:1px solid var(--line);border-radius:10px;padding:10px 12px}
.stat small{display:block;color:var(--muted);font-size:.78rem}
.stat b{font-size:1.25rem}
.status{padding:10px 14px;border-radius:10px;background:#e7f5ff;border:1px solid #a5d8ff;font-size:.95rem;min-height:44px}
.status.warn{background:#fff3bf;border-color:#ffd43b}
.status.bad{background:#ffe3e3;border-color:#ff8787}
.status.good{background:#ebfbee;border-color:#8ce99a}
.panel h3{margin:0 0 12px}
.ctl{margin:13px 0}
.ctl label{display:flex;justify-content:space-between;font-size:.9rem;font-weight:600;margin-bottom:4px}
.ctl label span{color:var(--accent);font-weight:700}
input[type=range]{width:100%;accent-color:var(--accent)}
.seg{display:flex;border:1.5px solid var(--line);border-radius:9px;overflow:hidden}
.seg button{flex:1;border:0;background:#fff;padding:8px 6px;font:inherit;font-size:.9rem;cursor:pointer}
.seg button.on{background:var(--navy);color:#fff}
.checks label{display:flex;gap:8px;align-items:center;margin:6px 0;font-size:.92rem;cursor:pointer}
.meter{height:16px;border-radius:9px;background:linear-gradient(90deg,#ffe3e3,#fff3bf,#d3f9d8);border:1px solid var(--line);position:relative;overflow:hidden}
.meter i{position:absolute;top:0;bottom:0;width:4px;background:var(--navy);left:0;transition:left .1s}
.row{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px}
select{font:inherit;padding:6px 8px;border-radius:7px;border:1.5px solid var(--line);background:#fff}
.legend{height:10px;border-radius:5px;background:linear-gradient(90deg,hsl(200,80%,55%),hsl(100,80%,50%),hsl(50,90%,52%),hsl(0,85%,50%));margin-top:6px}
.legend-l{display:flex;justify-content:space-between;font-size:.75rem;color:var(--muted)}

/* quiz */
.q{margin-bottom:18px}
.q p{font-weight:600;margin:0 0 8px}
.opt{display:block;width:100%;text-align:left;border:1.5px solid var(--line);background:#fff;border-radius:9px;padding:9px 12px;margin:6px 0;font:inherit;cursor:pointer}
.opt:hover{border-color:var(--accent)}
.opt.right{background:#ebfbee;border-color:var(--good)}
.opt.wrong{background:#ffe3e3;border-color:var(--bad)}
.fb{font-size:.9rem;color:var(--muted);margin-top:4px;display:none}
footer{background:var(--navy);color:#c3cadb;text-align:center;padding:28px 5vw;font-size:.92rem}
footer b{color:#fff}
</style>
</head>
<body>

<nav>
  <a class="logo" href="#top">Dia<span>Lab</span></a>
  <ul>
    <li><a href="#aim">Aim</a></li>
    <li><a href="#theory">Theory</a></li>
    <li><a href="#sim">Simulator</a></li>
    <li><a href="#exp">Experiment</a></li>
    <li><a href="#quiz">Quiz</a></li>
  </ul>
</nav>

<header class="hero" id="top">
  <h1>Shortwave diathermy <em>virtual lab.</em></h1>
  <p>Run a virtual shortwave diathermy unit on a limb. Tune the resonator, switch between capacitor and inductive methods, compare continuous and pulsed output, and watch skin, fat and muscle temperatures change. No hardware needed.</p>
  <div class="row">
    <a class="btn" href="#sim">Start the simulator</a>
    <a class="btn ghost" href="#theory">Read the theory</a>
  </div>
  <div class="credit">Biomedical Equipment / Electrotherapy Lab · Created by <b id="authorTop">Lakshana</b></div>
</header>

<section id="aim">
  <h2>Aim and apparatus</h2>
  <p class="sub">Study how a shortwave diathermy (SWD) unit produces deep heat and how the settings change where and how much the tissue heats.</p>
  <div class="grid g3">
    <div class="card">
      <h3>Aim</h3>
      <ul>
        <li>Understand the parts of an SWD unit and its 27.12 MHz operating frequency</li>
        <li>Tune the patient circuit to resonance with the oscillator</li>
        <li>Compare the capacitor field and inductive (coil) methods</li>
        <li>Compare continuous and pulsed shortwave diathermy</li>
        <li>Judge dosage and recognise burn and safety hazards</li>
      </ul>
    </div>
    <div class="card">
      <h3>Virtual apparatus</h3>
      <ul>
        <li>Shortwave generator with intensity, tuning and mode controls</li>
        <li>Condenser (capacitor) plate electrodes and an inductive cable/drum coil</li>
        <li>Limb model: skin, subcutaneous fat, muscle and bone</li>
        <li>Resonance (current) meter and temperature recorder</li>
      </ul>
    </div>
    <div class="card">
      <h3>Safety rule</h3>
      <p>Heat the target tissue to the therapeutic range of <b>40 to 45 °C</b>. Fat has a poor blood supply and few pain receptors, so it can overheat before the patient complains. Dose by patient warmth, and never by the dial alone.</p>
    </div>
  </div>
</section>

<hr class="sp">

<section id="theory">
  <h2>Theory</h2>
  <p class="sub">Shortwave diathermy uses a high-frequency electromagnetic field to heat deep tissue without direct contact conduction.</p>

  <div class="grid g2">
    <div class="card">
      <h3>What is shortwave diathermy?</h3>
      <p>Diathermy means "heating through". An oscillator generates a radio-frequency current, and the tissues absorb its energy as heat. The frequencies allowed for medical use (ISM bands) are:</p>
      <div class="tablewrap">
      <table>
        <tr><th>Frequency</th><th>Wavelength</th><th>Use</th></tr>
        <tr><td>27.12 MHz</td><td>about 11 m</td><td>Most common</td></tr>
        <tr><td>13.56 MHz</td><td>about 22 m</td><td>Less common</td></tr>
        <tr><td>40.68 MHz</td><td>about 7.4 m</td><td>Rare</td></tr>
      </table>
      </div>
    </div>

    <div class="card">
      <h3>Main parts of the unit</h3>
      <ol>
        <li><b>Power supply:</b> mains to the high-voltage DC the oscillator needs</li>
        <li><b>Oscillator (generator) circuit:</b> valve or transistor circuit producing 27.12 MHz, crystal controlled for stable frequency</li>
        <li><b>Patient (resonator) circuit:</b> contains the electrodes and the patient, tuned by a variable capacitor</li>
        <li><b>Tuning control and meter:</b> the meter reads highest when both circuits are in resonance</li>
        <li><b>Pulse circuit (pulsed SWD):</b> switches the output on and off in short bursts</li>
      </ol>
    </div>

    <div class="card">
      <h3>Capacitor field method</h3>
      <p>The limb sits between two condenser plates, like the dielectric of a capacitor. An electric field passes through the tissues in series.</p>
      <ul>
        <li>High-impedance tissue (<b>fat</b>) absorbs the most energy, so superficial tissues heat most</li>
        <li>A larger air gap gives a deeper field but a weaker one</li>
        <li>Equal plates give even heating; a smaller plate gives more heat beneath it (higher current density)</li>
        <li>Plate arrangements: contraplanar, coplanar and longitudinal</li>
      </ul>
    </div>

    <div class="card">
      <h3>Inductive (coil) method</h3>
      <p>A cable or drum coil wrapped round or placed over the part creates a changing magnetic field, and this induces <b>eddy currents</b> in the tissue.</p>
      <ul>
        <li>Eddy currents flow best in <b>water-rich, low-impedance tissue</b>, so <b>muscle</b> heats most</li>
        <li>Fat heats less, so the inductive method is preferred for deeper muscle heating and for thick fat layers</li>
        <li>Drum coils (monode) and cable coils are common</li>
      </ul>
    </div>

    <div class="card">
      <h3>Why tuning matters</h3>
      <p>Maximum energy is transferred only when the patient circuit has the same resonant frequency as the oscillator circuit. Changing the electrode gap, the limb size or the electrode position changes the patient circuit, so the unit must be <b>retuned each time</b>.</p>
      <p>Detuned means the meter reads low and the patient gets less energy than the dial suggests.</p>
    </div>

    <div class="card">
      <h3>Continuous and pulsed output</h3>
      <p><b>Continuous SWD (CSWD)</b> is the thermal form. <b>Pulsed SWD (PSWD)</b> delivers short bursts so the average power is low and the effect is mostly non-thermal, unless the settings are high.</p>
      <p>Mean power:</p>
      <span class="formula">Mean power = Peak power × Pulse width × Pulse repetition rate</span>
      <p>Typical PSWD: pulse width 20 to 400 µs, repetition rate 15 to 800 pulses/s, peak power up to about 1000 W.</p>
    </div>

    <div class="card">
      <h3>Dosage (Schliephake scale)</h3>
      <div class="tablewrap">
      <table>
        <tr><th>Dose</th><th>Patient sensation</th><th>Use</th></tr>
        <tr><td>I</td><td>No sensation of warmth</td><td>Acute conditions</td></tr>
        <tr><td>II</td><td>Just perceptible warmth</td><td>Subacute</td></tr>
        <tr><td>III</td><td>Moderate, comfortable warmth</td><td>Subacute / chronic</td></tr>
        <tr><td>IV</td><td>Strong but tolerable warmth</td><td>Chronic, deep</td></tr>
      </table>
      </div>
      <p>Usual treatment time is 15 to 20 minutes.</p>
    </div>

    <div class="card">
      <h3>Effects and uses</h3>
      <ul>
        <li>Increased blood flow and metabolism, and reduced muscle spasm</li>
        <li>Pain relief and increased tissue extensibility</li>
        <li>Used in osteoarthritis, low back pain, sprains and strains, and chronic joint conditions</li>
        <li>Pulsed: used in soft-tissue injury and oedema</li>
      </ul>
    </div>

    <div class="card">
      <h3>Contraindications and precautions</h3>
      <ul>
        <li><b>Cardiac pacemakers</b> and other implanted electronic devices (interference)</li>
        <li><b>Metal implants</b> in the field (concentrated heating)</li>
        <li>Pregnancy (over the abdomen or pelvis), malignancy, active bleeding</li>
        <li>Impaired skin sensation or poor circulation</li>
        <li>Moist dressings or sweat (hot spots), and wet clothing</li>
        <li>Growing epiphyses in children, the eyes, and the testes</li>
        <li>Remove jewellery, watches, hearing aids and mobile phones from the area</li>
      </ul>
    </div>
  </div>
</section>

<hr class="sp">

<section id="sim">
  <h2>Shortwave diathermy simulator</h2>
  <p class="sub">The thermal map shows the temperature of each layer. Bright field lines mean the circuit is well tuned.</p>

  <div class="simwrap">
    <div>
      <canvas id="limb" width="600" height="430"></canvas>
      <div class="legend"></div>
      <div class="legend-l"><span>36 °C</span><span>40 °C</span><span>44 °C</span><span>48 °C</span></div>

      <div class="stats">
        <div class="stat"><small>Mean power</small><b id="sP">0 W</b></div>
        <div class="stat"><small>Tuning efficiency</small><b id="sE">0 %</b></div>
        <div class="stat"><small>Skin</small><b id="sS">37.0 °C</b></div>
        <div class="stat"><small>Fat</small><b id="sF">37.0 °C</b></div>
        <div class="stat"><small>Muscle</small><b id="sM">37.0 °C</b></div>
        <div class="stat"><small>Time</small><b id="sT">0:00</b></div>
        <div class="stat"><small>Patient dose</small><b id="sD">-</b></div>
        <div class="stat"><small>Burn events</small><b id="sB">0</b></div>
      </div>
      <div class="status" id="status">Press Start to begin treatment.</div>

      <h3 style="margin:22px 0 6px">Temperature recorder</h3>
      <canvas id="tchart" width="600" height="250"></canvas>
      <p class="sub" style="margin:6px 0 0;font-size:.85rem">Shaded band = therapeutic range 40 to 45 °C.</p>

      <h3 style="margin:22px 0 6px">Resonance curve</h3>
      <canvas id="rchart" width="600" height="230"></canvas>
      <p class="sub" style="margin:6px 0 0;font-size:.85rem">Dot = your tuning knob. Dashed dots = resonant positions found by the tuning sweep.</p>
    </div>

    <div class="card panel">
      <h3>Generator panel</h3>

      <div class="ctl">
        <label>Method</label>
        <div class="seg" id="segMethod">
          <button data-v="cap" class="on">Capacitor field</button>
          <button data-v="ind">Inductive coil</button>
        </div>
      </div>

      <div class="ctl">
        <label>Output mode</label>
        <div class="seg" id="segMode">
          <button data-v="cont" class="on">Continuous</button>
          <button data-v="puls">Pulsed</button>
        </div>
      </div>

      <div class="ctl">
        <label>Intensity <span id="vInt">30 %</span></label>
        <input type="range" id="rInt" min="0" max="100" value="30">
      </div>

      <div id="pulseCtl" style="display:none">
        <div class="ctl">
          <label>Pulse repetition rate <span id="vPrr">100 pps</span></label>
          <input type="range" id="rPrr" min="15" max="800" step="5" value="100">
        </div>
        <div class="ctl">
          <label>Pulse width <span id="vPw">100 µs</span></label>
          <input type="range" id="rPw" min="20" max="400" step="10" value="100">
        </div>
      </div>

      <div class="ctl">
        <label>Tuning knob <span id="vTune">50</span></label>
        <input type="range" id="rTune" min="0" max="100" value="50">
        <div class="meter" style="margin-top:6px"><i id="meterNeedle"></i></div>
        <div class="legend-l"><span>Detuned</span><span>Resonance</span></div>
      </div>

      <div class="ctl">
        <label>Electrode to skin gap <span id="vGap">3 cm</span></label>
        <input type="range" id="rGap" min="1" max="8" step="0.5" value="3">
      </div>

      <div class="ctl">
        <label>Subcutaneous fat thickness <span id="vFat">1.5 cm</span></label>
        <input type="range" id="rFat" min="0.3" max="3" step="0.1" value="1.5">
      </div>

      <div class="ctl">
        <label>Treatment time <span id="vDur">15 min</span></label>
        <input type="range" id="rDur" min="5" max="30" step="1" value="15">
      </div>

      <div class="ctl checks">
        <label>Hazards</label>
        <label><input type="checkbox" id="cImp"> Metal implant in the field</label>
        <label><input type="checkbox" id="cMoist"> Moist dressing / sweat on skin</label>
      </div>

      <div class="ctl">
        <label>Simulation speed</label>
        <select id="selSpeed">
          <option value="10">10×</option>
          <option value="30" selected>30×</option>
          <option value="60">60×</option>
        </select>
      </div>

      <div class="row">
        <button class="btn" id="bStart">Start</button>
        <button class="btn light" id="bReset">Reset</button>
      </div>
    </div>
  </div>
</section>

<hr class="sp">

<section id="exp">
  <h2>Experiment: tuning and heating pattern</h2>
  <p class="sub">Find the resonant tuning position for different electrode gaps, then compare how the two methods heat each tissue layer.</p>

  <div class="grid g2">
    <div class="card">
      <h3>Procedure</h3>
      <ol>
        <li>Select <b>Capacitor field</b> and a gap of 3 cm. Press <b>Run tuning sweep</b>. The knob scans 0 to 100 and stops at the highest meter reading. Note the position.</li>
        <li>Change the gap (at least four values) and repeat. Observe that the resonant position moves, so the unit must be retuned.</li>
        <li>Switch to <b>Inductive coil</b> and repeat the sweep.</li>
        <li>Keep the knob at resonance, use <b>Continuous</b> mode at 30% intensity, press <b>Start</b> and let the 15 min run finish. Press <b>Record heating</b>. Repeat with the other method.</li>
        <li>With the capacitor method, change the fat thickness from 0.5 cm to 3 cm and record each run.</li>
        <li>Switch to <b>Pulsed</b>. Compare 100 pps / 100 µs with 800 pps / 400 µs at the same intensity and look at the mean power and temperatures.</li>
        <li>Tick the hazard boxes one at a time and note the effect on skin and muscle temperature.</li>
      </ol>
      <div class="row">
        <button class="btn sec" id="bSweep">Run tuning sweep</button>
        <button class="btn" id="bRecord">Record heating</button>
        <button class="btn light" id="bClear">Clear tables</button>
      </div>
      <div class="callout" id="sweepMsg">Tuning sweep not run yet.</div>
    </div>

    <div class="card">
      <h3>Table 1: Resonant tuning position</h3>
      <div class="tablewrap">
      <table id="tabTune">
        <thead><tr><th>#</th><th>Method</th><th>Gap (cm)</th><th>Resonant knob</th><th>Meter (%)</th></tr></thead>
        <tbody><tr><td colspan="5">No readings yet</td></tr></tbody>
      </table>
      </div>
    </div>
  </div>

  <div class="card" style="margin-top:18px">
    <h3>Table 2: Heating observations</h3>
    <div class="tablewrap">
    <table id="tabHeat">
      <thead><tr><th>#</th><th>Method</th><th>Mode</th><th>Mean P (W)</th><th>Eff. (%)</th><th>Gap (cm)</th><th>Fat (cm)</th><th>Time (min)</th>
