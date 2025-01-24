<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Game Tài Xỉu</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #f4f4f4;
      color: #333;
    }
    .container {
      margin-top: 50px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin: 10px;
      cursor: pointer;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
    }
    button:hover {
      background-color: #0056b3;
    }
    .result {
      margin-top: 20px;
      font-size: 20px;
    }
    .dice {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 20px 0;
    }
    .dice canvas {
      width: 50px;
      height: 50px;
      border: 1px solid #333;
      background-color: white;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Trò chơi Tài Xỉu</h1>
    <p>Đặt cược của bạn:</p>
    <button onclick="playGame('tai')">Tài (11-18)</button>
    <button onclick="playGame('xiu')">Xỉu (3-10)</button>

    <div class="dice" id="dice"></div>
    <div class="result" id="result"></div>
  </div>

  <script>
    function rollDice() {
      // Tung 3 xúc xắc
      return [
        Math.floor(Math.random() * 6) + 1,
        Math.floor(Math.random() * 6) + 1,
        Math.floor(Math.random() * 6) + 1
      ];
    }

    function playGame(choice) {
      const dice = rollDice();
      const total = dice.reduce((sum, value) => sum + value, 0);
      const resultText = document.getElementById('result');
      const diceContainer = document.getElementById('dice');

      diceContainer.innerHTML = '';
      dice.forEach(value => {
        const canvas = document.createElement('canvas');
        canvas.width = 50;
        canvas.height = 50;
        drawDice(canvas, value);
        diceContainer.appendChild(canvas);
      });

      if (total >= 11 && total <= 18) {
        resultText.innerHTML = `Tổng: ${total} (Tài)<br>${choice === 'tai' ? 'Bạn thắng!' : 'Bạn thua!'}`;
      } else {
        resultText.innerHTML = `Tổng: ${total} (Xỉu)<br>${choice === 'xiu' ? 'Bạn thắng!' : 'Bạn thua!'}`;
      }
    }

    function drawDice(canvas, value) {
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Vẽ viền xúc xắc
      ctx.fillStyle = 'white';
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = 'black';
      ctx.strokeRect(0, 0, canvas.width, canvas.height);

      // Tọa độ chấm tròn
      const dotPositions = {
        1: [[25, 25]],
        2: [[10, 10], [40, 40]],
        3: [[10, 10], [25, 25], [40, 40]],
        4: [[10, 10], [10, 40], [40, 10], [40, 40]],
        5: [[10, 10], [10, 40], [25, 25], [40, 10], [40, 40]],
        6: [[10, 10], [10, 25], [10, 40], [40, 10], [40, 25], [40, 40]]
      };

      ctx.fillStyle = 'black';
      dotPositions[value].forEach(([x, y]) => {
        ctx.beginPath();
        ctx.arc(x, y, 5, 0, Math.PI * 2);
        ctx.fill();
      });
    }
  </script>
</body>
</html>
