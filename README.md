<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stopwatch Application</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f4f4f4;
    }

    .stopwatch {
      text-align: center;
      background-color: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .time {
      font-size: 48px;
      margin-bottom: 20px;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 15px;
    }

    .buttons button {
      padding: 10px 20px;
      font-size: 18px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
      background-color: #333;
      color: white;
      transition: background-color 0.3s;
    }

    .buttons button:hover {
      background-color: #555;
    }

    .laps {
      margin-top: 20px;
    }

    .lap-list {
      list-style-type: none;
      padding: 0;
      max-height: 150px;
      overflow-y: auto;
      border-top: 1px solid #ccc;
      margin-top: 10px;
    }

    .lap-item {
      background-color: #e9e9e9;
      padding: 5px 10px;
      margin-bottom: 5px;
      border-radius: 5px;
    }

  </style>
</head>
<body>

  <div class="stopwatch">
    <div class="time" id="time">00:00:00.000</div>
    <div class="buttons">
      <button id="startBtn">Start</button>
      <button id="pauseBtn" disabled>Pause</button>
      <button id="resetBtn" disabled>Reset</button>
      <button id="lapBtn" disabled>Lap</button>
    </div>
    <div class="laps">
      <h3>Laps</h3>
      <ul class="lap-list" id="laps"></ul>
    </div>
  </div>

  <script>
    let startTime = 0;
    let elapsedTime = 0;
    let timerInterval;
    let lapCount = 0;

    const timeDisplay = document.getElementById("time");
    const startBtn = document.getElementById("startBtn");
    const pauseBtn = document.getElementById("pauseBtn");
    const resetBtn = document.getElementById("resetBtn");
    const lapBtn = document.getElementById("lapBtn");
    const lapsContainer = document.getElementById("laps");

    function formatTime(time) {
      let date = new Date(time);
      let minutes = String(date.getUTCMinutes()).padStart(2, '0');
      let seconds = String(date.getUTCSeconds()).padStart(2, '0');
      let milliseconds = String(date.getUTCMilliseconds()).padStart(3, '0');
      return `${minutes}:${seconds}.${milliseconds}`;
    }

    function startTimer() {
      startTime = Date.now() - elapsedTime;
      timerInterval = setInterval(() => {
        elapsedTime = Date.now() - startTime;
        timeDisplay.textContent = formatTime(elapsedTime);
      }, 10);
      startBtn.disabled = true;
      pauseBtn.disabled = false;
      resetBtn.disabled = false;
      lapBtn.disabled = false;
    }

    function pauseTimer() {
      clearInterval(timerInterval);
      startBtn.disabled = false;
      pauseBtn.disabled = true;
    }

    function resetTimer() {
      clearInterval(timerInterval);
      elapsedTime = 0;
      lapCount = 0;
      timeDisplay.textContent = "00:00:00.000";
      lapsContainer.innerHTML = "";
      startBtn.disabled = false;
      pauseBtn.disabled = true;
      resetBtn.disabled = true;
      lapBtn.disabled = true;
    }

    function recordLap() {
      lapCount++;
      const lapTime = formatTime(elapsedTime);
      const lapItem = document.createElement("li");
      lapItem.className = "lap-item";
      lapItem.textContent = `Lap ${lapCount}: ${lapTime}`;
      lapsContainer.appendChild(lapItem);
    }

    startBtn.addEventListener("click", startTimer);
    pauseBtn.addEventListener("click", pauseTimer);
    resetBtn.addEventListener("click", resetTimer);
    lapBtn.addEventListener("click", recordLap);
  </script>

</body>
</html>

