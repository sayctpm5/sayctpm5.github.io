<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Is this dog the father?</title>
<style>
  :root{
    --bg:#0b0b0b;
    --card:#141414;
    --accent:#ff2f2f;
    --text:#fff;
  }
  html,body{
    margin:0;height:100%;
    font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;
    background:linear-gradient(180deg,#050505,#0b0b0b);
    color:var(--text);
  }
  .wrap{
    min-height:100%;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    gap:30px;
    padding:24px;
    box-sizing:border-box;
  }
  .big-btn{
    background:var(--accent);
    color:#fff;
    border:0;
    padding:30px 44px;
    font-size:28px;
    font-weight:700;
    border-radius:16px;
    box-shadow:0 10px 30px rgba(0,0,0,0.6);
    cursor:pointer;
    transition:transform .14s, box-shadow .14s;
  }
  .big-btn:active{transform:translateY(2px) scale(.98);}
  .poll{
    display:flex;
    gap:24px;
    flex-wrap:wrap;
    justify-content:center;
    background:rgba(255,255,255,0.05);
    padding:20px 30px;
    border-radius:12px;
    text-align:center;
  }
  .vote-btn{
    background:#202020;
    color:#fff;
    border:2px solid var(--accent);
    border-radius:8px;
    padding:14px 20px;
    font-size:18px;
    cursor:pointer;
    transition:background .15s,border-color .15s;
  }
  .vote-btn:hover{background:#2b2b2b;}
  .vote-count{
    display:block;
    font-size:14px;
    margin-top:6px;
    color:#ddd;
  }
  /* Modal styling */
  .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,0.65);z-index:1000;padding:24px;}
  .modal.show{display:flex;}
  .player{width:min(1100px,96%);background:var(--card);border-radius:12px;overflow:hidden;box-shadow:0 30px 80px rgba(0,0,0,0.7);position:relative;}
  video{width:100%;display:block;background:black;}
  .close-btn{position:absolute;right:8px;top:8px;background:rgba(0,0,0,0.5);border:0;color:#fff;padding:8px 10px;border-radius:8px;cursor:pointer;font-weight:600;}
  .controls{display:flex;align-items:center;justify-content:space-between;padding:10px 12px;background:rgba(0,0,0,0.35);}
  .alt-panel{margin-top:10px;text-align:center;color:#aaa;font-size:13px;}
</style>
</head>
<body>
<div class="wrap">
  <button id="openBtn" class="big-btn">Is this dog the father?</button>

  <!-- Poll section -->
  <div class="poll">
    <div>
      <button class="vote-btn" id="yesBtn">Is the father</button>
      <span class="vote-count" id="yesCount">0 votes</span>
    </div>
    <div>
      <button class="vote-btn" id="noBtn">Not the father</button>
      <span class="vote-count" id="noCount">0 votes</span>
    </div>
  </div>
</div>

<!-- Video modal -->
<div id="videoModal" class="modal">
  <div class="player">
    <button class="close-btn" id="closeBtn">✕</button>
    <video id="theVideo" controls playsinline preload="metadata">
      <source src="assets/dogfather.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <div class="controls">
      <button id="playPause">Play</button>
      <label>
        <input id="fileInput" type="file" accept="video/*" style="display:none">
        <span style="text-decoration:underline;cursor:pointer;">Use local file</span>
      </label>
    </div>
    <div class="alt-panel">Upload <code>assets/dogfather.mp4</code> to your repo.</div>
  </div>
</div>

<script>
  // ----- Video modal logic -----
  const openBtn=document.getElementById('openBtn');
  const modal=document.getElementById('videoModal');
  const closeBtn=document.getElementById('closeBtn');
  const video=document.getElementById('theVideo');
  const playPause=document.getElementById('playPause');
  const fileInput=document.getElementById('fileInput');
  let currentObjectURL=null;

  function openPlayer(){
    modal.classList.add('show');
    video.currentTime=0;
    video.play().catch(()=>{});
  }
  function closePlayer(){
    modal.classList.remove('show');
    video.pause();
  }
  openBtn.onclick=openPlayer;
  closeBtn.onclick=closePlayer;
  modal.onclick=(e)=>{if(e.target===modal)closePlayer();};
  playPause.onclick=()=>video.paused?video.play():video.pause();
  video.onplay=()=>playPause.textContent='Pause';
  video.onpause=()=>playPause.textContent='Play';
  fileInput.onchange=(ev)=>{
    const f=ev.target.files[0];
    if(!f)return;
    if(currentObjectURL)URL.revokeObjectURL(currentObjectURL);
    currentObjectURL=URL.createObjectURL(f);
    video.src=currentObjectURL;
    video.play();
    if(!modal.classList.contains('show'))modal.classList.add('show');
  };

  // ----- Poll logic -----
  const yesBtn=document.getElementById('yesBtn');
  const noBtn=document.getElementById('noBtn');
  const yesCount=document.getElementById('yesCount');
  const noCount=document.getElementById('noCount');

  // load saved votes (localStorage)
  let votesYes=parseInt(localStorage.getItem('dogfather_yes')||'0');
  let votesNo=parseInt(localStorage.getItem('dogfather_no')||'0');
  function updateCounts(){
    yesCount.textContent=`${votesYes} vote${votesYes!==1?'s':''}`;
    noCount.textContent=`${votesNo} vote${votesNo!==1?'s':''}`;
  }
  updateCounts();

  yesBtn.onclick=()=>{
    votesYes++;
    localStorage.setItem('dogfather_yes',votesYes);
    updateCounts();
    yesBtn.disabled=true; noBtn.disabled=true;
  };
  noBtn.onclick=()=>{
    votesNo++;
    localStorage.setItem('dogfather_no',votesNo);
    updateCounts();
    yesBtn.disabled=true; noBtn.disabled=true;
  };
</script>
</body>
</html>
