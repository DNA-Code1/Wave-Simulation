<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Interactive wave simulation showing effects of frequency, amplitude, and wavelength." />
  <title>Wave Simulation</title>

  <style>
    /* ===== Global Styles ===== */
    :root {
      --bg-gradient: linear-gradient(135deg, #001f3f, #001233, #000814);
      --panel-bg: rgba(255, 255, 255, 0.05);
      --accent: #00e0ff;
      --text: #f0f0f0;
      --wave-color: #00f5ff;
    }

    * {
      box-sizing: border-box;
      transition: 0.2s ease;
    }

    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      background: var(--bg-gradient);
      color: var(--text);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      min-height: 100vh;
      overflow-x: hidden;
    }

    h1 {
      margin-top: 40px;
      font-size: 2.2rem;
      background: linear-gradient(90deg, #00e0ff, #00ff8f);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      letter-spacing: 1px;
      text-shadow: 0 0 10px rgba(0, 255, 255, 0.3);
    }

    /* ===== Canvas ===== */
    canvas {
      border-radius: 12px;
      margin-top: 25px;
      background: rgba(0, 0, 0, 0.3);
      box-shadow: 0 0 25px rgba(0, 255, 255, 0.15);
    }

    /* ===== Control Panel ===== */
    .controls {
      margin-top: 25px;
      background: var(--panel-bg);
      padding: 25px 35px;
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(0, 255, 255, 0.1);
      backdrop-filter: blur(12px);
      width: 90%;
      max-width: 600px;
    }

    .controls label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 1rem;
      margin: 15px 0;
      color: #d8faff;
    }

    .slider-group {
      display: flex;
      align-items: center;
      width: 100%;
      gap: 15px;
    }

    .slider-group input[type="range"] {
      flex: 1;
      appearance: none;
      height: 6px;
      background: rgba(0, 255, 255, 0.15);
      border-radius: 5px;
      outline: none;
    }

    .slider-group input[type="range"]::-webkit-slider-thumb {
      appearance: none;
      width: 18px;
      height: 18px;
      background: var(--accent);
      border-radius: 50%;
      box-shadow: 0 0 10px var(--accent);
      cursor: pointer;
    }

    .slider-group input[type="range"]:hover::-webkit-slider-thumb {
      transform: scale(1.2);
    }

    .value {
      width: 45px;
      text-align: right;
      font-weight: 500;
      color: #aefcff;
    }

    @media (max-width: 600px) {
      h1 {
        font-size: 1.8rem;
      }

      .controls {
        padding: 20px;
      }
    }
  </style>
</head>
<body>
  <h1>Wave Simulation</h1>

  <!-- Control Panel -->
  <div class="controls">
    <label>
      Amplitude
      <div class="slider-group">
        <input type="range" id="amplitude" min="10" max="100" value="50">
        <span class="value" id="ampValue">50</span>
      </div>
    </label>

    <label>
      Frequency
      <div class="slider-group">
        <input type="range" id="frequency" min="0.5" max="5" value="1" step="0.1">
        <span class="value" id="freqValue">1.0</span>
      </div>
    </label>

    <label>
      Wavelength
      <div class="slider-group">
        <input type="range" id="wavelength" min="100" max="500" value="200">
        <span class="value" id="waveValue">200</span>
      </div>
    </label>
  </div>

  <!-- Wave Canvas -->
  <canvas id="waveCanvas" width="900" height="400"></canvas>

  <script>
    // ===== Wave Simulation Logic =====
    const canvas = document.getElementById("waveCanvas");
    const ctx = canvas.getContext("2d");

    let amplitude = 50;
    let frequency = 1;
    let wavelength = 200;
    let time = 0;

    const ampSlider = document.getElementById("amplitude");
    const freqSlider = document.getElementById("frequency");
    const waveSlider = document.getElementById("wavelength");
    const ampValue = document.getElementById("ampValue");
    const freqValue = document.getElementById("freqValue");
    const waveValue = document.getElementById("waveValue");

    ampSlider.oninput = (e) => {
      amplitude = +e.target.value;
      ampValue.textContent = amplitude;
    };
    freqSlider.oninput = (e) => {
      frequency = +e.target.value;
      freqValue.textContent = frequency.toFixed(1);
    };
    waveSlider.oninput = (e) => {
      wavelength = +e.target.value;
      waveValue.textContent = wavelength;
    };

    function drawWave() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw gradient background on canvas
      const gradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
      gradient.addColorStop(0, "rgba(0, 255, 255, 0.1)");
      gradient.addColorStop(1, "rgba(0, 0, 50, 0.2)");
      ctx.fillStyle = gradient;
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Draw the sine wave
      ctx.beginPath();
      for (let x = 0; x < canvas.width; x++) {
        const y = canvas.height / 2 + amplitude * Math.sin((2 * Math.PI * frequency * (x / wavelength)) - time);
        ctx.lineTo(x, y);
      }

      ctx.strokeStyle = "#00f5ff";
      ctx.lineWidth = 3;
      ctx.shadowColor = "#00f5ff";
      ctx.shadowBlur = 10;
      ctx.stroke();

      // Reset shadow for next frame
      ctx.shadowBlur = 0;

      time += 0.05;
      requestAnimationFrame(drawWave);
    }

    drawWave();
  </script>
</body>
</html>
