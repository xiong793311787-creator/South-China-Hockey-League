# South-China-Hockey-League
一个网站
import { useState, useRef } from "react";
// ============================================================
// DESIGN TOKENS - SCHL Red/White/Black/Gold
// ============================================================
const CSS = `
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Barlow+Condensed:wght
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
:root{
--red:#D92B2B; --red2:#B01F1F; --red3:#FF4444;
--gold:#F4C542; --gold2:#E8A800;
--black:#111111; --black2:#1A1A1A; --black3:#222222;
--white:#FFFFFF; --white2:#F5F5F5; --white3:#E8E8E8;
--gray:#8A8A8A; --gray2:#BBBBBB; --gray3:#EEEEEE;
--text-on-red:#FFFFFF; --text-on-white:#111111;
--border-light:rgba(0,0,0,0.10); --border-dark:rgba(255,255,255,0.12);
--r:14px; --r2:20px;
}
html,body{background:var(--white2);color:var(--black);font-family:'Noto Sans SC',sans-serif;m
.app{max-width:430px;margin:0 auto;min-height:100vh;background:var(--white2);position:relativ
::-webkit-scrollbar{width:3px;height:3px;} ::-webkit-scrollbar-track{background:transparent;}
.bebas{font-family:'Bebas Neue',sans-serif;letter-spacing:.06em;}
.barlow{font-family:'Barlow Condensed',sans-serif;}
/* NAV */
.nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:430px;max-width:100vw;
.nb{display:flex;flex-direction:column;align-items:center;gap:3px;background:none;border:none
.nb.on{color:var(--red);}
.nb svg{width:22px;height:22px;}
.nb-lbl{font-size:10px;font-weight:700;}
/* HERO HEADER - RED TOP */
.hero-header{background:var(--red);padding:52px 20px 28px;position:relative;overflow:hidden;}
.hero-header::after{content:'';position:absolute;bottom:-1px;left:0;right:0;height:28px;backg
.hero-bg-pattern{position:absolute;inset:0;opacity:.07;background-image:repeating-linear-grad
.hero-logo-row{display:flex;align-items:center;gap:14px;position:relative;}
.schl-logo-svg{width:72px;height:72px;filter:drop-shadow(0 4px 12px rgba(0,0,0,.4));}
.hero-text{}
.hero-league{font-family:'Bebas Neue',sans-serif;font-size:13px;color:rgba(255,255,255,.7);le
.hero-title{font-family:'Bebas Neue',sans-serif;font-size:38px;color:#fff;line-height:1;lette
.hero-season{font-size:11px;color:rgba(255,255,255,.6);letter-spacing:.2em;margin-top:2px;}
/* WHITE BODY */
.white-body{background:var(--white2);padding:16px 0 0;}
/* CARDS */
.card-w{background:var(--white);border-radius:var(--r);overflow:hidden;box-shadow:0 2px 12px
.card-r{background:var(--red);border-radius:var(--r);overflow:hidden;}
.card-dark{background:var(--black2);border-radius:var(--r);overflow:hidden;}
/* SECTION */
.sec{padding:16px 16px;}
.sec-ttl{font-family:'Bebas Neue',sans-serif;font-size:20px;color:var(--red);letter-spacing:.
.sec-ttl::after{content:'';flex:1;height:2px;background:linear-gradient(90deg,var(--red),tran
/* TABS */
.tabs{display:flex;gap:0;background:var(--white3);border-radius:10px;padding:4px;overflow:hid
.tab{flex:1;padding:8px 4px;border:none;background:none;color:var(--gray);border-radius:8px;c
.tab.on{background:var(--red);color:#fff;}
/* LIVE BADGE */
.live-badge{background:var(--red);color:#fff;font-size:9px;font-weight:900;padding:3px @keyframes pulse{0%,100%{opacity:1}50%{opacity:.5}}
8px;bo
/* SCORE CARD */
.score-card-hero{background:linear-gradient(135deg,var(--black) 0%,var(--black2) 100%);border
.score-team-name{font-size:13px;font-weight:900;color:#fff;}
.score-num{font-family:'Bebas Neue',sans-serif;font-size:52px;color:#fff;line-height:1;}
.score-num.leading{color:var(--gold);text-shadow:0 0 20px rgba(244,197,66,.5);}
/* STANDINGS */
.srow{display:flex;align-items:center;padding:11px 14px;border-bottom:1px solid var(--border-
.srow:hover{background:var(--white3);}
.srow:last-child{border-bottom:none;}
.rnk{font-family:'Bebas Neue',sans-serif;font-size:18px;color:var(--gray);width:22px;text-ali
.rnk.t1{color:var(--gold);font-size:22px;}
.rnk.t2{color:#A8A8A8;}
.rnk.t3{color:#CD7F32;}
.stn{flex:1;font-size:13px;font-weight:700;color:var(--black);}
.stn span{font-size:10px;color:var(--gray);font-weight:400;display:block;}
.sc{font-family:'Barlow Condensed',sans-serif;font-size:14px;color:var(--gray);text-align:cen
.sc.pts{color:var(--red);font-weight:900;font-size:17px;}
/* PLAYER ROW */
.prow{display:flex;align-items:center;padding:11px 14px;border-bottom:1px solid var(--border-
.prow:hover{background:var(--white3);}
.prow:last-child{border-bottom:none;}
.prank{font-family:'Bebas Neue',sans-serif;font-size:20px;color:var(--gray);width:24px;}
.prank.t1{color:var(--gold);}
.prank.t2{color:#A8A8A8;}
.prank.t3{color:#CD7F32;}
.pnm{flex:1;}
.pnm-main{font-size:14px;font-weight:900;color:var(--black);}
.pnm-sub{font-size:11px;color:var(--gray);}
.pstat{font-family:'Barlow Condensed',sans-serif;font-size:22px;font-weight:900;color:var(--r
/* POS BADGE */
.pos{font-size:10px;font-weight:900;width:32px;height:32px;border-radius:8px;display:flex;ali
/* SEARCH */
.srch-inp{width:100%;background:var(--white);border:2px solid var(--white3);border-radius:30p
.srch-inp:focus{border-color:var(--red);}
.sri{display:flex;align-items:center;padding:12px 16px;cursor:pointer;border-bottom:1px solid
.sri:hover{background:var(--white3);}
.sri:last-child{border-bottom:none;}
.sri-type{font-size:9px;padding:3px 8px;border-radius:6px;background:var(--red);color:#fff;wh
/* PLAYER HERO */
.phero{position:relative;min-height:240px;display:flex;flex-direction:column;justify-content:
.phero-num{position:absolute;right:-16px;top:-24px;font-family:'Bebas Neue',sans-serif;font-s
.phero-name{font-family:'Bebas Neue',sans-serif;font-size:46px;line-height:1;position:relativ
.phero-pos{font-size:11px;letter-spacing:.3em;color:rgba(255,255,255,.7);margin-bottom:6px;po
/* STAT GRID */
.sgrid{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--border-light
.sbox{background:var(--white);padding:16px 8px;text-align:center;}
.sbox-v{font-family:'Bebas Neue',sans-serif;font-size:30px;line-height:1;}
.sbox-l{font-size:10px;color:var(--gray);letter-spacing:.08em;margin-top:4px;text-transform:u
/* BADGE */
.badge-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;padding:16px;}
.badge-ic{width:60px;height:60px;border-radius:50%;display:flex;align-items:center;justify-co
.badge-ic.on{box-shadow:0 0 16px currentColor;}
.badge-ic.off{filter:grayscale(1);opacity:.35;}
.badge-nm{font-size:9px;color:var(--gray);text-align:center;margin-top:4px;}
/* TEAM HERO */
.thero{padding:36px 20px 24px;position:relative;overflow:hidden;}
.thero-logo{font-size:64px;margin-bottom:8px;position:relative;}
.thero-name{font-family:'Bebas Neue',sans-serif;font-size:38px;line-height:1;position:relativ
/* GAME MINI */
.gmini{display:flex;align-items:center;gap:8px;padding:11px 14px;border-bottom:1px solid var(
.gmini:hover{background:var(--white3);}
.gmini:last-child{border-bottom:none;}
/* TIMELINE */
.tl{display:flex;gap:12px;padding:10px 16px;border-left:3px solid #eee;margin-left:32px;posit
.tl::before{content:'';position:absolute;left:-6px;top:14px;width:10px;height:10px;border-rad
.tl.goal{border-left-color:var(--red);}
.tl.goal::before{background:var(--red);box-shadow:0 0 8px var(--red);}
.tl.pen{border-left-color:var(--gold2);}
.tl.pen::before{background:var(--gold2);}
.tl-time{font-family:'Bebas Neue',sans-serif;font-size:20px;color:var(--gray);min-width:34px;
.tl-type{font-size:10px;letter-spacing:.12em;font-weight:700;}
.tl-main{font-size:14px;font-weight:900;margin-top:2px;cursor:pointer;color:var(--black);}
.tl-main:hover{color:var(--red);}
.tl-sub{font-size:12px;color:var(--gray);cursor:pointer;}
.tl-sub:hover{color:var(--red);}
/* SHOP */
.shop-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;padding:0 16px;}
.shcard{background:var(--white);border-radius:var(--r);overflow:hidden;cursor:pointer;transit
.shcard:hover{transform:translateY(-2px);box-shadow:0 8px 24px rgba(217,43,43,.2);}
.shimg{height:96px;display:flex;align-items:center;justify-content:center;font-size:44px;back
.shinfo{padding:10px;}
.shnm{font-size:12px;font-weight:700;line-height:1.3;color:var(--black);}
.shprice{font-family:'Bebas Neue',sans-serif;font-size:20px;color:var(--red);margin-top:4px;}
/* PRODUCT */
.pm-img{height:200px;display:flex;align-items:center;justify-content:center;font-size:80px;ba
.pm-thumb{width:56px;height:56px;border-radius:8px;background:var(--white3);display:flex;alig
.pm-thumb.on{border-color:var(--red);}
.atcbtn{background:var(--red);color:#fff;border:none;border-radius:30px;padding:15px 32px;fon
.atcbtn:hover{background:var(--red2);}
/* FLOATING CART */
.cart-fab{position:fixed;bottom:96px;right:calc(50vw - 200px);width:52px;height:52px;border-r
.cart-fab:hover{transform:scale(1.1);}
.cart-cnt{position:absolute;top:-4px;right:-4px;background:var(--gold);color:var(--black);bor
/* CART DRAWER */
.cart-ovr{position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:500;}
.cart-drw{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:430px;max-width:1
/* TOAST */
.toast{position:fixed;top:20px;left:50%;transform:translateX(-50%);background:var(--red);colo
@keyframes tin{from{opacity:0;top:0}to{opacity:1;top:20px}}
/* CHIP */
.chip{padding:4px 10px;border-radius:20px;font-size:11px;font-weight:700;}
.back-btn{background:none;border:none;color:var(--gray);cursor:pointer;display:flex;align-ite
/* RESULT */
.rW{color:var(--red);font-weight:900;} .rL{color:var(--gray);} .rT{color:var(--gray);}
/* SCHEDULE */
.sch-card{background:var(--white);border-radius:var(--r);padding:14px;margin-bottom:8px;borde
.sch-future{border-left-color:var(--gold2);}
/* MY PAGE */
.my-hero{background:var(--red);padding:52px 20px 36px;position:relative;overflow:hidden;}
.my-hero::after{content:'';position:absolute;bottom:-1px;left:0;right:0;height:28px;backgroun
.my-avatar{width:88px;height:88px;border-radius:50%;background:var(--black);border:4px solid
.my-avatar img{width:100%;height:100%;object-fit:cover;}
/* RANKING TABLE */
.rank-table{width:100%;border-collapse:collapse;}
.rank-table th{font-size:11px;color:var(--gray);padding:6px 8px;text-align:center;font-weight
.rank-table td{padding:10px 8px;text-align:center;border-bottom:1px solid var(--border-light)
.rank-table tr.me td{background:rgba(217,43,43,.06);font-weight:900;color:var(--red);}
.rank-table .rank-n{font-family:'Bebas Neue',sans-serif;font-size:18px;}
/* HISTORY TABLE */
.hist-wrap{overflow-x:auto;}
.hist-table{border-collapse:collapse;min-width:540px;width:100%;}
.hist-table th{font-size:10px;color:#fff;padding:8px 10px;background:var(--red);white-space:n
.hist-table td{padding:10px 10px;border-bottom:1px solid var(--border-light);text-align:cente
.hist-table tr:hover td{background:var(--white3);}
/* PROMO */
.promo{margin:0 16px 16px;border-radius:var(--r);background:linear-gradient(135deg,var(--blac
/* QTY */
.qty-btn{background:var(--white3);border:none;color:var(--black);width:32px;height:32px;borde
/* GC */
.gcA{color:var(--gold);} .gcB{color:var(--gray2);} .gcC{color:var(--red);}
@keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(
.fin{animation:fadeIn .25s ease;}
/* UPLOAD AREA */
.upload-area{border:2px dashed var(--gray2);border-radius:var(--r);padding:20px;text-align:ce
.upload-area:hover{border-color:var(--red);}
/* FUTURE GAME TAG */
.future-tag{background:var(--gold);color:var(--black);font-size:9px;font-weight:900;padding:2
`;
// ============================================================
// DATA
// ============================================================
const TEAMS = [
{id:"WH", name:"武汉冰⻰", nameEn:"Wuhan Ice Dragons", group:"A",city:"武汉",primary:"#C810
{id:"SHF",name:"上海⻜扬", nameEn:"Shanghai Flyers", group:"A",city:"上海",primary:"#0030
{id:"SHG",name:"上海国⾦", nameEn:"Shanghai Gold", group:"A",city:"上海",primary:"#8B69
{id:"KL", name:"昆仑鸿星", nameEn:"Kunlun Red Star", group:"B",city:"北京",primary:"#CC00
{id:"SZ", name:"深圳威⻰", nameEn:"Shenzhen Mighty", group:"B",city:"深圳",primary:"#0052
{id:"GZ", name:"⼴州闪电", nameEn:"Guangzhou Thunder", group:"B",city:"⼴州",primary:"#F4A5
{id:"CD", name:"成都勇⼠", nameEn:"Chengdu Warriors", group:"C",city:"成都",primary:"#0064
{id:"CQ", name:"重庆王⼦", nameEn:"Chongqing Princes", group:"C",city:"重庆",primary:"#4B00
{id:"HK", name:"⾹港国王", nameEn:"Hong Kong Kings", group:"C",city:"⾹港",primary:"#2C3E
];
// deterministic pseudo-random
const rng=(s)=>{ let x=Math.sin(s)*99999; return x-Math.floor(x); };
const FIRST=["张","李","王","刘","陈","杨","赵","⻩","周","吴","徐","孙","朱","⻢","胡","郭","何","
const LAST= ["伟","鑫","磊","浩","昊","宇","超","涛","明","亮","峰","强","⻜","⻰","⻁","雷","毅","
const POSITIONS=["LW","C","RW","D","D","D"];
const PC={LW:"#0088CC",C:"#D92B2B",RW:"#E8A800",D:"#555555",G:"#7B2FBE"};
const GC={A:"#E8A800",B:"#888888",C:"#D92B2B"};
// Build players — fixed stars first
const FIXED_PLAYERS = [
{id:"P_WZM",name:"王梓萌",teamId:"WH",position:"LW",number:87,gp:5,g:19,a:28,pts:47,pim:6,he
{id:"P_CYK",name:"陈芸楷",teamId:"WH",position:"C", number:17,gp:5,g:24,a:18,pts:42,pim:46,h
{id:"P_XTY",name:"熊天逸",teamId:"WH",position:"D",number:93,gp:5,g:15,a:22,pts:37,pim:10,h
];
const ALL_PLAYERS = [...FIXED_PLAYERS];
const FIXED_IDS = new Set(["P_WZM","P_CYK","P_XTY"]);
let _pid=0;
TEAMS.forEach((team,ti)=>{
// 6 skaters (skip first 3 slots for WH which are fixed)
const start = team.id==="WH" ? 3 : 0;
for(let i=start;i<6;i++){
const s=rng(ti*100+i);
const g=Math.floor(s*18)+2;
const a=Math.floor(rng(ti*100+i+50)*24)+2;
const pim=Math.floor(rng(ti*100+i+30)*12)*2;
ALL_PLAYERS.push({
id:`P${String(++_pid).padStart(3,"0")}`,
name:FIRST[(ti*7+i)%20]+LAST[(ti*5+i*3)%20],
teamId:team.id, position:POSITIONS[i%POSITIONS.length],
number:10+i+ti, gp:5, g, a, pts:g+a, pim,
height:170+Math.floor(rng(ti*100+i+10)*20),
weight:70+Math.floor(rng(ti*100+i+20)*30),
age:18+Math.floor(rng(ti*100+i+40)*14),
isGoalie:false
});
}
// 1 goalie
const s=rng(ti*200+99);
const shots=130+Math.floor(s*70);
const saves=shots-Math.floor(rng(ti*200+98)*18);
ALL_PLAYERS.push({
id:`G${String(ti+1).padStart(3,"0")}`,
name:FIRST[(ti*3+6)%20]+LAST[(ti*4+8)%20],
teamId:team.id, position:"G", number:ti+1,
gp:5, g:0, a:Math.floor(s*2), pts:0, pim:Math.floor(rng(ti*200+97)*3)*2,
height:180+Math.floor(s*10), weight:82+Math.floor(rng(ti*200+96)*18),
age:20+Math.floor(rng(ti*200+95)*14), isGoalie:true,
shots, saves, savePct:(saves/shots).toFixed(3), gaa:(2+rng(ti*200+94)*2).toFixed(2)
});
});
// 25 games — interleaved so no team plays back-to-back
const MATCHUPS=[
["WH","SHF"],["KL","GZ"],["CD","HK"],["SHG","SZ"],["CQ","SHF"],
["WH","KL"],["GZ","CD"],["SHF","SHG"],["HK","CQ"],["SZ","WH"],
["GZ","SHG"],["CD","KL"],["WH","GZ"],["SHF","HK"],["CQ","SZ"],
["SHG","WH"],["KL","HK"],["CD","CQ"],["SZ","GZ"],["WH","CD"],
["HK","SHG"],["SHF","KL"],["GZ","CQ"],["SZ","SHF"],["WH","HK"],
];
const INFRACTIONS=["⼲扰","绊倒","钩球","⾼杆","粗野⾏为","延误⽐赛","争议判罚","肘击"];
const buildGameEvents=(home,away,homeScore,awayScore,gi)=>{
const totalGoals=homeScore+awayScore;
const penCount=3+Math.floor(rng(gi*97)*4);
const homeSk=ALL_PLAYERS.filter(p=>p.teamId===home&&!p.isGoalie);
const awaySk=ALL_PLAYERS.filter(p=>p.teamId===away&&!p.isGoalie);
const times=Array.from({length:totalGoals+penCount},(_,i)=>Math.floor(rng(gi*88+i)*59)+1).s
let hg=0,ag=0;
const events=[];
// interleave goals between teams
const homeGoalSlots=[],awayGoalSlots=[];
for(let i=0;i<homeScore;i++) homeGoalSlots.push(i);
for(let i=0;i<awayScore;i++) awayGoalSlots.push(i);
let hi=0,ai=0;
for(let i=0;i<totalGoals;i++){
const needHome=hi<homeScore, needAway=ai<awayScore;
let isH;
if(needHome&&needAway) isH=i%2===0; // alternate
else isH=needHome;
const pool=isH?homeSk:awaySk;
const scorer=pool[Math.floor(rng(gi*70+i)*pool.length)];
const rest=pool.filter(p=>p.id!==scorer.id);
const assister=rest.length?rest[Math.floor(rng(gi*60+i)*rest.length)]:null;
if(isH)hi++;else ai++;
events.push({type:"goal",time:times[i],team:isH?home:away,scorer:scorer.name,scorerId:sco
}
// penalties alternated too
for(let i=0;i<penCount;i++){
const isH=i%2===0;
const pool=isH?homeSk:awaySk;
const player=pool[Math.floor(rng(gi*40+i)*pool.length)];
events.push({type:"penalty",time:times[totalGoals+i],team:isH?home:away,player:player.nam
}
return events.sort((a,b)=>a.time-b.time);
};
const PAST_DATES=["2025-01-12","2025-01-19","2025-01-26","2025-02-02","2025-02-09",
"2025-02-16","2025-02-23","2025-03-02","2025-03-09","2025-03-16",
"2025-03-23","2025-03-30","2025-04-06","2025-04-13","2025-04-20"];
const GAMES=MATCHUPS.map(([home,away],gi)=>{
const s=rng(gi*999);
const homeScore=Math.floor(s*5)+1;
const awayScore=Math.floor(rng(gi*998)*4);
return{
id:`G${String(gi+1).padStart(3,"0")}`,
homeTeam:home,awayTeam:away,homeScore,awayScore,
date:PAST_DATES[gi%15],
arena:TEAMS.find(t=>t.id===home).arena,
events:buildGameEvents(home,away,homeScore,awayScore,gi),
status:"final"
};
});
// Future schedule (10 games)
const FUTURE_MATCHUPS=[
["SHF","WH","2025-05-18","上海银联冰场"],
["KL","SHG","2025-05-18","五棵松体育馆"],
["HK","GZ","2025-05-25","将军澳冰场"],
["WH","CD","2025-05-25","武汉冰上运动中⼼"],
["CQ","KL","2025-06-01","重庆冰雪世界"],
["SZ","HK","2025-06-01","深圳冰纷万象冰场"],
["SHG","WH","2025-06-08","国⾦中⼼冰场"],
["GZ","SHF","2025-06-08","⼴州天河冰场"],
["CD","SZ","2025-06-15","成都⻄岭冰场"],
["WH","CQ","2025-06-15","武汉冰上运动中⼼"],
];
const getTeamGames=(tid)=>GAMES.filter(g=>g.homeTeam===tid||g.awayTeam===tid).sort((a,b)=>a.d
const getTeamRecord=(tid)=>{
const gs=GAMES.filter(g=>g.homeTeam===tid||g.awayTeam===tid);
let w=0,l=0,t=0,pts=0,gf=0,ga=0;
gs.forEach(g=>{const ih=g.homeTeam===tid;const ts=ih?g.homeScore:g.awayScore,os=ih?g.awaySc
return{w,l,t,pts,gp:gs.length,gf,ga,diff:gf-ga};
};
const STANDINGS=TEAMS.map(t=>({...t,...getTeamRecord(t.id)})).sort((a,b)=>b.pts-a.pts||b.diff
// Build global rankings
const buildRankings=()=>{
const sk=[...ALL_PLAYERS].filter(p=>!p.isGoalie);
const byG=[...sk].sort((a,b)=>b.g-a.g||b.pts-a.pts);
const byA=[...sk].sort((a,b)=>b.a-a.a||b.pts-a.pts);
const byPim=[...sk].sort((a,b)=>b.pim-a.pim);
const gRank={},aRank={},pimRank={};
byG.forEach((p,i)=>gRank[p.id]=i+1);
byA.forEach((p,i)=>aRank[p.id]=i+1);
byPim.forEach((p,i)=>pimRank[p.id]=i+1);
return{byG,byA,byPim,gRank,aRank,pimRank};
};
const RANKINGS=buildRankings();
// Get surrounding 3 players for a stat
const getSurrounding=(byList,pid,n=3)=>{
const idx=byList.findIndex(p=>p.id===pid);
if(idx<0)return[];
const start=Math.max(0,Math.min(idx-1,byList.length-n));
return byList.slice(start,start+n).map((p,i)=>({...p,rank:start+i+1,isMe:p.id===pid}));
};
// Per-player game log (filter game events for this player)
const getPlayerGameLog=(pid)=>{
const player=ALL_PLAYERS.find(p=>p.id===pid);
if(!player)return[];
return getTeamGames(player.teamId).map(g=>{
const ih=g.homeTeam===player.teamId;
const opp=TEAMS.find(t=>t.id===(ih?g.awayTeam:g.homeTeam));
const ts=ih?g.homeScore:g.awayScore, os=ih?g.awayScore:g.homeScore;
const res=ts>os?"W":ts<os?"L":"T";
const goals=g.events.filter(e=>e.type==="goal"&&e.scorerId===pid).length;
const assists=g.events.filter(e=>e.type==="goal"&&e.assisterId===pid).length;
const pim=g.events.filter(e=>e.type==="penalty"&&e.playerId===pid).reduce((s,e)=>s+e.minu
return{gameId:g.id,date:g.date,opp:opp?.name,oppLogo:opp?.logo,ts,os,res,goals,assists,pi
});
};
// Shop
const SHOP_ITEMS=[
{id:"S001",name:"精英系列球⾐", price:688,tier:"elite",images:[" "," "," "],desc:"采⽤⾼性
{id:"S002",name:"总冠军勋章相框",price:299,tier:"all", images:[" "," "," "],desc:"精致合⾦
{id:"S003",name:"球队⻢克杯", price:99, tier:"all", images:[" "," "," "],desc:"陶瓷⻢克
{id:"S004",name:"武汉站纪念章", price:199,tier:"all", images:[" "," "," "],desc:"限量武汉
{id:"S005",name:"头盔贴纸套装", price:49, tier:"all", images:[" "," "," "],desc:"⾼清防⽔
{id:"S006",name:"精英⾦章球员包",price:458,tier:"elite",images:[" "," "," "],desc:"专属精英
{id:"S007",name:"SCHL官⽅球棒", price:1288,tier:"all", images:[" "," "," "],desc:"碳纤维材
{id:"S008",name:"守⻔员头盔签名版",price:899,tier:"all",images:[" "," "," "],desc:"ABS⾼强度
];
// ============================================================
// SVG LOGO
// ============================================================
function SchlLogo({size=64}){
return(
<svg width={size} height={size} viewBox="0 0 120 120" xmlns="http://www.w3.org/2000/svg">
<defs>
<filter id="ds"><feDropShadow dx="0" dy="2" stdDeviation="3" floodOpacity=".4"/></fil
</defs>
{/* Outer badge shape */}
<path d="M60,6 L88,14 L108,34 L112,60 L108,86 L88,106 L60,114 L32,106 L12,86 L8,60 L12,
<path d="M60,10 L86,17 L104,36 L108,60 L104,84 L86,103 L60,110 L34,103 L16,84 L12,60 L1
<path d="M60,16 L84,22 L100,38 L104,60 L100,82 L84,98 L60,104 L36,98 L20,82 L16,60 L20,
{/* Inner */}
<rect x="26" y="36" width="68" height="48" rx="6" fill="#111"/>
{/* SCH text */}
<text x="60" y="68" fontFamily="Arial Black,sans-serif" fontSize="28" fontWeight="900"
{/* Stars */}
<text x="40" y="34" fontSize="12" textAnchor="middle">★</text>
<text x="60" y="30" fontSize="12" textAnchor="middle" fill="#F4C542">★</text>
<text x="80" y="34" fontSize="12" textAnchor="middle">★</text>
{/* LEAGUE bar */}
<rect x="34" y="76" width="52" height="14" rx="3" fill="#F4C542"/>
<text x="60" y="87" fontFamily="Arial Black,sans-serif" fontSize="9" fontWeight="900" f
</svg>
);
}
// ============================================================
// SMALL COMPONENTS
// ============================================================
function Toast({msg,done}){
const[v,setV]=useState(true);
if(!v)return null;
setTimeout(()=>{setV(false);done();},2200);
return <div className="toast">{msg}</div>;
}
function BackBtn({label="← 返回",onClick}){
return <button className="back-btn" onClick={onClick}>{label}</button>;
}
function GroupBadge({g}){
const c=GC[g]||"#888";
return <span className="chip" style={{background:`${c}22`,color:c,border:`1px solid ${c}44`
}
// Surrounding rank table (3 rows, self highlighted)
function RankNeighbors({title,byList,pid,statKey,label}){
const rows=getSurrounding(byList,pid,3);
return(
<div>
<div style={{fontSize:11,fontWeight:900,color:"var(--gray)",marginBottom:6,letterSpacin
<table className="rank-table" style={{width:"100%"}}>
<thead><tr>
<th>排名</th><th style={{textAlign:"left"}}>球员</th><th>{label}</th>
</tr></thead>
<tbody>
{rows.map(r=>(
<tr key={r.id} className={r.isMe?"me":""}>
<td className="rank-n">{r.rank}</td>
<td style={{textAlign:"left",fontWeight:r.isMe?900:400}}>{r.name}</td>
<td style={{color:r.isMe?"var(--red)":"var(--black)",fontWeight:r.isMe?900:400,
</tr>
))}
</tbody>
</table>
</div>
);
}
// ============================================================
// HOME
// ============================================================
function Home({nav}){
const[tab,setTab]=useState("team");
const[grp,setGrp]=useState("A");
const grpRanks={};
["A","B","C"].forEach(g=>{grpRanks[g]=STANDINGS.filter(t=>t.group===g).map((t,i)=>({...t,gr
const liveH=TEAMS[0],liveA=TEAMS[3];
return(
<div className="fin">
{/* RED HERO HEADER */}
<div className="hero-header">
<div className="hero-bg-pattern"/>
<div className="hero-logo-row">
<SchlLogo size={72}/>
<div className="hero-text">
<div className="hero-league">SOUTH CHINA HOCKEY LEAGUE</div>
<div className="hero-title">中国南⽅冰球联赛</div>
<div className="hero-season">2024–25 赛季</div>
</div>
</div>
</div>
{/* WHITE BODY */}
<div className="white-body">
<div className="sec">
<div className="sec-ttl bebas"> 赛事动态</div>
{/* live */}
<div className="score-card-hero" style={{marginBottom:10}}>
<div style={{display:"flex",justifyContent:"space-between",alignItems:"center",ma
<span className="live-badge">LIVE</span>
<span style={{fontSize:11,color:"rgba(255,255,255,.5)"}}>2nd · 14:22</span>
</div>
<div style={{display:"flex",alignItems:"center",justifyContent:"space-between"}}>
<div style={{textAlign:"center",cursor:"pointer"}} onClick={()=>nav("team",live
<div style={{fontSize:36}}>{liveH.logo}</div>
<div className="score-team-name">{liveH.name}</div>
</div>
<div style={{textAlign:"center"}}>
<div style={{display:"flex",alignItems:"center",gap:4}}>
<span className="score-num leading">3</span>
<span style={{fontFamily:"Bebas Neue",fontSize:32,color:"rgba(255,255,255,.
<span className="score-num">1</span>
</div>
</div>
<div style={{textAlign:"center",cursor:"pointer"}} onClick={()=>nav("team",live
<div style={{fontSize:36}}>{liveA.logo}</div>
<div className="score-team-name">{liveA.name}</div>
</div>
</div>
</div>
{GAMES.slice(0,3).map(g=>{
const ht=TEAMS.find(t=>t.id===g.homeTeam),at=TEAMS.find(t=>t.id===g.awayTeam);
return(
<div key={g.id} onClick={()=>nav("game",g.id)} style={{display:"flex",alignItem
<span style={{fontSize:11,color:"var(--gray)",minWidth:28,fontWeight:700}}>完
<span style={{fontSize:13,flex:1,fontWeight:700}}>{ht?.logo}{ht?.name}</span>
<span className="bebas" style={{fontSize:22,color:"var(--red)"}}>{g.homeScore
<span style={{fontSize:13,flex:1,textAlign:"right",fontWeight:700}}>{at?.name
</div>
);
})}
</div>
<div className="sec">
<div className="sec-ttl bebas"> 排⾏榜</div>
<div className="tabs" style={{marginBottom:12}}>
{[["team","球队榜"],["scorer","射⼿榜"],["assist","助攻榜"],["goalie","⻔将榜"]].map(
<button key={k} className={`tab ${tab===k?"on":""}`} onClick={()=>setTab(k)}>{v
))}
</div>
{tab==="team"&&<>
<div className="tabs" style={{marginBottom:12}}>
{["A","B","C"].map(g=><button key={g} className={`tab ${grp===g?"on":""}`} onCl
</div>
<div className="card-w">
<div style={{display:"flex",padding:"6px 14px",gap:8,borderBottom:"1px solid va
<div style={{width:22}}/><div style={{width:22}}/><div style={{flex:1}}/>
{["场","胜","负","分"].map(h=><div key={h} className="sc" style={{fontSize:10,
</div>
{(grpRanks[grp]||[]).map((t,i)=>(
<div key={t.id} className="srow" onClick={()=>nav("team",t.id)}>
<div className={`rnk ${i===0?"t1":i===1?"t2":i===2?"t3":""}`}>{t.gr}</div>
<div style={{fontSize:18}}>{t.logo}</div>
<div className="stn">{t.name}<span>{t.nameEn}</span></div>
<div className="sc">{t.gp}</div><div className="sc">{t.w}</div><div classNa
<div className="sc pts">{t.pts}</div>
</div>
))}
</div>
</>}
{tab==="scorer"&&<div className="card-w">
{[...ALL_PLAYERS].filter(p=>!p.isGoalie).sort((a,b)=>b.g-a.g||b.pts-a.pts).slice(
const team=TEAMS.find(t=>t.id===p.teamId);
return(
<div key={p.id} className="prow" onClick={()=>nav("player",p.id)}>
<div className={`prank ${i===0?"t1":i===1?"t2":i===2?"t3":""}`}>{i+1}</div>
<div className="pos" style={{background:`${PC[p.position]}18`,color:PC[p.po
<div className="pnm"><div className="pnm-main">{p.name}</div><div className
<div style={{textAlign:"right"}}><div className="pstat">{p.g}</div><div sty
</div>
);
})}
</div>}
{tab==="assist"&&<div className="card-w">
{[...ALL_PLAYERS].filter(p=>!p.isGoalie).sort((a,b)=>b.a-a.a||b.pts-a.pts).slice(
const team=TEAMS.find(t=>t.id===p.teamId);
return(
<div key={p.id} className="prow" onClick={()=>nav("player",p.id)}>
<div className={`prank ${i===0?"t1":i===1?"t2":i===2?"t3":""}`}>{i+1}</div>
<div className="pos" style={{background:`${PC[p.position]}18`,color:PC[p.po
<div className="pnm"><div className="pnm-main">{p.name}</div><div className
<div style={{textAlign:"right"}}><div className="pstat">{p.a}</div><div sty
</div>
);
})}
</div>}
{tab==="goalie"&&<div className="card-w">
{ALL_PLAYERS.filter(p=>p.isGoalie).sort((a,b)=>parseFloat(b.savePct)-parseFloat(a
const team=TEAMS.find(t=>t.id===p.teamId);
return(
<div key={p.id} className="prow" onClick={()=>nav("player",p.id)}>
<div className={`prank ${i===0?"t1":i===1?"t2":i===2?"t3":""}`}>{i+1}</div>
<div className="pos" style={{background:"#7B2FBE18",color:"#7B2FBE"}}>G</di
<div className="pnm"><div className="pnm-main">{p.name}</div><div className
<div style={{textAlign:"right"}}><div className="pstat" style={{fontSize:16
</div>
);
})}
</div>}
</div>
</div>
</div>
);
}
// ============================================================
// PLAYER
// ============================================================
function Player({pid,nav}){
const p=ALL_PLAYERS.find(x=>x.id===pid);
if(!p)return<div style={{padding:40,textAlign:"center",color:"var(--gray)"}}>球员未找到</div>
const team=TEAMS.find(t=>t.id===p.teamId);
const primary=team?.primary||"#D92B2B";
const gamelog=getPlayerGameLog(pid);
const BADGES=[
{n:"总冠军",ic:" ",e:p.pts>40},{n:"MVP",ic:" ",e:p.g>22},
{n:"精英组",ic:" ",e:team?.group==="A"},{n:"射⼿王",ic:" ",e:RANKINGS.gRank[p.id]===1},
{n:"城市章",ic:" ",e:p.pim<8},{n:"最佳阵容",ic:" ",e:p.pts>=37},
{n:"铁⼈奖",ic:" ",e:p.gp===5},{n:"进步奖",ic:" ",e:p.a>=20},
];
return(
<div className="fin">
<BackBtn onClick={()=>nav("home")}/>
{/* HERO */}
<div className="phero" style={{background:`linear-gradient(150deg,${primary} 0%,${prima
<div className="phero-num">{p.number}</div>
<div className="phero-pos">#{p.number} · {p.isGoalie?"守⻔员":p.position} · {p.age}岁<
<div className="phero-name">{p.name}</div>
<div style={{display:"flex",gap:8,marginTop:10,flexWrap:"wrap",position:"relative"}}>
<span className="chip" style={{background:"rgba(0,0,0,.3)",color:"#fff",border:"1px
{team?.logo} {team?.name}
</span>
<span className="chip" style={{background:GC[team?.group]||"#888",color:"#fff"}}>
{team?.group}组
</span>
</div>
</div>
{/* BIO */}
<div className="sec">
<div className="sec-ttl bebas">球员档案</div>
<div className="card-w" style={{padding:16}}>
{[["球员ID",p.id],["身⾼",p.height+"cm"],["体重",p.weight+"kg"],["位置",p.isGoalie?"守
<div key={k} style={{display:"flex",justifyContent:"space-between",padding:"9px 0
<span style={{fontSize:12,color:"var(--gray)"}}>{k}</span>
<span style={{fontSize:13,fontWeight:700}}>{v}</span>
</div>
))}
</div>
</div>
{/* GAME LOG per-game stats */}
<div className="sec">
<div className="sec-ttl bebas">历史⽐赛数据</div>
<div className="card-w">
<div className="hist-wrap">
<table className="hist-table">
<thead><tr>
<th>⽇期</th><th>对阵</th><th>⽐分</th><th>进球</th><th>助攻</th><th>受罚</th>
</tr></thead>
<tbody>
{gamelog.map((row,i)=>(
<tr key={i} style={{cursor:"pointer"}} onClick={()=>nav("game",row.gameId)}
<td>{row.date.slice(5)}</td>
<td style={{textAlign:"left"}}>{row.oppLogo} {row.opp}</td>
<td style={{color:row.res==="W"?"var(--red)":"var(--gray)",fontWeight:900
<td style={{color:"var(--red)",fontWeight:row.goals>0?900:400}}>{row.goal
<td style={{color:"#0088CC",fontWeight:row.assists>0?900:400}}>{row.assis
<td style={{color:"var(--gray)"}}>{row.pim>0?row.pim+"min":"—"}</td>
</tr>
))}
</tbody>
</table>
</div>
</div>
</div>
{/* RANKING NEIGHBORS — 3 columns */}
{!p.isGoalie&&<div className="sec">
<div className="sec-ttl bebas">联盟排名对⽐</div>
<div className="card-w" style={{padding:16}}>
<div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:16}}>
<RankNeighbors title="进球榜" byList={RANKINGS.byG} pid={pid} statKey="g" label="G
<RankNeighbors title="助攻榜" byList={RANKINGS.byA} pid={pid} statKey="a" label="A
<RankNeighbors title="受罚榜" byList={RANKINGS.byPim} pid={pid} statKey="pim" labe
</div>
</div>
</div>}
{/* BADGES */}
<div className="sec">
<div className="sec-ttl bebas">勋章陈列馆</div>
<div className="card-w">
<div className="badge-grid">
{BADGES.map((b,i)=>(
<div key={i} style={{display:"flex",flexDirection:"column",alignItems:"center"}
<div className={`badge-ic ${b.e?"on":"off"}`} style={{borderColor:b.e?"var(--
<div className="badge-nm">{b.n}</div>
</div>
))}
</div>
</div>
</div>
</div>
);
}
// ============================================================
// TEAM
// ============================================================
function Team({tid,nav}){
const team=TEAMS.find(t=>t.id===tid);
if(!team)return null;
const players=ALL_PLAYERS.filter(p=>p.teamId===tid);
const games=getTeamGames(tid);
const rec=getTeamRecord(tid);
const primary=team.primary;
return(
<div className="fin">
<BackBtn onClick={()=>nav("home")}/>
<div className="thero" style={{background:`linear-gradient(150deg,${primary} 0%,${prima
<div style={{position:"absolute",inset:0,opacity:.07,background:"repeating-linear-gra
<div className="thero-logo">{team.logo}</div>
<div className="thero-name" style={{color:"#fff"}}>{team.name}</div>
<div style={{fontSize:11,color:"rgba(255,255,255,.7)",letterSpacing:".2em",position:"
<div style={{marginTop:10,position:"relative"}}>
<span className="chip" style={{background:"rgba(0,0,0,.3)",color:"#fff",border:"1px
</div>
</div>
<div className="sgrid">
{[["胜",rec.w,"var(--red)"],["负",rec.l,"var(--gray)"],["积分",rec.pts,primary],["净胜"
<div key={l} className="sbox"><div className="sbox-v" style={{color:c}}>{v}</div><d
))}
</div>
<div className="sec">
<div className="sec-ttl bebas">球队信息</div>
<div className="card-w" style={{padding:16}}>
{[["主教练",team.coach],["主场",team.arena],["成⽴",team.founded+"年"],["组别",team.gr
<div key={k} style={{display:"flex",justifyContent:"space-between",padding:"9px 0
<span style={{fontSize:12,color:"var(--gray)"}}>{k}</span>
<span style={{fontSize:13,fontWeight:700}}>{v}</span>
</div>
))}
</div>
</div>
<div className="sec">
<div className="sec-ttl bebas">球员阵容</div>
<div className="card-w">
{players.map(p=>(
<div key={p.id} className="prow" onClick={()=>nav("player",p.id)}>
<div className="pos" style={{background:`${PC[p.position]}18`,color:PC[p.positi
<div className="bebas" style={{fontSize:20,color:"var(--gray)",minWidth:36,text
<div className="pnm">
<div className="pnm-main">{p.name}</div>
<div className="pnm-sub">{p.isGoalie?`扑救率 ${p.savePct} · GAA ${p.gaa}`:`${p
</div>
<span style={{color:"var(--gray)"}}>›</span>
</div>
))}
</div>
</div>
<div className="sec">
<div className="sec-ttl bebas">近期战绩</div>
<div className="card-w">
{games.slice(0,5).map(g=>{
const ih=g.homeTeam===tid;
const opp=TEAMS.find(t=>t.id===(ih?g.awayTeam:g.homeTeam));
const ts=ih?g.homeScore:g.awayScore,os=ih?g.awayScore:g.homeScore;
const r=ts>os?"W":ts<os?"L":"T";
return(
<div key={g.id} className="gmini" onClick={()=>nav("game",g.id)}>
<span className={`bebas r${r}`} style={{fontSize:22,width:24}}>{r}</span>
<span style={{fontSize:11,color:"var(--gray)",minWidth:55}}>{g.date.slice(5)}
<span style={{flex:1,fontSize:13,fontWeight:700}}>vs {opp?.logo}{opp?.name}</
<span className="bebas" style={{fontSize:22}}>{ts}<span style={{color:"var(--
</div>
);
})}
</div>
</div>
</div>
);
}
// ============================================================
// GAME DETAIL
// ============================================================
function Game({gid,nav}){
const g=GAMES.find(x=>x.id===gid);
if(!g)return null;
const ht=TEAMS.find(t=>t.id===g.homeTeam),at=TEAMS.find(t=>t.id===g.awayTeam);
return(
<div className="fin">
<BackBtn onClick={()=>nav("home")}/>
<div style={{background:`linear-gradient(135deg,${ht?.primary||"#D92B2B"} 0%,${at?.prim
<div style={{textAlign:"center",fontSize:11,color:"rgba(255,255,255,.6)",letterSpacin
<div style={{display:"flex",alignItems:"center",justifyContent:"space-between"}}>
<div style={{textAlign:"center",flex:1,cursor:"pointer"}} onClick={()=>nav("team",h
<div style={{fontSize:44}}>{ht?.logo}</div>
<div style={{fontSize:14,fontWeight:900,color:"#fff"}}>{ht?.name}</div>
<div style={{fontSize:11,color:"rgba(255,255,255,.6)"}}>主场</div>
</div>
<div style={{textAlign:"center",padding:"0 12px"}}>
<div className="bebas" style={{fontSize:56,lineHeight:1,color:"#fff",textShadow:"
{g.homeScore}<span style={{fontSize:32,opacity:.5}}>:</span>{g.awayScore}
</div>
</div>
<div style={{textAlign:"center",flex:1,cursor:"pointer"}} onClick={()=>nav("team",a
<div style={{fontSize:44}}>{at?.logo}</div>
<div style={{fontSize:14,fontWeight:900,color:"#fff"}}>{at?.name}</div>
<div style={{fontSize:11,color:"rgba(255,255,255,.6)"}}>客场</div>
</div>
</div>
<div style={{textAlign:"center",fontSize:11,color:"rgba(255,255,255,.5)",marginTop:12
</div>
<div className="sec">
<div className="sec-ttl bebas"> ⽐赛时间轴</div>
<div style={{position:"relative"}}>
{g.events.map((ev,i)=>{
const evTeam=TEAMS.find(t=>t.id===ev.team);
const isGoal=ev.type==="goal";
return(
<div key={i} className={`tl ${isGoal?"goal":"pen"}`}>
<div className="tl-time">{ev.time}'</div>
<div style={{flex:1}}>
<div className="tl-type" style={{color:isGoal?"var(--red)":"var(--gold2)"}}
{isGoal?" 进球":" 受罚"} · {evTeam?.name}
</div>
{isGoal?(
<>
<div className="tl-main" onClick={()=>nav("player",ev.scorerId)}> {ev
{ev.assister&&<div className="tl-sub" onClick={()=>nav("player",ev.assi
):(
</>
<>
<div className="tl-main" onClick={()=>nav("player",ev.playerId)}>{ev.pl
<div className="tl-sub">{ev.infraction} · {ev.minutes}分钟</div>
</>
)}
</div>
</div>
);
})}
</div>
</div>
</div>
);
}
// ============================================================
// GAMES / SCHEDULE
// ============================================================
function Games({nav}){
const[filter,setFilter]=useState("all");
const[tab,setTab]=useState("results");
const filtered=filter==="all"?GAMES:GAMES.filter(g=>g.homeTeam===filter||g.awayTeam===filte
const futureFiltered=filter==="all"?FUTURE_MATCHUPS:FUTURE_MATCHUPS.filter(([h,a])=>h===fil
return(
<div className="fin">
<div className="hero-header">
<div className="hero-bg-pattern"/>
<div style={{position:"relative"}}>
<div className="hero-league">SCHEDULE · 赛程中⼼</div>
<div className="hero-title" style={{fontSize:32}}>赛事中⼼</div>
</div>
</div>
<div className="white-body">
<div className="sec" style={{paddingBottom:8}}>
<div className="tabs" style={{marginBottom:12}}>
<button className={`tab ${tab==="results"?"on":""}`} onClick={()=>setTab("results
<button className={`tab ${tab==="schedule"?"on":""}`} onClick={()=>setTab("schedu
</div>
<div style={{overflowX:"auto",paddingBottom:8}}>
<div style={{display:"flex",gap:6,width:"max-content"}}>
<button className={`tab ${filter==="all"?"on":""}`} style={{padding:"6px {TEAMS.map(t=>(
<button key={t.id} className={`tab ${filter===t.id?"on":""}`} style={{padding
{t.logo} {t.name}
</button>
12px"}
))}
</div>
</div>
</div>
{tab==="results"&&<div className="card-w" style={{margin:"0 16px"}}>
{filtered.map(g=>{
const ht=TEAMS.find(t=>t.id===g.homeTeam),at=TEAMS.find(t=>t.id===g.awayTeam);
return(
<div key={g.id} className="gmini" style={{flexDirection:"column",alignItems:"st
<div style={{display:"flex",justifyContent:"space-between",alignItems:"center
<span style={{fontSize:13,fontWeight:700}}>{ht?.logo} {ht?.name}</span>
<span className="bebas" style={{fontSize:22,color:"var(--red)"}}>{g.homeSco
</div>
<div style={{display:"flex",justifyContent:"space-between",alignItems:"center
<span style={{fontSize:13,fontWeight:700}}>{at?.logo} {at?.name}</span>
<span className="bebas" style={{fontSize:22}}>{g.awayScore}</span>
</div>
<div style={{fontSize:10,color:"var(--gray)",marginTop:2}}>{g.date} · {g.aren
</div>
);
})}
</div>}
{tab==="schedule"&&<div style={{padding:"0 16px"}}>
{futureFiltered.map(([home,away,date,arena],i)=>{
const ht=TEAMS.find(t=>t.id===home),at=TEAMS.find(t=>t.id===away);
return(
<div key={i} className="sch-card sch-future">
<div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-s
<span className="future-tag">即将开赛</span>
<span style={{fontSize:12,color:"var(--gray)",fontWeight:700}}>{date}</span
</div>
<div style={{display:"flex",alignItems:"center",justifyContent:"space-between
<div style={{textAlign:"center",flex:1}}>
<div style={{fontSize:28}}>{ht?.logo}</div>
<div style={{fontSize:13,fontWeight:900}}>{ht?.name}</div>
<div style={{fontSize:10,color:"var(--gray)"}}>主场</div>
</div>
<div className="bebas" style={{fontSize:28,color:"var(--gray)",padding:"0 1
<div style={{textAlign:"center",flex:1}}>
<div style={{fontSize:28}}>{at?.logo}</div>
<div style={{fontSize:13,fontWeight:900}}>{at?.name}</div>
<div style={{fontSize:10,color:"var(--gray)"}}>客场</div>
</div>
</div>
<div style={{fontSize:10,color:"var(--gray)",marginTop:8,textAlign:"center"}}
</div>
);
})}
</div>}
</div>
</div>
);
}
// ============================================================
// SEARCH
// ============================================================
function Search({nav}){
const[q,setQ]=useState("");
const res=q.length>=1?[
...ALL_PLAYERS.filter(p=>p.name.includes(q)||p.id.toLowerCase().includes(q.toLowerCase())
...TEAMS.filter(t=>t.name.includes(q)||t.nameEn.toLowerCase().includes(q.toLowerCase())||
].slice(0,15):[];
return(
<div className="fin">
<div className="hero-header">
<div className="hero-bg-pattern"/>
<div style={{position:"relative"}}>
<div className="hero-league">SEARCH · 搜索</div>
<div className="hero-title" style={{fontSize:32}}>搜索</div>
</div>
</div>
<div className="white-body">
<div className="sec">
<div style={{position:"relative"}}>
<span style={{position:"absolute",left:16,top:"50%",transform:"translateY(-50%)",
<input className="srch-inp" placeholder="搜索球队 · 球员 · 编号..." value={q} onChan
</div>
</div>
{res.length>0&&<div className="card-w" style={{margin:"0 16px"}}>
{res.map((r,i)=>(
<div key={i} className="sri" onClick={()=>nav(r.type,r.id)}>
<span className="sri-type">{r.type==="player"?"球员":"球队"}</span>
<div style={{flex:1}}><div style={{fontSize:14,fontWeight:700}}>{r.name}</div><
<span style={{color:"var(--gray)"}}>›</span>
</div>
))}
</div>}
{q.length>=1&&res.length===0&&<div style={{textAlign:"center",padding:40,color:"var(-
{!q&&<div className="sec">
<div className="sec-ttl bebas">快速跳转</div>
<div style={{display:"flex",flexWrap:"wrap",gap:8}}>
{TEAMS.map(t=>(
<button key={t.id} onClick={()=>nav("team",t.id)} style={{background:"var(--whi
{t.logo} {t.name}
</button>
))}
</div>
</div>}
</div>
</div>
);
}
// ============================================================
// SHOP
// ============================================================
function Shop({nav,addToCart,cartCount,openCart}){
return(
<div className="fin">
<div className="hero-header">
<div className="hero-bg-pattern"/>
<div style={{position:"relative"}}>
<div className="hero-league">MERCHANDISE · 装备商城</div>
<div className="hero-title" style={{fontSize:32}}>装备商城</div>
</div>
</div>
<div className="white-body">
<div className="promo">
<div style={{fontSize:32}}> </div>
<div style={{fontSize:12,color:"var(--gray2)",lineHeight:1.5}}>
<strong style={{color:"var(--gold)",display:"block",fontSize:14,marginBottom:2}}>
武汉站纪念章限量发售，赛站结束即下架
</div>
</div>
<div className="shop-grid">
{SHOP_ITEMS.map(item=>(
<div key={item.id} className="shcard" onClick={()=>nav("product",item.id)}>
<div className="shimg">{item.images[0]}</div>
<div className="shinfo">
<div className="shnm">{item.name}</div>
<div className="shprice">¥{item.price}</div>
{item.tier==="elite"&&<div style={{fontSize:10,color:"var(--gray)",marginTop:
</div>
</div>
))}
</div>
<div style={{height:80}}/>
</div>
{/* Floating cart button */}
<button className="cart-fab" onClick={openCart} style={{bottom:96}}>
{cartCount>0&&<span className="cart-cnt">{cartCount}</span>}
</button>
</div>
);
}
function Product({pid,nav,addToCart}){
const item=SHOP_ITEMS.find(i=>i.id===pid);
if(!item)return null;
const[activeImg,setActiveImg]=useState(0);
const[qty,setQty]=useState(1);
return(
<div className="fin">
<BackBtn label="← 商城" onClick={()=>nav("shop")}/>
<div className="pm-img">{item.images[activeImg]}</div>
<div style={{display:"flex",gap:8,padding:"12px 16px"}}>
{item.images.map((img,i)=>(
<div key={i} className={`pm-thumb ${activeImg===i?"on":""}`} onClick={()=>setActive
))}
</div>
<div className="bebas" style={{fontSize:28,padding:"0 16px",color:"var(--black)"}}>{ite
<div className="bebas" style={{fontSize:36,color:"var(--red)",padding:"4px 16px"}}>¥{it
{item.tier==="elite"&&(
<div style={{padding:"6px 16px"}}>
<span style={{background:"#E8A80022",color:"var(--gold2)",padding:"4px 12px",border
</div>
)}
<div style={{fontSize:13,color:"var(--gray)",lineHeight:1.8,padding:"12px 16px"}}>{item
<div style={{padding:"0 16px 16px",display:"flex",alignItems:"center",gap:12}}>
<span style={{fontSize:13,color:"var(--gray)",fontWeight:700}}>数量</span>
<button className="qty-btn" onClick={()=>setQty(Math.max(1,qty-1))}>−</button>
<span className="bebas" style={{fontSize:24,minWidth:30,textAlign:"center"}}>{qty}</s
<button className="qty-btn" onClick={()=>setQty(qty+1)}>+</button>
<span style={{fontSize:11,color:"var(--gray)"}}>库存 {item.stock}</span>
</div>
<button className="atcbtn" onClick={()=>addToCart(item,qty)}>加⼊购物⻋</button>
</div>
);
}
function CartDrawer({cart,close}){
const total=cart.reduce((s,i)=>s+i.price*i.qty,0);
return(
<>
<div className="cart-ovr" onClick={close}/>
<div className="cart-drw">
<div style={{padding:"18px 20px 14px",display:"flex",justifyContent:"space-between",a
<div className="bebas" style={{fontSize:26,color:"var(--red)"}}>购物⻋</div>
<button onClick={close} style={{background:"none",border:"none",color:"var(--gray)"
</div>
{cart.length===0?(
<div style={{padding:48,textAlign:"center",color:"var(--gray)"}}>购物⻋是空的 ):(
</div
<>
{cart.map((item,i)=>(
<div key={i} style={{display:"flex",alignItems:"center",gap:12,padding:"13px 20
<div style={{width:48,height:48,background:"var(--white3)",borderRadius:8,dis
<div style={{flex:1}}>
<div style={{fontSize:13,fontWeight:700}}>{item.name}</div>
<div style={{fontSize:11,color:"var(--gray)"}}>×{item.qty}</div>
</div>
<div className="bebas" style={{fontSize:18,color:"var(--red)"}}>¥{item.price*
</div>
))}
<div style={{padding:20,borderTop:"1px solid var(--border-light)",display:"flex",
<div>
<div style={{fontSize:12,color:"var(--gray)"}}>合计</div>
<div className="bebas" style={{fontSize:28,color:"var(--red)"}}>¥{total}</div
</div>
<button style={{background:"var(--red)",color:"#fff",border:"none",borderRadius
</div>
</>
)}
</div>
</>
);
}
// ============================================================
// MY PAGE — 熊天逸
// ============================================================
const MY_PLAYER_ID="P_XTY";
function MyPage({nav}){
const me=ALL_PLAYERS.find(p=>p.id===MY_PLAYER_ID); // 熊天逸
const team=TEAMS.find(t=>t.id===me?.teamId);
const[bio,setBio]=useState(" 武汉冰⻰ #93 | Just a normal bear");
const[editBio,setEditBio]=useState(false);
const[bioTmp,setBioTmp]=useState(bio);
const[photos,setPhotos]=useState([]);
const[videos,setVideos]=useState([]);
const photoRef=useRef();
const videoRef=useRef();
const gamelog=getPlayerGameLog(MY_PLAYER_ID);
const handlePhoto=(e)=>{
const files=Array.from(e.target.files||[]);
files.forEach(f=>{
const url=URL.createObjectURL(f);
setPhotos(prev=>[...prev,{url,name:f.name}]);
});
};
const handleVideo=(e)=>{
const files=Array.from(e.target.files||[]);
files.forEach(f=>{
const url=URL.createObjectURL(f);
setVideos(prev=>[...prev,{url,name:f.name}]);
});
};
const BADGES=[
{n:"总冠军",ic:" ",e:false},{n:"MVP",ic:" ",e:false},
{n:"精英组",ic:" ",e:team?.group==="A"},{n:"射⼿王",ic:" ",e:false},
{n:"城市章",ic:" ",e:true},{n:"最佳阵容",ic:" ",e:true},
{n:"铁⼈奖",ic:" ",e:true},{n:"进步奖",ic:" ",e:true},
];
return(
<div className="fin">
{/* MY HERO */}
<div className="my-hero">
<div style={{position:"absolute",inset:0,opacity:.07,background:"repeating-linear-gra
<div style={{display:"flex",gap:16,alignItems:"flex-end",position:"relative"}}>
<div className="my-avatar" onClick={()=>photoRef.current?.click()}>
{photos.length>0?<img src={photos[photos.length-1].url} alt="avatar"/>:<span styl
<div style={{position:"absolute",bottom:0,right:0,background:"var(--gold)",border
</div>
<div>
<div style={{fontSize:10,color:"rgba(255,255,255,.6)",letterSpacing:".2em"}}># {m
<div className="bebas" style={{fontSize:36,color:"#fff",lineHeight:1,textShadow:"
<div style={{fontSize:11,color:"rgba(255,255,255,.6)",marginTop:2}}>{team?.logo}
</div>
</div>
{/* bio */}
<div style={{marginTop:16,position:"relative"}}>
{editBio?(
<div>
<input value={bioTmp} onChange={e=>setBioTmp(e.target.value)} style={{width:"10
<div style={{display:"flex",gap:8,marginTop:6}}>
<button onClick={()=>{setBio(bioTmp);setEditBio(false);}} style={{background:
<button onClick={()=>setEditBio(false)} style={{background:"rgba(255,255,255,
</div>
</div>
):(
<div style={{display:"flex",gap:8,alignItems:"center"}}>
<div style={{fontSize:13,color:"rgba(255,255,255,.85)",flex:1,fontStyle:"italic
<button onClick={()=>{setBioTmp(bio);setEditBio(true);}} style={{background:"rg
</div>
)}
</div>
</div>
<input ref={photoRef} type="file" accept="image/*" multiple style={{display:"none"}} on
<input ref={videoRef} type="file" accept="video/*" multiple style={{display:"none"}} on
{/* WHITE BODY */}
<div className="white-body">
{/* Upload media */}
<div className="sec">
<div className="sec-ttl bebas"> 我的媒体</div>
<div style={{display:"flex",gap:10}}>
<div className="upload-area" style={{flex:1}} onClick={()=>photoRef.current?.clic
<div style={{fontSize:28,marginBottom:4}}> </div>
<div style={{fontSize:12,color:"var(--gray)",fontWeight:700}}>上传照⽚</div>
<div style={{fontSize:10,color:"var(--gray2)"}}>⽀持 JPG · PNG</div>
</div>
<div className="upload-area" style={{flex:1}} onClick={()=>videoRef.current?.clic
<div style={{fontSize:28,marginBottom:4}}> </div>
<div style={{fontSize:12,color:"var(--gray)",fontWeight:700}}>上传视频</div>
<div style={{fontSize:10,color:"var(--gray2)"}}>⽀持 MP4 · MOV</div>
</div>
</div>
{photos.length>0&&(
<div style={{marginTop:12,display:"flex",gap:8,overflowX:"auto",paddingBottom:4}}
{photos.map((p,i)=>(
<img key={i} src={p.url} alt={p.name} style={{width:72,height:72,objectFit:"c
))}
</div>
)}
{videos.length>0&&(
<div style={{marginTop:8}}>
{videos.map((v,i)=>(
<div key={i} style={{background:"var(--white3)",borderRadius:8,padding:"8px 1
<span style={{fontSize:20}}> </span>
<span style={{fontSize:12,color:"var(--gray)"}}>{v.name}</span>
</div>
))}
</div>
)}
</div>
{/* RANKING NEIGHBORS */}
<div className="sec">
<div className="sec-ttl bebas">联盟排名对⽐</div>
<div className="card-w" style={{padding:16}}>
<div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:16}}>
<RankNeighbors title="进球榜" byList={RANKINGS.byG} pid={MY_PLAYER_ID} statKey="
<RankNeighbors title="助攻榜" byList={RANKINGS.byA} pid={MY_PLAYER_ID} statKey="
<RankNeighbors title="受罚榜" byList={RANKINGS.byPim} pid={MY_PLAYER_ID} statKey
</div>
</div>
</div>
{/* GAME LOG */}
<div className="sec">
<div className="sec-ttl bebas">历史⽐赛记录</div>
<div className="card-w">
<div className="hist-wrap">
<table className="hist-table">
<thead><tr>
<th>⽇期</th><th>对阵</th><th>⽐分</th><th>进球</th><th>助攻</th><th>受罚</th>
</tr></thead>
<tbody>
{gamelog.map((row,i)=>(
<tr key={i} style={{cursor:"pointer"}} onClick={()=>nav("game",row.gameId
<td>{row.date.slice(5)}</td>
<td style={{textAlign:"left"}}>{row.oppLogo} {row.opp}</td>
<td style={{color:row.res==="W"?"var(--red)":"var(--gray)",fontWeight:9
<td style={{color:"var(--red)",fontWeight:row.goals>0?900:400}}>{row.go
<td style={{color:"#0088CC",fontWeight:row.assists>0?900:400}}>{row.ass
<td>{row.pim>0?row.pim+"min":"—"}</td>
</tr>
))}
</tbody>
</table>
</div>
</div>
</div>
{/* BADGES */}
<div className="sec">
<div className="sec-ttl bebas">我的勋章墙</div>
<div className="card-w">
<div className="badge-grid">
{BADGES.map((b,i)=>(
<div key={i} style={{display:"flex",flexDirection:"column",alignItems:"center
<div className={`badge-ic ${b.e?"on":"off"}`} style={{borderColor:b.e?"var(
<div className="badge-nm">{b.n}</div>
</div>
))}
</div>
</div>
</div>
<div style={{height:20}}/>
</div>
</div>
);
}
// ============================================================
// APP
// ============================================================
export default function App(){
const[page,setPage]=useState("home");
const[subId,setSubId]=useState(null);
const[cart,setCart]=useState([]);
const[showCart,setShowCart]=useState(false);
const[toast,setToast]=useState(null);
const nav=(pg,id=null)=>{setPage(pg);setSubId(id);window.scrollTo(0,0);};
const addToCart=(item,qty)=>{
setCart(prev=>{
const ex=prev.findIndex(i=>i.id===item.id);
if(ex>=0){const n=[...prev];n[ex]={...n[ex],qty:n[ex].qty+qty};return n;}
return[...prev,{...item,qty}];
});
setToast("已加⼊购物⻋: "+item.name);
nav("shop");
};
const cartCount=cart.reduce((s,i)=>s+i.qty,0);
const mainPg=["home","games","search","shop","my"].includes(page)?page:
page==="product"?"shop":
page==="game"||page==="player"||page==="team"?"home":"home";
const NavIcon=({id,label,icon})=>(
<button className={`nb ${mainPg===id?"on":""}`} onClick={()=>nav(id)}>
{icon}
<span className="nb-lbl">{label}</span>
</button>
);
return(
<>
<style>{CSS}</style>
<div className="app">
{page==="home"&&<Home nav={nav}/>}
{page==="player"&&<Player pid={subId} nav={nav}/>}
{page==="team"&&<Team tid={subId} nav={nav}/>}
{page==="game"&&<Game gid={subId} nav={nav}/>}
{page==="games"&&<Games nav={nav}/>}
{page==="search"&&<Search nav={nav}/>}
{page==="shop"&&<Shop nav={nav} addToCart={addToCart} cartCount={cartCount} openCart=
{page==="product"&&<Product pid={subId} nav={nav} addToCart={addToCart}/>}
{page==="my"&&<MyPage nav={nav}/>}
<nav className="nav">
<NavIcon id="home" label="⾸⻚" icon={<svg viewBox="0 0 24 24" fill="none" stroke="c
<NavIcon id="games" label="赛事" icon={<svg viewBox="0 0 24 24" fill="none" stroke="
<NavIcon id="search" label="搜索" icon={<svg viewBox="0 0 24 24" fill="none" stroke=
<NavIcon id="shop" label="商城" icon={<svg viewBox="0 0 24 24" fill="none" stroke="c
<NavIcon id="my" label="我的" icon={<svg viewBox="0 0 24 24" fill="none" stroke="cur
</nav>
{showCart&&<CartDrawer cart={cart} close={()=>setShowCart(false)}/>}
{toast&&<Toast msg={toast} done={()=>setToast(null)}/>}
</div>
</>
);
}