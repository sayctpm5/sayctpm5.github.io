<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Is this dog the father?</title>
  <style>
    :root{
      --bg:#0b0b0b;
      --card:#0f0f0f;
      --accent:#ff2f2f;
      --text:#fff;
    }
    html,body{
      height:100%;
      margin:0;
      font-family:system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: linear-gradient(180deg,#050505 0%, #0b0b0b 100%);
      color:var(--text);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
    }

    /* Centering container */
    .wrap{
      min-height:100%;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
      box-sizing:border-box;
    }

    /* Big red button */
    .big-btn{
      background:var(--accent);
      color:#fff;
      border:0;
      padding:30px 44px;
      font-size:28px;
      font-weight:700;
      border-radius:16px;
      box-shadow:0 10px 30px rgba(0,0,0,0.6), 0 3px 8px rgba(0,0,0,0.4) inset;
      cursor:pointer;
      transition:transform .14s ease, box-shadow .14s ease, filter .14s;
      letter-spacing: .2px;
    }
    .big-btn:active{ transform:translateY(2px) scale(.997); }
    .big-btn:focus{ outline:3px solid rgba(255,47,47,0.15); outline-offset:4px; }

    /* Modal (video player) */
    .modal {
      position: fixed;
      inset: 0;
      display: none;
      align-items: center;
      justify-content: center;
      background: rgba(0,0,0,0.65);
      z-index: 1000;
      padding: 24px;
    }
    .modal.show{ display:flex; }

    .player {
      width: min(1100px, 96%);
      background: var(--card);
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 30px 80px rgba(0,0,0,0.7);
      position:relative;
    }

    video{
      width:100%;
      height:auto;
      display:block;
      background: black;
    }

    .controls {
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      padding:10px 12px;
      background: rgba(0,0,0,0.35);
    }

    .close-btn{
      position:absolute;
      right:8px;
      top:8px;
      background: rgba(0,0,0,0.5);
      border:0;
      color:#fff;
      padding:8px 10px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
    }

    .alt-panel{
      margin-top:12px;
      text-align:center;
      color:#ddd;
      font-size:13px;
    }

    /* Small screens adapt */
    @media (max-width:520px){
      .big-btn{ padding:22px 26px; font-size:20px; border-radius:12px; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <button id="openBtn" class="big-btn" aria-haspopup="dialog" aria-controls="videoModal">
      Is this dog the father?
    </button>
  </div>

  <!-- Modal with video -->
  <div id="videoModal" class="modal" role="dialog" aria-modal="true" aria-labelledby="dialogTitle">
    <div class="player" role="document">
      <button class="close-btn" id="closeBtn" title="Close player">✕</button>

      <!-- Primary: hosted video (replace the src path with your uploaded file path) -->
      <video id="theVideo" controls playsinline preload="metadata" tabindex="0">
        <!-- Default source: change path if your file name/path differs -->
        <source src="assets/dogfather.mp4" type="video/mp4">
        <!-- Fallback note -->
        Your browser does not support the video tag. If the video doesn't load, check that the file is uploaded to <code>assets/dogfather.mp4</code> in the repo or choose a file below.
      </video>

      <div class="controls">
        <div>
          <button id="playPause">Play</button>
        </div>
        <div style="flex:1;text-align:center;color:#eee;font-size:13px;">
          Tip: Click the video or the Play button. Press Esc to close.
        </div>
        <div style="text-align:right;">
          <!-- Local-file fallback: user can pick a local file to test -->
          <label style="cursor:pointer;font-size:13px;">
            <input id="fileInput" type="file" accept="video/*" style="display:none">
            <span style="text-decoration:underline;opacity:0.95;cursor:pointer">Use local file</span>
          </label>
        </div>
      </div>

      <div class="alt-panel" aria-hidden="true">
        Recommended upload format: MP4 (H.264 video, AAC audio). Keep file size reasonable for web.
      </div>
    </div>
  </div>

  <script>
    const openBtn = document.getElementById('openBtn');
    const modal = document.getElementById('videoModal');
    const closeBtn = document.getElementById('closeBtn');
    const video = document.getElementById('theVideo');
    const playPause = document.getElementById('playPause');
    const fileInput = document.getElementById('fileInput');

    // Open modal and play
    function openPlayer(){
      modal.classList.add('show');
      // try to load and play the hosted video; play may be blocked on some devices unless user interacts (we already are in a click handler)
      // reset to start
      try { video.currentTime = 0; } catch(e){}
      video.play().catch(()=> {
        // If play is blocked, update button text and let user click play
        playPause.textContent = 'Play';
      });
      // focus on video for keyboard controls
      setTimeout(()=> video.focus(), 150);
    }

    // Close modal and pause
    function closePlayer(){
      modal.classList.remove('show');
      try { video.pause(); } catch(e){}
      // if we used an object URL, revoke it when closed (handled on file select)
      openBtn.focus();
    }

    openBtn.addEventListener('click', openPlayer);
    closeBtn.addEventListener('click', closePlayer);

    // Play/pause toggle
    playPause.addEventListener('click', () => {
      if (video.paused) {
        video.play();
      } else {
        video.pause();
      }
    });

    video.addEventListener('play', () => { playPause.textContent = 'Pause'; });
    video.addEventListener('pause', () => { playPause.textContent = 'Play'; });

    // close on background click (but not when clicking player itself)
    modal.addEventListener('click', (e) => {
      if (e.target === modal) closePlayer();
    });

    // Esc to close
    window.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && modal.classList.contains('show')) closePlayer();
    });

    // Local file picker fallback (for testing)
    let currentObjectURL = null;
    fileInput.addEventListener('change', (ev) => {
      const f = ev.target.files && ev.target.files[0];
      if (!f) return;
      if (currentObjectURL) {
        URL.revokeObjectURL(currentObjectURL);
        currentObjectURL = null;
      }
      currentObjectURL = URL.createObjectURL(f);
      // Swap video source to the local file and play
      video.src = currentObjectURL;
      video.load();
      video.play().catch(()=>{});
      // open modal if not already open
      if (!modal.classList.contains('show')) modal.classList.add('show');
    });

    // Accessibility: trap focus within modal while open (simple)
    document.addEventListener('focus', function (e) {
      if (!modal.classList.contains('show')) return;
      if (!modal.contains(e.target)) {
        e.stopPropagation();
        modal.focus();
      }
    }, true);
  </script>
</body>
</html>
