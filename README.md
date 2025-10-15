<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>About Me 🦁</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: "Helvetica Neue", Arial, sans-serif;
      color: #222;
      background: #fdfdfd;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 2rem;
      position: relative;
      overflow: hidden;
    }

    body::before {
      content: "";
      position: absolute;
      inset: 0;
      background-image: url("https://upload.wikimedia.org/wikipedia/commons/7/73/Lion_icon.svg");
      background-repeat: no-repeat;
      background-position: center;
      background-size: 200px;
      opacity: 0.05;
      pointer-events: none;
    }

    h1 {
      font-size: 2.2rem;
      color: #222;
      margin-bottom: 0.5rem;
      z-index: 1;
    }

    h2 {
      font-size: 1.1rem;
      color: #555;
      font-weight: normal;
      margin-bottom: 1.5rem;
      z-index: 1;
    }

    p {
      max-width: 600px;
      line-height: 1.6;
      color: #444;
      z-index: 1;
    }

    .button {
      display: inline-block;
      margin-top: 2rem;
      padding: 0.7rem 1.5rem;
      background-color: #0066cc;
      color: #fff;
      text-decoration: none;
      border-radius: 6px;
      transition: background-color 0.3s;
      z-index: 1;
      position: relative;
      cursor: pointer;
    }

    .button:hover {
      background-color: #004999;
    }
  </style>
</head>
<body>
  <h1>Hey Jerry, want to hear a lion roar?</h1>
  <h2>Juat click the blue button.</h2>
  <p>
    🦁
  </p>

  <!-- Button that plays roar -->
  <a class="button" onclick="playRoar()">Press Here</a>

  <!-- Audio element -->
  <audio id="roarSound" src="lion.mp3"></audio>

  <script>
    function playRoar() {
      const sound = document.getElementById('roarSound');
      sound.currentTime = 0; // Restart if clicked again
      sound.play();
    }
  </script>
</body>
</html>
