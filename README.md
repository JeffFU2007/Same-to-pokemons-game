<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>心理學冒險遊戲</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f0f8ff;
    }
    #gameCanvas {
      border: 2px solid black;
      margin-top: 20px;
    }
    #dialogBox {
      margin: 20px;
      padding: 10px;
      background-color: #fff;
      border: 2px solid #000;
      display: none;
    }
    button {
      margin-top: 10px;
      padding: 5px 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
    #quiz {
      margin: 20px;
    }
  </style>
</head>
<body>

  <h1>心理學冒險遊戲</h1>

  <canvas id="gameCanvas" width="800" height="600"></canvas>
  <div id="dialogBox"></div>
  <div id="quiz"></div>

  <script>
    // 地圖探索功能
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const gridSize = 50;
    const mapWidth = 16;
    const mapHeight = 12;
    let playerX = 1, playerY = 1;

    function drawMap() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);  // 清空畫布

      for (let i = 0; i < mapWidth; i++) {
        for (let j = 0; j < mapHeight; j++) {
          ctx.fillStyle = (i === playerX && j === playerY) ? 'blue' : 'lightgreen';
          ctx.fillRect(i * gridSize, j * gridSize, gridSize, gridSize);
          ctx.strokeRect(i * gridSize, j * gridSize, gridSize, gridSize);
        }
      }
    }

    function movePlayer(direction) {
      if (direction === 'up' && playerY > 0) playerY--;
      if (direction === 'down' && playerY < mapHeight - 1) playerY++;
      if (direction === 'left' && playerX > 0) playerX--;
      if (direction === 'right' && playerX < mapWidth - 1) playerX++;
      drawMap();
    }

    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowUp') movePlayer('up');
      if (e.key === 'ArrowDown') movePlayer('down');
      if (e.key === 'ArrowLeft') movePlayer('left');
      if (e.key === 'ArrowRight') movePlayer('right');
    });

    drawMap();

    // NPC互動功能
    const npcData = {
      name: "村莊長者",
      dialogue: [
        "你是否準備好踏上旅程？",
        "你選擇的路將決定你的一生。"
      ],
      options: [
        { text: "是的，我準備好了", nextEvent: "startQuest" },
        { text: "我還需要更多時間", nextEvent: "askMore" }
      ]
    };

    function showDialogue() {
      let dialogueBox = document.getElementById('dialogBox');
      dialogueBox.style.display = 'block';
      dialogueBox.innerHTML = `<h3>${npcData.name}</h3><p>${npcData.dialogue[0]}</p>`;
      npcData.options.forEach(option => {
        let button = document.createElement('button');
        button.innerText = option.text;
        button.onclick = () => handleChoice(option.nextEvent);
        dialogueBox.appendChild(button);
      });
    }

    function handleChoice(event) {
      let dialogueBox = document.getElementById('dialogBox');
      if (event === "startQuest") {
        alert("你已開始你的冒險!");
        dialogueBox.innerHTML = "你的冒險旅程即將開始。";
      } else {
        alert("你決定等待更多的時間...");
      }
    }

    // 假設當玩家進入NPC範圍時會觸發這個函數
    if (playerX === 5 && playerY === 5) {
      showDialogue();
    }

    // 心理學測驗功能
    const quizQuestions = [
      { question: "當你面對挑戰時，你通常怎麼反應?", options: ["冷靜分析", "焦慮", "逃避"] },
      { question: "當面臨困難選擇時，你更傾向於?", options: ["尋求他人幫助", "獨自解決", "拖延"] }
    ];

    function showQuiz() {
      let quizDiv = document.getElementById('quiz');
      quizQuestions.forEach(q => {
        let questionDiv = document.createElement('div');
        questionDiv.innerHTML = `<p>${q.question}</p>`;
        q.options.forEach(option => {
          let button = document.createElement('button');
          button.innerText = option;
          button.onclick = () => submitAnswer(option);
          questionDiv.appendChild(button);
        });
        quizDiv.appendChild(questionDiv);
      });
    }

    function submitAnswer(answer) {
      alert(`你選擇了：${answer}`);
      // 基於選擇給出即時心理學分析
    }

    showQuiz();

    // 角色發展與進度儲存
    let playerAttributes = {
      intelligence: 5,
      emotionalControl: 5
    };

    function updateAttributes(choice) {
      if (choice === '冷靜分析') {
        playerAttributes.intelligence += 1;
      } else if (choice === '焦慮') {
        playerAttributes.emotionalControl -= 1;
      }
      console.log("Updated Attributes:", playerAttributes);
    }

    // 儲存進度
    localStorage.setItem('playerProgress', JSON.stringify(playerAttributes));

    // 讀取進度
    let playerData = JSON.parse(localStorage.getItem('playerProgress'));
    console.log(playerData);

  </script>
</body>
</html>
