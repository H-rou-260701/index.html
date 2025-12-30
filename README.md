<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<title>Stray Kids Bias Gallery</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
  background: linear-gradient(135deg, #0c0c0c, #3b0000);
  color: white;
}

header {
  text-align: center;
  padding: 20px;
  font-size: 32px;
  font-weight: bold;
}

.container {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
}

.idols {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}

.card {
  background: rgba(255,255,255,0.08);
  border-radius: 18px;
  padding: 15px;
  text-align: center;
  cursor: pointer;
  transition: transform 0.3s;
}

.card:hover {
  transform: scale(1.05);
}

.card img {
  width: 100%;
  height: 260px;
  object-fit: cover;
  border-radius: 14px;
}

.names {
  margin-top: 10px;
  font-size: 14px;
}

.bias-btn {
  margin-top: 10px;
  padding: 8px 14px;
  border: none;
  border-radius: 20px;
  background: #ff2d2d;
  color: white;
  cursor: pointer;
}

.bias-btn.active {
  background: gold;
  color: black;
}

.section {
  margin-top: 40px;
}

textarea {
  width: 100%;
  height: 120px;
  border-radius: 12px;
  padding: 10px;
}

footer {
  text-align: center;
  padding: 20px;
  font-size: 12px;
  opacity: 0.6;
}

/* ===== Modal ===== */
#modal {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.85);
  z-index: 999;
}

.modal-box {
  max-width: 900px;
  margin: 40px auto;
  background: #111;
  padding: 20px;
  border-radius: 16px;
}

.modal-photos {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px,1fr));
  gap: 10px;
}

.modal-photos img {
  width: 100%;
  border-radius: 12px;
}

.close-btn {
  margin-top: 15px;
}
#player {
  width: 0;
  height: 0;
}
</style>
</head>

<body>

<header>Stray Kids ✦ Member Gallery</header>

<div class="container">

<div id="player"></div>

<h2>成員列表（點擊查看多張照片）</h2>
<div class="idols" id="idolList"></div>

<div class="section">
  <h2>📣 應援牆（永久保存）</h2>
  <textarea id="cheerText" placeholder="輸入你的應援話語…"></textarea>
</div>

</div>

<footer>Stray Kids Fan Page｜作業展示</footer>

<!-- ===== Modal ===== -->
<div id="modal">
  <div class="modal-box">
    <h2 id="modalTitle"></h2>
    <div class="modal-photos" id="modalPhotos"></div>
    <button class="close-btn" onclick="closeModal()">關閉</button>
  </div>
</div>

<script>
// ===== 成員資料（多照片）=====
const idols = [
  { id:"bangchan", cn:"方燦", en:"Bang Chan", kr:"방찬", photos:["images/bangchan/1.jpg","images/bangchan/2.jpg","images/bangchan/3.jpg"] },
  { id:"leeknow", cn:"李旻浩", en:"Lee Know", kr:"리노", photos:["images/leeknow/1.jpg","images/leeknow/2.jpg"] },
  { id:"changbin", cn:"徐彰彬", en:"Changbin", kr:"창빈", photos:["images/changbin/1.jpg"] },
  { id:"hyunjin", cn:"黃鉉辰", en:"Hyunjin", kr:"현진", photos:["images/hyunjin/1.jpg","images/hyunjin/2.jpg","images/hyunjin/3.jpg"] },
  { id:"han", cn:"韓知城", en:"HAN", kr:"한", photos:["images/han/1.jpg"] },
  { id:"felix", cn:"李龍馥", en:"Felix", kr:"필릭스", photos:["images/felix/1.jpg"] },
  { id:"seungmin", cn:"金昇玟", en:"Seungmin", kr:"승민", photos:["images/seungmin/1.jpg"] },
  { id:"in", cn:"梁精寅", en:"I.N", kr:"아이엔", photos:["images/in/1.jpg"] }
];

const idolList = document.getElementById("idolList");
let bias = JSON.parse(localStorage.getItem("bias")) || [];

idols.forEach(i => {
  const card = document.createElement("div");
  card.className = "card";

  const isBias = bias.includes(i.id);

  card.innerHTML = `
    <img src="${i.photos[0]}" alt="${i.en}">
    <div class="names">
      <div>${i.cn}</div>
      <div>${i.en}</div>
      <div>${i.kr}</div>
    </div>
    <button class="bias-btn ${isBias ? "active" : ""}">
      ${isBias ? "★ 我的本命" : "加入本命"}
    </button>
  `;

  card.querySelector("img").onclick = () => openModal(i);

  card.querySelector("button").onclick = (e) => {
    e.stopPropagation();
    bias = isBias ? bias.filter(b => b !== i.id) : [...bias, i.id];
    localStorage.setItem("bias", JSON.stringify(bias));
    location.reload();
  };

  idolList.appendChild(card);
});

// ===== Modal =====
function openModal(idol) {
  document.getElementById("modal").style.display = "block";
  document.getElementById("modalTitle").textContent =
    `${idol.cn} ${idol.en} (${idol.kr})`;
  const box = document.getElementById("modalPhotos");
  box.innerHTML = "";
  idol.photos.forEach(p => {
    const img = document.createElement("img");
    img.src = p;
    box.appendChild(img);
  });
}

function closeModal() {
  document.getElementById("modal").style.display = "none";
}

// ===== 應援牆 =====
const cheerText = document.getElementById("cheerText");
cheerText.value = localStorage.getItem("cheer") || "";
cheerText.addEventListener("input", () => {
  localStorage.setItem("cheer", cheerText.value);
});

// ===== YouTube 音樂 =====
const songs = ["3R7w5lR6eCc","TQTlCHxyuu8","a4jiotDjHe4","OvioeS1ZZ7o"];
function onYouTubeIframeAPIReady() {
  new YT.Player("player", {
    videoId: songs[Math.floor(Math.random() * songs.length)],
    playerVars: { autoplay: 1, controls: 0 }
  });
}
const tag = document.createElement("script");
tag.src = "https://www.youtube.com/iframe_api";
document.body.appendChild(tag);
</script>

</body>
</html>
