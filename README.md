<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Interactive wave simulation showing effects of frequency, amplitude, and wavelength." />
  <meta name="author" content="Your Name" />
  <title>Wave Simulation</title>

  <style>
    /* ===== General Page Style ===== */
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #0d1117;
      color: #ffffff;
      margin: 0;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
    }

    h1 {
      margin-top: 40px;
      color: #00ffff;
      letter-spacing: 1px;
    }

    /* ===== Canvas Styling ===== */
    canvas {
      border: 1px solid #30363d;
      background: #010409;
      margin-top: 20px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0, 255, 255, 0.2);
    }

    /* ===== Controls Styling ===== */
    .controls {
      margin: 20px auto;
      width: 80%;
      max-width: 600px;
      background: #161b22;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);
    }

    label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin: 10px 0;
    }

    input[type="range"] {
      width: 60%;
      accent-color: #00ffff;
    }

    .value {
      width: 60px;
      text-align: right;
    }

    footer {
      margin-top: auto;
      padding: 10px;
      color: #999;
      font-size: 14px;
    }

    footer a {
      color: #00ffff;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <h1>🌊 Wave Simulation</h1>

  <!-- ===== Control Panel ===== -->
  <div class="controls">
    <label>
      Amplitude:
      <input type="range" id="amplitude" min="10" max="100" value="50">
      <span class="value" id="ampValue">50</span>
    </label>
    <label>
      Frequency:
      <input type="range" id="frequency" min="0.5" max="5" value="1" step="0.1">
      <span class="value" id="freqValue">1.0</span>
    </label>
    <label>
      Wavelength:
      <input type="range" id="wavelength" min="100" max="500" value="200">
      <span class="value" id="waveValue">200</span>
    </label>
  </div>

  <!-- ===== Canvas for Wave Animation ===== -->
  <canvas id="waveCanvas" width="800" height="400"></canvas>

  <footer>
    Made with ❤️ by <a href="https://github.com/yourusername" target="_blank">Your Name</a> |
    <a href="https://github.com/yourusername/wave-simulation" target="_blank">View Source on GitHub</a>
  </footer>

  <script>
    // ===== Wave Simulation Script =====

    const canvas = document.getElementById("waveCanvas");
    const ctx = canvas.getContext("2d");

    // Default parameters
    let amplitude = 50;
    let frequency = 1;
    let wavelength = 200;
    let time = 0;

    // UI elements
    const ampSlider = document.getElementById("amplitude");
    const freqSlider = document.getElementById("frequency");
    const waveSlider = document.getElementById("wavelength");
    const ampValue = document.getElementById("ampValue");
    const freqValue = document.getElementById("freqValue");
    const waveValue = document.getElementById("waveValue");

    // Event listeners for controls
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

    // Main draw loop
    function drawWave() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.beginPath();

      // Draw sine wave using math equation
      for (let x = 0; x < canvas.width; x++) {
        const y = canvas.height / 2 + amplitude * Math.sin((2 * Math.PI * frequency * (x / wavelength)) - time);
        ctx.lineTo(x, y);
      }

      // Style the wave
      ctx.strokeStyle = "#00ffff";
      ctx.lineWidth = 2;
      ctx.stroke();

      // Animate by updating time
      time += 0.05;
      requestAnimationFrame(drawWave);
    }

    // Start the animation
    drawWave();
  </script>
</body>
</html>
