<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>線上塔羅抽牌</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Jost:wght@400;500&family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
<style>
/* VOICE: quiet, celestial, minimal — following the user's reference screenshots
   IDEA: a night sky; pick a question area, a spread, then draw from a numbered 6×13 deck
   PICTURE: moon emblem header, live spread-shape preview, then the laid-out Rider-Waite cards
   COLOUR and TYPE: ink #1a1926, champagne gold #e3d0a2, thin gold rules; Jost spaced caps + Noto Sans TC
   LAYOUT: one centered column up to 760px, sticky bottom bar with the two buttons */
:root{color-scheme:dark;--bg:#1a1926;--panel:#1f1e2c;--card:#232233;--gold:#e3d0a2;--gold-d:#b9a477;--line:rgba(227,208,162,.32);--soft:rgba(255,255,255,.22);--ink:#ece7da;--mute:#a7a2b3;--rev:#e59a8a;--up:#a9d4b8}
*{box-sizing:border-box;min-width:0}
body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.65 "Noto Sans TC",system-ui,sans-serif;overflow-x:clip;padding-bottom:120px}
.wrap{max-width:760px;margin:0 auto;padding:0 max(4vw,16px)}
header{position:relative;text-align:center;padding:40px 0 30px;overflow:hidden;background:linear-gradient(rgba(42,41,56,.85),rgba(26,25,38,0) 120px)}
.box,.ovw,.stats{background:rgba(26,25,38,.78)}
#stars{position:fixed;inset:0;width:100%;height:100%;z-index:-1;pointer-events:none}
.mbtn{display:inline-flex;align-items:center;gap:8px;min-height:44px;padding:0 16px;border-radius:999px;border:1px solid var(--line);background:rgba(26,25,38,.7);color:var(--mute);font:500 14px "Noto Sans TC",sans-serif;cursor:pointer;backdrop-filter:blur(6px);transition:color .2s,border-color .2s}
.mbtn svg{width:18px;height:18px}
.mbtn.on{color:var(--gold);border-color:var(--gold-d)}
.mbtn.on svg{animation:pulse 2.4s ease-in-out infinite}
@keyframes pulse{50%{transform:scale(1.15)}}
.mbtn:focus-visible{outline:2px solid var(--gold);outline-offset:2px}
.mbtn.float{position:fixed;top:14px;right:14px;z-index:30}
body.reading .mbtn.float{display:none}
.topnav .right{display:flex;gap:10px;align-items:center}
@media(max-width:600px){.mbtn span{display:none}.mbtn{padding:0 13px}.topnav .btn{padding:0 18px}}
@media(prefers-reduced-motion:reduce){.mbtn.on svg{animation:none}}
#sky{position:absolute;inset:0;width:100%;height:100%;pointer-events:none}
.emblem{position:relative;width:clamp(150px,30vw,210px);display:block;margin:0 auto}
.brand{position:relative;font:400 clamp(24px,4.4vw,34px)/1.2 Jost,sans-serif;letter-spacing:.42em;padding-left:.42em;color:var(--gold);margin:18px 0 26px}
h1{position:relative;font-size:clamp(28px,5vw,40px);line-height:1.15;color:var(--gold);margin:0 0 14px;font-weight:700}
.tag{position:relative;margin:0;font-size:17px}
hr{border:0;border-top:1px solid var(--line);margin:0 0 28px}
.lbl{display:block;font-size:17px;font-weight:500;margin:26px 0 12px}
.hint{color:var(--mute);margin:-4px 0 16px;font-size:15px}
/* controls */
.dd{position:relative}
.dd select{appearance:none;width:100%;min-height:56px;border-radius:999px;border:1px solid var(--soft);background:var(--bg);color:var(--ink);font:16px "Noto Sans TC",sans-serif;padding:0 56px 0 26px;cursor:pointer}
.dd::after{content:"";position:absolute;right:28px;top:50%;width:10px;height:10px;border:solid var(--ink);border-width:0 1.6px 1.6px 0;transform:translateY(-70%) rotate(45deg);pointer-events:none}
.dd select:focus-visible,textarea:focus-visible{outline:2px solid var(--gold);outline-offset:2px}
.radios{display:grid;gap:6px}
.radio{display:flex;align-items:center;gap:16px;min-height:52px;cursor:pointer;font-size:clamp(17px,3vw,20px)}
.radio input{appearance:none;flex:none;width:34px;height:34px;border-radius:50%;border:1.5px solid var(--soft);margin:0;cursor:pointer;display:grid;place-items:center}
.radio input:checked{background:#f3f0ea;border-color:#f3f0ea}
.radio input:checked::after{content:"";width:10px;height:10px;border-radius:50%;background:var(--bg)}
.radio input:focus-visible{outline:2px solid var(--gold);outline-offset:3px}
.radio small{color:var(--mute);font-size:.85em}
.box{border:1px solid var(--gold-d);border-radius:14px;padding:16px 18px 20px;margin-top:22px}
.box h3{margin:0 0 4px;color:var(--gold);font-size:21px;font-weight:700}
.box p{margin:0 0 12px;color:var(--mute);font-size:14px}
.stage{position:relative;width:100%}
.mini{position:absolute;border:1px solid var(--gold-d);border-radius:8px;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:4px;background:var(--bg)}
.mini b{font:500 clamp(15px,3vw,20px)/1.2 Jost,sans-serif;color:var(--gold)}
.mini span{font-size:12px;line-height:1.3;margin-top:4px}
.mini.cross{transform:rotate(90deg);background:rgba(26,25,38,.92)}
textarea{width:100%;min-height:110px;border-radius:14px;border:1px solid var(--soft);background:var(--bg);color:var(--ink);font:16px/1.6 "Noto Sans TC",sans-serif;padding:16px 26px;resize:vertical}
textarea::placeholder{color:#8f8a9b}
.check{display:flex;align-items:center;gap:16px;margin-top:22px;cursor:pointer;font-size:clamp(17px,3vw,20px);min-height:44px}
.check input{appearance:none;flex:none;width:34px;height:34px;border-radius:50%;border:1.5px solid var(--soft);margin:0;cursor:pointer;display:grid;place-items:center}
.check input:checked{background:#f3f0ea;border-color:#f3f0ea}
.check input:checked::after{content:"";width:12px;height:7px;border:solid var(--bg);border-width:0 0 2.5px 2.5px;transform:translateY(-2px) rotate(-45deg)}
.check input:focus-visible{outline:2px solid var(--gold);outline-offset:3px}
/* deck grid */
.deckwrap{max-width:1120px;margin:0 auto;padding:0 max(3vw,10px)}
.deck{display:grid;grid-template-columns:repeat(13,minmax(0,1fr));gap:clamp(3px,.9vw,10px)}
.back{position:relative;aspect-ratio:7/11;border-radius:8px;border:1px solid var(--line);background:var(--card);color:var(--gold);cursor:pointer;padding:0;font:inherit;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:4%;transition:transform .2s,border-color .2s,background .2s,box-shadow .2s}
.back::before{content:"";position:absolute;inset:4px;border:1px solid rgba(227,208,162,.18);border-radius:5px}
.back svg{width:42%;opacity:.8}
.back .no{font:500 clamp(10px,1.5vw,15px)/1 Jost,sans-serif;letter-spacing:.06em}
.back:hover{border-color:var(--gold-d);transform:translateY(-3px)}
.back:focus-visible{outline:2px solid var(--gold);outline-offset:2px}
.back.sel{background:var(--gold);color:var(--bg);border-color:var(--gold);box-shadow:0 0 18px rgba(227,208,162,.35)}
.back.sel::before{border-color:rgba(26,25,38,.35)}
.back .ord{position:absolute;top:-8px;right:-6px;min-width:22px;height:22px;border-radius:11px;background:var(--bg);color:var(--gold);border:1px solid var(--gold);font:500 12px/20px Jost,sans-serif;text-align:center;padding:0 5px}
.deck.locked .back{cursor:default}.deck.locked .back:hover{transform:none}
.deck.shuffling .back{animation:shuf .55s ease}
@keyframes shuf{45%{transform:translate(var(--dx),var(--dy)) rotate(var(--dr));opacity:.5}}
/* bottom bar */
.bar{transition:opacity .45s ease;position:fixed;left:0;right:0;bottom:0;z-index:20;background:rgba(26,25,38,.92);backdrop-filter:blur(8px);border-top:1px solid var(--line);padding:12px max(4vw,16px) calc(12px + env(safe-area-inset-bottom))}
.bar .in{max-width:760px;margin:0 auto;display:flex;gap:12px;justify-content:center}
.status{flex:1 1 200px;color:var(--mute);font-size:15px}
.status b{color:var(--gold);font-weight:500}
.btn{min-height:46px;padding:0 24px;border-radius:999px;font:500 16px "Noto Sans TC",sans-serif;cursor:pointer;border:1px solid var(--gold);transition:opacity .2s,transform .1s}
.btn:active{transform:scale(.97)}
.btn:focus-visible{outline:2px solid var(--gold);outline-offset:3px}
.primary{background:var(--gold);color:var(--bg)}
.ghost{background:transparent;color:var(--gold);border-color:var(--line)}
.btn:disabled{opacity:.4;cursor:not-allowed}
/* result */
#result{display:none;padding-top:40px}#result.on{display:block}
.rhead{text-align:center}
.rhead h2{color:var(--gold);font-size:28px;font-weight:700;margin:0 0 4px}
.q{max-width:560px;margin:10px auto 0;padding:12px 18px;border:1px solid var(--line);border-radius:12px;color:var(--ink);font-size:15px;text-align:left}
.q:empty{display:none}
#spread{margin:28px 0 8px}
.slot{position:absolute;display:flex;flex-direction:column;align-items:center;text-align:center}
.pos{font-size:12px;color:var(--mute);line-height:1.3;height:32px;display:flex;align-items:flex-end;justify-content:center;overflow:hidden}
.ori{font-weight:700;font-size:14px;letter-spacing:.12em;line-height:20px;height:22px;white-space:nowrap}
.ori .up{color:var(--up)}.ori .rv{color:var(--rev)}
.c3{perspective:900px}
.flip{position:relative;width:100%;height:100%;transform-style:preserve-3d;transition:transform .8s cubic-bezier(.3,.7,.2,1);transform:rotateY(180deg)}
.flip.shown{transform:rotateY(0)}
.flip>div{position:absolute;inset:0;backface-visibility:hidden;border-radius:6px;overflow:hidden;box-shadow:0 8px 22px rgba(0,0,0,.5)}
.cb{transform:rotateY(180deg);background:var(--card);border:1px solid var(--gold-d);display:grid;place-items:center;color:var(--gold)}
.cb svg{width:46%}
.face img{width:100%;height:100%;object-fit:cover;display:block}
.slot.cross{z-index:2}
.slot.cross .c3{transform:rotate(90deg) scale(.82)}
.list{margin-top:30px;border-top:1px solid var(--line)}
.list div{display:flex;gap:12px;padding:12px 2px;border-bottom:1px solid var(--line);font-size:15px;flex-wrap:wrap}
.list b{color:var(--gold);font-weight:500;flex:0 0 8.5em}
.stats{display:grid;gap:8px;border:1px solid var(--line);border-radius:14px;padding:16px 18px}
.st{display:grid;grid-template-columns:6.5em minmax(0,1fr) 1.6em 7.5em;gap:10px;align-items:center;font-size:14px}
.st .tr{height:8px;border-radius:4px;background:rgba(255,255,255,.08);overflow:hidden}
.st .tr i{display:block;height:100%;background:var(--gold);border-radius:4px}
.st b{color:var(--gold);font:500 15px Jost,sans-serif;text-align:right}
.st small{color:var(--mute);font-size:13px}
.ovw b{color:var(--gold);font-weight:700}
.rd dl{margin:10px 0 0;display:grid;gap:2px}
.rd dt{font-size:13px;color:var(--gold);margin-top:8px}
.rd dd{margin:0;font-size:15px}
@media(max-width:600px){.st{grid-template-columns:5.5em minmax(0,1fr) 1.4em}.st small{display:none}.rd{grid-template-columns:56px minmax(0,1fr);gap:12px}.rd img{width:56px}}
#drawView,#readView{transition:opacity .45s ease}.fading{opacity:0}
@media(prefers-reduced-motion:reduce){#drawView,#readView{transition:none}}
body.reading{padding-bottom:40px}body.reading .bar{display:none}
.topnav{position:sticky;top:0;z-index:20;background:rgba(26,25,38,.92);backdrop-filter:blur(8px);border-bottom:1px solid var(--line)}
.topnav .in{max-width:960px;margin:0 auto;padding:10px max(4vw,16px);display:flex;align-items:center;justify-content:space-between;gap:12px}
.topnav .btn{min-height:44px;padding:0 22px}
.link{background:none;border:0;color:var(--gold);font:500 15px "Noto Sans TC",sans-serif;cursor:pointer;min-height:44px;padding:0 4px}
.link:focus-visible{outline:2px solid var(--gold);outline-offset:2px}
.shot{position:relative;max-width:960px;margin:10px auto 0;padding:24px clamp(14px,3vw,40px) 26px;text-align:center}

.sbrand{margin:0;font:400 14px Jost,sans-serif;letter-spacing:.42em;padding-left:.42em;color:var(--gold-d)}
.shot h1{font-size:clamp(26px,4.6vw,44px);line-height:1.25;text-wrap:balance;margin:14px auto 10px;max-width:22em;overflow-wrap:anywhere}
.meta{margin:0;color:var(--mute);font-size:15px}
.meta b{color:var(--gold);font-weight:500}
.shot #spread{margin:26px 0 6px}
.foot{margin:6px 0 0;font:400 12px Jost,"Noto Sans TC",sans-serif;letter-spacing:.12em;color:var(--gold-d)}
#readView .wrap{padding-top:10px}
.rd2{padding:26px 0 30px;border-bottom:1px solid var(--line)}
.rd2 h4{font-size:clamp(20px,3.4vw,24px);font-weight:700;margin:0 0 16px;color:var(--ink)}
.rtop{display:grid;grid-template-columns:clamp(110px,24vw,150px) minmax(0,1fr);gap:clamp(14px,3vw,26px);align-items:start;margin-bottom:14px}
.rtop img{width:100%;border-radius:10px;display:block;box-shadow:0 6px 18px rgba(0,0,0,.4)}
.info p{margin:0 0 8px;font-size:15px}
.info .cn{font-size:19px;font-weight:700;color:var(--gold)}
.info .cn span{font:500 16px Jost,sans-serif;margin-left:8px;color:var(--ink)}
.info b{color:var(--gold);font-weight:500}
.info .dim{opacity:.55}
.rd2>p{margin:12px 0;font-size:16px}
.rd2 h5{font-size:19px;font-weight:700;margin:26px 0 6px;color:var(--gold)}
.t4{width:100%;border-collapse:collapse;margin:10px 0 6px;font-size:15px}
.t4 th{text-align:left;font-weight:700;color:var(--gold);padding:10px 8px 10px 0;border-bottom:1px solid var(--line)}
.t4 td{padding:12px 8px 12px 0;border-bottom:1px solid rgba(227,208,162,.14);vertical-align:top}
.t4 td:first-child{width:9em;color:var(--ink);font-weight:500}
@media(max-width:600px){.t4 td:first-child{width:6.5em}.info p{font-size:14px}.info .cn{font-size:17px}}
.rt{color:var(--gold);font-size:22px;font-weight:700;margin:38px 0 14px}
.ovw{border:1px solid var(--gold-d);border-radius:14px;padding:6px 20px}
.ovw p{margin:12px 0}
.adv{background:rgba(227,208,162,.06)}
.rd{display:grid;grid-template-columns:72px minmax(0,1fr);gap:16px;padding:16px 0;border-bottom:1px solid var(--line)}
.rd img{width:72px;border-radius:5px;display:block}
.rd h4{margin:0;font-size:18px;font-weight:700;display:flex;flex-wrap:wrap;align-items:baseline;gap:4px 10px}
.rd h4 span{flex-basis:100%;font-size:13px;font-weight:500;color:var(--gold)}
.rd em{font-style:normal;font-size:13px;letter-spacing:.1em}
.rd em.up{color:var(--up)}.rd em.rv{color:var(--rev)}
.rd p{margin:6px 0 0;font-size:15px}
.kw{display:flex;flex-wrap:wrap;gap:6px}
.kw i{font-style:normal;font-size:13px;border:1px solid var(--line);border-radius:999px;padding:1px 10px;color:var(--gold)}
.note{font-size:13px;color:var(--mute);margin-top:18px}
@media(max-width:700px){.back svg{display:none}.back::before{display:none}.back{border-radius:5px}}
@media(max-width:600px){.bar .btn{flex:1;max-width:200px;padding:0 12px}.back .ord{min-width:16px;height:16px;font-size:10px;line-height:14px;top:-7px;right:-5px;padding:0 3px}.mini span{font-size:11px}}
@media(prefers-reduced-motion:reduce){.flip,.back{transition:none}.deck.shuffling .back{animation:none}}
/* ---- responsive: tablet & desktop ---- */
@media(min-width:1100px){
  .wide{max-width:1120px}
  .setup{display:grid;grid-template-columns:minmax(0,1fr) minmax(0,1fr);grid-template-areas:"a b" "c b";column-gap:56px;align-items:start}
  .colA{grid-area:a}.colC{grid-area:c}
  .colB{grid-area:b;position:sticky;top:24px;padding-top:26px}
  .colB .box{margin-top:12px}
  .hint{margin-bottom:20px}
}
@media(min-width:1440px){
  body{font-size:17px}
  .wide{max-width:1240px}
  .deckwrap{max-width:1280px}
  #readView .wrap{max-width:880px}
  .lbl{font-size:18px}
  .radio,.check{font-size:20px}
  .emblem{width:220px}
  .tag{font-size:18px}
  .rd2>p,.ovw p{font-size:17px}
  .t4,.info p{font-size:16px}
}
@media(min-width:1800px){
  .wide{max-width:1360px}
  .deckwrap{max-width:1440px}
  #readView .wrap{max-width:960px}
  .shot{max-width:1200px}
  .shot h1{font-size:52px}
}
@media(max-width:380px){
  .radio,.check{font-size:16px;gap:12px}
  .radio input,.check input{width:30px;height:30px}
  .dd select{padding-left:20px}
  textarea{padding:14px 18px}
  .box{padding:14px 12px 16px}
}
</style>
</head>
<body>
<canvas id="stars" aria-hidden="true"></canvas>
<button class="mbtn float" aria-pressed="false" aria-label="背景音樂開關"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M9 18V5l11-2v13" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/><circle cx="6.5" cy="18" r="2.6" fill="currentColor"/><circle cx="17.5" cy="16" r="2.6" fill="currentColor"/></svg><span>音樂 關</span></button>
<div id="drawView">
<header>
  <svg class="emblem" viewBox="0 0 200 200" aria-hidden="true">
    <circle cx="100" cy="100" r="88" fill="none" stroke="#e3d0a2" stroke-width="2.5" stroke-dasharray="7 9" opacity=".75"/>
    <circle cx="100" cy="100" r="76" fill="none" stroke="#e3d0a2" stroke-width="1.2" opacity=".55"/>
    <path d="M128 44a60 60 0 1 0 32 92a52 52 0 1 1-32-92z" fill="#e3d0a2"/>
    <path d="M152 46l5 18 18 5-18 5-5 18-5-18-18-5 18-5z" fill="#e3d0a2"/>
    <circle cx="132" cy="80" r="3" fill="#e3d0a2" opacity=".6"/><circle cx="112" cy="94" r="1.6" fill="#e3d0a2" opacity=".6"/>
  </svg>
  <p class="brand">TAROT</p>
  <h1>線上塔羅抽牌</h1>
  <p class="tag">靜下心來，想著你想問的事。</p>
</header>

<main class="wrap wide">
  <hr>
  <div class="setup"><div class="colA">
  <label class="lbl" for="cat">你想詢問哪個領域？</label>
  <div class="dd"><select id="cat"></select></div>

  <div id="pickWrap">
    <span class="lbl" id="pickLbl">選擇牌陣</span>
    <div class="radios" id="spreads" role="radiogroup" aria-labelledby="pickLbl"></div>
  </div>
  </div>
  <div class="colB"><div class="box">
    <h3>牌陣位置預覽</h3>
    <p id="pvSub"></p>
    <div class="stage" id="preview"></div>
  </div></div>

  <div class="colC"><label class="lbl" for="q">你想詢問的問題</label>
  <textarea id="q" placeholder="寫下此刻想探索的事情……"></textarea>

  <label class="check"><input type="checkbox" id="useRev" checked> 使用正逆位（取消則全部使用正位）</label></div>
  </div>

  <hr style="margin-top:34px">
  <span class="lbl" id="drawTitle"></span>
  <p class="hint" id="drawHint">點選牌卡即可選取，再點一次取消。</p>
</main>
<div class="deckwrap"><div class="deck" id="deck" role="group" aria-label="78 張塔羅牌"></div></div>
</div>

<div id="readView" hidden>
  <div class="topnav"><div class="in"><button class="link" id="back">← 返回抽牌頁</button><div class="right"><button class="mbtn" aria-pressed="false" aria-label="背景音樂開關"><svg viewBox="0 0 24 24" aria-hidden="true"><path d="M9 18V5l11-2v13" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/><circle cx="6.5" cy="18" r="2.6" fill="currentColor"/><circle cx="17.5" cy="16" r="2.6" fill="currentColor"/></svg><span>音樂 關</span></button><button class="btn primary" id="again">重新占卜</button></div></div></div>
  <section class="shot" id="shot" aria-live="polite">
    <p class="sbrand">TAROT READING</p>
    <h1 id="rQ"></h1>
    <div class="stage" id="spread"></div>
    <p class="foot" id="rFoot"></p>
  </section>
  <main class="wrap">
    <div id="reading"></div>
    <p class="note">牌面為 1909 年偉特（Rider-Waite-Smith）原版塔羅，已屬公有領域。</p>
  </main>
</div>

<div class="bar"><div class="in">
  <button class="btn primary" id="reveal">揭開塔羅牌</button>
  <button class="btn ghost" id="shuffle">重新洗牌</button>
</div></div>

<script>
{
const MOON='<svg viewBox="0 0 40 40" aria-hidden="true"><path d="M24 6a14 14 0 1 0 10 22A12 12 0 1 1 24 6z" fill="currentColor"/><path d="M31 7l1.2 4 4 1.2-4 1.2L31 17.4l-1.2-4-4-1.2 4-1.2z" fill="currentColor"/></svg>';

/* ---- 78 cards ---- */
const MAJ=["愚者","魔術師","女祭司","皇后","皇帝","教皇","戀人","戰車","力量","隱者","命運之輪","正義","吊人","死神","節制","惡魔","高塔","星星","月亮","太陽","審判","世界"];
const SUITS=[["權杖","w"],["聖杯","c"],["寶劍","s"],["錢幣","p"]];
const RANK=["王牌","二","三","四","五","六","七","八","九","十","侍者","騎士","皇后","國王"];
const pad=n=>String(n).padStart(2,"0");
const CARDS=[];
MAJ.forEach((m,i)=>CARDS.push({name:m,img:`cards/m${pad(i)}.jpg`}));
SUITS.forEach(s=>RANK.forEach((r,ri)=>CARDS.push({name:s[0]+r,img:`cards/${s[1]}${pad(ri+1)}.jpg`})));

/* ---- 牌義：正位關鍵字|正位解讀|逆位關鍵字|逆位解讀（之後可移到後台管理） ---- */
const MEAN=`新開始、冒險、自由|帶著純真與勇氣踏出第一步，不必想得太周全，機會就藏在嘗試之中。|魯莽、猶豫、不切實際|太衝動或太害怕都會錯失時機，先看清腳下再出發。
創造、行動、資源具足|你已擁有需要的工具與能力，現在是主動出擊、把想法化為現實的時候。|能力未發揮、分心、欺瞞|才能被浪費或方向分散，也要留意他人的話術或自我欺騙。
直覺、內在智慧、靜待|答案藏在你心裡，放慢腳步，傾聽直覺與潛意識的聲音。|忽略直覺、隱藏的事、封閉|你可能忽略了內心的訊號，或有些事還沒浮上檯面。
豐盛、滋養、創造力|生活進入豐收期，以溫柔和耐心照顧自己與他人，成果自然開花。|付出失衡、停滯、忽略自己|照顧別人太多或創造力受阻，記得先好好照顧自己。
穩定、權威、秩序|以理性和紀律建立結構，掌握主導權，事情會朝穩固的方向發展。|控制欲、僵化、失去主導|太強勢或規則太死，反而失去彈性與人心。
傳統、指引、信念|遵循既有制度或請教前輩，可靠的指引能幫助你。|打破常規、教條、不信任|你想跳脫框架，或發現既有規範已不適合自己。
愛、選擇、價值一致|一段和諧的連結或重要抉擇，依照內心真正重視的價值去選。|不和諧、選擇困難、價值衝突|關係失衡或在選擇中搖擺，需要誠實面對自己想要什麼。
意志、勝利、前進|憑著決心與自律克服阻礙，掌握方向就能一路向前。|失控、方向混亂、受阻|力量分散或急於求成，先穩住自己再出發。
勇氣、溫柔的堅定、自制|以耐心和柔軟駕馭困難，真正的力量來自內在的平靜。|自我懷疑、情緒失控、軟弱|信心不足或被情緒牽著走，需要重新找回內在力量。
內省、獨處、尋找答案|暫時退一步，在安靜中思考，你會找到屬於自己的答案。|孤立、逃避、迷失|過度封閉或拒絕面對，適時向外求助會更好。
轉機、循環、好運|局勢正在轉動，順勢而為，把握突如其來的機會。|時運不濟、抗拒改變、停滯|暫時處於低谷，接受變化並耐心等待下一個轉折。
公平、因果、理性判斷|事情會依應有的道理發展，誠實面對並做出公正的決定。|不公、逃避責任、偏見|判斷可能失衡，或有尚未負起的責任需要正視。
暫停、換個角度、犧牲|停下來換個視角看事情，暫時的等待會帶來新的領悟。|拖延、無謂的犧牲、僵持|卡住不動卻不願放手，是時候做出改變了。
結束、轉化、新生|一個階段正在結束，放下過去，才能迎接新的開始。|抗拒改變、停滯、緩慢轉變|緊抓不放只會延長痛苦，接受結束才能前進。
平衡、調和、耐心|找到中庸之道，循序漸進地調和各方，事情會慢慢變好。|失衡、過度、急躁|生活或關係失去平衡，需要重新調整步調。
束縛、慾望、執著|你可能被慾望、習慣或關係綁住，看清束縛才有機會掙脫。|解脫、覺醒、擺脫控制|你開始意識到困住自己的東西，正一步步重獲自由。
突變、崩解、真相揭露|突如其來的改變打破舊有結構，雖然震撼，卻能清出新的空間。|延遲的改變、內在動盪|危機被暫時壓下，但根本問題仍需要處理。
希望、療癒、信心|風雨過後出現曙光，保持信念，你正走在療癒與實現的路上。|失望、缺乏信心、迷惘|暫時看不到希望，試著重新連結讓你安心的人事物。
不安、幻象、潛意識|事情還不明朗，留意恐懼與錯覺，相信直覺但別急著下定論。|迷霧散去、真相浮現|混亂漸漸清晰，你開始看清真相、釋放恐懼。
成功、喜悅、活力|一切明朗而溫暖，自信地展現自己，好事正在發生。|短暫低落、延遲的成功|喜悅仍在，只是需要多一點時間或更務實的心態。
覺醒、重生、召喚|是時候回顧過去、做出關鍵決定，回應新階段的召喚。|自我批判、猶豫、錯過時機|對過去放不下，或不敢回應內心的呼喚。
完成、圓滿、整合|一個循環圓滿落幕，你收穫成果，也準備好迎接下一段旅程。|未完成、缺少收尾|離終點只差一步，找出遺漏的部分把它完成。
新的熱情、靈感、開始|一股新的動力出現，勇敢點燃它並開始行動。|動力不足、延遲、方向不明|熱情被澆熄或時機未到，先找回動機。
規劃、遠見、抉擇|站在起點眺望未來，做好規劃就能踏出下一步。|害怕改變、規劃不足|猶豫不前，或計畫還不夠周全。
拓展、等待成果、遠景|前期的努力開始有回應，把眼光放遠，機會正在靠近。|延誤、受挫、視野受限|進展不如預期，需要調整期待與策略。
慶祝、穩定、歸屬|值得慶祝的階段性成果，享受與家人朋友的和樂。|不穩定、缺乏歸屬、過渡期|環境或關係還沒安定下來。
競爭、衝突、磨合|意見分歧或良性競爭，把摩擦當成成長的機會。|內耗、壓抑不滿、和解|衝突漸緩，但也要留意壓抑情緒造成的內耗。
勝利、肯定、好消息|你的努力被看見，獲得認可與好消息。|自負、期待落空|期待的掌聲沒有到來，別讓得失心影響你。
堅守立場、勇氣、防衛|面對挑戰時堅持自己的立場，你有能力守住。|疲於應付、被壓倒|壓力過大，需要決定哪些事值得堅持。
迅速、進展、消息|事情快速推進，好消息或行動接連到來。|延遲、混亂、操之過急|進度卡住或太急躁，放慢反而更順。
堅持、韌性、最後防線|雖然疲憊，但只差一點，撐住就能通過考驗。|筋疲力盡、防衛過度|長期緊繃讓你耗盡，需要休息與支援。
重擔、責任、壓力|你扛著太多責任，學著分擔或放下不必要的負擔。|卸下重擔、過勞|你終於意識到要放手，或壓力已到極限。
好奇、新消息、探索|帶著熱情嘗試新事物，令人興奮的消息即將出現。|三分鐘熱度、不成熟|熱情來得快去得也快，需要持之以恆。
冒險、衝勁、行動派|充滿幹勁地向前衝，勇於冒險會帶來突破。|衝動、魯莽、半途而廢|行動太急而缺乏計畫。
自信、熱情、魅力|以溫暖與自信影響身邊的人，大方展現你的光芒。|嫉妒、缺乏自信、強勢|不安感讓你變得防衛或強勢。
領導、遠見、開創|以宏觀的視野帶領方向，你有能力開創局面。|專斷、急躁、期望過高|過於強勢或衝動的決策可能帶來反效果。
新感情、情感流動、直覺|一段新的情感或喜悅湧現，敞開心接受愛。|情感壓抑、空虛|心門緊閉，或感到情感匱乏。
連結、伴侶、互相吸引|雙方心意相通，是一段平等而美好的關係。|失和、誤解、失衡|關係出現隔閡，需要真誠溝通。
友誼、慶祝、團聚|與朋友共享喜悅，人際關係帶來支持與歡樂。|過度玩樂、疏離|社交帶來負擔，或關係產生裂痕。
倦怠、冷漠、沉思|對眼前的事提不起勁，可能忽略了身邊的新機會。|重新振作、接受機會|走出低潮，開始願意接受新事物。
失落、悲傷、遺憾|為失去而難過，但別忘了身後還有留下的事物。|釋懷、走出傷痛|逐漸放下過去，重新站起來。
回憶、純真、舊情|過去的美好回憶或舊識重現，帶來溫暖。|沉溺過去、無法前進|太依戀過去，反而看不見現在。
幻想、選擇太多、迷惑|眼前選項看似誘人，需要分辨哪些是真實可行的。|看清現實、做出決定|幻象消散，你知道自己真正要什麼。
離開、追尋、放下|主動離開不再滿足你的事物，去尋找更深的意義。|害怕離開、徘徊|想走又不敢走，或用逃避代替面對。
願望實現、滿足、享受|心願成真的牌，好好享受此刻的滿足。|不滿足、期待落空|得到了卻仍覺得空虛，想想你真正需要什麼。
幸福、家庭、圓滿|情感上的圓滿與和諧，家庭與關係帶來幸福。|家庭失和、理想破滅|關係中的和諧被打破，需要修補。
浪漫、直覺、好消息|帶來感情或創意上的好消息，用柔軟的心去感受。|情緒化、不成熟|情緒起伏大，或感情訊息不如預期。
浪漫、追求、邀約|有人帶著真心靠近，或你正追隨內心的理想。|不切實際、反覆|承諾不可靠，或行動過於情緒化。
溫柔、同理、直覺|以體貼與包容照顧他人，也別忘了照顧自己的情緒。|情緒依賴、過度敏感|情緒界線模糊，容易被他人影響。
情緒成熟、包容、穩重|以平穩的情緒面對事情，理性與感性兼具。|情緒操控、壓抑、冷漠|表面平靜，內心卻波濤洶湧。
清晰、真相、突破|思緒豁然開朗，用理性和誠實切開迷霧。|混亂、誤判、言語傷人|想法不清或溝通出錯，說話前多想一下。
僵局、猶豫、逃避抉擇|你在兩難中選擇不看，但遲早需要面對。|焦慮、僵局鬆動|僵局開始鬆動，但焦慮也可能讓你更難決定。
心碎、悲傷、痛苦真相|一段讓人心痛的經歷，允許自己難過，傷口才會癒合。|復原、釋放痛苦|傷痛正在慢慢癒合。
休息、復原、沉澱|暫時停下來休養生息，恢復元氣後再出發。|無法休息、倦怠|身心過勞，或準備結束休息重新出發。
衝突、勝之不武、損失|爭贏了卻失去更多，想想這場仗是否值得。|和解、放下爭執|衝突結束，開始反思與修復。
過渡、離開困境、前進|正慢慢離開艱難的處境，前方會比較平靜。|無法脫身、未完的課題|想離開卻被牽絆，問題尚未真正解決。
策略、隱瞞、獨自行動|用智取代替正面衝突，也要留意不誠實的人事。|坦白、被揭穿|隱藏的事浮上檯面，誠實是最好的選擇。
受困、自我限制、無力感|你覺得被困住，其實束縛多半來自自己的想法。|掙脫、看見出路|開始放下限制性的信念，找到出口。
焦慮、失眠、擔憂|擔心與恐懼讓你夜不成眠，事情可能沒有想像中糟。|釋放焦慮、求助|焦慮漸緩，向信任的人傾訴會有幫助。
結束、谷底、徹底放下|最糟的已經過去，觸底之後只會往上。|復甦、殘留痛苦|正慢慢從谷底爬起。
好奇、觀察、求知|保持警覺與好奇心，多觀察、多學習。|多疑、八卦、說話傷人|留意言語和流言造成的傷害。
果斷、直接、行動迅速|想到就做，以清晰的目標快速推進。|衝動、急躁、口不擇言|太急於求成，或說話傷人。
獨立、清晰、直率|用理性和清楚的界線處理事情，誠實而有原則。|冷漠、尖銳、苛刻|過於嚴厲，忽略了別人的感受。
理性、權威、公正判斷|以邏輯和原則做決定，客觀處理問題。|冷酷、操控、濫用權力|理性變成了控制或冷酷。
新機會、財富、踏實開始|一個踏實的新機會出現，可能與金錢或工作有關。|錯失機會、規劃不足|機會可能溜走，財務需要更謹慎。
平衡、彈性、多工|在多件事情之間靈活調度，保持平衡就能應付自如。|失衡、手忙腳亂|事情太多導致失序，先排出優先順序。
合作、專業、技藝|團隊合作與專業能力受到重視，成果會被看見。|合作不順、品質不佳|團隊溝通或分工出現問題。
保守、守財、安全感|緊抓手上擁有的，穩定之餘也別太封閉。|過度吝嗇、學習放手|太執著於擁有，或正開始學著放開。
匱乏、困境、被冷落|物質或情感上感到困難，但幫助其實就在不遠處。|走出困境、獲得援助|最難的時期正在過去。
給予、接受、慷慨|資源的分享與流動，付出與接受都是平衡的一部分。|不平等、附帶條件|付出與回報失衡，留意隱藏的條件。
耐心、評估、長期投資|停下來檢視努力的成果，耐心等待收成。|急躁、白費力氣|努力暫時沒有回報，需要調整方向。
專注、精進、勤奮|埋頭苦幹、磨練技能，踏實的努力會累積成果。|缺乏動力、完美主義|對工作失去熱情，或過度追求完美。
獨立、富足、自給自足|靠自己的努力獲得成果，享受穩定與自在。|過度工作、表面富足|看似富足，內心卻不安穩。
財富、家族、長久穩定|長期累積帶來豐厚基礎，家庭與財務都很穩固。|家庭糾紛、財務損失|家族或長期財務出現問題。
學習、踏實、新計畫|以認真的態度學習新技能，打好基礎。|拖延、缺乏目標|計畫停留在想像，遲遲沒有行動。
穩健、負責、按部就班|慢慢來但很可靠，持續的努力一定會有成果。|停滯、懶散、過度保守|進度太慢，或固執於舊方法。
務實、照顧、富足|兼顧生活與工作，以務實和溫暖經營身邊的一切。|過度操勞、忽略自己|為他人付出太多，忘了照顧自己。
成功、穩定、財務掌控|事業與財務成熟穩定，你有能力管理資源。|貪婪、固執、物質至上|過於看重金錢或控制。`.split("\n").map(l=>l.split("|"));
CARDS.forEach((c,i)=>{c.maj=i<22;c.suit=i<22?-1:Math.floor((i-22)/14);c.court=i>=22&&(i-22)%14>=10;[c.kwU,c.mU,c.kwR,c.mR]=MEAN[i]});
const ELEM=[["權杖","火元素","行動、熱情與企圖心"],["聖杯","水元素","情感、關係與內心感受"],["寶劍","風元素","思考、溝通與衝突"],["錢幣","土元素","金錢、工作與現實層面"]];

const NUMS=["開始、潛能與種子","平衡、選擇與互動","成長、合作與初步成果","穩定、鞏固，也可能停滯","衝突、失去與變動","和諧、修復與分享","考驗、評估與堅持","推進、轉變與專注","接近完成，能量達到極致","完成、結束與新循環的開始"];
const COURT=["侍者代表學習者與新消息","騎士代表行動與推進","皇后代表內在的成熟與照顧","國王代表外在的掌控與權威"];
const ELEMD=["火元素：行動、熱情、企圖心","水元素：情感、關係、直覺","風元素：思考、溝通、衝突","土元素：金錢、工作、身體與現實"];
const POSF=[[/希望與恐懼/,"這個位置同時反映你期待和害怕的事，兩者常常是同一件事的兩面。"],
 [/阻礙|挑戰|缺點|陰影|盲點|課題|放下/,"這個位置點出你需要面對或克服的課題。"],
 [/建議|對策|行動|下一步|運用|如何|激發|指引/,"這個位置是牌給你的行動方向。"],
 [/過去|根源|來源|原因|分開/,"這個位置說明事情的背景與來由。"],
 [/結果|未來|走向|發展|可能|展現|使命|月|週|目標/,"這個位置顯示照目前的方向發展下去，可能出現的情況。"],
 [/對方/,"這個位置反映對方的狀態與想法。"],
 [/環境|周遭|人脈|收入|支出/,"這個位置代表外在環境與周遭帶來的影響。"],
 [/現況|現在|目前|狀況|狀態|你|我|身體|心理|靈性/,"這個位置描述你此刻的處境與狀態。"]];
const posFrame=p=>(POSF.find(f=>f[0].test(p))||[0,`這個位置回應「${p}」這個面向。`])[1];
const IMGS=`一位年輕人背著小包袱、手拿白玫瑰，站在懸崖邊仰望天空，小白狗在腳邊跳躍。|懸崖代表眼前未知的風險，白玫瑰代表純真，仰望天空表示他信任這趟旅程；小狗是提醒他留意腳下的本能。
魔術師一手舉杖指天、一手指地，桌上擺著權杖、聖杯、寶劍、錢幣，頭上有無限符號。|「上指天、下指地」表示把靈感帶到現實；桌上四元素齊備，代表需要的資源都在手上；無限符號象徵源源不絕的潛能。
女祭司坐在黑白兩根柱子之間，手持半掩的卷軸，腳邊有新月，身後垂著石榴帷幕。|黑白柱代表二元對立，她坐在中間保持平衡；半掩的卷軸與帷幕代表尚未揭露的知識，新月象徵直覺與潛意識。
皇后坐在金黃麥田與森林之間，身旁有刻著金星符號的愛心盾牌，頭戴星冠，溪流從旁流過。|成熟的麥田象徵豐收，流水代表情感與生命力，金星符號代表愛與美，整個畫面是滋養與富足。
皇帝穿著盔甲坐在刻有公羊頭的石座上，手持權杖與寶球，背後是荒涼的高山。|石座與盔甲代表穩固與防衛，公羊象徵牡羊座的開創與領導，荒山則暗示他靠紀律在嚴峻環境中建立秩序。
教皇坐在兩柱之間舉手祝福，腳前跪著兩位信徒，下方放著交叉的鑰匙。|祝福手勢與信徒代表傳承與教導，交叉的鑰匙象徵開啟知識之門，整體指向制度、傳統與精神導師。
天使在上方張開雙手祝福，下方一男一女裸身相對，女性身後是盤著蛇的知識之樹。|天使代表更高的指引，赤裸代表坦誠相見；蛇與樹暗示誘惑與抉擇，所以這張牌同時代表愛與選擇。
戰士站在城外的戰車上，前方是一黑一白兩隻人面獅身獸，他手上沒有握韁繩。|黑白兩獸代表相反的力量，他不靠韁繩而是靠意志駕馭；離開城市代表向外征服，因此是意志與前進。
一位女性溫柔地合上獅子的嘴，頭上有無限符號，身上戴著花環。|她不用武力而是用溫柔馴服獅子，代表以柔克剛；獅子象徵本能與情緒，無限符號代表源源不絕的內在力量。
白髮老人站在雪山頂，手提一盞裝著星星的燈，拄著長杖低頭前行。|雪山頂代表孤獨的高處，燈象徵內在的智慧之光；燈只照亮腳前一步，代表向內探索、慢慢找答案。
天空中有一個巨輪在轉動，上有人面獅身獸、下有胡狼神，旁有一條蛇，四角是四隻有翅膀、在讀書的生物。|輪子不停轉動，有人上升有人下降，代表運勢的循環；四角生物對應四個固定星座，提醒變化之中仍有不變的法則。
正義女神坐在兩柱間，右手舉劍、左手持天秤，正面直視前方。|天秤代表衡量與公平，劍代表果斷與真相，直視的目光表示誠實面對，所以強調因果與理性判斷。
一個男人倒吊在T字形的樹上，表情平靜，頭部散發光環。|倒吊代表顛倒視角，平靜的表情表示這是自願的暫停，頭上的光環象徵因此得到的領悟。
骷髏騎士騎著白馬、舉著白玫瑰旗幟前進，國王倒下，主教與孩童迎面，遠方太陽在兩塔之間升起。|不論身分都要面對死神，代表無可避免的結束；遠方升起的太陽暗示結束後的新生，所以是「轉化」而不是單純的失去。
天使一腳踩在水中、一腳踩在岸上，把水在兩個杯子間來回傾倒，遠方有一條通往山頂的小路。|水陸各一腳代表平衡，兩杯間調和的水象徵融合與耐心，遠方的小路表示循序漸進就能抵達目標。
有角的惡魔蹲坐在台座上，下方一男一女被鎖鏈拴住，但鎖鏈其實很鬆。|鎖鏈象徵慾望與執著的束縛，但鬆鬆的鏈子表示他們其實可以自己離開，所以重點在於看清束縛。
閃電擊中高塔頂端，王冠被打落，火焰竄出，兩個人從塔上墜落。|高塔代表建立在不穩基礎上的結構，閃電是突如其來的衝擊，墜落代表舊秩序崩解、真相被迫揭露。
一位裸身女子跪在水邊，一手把水倒進池中、一手倒向大地，天空有一顆大星與七顆小星。|平靜地倒水代表療癒與付出，夜空中的星光象徵黑暗後的希望，裸身代表卸下防備、真誠面對自己。
月亮高掛在兩座塔之間，下方一隻狗和一匹狼對月嚎叫，一隻龍蝦從水池爬出，小路通往遠方。|朦朧的月光代表看不清，狗與狼象徵被馴化與野性的本能，從水中爬出的龍蝦代表浮現的潛意識恐懼。
燦爛的太陽下，一個孩子騎著白馬、張開雙臂，背後是盛開的向日葵。|孩子代表純真與喜悅，張開雙臂表示毫無保留，太陽與向日葵象徵成功、活力與一切明朗。
天使在雲端吹響號角，下方的人們從棺木中站起，張開雙手仰望。|號角是覺醒的召喚，從棺木中站起代表重生，仰望天使表示回應更高的使命、迎接新階段。
一位舞者手持兩根權杖，在月桂花環中起舞，四角有天使、老鷹、公牛與獅子。|花環代表圓滿與成就，舞動代表在完成中仍保持流動，四角生物齊聚表示各方面都已整合。
一隻手從雲中伸出，握著一根冒出新葉的權杖，遠方有座城堡。|從雲中伸出的手代表天賜的機會，冒芽的權杖象徵新的生命力與熱情，遠方的城堡是可以追求的目標。
一位男子手捧地球儀，站在城牆上眺望遠方，身旁立著兩根權杖。|手中的地球儀代表他在規劃更大的世界；站在城牆內眺望，表示他還在安全區，正準備決定下一步。
男子背對我們站在山崖上，望著海上的船隻，身邊有三根權杖。|看著出航的船代表等待投入的成果歸來，站在高處眺望表示視野開闊，事業正在向外拓展。
四根權杖撐起掛滿花環的門架，後方有人舉著花束歡慶，城堡在背景中。|花環門架象徵慶典與穩固的基礎，歡迎的人群代表歸屬與和諧，是階段性成功的畫面。
五個年輕人各拿一根權杖互相揮舞，看起來在打鬥，但沒有人受傷。|雜亂揮舞的權杖代表意見分歧與競爭，但沒有真正的傷害，更像是練習與磨合。
一位頭戴月桂冠的騎士騎馬前進，權杖上也掛著月桂環，身旁人群簇擁。|月桂冠代表勝利，騎在馬上被人群環繞，表示獲得公開的肯定與好消息。
一位男子站在高處，手持權杖抵擋從下方伸來的六根權杖。|站在高處有地利優勢，以一敵六代表面對挑戰時堅守立場；他兩腳穿著不同的鞋，暗示是在倉促中應戰。
八根權杖在空中平行飛過開闊的田野，畫面中沒有任何人物。|飛行中的權杖代表事情快速推進；沒有人物表示事件本身在動，象徵消息與進展迅速到來。
一位頭上纏著繃帶的男子靠著權杖，警戒地望向一旁，身後的八根權杖排成柵欄。|繃帶代表他已經受過傷，警戒的眼神表示仍在防守，柵欄般的權杖象徵最後一道防線與韌性。
一位男子彎著腰，吃力地抱著十根權杖走向遠方的村莊。|抱住所有權杖、遮住了視線，代表一肩扛起太多責任；彎腰的姿勢顯示壓力沉重。
一位年輕人站在荒漠中，雙手握著權杖，好奇地凝視頂端的新芽。|盯著新芽代表對新事物充滿好奇，荒漠背景表示這是剛開始探索的領域。
騎士騎著前腳高高抬起的駿馬，手持權杖向前衝，背景是沙漠與金字塔。|躍起的馬代表衝勁與行動力，但也暗示過於急躁；沙漠背景象徵冒險與遠行。
皇后坐在有獅子裝飾的王座上，一手拿權杖、一手拿向日葵，腳前坐著一隻黑貓。|獅子與向日葵象徵自信與溫暖的魅力，黑貓代表她的直覺與獨立。
國王坐在刻有獅子與火蜥蜴的王座上，手持發芽的權杖，身體前傾望向遠方。|火蜥蜴代表火元素與開創力，身體前傾表示隨時準備行動，是有遠見的領導者。
一隻手從雲中托出一個聖杯，五道水流溢出，一隻白鴿銜著聖餅俯衝而下，池中開滿蓮花。|滿溢的水象徵豐沛的情感，白鴿代表愛與平靜，蓮花代表心靈的開展，是新感情的開始。
一男一女互相舉杯對望，上方有一個帶翅膀的獅頭與雙蛇杖。|兩人平等地交換杯子，代表情感的對等與連結；獅頭與雙蛇杖象徵這段結合帶來療癒與力量。
三位女子舉杯共舞，腳下滿是水果與花朵。|舉杯共舞代表友誼與慶祝，豐收的果實象徵共享的喜悅。
一位男子盤坐樹下，雙手抱胸看著面前三個杯子，雲中伸出一隻手遞上第四個杯子。|抱胸代表封閉與不滿；他沒注意到遞來的新杯子，象徵忽略了眼前的機會。
一個披著黑斗篷的人低頭看著三個倒下的杯子，身後還有兩個立著的杯子，遠方有一座橋。|低頭凝視打翻的杯子代表沉浸在失落中；身後仍立著的兩杯與遠方的橋，暗示仍有希望與出路。
一個孩子把插著花的杯子遞給另一個更小的孩子，背景是古老的庭院。|孩子間的贈與代表純真與善意，懷舊的庭院象徵回憶與過去的美好。
一個人背對我們，看著雲中浮現七個杯子，杯中有城堡、珠寶、蛇、龍等各種東西。|雲中的杯子代表幻想與白日夢，形形色色的內容象徵選擇太多，有些是機會、有些是陷阱。
一個人在月光下拄著拐杖離開，背後留下八個疊好的杯子。|疊好的杯子代表他曾經建立的東西；在夜裡獨自離開，象徵為了尋找更深的意義而放下。
一個男人滿足地交叉雙臂坐著，身後的弧形桌上整齊排著九個杯子。|滿意的表情與滿桌的杯子代表願望實現，因此這張牌常被稱為「願望牌」。
一對夫婦張開雙手望向彩虹，彩虹中有十個杯子，身旁兩個孩子在跳舞，遠方有一棟房子。|彩虹象徵承諾與圓滿，家人共聚與遠方的家代表情感與家庭的幸福。
一位年輕人手持杯子，一條小魚從杯中探出頭來，他好奇地看著牠。|從杯中冒出的魚代表意外的靈感或情感訊息，好奇的表情表示以開放的心接受。
騎士騎著緩步前進的白馬，手捧聖杯，像是要遞給某人。|緩步而行的馬代表溫和，手捧聖杯像在獻上心意，象徵追求與浪漫的邀約。
皇后坐在海邊的王座上，專注地凝視手中精緻的有蓋聖杯。|靠近水邊代表與情感深深連結；專注凝視有蓋的杯子，象徵她理解深層的情緒與直覺。
國王坐在漂浮於波濤海面上的王座，手持聖杯與權杖，神情平穩。|身處洶湧的大海卻保持平靜，代表情緒成熟，能在波動中維持穩定。
一隻手從雲中握住一把直立的寶劍，劍尖穿過王冠，兩旁垂下橄欖枝與棕櫚葉。|直立的劍代表清晰的思想與真相，王冠象徵勝利，是用理性突破困境的畫面。
一位蒙眼的女子坐在海邊，雙手交叉持著兩把寶劍，背後有一彎新月。|蒙眼代表不願看清，交叉的劍表示兩難與僵持；平靜的海面下其實暗潮洶湧。
一顆紅心被三把寶劍刺穿，背景是灰色的雲層與大雨。|被刺穿的心是最直接的心痛象徵，雨天代表悲傷的情緒正在宣洩。
一位騎士雙手合十躺在石棺上，牆上掛著三把劍，身下放著一把劍，窗上是彩色玻璃。|安靜躺著代表休息與復原，劍被放下表示暫時停止戰鬥，教堂般的場景帶著沉澱的意味。
一個人得意地抱著三把劍，看著兩個垂頭喪氣離開的人，地上丟著兩把劍。|贏家的得意與輸家的落寞形成對比，代表勝利的代價與關係的破裂。
船夫撐著船，載著一位婦人與孩子駛向對岸，船上插著六把劍。|從湍急的水域駛向平靜的對岸，代表離開困境；劍仍在船上，表示帶著過去的傷痕繼續前進。
一個男人踮著腳，抱著五把劍偷偷離開營地，回頭張望，身後留下兩把劍。|偷偷摸摸的姿態代表策略或隱瞞，回頭張望表示心虛，也暗示事情可能不夠光明正大。
一位被蒙眼、身體被綁住的女子，站在八把插在地上的劍之間。|她被困在劍之間，但劍並沒有完全包圍她、腳也沒被綁，代表困住她的多半是自己的想法。
一個人半夜從床上坐起，雙手摀住臉，牆上橫掛九把劍。|摀臉代表焦慮與痛苦，牆上的劍是揮之不去的念頭，象徵失眠與擔憂。
一個人趴在地上，背上插著十把劍，遠方天空正透出曙光。|十把劍代表痛苦到了極點、不會再更糟；遠方的曙光象徵谷底之後的新開始。
一位年輕人站在風很大的山丘上，雙手舉劍，警覺地回頭張望。|舉劍回望代表保持警覺與觀察，強風象徵思緒活躍、資訊多變。
騎士揮劍騎著奔馳的白馬向前衝，雲朵與樹木都被強風吹動。|全速衝刺代表果斷與速度，但被風吹亂的背景也暗示過於急躁、不顧後果。
皇后坐在雲上的王座，右手直立舉劍，左手向前伸出，表情嚴肅。|直立的劍代表清晰的判斷，伸出的手表示願意溝通但有界線，嚴肅的神情代表獨立與理性。
國王正面坐在王座上，手持一把寶劍，直視前方。|正面直視代表客觀公正，手中的劍象徵以理性與原則做決定。
一隻手從雲中托出一枚錢幣，下方是開滿花的花園與一道拱門。|托出的錢幣代表實際的機會與資源，花園與拱門象徵通往富足的入口。
一個年輕人一邊跳舞一邊拋接兩枚錢幣，錢幣被無限符號形狀的帶子連著，背後海上的船隻隨浪起伏。|拋接代表在多件事之間保持平衡，無限符號表示持續的調整，海浪象徵生活中的起伏。
一位工匠站在長椅上雕刻教堂拱門，旁邊兩個人拿著設計圖與他討論。|工匠與設計者合作代表團隊與專業，教堂建築象徵經得起時間考驗的成果。
一個男人坐著緊抱一枚錢幣，頭上頂著一枚、腳下踩著兩枚，背後是城市。|緊緊抓著錢幣代表守財與安全感，但也顯示他封閉、不願讓資源流動。
兩個衣衫襤褸的人在雪中經過一扇亮著燈的彩繪玻璃窗，其中一人拄著拐杖。|雪中的窮人代表匱乏與困境；窗內的光代表其實有援助，只是他們沒有看見。
一位富商手持天秤，把錢幣分給跪著的兩個人。|天秤代表公平分配，給予與接受的畫面象徵資源的流動，也提醒雙方的位置是否平等。
一個農夫拄著鋤頭，看著藤蔓上長出的七枚錢幣。|停下來看著作物代表評估成果，耐心等待收成，象徵長期投資。
一位工匠坐在工作檯前專心雕刻錢幣，牆上已掛著好幾枚完成品。|一枚一枚地雕刻代表專注與精進，牆上的成品象徵累積起來的努力。
一位穿著華服的女子站在葡萄園中，手上停著一隻獵鷹，身旁滿是錢幣。|豐收的葡萄代表富足，馴服的獵鷹象徵自律，她獨自享受成果，代表獨立與自給自足。
一位老人坐在拱門下，身旁有狗、家人與孩子，十枚錢幣排成生命之樹的形狀。|三代同堂代表家族與傳承，生命之樹的排列象徵長久穩固的財富與根基。
一位年輕人站在草地上，雙手捧著一枚錢幣專注凝視。|專注凝視錢幣代表認真學習、踏實規劃，草地象徵剛開始耕耘的領域。
騎士騎在一匹靜止不動的黑馬上，手捧錢幣望著已耕好的田地。|靜止的馬代表穩健、不急躁，耕好的田象徵按部就班的努力。
皇后坐在花草環繞的王座上，雙手捧著錢幣低頭看著，腳邊有一隻兔子。|豐盛的花園與兔子象徵富足與生命力，低頭照看錢幣代表務實地照顧生活。
國王坐在刻有公牛頭的王座上，身穿葡萄藤圖案的長袍，手持錢幣，背後是城堡。|公牛代表金牛座的穩定與物質，葡萄藤與城堡象徵事業與財富的成就。`.split("\n").map(l=>l.split("|"));
CARDS.forEach((c,i)=>{[c.scene,c.sym]=IMGS[i];c.rank=i<22?i:(i-22)%14});
const elemLine=c=>c.maj?`大阿爾克那第 ${c.rank} 號。大阿爾克那代表人生中的重要課題與轉折，分量比小牌更重。`
  :`${SUITS[c.suit][0]}屬於${ELEMD[c.suit]}。${c.court?COURT[c.rank-10]:`數字 ${c.rank===0?"A（一）":c.rank+1} 代表${NUMS[c.rank]}`}。`;

const DEEP=`信任與冒險之間的平衡|他的眼睛看著天空，而不是腳下的懸崖。這份天真是勇氣的來源，也是盲點所在；小白狗正努力提醒他，但他還沒有回頭。|勇敢起步:已經準備好嘗試，不想再被過度的計畫綁住;回到初心:想找回最單純的熱情與好奇;輕裝上路:願意放下包袱，只帶最需要的東西出發;相信過程:不急著知道結果，相信路會越走越清楚|衝動行事:想逃離現況，所以急著跳進新的事情;害怕跨出:明明想改變，卻被風險嚇得原地不動;不顧後果:只看見自由，忽略了需要承擔的責任;錯過時機:一直等待完美時刻，結果機會悄悄過去
把想法化為行動的能力|桌上的四樣工具都已就位，但真正讓它們運作的，是他舉起的那隻手。資源本身不會自動變成成果，需要你主動去使用。|資源到位:你擁有的比你以為的還要多;主動出擊:是時候把計畫說出口、付諸行動;專注意圖:清楚知道自己要什麼，能量就會集中;展現才能:適合表現自己、爭取被看見的機會|能力閒置:有才華卻遲遲沒有拿出來用;方向分散:想做的太多，結果每件都只做一半;包裝過度:說得很好聽，實際內容卻不足;自我懷疑:懷疑自己的能力，所以不敢開始
傾聽內在的聲音|她手中的卷軸只露出一部分，身後的帷幕也遮住了後方的景色。有些答案不是不存在，而是時候未到，需要靜下心才看得見。|相信直覺:你的第一感覺往往是對的;靜觀其變:現在不適合急著行動，先觀察;向內探索:答案在內心，而不是別人的意見;保守秘密:有些事還不適合說出口|忽略直覺:明明心裡有感覺，卻用理性壓下去;資訊不明:還有隱藏的事情沒有浮上檯面;情緒封閉:把感受藏得太深，連自己都看不清;過度依賴他人:一直問別人，卻不敢聽自己的聲音
滋養與豐盛|她身邊的麥田已經成熟，但她並沒有急著收割，而是安穩地坐著。豐盛不只是努力的結果，也來自懂得接受與享受。|豐收期:努力開始有了具體的成果;照顧他人:你的溫柔正在滋養身邊的人;創造力湧現:很適合投入藝術、創作或新計畫;享受生活:允許自己放鬆，好好感受當下的美好|過度付出:一直照顧別人，卻忘了自己;創造停滯:靈感卡住，事情遲遲沒有進展;依賴與控制:愛變成了過度保護或依賴;忽略身體:身心疲憊，需要好好休息與補充能量
建立秩序與承擔責任|他身後是一片荒涼的高山，什麼都沒有長出來。皇帝的力量來自紀律與結構，但過度的控制，也可能讓生命失去柔軟與生機。|建立規則:需要為生活或工作建立清楚的結構;承擔領導:你被期待站出來做決定;穩定基礎:先把根基打穩，再談擴張;理性判斷:用邏輯而不是情緒處理問題|控制過度:想掌控一切，反而讓人想逃離;僵化固執:堅持舊方法，不願意調整;權威衝突:和上司、長輩或制度之間有拉扯;失去主導:覺得自己沒有力量做決定
傳承、信念與歸屬|他舉手祝福，腳前的兩位信徒卻背對著我們。這張牌談的是群體與傳承，也提醒你：跟隨規範的同時，別忘了自己真正相信的是什麼。|尋求指引:向有經驗的人請教會有幫助;遵循傳統:用穩定、被驗證過的方法比較安全;學習成長:適合進修、拜師或加入團體;價值認同:找到和你信念相同的群體|打破框架:你開始質疑原本相信的規則;教條束縛:覺得被制度或他人的期待綁住;另闢蹊徑:想用自己的方式走一條不同的路;信任動搖:對原本依靠的權威或導師感到失望
依照真正的價值做選擇|男人看著女人，女人卻看著上方的天使。這張牌不只是關於愛情，更是關於你做選擇時，看著的是眼前的人，還是更高的價值。|真心連結:一段坦誠、彼此吸引的關係;重要抉擇:你站在一個需要依照內心選擇的路口;價值一致:你和對方（或你和自己）想要的一樣;自我接納:接受真實的自己，才能真正被愛|選擇困難:在兩個方向之間來回搖擺;關係失衡:一方付出較多，或彼此期待不同;價值衝突:想要的東西和應該做的不一樣;逃避承諾:害怕選了之後就沒有退路
用意志駕馭相反的力量|他沒有握著韁繩，黑白兩隻獸也朝著不同方向。戰車前進靠的不是外力拉扯，而是內在的意志把矛盾的力量整合在一起。|全力前進:目標清楚，現在就是行動的時候;克服矛盾:能夠整合內心兩股不同的力量;自律致勝:靠紀律而不是運氣得到成果;向外拓展:適合出發、旅行或開創新局|失去方向:想往前衝，卻不確定要去哪裡;內在拉扯:兩種想法互相抵消，原地打轉;操之過急:太想贏，反而讓事情失控;被迫停下:外在阻礙讓計畫暫時受阻
以柔克剛的內在力量|她沒有用力壓制獅子，而是輕輕合上牠的嘴，表情平靜。真正的力量不是消滅情緒，而是溫柔地與它相處。|溫柔堅定:用耐心而不是強硬來處理問題;情緒穩定:能和自己的恐懼、憤怒和平共處;自信成長:你比你以為的更有勇氣;包容他人:用理解化解衝突|自我懷疑:覺得自己不夠好、撐不下去;情緒失控:壓抑太久的情緒突然爆發;過度忍耐:一直忍，卻沒有照顧自己的需要;逞強硬撐:用力證明自己，反而更累
在獨處中找到自己的答案|他手中的燈只照亮腳前的一小段路，而不是整座山。內在的智慧不會一次給你全部答案，只會讓你看清下一步。|沉澱思考:需要一段安靜的時間整理自己;尋找意義:開始思考真正重要的是什麼;導師角色:你可能成為別人的指引，或遇到一位;獨立前行:不需要別人的認同，也能走自己的路|過度孤立:把自己關起來，拒絕所有人;逃避現實:用獨處當作不面對問題的理由;迷失方向:想了很久，卻還是找不到答案;害怕孤單:不敢獨處，用忙碌填滿所有時間
接受變化，順勢而為|輪子上有人上升、有人下降，但輪子的中心是不動的。運勢會起落，你可以選擇站在比較穩定的核心，而不是被外圍的轉動甩來甩去。|轉機出現:局勢正在轉向對你有利的方向;順勢而為:跟著變化走，比硬撐更有效;因果循環:過去的努力開始帶來回報;意外機會:突如其來的人事物帶來新可能|低潮循環:暫時運勢不佳，需要耐心等待;抗拒改變:想留住過去，卻擋不住變化;重複模式:同樣的問題一再發生，需要打破循環;失去掌控:覺得事情超出自己的控制
誠實面對因果|她的劍是直立的，天秤是平衡的。正義不是冷酷的審判，而是先誠實地衡量，再果斷地做出決定。|公平決定:事情會依照合理的方式被處理;承擔責任:面對自己過去的選擇與結果;理性分析:把情緒放一邊，客觀檢視事實;合約文件:簽約、法律或正式事務順利|不公平感:覺得自己被不公平對待;逃避責任:不想面對自己造成的結果;判斷偏頗:被情緒或偏見影響了決定;結果延宕:正式事務延遲或出現爭議
暫停，換個角度看|他雖然倒吊著，臉上卻沒有痛苦，頭上甚至發出光芒。這不是被懲罰的停滯，而是自願停下來，才看見平常看不到的東西。|主動暫停:停下來不是失敗，而是在等待更好的時機;轉換視角:換個角度，問題可能就不是問題了;願意放下:放下某些堅持，換來更重要的東西;靜待時機:現在的等待是有意義的|僵持不動:卡住了，卻不願意做出任何改變;無謂犧牲:一直退讓，卻沒有換來任何東西;拖延逃避:用「再等等」當作不面對的藉口;受害心態:覺得自己被困住，卻沒看到其實可以選擇
結束是為了轉化|遠方的太陽正在兩座塔之間升起。死神帶走的是舊的形式，而不是你本身；結束之後，新的階段才有空間開始。|自然結束:某段關係、工作或習慣已經走到盡頭;徹底轉變:你正在經歷深層的蛻變;清出空間:放下舊東西，才能迎接新的;新生開始:結束之後，是一個全新的階段|抗拒結束:明知道該結束，卻緊抓不放;轉變緩慢:改變正在發生，但進展得很慢;害怕失去:對未知的恐懼讓你不敢放手;半途停滯:改變到一半，卡在新舊之間
找到平衡與融合|天使一腳在水中、一腳在陸地，水在兩杯之間流動卻沒有灑出來。這份平衡不是靜止的，而是持續不斷的細微調整。|調和平衡:在工作與生活、理性與感性之間找到中點;耐心等待:循序漸進，事情會慢慢變好;療癒修復:身心正在慢慢恢復;合作融合:不同的人或想法能夠好好融合|失去平衡:某一方面過度，另一方面被忽略;急躁冒進:想一步到位，結果適得其反;極端反覆:在兩種狀態之間大起大落;內在衝突:心裡的兩種需求無法協調
看清束縛你的是什麼|鎖鏈其實鬆鬆地掛在兩人的脖子上，只要願意就能拿下來。惡魔的力量，來自我們相信自己沒有選擇。|慾望驅動:被金錢、感官或物質強烈吸引;不健康的依附:一段讓你離不開、卻也不快樂的關係;習慣成癮:某些習慣正在控制你;面對陰影:看見自己不願承認的那一面|開始覺醒:意識到自己被什麼困住;掙脫束縛:正在一步步離開讓你不自由的處境;重拾自主:找回對生活的控制權;仍在拉扯:想離開卻又忍不住回頭
崩解之後的真相|塔頂的王冠被閃電打落，代表最高的那個假設先被推翻。倒下的往往不是你本身，而是你一直以為穩固、其實不穩的東西。|突然改變:意料之外的事件打亂原本的計畫;真相揭露:隱藏的問題終於浮上檯面;打破幻象:你看清了某件事的真實樣貌;重建契機:舊結構倒下，才有機會重新蓋起|延遲崩解:問題被暫時壓下，但沒有真正解決;內在動盪:外表沒事，內心正在劇烈變化;害怕改變:因為恐懼而勉強維持現狀;化險為夷:避開了一場可能的危機
在黑暗之後重新相信|她一手把水倒回池中、一手倒向大地，代表一邊療癒自己、一邊回饋世界。星星排在高塔之後，希望是在經歷崩解之後才變得真實。|重拾希望:最難熬的已經過去，曙光出現了;療癒自己:允許自己慢慢恢復;真誠展現:卸下防備，做真實的自己;靈感指引:有清楚的願景引導你前進|失去信心:暫時看不到希望，覺得努力沒有意義;過度理想:期待太高，與現實落差太大;自我封閉:不敢再相信，把心關了起來;需要充電:身心耗盡，需要重新連結讓你安心的事物
穿越不確定與恐懼|小路從水池一路延伸到遠方的山，中間經過狗與狼。即使月光朦朧，路其實一直都在，只是需要你一邊害怕、一邊往前走。|看不清楚:事情還在迷霧中，別急著下結論;內在恐懼:不安感可能被放大了;直覺訊息:夢境和直覺正在提醒你某些事;隱藏因素:有些資訊還沒有被揭露|迷霧散去:事情漸漸清楚，真相浮現;釋放恐懼:你開始看清哪些擔心是多餘的;面對欺瞞:發現某些人或事並不如表面;混亂加劇:不安的情緒暫時讓判斷更困難
純粹的喜悅與成功|騎在馬上的是一個孩子，而不是國王或戰士。太陽的成功不需要偽裝或防備，是一種可以坦然、開心地展現自己的狀態。|成功順利:事情朝好的方向發展;自信展現:適合大方地表現自己;快樂單純:回到簡單的喜悅;關係溫暖:身邊的人帶來支持與光亮|延遲的喜悅:好事會來，只是需要多一點時間;過度樂觀:忽略了一些現實的細節;短暫低落:暫時失去熱情，但很快會恢復;內在小孩受傷:需要重新找回單純的快樂
回應內心的召喚|號角響起時，人們是自己從棺木中站起來的。覺醒無法由別人代勞，需要你自己回應那個聲音。|覺醒時刻:你清楚意識到自己真正要什麼;重新評價:回顧過去，做出關鍵的決定;二次機會:過去的事有機會重新來過;使命召喚:感受到一股想去做某件事的力量|自我批判:對過去的錯誤放不下;猶豫不決:聽到了召喚，卻不敢回應;錯過時機:一直拖延，機會可能過去;拒絕反省:不願意面對過去帶來的教訓
圓滿完成，邁向下一段旅程|舞者在花環中仍然保持舞動的姿態。完成不是停止，而是在整合了所有經驗之後，自在地進入下一個循環。|圓滿完成:一個階段畫下美好的句點;成就達成:努力得到完整的回報;整合經驗:過去的一切都有了意義;向外擴展:準備好迎向更大的世界|差一步完成:只剩最後一點，卻遲遲沒有收尾;缺乏完結感:事情結束了，心裡卻還沒放下;停滯不前:在熟悉的圈子裡繞圈;害怕下一步:不敢踏出舒適圈進入新的階段
點燃新的熱情|權杖上冒出的新葉有幾片正在飄落，代表這股靈感是活的、會流動的。如果不及時行動，它也可能隨風而去。|新的開始:一個讓你興奮的計畫正在萌芽;靈感湧現:創意和想法源源不絕;行動力上升:很想立刻動手去做;機會降臨:有人或有事帶來新的可能|動力不足:想做，卻提不起勁開始;時機未到:靈感還不成熟，需要再醞釀;熱情消退:一開始很興奮，後來慢慢冷掉;方向不明:有能量卻不知道要用在哪裡
在安全區與遠方之間做規劃|他手捧著整個世界，卻仍站在城牆裡。這張牌談的是「看見可能性」與「真正出發」之間的距離。|規劃未來:正在思考長遠的方向;擁有選擇:手上有好幾條路可以走;準備出發:已經看見遠方，正在做最後準備;拓展野心:想要比現在更大的舞台|害怕離開:舒適圈讓你遲遲不敢行動;規劃不足:想法很大，但細節還沒想清楚;猶豫不決:在兩個選擇之間拉扯;眼高手低:看得很遠，卻沒有踏出第一步
等待投入的成果歸來|他背對著我們，面向大海與船隻。這是已經做出行動、正在等待回應的時刻，視野也比之前更遠了。|成果在路上:之前的努力開始有回應;拓展視野:適合跨領域、跨地區發展;遠見規劃:能看到比別人更長遠的發展;合作機會:遠方或外部帶來新的合作|進度延誤:等待的結果遲遲沒有到來;期望落空:實際情況和想像有落差;視野受限:只看眼前，錯失長遠的機會;準備不足:出發得太急，後勁不夠
階段性的慶祝與歸屬|花環門架是開放的，沒有牆。這份穩定不是封閉的城堡，而是一個歡迎人進來的家。|值得慶祝:一個階段性成果完成了;家庭和樂:與家人朋友的關係溫暖;安定下來:找到讓你有歸屬感的地方;重要儀式:可能有喜事、搬家或聚會|過渡期:處在兩個階段之間，還沒完全安定;歸屬感不足:身在其中，卻覺得不屬於這裡;家庭摩擦:親近的人之間有些不愉快;慶祝延後:好事稍微延遲，但還是會來
在競爭中磨合|五個人各自揮舞權杖，但沒有人真的打中誰。這更像一場練習或辯論，摩擦本身就是成長的過程。|良性競爭:和別人比較，激發出你的潛能;意見分歧:大家都有自己的想法，需要整合;腦力激盪:衝突中可能產生好點子;磨合期:團隊或關係正在找彼此的節奏|避免衝突:為了和平而壓抑自己的意見;內耗:在不重要的事情上消耗太多能量;衝突升級:小摩擦開始變成真正的對立;找到共識:爭執漸漸平息，開始合作
被看見的勝利|騎士頭上和權杖上都有月桂冠，身邊的人簇擁著他。這份勝利是公開的、被眾人看見的。|獲得肯定:你的努力被看見、被讚美;好消息:等待的結果傳來好消息;建立自信:成功讓你更相信自己;領導地位:你在群體中受到注目|自負驕傲:成功讓你有點飄飄然;期待落空:預期的掌聲沒有到來;在意評價:太在乎別人怎麼看你;成功延遲:成果還需要一點時間才會被看見
堅守自己的立場|他兩隻腳穿著不同的鞋子，看起來是匆忙中應戰。但他站在高處，即使準備不完美，仍然有守住的優勢。|堅持立場:面對質疑時，相信自己的判斷;捍衛成果:保護你辛苦得來的東西;勇敢面對:挑戰很多，但你撐得住;競爭優勢:你的位置其實比對手有利|疲於應付:挑戰一個接一個，讓你累壞了;想要放棄:覺得不值得再堅持下去;過度防衛:把所有人都當成敵人;被壓倒:壓力已經超過你能承受的程度
事情快速推進|畫面中沒有任何人物，只有飛行的權杖。事情的發展已經啟動，接下來會自己往前跑，你需要做的是跟上節奏。|快速進展:事情會比預期更快有結果;消息到來:等待的訊息即將傳來;行動高峰:很多事情同時推進;旅行移動:可能有出差、旅行或移動|進度延遲:事情卡住，遲遲沒有消息;操之過急:太急著推進，反而出錯;方向混亂:一次做太多事，失去焦點;溝通誤會:訊息傳遞出現落差
帶著傷繼續堅持|他頭上纏著繃帶，表示已經受過傷，但仍然站著守住防線。這張牌的力量來自韌性，而不是毫髮無傷。|堅持到底:只差最後一段，撐住就能過關;經驗累積:過去的傷讓你更有智慧;保持警覺:你已經知道哪裡可能出問題;韌性十足:比你以為的更能承受|筋疲力盡:長期緊繃，已經快撐不住;防衛過度:受過傷後，不敢再相信別人;想要放棄:覺得再努力也沒有用;需要支援:該讓別人幫你分擔了
學會放下多餘的負擔|他抱著所有權杖，連前方的路都看不到。重擔不只讓人疲累，也會遮住視線，讓你看不見真正的目標。|責任過重:你扛了太多不屬於你的事;接近終點:雖然很累，但目的地就在前方;努力過頭:用力到忘了最初為什麼開始;需要分工:有些事可以交給別人|卸下重擔:開始學著放手、說不;過勞崩潰:壓力已經到極限;推卸責任:想把負擔丟給別人;重新排序:決定哪些事真的值得扛
帶著好奇探索新領域|他站在荒漠中，好奇地看著權杖頂端的新芽。新的熱情往往出現在看似荒蕪的地方，只要你願意去看。|好奇探索:對新事物充滿興趣;新消息:有令人興奮的消息即將到來;學習熱情:適合開始學新的技能;勇於嘗試:不怕犯錯，先試再說|三分鐘熱度:開始很興奮，很快就失去興趣;不夠成熟:想法很好，但還不夠穩定;壞消息:期待的消息不如預期;缺乏方向:有熱情卻不知道該往哪裡去
熱情衝刺的冒險者|他的馬前腳高高抬起，隨時要衝出去，但背景是一片沙漠。熱情帶來行動力，也可能讓人沒有準備就上路。|勇於冒險:敢於挑戰沒走過的路;行動派:想到就做，充滿衝勁;熱情追求:對某人或某事全力以赴;改變環境:可能有搬家、旅行或換跑道|衝動魯莽:沒想清楚就行動;半途而廢:衝得快，但持續力不足;脾氣急躁:容易因為不耐煩而起衝突;延遲出發:計畫被擱置，動不起來
自信而溫暖的存在|她腳邊坐著一隻黑貓，象徵她的獨立與神秘。她的光芒很溫暖，但也清楚自己的界線。|自信魅力:你的熱情吸引了身邊的人;獨立自主:能夠照顧好自己;溫暖領導:用熱情帶動周遭的人;勇於表現:大方展現自己的才能|缺乏自信:懷疑自己，不敢展現;嫉妒比較:對別人的成就感到不安;過度強勢:想要掌控一切;能量耗盡:照顧太多人，自己沒力氣了
有遠見的開創者|他的身體微微前傾，看起來隨時準備起身。這位國王不是坐著享受成果，而是一直在看下一個目標。|遠見領導:能看到大方向並帶領別人;開創事業:適合開創新事業或新計畫;果斷決策:能快速做出決定;激勵他人:你的熱情能鼓舞團隊|專斷獨行:不聽別人的意見;急躁衝動:決定太快，缺乏考慮;期望過高:對自己或他人要求太多;權力欲:過度追求掌控與地位
敞開心接受愛|聖杯中流出的五道水落入下方的蓮花池。愛不是被留在杯中，而是不斷流向外面的世界。|新感情:一段新的關係或情感正在開始;情感充沛:內心充滿愛與感動;直覺開啟:對他人的感受特別敏銳;自我疼愛:學會好好愛自己|情感壓抑:把感受藏起來，不敢表達;空虛感:心裡覺得缺了什麼;錯失緣分:愛出現了卻沒有把握;愛自己不足:付出很多，卻沒有給自己
平等而真誠的連結|兩人站在同一個高度，互相交換杯子。這段關係的核心是對等，沒有誰在上、誰在下。|心意相通:彼此有好感，感覺很契合;平等關係:付出與接受是平衡的;和解修復:有機會修補一段關係;合作夥伴:找到志同道合的人|關係失衡:一方付出比較多;溝通誤會:彼此的想法沒有對上;分歧產生:原本的默契出現裂痕;期待不同:雙方想要的關係不一樣
共享喜悅的友誼|三位女子圍成一圈舉杯，沒有誰是中心。這是一種平等、互相支持的歡樂。|朋友支持:身邊有人陪伴與支持你;值得慶祝:有喜事可以分享;社交擴展:適合參加聚會、認識新朋友;團隊合作:大家相處融洽|過度玩樂:社交多到忽略了重要的事;小圈圈:群體中出現排擠或八卦;關係疏離:和朋友漸行漸遠;三角關係:感情中出現第三者的影子
在倦怠中看不見機會|雲中伸出的手正遞上第四個杯子，但他雙手抱胸，只看著地上的三個杯子。新的機會已經在眼前，只是他還沒抬頭。|倦怠無感:對眼前的事提不起勁;向內沉思:需要時間思考自己真正要什麼;忽略機會:可能沒注意到身邊的新可能;不滿現況:覺得現在擁有的不夠|重新振作:開始走出低潮;接受機會:願意看看新的可能性;找回動力:重新找到讓你在意的事;仍在逃避:用冷漠保護自己，不想面對
在失落中看見留下的|他低頭看著倒下的三個杯子，卻沒發現身後還立著兩個杯子，遠方也有一座橋。失去是真的，但不是全部。|悲傷失落:正在為失去的人事物難過;後悔遺憾:對過去的選擇感到懊悔;需要哀悼:允許自己好好難過;只看負面:注意力都放在失去的部分|逐漸釋懷:開始接受發生的事;看見希望:發現還有值得珍惜的東西;重新站起:準備好向前走;放不下:仍然困在過去的情緒中
回憶與純真的溫暖|孩子遞出插著花的杯子，背景是安全的庭院。這張牌連結的是過去的單純與善意。|美好回憶:過去的回憶帶來溫暖;舊人重逢:老朋友或舊情人再次出現;純真善意:用單純的心對待彼此;回到根源:回家鄉、回到熟悉的地方|沉溺過去:太依戀過去，無法向前;美化回憶:記憶中的比真實的更美好;不願長大:逃避現實的責任;走出過去:開始把注意力放回現在
在眾多幻想中做出選擇|雲中的七個杯子裡，有城堡、珠寶，也有蛇和龍。誘人的選項中，往往同時藏著機會與陷阱。|選擇太多:眼前有很多可能，不知道選哪個;白日夢:想像很美好，但還沒落實;需要分辨:要看清楚哪些是真的可行;創意發想:想像力豐富，適合發想階段|看清現實:幻想消散，知道自己真正要什麼;做出決定:終於從選項中選了一個;被誘惑:被不切實際的承諾吸引;逃避現實:用幻想代替行動
安全感與內在渴望之間的拉扯|這些聖杯並沒有破碎，排列得整整齊齊。人物不是因為一無所有而離開，而是即使已經擁有一些事物，仍然感到有所欠缺。|主動離開:準備離開不再滿足你的事物;追尋意義:想找到更深層的價值;放下投入:願意放下已經付出的東西;獨自前行:需要一段獨處與探索的旅程|抗拒改變:知道現況不符合期待，卻害怕未知;留戀過去:因為已經投入很多，不願意放下;重新評估:開始發現原有的事物仍然具有價值;逃避式探索:不斷尋找新方向，卻沒有深入經營任何方向
願望實現的滿足|他滿足地交叉雙臂坐著，杯子卻都排在他的身後。滿足感來自擁有，也需要你真正去享受，而不只是展示。|願望實現:心裡想要的事情成真;自我滿足:對自己現在的狀態很滿意;享受生活:好好犒賞自己;感恩知足:看見自己已經擁有的|空虛感:得到了，卻還是不快樂;過度放縱:用物質或享樂填補內心;自滿停滯:太滿足於現狀而停止成長;期待落空:願望沒有如預期實現
情感上的圓滿|一家人望著天上的彩虹，孩子們在旁邊跳舞。這份幸福不是來自物質，而是來自彼此的陪伴。|家庭幸福:與家人關係和樂;關係圓滿:感情穩定、彼此承諾;情感滿足:內心感到安穩與被愛;理想生活:正在實現你想要的生活|家庭失和:家人之間出現摩擦;理想破滅:現實和想像中的幸福有落差;表面和諧:看起來很好，其實有隱藏的問題;價值不同:對未來的想像不一致
用柔軟的心接收訊息|一條魚從杯中探出頭來，他沒有驚嚇，反而好奇地看著。意外的情感或靈感，需要用開放的心去接納。|浪漫訊息:可能收到告白或感情好消息;創意靈感:突如其來的點子;直覺敏銳:你的感受力正在提升;單純好感:一段剛萌芽的情感|情緒化:容易因為小事而起伏;不成熟:用孩子氣的方式處理感情;失望訊息:感情消息不如預期;逃避感受:不敢面對自己真正的情緒
帶著心意前來|他的馬是緩步前進的，不像其他騎士那樣衝刺。這是一份溫柔、帶著真心的靠近。|浪漫追求:有人帶著真心向你靠近;邀約到來:可能有約會或合作邀請;追隨理想:跟隨內心的熱情行動;溫柔表達:用柔軟的方式表達心意|承諾不實:說得很好聽，但不一定做得到;情緒反覆:感情忽冷忽熱;不切實際:太過浪漫，忽略現實;逃避衝突:只想要美好，不想面對問題
深刻的同理與直覺|她手中的杯子是有蓋的，只有她自己知道裡面裝著什麼。她能理解他人的情緒，同時也保有自己內心的深度。|同理體貼:你很能理解別人的感受;直覺準確:相信你的第六感;情感成熟:能溫柔地照顧自己的情緒;療癒能量:你的存在讓別人感到安心|情緒依賴:太在意別人的反應;界線模糊:把別人的情緒當成自己的;過度敏感:容易被小事影響;忽略自己:照顧別人卻忘了照顧自己
在波濤中保持平靜|他的王座漂浮在洶湧的海面上，但他神情平穩。情緒成熟不是沒有波動，而是能在波動中保持穩定。|情緒穩定:能冷靜處理感情問題;包容大度:願意理解和接納別人;成熟關懷:用穩重的方式照顧身邊的人;理性感性兼具:同時顧及感受與現實|壓抑情緒:表面平靜，內心波濤洶湧;情緒操控:用情感影響或控制別人;冷漠疏離:不願意表達真正的感受;情緒爆發:壓抑太久，突然失控
用清晰的思考切開迷霧|劍尖穿過王冠，兩旁垂掛著橄欖枝與棕櫚葉。清晰的真相能帶來勝利，但劍是雙面刃，需要智慧地使用。|思緒清晰:終於想通了某件事;真相大白:事情的真相浮現;做出決定:有能力做出果斷的判斷;突破困境:用新的想法打開僵局|思緒混亂:腦中有太多想法，理不清;判斷錯誤:被錯誤資訊誤導;言語傷人:說話太直接，傷了別人;拒絕面對:不想看清真相
在兩難中選擇不看|她蒙著眼睛，交叉握著兩把劍，背後是平靜的海面。她不是做不了決定，而是還不想面對決定之後的後果。|兩難抉擇:在兩個選項之間無法決定;暫時休戰:選擇先不處理衝突;逃避面對:用不看來維持表面的平衡;需要更多資訊:還缺少做決定的關鍵|僵局鬆動:開始願意面對問題;資訊過載:知道得越多，越難決定;焦慮升高:拖延的壓力開始浮現;做出決定:終於拿下眼罩，做出選擇
允許自己心痛|三把劍刺穿紅心，背景下著雨。這張牌很直接地承認：有些痛就是痛，不需要假裝沒事。|心碎難過:正在經歷感情或人際的傷痛;痛苦真相:聽到或看到讓你難受的事實;悲傷釋放:讓眼淚流出來，才能開始癒合;三角關係:感情中有第三方的影響|慢慢復原:傷口正在癒合中;釋放痛苦:願意放下過去的傷;壓抑悲傷:不讓自己難過，傷痛反而更久;開始原諒:開始原諒別人或自己
休息是為了再出發|騎士雙手合十躺著，但劍還掛在牆上，沒有被丟掉。這是暫時的休息與沉澱，而不是放棄戰鬥。|好好休息:身心需要充電;沉澱思考:暫停一下，整理思緒;療養恢復:從疲憊或病痛中慢慢恢復;靜心獨處:適合安靜地和自己相處|無法休息:明明很累，卻停不下來;倦怠疲乏:長期累積的疲勞爆發;重新出發:休息夠了，準備回到戰場;被迫停下:身體或環境逼你暫停
勝利的代價|贏家抱著劍得意地看著，輸家垂頭離開，天上的雲也被吹散。這場爭執有人贏了，但關係也因此破裂。|勝之不武:爭贏了，卻失去更多;爭執衝突:人際間有不愉快的對立;自私行為:只顧自己的利益;看清局勢:想清楚這場仗是否值得打|放下爭執:願意結束衝突;和解修復:嘗試修補關係;反省後悔:回頭看見自己的錯;持續怨恨:心裡仍放不下過去的爭吵
帶著傷痕前往平靜的彼岸|船上的人背對我們，看不到他們的表情，但船正駛向平靜的水面。離開困境不一定是開心的，但是必要的。|離開困境:正在遠離讓你痛苦的處境;過渡時期:從一個階段移動到下一個;尋求協助:有人幫你度過難關;心境轉換:心情慢慢平靜下來|無法離開:想走卻被牽絆住;未完的課題:問題跟著你到新的地方;抗拒過渡:不願意接受改變;回到原點:兜了一圈又回到老問題
策略與隱瞞|他抱著五把劍偷偷離開，卻留下了兩把。用策略可以避開衝突，但不夠光明的做法，總有被發現的一天。|運用策略:用聰明的方式解決問題;獨自行動:不想讓別人知道你的計畫;留意欺瞞:身邊可能有不誠實的人事;避免正面衝突:選擇迂迴的方式|坦白承認:決定說出真相;被揭穿:隱藏的事情浮上檯面;良心不安:心裡對某件事感到愧疚;自我欺騙:用藉口騙自己
困住你的是自己的想法|她被蒙住眼睛、雙手被綁，但雙腳是自由的，周圍的劍也沒有完全包圍她。出口一直都在，只是她暫時看不見。|自我限制:覺得自己被困住、沒有選擇;無力感:不知道該怎麼改變現況;受害者心態:把問題都歸咎於環境;需要轉念:換個想法，路就會出現|掙脫束縛:開始看見出路;放下恐懼:不再被負面想法控制;重獲自由:走出讓你窒息的處境;仍在掙扎:知道要改變，卻還在拉扯
深夜的焦慮|牆上的劍並沒有刺向他，只是掛在那裡。讓人失眠的往往不是事情本身，而是腦中反覆的想像。|焦慮失眠:擔心到睡不好;過度擔憂:把事情想得比實際更糟;內疚自責:為過去的事情責怪自己;需要傾訴:悶在心裡只會更痛苦|焦慮減輕:開始放下多餘的擔心;尋求幫助:願意向人說出心事;面對恐懼:發現事情沒有想像中可怕;陷入絕望:負面情緒累積到極限
觸底之後只會往上|他背上插著十把劍，看起來很慘，但遠方天空已經透出曙光。最糟的已經發生，接下來只能慢慢好起來。|徹底結束:某件事真正畫下句點;谷底時期:你正處在最低潮的時候;放下掙扎:接受結果，不再抵抗;新的黎明:痛苦之後即將迎來轉機|慢慢復甦:開始從谷底爬起來;拒絕結束:不願意接受已經結束的事實;殘留傷痛:舊傷還在隱隱作痛;避開最壞:差點發生的事情沒有發生
保持警覺與好奇|他站在風很大的山丘上，舉著劍回頭張望。他的好奇心很強，也有點神經緊繃，隨時準備應對變化。|求知慾強:對新知識充滿好奇;觀察入微:能注意到別人沒發現的細節;保持警覺:留意身邊的變化;新的想法:開始思考新的方向|多疑猜忌:對人不太信任;八卦是非:小心流言與傳話;說話傷人:用尖銳的語言回應;想太多:想很多，卻沒有行動
果斷而迅速的行動|他全速衝刺，連背景的雲和樹都被風吹亂。速度是他的優勢，但也容易讓他忽略周遭的人。|果斷行動:想到就去做;目標明確:清楚知道要往哪裡去;直言不諱:勇於說出自己的想法;快速推進:事情會加速進行|衝動急躁:沒想清楚就行動;口不擇言:說話太衝，傷害別人;橫衝直撞:不顧後果地往前衝;失去方向:速度很快，卻不知道終點在哪
清晰的界線與獨立|她一手舉劍、一手向前伸出，像是在說「我願意聽，但我有我的原則」。她的理性往往來自經歷過的傷痛。|獨立自主:能清楚地照顧好自己;理性溝通:用清楚的方式表達想法;設立界線:知道什麼可以、什麼不行;誠實直率:不拐彎抹角，說真話|冷漠疏離:用理性把情感隔開;尖銳苛刻:對別人要求太高;孤立自己:不讓別人靠近;受傷防衛:因為過去的傷而築起高牆
以理性與原則做判斷|他正面坐著、直視前方，是四位國王中最不帶情緒的一位。他的決定依據的是邏輯與公正，而不是個人好惡。|理性判斷:能客觀地分析問題;專業權威:適合請教專家或扮演專業角色;公正處理:用原則而不是情緒處理事情;清晰溝通:說話有條理、有說服力|濫用權力:用權威壓制別人;冷酷無情:只講道理，不講人情;操控他人:用言語或邏輯控制別人;判斷失準:太相信自己，忽略了其他觀點
踏實的新機會|錢幣下方的花園有一道拱門，門外是遠方的山。這個機會本身是一個入口，真正的成果需要你走出去慢慢耕耘。|新機會:可能有新工作、收入或投資的機會;踏實起步:適合開始一個實際可行的計畫;財務好轉:經濟狀況有改善的跡象;身體健康:身體狀況穩定|錯失機會:機會溜走，沒有把握住;規劃不足:想法很好，但不夠實際;財務緊縮:要更謹慎地使用金錢;貪快求成:太急著想看到回報
在變動中保持平衡|他一邊跳舞一邊拋接錢幣，背後的船在大浪中起伏。生活本來就不會完全平穩，重點是你能不能保持彈性。|靈活調度:在多件事之間找到平衡;適應變化:能夠應對生活的起伏;時間管理:需要安排好優先順序;輕鬆應對:用輕鬆的心態面對壓力|手忙腳亂:事情太多，顧此失彼;財務失衡:收支開始不平衡;過度承擔:接了太多事情;需要取捨:是時候放掉一些事了
合作與專業被看見|工匠站在高處工作，下方兩人拿著設計圖和他討論。好的成果需要專業技術，也需要不同角色的合作。|團隊合作:和他人合作會有好結果;專業認可:你的能力被肯定;學習精進:適合向專業人士學習;打好基礎:扎實的努力正在累積|合作不順:團隊溝通出現問題;品質不佳:成果不如預期;缺乏認可:努力沒有被看見;各做各的:缺少協調與共識
守住擁有與害怕失去|他頭上、懷裡、腳下都有錢幣，把自己整個圍住。安全感很重要，但抓得太緊，也會讓資源和關係無法流動。|守住資源:適合儲蓄、保守理財;建立安全感:努力讓生活穩定下來;設立界線:保護自己的時間與能量;穩定守成:先守住現有的成果|過度吝嗇:太害怕失去，不願付出;學習放手:開始願意讓資源流動;控制慾:想緊緊掌控身邊的人事物;財務失控:花費超出預期
困境中的援助其實很近|兩人在雪中走過一扇亮著燈的窗，卻沒有往裡面看。幫助其實就在身邊，只是在困難中，人常常忘了抬頭。|經濟困難:物質上遇到挑戰;孤立無援:覺得被冷落或拋下;健康警訊:需要多注意身體;忽略援助:身邊其實有人可以幫你|走出困境:最困難的時期正在過去;獲得幫助:願意接受別人的援助;重新振作:找到新的資源與出路;仍在掙扎:困難還沒完全解決
給予與接受的平衡|富商手上拿著天秤，代表他的給予是有衡量的。這張牌談的是資源的流動，也提醒你看看雙方的位置是否平等。|慷慨給予:你有能力幫助別人;獲得援助:有人願意支持你;資源流動:付出與回報達到平衡;公平交易:合作或交易是對等的|附帶條件:幫助背後有其他目的;不對等關係:一方總是付出，另一方總是接受;債務問題:注意借貸與欠款;自尊受傷:不好意思接受別人的幫忙
耐心評估長期的投資|他拄著鋤頭看著藤蔓上的錢幣，作物已經長出來，但還沒到收成的時候。這是停下來檢視、而不是急著收割的時刻。|耐心等待:努力需要時間才會開花結果;評估成果:停下來檢視目前的進度;長期投資:適合規劃長遠的計畫;調整策略:根據現況做些修正|急於求成:等不及結果，想要馬上看到回報;白費力氣:投入的方向可能不對;失去耐心:想放棄正在進行的事;成果不佳:付出與收穫不成比例
專注與精進|牆上已經掛了好幾枚完成的錢幣，他仍然專心地雕刻下一枚。精進來自日復一日的重複練習。|專注投入:全心全意做好一件事;技能精進:持續練習，能力會越來越好;勤奮踏實:用努力換取成果;學徒精神:願意從基礎學起|缺乏動力:對工作失去熱情;完美主義:過度追求細節，進度停滯;重複無趣:覺得每天都在做一樣的事;偷工減料:想走捷徑，品質下降
獨立的富足|她手上停著一隻戴著頭罩的獵鷹，代表馴服與自律。她的富足來自長期的自我管理，也讓她能夠自在地獨處。|自給自足:靠自己的能力過上好生活;享受成果:好好犒賞努力過的自己;獨立自主:不依賴別人也能過得很好;品味生活:懂得欣賞生活中的美好|過度工作:為了物質犧牲生活;表面富足:看起來很好，內心卻不安;依賴他人:經濟或情感上依賴別人;孤單感:獨立過頭，感到寂寞
長久的根基與傳承|老人身邊有家人、孩子和狗，十枚錢幣排成生命之樹。真正的財富不只是金錢，而是能夠延續下去的家族與連結。|家族支持:家人是你的後盾;長期穩定:財務與生活都很穩固;傳承延續:適合思考長遠的規劃;團聚和樂:家族關係和諧|家族糾紛:家人之間因金錢或財產起爭執;財務損失:長期投資出現問題;傳統束縛:被家族期待綁住;根基動搖:原本穩定的東西出現變化
認真學習、踏實起步|他雙手捧著錢幣，專注得像在研究一件寶物。這張牌的力量來自認真的態度，而不是速度。|認真學習:適合進修、考證照;新計畫:開始一個實際的計畫;踏實務實:一步一步打好基礎;好消息:工作或財務上的好消息|拖延懶散:計畫一直停在想像階段;缺乏目標:不知道要往哪裡努力;不切實際:想法不夠落地;學習中斷:學到一半就放棄
穩健可靠的前進|他是唯一一位騎在靜止的馬上的騎士，望著已經耕好的田。他不急，但每一步都很扎實。|穩健前進:按部就班地完成計畫;負責可靠:別人可以信任你;耐心耕耘:持續的努力一定會有收穫;務實規劃:重視細節與執行|停滯不前:進度太慢，動不起來;過度保守:不敢做任何改變;固執死板:堅持舊方法，不願調整;無聊乏味:生活缺少新鮮感
務實而溫暖的照顧|她身邊開滿花，腳邊有一隻兔子，低頭照看手中的錢幣。她把生活打理得豐盛，是因為用心照顧身邊的每一件小事。|照顧家庭:兼顧工作與生活;務實理財:懂得管理金錢與資源;溫暖穩定:讓身邊的人感到安心;享受生活:在日常中找到滿足|過度操勞:為別人付出太多;忽略自己:忘了照顧自己的需要;物質焦慮:太擔心金錢與安全感;工作生活失衡:把所有時間都給了工作或家庭
成熟的財富與掌控|他的長袍上長滿葡萄藤，幾乎和王座融為一體。他的成就是長時間耕耘出來的，穩定而豐厚。|事業成功:工作與財務都很穩定;資源管理:懂得運用與分配資源;穩重可靠:是別人可以依靠的對象;實踐成果:想法都能落實成真|貪婪物質:過度看重金錢;固執保守:不願意冒任何風險;控制慾強:用金錢或地位控制別人;工作狂:把成就看得比一切都重要`.split("\n").map(l=>{const [core,detail,up,rv]=l.split("|"),f=t=>t.split(";").map(r=>r.split(":"));return {core,detail,up:f(up),rv:f(rv)}});
const MAJ_EN=["The Fool","The Magician","The High Priestess","The Empress","The Emperor","The Hierophant","The Lovers","The Chariot","Strength","The Hermit","Wheel of Fortune","Justice","The Hanged Man","Death","Temperance","The Devil","The Tower","The Star","The Moon","The Sun","Judgement","The World"];
const RANK_EN=["Ace","Two","Three","Four","Five","Six","Seven","Eight","Nine","Ten","Page","Knight","Queen","King"],SUIT_EN=["Wands","Cups","Swords","Pentacles"];
CARDS.forEach((c,i)=>{c.deep=DEEP[i];c.en=i<22?MAJ_EN[i]:`${RANK_EN[(i-22)%14]} of ${SUIT_EN[Math.floor((i-22)/14)]}`});
function buildReading(useRev){
  const cs=picked.map((ci,i)=>({c:CARDS[ci],r:useRev&&rev[ci],pos:sp.pos[i][0],i}));
  const n=cs.length,maj=cs.filter(x=>x.c.maj),rv=cs.filter(x=>x.r),court=cs.filter(x=>x.c.court);
  const pip=cs.filter(x=>!x.c.maj&&!x.c.court);
  const cnt=[0,0,0,0];cs.forEach(x=>{if(x.c.suit>=0)cnt[x.c.suit]++});
  const top=Math.max(...cnt),dom=top>=2&&cnt.filter(v=>v===top).length===1?cnt.indexOf(top):-1;
  const nm=x=>`${x.c.name}（${x.r?"逆位":"正位"}）`,mean=x=>x.r?x.c.mR:x.c.mU,kw=x=>x.r?x.c.kwR:x.c.kwU;
  const missing=ELEM.filter((_,i)=>!cnt[i]&&n>=4).map(e=>e[0]);
  /* 判讀依據 */
  const bar=(l,v,t)=>`<div class="st"><span>${l}</span><div class="tr"><i style="width:${n?v/n*100:0}%"></i></div><b>${v}</b><small>${t}</small></div>`;
  const stats=`<div class="stats">${bar("大阿爾克那",maj.length,"人生課題")}${ELEM.map((e,i)=>bar(e[0],cnt[i],e[1])).join("")}${useRev?bar("逆位",rv.length,"受阻、向內"):""}${bar("宮廷牌",court.length,"人物、角色")}</div>`;
  const ov=[];
  if(n===1){const x=cs[0];ov.push(`這次只抽一張，所以整個答案都集中在${nm(x)}。畫面中，${x.c.scene}${x.c.sym}`,`因此它回應你的訊息是「${kw(x)}」：${mean(x)}`)}
  else{
    if(!maj.length)ov.push(`<b>沒有大阿爾克那。</b>${n} 張全是小阿爾克那，表示這件事多半落在日常層面，不是命運式的大轉折，你可以靠自己的選擇與行動去調整。`);
    else if(maj.length/n>=.5)ov.push(`<b>大阿爾克那佔了 ${maj.length} / ${n} 張</b>（${maj.map(x=>`「${x.pos}」的${x.c.name}`).join("、")}）。超過一半是代表人生課題的大牌，表示這件事牽動的是較深層的轉變，背後有比較大的力量在推動，順勢而為比硬拚更重要。`);
    else ov.push(`<b>出現 ${maj.length} 張大阿爾克那</b>（${maj.map(x=>`「${x.pos}」的${x.c.name}`).join("、")}）。大牌分量比小牌重，它們落在的位置就是這次占卜的關鍵，其餘小牌描述的是日常的細節。`);
    if(dom>-1)ov.push(`<b>${ELEM[dom][0]}最多（${top} 張）。</b>${ELEM[dom][0]}屬於${ELEMD[dom]}，所以這件事的重心在於${ELEM[dom][2]}。${missing.length?`相對地，完全沒有出現${missing.join("、")}，代表${missing.map(m=>ELEM[ELEM.findIndex(e=>e[0]===m)][2]).join("，以及")}目前不是主要的影響因素，也可能是你忽略的面向。`:""}`);
    else if(missing.length)ov.push(`<b>花色分布平均，</b>但沒有出現${missing.join("、")}，代表${missing.map(m=>ELEM[ELEM.findIndex(e=>e[0]===m)][2]).join("，以及")}這些面向在這件事中較少被觸及，也可能是你需要補上的部分。`);
    if(pip.length>=2){const avg=pip.reduce((a,x)=>a+x.c.rank+1,0)/pip.length;
      ov.push(`<b>數字牌的平均約為 ${avg.toFixed(1)}。</b>小牌的數字代表事情所處的階段：${avg<4?"數字偏小（A 到 3），代表事情還在起步、萌芽的階段，很多事情尚未定型，現在的選擇影響很大。":avg<7?"數字落在中段（4 到 6），代表事情正在發展與調整中，會經歷穩定、衝突與修復的過程。":"數字偏大（7 到 10），代表事情已走到考驗或收尾的階段，某個循環即將完成，新的開始也在醞釀。"}`)}
    if(useRev){
      if(!rv.length)ov.push("<b>全部都是正位。</b>正位代表牌的能量能順暢地向外發揮，事情大致朝你期待的方向流動。");
      else if(rv.length/n>=.5)ov.push(`<b>逆位牌有 ${rv.length} / ${n} 張。</b>逆位代表能量受阻、過度或轉向內在；超過一半是逆位，表示問題可能主要來自內在的卡點，先調整自己的狀態再行動會更順利。`);
      else ov.push(`<b>有 ${rv.length} 張逆位</b>（${rv.map(x=>`「${x.pos}」`).join("、")}）。逆位代表能量受阻或轉向內在，這些位置就是你需要多花心思調整的地方，其餘正位的面向則相對順利。`);
    }
    if(court.length>=2)ov.push(`<b>出現 ${court.length} 張宮廷牌</b>（${court.map(x=>x.c.name).join("、")}）。宮廷牌通常代表人物或你扮演的角色，數量多表示這件事和身邊的人關係密切，他人的態度與互動會影響結果。`);
    const f=cs[0],l=cs[n-1];
    ov.push(`<b>從起點到終點：</b>「${f.pos}」的${nm(f)}代表「${kw(f)}」，最後落在「${l.pos}」的${nm(l)}，代表「${kw(l)}」。${f.c.maj===l.c.maj?"":l.c.maj?"結尾是一張大牌，表示事情最後會走向一個重要的轉折。":"結尾是一張小牌，表示最後的結果會回到日常層面、由你的行動決定。"}${!f.r&&l.r?"從正位走到逆位，提醒你後段要留意能量的消耗或阻礙。":f.r&&!l.r?"從逆位走到正位，代表一開始的卡點有機會被化解。":""}`);
  }
  const adv=cs.find(x=>/建議|對策|行動|運用|下一步|如何|激發/.test(x.pos))||cs[n-1];
  const advice=`${adv.pos==="指引"?"":`這次的建議主要看「${adv.pos}」這張${nm(adv)}。`}牌面中，${adv.c.scene.replace(/。$/,"")}，${adv.r?"而它以逆位出現，表示這股力量目前被卡住了。":"這正是你現在可以借用的姿態。"}${mean(adv)}${adv.r?"不妨先找出讓你卡住的原因，處理內在的阻礙，再一步步往前。":"帶著這份能量去行動，你會開始看到改變。"}`;
  const CN=["一","二","三","四","五","六","七","八","九","十","十一","十二"];
  const per=cs.map(x=>{const c=x.c,o=x.r?"逆位":"正位",d=c.deep,rows=x.r?d.rv:d.up;
    const el=c.maj?`<p><b>類別：</b>大阿爾克那・第 ${c.rank} 號</p>`
      :`<p><b>元素：</b>${"火水風土"[c.suit]}（${ELEMD[c.suit].split("：")[1]}）</p><p><b>${c.court?"階級":"數字"}：</b>${c.court?COURT[c.rank-10]:`${c.rank===0?"A":c.rank+1}，${NUMS[c.rank]}`}</p>`;
    return `<article class="rd2">
    <h4>${CN[x.i]}、${x.pos}｜${c.name}・${o}</h4>
    <div class="rtop"><img src="${c.img}" alt="${c.name}" loading="lazy"><div class="info">
      <p class="cn">${c.name}<span>${c.en}</span></p>${el}
      <p class="${x.r?"dim":"on"}"><b>正位：</b>${c.kwU}。</p>
      <p class="${x.r?"on":"dim"}"><b>逆位：</b>${c.kwR}。</p>
      <p><b>核心課題：</b>${d.core}。</p></div></div>
    <p>牌面中，${c.scene}</p>
    <p>${c.sym}</p>
    <p>值得注意的是，${d.detail}</p>
    <p>${x.r?`逆位時，這段旅程可能出現卡關、猶豫或轉向內在。${c.mR}`:`正位時，這些象徵的能量能順利發揮。${c.mU}`}</p>
    <p>${posFrame(x.pos)}${c.name}${o}出現在「${x.pos}」，提醒你留意「${(x.r?c.kwR:c.kwU).split("、").slice(0,2).join("」與「")}」在這個面向上的影響。</p>
    <h5>${c.name}${o}的四種解讀</h5>
    <p>${x.r?"逆位不一定代表負面的結果，也可能代表正在重新思考原本的決定。":"同一張牌在不同人身上會有不同的樣貌，以下是常見的四種解讀方向。"}</p>
    <table class="t4"><thead><tr><th>解讀方向</th><th>可能的內在狀態</th></tr></thead><tbody>${rows.map(r=>`<tr><td>${r[0]}</td><td>${r[1]}</td></tr>`).join("")}</tbody></table>
    <p>放在「${x.pos}」的位置，不妨想想哪一種最接近你現在的狀態。</p></article>`}).join("");
  return `<h3 class="rt">整體解讀</h3><div class="ovw">${ov.map(t=>`<p>${t}</p>`).join("")}</div>
    <h3 class="rt">判讀依據</h3><p class="hint">上面的整體解讀，是根據這些牌面資訊推論出來的：</p>${stats}
    <h3 class="rt">逐張解讀</h3>${per}
    <h3 class="rt">給你的建議</h3><div class="ovw adv"><p>${advice}</p></div>
    <p class="note">塔羅解讀僅供參考與自我覺察，重要決定請以自己的判斷為主。</p>`;
}

/* ---- spreads: [位置名稱, x, y, 是否橫放交叉] ---- */
const CATS=[
 {name:"基礎通用",list:[
  {name:"單張指引",sub:"一張牌，回應你此刻最需要的訊息。",pos:[["指引",1,1]]},
  {name:"時間之流",sub:"從過去、現在到未來，看見事情的走向。",pos:[["過去",1,1],["現在",2,1],["未來",3,1]]},
  {name:"狀況・行動・結果",sub:"看清現況，找出該做的事與可能的結果。",pos:[["目前狀況",1,1.5],["建議行動",2,1],["可能結果",3,1.5]]},
  {name:"身心靈",sub:"檢視身體、情緒與內在的平衡。",pos:[["身體",1,1],["心理",3,1],["靈性",2,1.8]]}]},
 {name:"決策分析",list:[
  {name:"二選一",sub:"比較兩個選擇各自的過程與結果。",pos:[["目前狀況",2,3],["選擇 A 的過程",1,2],["選擇 B 的過程",3,2],["選擇 A 的結果",1,1],["選擇 B 的結果",3,1]]},
  {name:"三選一",sub:"三條路並排比較，看哪一條最適合你。",pos:[["目前狀況",2,3],["選擇 A 的過程",1,2],["選擇 B 的過程",2,2],["選擇 C 的過程",3,2],["選擇 A 的結果",1,1],["選擇 B 的結果",2,1],["選擇 C 的結果",3,1]]},
  {name:"優缺分析",sub:"把一個決定的好處、代價與建議攤開來看。",pos:[["目前狀況",2,1],["優點",1,2],["缺點",3,2],["建議",2,3]]}]},
 {name:"經典進階",list:[
  {name:"凱爾特十字",sub:"最經典的完整牌陣，適合深入探討一個問題。",gap:28,pos:[["現況",2,2.5],["阻礙",2,2.5,1],["根源",2,3.5],["過去",1,2.5],["目標",2,1.5],["近未來",3,2.5],["自我",4.3,4],["環境",4.3,3],["希望與恐懼",4.3,2],["最終結果",4.3,1]]},
  {name:"六芒星",sub:"兩個交錯的三角形，看見事情的全貌。",pos:[["過去",2,1],["現在",3,3.2],["未來",1,3.2],["對策",2,4],["周遭環境",3,1.8],["內心期望",1,1.8],["最終結果",2,2.5]]},
  {name:"馬蹄形",sub:"像馬蹄一樣的 U 形，帶來好運與方向。",pos:[["過去",1,1],["現在",1,2],["未來",1,3],["建議",2,3.6],["周遭影響",3,3],["希望與恐懼",3,2],["最終結果",3,1]]}]},
 {name:"感情人際",list:[
  {name:"戀人金字塔",sub:"看見兩人的狀態與關係的未來。",pos:[["你",1,2],["對方",3,2],["關係現況",2,2],["未來發展",2,1]]},
  {name:"關係透視",sub:"比較雙方的想法與感受。",pos:[["你的想法",1,1],["對方的想法",4,1],["你的感受",1,2],["對方的感受",4,2],["關係現況",2.5,1],["未來發展",2.5,2]]},
  {name:"復合可能",sub:"了解分開的原因與重新開始的可能。",pos:[["目前關係",2,2],["你的現況",1,2],["對方的現況",3,2],["分開原因",2,1],["未來走向",2,3]]}]},
 {name:"自我探索",list:[
  {name:"天賦與潛能",sub:"找出你與生俱來的才能與發展方向。",pos:[["天生天賦",1,1.8],["潛在能力",2,1.3],["發展阻礙",3,1],["激發方式",4,1.3],["未來展現",5,1.8]]},
  {name:"陰影探索",sub:"溫柔地面對你不願看見的那一面。",pos:[["表面的我",1,1],["隱藏的陰影",3,1],["陰影的來源",2,2],["陰影的影響",1,3],["如何接納",3,3],["整合後的力量",2,4]]},
  {name:"人生使命",sub:"從根基往上，看見你的使命方向。",pos:[["現在的我",1,3],["核心價值",2,3],["天賦",3,3],["人生課題",4,3],["需要放下的",1.5,2],["下一步",3.5,2],["使命方向",2.5,1]]},
  {name:"行動力",sub:"一步一步往上，找到前進的動力。",pos:[["目標",1,2.6],["現有動力",2,2.2],["阻礙",3,1.8],["行動建議",4,1.4],["結果",5,1]]},
  {name:"隱藏資源",sub:"發掘你身邊還沒善用的資源。",pos:[["知識",1,1],["技能",2,1],["人脈",3,1],["物質資源",1,2],["未善用的資源",2,2],["運用方式",3,2]]}]},
 {name:"職涯財富",list:[
  {name:"工作現況",sub:"快速掌握目前工作的狀態。",pos:[["目前狀態",1,1],["優勢",2,1],["挑戰",1,2],["建議",2,2]]},
  {name:"職涯發展",sub:"看見職涯的低谷與轉機。",pos:[["現況",1,1],["阻礙",2,1.5],["轉機",3,2],["建議",4,1.5],["未來走向",5,1]]},
  {name:"財運牌陣",sub:"檢視收入、支出與未來的財運。",pos:[["目前財務",2,2],["收入來源",1,1],["支出狀況",3,1],["財務盲點",1,3],["未來財運",3,3]]}]},
 {name:"時間週期",list:[
  {name:"未來三個月",sub:"每個月一張牌，看見接下來的變化。",pos:[["第一個月",1,1],["第二個月",2,1],["第三個月",3,1]]},
  {name:"一週運勢",sub:"週一到週日，每天一張牌。",pos:[["週一",1,1],["週二",2,1],["週三",3,1],["週四",4,1],["週五",1.5,2],["週六",2.5,2],["週日",3.5,2]]},
  {name:"年度運勢",sub:"十二張牌環成一圈，對應一年十二個月。",pos:[["一月",1,1],["二月",2,1],["三月",3,1],["四月",4,1],["五月",4,2],["六月",4,3],["七月",4,4],["八月",3,4],["九月",2,4],["十月",1,4],["十一月",1,3],["十二月",1,2]]}]},
 {name:"自訂牌陣",custom:true,list:[]}
];
const customSpread=n=>({name:`自訂 ${n} 張牌陣`,sub:"依你選牌的順序排列。",pos:Array.from({length:n},(_,i)=>[`第 ${i+1} 張`,i%5+1,Math.floor(i/5)+1])});

const $=id=>document.getElementById(id);
const catEl=$("cat"),spEl=$("spreads"),pvEl=$("preview"),deckEl=$("deck"),btnR=$("reveal"),btnS=$("shuffle"),drawV=$("drawView"),readV=$("readView"),revEl=$("useRev");
let cat=4,sp=null,order=[],rev={},picked=[],revealed=false,customN=5;

/* ---- layout engine: places slots by (x,y) inside a stage, sized to its width ---- */
function layout(stage,spread,o){
  const W=stage.clientWidth||600,P=spread.pos,gx=spread.gap||o.gx;
  const maxX=Math.max(...P.map(p=>p[1])),maxY=Math.max(...P.map(p=>p[2]));
  let cw=Math.min(o.max,(W-(maxX-1)*gx)/maxX);if(o.maxH)cw=Math.min(cw,Math.max(o.minW||0,(o.maxH-maxY*o.label-(maxY-1)*o.gy-(o.pad||0))/(maxY*o.ratio)));const ch=cw*o.ratio,sh=o.label+ch,step=sh+o.gy;
  const total=(maxX-1)*(cw+gx)+cw,off=(W-total)/2;
  stage.style.height=((maxY-1)*step+sh+(o.pad||0))+"px";
  return P.map(p=>({left:off+(p[1]-1)*(cw+gx),top:(p[2]-1)*step,w:cw,h:ch}));
}
function drawPreview(){
  const box=layout(pvEl,sp,{max:96,ratio:1.25,label:0,gx:10,gy:10});
  pvEl.innerHTML=sp.pos.map((p,i)=>`<div class="mini${p[3]?" cross":""}" style="left:${box[i].left}px;top:${box[i].top}px;width:${box[i].w}px;height:${box[i].h}px"><b>${i+1}</b><span>${p[0]}</span></div>`).join("");
  $("pvSub").textContent=sp.sub;
}
const pvRO=new ResizeObserver(()=>{if(sp)drawPreview()});pvRO.observe(pvEl);

/* ---- category & spread pickers ---- */
catEl.innerHTML=CATS.map((c,i)=>`<option value="${i}">${c.name}</option>`).join("");
catEl.value=cat;
function renderSpreads(){
  const c=CATS[cat];
  if(c.custom){
    spEl.innerHTML=`<div class="dd"><select id="cn" aria-label="牌數">${Array.from({length:10},(_,i)=>`<option value="${i+1}"${i+1===customN?" selected":""}>${i+1} 張牌</option>`).join("")}</select></div>`;
    $("cn").addEventListener("change",e=>{customN=+e.target.value;setSpread(customSpread(customN))});
    setSpread(customSpread(customN));return;
  }
  spEl.innerHTML=c.list.map((s,i)=>`<label class="radio"><input type="radio" name="sp" value="${i}"${i===0?" checked":""}> ${s.name}<small>（${s.pos.length} 張）</small></label>`).join("");
  setSpread(c.list[0]);
}
spEl.addEventListener("change",e=>{if(e.target.name==="sp")setSpread(CATS[cat].list[+e.target.value])});
catEl.addEventListener("change",()=>{cat=+catEl.value;renderSpreads()});
function setSpread(s){sp=s;drawPreview();resetDraw(false)}

/* ---- deck ---- */


function shuffleData(){
  order=CARDS.map((_,i)=>i);
  for(let i=order.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[order[i],order[j]]=[order[j],order[i]]}
  rev={};CARDS.forEach((_,i)=>rev[i]=Math.random()<.5);
}
function renderDeck(){
  deckEl.innerHTML=order.map((ci,k)=>`<button class="back" data-ci="${ci}" aria-pressed="false" aria-label="第 ${k+1} 號牌">${MOON}<span class="no">${pad(k+1)}</span></button>`).join("");
  syncSel();
}
function syncSel(){
  deckEl.classList.toggle("locked",revealed);
  deckEl.querySelectorAll(".back").forEach(el=>{const n=picked.indexOf(+el.dataset.ci);
    el.classList.toggle("sel",n>-1);el.setAttribute("aria-pressed",n>-1);
    const o=el.querySelector(".ord");
    if(n>-1){if(o)o.textContent=n+1;else el.insertAdjacentHTML("beforeend",`<span class="ord">${n+1}</span>`)}else if(o)o.remove()});
  updateStatus();
}
function updateStatus(){
  const n=picked.length,N=sp.pos.length;
  $("drawTitle").textContent=`請抽出 ${N} 張牌（已選 ${n} / ${N}）`;
  $("drawHint").textContent=revealed?"已揭開牌陣。想再問一次，請按「重新洗牌」。":n===N?"已選滿，可以按下方「揭開塔羅牌」。":"點選牌卡即可選取，再點一次取消。";
  btnR.disabled=n!==N||revealed;
}
deckEl.addEventListener("click",e=>{
  const b=e.target.closest(".back");if(!b||revealed)return;
  const ci=+b.dataset.ci,i=picked.indexOf(ci),N=sp.pos.length;
  if(i>-1)picked.splice(i,1);else if(picked.length<N)picked.push(ci);else{$("drawHint").textContent=`這個牌陣只需要 ${N} 張牌，可先取消一張再選。`;return}
  syncSel();
});

/* ---- reveal ---- */
function drawSpread(animate){
  const el=$("spread"),useRev=revEl.checked;
  const box=layout(el,sp,{ratio:1.714,label:56,gx:14,gy:18,pad:10,maxH:Math.max(innerHeight*.85,680),minW:el.clientWidth>640?(el.clientWidth>1100?120:100):64,max:el.clientWidth>1100?180:150});
  el.innerHTML=sp.pos.map((p,i)=>{const ci=picked[i],c=CARDS[ci],r=useRev&&rev[ci],b=box[i];
    const head=p[3]?`<div class="pos" style="visibility:hidden">.</div><div class="ori" style="visibility:hidden">.</div>`
      :`<div class="pos">${i+1} ${p[0]}${sp.pos[i+1]&&sp.pos[i+1][3]?`・${i+2} ${sp.pos[i+1][0]}`:""}</div><div class="ori">${ori(r)}${sp.pos[i+1]&&sp.pos[i+1][3]?`・${ori(useRev&&rev[picked[i+1]])}`:""}</div>`;
    return `<div class="slot${p[3]?" cross":""}" style="left:${b.left}px;top:${b.top}px;width:${b.w}px">${head}
      <div class="c3" style="width:${b.w}px;height:${b.h}px"><div class="flip${animate?"":" shown"}" style="transition-delay:${i*160}ms"><div class="cb">${MOON}</div><div class="face"><img src="${c.img}" alt="${c.name}"></div></div></div></div>`}).join("");
  if(animate)requestAnimationFrame(()=>requestAnimationFrame(()=>el.querySelectorAll(".flip").forEach(f=>f.classList.add("shown"))));
}
const ori=r=>`<span class="${r?"rv":"up"}">${r?"逆位":"正位"}</span>`;
const esc=s=>s.replace(/[&<>"']/g,m=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[m]));
function reveal(){
  if(picked.length!==sp.pos.length)return;
  revealed=true;syncSel();
  const q=$("q").value.trim();
  $("rQ").textContent=q||"我的塔羅占卜";
  $("rFoot").textContent=`${sp.pos.length} 張牌・${revEl.checked?"正逆位":"全正位"}`;
  $("reading").innerHTML=buildReading(revEl.checked);
  if(location.hash!=="#reading")location.hash="reading";else showView();
}
let fadeT=0;
function showView(){
  const read=location.hash==="#reading"&&revealed;
  if(location.hash==="#reading"&&!revealed)location.hash="";
  const from=read?drawV:readV,to=read?readV:drawV,quick=from.hidden||matchMedia("(prefers-reduced-motion:reduce)").matches;
  const swap=()=>{
    from.hidden=true;from.classList.remove("fading");
    to.classList.add("fading");to.hidden=false;
    btnR.hidden=read;document.body.classList.toggle("reading",read);
    window.scrollTo(0,0);
    if(!read&&pendingShuffle){pendingShuffle=false;setTimeout(playShuffle,460)}
    if(read)drawSpread(true);
    requestAnimationFrame(()=>requestAnimationFrame(()=>to.classList.remove("fading")));
  };
  clearTimeout(fadeT);
  if(quick){swap();return}
  from.classList.add("fading");document.querySelector(".bar").classList.add("fading");
  fadeT=setTimeout(()=>{swap();document.querySelector(".bar").classList.remove("fading")},450);
}
window.addEventListener("hashchange",showView);
$("back").addEventListener("click",()=>{location.hash=""});
const spRO=new ResizeObserver(()=>{if(revealed&&!readV.hidden)drawSpread(false)});spRO.observe($("spread"));
let pendingShuffle=false;
function resetDraw(anim){
  shuffleData();picked=[];revealed=false;renderDeck();
  if(!anim)return;
  playShuffle();
  requestAnimationFrame(()=>deckEl.scrollIntoView({behavior:"smooth",block:"center"}));
}
function playShuffle(){
  deckEl.querySelectorAll(".back").forEach(b=>{b.style.setProperty("--dx",(Math.random()*60-30)+"px");b.style.setProperty("--dy",(Math.random()*30-15)+"px");b.style.setProperty("--dr",(Math.random()*30-15)+"deg")});
  deckEl.classList.remove("shuffling");void deckEl.offsetWidth;deckEl.classList.add("shuffling");
  setTimeout(()=>deckEl.classList.remove("shuffling"),600);
}
btnR.addEventListener("click",reveal);
btnS.addEventListener("click",()=>resetDraw(true));
$("again").addEventListener("click",()=>{resetDraw(false);pendingShuffle=true;location.hash=""});
renderSpreads();showView();
}

/* ---- 背景星空動畫：閃爍的星星 + 偶爾劃過的流星 ---- */
{
const cv=document.getElementById("stars"),cx=cv.getContext("2d"),still=matchMedia("(prefers-reduced-motion:reduce)").matches;
let W=0,H=0,dpr=1,stars=[],shoot=[],nextShoot=0,raf=0;
function size(){dpr=Math.min(devicePixelRatio||1,2);W=innerWidth;H=innerHeight;cv.width=W*dpr;cv.height=H*dpr;cx.setTransform(dpr,0,0,dpr,0,0);
  const n=Math.round(W*H/7000);stars=Array.from({length:n},()=>({x:Math.random()*W,y:Math.random()*H,r:Math.random()<.12?1.6+Math.random():.5+Math.random()*.9,a:.25+Math.random()*.6,p:Math.random()*6.3,s:.4+Math.random()*1.4,v:.02+Math.random()*.05}));
  if(still)frame(0)}
function launch(t){const fromTop=Math.random()<.65;
  shoot.push({x:fromTop?W*(.3+Math.random()*.8):W+20,y:fromTop?-20:H*Math.random()*.4,vx:-(5+Math.random()*4),vy:2.2+Math.random()*2.2,len:110+Math.random()*120,life:0,max:70+Math.random()*40});
  nextShoot=t+3500+Math.random()*6000}
function frame(t){
  cx.clearRect(0,0,W,H);
  for(const s of stars){
    if(!still){s.y+=s.v;if(s.y>H+2){s.y=-2;s.x=Math.random()*W}}
    const a=still?s.a:s.a*(.55+.45*Math.sin(t/1000*s.s+s.p));
    cx.globalAlpha=a;cx.fillStyle="#e9dbb4";cx.beginPath();cx.arc(s.x,s.y,s.r,0,6.283);cx.fill();
    if(s.r>1.6){cx.globalAlpha=a*.25;cx.beginPath();cx.arc(s.x,s.y,s.r*3,0,6.283);cx.fill()}
  }
  cx.globalAlpha=1;
  if(!still){
    if(!nextShoot)nextShoot=t+1500;
    if(t>nextShoot)launch(t);
    shoot=shoot.filter(m=>m.life<m.max);
    for(const m of shoot){m.life++;m.x+=m.vx;m.y+=m.vy;
      const f=Math.min(1,m.life/10)*(1-m.life/m.max),k=m.len/Math.hypot(m.vx,m.vy);
      const g=cx.createLinearGradient(m.x,m.y,m.x-m.vx*k,m.y-m.vy*k);
      g.addColorStop(0,`rgba(255,246,220,${.95*f})`);g.addColorStop(1,"rgba(255,246,220,0)");
      cx.strokeStyle=g;cx.lineWidth=1.6;cx.lineCap="round";cx.beginPath();cx.moveTo(m.x,m.y);cx.lineTo(m.x-m.vx*k,m.y-m.vy*k);cx.stroke();
      cx.globalAlpha=f;cx.fillStyle="#fff8e6";cx.beginPath();cx.arc(m.x,m.y,1.8,0,6.283);cx.fill();cx.globalAlpha=1}
    raf=requestAnimationFrame(frame);
  }
}
addEventListener("resize",()=>{cancelAnimationFrame(raf);size();if(!still)raf=requestAnimationFrame(frame)});
document.addEventListener("visibilitychange",()=>{cancelAnimationFrame(raf);if(!document.hidden&&!still)raf=requestAnimationFrame(frame)});
size();if(!still)raf=requestAnimationFrame(frame);
}

/* ---- 背景音樂：用 Web Audio 即時生成的平靜純音樂（和弦長音 + 輕柔鐘聲） ---- */
{
const btns=[...document.querySelectorAll(".mbtn")];
let ac=null,master,bus,timer=0,on=false,ci=0,nextC=0,nextB=0;
const hz=n=>440*2**((n-69)/12);
const CHORDS=[[50,57,62,66,69],[47,54,59,62,66],[43,50,55,59,66],[45,52,57,61,64]];
const BELL=[74,76,78,81,83,86];
function init(){
  ac=new (window.AudioContext||window.webkitAudioContext)();
  master=ac.createGain();master.gain.value=0;
  const lp=ac.createBiquadFilter();lp.type="lowpass";lp.frequency.value=2200;
  master.connect(lp).connect(ac.destination);
  const rev=ac.createConvolver(),len=ac.sampleRate*5,buf=ac.createBuffer(2,len,ac.sampleRate);
  for(let c=0;c<2;c++){const d=buf.getChannelData(c);for(let i=0;i<len;i++)d[i]=(Math.random()*2-1)*(1-i/len)**3}
  rev.buffer=buf;const wet=ac.createGain();wet.gain.value=.7;rev.connect(wet).connect(master);
  bus=ac.createGain();bus.connect(master);bus.connect(rev);
}
function pad(t,notes,dur){
  const f=ac.createBiquadFilter();f.type="lowpass";f.frequency.value=900;f.connect(bus);
  const g=ac.createGain();g.gain.setValueAtTime(0,t);g.gain.linearRampToValueAtTime(.045,t+3);g.gain.setValueAtTime(.045,t+dur-3.5);g.gain.linearRampToValueAtTime(0,t+dur);g.connect(f);
  notes.forEach(n=>["sine","triangle"].forEach((type,k)=>{const o=ac.createOscillator();o.type=type;o.frequency.value=hz(n);o.detune.value=k?5:-5;o.connect(g);o.start(t);o.stop(t+dur+.1)}));
}
function bell(t,n){
  const g=ac.createGain();g.gain.setValueAtTime(0,t);g.gain.linearRampToValueAtTime(.05,t+.02);g.gain.exponentialRampToValueAtTime(.0001,t+3.2);g.connect(bus);
  [[1,1],[2,.25]].forEach(([m,v])=>{const o=ac.createOscillator(),og=ac.createGain();o.frequency.value=hz(n)*m;og.gain.value=v;o.connect(og).connect(g);o.start(t);o.stop(t+3.3)});
}
function tick(){
  const t=ac.currentTime;
  while(nextC<t+1){pad(nextC,CHORDS[ci++%CHORDS.length],10);nextC+=8}
  while(nextB<t+1){if(Math.random()<.75)bell(nextB,BELL[Math.floor(Math.random()*BELL.length)]);nextB+=1.4+Math.random()*2.8}
}
async function start(){
  try{if(!ac)init();await ac.resume()}catch(e){return}
  const t=ac.currentTime;nextC=Math.max(nextC,t+.1);nextB=Math.max(nextB,t+2);
  clearInterval(timer);timer=setInterval(tick,250);tick();
  master.gain.cancelScheduledValues(t);master.gain.setValueAtTime(master.gain.value,t);master.gain.linearRampToValueAtTime(.55,t+3);
  set(true);
}
function stop(){
  if(!ac)return;const t=ac.currentTime;
  master.gain.cancelScheduledValues(t);master.gain.setValueAtTime(master.gain.value,t);master.gain.linearRampToValueAtTime(0,t+1.5);
  clearInterval(timer);setTimeout(()=>{if(!on)ac.suspend()},1700);set(false);
}
function set(v){on=v;btns.forEach(b=>{b.classList.toggle("on",v);b.setAttribute("aria-pressed",v);b.querySelector("span").textContent=v?"音樂 開":"音樂 關"});
  try{localStorage.setItem("tarot-music",v?"1":"0")}catch(e){}}
btns.forEach(b=>b.addEventListener("click",e=>{e.stopPropagation();on?stop():start()}));
let pref=null;try{pref=localStorage.getItem("tarot-music")}catch(e){}
if(pref==="1"){const once=()=>{document.removeEventListener("pointerdown",once,true);if(!on)start()};document.addEventListener("pointerdown",once,true)}
}
</script>
</body>
</html>
