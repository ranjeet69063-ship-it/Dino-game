# Dino-game
First game launched 
<!DOCTYPE html>
<html>
<head>
  <title>Dino Game with Speedometer</title>
  <style>
    body { margin: 0; text-align: center; background: #f7f7f7; }
    #game {
      width: 100%; height: 400px;
      background: white; border: 2px solid black;
      position: relative; overflow: hidden;
    }
    #dino {
      width: 50px; height: 50px;
      background: black; position: absolute;
      bottom: 0; left: 80px;
    }
    .obstacle {
      position: absolute;
      bottom: 0;
    }
    #cactus {
      width: 30px; height: 60px; background: green; right: 0;
    }
    #bird {
      width: 50px; height: 30px; background: red; bottom: 150px; right: -200px;
    }
    #stone {
      width: 40px; height: 20px; background: gray; bottom: 0; right: -400px;
    }
    #scoreboard {
      font-family: Arial; font-size: 20px;
      margin-top: 10px; font-weight: bold;
    }
    #speedometer {
      font-family: Arial; font-size: 16px;
      margin-top: 5px; font-weight: bold; color: blue;
    }
  </style>
</head>
<body>
  <h2>🐱‍👤 Tap / Press Space to Jump</h2>
  <div id="game">
    <div id="dino"></div>
    <div id="cactus" class="obstacle"></div>
    <div id="bird" class="obstacle"></div>
    <div id="stone" class="obstacle"></div>
  </div>
  <div id="scoreboard">Score: 0</div>
  <div id="speedometer">Speed: 6</div>

  <script>
    let dino = document.getElementById("dino");
    let cactus = document.getElementById("cactus");
    let bird = document.getElementById("bird");
    let stone = document.getElementById("stone");
    let scoreDisplay = document.getElementById("scoreboard");
    let speedDisplay = document.getElementById("speedometer");

    let isJumping = false, score = 0, speed = 6;

    function jump() {
      if (isJumping) return;
      isJumping = true;
      let pos = 0;
      let up = setInterval(() => {
        if (pos >= 180) {
          clearInterval(up);
          let down = setInterval(() => {
            if (pos <= 0) {
              clearInterval(down);
              isJumping = false;
            }
            pos -= 6;
            dino.style.bottom = pos + "px";
          }, 20);
        }
        pos += 6;
        dino.style.bottom = pos + "px";
      }, 20);
    }

    // Controls
    document.addEventListener("keydown", e => { if (e.code === "Space" || e.code === "ArrowUp") jump(); });
    document.addEventListener("click", jump);
    document.addEventListener("touchstart", jump);

    // Move function for obstacles
    function moveObstacle(obstacle, extraSpeed = 0) {
      let obstacleLeft = parseInt(window.getComputedStyle(obstacle).getPropertyValue("left"));
      let dinoBottom = parseInt(window.getComputedStyle(dino).getPropertyValue("bottom"));

      if (obstacleLeft <= 110 && obstacleLeft > 60 && dinoBottom < (obstacle.offsetHeight)) {
        alert("💥 Game Over! Your Score: " + score);
        score = 0; speed = 6;
        cactus.style.left = "100%";
        bird.style.left = "100%";
        stone.style.left = "100%";
      } else {
        obstacle.style.left = (obstacleLeft - (speed + extraSpeed)) + "px";
      }

      if (obstacleLeft < -100) {
        obstacle.style.left = "100%";
        score++;
        scoreDisplay.innerText = "Score: " + score;
        if (score % 5 === 0) {
          speed++; // increase main speed
          speedDisplay.innerText = "Speed: " + speed;
        }
      }
    }

    // Game Loop
    setInterval(() => {
      moveObstacle(cactus, 0);
      moveObstacle(bird, 2);
      moveObstacle(stone, 1);
    }, 20);
  </script>
</body>
</html>
