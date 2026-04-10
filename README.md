# PROCESSDEMO
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>OptiMod Pro 1600 - Broadcast Audio Processor</title>
<style>
:root {
  --bg-primary: #0a0a0a;
  --bg-secondary: #1a1a1a;
  --bg-panel: #141414;
  --border-color: #333;
  --text-primary: #e0e0e0;
  --text-secondary: #888;
  --accent-amber: #ffb700;
  --accent-green: #39ff14;
  --accent-red: #ff3333;
  --accent-blue: #00aaff;
  --meter-green: #00ff41;
  --meter-yellow: #ffcc00;
  --meter-red: #ff0000;
  --knob-size: 52px;
  --knob-small: 38px;
  --fader-width: 18px;
  --fader-height: 140px;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'Segoe UI', 'Roboto', 'Arial', sans-serif;
  background: var(--bg-primary);
  color: var(--text-primary);
  overflow-x: hidden;
  min-height: 100vh;
}

/* HEADER */
.header {
  background: linear-gradient(180deg, #1a1a1a 0%, #0d0d0d 100%);
  border-bottom: 2px solid var(--accent-amber);
  padding: 10px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 20px rgba(255,183,0,0.1);
}

.logo { display: flex; align-items: center; gap: 15px; }
.logo-icon {
  width: 50px; height: 50px;
  background: radial-gradient(circle, var(--accent-amber) 0%, #cc8800 100%);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 20px; font-weight: bold; color: #000;
  box-shadow: 0 0 15px rgba(255,183,0,0.4);
}
.logo-text h1 { font-size: 18px; font-weight: 700; color: var(--accent-amber); letter-spacing: 3px; text-transform: uppercase; }
.logo-text span { font-size: 11px; color: var(--text-secondary); letter-spacing: 2px; }
.header-controls { display: flex; align-items: center; gap: 15px; }
.power-btn {
  width: 40px; height: 40px; border-radius: 50%; border: 2px solid #555;
  background: radial-gradient(circle, #333 0%, #111 100%); color: #666;
  cursor: pointer; display: flex; align-items: center; justify-content: center;
  font-size: 18px; transition: all 0.3s; box-shadow: 0 2px 5px rgba(0,0,0,0.5);
}
.power-btn.active { border-color: var(--accent-green); color: var(--accent-green); box-shadow: 0 0 15px rgba(57,255,20,0.3); }
.menu-btn { padding: 6px 14px; background: var(--bg-secondary); border: 1px solid var(--border-color); color: var(--text-primary); border-radius: 4px; cursor: pointer; font-size: 12px; transition: all 0.2s; }
.menu-btn:hover { background: #2a2a2a; border-color: var(--accent-amber); }

/* MAIN LAYOUT */
.main-container { display: flex; flex-direction: column; padding: 10px; gap: 10px; max-width: 1600px; margin: 0 auto; }
.top-section { display: grid; grid-template-columns: 1fr 2fr 1fr; gap: 10px; }
.panel { background: var(--bg-panel); border: 1px solid var(--border-color); border-radius: 8px; padding: 12px; position: relative; overflow: hidden; }
.panel::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: linear-gradient(90deg, transparent, rgba(255,183,0,0.3), transparent); }
.panel-title { font-size: 10px; text-transform: uppercase; letter-spacing: 2px; color: var(--accent-amber); margin-bottom: 10px; text-align: center; font-weight: 600; }

/* KNOB STYLES */
.knob-container { display: flex; flex-direction: column; align-items: center; gap: 4px; }
.knob-wrapper { position: relative; cursor: grab; }
.knob-wrapper:active { cursor: grabbing; }
.knob {
  width: var(--knob-size); height: var(--knob-size); border-radius: 50%;
  background: conic-gradient(from 220deg, #444 0%, #222 30%, #111 50%, #333 70%, #222 100%);
  box-shadow: 0 4px 10px rgba(0,0,0,0.6), inset 0 1px 2px rgba(255,255,255,0.1), 0 0 0 1px #111;
  position: relative; transition: box-shadow 0.1s;
}
.knob::before { content: ''; position: absolute; top: 50%; left: 50%; width: 2px; height: 35%; background: var(--accent-amber); transform-origin: bottom center; transform: translate(-50%, -100%) rotate(var(--knob-angle, 0deg)); border-radius: 1px; box-shadow: 0 0 4px rgba(255,183,0,0.5); }
.knob::after { content: ''; position: absolute; top: 50%; left: 50%; width: 8px; height: 8px; background: #444; border-radius: 50%; transform: translate(-50%, -50%); box-shadow: inset 0 1px 2px rgba(0,0,0,0.5); }
.knob.small { width: var(--knob-small); height: var(--knob-small); }
.knob.small::before { height: 30%; width: 1.5px; }
.knob-value { font-size: 10px; color: var(--accent-green); font-family: 'Courier New', monospace; font-weight: bold; min-height: 14px; }
.knob-label { font-size: 9px; color: var(--text-secondary); text-transform: uppercase; letter-spacing: 1px; text-align: center; }

/* METER STYLES */
.meter-container { display: flex; gap: 8px; justify-content: center; align-items: flex-end; height: 120px; padding: 5px; }
.meter { width: 30px; height: 100%; background: #0a0a0a; border-radius: 4px; position: relative; overflow: hidden; border: 1px solid #333; }
.meter-fill { position: absolute; bottom: 0; left: 0; right: 0; height: 0%; background: linear-gradient(0deg, var(--meter-green) 0%, var(--meter-green) 60%, var(--meter-yellow) 80%, var(--meter-red) 100%); transition: height 0.05s; border-radius: 0 0 3px 3px; }
.meter-peak { position: absolute; bottom: 0; left: 0; right: 0; height: 2px; background: #fff; transition: bottom 0.02s; }
.meter-label { text-align: center; font-size: 9px; color: var(--text-secondary); margin-top: 4px; letter-spacing: 1px; }

/* SPECTRUM */
.spectrum-panel { grid-column: 1 / -1; }
.spectrum-canvas { width: 100%; height: 100px; background: #050505; border-radius: 4px; border: 1px solid #222; }

/* DSP PANELS */
.dsp-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 10px; }
.dsp-panel { background: var(--bg-panel); border: 1px solid var(--border-color); border-radius: 8px; padding: 12px; position: relative; }
.dsp-panel::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: linear-gradient(90deg, transparent, rgba(57,255,20,0.2), transparent); }
.dsp-panel-title { font-size: 10px; text-transform: uppercase; letter-spacing: 2px; color: var(--accent-green); margin-bottom: 8px; display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
.dsp-panel-title .status-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent-green); box-shadow: 0 0 6px rgba(57,255,20,0.5); }

/* BYPASS BUTTON */
.dsp-panel-title .bypass-btn { margin-left: auto; padding: 4px 12px; background: rgba(255,51,51,0.15); border: 1px solid var(--accent-red); color: var(--accent-red); border-radius: 4px; cursor: pointer; font-size: 9px; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; transition: all 0.2s; }
.dsp-panel-title .bypass-btn:hover { border-color: var(--accent-amber); background: rgba(255,183,0,0.15); color: var(--accent-amber); }
.dsp-panel-title .bypass-btn.active { background: rgba(57,255,20,0.2); border-color: var(--accent-green); color: var(--accent-green); box-shadow: 0 0 8px rgba(57,255,20,0.3); }

.knobs-grid { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; }

/* GRAPHIC EQ DISPLAY */
.eq-display { width: 100%; height: 70px; background: #050505; border-radius: 4px; margin-bottom: 8px; border: 1px solid #222; }

/* BAND SECTION */
.band-section { background: rgba(255,255,255,0.02); border: 1px solid rgba(255,255,255,0.05); border-radius: 6px; padding: 8px; margin-bottom: 8px; }
.band-title { font-size: 9px; color: var(--accent-amber); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; text-align: center; }

/* BUTTONS */
.toggle-btn { padding: 4px 10px; background: #222; border: 1px solid #444; color: var(--text-secondary); border-radius: 3px; cursor: pointer; font-size: 10px; text-transform: uppercase; letter-spacing: 1px; transition: all 0.2s; }
.toggle-btn.active { background: rgba(57,255,20,0.15); border-color: var(--accent-green); color: var(--accent-green); }
.toggle-btn:hover { border-color: var(--accent-amber); }
.select-styled { background: #1a1a1a; border: 1px solid #333; color: var(--text-primary); padding: 4px 8px; border-radius: 3px; font-size: 10px; cursor: pointer; }
.presets-bar { display: flex; gap: 6px; flex-wrap: wrap; justify-content: center; padding: 8px 0; }
.preset-btn { padding: 5px 12px; background: linear-gradient(180deg, #2a2a2a 0%, #1a1a1a 100%); border: 1px solid #444; color: var(--text-primary); border-radius: 3px; cursor: pointer; font-size: 10px; text-transform: uppercase; letter-spacing: 1px; transition: all 0.2s; }
.preset-btn:hover { border-color: var(--accent-amber); background: linear-gradient(180deg, #333 0%, #222 100%); }
.preset-btn.active { border-color: var(--accent-amber); background: linear-gradient(180deg, rgba(255,183,0,0.2) 0%, rgba(255,183,0,0.05) 100%); color: var(--accent-amber); }

/* CONFIG */
.config-overlay { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.85); z-index: 1000; justify-content: center; align-items: center; }
.config-overlay.visible { display: flex; }
.config-panel { background: var(--bg-secondary); border: 1px solid var(--accent-amber); border-radius: 12px; padding: 25px; width: 90%; max-width: 500px; max-height: 80vh; overflow-y: auto; }
.config-panel h2 { color: var(--accent-amber); font-size: 16px; margin-bottom: 15px; text-transform: uppercase; letter-spacing: 2px; }
.config-section { margin-bottom: 15px; padding: 12px; background: rgba(255,255,255,0.03); border-radius: 6px; border: 1px solid #333; }
.config-section h3 { font-size: 11px; color: var(--accent-green); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }
.config-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; }
.config-row label { font-size: 11px; color: var(--text-secondary); }
.config-row select, .config-row input { background: #111; border: 1px solid #444; color: var(--text-primary); padding: 4px 8px; border-radius: 3px; font-size: 11px; }
.config-close { margin-top: 15px; padding: 8px 20px; background: var(--accent-amber); border: none; color: #000; font-weight: bold; border-radius: 4px; cursor: pointer; width: 100%; text-transform: uppercase; letter-spacing: 2px; }
.mode-selector { display: flex; gap: 4px; justify-content: center; }
.mode-btn { padding: 6px 16px; background: #1a1a1a; border: 1px solid #333; color: var(--text-secondary); border-radius: 3px; cursor: pointer; font-size: 10px; text-transform: uppercase; letter-spacing: 1px; transition: all 0.2s; }
.mode-btn.active { background: rgba(255,183,0,0.15); border-color: var(--accent-amber); color: var(--accent-amber); }

/* SCROLLBAR */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg-primary); }
::-webkit-scrollbar-thumb { background: #444; border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: #555; }

/* RESPONSIVE */
@media (max-width: 768px) {
  .top-section { grid-template-columns: 1fr; }
  .dsp-row { grid-template-columns: 1fr; }
  .header { flex-direction: column; gap: 10px; }
  .knob { --knob-size: 42px; }
  .knob.small { --knob-small: 32px; }
}

@keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.5; } }
.processing-active { animation: pulse 2s infinite; }
.tooltip { position: absolute; background: rgba(0,0,0,0.9); border: 1px solid var(--accent-amber); color: var(--text-primary); padding: 4px 8px; border-radius: 4px; font-size: 10px; pointer-events: none; z-index: 100; display: none; white-space: nowrap; }
.gr-meter { width: 100%; height: 8px; background: #111; border-radius: 4px; overflow: hidden; margin-top: 4px; }
.gr-meter-fill { height: 100%; background: linear-gradient(90deg, var(--accent-green), #ffcc00, var(--accent-red)); width: 0%; transition: width 0.05s; border-radius: 4px; }
.file-input { display: none; }
.status-bar { display: flex; justify-content: space-between; align-items: center; padding: 4px 12px; background: #0a0a0a; border-top: 1px solid #222; font-size: 10px; color: #555; }
.status-bar .status-item { display: flex; align-items: center; gap: 5px; }
.status-bar .indicator { width: 5px; height: 5px; border-radius: 50%; }
.status-bar .indicator.green { background: var(--accent-green); }
.status-bar .indicator.amber { background: var(--accent-amber); }
.status-bar .indicator.red { background: var(--accent-red); }

/* ============================================ */
/* GRAPHIC EQ 31 BANDS - FADER STYLES           */
/* ============================================ */
.geq-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 4px;
  background: rgba(0,0,0,0.3);
  border-radius: 6px;
  border: 1px solid #222;
}

.geq-faders {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 2px;
  padding: 4px 0;
  width: 100%;
  overflow-x: auto;
  scrollbar-width: thin;
  scrollbar-color: #444 #111;
}

.geq-faders::-webkit-scrollbar { height: 4px; }
.geq-faders::-webkit-scrollbar-track { background: #111; border-radius: 2px; }
.geq-faders::-webkit-scrollbar-thumb { background: #444; border-radius: 2px; }

.geq-channel {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 28px;
  flex-shrink: 0;
}

.geq-value {
  font-size: 8px;
  color: var(--accent-green);
  font-family: 'Courier New', monospace;
  font-weight: bold;
  height: 12px;
  margin-bottom: 2px;
  text-align: center;
  white-space: nowrap;
}

.geq-fader-track {
  position: relative;
  width: 16px;
  height: var(--fader-height);
  background: linear-gradient(180deg, #1a1a1a 0%, #0d0d0d 50%, #1a1a1a 100%);
  border-radius: 8px;
  border: 1px solid #333;
  cursor: pointer;
  touch-action: none;
}

.geq-fader-track::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 2px;
  background: linear-gradient(180deg, var(--accent-green) 0%, var(--accent-green) 45%, var(--accent-amber) 48%, var(--accent-amber) 52%, var(--accent-green) 55%, var(--accent-green) 100%);
  transform: translateX(-50%);
  opacity: 0.3;
  border-radius: 1px;
}

.geq-fader-thumb {
  position: absolute;
  left: 50%;
  width: 14px;
  height: 8px;
  background: linear-gradient(180deg, #666 0%, #444 50%, #555 100%);
  border: 1px solid #777;
  border-radius: 2px;
  transform: translate(-50%, 50%);
  box-shadow: 0 1px 3px rgba(0,0,0,0.5);
  pointer-events: none;
}

.geq-fader-thumb::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 10px;
  height: 1px;
  background: #888;
  transform: translate(-50%, -50%);
}

.geq-freq-label {
  font-size: 7px;
  color: var(--text-secondary);
  font-family: 'Courier New', monospace;
  margin-top: 3px;
  text-align: center;
  white-space: nowrap;
  transform: rotate(0deg);
  height: 10px;
}

.geq-freq-label.highlight {
  color: var(--accent-amber);
  font-weight: bold;
}

/* EQ Reset button */
.geq-reset-btn {
  padding: 4px 10px;
  background: rgba(255,51,51,0.15);
  border: 1px solid var(--accent-red);
  color: var(--accent-red);
  border-radius: 3px;
  cursor: pointer;
  font-size: 9px;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.2s;
}
.geq-reset-btn:hover {
  background: rgba(255,183,0,0.15);
  border-color: var(--accent-amber);
  color: var(--accent-amber);
}
</style>
</head>
<body>

<header class="header">
  <div class="logo">
    <div class="logo-icon">🎛️</div>
    <div class="logo-text">
      <h1>OptiMod Pro 1600</h1>
      <span>Broadcast Audio Processor</span>
    </div>
  </div>
  <div class="header-controls">
    <select class="select-styled" id="modeSelect">
      <option value="streaming">📡 Streaming</option>
      <option value="radio">📻 Radio FM</option>
      <option value="mastering">🎵 Mastering</option>
      <option value="podcast">🎙️ Podcast</option>
    </select>
    <button class="menu-btn" id="presetsBtn">💾 Presets</button>
    <button class="menu-btn" id="configBtn">⚙️ Config</button>
    <button class="power-btn" id="powerBtn" title="Power ON/OFF">⏻</button>
  </div>
</header>

<div class="presets-bar" id="presetsBar">
  <button class="preset-btn" data-preset="bypass">Bypass</button>
  <button class="preset-btn" data-preset="fm-hot">FM Hot</button>
  <button class="preset-btn" data-preset="fm-clean">FM Clean</button>
  <button class="preset-btn" data-preset="streaming">Streaming</button>
  <button class="preset-btn" data-preset="podcast">Podcast</button>
  <button class="preset-btn" data-preset="voice">Voice</button>
  <button class="preset-btn" data-preset="music">Music</button>
  <button class="preset-btn" data-preset="rock">Rock</button>
  <button class="preset-btn" data-preset="classical">Classical</button>
  <button class="preset-btn" data-preset="bass-boost">Bass Boost</button>
  <button class="preset-btn" data-preset="bright">Bright</button>
  <button class="menu-btn" id="exportBtn">📤 Export</button>
  <button class="menu-btn" id="importBtn">📥 Import</button>
  <input type="file" class="file-input" id="importFile" accept=".json">
</div>

<div class="main-container">
  <div class="top-section">
    <div class="panel">
      <div class="panel-title">📥 Input</div>
      <div class="knobs-grid">
        <div class="knob-container"><div class="knob-wrapper" data-param="inputGain" data-min="-24" data-max="24" data-default="0"><div class="knob"></div></div><div class="knob-value">0.0 dB</div><div class="knob-label">Gain</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="inputPan" data-min="-100" data-max="100" data-default="0"><div class="knob"></div></div><div class="knob-value">C</div><div class="knob-label">Pan</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="inputPhase" data-min="0" data-max="1" data-default="0"><div class="knob small"></div></div><div class="knob-value">NOR</div><div class="knob-label">Phase</div></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title">📊 Level Meters</div>
      <div style="display:flex; gap:15px; justify-content:center;">
        <div><div class="meter-container"><div class="meter"><div class="meter-fill" id="inputMeterL"></div><div class="meter-peak" id="inputPeakL"></div></div><div class="meter"><div class="meter-fill" id="inputMeterR"></div><div class="meter-peak" id="inputPeakR"></div></div></div><div class="meter-label">INPUT</div></div>
        <div><div class="meter-container"><div class="meter"><div class="meter-fill" id="procMeterL"></div><div class="meter-peak" id="procPeakL"></div></div><div class="meter"><div class="meter-fill" id="procMeterR"></div><div class="meter-peak" id="procPeakR"></div></div></div><div class="meter-label">PROCESSED</div></div>
        <div><div class="meter-container"><div class="meter"><div class="meter-fill" id="outputMeterL"></div><div class="meter-peak" id="outputPeakL"></div></div><div class="meter"><div class="meter-fill" id="outputMeterR"></div><div class="meter-peak" id="outputPeakR"></div></div></div><div class="meter-label">OUTPUT</div></div>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title">📤 Output</div>
      <div class="knobs-grid">
        <div class="knob-container"><div class="knob-wrapper" data-param="outputGain" data-min="-24" data-max="12" data-default="0"><div class="knob"></div></div><div class="knob-value">0.0 dB</div><div class="knob-label">Gain</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="outputLimit" data-min="0" data-max="100" data-default="100"><div class="knob"></div></div><div class="knob-value">100%</div><div class="knob-label">Limit</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="stereoMode" data-min="0" data-max="2" data-default="0"><div class="knob small"></div></div><div class="knob-value">ST</div><div class="knob-label">Stereo</div></div>
      </div>
    </div>
  </div>

  <div class="panel spectrum-panel">
    <div class="panel-title">📈 Spectrum Analyzer</div>
    <canvas class="spectrum-canvas" id="spectrumCanvas"></canvas>
  </div>

  <!-- GRAPHIC EQ 31 BANDAS -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>Ecualizador Gráfico 31 Bandas
      <button class="bypass-btn active" data-module="geq">ACTIVE</button>
      <button class="geq-reset-btn" id="geqResetBtn">↺ Reset</button>
    </div>
    <canvas class="eq-display" id="eqCanvas"></canvas>
    <div class="geq-container">
      <div class="geq-faders" id="geqFaders"></div>
    </div>
  </div>

  <!-- AGC PANEL -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>AGC - Automatic Gain Control
      <button class="bypass-btn active" data-module="agc">ACTIVE</button>
    </div>
    <div class="knobs-grid" id="agcKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="agcGrMeter"></div></div>
  </div>

  <!-- 3-BAND COMPRESSOR -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>Compresor 3 Bandas
      <button class="bypass-btn active" data-module="comp3">ACTIVE</button>
    </div>
    <div id="comp3Knobs"></div>
  </div>

  <!-- BAND LIMITER 3 BANDAS -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>Band Limiter 3 Bandas
      <button class="bypass-btn active" data-module="limiter3">ACTIVE</button>
    </div>
    <div id="limiter3Knobs"></div>
  </div>

  <!-- FINAL LIMITER -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>Limiter Final
      <button class="bypass-btn active" data-module="finalLimiter">ACTIVE</button>
    </div>
    <div class="knobs-grid" id="finalLimiterKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="limGrMeter"></div></div>
  </div>

  <!-- POWER LIMITER -->
  <div class="dsp-panel">
    <div class="dsp-panel-title">
      <span class="status-dot"></span>Power Limiter
      <button class="bypass-btn active" data-module="powerLimiter">ACTIVE</button>
    </div>
    <div class="knobs-grid" id="powerLimiterKnobs"></div>
    <div class="gr-meter"><div class="gr-meter-fill" id="powLimGrMeter"></div></div>
  </div>

  <!-- PRE-EMPHASIS & STEREO -->
  <div class="dsp-row">
    <div class="dsp-panel">
      <div class="dsp-panel-title">
        <span class="status-dot"></span>Pre-Emphasis
        <button class="bypass-btn active" data-module="preEmphasis">ACTIVE</button>
      </div>
      <div class="knobs-grid">
        <div class="knob-container"><div class="knob-wrapper" data-param="preEmphasis" data-min="0" data-max="1" data-default="0"><div class="knob small"></div></div><div class="knob-value">50μs</div><div class="knob-label">Time</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="preEmphasisGain" data-min="0" data-max="12" data-default="0"><div class="knob small"></div></div><div class="knob-value">0 dB</div><div class="knob-label">Amount</div></div>
      </div>
    </div>
    <div class="dsp-panel">
      <div class="dsp-panel-title"><span class="status-dot"></span>Stereo Mode</div>
      <div class="mode-selector" style="margin-bottom:10px;">
        <button class="mode-btn active" data-mode="stereo">Stereo</button>
        <button class="mode-btn" data-mode="mono">Mono</button>
        <button class="mode-btn" data-mode="mpx">MPX</button>
      </div>
      <div class="knobs-grid">
        <div class="knob-container"><div class="knob-wrapper" data-param="stereoWidth" data-min="0" data-max="200" data-default="100"><div class="knob"></div></div><div class="knob-value">100%</div><div class="knob-label">Width</div></div>
        <div class="knob-container"><div class="knob-wrapper" data-param="stereoBalance" data-min="-100" data-max="100" data-default="0"><div class="knob"></div></div><div class="knob-value">C</div><div class="knob-label">Balance</div></div>
      </div>
    </div>
  </div>
</div>

<div class="status-bar">
  <div class="status-item"><span class="indicator green"></span> DSP: <span id="dspStatus">Ready</span></div>
  <div class="status-item"><span class="indicator amber"></span> Sample Rate: <span id="sampleRateDisplay">--</span></div>
  <div class="status-item"><span class="indicator green"></span> Buffer: <span id="bufferSizeDisplay">--</span></div>
  <div class="status-item">Latency: <span id="latencyDisplay">--</span></div>
  <div class="status-item">CPU: <span id="cpuDisplay">--</span></div>
  <div class="status-item"><span class="indicator" id="clipIndicator"></span> Clip</div>
</div>

<div class="config-overlay" id="configOverlay">
  <div class="config-panel">
    <h2>⚙️ Configuración General</h2>
    <div class="config-section">
      <h3>🔊 Audio</h3>
      <div class="config-row"><label>Dispositivo Entrada</label><select id="inputDevice" style="width:250px; height:20px;"></select></div>
      <div class="config-row"><label>Dispositivo Salida</label><select id="outputDevice" style="width:250px; height:20px;"></select></div>
      <div class="config-row"><label>Sample Rate</label><select id="configSampleRate"><option value="44100">44100 Hz</option><option value="48000" selected>48000 Hz</option><option value="96000">96000 Hz</option></select></div>
      <div class="config-row"><label>Buffer Size</label><select id="configBufferSize"><option value="128">128 samples</option><option value="256" selected>256 samples</option><option value="512">512 samples</option><option value="1024">1024 samples</option></select></div>
    </div>
    <div class="config-section">
      <h3>📊 AGC</h3>
      <div class="config-row"><label>Modo AGC</label><select id="agcMode"><option value="smooth">Smooth</option><option value="aggressive">Aggressive</option><option value="broadcast">Broadcast</option></select></div>
      <div class="config-row"><label>Lookahead (ms)</label><input type="number" id="agcLookahead" value="2" min="0" max="20"></div>
    </div>
    <div class="config-section">
      <h3>🔧 DSP</h3>
      <div class="config-row"><label>Oversampling</label><select id="oversampling"><option value="1">1x</option><option value="2" selected>2x</option><option value="4">4x</option></select></div>
      <div class="config-row"><label>Anti-Clipping</label><select id="antiClipping"><option value="on" selected>On</option><option value="off">Off</option></select></div>
      <div class="config-row"><label>Saturación Armónica</label><select id="harmonicSat"><option value="0">Off</option><option value="1" selected>Low</option><option value="2">Medium</option><option value="3">High</option></select></div>
    </div>
    <button class="config-close" id="configClose">Cerrar</button>
  </div>
</div>

<div class="tooltip" id="tooltip"></div>

<script>
// ============================================================
// OptiMod Pro 1600 - Broadcast Audio Processor
// Graphic EQ 31 Bands + Bypass controls
// ============================================================

class BroadcastProcessor {
  constructor() {
    this.audioCtx = null;
    this.audioWorklet = null;
    this.inputStream = null;
    this.isEnabled = false;
    this.sampleRate = 48000;
    this.bufferSize = 256;
    this.params = {};
    this.meters = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.peaks = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.peakHold = { inputL: 0, inputR: 0, outputL: 0, outputR: 0 };
    this.spectrumData = new Float32Array(128);
    this.eqData = new Float32Array(128);
    this.clipped = false;
    this.cpuLoad = 0;
    this.lastFrameTime = performance.now();
    this.frameCount = 0;
    
    // ISO 1/3 Octave frequencies for 31-band GEQ
    this.geqFrequencies = [20, 25, 31.5, 40, 50, 63, 80, 100, 125, 160, 200, 250, 315, 400, 500, 630, 800, 1000, 1250, 1600, 2000, 2500, 3150, 4000, 5000, 6300, 8000, 10000, 12500, 16000, 20000];
    
    this.initDefaultParams();
    this.initUI();
    this.initBypassButtons();
    this.initKnobs();
    this.initGEQFaders();
    this.initSpectrum();
    this.initEQDisplay();
    this.loadPreset('fm-hot');
  }

  initDefaultParams() {
    this.params.inputGain = 0;
    this.params.inputPan = 0;
    this.params.inputPhase = 0;
    
    // Graphic EQ 31 bands
    this.params.geq = new Float32Array(31);
    this.params.geqMaster = { enabled: true };
    
    // AGC
    this.params.agc = { threshold: -20, ratio: 3, attack: 10, release: 100, enabled: true };
    
    // 3-Band Compressor
    this.params.comp3 = [
      { name: 'Low', freq: 200, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, enabled: true },
      { name: 'Mid', freq: 2000, threshold: -18, ratio: 3.5, attack: 10, release: 60, gain: 0, enabled: true },
      { name: 'High', freq: 8000, threshold: -15, ratio: 3, attack: 5, release: 40, gain: 0, enabled: true }
    ];
    this.params.comp3Master = { enabled: true };

    // 3-Band Limiter
    this.params.limiter3 = [
      { name: 'Low', freq: 200, threshold: -3, ratio: 20, attack: 1, release: 50, gain: 0, enabled: true },
      { name: 'Mid', freq: 2000, threshold: -3, ratio: 20, attack: 1, release: 40, gain: 0, enabled: true },
      { name: 'High', freq: 8000, threshold: -3, ratio: 20, attack: 0.5, release: 30, gain: 0, enabled: true }
    ];
    this.params.limiter3Master = { enabled: true };

    // Final Limiter
    this.params.finalLimiter = { threshold: -1, ratio: 20, attack: 0.5, release: 50, gain: 0, enabled: true };
    this.params.powerLimiter = { threshold: -0.5, ratio: 30, attack: 0.1, release: 20, gain: 0, enabled: true };
    this.params.preEmphasis = { time: 0, gain: 0, enabled: true };
    this.params.outputGain = 0;
    this.params.outputLimit = 100;
    this.params.stereoMode = 0;
    this.params.stereoWidth = 100;
    this.params.stereoBalance = 0;
    this.params.mode = 'streaming';
    this.params.inputDevice = localStorage.getItem('inputDevice') || 'default';
    this.params.outputDevice = localStorage.getItem('outputDevice') || 'default';
  }

  async initAudio() {
    try {
      this.audioCtx = new (window.AudioContext || window.webkitAudioContext)({ sampleRate: this.sampleRate, latencyHint: 'interactive' });
      document.getElementById('sampleRateDisplay').textContent = this.audioCtx.sampleRate + ' Hz';
      document.getElementById('bufferSizeDisplay').textContent = this.bufferSize + ' samples';

      const workletCode = this.generateWorkletCode();
      const blob = new Blob([workletCode], { type: 'application/javascript' });
      const workletUrl = URL.createObjectURL(blob);
      await this.audioCtx.audioWorklet.addModule(workletUrl);
      URL.revokeObjectURL(workletUrl);

      this.audioWorklet = new AudioWorkletNode(this.audioCtx, 'broadcast-processor', { outputChannelCount: [2], processorOptions: { sampleRate: this.audioCtx.sampleRate } });
      this.audioWorklet.port.onmessage = (e) => {
        if (e.data.type === 'meters') { this.meters = e.data.meters; this.updateMeters(); }
        if (e.data.type === 'spectrum') { this.spectrumData = new Float32Array(e.data.spectrum); this.drawSpectrum(); }
        if (e.data.type === 'eqcurve') { this.eqData = new Float32Array(e.data.eqcurve); this.drawEQCurve(); }
        if (e.data.type === 'gr') this.updateGRMeters(e.data.gr);
        if (e.data.type === 'clip') {
          this.clipped = true;
          setTimeout(() => this.clipped = false, 200);
          const ci = document.getElementById('clipIndicator');
          ci.style.background = 'var(--accent-red)';
          ci.style.boxShadow = '0 0 6px rgba(255,0,0,0.8)';
        }
        if (e.data.type === 'cpu') { this.cpuLoad = e.data.cpu; document.getElementById('cpuDisplay').textContent = this.cpuLoad.toFixed(1) + '%'; }
      };

      const constraints = { audio: { echoCancellation: false, noiseSuppression: false, autoGainControl: false, sampleRate: this.sampleRate } };
      if (this.params.inputDevice && this.params.inputDevice !== 'default') constraints.audio.deviceId = { exact: this.params.inputDevice };
      this.inputStream = await navigator.mediaDevices.getUserMedia(constraints);
      const source = this.audioCtx.createMediaStreamSource(this.inputStream);
      if (this.params.outputDevice && this.params.outputDevice !== 'default' && this.audioCtx.setSinkId) await this.audioCtx.setSinkId(this.params.outputDevice);
      source.connect(this.audioWorklet);
      this.audioWorklet.connect(this.audioCtx.destination);

      this.isEnabled = true;
      document.getElementById('dspStatus').textContent = 'Active';
      document.getElementById('powerBtn').classList.add('active');
      this.sendAllParams();
    } catch (err) {
      console.error('Audio init error:', err);
      document.getElementById('dspStatus').textContent = 'Error: ' + err.message;
    }
  }

  generateWorkletCode() {
    return `
    class BroadcastProcessor extends AudioWorkletProcessor {
      constructor(options) {
        super();
        this.sampleRate = options.sampleRate || 48000;
        
        this.geqMasterEnabled = true;
        this.agcEnabled = true;
        this.comp3MasterEnabled = true;
        this.limiter3MasterEnabled = true;
        this.finalLimiterEnabled = true;
        this.powerLimiterEnabled = true;
        this.preEmphasisEnabled = true;
        
        this.inputGain = 1.0;
        this.inputPan = 0.0;
        this.outputGain = 1.0;
        this.stereoWidth = 1.0;
        this.stereoBalance = 0.0;
        this.stereoMode = 0;
        
        // Graphic EQ 31 bands - peaking filters
        this.geqFrequencies = [20, 25, 31.5, 40, 50, 63, 80, 100, 125, 160, 200, 250, 315, 400, 500, 630, 800, 1000, 1250, 1600, 2000, 2500, 3150, 4000, 5000, 6300, 8000, 10000, 12500, 16000, 20000];
        this.geqGains = new Float32Array(31);
        this.geqQ = 4.318; // 1/3 octave Q
        this.geqFilters = [];
        this.initGEQ();
        
        // AGC
        this.agcState = { threshold: -20, ratio: 3, attack: 10, release: 100, enabled: true };
        this.agcEnvelope = 0;
        this.agcGain = 1.0;
        
        // 3-Band Compressor
        this.comp3 = [
          { freq: 200, threshold: -20, ratio: 4, attack: 15, release: 80, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 2000, threshold: -18, ratio: 3.5, attack: 10, release: 60, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 8000, threshold: -15, ratio: 3, attack: 5, release: 40, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null }
        ];
        this.initCrossovers3('comp3');
        
        // 3-Band Limiter
        this.limiter3 = [
          { freq: 200, threshold: -3, ratio: 20, attack: 1, release: 50, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 2000, threshold: -3, ratio: 20, attack: 1, release: 40, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null },
          { freq: 8000, threshold: -3, ratio: 20, attack: 0.5, release: 30, gain: 0, envelope: 0, g: 1.0, lowFilter: null, highFilter: null }
        ];
        this.initCrossovers3('limiter3');
        
        this.finalLimiter = { threshold: -1, ratio: 20, attack: 0.5, release: 50, gain: 0, envelope: 0, g: 1.0 };
        this.powerLimiter = { threshold: -0.5, ratio: 30, attack: 0.1, release: 20, gain: 0, envelope: 0, g: 1.0 };
        this.preEmphasisTime = 0;
        this.preEmphasisGain = 0;
        this.preEmphState = 0;
        
        this.meterInputL = 0; this.meterInputR = 0;
        this.meterOutputL = 0; this.meterOutputR = 0;
        this.meterPeakIL = 0; this.meterPeakIR = 0;
        this.meterPeakOL = 0; this.meterPeakOR = 0;
        this.spectrumBuffer = new Float32Array(256);
        this.spectrumIdx = 0;
        this.saturationAmount = 0.1;
        this.antiClipEnabled = true;
        this.meterUpdateInterval = Math.floor(this.sampleRate / 30);
        this.frameCounter = 0;
        
        this.port.onmessage = (e) => this.handleMessage(e.data);
      }
      
      initGEQ() {
        this.geqFilters = [];
        for (let i = 0; i < 31; i++) {
          this.geqFilters.push({
            freq: this.geqFrequencies[i], gain: 0, q: this.geqQ,
            b0:0,b1:0,b2:0,a1:0,a2:0,
            x1L:0,x2L:0,y1L:0,y2L:0,
            x1R:0,x2R:0,y1R:0,y2R:0
          });
          this.updateGEQCoeffs(i);
        }
      }
      
      updateGEQCoeffs(index) {
        const f = this.geqFilters[index];
        const sr = this.sampleRate;
        const freq = Math.max(20, Math.min(20000, f.freq));
        const gain = this.geqGains[index];
        const q = f.q;
        const A = Math.pow(10, gain / 40);
        const omega = 2 * Math.PI * freq / sr;
        const sinW = Math.sin(omega);
        const cosW = Math.cos(omega);
        const alpha = sinW / (2 * q);
        
        // Peaking EQ biquad
        const b0 = 1 + alpha * A;
        const b1 = -2 * cosW;
        const b2 = 1 - alpha * A;
        const a0 = 1 + alpha / A;
        const a1 = -2 * cosW;
        const a2 = 1 - alpha / A;
        
        f.b0 = b0/a0; f.b1 = b1/a0; f.b2 = b2/a0;
        f.a1 = a1/a0; f.a2 = a2/a0;
      }
      
      processGEQ(xL, xR) {
        let yL = xL, yR = xR;
        for (let i = 0; i < 31; i++) {
          const f = this.geqFilters[i];
          if (Math.abs(this.geqGains[i]) > 0.01) {
            const nL = f.b0*yL + f.b1*f.x1L + f.b2*f.x2L - f.a1*f.y1L - f.a2*f.y2L;
            const nR = f.b0*yR + f.b1*f.x1R + f.b2*f.x2R - f.a1*f.y1R - f.a2*f.y2R;
            f.x2L=f.x1L; f.x1L=yL; f.y2L=f.y1L; f.y1L=nL;
            f.x2R=f.x1R; f.x1R=yR; f.y2R=f.y1R; f.y1R=nR;
            yL = nL; yR = nR;
          }
        }
        return { yL, yR };
      }
      
      handleMessage(data) {
        switch(data.type) {
          case 'param': this.setParam(data.key, data.value); break;
          case 'geq':
            this.geqGains[data.index] = data.value;
            this.updateGEQCoeffs(data.index);
            break;
          case 'geqReset':
            for(let i=0;i<31;i++){this.geqGains[i]=0; this.updateGEQCoeffs(i);}
            break;
          case 'geqMaster': this.geqMasterEnabled = data.enabled; break;
          case 'agc': Object.assign(this.agcState, data.params); break;
          case 'agcEnabled': this.agcEnabled = data.enabled; break;
          case 'comp3':
            if(this.comp3[data.index]) Object.assign(this.comp3[data.index], data.params);
            this.initCrossovers3('comp3');
            break;
          case 'comp3Master': this.comp3MasterEnabled = data.enabled; break;
          case 'limiter3':
            if(this.limiter3[data.index]) Object.assign(this.limiter3[data.index], data.params);
            this.initCrossovers3('limiter3');
            break;
          case 'limiter3Master': this.limiter3MasterEnabled = data.enabled; break;
          case 'finalLimiter': Object.assign(this.finalLimiter, data.params); break;
          case 'finalLimiterEnabled': this.finalLimiterEnabled = data.enabled; break;
          case 'powerLimiter': Object.assign(this.powerLimiter, data.params); break;
          case 'powerLimiterEnabled': this.powerLimiterEnabled = data.enabled; break;
          case 'preEmphasis': this.preEmphasisTime = data.value; break;
          case 'preEmphasisGain': this.preEmphasisGain = data.value; break;
          case 'preEmphasisEnabled': this.preEmphasisEnabled = data.enabled; break;
        }
      }
      
      setParam(key, value) {
        switch(key) {
          case 'inputGain': this.inputGain = Math.pow(10, value/20); break;
          case 'inputPan': this.inputPan = value/100; break;
          case 'outputGain': this.outputGain = Math.pow(10, value/20); break;
          case 'outputLimit': this.outputLimit = value/100; break;
          case 'stereoMode': this.stereoMode = value; break;
          case 'stereoWidth': this.stereoWidth = value/100; break;
          case 'stereoBalance': this.stereoBalance = value/100; break;
          case 'saturation': this.saturationAmount = value; break;
        }
      }
      
      initCrossovers3(which) {
        const bands = this[which];
        const freqs = bands.map(b => b.freq);
        for(let i=0;i<bands.length;i++){
          const b = bands[i];
          b.lowFilter = this.createFilter('lowpass', freqs[i]);
          b.highFilter = this.createFilter('highpass', freqs[i]);
        }
      }
      
      createFilter(type, freq) {
        const rc = 1/(2*Math.PI*freq);
        const dt = 1/this.sampleRate;
        const alpha = dt/(rc+dt);
        return {type, alpha, stateL:0, stateR:0};
      }
      
      processFilter(filter, xL, xR) {
        if(!filter) return {yL:xL, yR:xR};
        if(filter.type==='lowpass'){
          const yL=filter.stateL+filter.alpha*(xL-filter.stateL);
          const yR=filter.stateR+filter.alpha*(xR-filter.stateR);
          filter.stateL=yL; filter.stateR=yR;
          return {yL, yR};
        } else {
          const lpL=filter.stateL+filter.alpha*(xL-filter.stateL);
          const lpR=filter.stateR+filter.alpha*(xR-filter.stateR);
          filter.stateL=lpL; filter.stateR=lpR;
          return {yL:xL-lpL, yR:xR-lpR};
        }
      }
      
      compressSample(signal, threshold, ratio, attack, release, envelope, gain) {
        const absSig = Math.abs(signal);
        const atkCoeff = Math.exp(-1/(attack*0.001*this.sampleRate));
        const relCoeff = Math.exp(-1/(release*0.001*this.sampleRate));
        if(absSig > envelope) envelope = atkCoeff*envelope+(1-atkCoeff)*absSig;
        else envelope = relCoeff*envelope+(1-relCoeff)*absSig;
        const linThresh = Math.pow(10, threshold/20);
        if(envelope > linThresh){
          const overshoot = envelope/linThresh;
          const logOvershoot = Math.log10(overshoot);
          const reduction = Math.pow(10, -logOvershoot*(1-1/ratio));
          gain = Math.min(gain, reduction);
        } else {
          const relSlow = Math.exp(-1/(release*3*0.001*this.sampleRate));
          gain = Math.min(1, gain+(1-gain)*(1-relSlow));
        }
        return {envelope, gain, output: signal*gain};
      }
      
      applySaturation(x) {
        if(this.saturationAmount<=0) return x;
        const s = this.saturationAmount;
        return Math.tanh(x*(1+s))/Math.tanh(1+s);
      }
      
      processPreEmphasis(x) {
        if(this.preEmphasisGain<=0) return x;
        const tau = this.preEmphasisTime===0 ? 50e-6 : 75e-6;
        const dt = 1/this.sampleRate;
        const alpha = dt/(tau+dt);
        const result = x + this.preEmphasisGain*(x-this.preEmphState)*alpha;
        this.preEmphState = x;
        return result;
      }
      
      process(inputs, outputs) {
        const input = inputs[0], output = outputs[0];
        if(!input||!input[0]||!output||!output[0]) return true;
        const inL=input[0], inR=input.length>1?input[1]:input[0];
        const outL=output[0], outR=output.length>1?output[1]:output[0];
        const blockSize = inL.length;
        let clipDetected=false, peakIL=0, peakIR=0, peakOL=0, peakOR=0, rmsIL=0, rmsIR=0, rmsOL=0, rmsOR=0;
        
        for(let i=0;i<blockSize;i++){
          let sL=inL[i], sR=inR[i];
          peakIL=Math.max(peakIL,Math.abs(sL)); peakIR=Math.max(peakIR,Math.abs(sR));
          rmsIL+=sL*sL; rmsIR+=sR*sR;
          
          sL*=this.inputGain; sR*=this.inputGain;
          if(this.inputPan!==0){
            const pL=Math.cos((this.inputPan+1)*Math.PI/4), pR=Math.sin((this.inputPan+1)*Math.PI/4);
            sL=sL*pL+sR*(1-Math.abs(pL)); sR=sR*pR+sL*(1-Math.abs(pR));
          }
          
          // Graphic EQ 31 bands
          if(this.geqMasterEnabled){
            const eq = this.processGEQ(sL, sR);
            sL=eq.yL; sR=eq.yR;
          }
          
          if(this.saturationAmount>0){ sL=this.applySaturation(sL); sR=this.applySaturation(sR); }
          
          if(this.agcEnabled && this.agcState.enabled){
            const agc=this.agcState;
            const mixed=(sL+sR)/2;
            const r=this.compressSample(mixed, agc.threshold, agc.ratio, agc.attack, agc.release, this.agcEnvelope, this.agcGain);
            this.agcEnvelope=r.envelope; this.agcGain=r.gain;
            sL*=this.agcGain; sR*=this.agcGain;
          }
          
          if(this.comp3MasterEnabled){
            for(let b=0;b<this.comp3.length;b++){
              const band=this.comp3[b];
              if(band.enabled && band.lowFilter && band.highFilter){
                let lo=this.processFilter(band.lowFilter, sL, sR);
                let hi=this.processFilter(band.highFilter, lo.yL, lo.yR);
                const mixed=(hi.yL+hi.yR)/2;
                const r=this.compressSample(mixed, band.threshold, band.ratio, band.attack, band.release, band.envelope, band.g);
                band.envelope=r.envelope; band.g=r.gain;
                const gf=band.g*Math.pow(10, band.gain/20);
                sL=lo.yL+hi.yL*gf; sR=lo.yR+hi.yR*gf;
              }
            }
          }
          
          if(this.limiter3MasterEnabled){
            for(let b=0;b<this.limiter3.length;b++){
              const band=this.limiter3[b];
              if(band.enabled && band.lowFilter && band.highFilter){
                let lo=this.processFilter(band.lowFilter, sL, sR);
                let hi=this.processFilter(band.highFilter, lo.yL, lo.yR);
                const mixed=(hi.yL+hi.yR)/2;
                const r=this.compressSample(mixed, band.threshold, band.ratio, band.attack, band.release, band.envelope, band.g);
                band.envelope=r.envelope; band.g=r.gain;
                const gf=band.g*Math.pow(10, band.gain/20);
                sL=lo.yL+hi.yL*gf; sR=lo.yR+hi.yR*gf;
              }
            }
          }
          
          if(this.finalLimiterEnabled && this.finalLimiter.enabled){
            const fl=this.finalLimiter;
            const mixed=(sL+sR)/2;
            const r=this.compressSample(mixed, fl.threshold, fl.ratio, fl.attack, fl.release, fl.envelope, fl.g);
            fl.envelope=r.envelope; fl.g=r.gain;
            const gf=fl.g*Math.pow(10, fl.gain/20);
            sL*=gf; sR*=gf;
          }
          
          if(this.powerLimiterEnabled && this.powerLimiter.enabled){
            const pl=this.powerLimiter;
            const mixed=(sL+sR)/2;
            const r=this.compressSample(mixed, pl.threshold, pl.ratio, pl.attack, pl.release, pl.envelope, pl.g);
            pl.envelope=r.envelope; pl.g=r.gain;
            const gf=pl.g*Math.pow(10, pl.gain/20);
            sL*=gf; sR*=gf;
          }
          
          if(this.antiClipEnabled){
            const mx=Math.max(Math.abs(sL),Math.abs(sR));
            if(mx>1){ sL/=mx; sR/=mx; clipDetected=true; }
          } else {
            if(Math.abs(sL)>1){sL=Math.sign(sL);clipDetected=true;}
            if(Math.abs(sR)>1){sR=Math.sign(sR);clipDetected=true;}
          }
          
          if(this.preEmphasisEnabled && this.preEmphasisGain>0){
            sL=this.processPreEmphasis(sL); sR=this.processPreEmphasis(sR);
          }
          
          if(this.stereoMode===1){ const m=(sL+sR)/2; sL=m; sR=m; }
          else if(this.stereoMode===2){ const m=(sL+sR)/2, sd=(sL-sR)/2; sL=m+sd*this.stereoWidth; sR=m-sd*this.stereoWidth; }
          else { const m=(sL+sR)/2, sd=(sL-sR)/2; sL=m+sd*this.stereoWidth; sR=m-sd*this.stereoWidth; }
          
          if(this.stereoBalance!==0){
            if(this.stereoBalance>0) sR*=(1-this.stereoBalance);
            else sL*=(1+this.stereoBalance);
          }
          
          sL*=this.outputGain*this.outputLimit; sR*=this.outputGain*this.outputLimit;
          sL=Math.max(-1,Math.min(1,sL)); sR=Math.max(-1,Math.min(1,sR));
          
          peakOL=Math.max(peakOL,Math.abs(sL)); peakOR=Math.max(peakOR,Math.abs(sR));
          rmsOL+=sL*sL; rmsOR+=sR*sR;
          if(this.spectrumIdx<this.spectrumBuffer.length){ this.spectrumBuffer[this.spectrumIdx]=sL; this.spectrumIdx++; }
          outL[i]=sL; outR[i]=sR;
        }
        
        this.frameCounter+=blockSize;
        if(this.frameCounter>=this.meterUpdateInterval){
          this.frameCounter=0;
          const rmsToDb=(rms,n)=>20*Math.log10(Math.max(Math.sqrt(rms/n),1e-10));
          this.meterInputL=rmsToDb(rmsIL,blockSize); this.meterInputR=rmsToDb(rmsIR,blockSize);
          this.meterOutputL=rmsToDb(rmsOL,blockSize); this.meterOutputR=rmsToDb(rmsOR,blockSize);
          this.meterPeakIL=20*Math.log10(Math.max(peakIL,1e-10)); this.meterPeakIR=20*Math.log10(Math.max(peakIR,1e-10));
          this.meterPeakOL=20*Math.log10(Math.max(peakOL,1e-10)); this.meterPeakOR=20*Math.log10(Math.max(peakOR,1e-10));
          this.port.postMessage({type:'meters', meters:{inputL:this.meterInputL,inputR:this.meterInputR,outputL:this.meterOutputL,outputR:this.meterOutputR}, peaks:{inputL:this.meterPeakIL,inputR:this.meterPeakIR,outputL:this.meterPeakOL,outputR:this.meterPeakOR}});
          if(this.spectrumIdx>0){ const spec=this.computeSpectrum(this.spectrumBuffer.slice(0,this.spectrumIdx)); this.spectrumIdx=0; this.port.postMessage({type:'spectrum',spectrum:spec}); }
          this.port.postMessage({type:'eqcurve',eqcurve:this.computeEQCurve()});
          this.port.postMessage({type:'gr',gr:{agc:this.agcState.enabled?20*Math.log10(Math.max(this.agcGain,1e-10)):0, finalLimiter:this.finalLimiter.enabled?20*Math.log10(Math.max(this.finalLimiter.g,1e-10)):0, powerLimiter:this.powerLimiter.enabled?20*Math.log10(Math.max(this.powerLimiter.g,1e-10)):0}});
          if(clipDetected) this.port.postMessage({type:'clip'});
        }
        return true;
      }
      
      computeSpectrum(buffer){
        const N=buffer.length, result=new Float32Array(128), bands=128;
        for(let b=0;b<bands;b++){
          const freq=20*Math.pow(1000,b/bands);
          const k=Math.round(freq*N/this.sampleRate);
          if(k<1||k>=N/2) continue;
          let re=0,im=0; const omega=2*Math.PI*k/N;
          for(let i=0;i<N;i++){re+=buffer[i]*Math.cos(omega*i); im-=buffer[i]*Math.sin(omega*i);}
          const energy=Math.sqrt(re*re+im*im)/N;
          result[b]=20*Math.log10(Math.max(energy,1e-10));
        }
        return result;
      }
      
      computeEQCurve(){
        const points=128, curve=new Float32Array(points), sr=this.sampleRate;
        for(let i=0;i<points;i++){
          const freq=20*Math.pow(1000,i/points);
          let response=0;
          for(let j=0;j<31;j++){
            if(Math.abs(this.geqGains[j])<0.01) continue;
            const f=this.geqFilters[j];
            const omega=2*Math.PI*freq/sr;
            const cosW=Math.cos(omega),sinW=Math.sin(omega),cos2W=Math.cos(2*omega),sin2W=Math.sin(2*omega);
            const numRe=f.b0+f.b1*cosW+f.b2*cos2W, numIm=f.b1*sinW+f.b2*sin2W;
            const denRe=1+f.a1*cosW+f.a2*cos2W, denIm=f.a1*sinW+f.a2*sin2W;
            const mag=Math.sqrt((numRe*numRe+numIm*numIm)/(denRe*denRe+denIm*denIm));
            response+=20*Math.log10(Math.max(mag,1e-10));
          }
          curve[i]=response;
        }
        return curve;
      }
    }
    registerProcessor('broadcast-processor', BroadcastProcessor);
    `;
  }

  async togglePower() {
    if (!this.isEnabled) await this.initAudio();
    else { this.audioCtx.close(); this.isEnabled = false; document.getElementById('dspStatus').textContent = 'Bypass'; document.getElementById('powerBtn').classList.remove('active'); }
  }

  sendAllParams() {
    if (!this.audioWorklet) return;
    const p = this.params, port = this.audioWorklet.port;
    // Send GEQ gains
    for(let i=0;i<31;i++) port.postMessage({type:'geq', index:i, value:p.geq[i]});
    port.postMessage({type:'agc', params:p.agc});
    p.comp3.forEach((band,i)=>port.postMessage({type:'comp3',index:i,params:band}));
    p.limiter3.forEach((band,i)=>port.postMessage({type:'limiter3',index:i,params:band}));
    port.postMessage({type:'finalLimiter',params:p.finalLimiter});
    port.postMessage({type:'powerLimiter',params:p.powerLimiter});
    port.postMessage({type:'preEmphasis',value:p.preEmphasis.time});
    port.postMessage({type:'preEmphasisGain',value:p.preEmphasis.gain});
    Object.keys(p).forEach(key=>{
      if(!['geq','geqMaster','agc','comp3','comp3Master','limiter3','limiter3Master','finalLimiter','powerLimiter','preEmphasis','mode','inputDevice','outputDevice'].includes(key))
        port.postMessage({type:'param',key,value:p[key]});
    });
    port.postMessage({type:'geqMaster',enabled:p.geqMaster?.enabled??true});
    port.postMessage({type:'agcEnabled',enabled:p.agc?.enabled??true});
    port.postMessage({type:'comp3Master',enabled:p.comp3Master?.enabled??true});
    port.postMessage({type:'limiter3Master',enabled:p.limiter3Master?.enabled??true});
    port.postMessage({type:'finalLimiterEnabled',enabled:p.finalLimiter?.enabled??true});
    port.postMessage({type:'powerLimiterEnabled',enabled:p.powerLimiter?.enabled??true});
    port.postMessage({type:'preEmphasisEnabled',enabled:p.preEmphasis?.enabled??true});
  }

  sendParam(key,value){ if(!this.audioWorklet)return; this.params[key]=value; this.audioWorklet.port.postMessage({type:'param',key,value}); }
  
  sendGEQ(index, value){
    if(!this.audioWorklet) return;
    this.params.geq[index] = value;
    this.audioWorklet.port.postMessage({type:'geq', index, value});
    this.updateGEQValue(index);
    this.drawEQCurve();
  }

  resetGEQ(){
    this.params.geq.fill(0);
    for(let i=0;i<31;i++){
      this.audioWorklet?.port.postMessage({type:'geq',index:i,value:0});
      this.updateGEQValue(i);
    }
    this.drawEQCurve();
  }

  sendComp3(index,params){ if(!this.audioWorklet)return; Object.assign(this.params.comp3[index],params); this.audioWorklet.port.postMessage({type:'comp3',index,params}); }
  sendLimiter3(index,params){ if(!this.audioWorklet)return; Object.assign(this.params.limiter3[index],params); this.audioWorklet.port.postMessage({type:'limiter3',index,params}); }
  sendFinalLimiter(params){ if(!this.audioWorklet)return; Object.assign(this.params.finalLimiter,params); this.audioWorklet.port.postMessage({type:'finalLimiter',params}); }
  sendPowerLimiter(params){ if(!this.audioWorklet)return; Object.assign(this.params.powerLimiter,params); this.audioWorklet.port.postMessage({type:'powerLimiter',params}); }

  updateMeters(){
    const m=this.meters,p=this.peaks;
    ['inputL','inputR','outputL','outputR'].forEach(ch=>{if(p[ch]>this.peakHold[ch])this.peakHold[ch]=p[ch];else this.peakHold[ch]=Math.max(this.peakHold[ch]-0.5,p[ch]);});
    const clampDb=(db)=>Math.max(-60,Math.min(0,db));
    const set=(id,val,pid,pv)=>{document.getElementById(id).style.height=Math.max(0,((clampDb(val)+60)/60*100))+'%';document.getElementById(pid).style.bottom=Math.max(0,((clampDb(pv)+60)/60*100))+'%';};
    set('inputMeterL',m.inputL,'inputPeakL',this.peakHold.inputL);set('inputMeterR',m.inputR,'inputPeakR',this.peakHold.inputR);
    set('outputMeterL',m.outputL,'outputPeakL',this.peakHold.outputL);set('outputMeterR',m.outputR,'outputPeakR',this.peakHold.outputR);
    document.getElementById('procMeterL').style.height=document.getElementById('outputMeterL').style.height;document.getElementById('procMeterR').style.height=document.getElementById('outputMeterR').style.height;
    document.getElementById('procPeakL').style.bottom=document.getElementById('outputPeakL').style.bottom;document.getElementById('procPeakR').style.bottom=document.getElementById('outputPeakR').style.bottom;
  }

  updateGRMeters(gr){
    document.getElementById('agcGrMeter').style.width=Math.max(0,Math.min(100,Math.abs(gr.agc)/40*100))+'%';
    document.getElementById('limGrMeter').style.width=Math.max(0,Math.min(100,Math.abs(gr.finalLimiter)/40*100))+'%';
    document.getElementById('powLimGrMeter').style.width=Math.max(0,Math.min(100,Math.abs(gr.powerLimiter)/40*100))+'%';
  }

  initSpectrum(){ this.canvas=document.getElementById('spectrumCanvas');this.ctx=this.canvas.getContext('2d');this.resizeCanvas();window.addEventListener('resize',()=>this.resizeCanvas());this.drawSpectrum(); }
  resizeCanvas(){this.canvas.width=this.canvas.offsetWidth*2;this.canvas.height=this.canvas.offsetHeight*2;}
  
  drawSpectrum(){
    const c=this.canvas,ctx=this.ctx,w=c.width,h=c.height;
    ctx.fillStyle='#050505';ctx.fillRect(0,0,w,h);
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=1;
    for(let i=0;i<=8;i++){const y=(h/8)*i;ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(w,y);ctx.stroke();}
    ctx.fillStyle='#444';ctx.font='16px Courier New';
    ['20','50','100','200','500','1k','2k','5k','10k','20k'].forEach((l,i)=>{ctx.fillText(l,(w/9)*i-10,h-5);});
    if(!this.spectrumData||this.spectrumData.length===0)return;
    const bw=w/this.spectrumData.length,minDb=-80,maxDb=0;
    for(let i=0;i<this.spectrumData.length;i++){
      const db=Math.max(minDb,Math.min(maxDb,this.spectrumData[i]));
      const nh=(db-minDb)/(maxDb-minDb),bh=nh*h*0.85;
      let r,g,b;
      if(nh<0.5){r=0;g=Math.floor(nh*2*255);b=0;}else if(nh<0.8){r=Math.floor((nh-0.5)*2*255);g=255;b=0;}else{r=255;g=Math.floor((1-(nh-0.8)*5)*255);b=0;}
      ctx.fillStyle=`rgb(${r},${g},${b})`;ctx.fillRect(i*bw,h-bh,bw-1,bh);
    }
    ctx.shadowBlur=10;ctx.shadowColor='rgba(57,255,20,0.3)';ctx.strokeStyle='rgba(57,255,20,0.5)';ctx.lineWidth=2;ctx.beginPath();
    for(let i=0;i<this.spectrumData.length;i++){const db=Math.max(minDb,Math.min(maxDb,this.spectrumData[i]));const nh=(db-minDb)/(maxDb-minDb);const bh=nh*h*0.85;const x=i*bw+bw/2,y=h-bh;if(i===0)ctx.moveTo(x,y);else ctx.lineTo(x,y);}
    ctx.stroke();ctx.shadowBlur=0;
  }

  initEQDisplay(){ this.eqCanvas=document.getElementById('eqCanvas');this.eqCtx=this.eqCanvas.getContext('2d');this.eqCanvas.width=this.eqCanvas.offsetWidth*2;this.eqCanvas.height=this.eqCanvas.offsetHeight*2;this.drawEQCurve(); }
  
  drawEQCurve(){
    const c=this.eqCanvas;if(!c)return;const ctx=this.eqCtx,w=c.width,h=c.height;
    ctx.fillStyle='#050505';ctx.fillRect(0,0,w,h);
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=1;
    for(let db=-12;db<=12;db+=3){const y=h/2-(db/12)*(h/2)*0.8;ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(w,y);ctx.stroke();}
    ctx.strokeStyle='#333';ctx.beginPath();ctx.moveTo(0,h/2);ctx.lineTo(w,h/2);ctx.stroke();
    ctx.fillStyle='#555';ctx.font='14px Courier New';
    for(let db=-12;db<=12;db+=6){const y=h/2-(db/12)*(h/2)*0.8;ctx.fillText((db>0?'+':'')+db+'dB',5,y+4);}
    ['20','50','100','200','500','1k','2k','5k','10k','20k'].forEach((l,i)=>{ctx.fillStyle='#555';ctx.fillText(l,(w/9)*i-12,h-5);});
    if(!this.eqData||this.eqData.length===0)return;
    ctx.shadowBlur=8;ctx.shadowColor='rgba(57,255,20,0.5)';ctx.strokeStyle='#39ff14';ctx.lineWidth=3;ctx.beginPath();
    for(let i=0;i<this.eqData.length;i++){const db=Math.max(-12,Math.min(12,this.eqData[i]));const x=(i/(this.eqData.length-1))*w;const y=h/2-(db/12)*(h/2)*0.8;if(i===0)ctx.moveTo(x,y);else ctx.lineTo(x,y);}
    ctx.stroke();ctx.shadowBlur=0;
    ctx.lineTo(w,h/2);ctx.lineTo(0,h/2);ctx.closePath();ctx.fillStyle='rgba(57,255,20,0.05)';ctx.fill();
  }

  initUI(){
    document.getElementById('powerBtn').addEventListener('click',()=>this.togglePower());
    document.getElementById('configBtn').addEventListener('click',()=>document.getElementById('configOverlay').classList.add('visible'));
    document.getElementById('configClose').addEventListener('click',()=>document.getElementById('configOverlay').classList.remove('visible'));
    document.getElementById('presetsBtn').addEventListener('click',()=>{const b=document.getElementById('presetsBar');b.style.display=b.style.display==='none'?'flex':'none';});
    document.querySelectorAll('.preset-btn').forEach(btn=>{btn.addEventListener('click',()=>{this.loadPreset(btn.dataset.preset);document.querySelectorAll('.preset-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');});});
    document.getElementById('modeSelect').addEventListener('change',(e)=>{this.params.mode=e.target.value;this.loadPreset(e.target.value);});
    document.querySelectorAll('.mode-btn').forEach(btn=>{btn.addEventListener('click',()=>{document.querySelectorAll('.mode-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');const m=btn.dataset.mode;const mv=m==='stereo'?0:m==='mono'?1:2;this.sendParam('stereoMode',mv);this.params.stereoMode=mv;});});
    document.getElementById('exportBtn').addEventListener('click',()=>this.exportConfig());
    document.getElementById('importBtn').addEventListener('click',()=>document.getElementById('importFile').click());
    document.getElementById('importFile').addEventListener('change',(e)=>this.importConfig(e.target.files[0]));
    ['agcMode','configSampleRate','configBufferSize','oversampling','antiClipping','harmonicSat'].forEach(id=>{const el=document.getElementById(id);if(el)el.addEventListener('change',()=>this.applyConfig());});
    document.getElementById('inputDevice').addEventListener('change',(e)=>{this.params.inputDevice=e.target.value;localStorage.setItem('inputDevice',e.target.value);if(this.isEnabled)this.restartAudio();});
    document.getElementById('outputDevice').addEventListener('change',async(e)=>{this.params.outputDevice=e.target.value;localStorage.setItem('outputDevice',e.target.value);if(this.audioCtx&&this.audioCtx.setSinkId){try{await this.audioCtx.setSinkId(e.target.value);}catch(err){console.error("Error:",err);}}});
    document.getElementById('geqResetBtn').addEventListener('click',()=>this.resetGEQ());
    this.enumerateDevices();
  }

  initBypassButtons(){
    const map={
      'geq':()=>{this.params.geqMaster.enabled=!this.params.geqMaster.enabled;this.audioWorklet?.port.postMessage({type:'geqMaster',enabled:this.params.geqMaster.enabled});},
      'agc':()=>{this.params.agc.enabled=!this.params.agc.enabled;this.audioWorklet?.port.postMessage({type:'agcEnabled',enabled:this.params.agc.enabled});},
      'comp3':()=>{this.params.comp3Master.enabled=!this.params.comp3Master.enabled;this.audioWorklet?.port.postMessage({type:'comp3Master',enabled:this.params.comp3Master.enabled});},
      'limiter3':()=>{this.params.limiter3Master.enabled=!this.params.limiter3Master.enabled;this.audioWorklet?.port.postMessage({type:'limiter3Master',enabled:this.params.limiter3Master.enabled});},
      'finalLimiter':()=>{this.params.finalLimiter.enabled=!this.params.finalLimiter.enabled;this.audioWorklet?.port.postMessage({type:'finalLimiterEnabled',enabled:this.params.finalLimiter.enabled});},
      'powerLimiter':()=>{this.params.powerLimiter.enabled=!this.params.powerLimiter.enabled;this.audioWorklet?.port.postMessage({type:'powerLimiterEnabled',enabled:this.params.powerLimiter.enabled});},
      'preEmphasis':()=>{this.params.preEmphasis.enabled=!this.params.preEmphasis.enabled;this.audioWorklet?.port.postMessage({type:'preEmphasisEnabled',enabled:this.params.preEmphasis.enabled});}
    };
    document.querySelectorAll('.bypass-btn').forEach(btn=>{btn.addEventListener('click',()=>{const m=btn.dataset.module;if(map[m])map[m]();btn.classList.toggle('active');btn.textContent=btn.classList.contains('active')?'ACTIVE':'BYPASS';});});
  }

  async enumerateDevices(){
    try{
      if(!this.inputStream){const s=await navigator.mediaDevices.getUserMedia({audio:true});s.getTracks().forEach(t=>t.stop());}
      const devices=await navigator.mediaDevices.enumerateDevices();
      const is=document.getElementById('inputDevice'),os=document.getElementById('outputDevice');
      if(!is||!os)return;is.innerHTML='';os.innerHTML='';
      devices.forEach(d=>{const o=document.createElement('option');o.value=d.deviceId;o.textContent=d.label||`${d.kind} (${d.deviceId.substring(0,5)})`;if(d.kind==='audioinput'){is.appendChild(o);if(d.deviceId===this.params.inputDevice)o.selected=true;}else if(d.kind==='audiooutput'){os.appendChild(o);if(d.deviceId===this.params.outputDevice)o.selected=true;}});
    }catch(err){console.error("Error:",err);}
  }

  async restartAudio(){if(this.inputStream)this.inputStream.getTracks().forEach(t=>t.stop());if(this.audioCtx)await this.audioCtx.close();this.initAudio();}
  applyConfig(){const sat=parseInt(document.getElementById('harmonicSat').value);this.sendParam('saturation',sat*0.15);this.params.saturation=sat*0.15;const buf=parseInt(document.getElementById('configBufferSize').value);this.bufferSize=buf;document.getElementById('bufferSizeDisplay').textContent=buf+' samples';document.getElementById('latencyDisplay').textContent=(buf/(this.audioCtx?this.audioCtx.sampleRate:48000)*1000).toFixed(2)+' ms';}

  // ============================================
  // GRAPHIC EQ 31 BANDS - FADERS
  // ============================================
  initGEQFaders(){
    const container = document.getElementById('geqFaders');
    if(!container) return;
    
    const freqs = this.geqFrequencies;
    let html = '';
    freqs.forEach((freq, i) => {
      const isHighlight = (freq >= 1000 && freq <= 10000) || freq === 80 || freq === 160 || freq === 315 || freq === 630 || freq === 250 || freq === 500 || freq === 4000 || freq === 8000;
      const label = freq >= 1000 ? (freq/1000).toFixed(freq%1000===0?0:1)+'k' : freq;
      html += `<div class="geq-channel">
        <div class="geq-value" id="geqVal${i}">0 dB</div>
        <div class="geq-fader-track" data-geqidx="${i}">
          <div class="geq-fader-thumb" id="geqThumb${i}"></div>
        </div>
        <div class="geq-freq-label ${isHighlight?'highlight':''}">${label}</div>
      </div>`;
    });
    container.innerHTML = html;
    
    // Bind fader interactions
    container.querySelectorAll('.geq-fader-track').forEach(track => {
      const idx = parseInt(track.dataset.geqidx);
      const thumb = track.querySelector('.geq-fader-thumb');
      let isDragging = false, startY = 0, startGain = 0;
      
      const updateFader = (gain) => {
        gain = Math.max(-12, Math.min(12, gain));
        this.params.geq[idx] = gain;
        const pct = ((gain + 12) / 24) * 100;
        thumb.style.bottom = pct + '%';
        const valEl = document.getElementById(`geqVal${idx}`);
        if(valEl) valEl.textContent = (gain > 0 ? '+' : '') + gain.toFixed(1) + ' dB';
        valEl.style.color = gain > 0.5 ? '#39ff14' : gain < -0.5 ? '#ff3333' : '#888';
        this.sendGEQ(idx, gain);
      };
      
      // Initial position
      updateFader(this.params.geq[idx]);
      
      const onMove = (clientY) => {
        if(!isDragging) return;
        const rect = track.getBoundingClientRect();
        const trackH = rect.height;
        const delta = (startY - clientY) / trackH * 24;
        updateFader(startGain + delta);
      };
      
      track.addEventListener('mousedown', (e) => { isDragging=true; startY=e.clientY; startGain=this.params.geq[idx]; e.preventDefault(); });
      window.addEventListener('mousemove', (e) => onMove(e.clientY));
      window.addEventListener('mouseup', () => { isDragging=false; });
      track.addEventListener('touchstart', (e) => { isDragging=true; startY=e.touches[0].clientY; startGain=this.params.geq[idx]; e.preventDefault(); });
      window.addEventListener('touchmove', (e) => { if(isDragging) onMove(e.touches[0].clientY); });
      window.addEventListener('touchend', () => { isDragging=false; });
      
      track.addEventListener('wheel', (e) => { e.preventDefault(); updateFader(this.params.geq[idx] - e.deltaY * 0.05); }, {passive:false});
    });
  }

  updateGEQValue(index){
    const gain = this.params.geq[index];
    const valEl = document.getElementById(`geqVal${index}`);
    const thumb = document.getElementById(`geqThumb${index}`);
    if(!valEl || !thumb) return;
    const pct = ((gain + 12) / 24) * 100;
    thumb.style.bottom = pct + '%';
    valEl.textContent = (gain > 0 ? '+' : '') + gain.toFixed(1) + ' dB';
    valEl.style.color = gain > 0.5 ? '#39ff14' : gain < -0.5 ? '#ff3333' : '#888';
  }

  initKnobs(){
    this.initStandardKnobs();
    this.initAGCKnobs();
    this.initComp3Knobs();
    this.initLimiter3Knobs();
    this.initFinalLimiterKnobs();
    this.initPowerLimiterKnobs();
  }

  initStandardKnobs(){
    document.querySelectorAll('.knob-wrapper').forEach(wrapper=>{
      const knob=wrapper.querySelector('.knob'),valueDisplay=wrapper.parentElement.querySelector('.knob-value'),param=wrapper.dataset.param;
      const min=parseFloat(wrapper.dataset.min),max=parseFloat(wrapper.dataset.max),defaultVal=parseFloat(wrapper.dataset.default);
      let currentValue=this.params[param]!==undefined?this.params[param]:defaultVal;
      let isDragging=false,startY=0,startValue=0,angle=((currentValue-min)/(max-min))*270-135;
      knob.style.setProperty('--knob-angle',angle+'deg');
      const updateValue=(val)=>{
        currentValue=Math.max(min,Math.min(max,val));angle=((currentValue-min)/(max-min))*270-135;knob.style.setProperty('--knob-angle',angle+'deg');
        let dv;if(param.includes('Gain')||param.includes('gain')||param.includes('threshold')||param.includes('Threshold'))dv=currentValue.toFixed(1)+' dB';else if(param.includes('Freq')||param.includes('freq'))dv=currentValue>=1000?(currentValue/1000).toFixed(1)+'k':Math.round(currentValue);else if(param.includes('Ratio')||param.includes('ratio'))dv=currentValue.toFixed(1)+':1';else if(param.includes('Attack')||param.includes('Release')||param.includes('attack')||param.includes('release'))dv=Math.round(currentValue)+' ms';else if(param.includes('Limit')||param.includes('limit'))dv=Math.round(currentValue)+'%';else if(param.includes('Pan')||param.includes('pan')||param.includes('Balance')||param.includes('balance')){if(Math.abs(currentValue)<1)dv='C';else if(currentValue<0)dv='L'+Math.abs(Math.round(currentValue));else dv='R'+Math.round(currentValue);}else if(param.includes('Width')||param.includes('width'))dv=Math.round(currentValue)+'%';else if(param==='preEmphasis')dv=currentValue===0?'50μs':'75μs';else if(param==='stereoMode')dv=['ST','MO','MX'][Math.round(currentValue)];else if(param==='inputPhase')dv=currentValue===0?'NOR':'REV';else dv=currentValue.toFixed(1);
        if(valueDisplay)valueDisplay.textContent=dv;
        if(!param.startsWith('eq_'))this.sendParam(param,currentValue);
      };
      wrapper.addEventListener('mousedown',(e)=>{isDragging=true;startY=e.clientY;startValue=currentValue;e.preventDefault();});
      window.addEventListener('mousemove',(e)=>{if(!isDragging)return;updateValue(startValue+(startY-e.clientY)*((max-min)/200));});
      window.addEventListener('mouseup',()=>{isDragging=false;});
      wrapper.addEventListener('touchstart',(e)=>{isDragging=true;startY=e.touches[0].clientY;startValue=currentValue;e.preventDefault();});
      window.addEventListener('touchmove',(e)=>{if(!isDragging)return;updateValue(startValue+(startY-e.touches[0].clientY)*((max-min)/200));});
      window.addEventListener('touchend',()=>{isDragging=false;});
      wrapper.addEventListener('dblclick',()=>updateValue(defaultVal));
      wrapper.addEventListener('wheel',(e)=>{e.preventDefault();updateValue(currentValue-e.deltaY*((max-min)/1000));},{passive:false});
      updateValue(currentValue);
    });
  }

  initAGCKnobs(){
    const container=document.getElementById('agcKnobs');if(!container)return;
    const agc=this.params.agc;
    container.innerHTML=`<div class="knobs-grid"><div class="knob-container"><div class="knob-wrapper" data-param="agc_threshold" data-min="-60" data-max="0" data-default="${agc.threshold}" id="agc_threshold"><div class="knob small"></div></div><div class="knob-value">${agc.threshold} dB</div><div class="knob-label">Threshold</div></div><div class="knob-container"><div class="knob-wrapper" data-param="agc_ratio" data-min="1" data-max="20" data-default="${agc.ratio}" id="agc_ratio"><div class="knob small"></div></div><div class="knob-value">${agc.ratio}:1</div><div class="knob-label">Ratio</div></div><div class="knob-container"><div class="knob-wrapper" data-param="agc_attack" data-min="0.1" data-max="200" data-default="${agc.attack}" id="agc_attack"><div class="knob small"></div></div><div class="knob-value">${agc.attack} ms</div><div class="knob-label">Attack</div></div><div class="knob-container"><div class="knob-wrapper" data-param="agc_release" data-min="10" data-max="2000" data-default="${agc.release}" id="agc_release"><div class="knob small"></div></div><div class="knob-value">${agc.release} ms</div><div class="knob-label">Release</div></div></div>`;
    this.bindKnobGroup('agc_threshold','threshold',agc.threshold,(v)=>{this.params.agc.threshold=v;this.audioWorklet?.port.postMessage({type:'agc',params:this.params.agc});});
    this.bindKnobGroup('agc_ratio','ratio',agc.ratio,(v)=>{this.params.agc.ratio=v;this.audioWorklet?.port.postMessage({type:'agc',params:this.params.agc});});
    this.bindKnobGroup('agc_attack','attack',agc.attack,(v)=>{this.params.agc.attack=v;this.audioWorklet?.port.postMessage({type:'agc',params:this.params.agc});});
    this.bindKnobGroup('agc_release','release',agc.release,(v)=>{this.params.agc.release=v;this.audioWorklet?.port.postMessage({type:'agc',params:this.params.agc});});
  }

  initComp3Knobs(){
    const container=document.getElementById('comp3Knobs');if(!container)return;
    let html='';
    this.params.comp3.forEach((band,i)=>{html+=`<div class="band-section"><div class="band-title">${band.name} (Split: ${band.freq} Hz)</div><div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;"><button class="toggle-btn ${band.enabled?'':'active'}" data-c3band="${i}" data-c3tgl="enabled">${band.enabled?'ON':'OFF'}</button></div><div class="knobs-grid"><div class="knob-container"><div class="knob-wrapper" data-param="c3_freq_${i}" data-min="40" data-max="16000" data-default="${band.freq}" id="c3_freq_${i}"><div class="knob small"></div></div><div class="knob-value">${band.freq>=1000?(band.freq/1000).toFixed(1)+'k':band.freq}</div><div class="knob-label">Freq</div></div><div class="knob-container"><div class="knob-wrapper" data-param="c3_thr_${i}" data-min="-60" data-max="0" data-default="${band.threshold}" id="c3_thr_${i}"><div class="knob small"></div></div><div class="knob-value">${band.threshold} dB</div><div class="knob-label">Threshold</div></div><div class="knob-container"><div class="knob-wrapper" data-param="c3_rat_${i}" data-min="1" data-max="20" data-default="${band.ratio}" id="c3_rat_${i}"><div class="knob small"></div></div><div class="knob-value">${band.ratio}:1</div><div class="knob-label">Ratio</div></div><div class="knob-container"><div class="knob-wrapper" data-param="c3_atk_${i}" data-min="0.1" data-max="200" data-default="${band.attack}" id="c3_atk_${i}"><div class="knob small"></div></div><div class="knob-value">${band.attack} ms</div><div class="knob-label">Attack</div></div><div class="knob-container"><div class="knob-wrapper" data-param="c3_rel_${i}" data-min="10" data-max="2000" data-default="${band.release}" id="c3_rel_${i}"><div class="knob small"></div></div><div class="knob-value">${band.release} ms</div><div class="knob-label">Release</div></div><div class="knob-container"><div class="knob-wrapper" data-param="c3_gn_${i}" data-min="-12" data-max="12" data-default="${band.gain}" id="c3_gn_${i}"><div class="knob small"></div></div><div class="knob-value">${band.gain} dB</div><div class="knob-label">Gain</div></div></div></div>`;});
    container.innerHTML=html;
    const pm={'c3_freq':'freq','c3_thr':'threshold','c3_rat':'ratio','c3_atk':'attack','c3_rel':'release','c3_gn':'gain'};
    this.params.comp3.forEach((band,i)=>{Object.entries(pm).forEach(([prefix,param])=>{this.bindKnobGroup(`${prefix}_${i}`,param,band[param],(v)=>{this.params.comp3[i][param]=v;this.sendComp3(i,{[param]:v});});});});
    container.querySelectorAll('[data-c3tgl]').forEach(btn=>{btn.addEventListener('click',()=>{const idx=parseInt(btn.dataset.c3band);this.params.comp3[idx].enabled=!this.params.comp3[idx].enabled;btn.textContent=this.params.comp3[idx].enabled?'ON':'OFF';btn.classList.toggle('active');this.sendComp3(idx,{enabled:this.params.comp3[idx].enabled});});});
  }

  initLimiter3Knobs(){
    const container=document.getElementById('limiter3Knobs');if(!container)return;
    let html='';
    this.params.limiter3.forEach((band,i)=>{html+=`<div class="band-section"><div class="band-title">${band.name} (Split: ${band.freq} Hz)</div><div style="display:flex;gap:4px;justify-content:center;margin-bottom:6px;"><button class="toggle-btn ${band.enabled?'':'active'}" data-l3band="${i}" data-l3tgl="enabled">${band.enabled?'ON':'OFF'}</button></div><div class="knobs-grid"><div class="knob-container"><div class="knob-wrapper" data-param="l3_freq_${i}" data-min="40" data-max="16000" data-default="${band.freq}" id="l3_freq_${i}"><div class="knob small"></div></div><div class="knob-value">${band.freq>=1000?(band.freq/1000).toFixed(1)+'k':band.freq}</div><div class="knob-label">Freq</div></div><div class="knob-container"><div class="knob-wrapper" data-param="l3_thr_${i}" data-min="-12" data-max="0" data-default="${band.threshold}" id="l3_thr_${i}"><div class="knob small"></div></div><div class="knob-value">${band.threshold} dB</div><div class="knob-label">Threshold</div></div><div class="knob-container"><div class="knob-wrapper" data-param="l3_rat_${i}" data-min="5" data-max="50" data-default="${band.ratio}" id="l3_rat_${i}"><div class="knob small"></div></div><div class="knob-value">${band.ratio}:1</div><div class="knob-label">Ratio</div></div><div class="knob-container"><div class="knob-wrapper" data-param="l3_atk_${i}" data-min="0.05" data-max="10" data-default="${band.attack}" id="l3_atk_${i}"><div class="knob small"></div></div><div class="knob-value">${band.attack} ms</div><div class="knob-label">Attack</div></div><div class="knob-container"><div class="knob-wrapper" data-param="l3_rel_${i}" data-min="5" data-max="200" data-default="${band.release}" id="l3_rel_${i}"><div class="knob small"></div></div><div class="knob-value">${band.release} ms</div><div class="knob-label">Release</div></div><div class="knob-container"><div class="knob-wrapper" data-param="l3_gn_${i}" data-min="-6" data-max="6" data-default="${band.gain}" id="l3_gn_${i}"><div class="knob small"></div></div><div class="knob-value">${band.gain} dB</div><div class="knob-label">Gain</div></div></div></div>`;});
    container.innerHTML=html;
    const pm={'l3_freq':'freq','l3_thr':'threshold','l3_rat':'ratio','l3_atk':'attack','l3_rel':'release','l3_gn':'gain'};
    this.params.limiter3.forEach((band,i)=>{Object.entries(pm).forEach(([prefix,param])=>{this.bindKnobGroup(`${prefix}_${i}`,param,band[param],(v)=>{this.params.limiter3[i][param]=v;this.sendLimiter3(i,{[param]:v});});});});
    container.querySelectorAll('[data-l3tgl]').forEach(btn=>{btn.addEventListener('click',()=>{const idx=parseInt(btn.dataset.l3band);this.params.limiter3[idx].enabled=!this.params.limiter3[idx].enabled;btn.textContent=this.params.limiter3[idx].enabled?'ON':'OFF';btn.classList.toggle('active');this.sendLimiter3(idx,{enabled:this.params.limiter3[idx].enabled});});});
  }

  initFinalLimiterKnobs(){
    const container=document.getElementById('finalLimiterKnobs');if(!container)return;
    const fl=this.params.finalLimiter;
    container.innerHTML=`<div class="knobs-grid"><div class="knob-container"><div class="knob-wrapper" data-param="fl_thr" data-min="-20" data-max="0" data-default="${fl.threshold}" id="fl_thr"><div class="knob small"></div></div><div class="knob-value">${fl.threshold} dB</div><div class="knob-label">Threshold</div></div><div class="knob-container"><div class="knob-wrapper" data-param="fl_rat" data-min="5" data-max="50" data-default="${fl.ratio}" id="fl_rat"><div class="knob small"></div></div><div class="knob-value">${fl.ratio}:1</div><div class="knob-label">Ratio</div></div><div class="knob-container"><div class="knob-wrapper" data-param="fl_atk" data-min="0.01" data-max="10" data-default="${fl.attack}" id="fl_atk"><div class="knob small"></div></div><div class="knob-value">${fl.attack} ms</div><div class="knob-label">Attack</div></div><div class="knob-container"><div class="knob-wrapper" data-param="fl_rel" data-min="5" data-max="500" data-default="${fl.release}" id="fl_rel"><div class="knob small"></div></div><div class="knob-value">${fl.release} ms</div><div class="knob-label">Release</div></div><div class="knob-container"><div class="knob-wrapper" data-param="fl_gn" data-min="-12" data-max="12" data-default="${fl.gain}" id="fl_gn"><div class="knob small"></div></div><div class="knob-value">${fl.gain} dB</div><div class="knob-label">Gain</div></div></div>`;
    this.bindKnobGroup('fl_thr','threshold',fl.threshold,(v)=>{this.params.finalLimiter.threshold=v;this.sendFinalLimiter({threshold:v});});
    this.bindKnobGroup('fl_rat','ratio',fl.ratio,(v)=>{this.params.finalLimiter.ratio=v;this.sendFinalLimiter({ratio:v});});
    this.bindKnobGroup('fl_atk','attack',fl.attack,(v)=>{this.params.finalLimiter.attack=v;this.sendFinalLimiter({attack:v});});
    this.bindKnobGroup('fl_rel','release',fl.release,(v)=>{this.params.finalLimiter.release=v;this.sendFinalLimiter({release:v});});
    this.bindKnobGroup('fl_gn','gain',fl.gain,(v)=>{this.params.finalLimiter.gain=v;this.sendFinalLimiter({gain:v});});
  }

  initPowerLimiterKnobs(){
    const container=document.getElementById('powerLimiterKnobs');if(!container)return;
    const pl=this.params.powerLimiter;
    container.innerHTML=`<div class="knobs-grid"><div class="knob-container"><div class="knob-wrapper" data-param="pl_thr" data-min="-20" data-max="0" data-default="${pl.threshold}" id="pl_thr"><div class="knob small"></div></div><div class="knob-value">${pl.threshold} dB</div><div class="knob-label">Threshold</div></div><div class="knob-container"><div class="knob-wrapper" data-param="pl_rat" data-min="5" data-max="100" data-default="${pl.ratio}" id="pl_rat"><div class="knob small"></div></div><div class="knob-value">${pl.ratio}:1</div><div class="knob-label">Ratio</div></div><div class="knob-container"><div class="knob-wrapper" data-param="pl_atk" data-min="0.01" data-max="5" data-default="${pl.attack}" id="pl_atk"><div class="knob small"></div></div><div class="knob-value">${pl.attack} ms</div><div class="knob-label">Attack</div></div><div class="knob-container"><div class="knob-wrapper" data-param="pl_rel" data-min="5" data-max="100" data-default="${pl.release}" id="pl_rel"><div class="knob small"></div></div><div class="knob-value">${pl.release} ms</div><div class="knob-label">Release</div></div><div class="knob-container"><div class="knob-wrapper" data-param="pl_gn" data-min="-12" data-max="12" data-default="${pl.gain}" id="pl_gn"><div class="knob small"></div></div><div class="knob-value">${pl.gain} dB</div><div class="knob-label">Gain</div></div></div>`;
    this.bindKnobGroup('pl_thr','threshold',pl.threshold,(v)=>{this.params.powerLimiter.threshold=v;this.sendPowerLimiter({threshold:v});});
    this.bindKnobGroup('pl_rat','ratio',pl.ratio,(v)=>{this.params.powerLimiter.ratio=v;this.sendPowerLimiter({ratio:v});});
    this.bindKnobGroup('pl_atk','attack',pl.attack,(v)=>{this.params.powerLimiter.attack=v;this.sendPowerLimiter({attack:v});});
    this.bindKnobGroup('pl_rel','release',pl.release,(v)=>{this.params.powerLimiter.release=v;this.sendPowerLimiter({release:v});});
    this.bindKnobGroup('pl_gn','gain',pl.gain,(v)=>{this.params.powerLimiter.gain=v;this.sendPowerLimiter({gain:v});});
  }

  bindKnobGroup(id,param,defaultVal,onChange){
    const wrapper=document.getElementById(id);if(!wrapper)return;
    const knob=wrapper.querySelector('.knob'),valueDisplay=wrapper.parentElement.querySelector('.knob-value');
    const min=parseFloat(wrapper.dataset.min),max=parseFloat(wrapper.dataset.max);
    let currentValue=defaultVal,isDragging=false,startY=0,startValue=0,angle=((currentValue-min)/(max-min))*270-135;
    knob.style.setProperty('--knob-angle',angle+'deg');
    const updateValue=(val)=>{
      currentValue=Math.max(min,Math.min(max,val));angle=((currentValue-min)/(max-min))*270-135;knob.style.setProperty('--knob-angle',angle+'deg');
      let display;if(param==='threshold')display=currentValue.toFixed(1)+' dB';else if(param==='ratio')display=currentValue.toFixed(1)+':1';else if(param==='attack')display=currentValue.toFixed(1)+' ms';else if(param==='release')display=Math.round(currentValue)+' ms';else if(param==='gain')display=currentValue.toFixed(1)+' dB';else if(param==='freq')display=currentValue>=1000?(currentValue/1000).toFixed(1)+'k':Math.round(currentValue);else display=currentValue.toFixed(1);
      if(valueDisplay)valueDisplay.textContent=display;
      onChange(currentValue);
    };
    wrapper.addEventListener('mousedown',(e)=>{isDragging=true;startY=e.clientY;startValue=currentValue;e.preventDefault();});
    window.addEventListener('mousemove',(e)=>{if(!isDragging)return;updateValue(startValue+(startY-e.clientY)*((max-min)/200));});
    window.addEventListener('mouseup',()=>{isDragging=false;});
    wrapper.addEventListener('wheel',(e)=>{e.preventDefault();updateValue(currentValue-e.deltaY*((max-min)/1000));},{passive:false});
    wrapper.addEventListener('dblclick',()=>updateValue(defaultVal));
  }

  presets = {
    bypass: { name:'Bypass', geq:new Float32Array(31), agc:{threshold:-30,ratio:2,attack:50,release:200}, comp3:[{freq:200,threshold:-30,ratio:2,attack:20,release:100,gain:0},{freq:2000,threshold:-28,ratio:2,attack:15,release:80,gain:0},{freq:8000,threshold:-26,ratio:1.5,attack:10,release:60,gain:0}], limiter3:[{freq:200,threshold:-6,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-6,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-6,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-3,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-1,ratio:30,attack:0.1,release:20,gain:0}, inputGain:0, outputGain:0 },
    'fm-hot': { name:'FM Hot', geq:[0,0.5,1,1.5,2,1.5,1,0.5,0,-0.5,0,0.5,1,1,0.5,0,0,0.5,1,1.5,2,2.5,2,1.5,1,0.5,0.5,1,1.5,2,2.5], agc:{threshold:-24,ratio:4,attack:8,release:80}, comp3:[{freq:200,threshold:-20,ratio:4,attack:15,release:80,gain:1},{freq:2000,threshold:-18,ratio:3.5,attack:10,release:60,gain:1.5},{freq:8000,threshold:-15,ratio:3,attack:5,release:40,gain:1}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:3, outputGain:0 },
    'fm-clean': { name:'FM Clean', geq:new Float32Array(31).fill(0), geq:[0,0,0,0,0.5,0.5,0,0,0,0,0,0,0,0,0,0,0,0,0,0.5,0.5,0.5,0.5,0.5,0,0,0,0,0,0,0], agc:{threshold:-22,ratio:3,attack:15,release:120}, comp3:[{freq:200,threshold:-22,ratio:3,attack:20,release:100,gain:0.5},{freq:2000,threshold:-20,ratio:3,attack:15,release:80,gain:0.5},{freq:8000,threshold:-18,ratio:2.5,attack:10,release:60,gain:0.5}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:0, outputGain:0 },
    streaming: { name:'Streaming', geq:[0,0,0.5,1,1,0.5,0,0,0,-0.5,0,0,0.5,0.5,0,0,0,0,0.5,1,1,1,0.5,0.5,0.5,0,0,0.5,1,1,1.5], agc:{threshold:-20,ratio:3,attack:12,release:100}, comp3:[{freq:200,threshold:-20,ratio:3.5,attack:15,release:80,gain:1},{freq:2000,threshold:-18,ratio:3,attack:10,release:60,gain:1},{freq:8000,threshold:-16,ratio:2.5,attack:5,release:40,gain:0.5}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:2, outputGain:0 },
    podcast: { name:'Podcast', geq:[0,0,0,0,-1,-2,-2,-1,0,0,0,1,2,3,3,2,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0], agc:{threshold:-18,ratio:5,attack:5,release:60}, comp3:[{freq:200,threshold:-22,ratio:4,attack:10,release:60,gain:0},{freq:2000,threshold:-20,ratio:5,attack:5,release:40,gain:2},{freq:8000,threshold:-18,ratio:3,attack:5,release:40,gain:1}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:5, outputGain:0 },
    voice: { name:'Voice', geq:[0,0,0,-1,-2,-3,-3,-2,-1,0,0,1,2,3,3,2,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0], agc:{threshold:-16,ratio:6,attack:3,release:50}, comp3:[{freq:200,threshold:-22,ratio:4,attack:10,release:60,gain:-1},{freq:2000,threshold:-18,ratio:6,attack:3,release:30,gain:3},{freq:8000,threshold:-16,ratio:3,attack:5,release:40,gain:1}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:6, outputGain:0 },
    music: { name:'Music', geq:[1,1,1.5,2,2,1.5,1,0.5,0,0,0,0,0,0,0,0,0,0,0,0.5,1,1.5,2,2,2,1.5,1,1,1,1,1.5], agc:{threshold:-24,ratio:2.5,attack:20,release:150}, comp3:[{freq:200,threshold:-22,ratio:2.5,attack:20,release:100,gain:0.5},{freq:2000,threshold:-20,ratio:2.5,attack:15,release:80,gain:0.5},{freq:8000,threshold:-18,ratio:2,attack:10,release:60,gain:0.5}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:0, outputGain:0 },
    rock: { name:'Rock', geq:[3,3,3.5,4,4,3.5,3,2,1,0,-1,-1,0,1,1,0,0,0,0.5,1,1.5,2,2.5,2.5,2.5,2,2,2.5,3,3.5,4], agc:{threshold:-22,ratio:4,attack:5,release:60}, comp3:[{freq:200,threshold:-18,ratio:5,attack:10,release:60,gain:2},{freq:2000,threshold:-16,ratio:4,attack:5,release:40,gain:2},{freq:8000,threshold:-14,ratio:3,attack:5,release:40,gain:2}], limiter3:[{freq:200,threshold:-2,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-2,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-2,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-0.5,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.3,ratio:30,attack:0.1,release:20,gain:0}, inputGain:3, outputGain:2 },
    classical: { name:'Classical', geq:new Float32Array(31).fill(0), agc:{threshold:-30,ratio:1.5,attack:100,release:500}, comp3:[{freq:200,threshold:-30,ratio:1.5,attack:30,release:150,gain:0},{freq:2000,threshold:-28,ratio:1.5,attack:20,release:120,gain:0},{freq:8000,threshold:-26,ratio:1.2,attack:15,release:100,gain:0}], limiter3:[{freq:200,threshold:-6,ratio:10,attack:2,release:100,gain:0},{freq:2000,threshold:-6,ratio:10,attack:2,release:80,gain:0},{freq:8000,threshold:-6,ratio:10,attack:1,release:60,gain:0}], finalLimiter:{threshold:-3,ratio:10,attack:1,release:100,gain:0}, powerLimiter:{threshold:-2,ratio:20,attack:0.5,release:50,gain:0}, inputGain:0, outputGain:0 },
    'bass-boost': { name:'Bass Boost', geq:[6,6,6,5.5,5,4.5,4,3,2,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0], agc:{threshold:-20,ratio:3,attack:10,release:80}, comp3:[{freq:200,threshold:-18,ratio:4,attack:15,release:80,gain:2},{freq:2000,threshold:-16,ratio:3,attack:10,release:60,gain:0},{freq:8000,threshold:-14,ratio:2.5,attack:5,release:40,gain:0}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:0, outputGain:0 },
    bright: { name:'Bright', geq:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,2,3,4,4.5,5,5], agc:{threshold:-22,ratio:3,attack:10,release:80}, comp3:[{freq:200,threshold:-20,ratio:2.5,attack:15,release:80,gain:-0.5},{freq:2000,threshold:-18,ratio:3,attack:10,release:60,gain:1},{freq:8000,threshold:-16,ratio:3,attack:5,release:40,gain:2}], limiter3:[{freq:200,threshold:-3,ratio:20,attack:1,release:50,gain:0},{freq:2000,threshold:-3,ratio:20,attack:1,release:40,gain:0},{freq:8000,threshold:-3,ratio:20,attack:0.5,release:30,gain:0}], finalLimiter:{threshold:-1,ratio:20,attack:0.5,release:50,gain:0}, powerLimiter:{threshold:-0.5,ratio:30,attack:0.1,release:20,gain:0}, inputGain:0, outputGain:0 }
  };

  loadPreset(name){
    const preset=this.presets[name]; if(!preset)return;
    if(preset.geq && preset.geq.length===31){
      for(let i=0;i<31;i++){
        const val = typeof preset.geq[i]==='number'?preset.geq[i]:0;
        this.params.geq[i]=val;
        this.audioWorklet?.port.postMessage({type:'geq',index:i,value:val});
        this.updateGEQValue(i);
      }
    } else { this.params.geq.fill(0); for(let i=0;i<31;i++){this.audioWorklet?.port.postMessage({type:'geq',index:i,value:0});this.updateGEQValue(i);} }
    this.params.agc={...this.params.agc,...preset.agc}; this.audioWorklet?.port.postMessage({type:'agc',params:this.params.agc});
    preset.comp3.forEach((band,i)=>{if(this.params.comp3[i]){this.params.comp3[i]={...this.params.comp3[i],...band};this.sendComp3(i,band);}});
    preset.limiter3.forEach((band,i)=>{if(this.params.limiter3[i]){this.params.limiter3[i]={...this.params.limiter3[i],...band};this.sendLimiter3(i,band);}});
    this.params.finalLimiter={...this.params.finalLimiter,...preset.finalLimiter}; this.sendFinalLimiter(preset.finalLimiter);
    this.params.powerLimiter={...this.params.powerLimiter,...preset.powerLimiter}; this.sendPowerLimiter(preset.powerLimiter);
    if(preset.inputGain!==undefined){this.sendParam('inputGain',preset.inputGain);this.params.inputGain=preset.inputGain;}
    if(preset.outputGain!==undefined){this.sendParam('outputGain',preset.outputGain);this.params.outputGain=preset.outputGain;}
    this.drawEQCurve();
    console.log(`Preset "${name}" loaded`);
  }

  exportConfig(){
    const config=JSON.stringify(this.params,null,2);
    const blob=new Blob([config],{type:'application/json'});
    const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download='optimod-pro-1600-config.json';a.click();URL.revokeObjectURL(url);
  }

  importConfig(file){
    if(!file)return;const reader=new FileReader();reader.onload=(e)=>{try{const config=JSON.parse(e.target.result);Object.assign(this.params,config);this.sendAllParams();this.drawEQCurve();console.log('Configuration imported');}catch(err){console.error('Import error:',err);}};reader.readAsText(file);
  }
}

const processor = new BroadcastProcessor();
document.getElementById('latencyDisplay').textContent = (processor.bufferSize / 48000 * 1000).toFixed(2) + ' ms';
let lastCpuCheck = performance.now();
setInterval(() => { const now = performance.now(); lastCpuCheck = now; processor.frameCount++; }, 16);
</script>
</body>
</html>
