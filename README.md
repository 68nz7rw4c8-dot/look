[kuamaydio.index.html](https://github.com/user-attachments/files/24434235/kuamaydio.index.html)
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>缺憾的複調：在失去中重組的風景</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@300;500;700;900&family=Cinzel:wght@700;900&display=swap');

        :root {
            --bg-main: #0a0a0a;
            --accent-gold: #c5a67c;
            --accent-gold-dim: rgba(197, 166, 124, 0.2);
            --text-main: #d4d4d4;
            --shutter-door: #0d0d0d;
            --detail-space: #050505;
            /* 金萱體與備援字體族群 */
            --font-main: "jf-jinxuan", "Noto Serif TC", "Songti TC", serif;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; outline: none; }
        body {
            font-family: var(--font-main);
            background-color: var(--bg-main);
            color: var(--text-main);
            margin: 0; padding: 0;
            overflow-x: hidden;
            line-height: 1.8;
        }

        /* 侘寂質感濾鏡 */
        body::before {
            content: ""; position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background-image: url("image/16.jpg");
            opacity: 0.15; pointer-events: none; z-index: 9999;
        }

        .reveal { opacity: 0; transform: translateY(40px); transition: all 1.2s cubic-bezier(0.19, 1, 0.22, 1); }
        .reveal.active { opacity: 1; transform: translateY(0); }

        /* --- Header --- */
        header {
            height: 100vh;
            display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center;
            background: linear-gradient(rgba(0,0,0,0.85), var(--bg-main)),
                        url('https://images.unsplash.com/photo-1506744038136-46273834b3fb?q=80&w=1600') center/cover fixed;
        }
        header h1 {
            font-size: clamp(2.5rem, 8vw, 5.5rem);
            font-weight: 900; letter-spacing: 0.2em; margin: 0;
            background: linear-gradient(to right, #fff, var(--accent-gold));
            -webkit-background-clip: text; color: transparent;
        }
        header .subtitle {
            font-family: 'Cinzel', serif; font-weight: 700; color: var(--accent-gold); 
            letter-spacing: 0.5em; margin-top: 20px; font-size: clamp(1rem, 3vw, 1.4rem);
        }
        header .team-list { color: #555; letter-spacing: 0.3em; margin-top: 50px; font-size: 0.85rem; font-weight: 700; }

        /* --- 策展論述：認識論轉向 --- */
        .curatorial-statement {
            max-width: 900px; margin: 150px auto; padding: 0 40px; text-align: center;
        }
        .curatorial-statement h2 { color: var(--accent-gold); font-size: 2.8rem; margin-bottom: 40px; font-weight: 900; }
        .curatorial-statement p { color: #888; font-size: 1.1rem; text-align: justify; margin-bottom: 30px; line-height: 2.2; }

        /* --- 子題視覺區隔 --- */
        .zone-header {
            max-width: 1400px; margin: 0 auto; padding: 40px;
            border-bottom: 1px solid var(--accent-gold-dim);
        }
        .zone-header h3 { font-family: 'Cinzel', serif; font-size: 3rem; color: #fff; margin: 0; font-weight: 900; }
        .zone-header span { color: var(--accent-gold); letter-spacing: 5px; font-size: 1.1rem; display: block; margin-top: 10px; font-weight: 700; }

        /* --- 藝術家區塊 (雜誌化排版) --- */
        .zone-container { max-width: 1400px; margin: 0 auto; padding: 80px 40px 180px; }
        .artist-entry {
            display: flex; align-items: center; gap: 100px; margin-bottom: 250px;
            cursor: pointer; position: relative;
        }
        .artist-entry:nth-child(even) { flex-direction: row-reverse; }

        .image-container {
            flex: 1.5; height: 550px; overflow: hidden; position: relative;
            border: 1px solid var(--accent-gold-dim); background: #111;
        }
        .image-container img {
            width: 100%; height: 100%; object-fit: cover;
            filter: grayscale(100%) brightness(0.5); transition: 1s cubic-bezier(0.19, 1, 0.22, 1);
        }
        .artist-entry:hover .image-container img { filter: grayscale(10%) brightness(0.7); transform: scale(1.03); }

        .text-container { flex: 1; }
        .text-container h4 { font-size: clamp(2rem, 5vw, 3.5rem); color: #fff; margin: 0; font-weight: 900; }
        .text-container small { font-family: 'Cinzel', serif; display: block; font-size: 1.2rem; color: var(--accent-gold); margin-top: 10px; }
        .action-tag { display: inline-block; padding: 4px 15px; border: 1px solid var(--accent-gold-dim); color: var(--accent-gold); font-size: 0.75rem; margin-top: 30px; letter-spacing: 3px; font-weight: 900; }

        /* --- 沉浸式詳情頁動畫 --- */
        #gallery-shutter {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: 10000; display: none; background-color: var(--detail-space);
        }
        .shutter {
            position: fixed; top: 0; width: 50%; height: 100%;
            background: var(--shutter-door); z-index: 10001;
            transition: transform 1.2s cubic-bezier(0.8, 0, 0.2, 1);
        }
        #shutter-l { left: 0; border-right: 1px solid #1a1a1a; }
        #shutter-r { right: 0; border-left: 1px solid #1a1a1a; }
        .modal-active #shutter-l { transform: translateX(-100%); }
        .modal-active #shutter-r { transform: translateX(100%); }

        .modal-content-wrap {
            position: relative; z-index: 10002; height: 100vh; overflow-y: auto;
            opacity: 0; transition: opacity 0.6s 0.8s;
        }
        .modal-active .modal-content-wrap { opacity: 1; }

        .detail-flex-box {
            max-width: 1200px; margin: 100px auto; display: grid;
            grid-template-columns: 1fr 1.2fr; gap: 80px; padding: 60px;
        }
        .detail-image img { width: 100%; border: 1px solid #222; }
        .detail-info h2 { font-size: 4rem; color: var(--accent-gold); margin: 0; font-weight: 900; }
        .detail-info h5 { color: #fff; border-bottom: 1px solid var(--accent-gold-dim); padding-bottom: 10px; margin-top: 50px; font-size: 1.2rem; }
        .detail-info p { color: #bbb; text-align: justify; line-height: 2.2; font-size: 1.05rem; }

        .exit-trigger { position: fixed; top: 40px; right: 50px; color: var(--accent-gold); cursor: pointer; z-index: 10003; letter-spacing: 5px; font-weight: 900; }

        /* --- 隨機金句刮刮樂橫幅 --- */
        .scratch-banner-area {
            position: relative; width: 100%; min-height: 400px; background: #080808; 
            border-top: 1px solid var(--accent-gold-dim); overflow: hidden;
        }
        .quote-container {
            position: absolute; width: 100%; height: 100%;
            display: flex; justify-content: center; align-items: center; z-index: 1;
        }
        .random-quote {
            font-size: clamp(1.2rem, 3.5vw, 2.3rem); color: var(--accent-gold); 
            font-weight: 900; letter-spacing: 8px; text-align: center; padding: 0 60px;
        }
        #scratch-layer { position: absolute; top: 0; left: 0; z-index: 10; cursor: crosshair; touch-action: none; width: 100%; height: 100%; }

        footer { padding: 120px 0; text-align: center; color: #444; font-size: 0.9rem; letter-spacing: 5px; border-top: 1px solid #111; }

        @media (max-width: 1024px) {
            .artist-entry, .detail-flex-box { grid-template-columns: 1fr; display: block; }
            .image-container { height: 350px; margin-bottom: 40px; }
            .detail-image { margin-bottom: 50px; }
        }
    </style>
</head>
<body>

<header>
    <h1>缺憾的複調</h1>
    <div class="subtitle">POLYPHONY OF IMPERFECTION</div>
    <div class="team-list">組員：薛柏倫、賴妍安、顏柏芸 </div>
</header>

<section class="curatorial-statement reveal">
    <h2>策展理念</h2>
    <p>
        本展覽旨在挑戰將「失去」視為匱乏的傳統觀點，將其重新定位為一種感官的認識論轉向 。藝術家被迫解構既有的感知層級，進而發明出全新的美學範式 。這並非意在彰顯傷痛，而是揭示其作為催化劑的強大力量。
    </p>
    <p>
        透過六位跨越時空的創作者，我們雄辯地證明了「失去」並非生命的終點，而是「重塑希望」的真實起點 。這些作品共同譜寫的是一首關於人類精神力量的頌歌，展現他們在受困與遺失中透過創作重啟人生的勇氣 。
    </p>
</section>

<div class="zone-header reveal">
    <h3>I. THE FILTERED REALITY</h3>
    <span>子題一：濾鏡下的真實</span>
</div>
<div class="zone-container">
    <div class="artist-entry reveal" onclick="openGalleryDetail('arsham')">
        <div class="image-container"><img src="images/129733.jpg"></img></div>
        <div class="text-container">
            <h4>丹尼爾·阿爾軒 <small>Daniel Arsham</small></h4>
            <span class="action-tag">未來考古學美學</span>
        </div>
    </div>
    <div class="artist-entry reveal" onclick="openGalleryDetail('meryon')">
        <div class="image-container"><img src="images/14.jpg"></div>
        <div class="text-container">
            <h4>查爾斯·梅里翁 <small>Charles Méryon</small></h4>
            <span class="action-tag">黑白蝕刻之巔</span>
        </div>
    </div>
    <div class="artist-entry reveal" onclick="openGalleryDetail('chang')">
        <div class="image-container"><img src="images/1234567.jpg"></div>
        <div class="text-container">
            <h4>張子言 <small>Ernest Chang</small></h4>
            <span class="action-tag">批判性矛盾濾鏡</span>
        </div>
    </div>
</div>

<div class="zone-header reveal">
    <h3>II. TOUCH AND CODE</h3>
    <span>子題二：指尖與代碼的溫度</span>
</div>
<div class="zone-container">
    <div class="artist-entry reveal" onclick="openGalleryDetail('bramblitt')">
        <div class="image-container"><img src="images/6.jpg"></div>
        <div class="text-container">
            <h4>約翰·布蘭布利特 <small>John Bramblitt</small></h4>
            <span class="action-tag">觸覺繪畫法先鋒</span>
        </div>
    </div>
    <div class="artist-entry reveal" onclick="openGalleryDetail('lian')">
        <div class="image-container"><img src="images/15.jpg"></div>
        <div class="text-container">
            <h4>連志忠 <small>Lian Zhi-zhong</small></h4>
            <span class="action-tag">數位編碼代償美學</span>
        </div>
    </div>
</div>

<div class="zone-header reveal">
    <h3>III. ABSENT SPIRIT</h3>
    <span>子題三：缺席的肢體，在場的靈魂</span>
</div>
<div class="zone-container">
    <div class="artist-entry reveal" onclick="openGalleryDetail('hsieh')">
        <div class="image-container"><img src="images/11.jpg"></div>
        <div class="text-container">
            <h4>謝坤山 <small>Hsieh Kun-shan</small></h4>
            <span class="action-tag">意志力的具象化</span>
        </div>
    </div>
</div>

<section class="scratch-banner-area reveal">
    <div class="quote-container">
        <div id="quote-display-target" class="random-quote"></div>
    </div>
    <canvas id="scratch-layer"></canvas>
</section>

<div id="gallery-shutter">
    <div id="shutter-l" class="shutter"></div>
    <div id="shutter-r" class="shutter"></div>
    <div class="modal-content-wrap">
        <span class="exit-trigger" onclick="closeGalleryDetail()">EXIT ✕</span>
        <div id="detail-injection-target"></div>
    </div>
</div>

<footer><p>《缺憾的複調：在失去中重組的風景》策展企劃書網頁版 © 2025</p></footer>

<script>
    const exhibitionData = {
        'arsham': {
            name: "丹尼爾·阿爾軒", eng: "Daniel Arsham",
            bio: "天生患有嚴重色盲。此限制使其發展出標誌性的『未來考古學』美學，專注於灰白黑與極致材質處理。",
            analysis: "代表作《被侵蝕的寶可夢》。將流行文化符號轉化為千年後的毀損文物，證明感官侷限能淬鍊對物質與時間的深刻專注。",
            img: "images/12973-2020080260579125.jpg"

        },
        'meryon': {
            name: "查爾斯·梅里翁", eng: "Charles Méryon",
            bio: "19世紀法國版畫家。因色盲放棄油畫，專注蝕刻版畫，在單色世界中創造超越時代的深度。",
            analysis: "代表作《小橋》。因為『失去了色彩』，梅里翁將精力灌注於線條、光影與結構，為展覽提供堅實藝術史脈絡。",
            img: "images/2345.jpg"
        },
        'chang': {
            name: "張子言", eng: "Ernest Chang",
            bio: "香港當代藝術家，先天紅綠色盲者。他將色盲視為一種獨特的觀察視角。融合普普藝術與社會批判。",
            analysis: "代表作《Space Rich 01》。作品以跳躍色彩探討嚴肅社會議題，將生理限制轉化為批判性美學。",
            img: "images/12345.jpg"
        },
        'bramblitt': {
            name: "約翰·布蘭布利特", eng: "John Bramblitt",
            bio: "美國德州藝術家，30歲時完全失明。在絕望中開創了獨特的『觸覺繪畫法』，透過油彩黏稠度辨識顏色。",
            analysis: "代表作《音樂的靈魂》。每一筆色彩都是藝術家觸覺的延伸，展現觸覺如何成為飽滿且獨立的藝術語言。",
            img: "images/7.jpg"
        },
        'lian': {
            name: "連志忠", eng: "Lian Zhi-zhong",
            bio: "台灣身障藝術家，因病導致聽覺障礙。以驚人毅力背誦色彩編號，透過嚴謹數位編碼進行電腦繪圖。",
            analysis: "代表作《晴空下的城堡》。將失去感官以極致理性邏輯進行代償，展現數位邏輯美學的高度。",
            img: "images/8.jpg"
        },
        'hsieh': {
            name: "謝坤山", eng: "Hsieh Kun-shan",
            bio: "因意外失去雙手與右眼。他不屈服命運，練習用嘴咬筆作畫。其作品呈現歷經磨難後的豁達與溫柔。",
            analysis: "代表作《金秋》。只見生命豐盈靜好，以『在場的靈魂』填補『不在場的雙手』，展現強大韌性。",
            img: "images/9.jpg"
        }
    };

    const curatorialQuotes = [
        "「失去」並非生命的終點，而是「重塑希望」的起點。",
        "真正的希望，是在認清遺憾之後，依然選擇擁抱生命。",
        "感官的「失去」能篩除雜訊，專注於本質的表達。",
        "希望並非空洞的樂觀，而是透過創作重啟人生的勇氣。",
        "不看失去的部分，而是專注於「僅存」的可能性。"
    ];

    function openGalleryDetail(id) {
        const d = exhibitionData[id];
        document.getElementById('detail-injection-target').innerHTML = `
            <div class="detail-flex-box">
                <div class="detail-image"><img src="${d.img}"></div>
                <div class="detail-info">
                    <h2>${d.name}</h2>
                    <p style="font-family:'Cinzel',serif; font-weight:700; color:#666; margin-top:10px;">${d.eng}</p>
                    <h5>【 策展分析：生理轉譯 】</h5>
                    <p>${d.bio}</p>
                    <h5>【 代表作品介紹 】 </h5>
                    <p>${d.analysis}</p>
                </div>
            </div>
        `;
        const modal = document.getElementById('gallery-shutter');
        modal.style.display = 'block';
        setTimeout(() => { modal.classList.add('modal-active'); }, 50);
        document.body.style.overflow = 'hidden';
    }

    function closeGalleryDetail() {
        const modal = document.getElementById('gallery-shutter');
        modal.classList.remove('modal-active');
        setTimeout(() => { modal.style.display = 'none'; document.body.style.overflow = 'auto'; }, 1200);
    }

    const scrollObserver = new IntersectionObserver(entries => {
        entries.forEach(e => { if(e.isIntersecting) e.target.classList.add('active'); });
    }, { threshold: 0.1 });
    document.querySelectorAll('.reveal').forEach(el => scrollObserver.observe(el));

    // --- 強化版即時刮刮樂 ---
    const canvas = document.getElementById('scratch-layer');
    const ctx = canvas.getContext('2d', { alpha: false });
    const area = canvas.parentElement;
    const target = document.getElementById('quote-display-target');
    let isActive = false;

    function refreshScratch() {
        // 隨機切換金句 
        target.innerText = curatorialQuotes[Math.floor(Math.random() * curatorialQuotes.length)];
        canvas.width = area.offsetWidth;
        canvas.height = area.offsetHeight;
        
        ctx.fillStyle = '#1a1a1a';
        ctx.fillRect(0,0,canvas.width,canvas.height);
        
        // 渲染侘寂金色紋理
        for(let i=0; i<400; i++) {
            ctx.fillStyle = `rgba(197, 166, 124, ${Math.random()*0.15})`;
            ctx.fillRect(Math.random()*canvas.width, Math.random()*canvas.height, 1.5, 1.5);
        }
        
        ctx.fillStyle = '#c5a67c'; 
        ctx.font = 'bold 18px "Noto Serif TC"'; 
        ctx.textAlign = 'center'; 
        ctx.fillText('刮開「失去」，發現內在生命價值 ', canvas.width/2, canvas.height/2+10);
    }

    function getCursor(e) {
        const rect = canvas.getBoundingClientRect();
        const cx = e.clientX || (e.touches && e.touches[0].clientX);
        const cy = e.clientY || (e.touches && e.touches[0].clientY);
        return { x: cx - rect.left, y: cy - rect.top };
    }

    function paint(e) {
        if(!isActive) return;
        const p = getCursor(e);
        ctx.globalCompositeOperation = 'destination-out';
        ctx.beginPath();
        ctx.arc(p.x, p.y, 45, 0, Math.PI * 2);
        ctx.fill();
    }

    canvas.addEventListener('mousedown', (e) => { isActive = true; paint(e); });
    window.addEventListener('mousemove', (e) => { if(isActive) requestAnimationFrame(() => paint(e)); });
    window.addEventListener('mouseup', () => isActive = false);

    canvas.addEventListener('touchstart', (e) => { isActive = true; paint(e); e.preventDefault(); }, {passive: false});
    canvas.addEventListener('touchmove', (e) => { if(isActive) requestAnimationFrame(() => paint(e)); e.preventDefault(); }, {passive: false});
    canvas.addEventListener('touchend', () => isActive = false);

    window.addEventListener('load', () => setTimeout(refreshScratch, 500));
    window.addEventListener('resize', refreshScratch);
</script>
</body>
</html>
