<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>TravelPilot</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#0F1B2D; --paper:#EEF1EE; --panel:#FFFFFF; --amber:#C9822E; --amber-bg:#F4E3C8;
    --teal:#2F6F62; --teal-bg:#DCEAE6; --rust:#B8462F; --rust-bg:#F5DCD5;
    --text:#16202C; --text-soft:#51606F; --line:#D7DCD6; --focus:#2F6F62;
  }
  :root:not([data-theme="light"]){
    @media (prefers-color-scheme: dark){
      --paper:#0B1220; --panel:#131C2B; --text:#E7ECF2; --text-soft:#9AA7B5; --line:#243044; --ink:#060B14;
      --amber-bg:#3A2C15; --teal-bg:#132923; --rust-bg:#301A15;
    }
  }
  :root[data-theme="dark"]{
    --paper:#0B1220; --panel:#131C2B; --text:#E7ECF2; --text-soft:#9AA7B5; --line:#243044; --ink:#060B14;
    --amber-bg:#3A2C15; --teal-bg:#132923; --rust-bg:#301A15;
  }
  *{box-sizing:border-box;}
  html{scroll-padding-top:env(safe-area-inset-top,0px);}
  html,body{height:100%;}
  body{
    margin:0; background:var(--paper); color:var(--text);
    font-family:'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
    -webkit-text-size-adjust:100%;
  }
  h1,h2,h3,h4,.label,.btn,.tab{font-family:'Space Grotesk',sans-serif;}
  .mono{font-family:'IBM Plex Mono',SFMono-Regular,monospace;}
  a{color:inherit;}
  button{font:inherit; cursor:pointer;}
  input,select,textarea{font:inherit; color:var(--text); background:var(--panel); border:1px solid var(--line); border-radius:3px; padding:7px 9px;}
  input:focus,select:focus,textarea:focus,button:focus-visible{outline:2px solid var(--focus); outline-offset:1px;}
  ::placeholder{color:var(--text-soft); opacity:0.8;}

  /* ---- Header board ---- */
  .board{
    background:var(--ink); color:#EDEFEF; padding:14px 18px;
    padding-top:calc(14px + env(safe-area-inset-top,0px));
    display:flex; flex-wrap:wrap; gap:6px 28px; align-items:baseline;
    position:sticky; top:0; z-index:20; border-bottom:2px solid var(--amber);
  }
  .board .brand{font-weight:700; font-size:1.05rem; letter-spacing:0.02em; display:flex; align-items:center; gap:8px;}
  .board .brand .dot{width:8px;height:8px;border-radius:50%;background:var(--amber);}
  .board .field{display:flex; flex-direction:column; gap:1px;}
  .board .field .k{font-size:0.62rem; color:#9FB0BE; letter-spacing:0.08em;}
  .board .field .v{font-size:0.85rem; font-family:'IBM Plex Mono',monospace;}
  .board .field .v.amber{color:var(--amber);}
  .board .field .v.rust{color:#E58369;}
  .board .field .v.teal{color:#7FCBB8;}
  .board .spacer{flex:1;}

  /* ---- Layout ---- */
  .shell{display:flex; min-height:calc(100vh - 64px);}
  .rail{
    width:180px; flex-shrink:0; background:var(--panel); border-right:1px solid var(--line);
    padding:14px 0; position:sticky; top:64px; align-self:flex-start; height:calc(100vh - 64px); overflow-y:auto;
  }
  .tab{
    display:block; width:100%; text-align:left; background:none; border:none; border-left:3px solid transparent;
    padding:11px 18px; font-size:0.88rem; color:var(--text-soft); font-weight:500;
  }
  .tab.active{color:var(--text); border-left-color:var(--amber); background:var(--amber-bg);}
  .tab:hover:not(.active){background:var(--paper);}
  .main{flex:1; min-width:0; padding:22px 22px 60px;}
  .container{max-width:880px;}
  @media (max-width:760px){
    .shell{flex-direction:column;}
    .rail{
      width:100%; height:auto; position:sticky; top:64px; display:flex; overflow-x:auto; padding:6px 4px;
      border-right:none; border-bottom:1px solid var(--line);
    }
    .tab{width:auto; white-space:nowrap; border-left:none; border-bottom:3px solid transparent; padding:9px 14px;}
    .tab.active{border-left-color:transparent; border-bottom-color:var(--amber);}
    .main{padding:16px 14px 50px;}
    .board{gap:4px 16px; padding:12px 14px; padding-top:calc(12px + env(safe-area-inset-top,0px));}
  }

  h2.pagehead{font-size:1.25rem; margin:0 0 4px;}
  p.sub{color:var(--text-soft); margin:0 0 20px; font-size:0.9rem; max-width:56ch;}
  .panel{background:var(--panel); border:1px solid var(--line); padding:16px 18px; margin-bottom:16px;}
  .panel h3{margin:0 0 12px; font-size:0.95rem;}
  .grid2{display:grid; grid-template-columns:1fr 1fr; gap:12px;}
  .grid3{display:grid; grid-template-columns:1fr 1fr 1fr; gap:12px;}
  @media (max-width:600px){ .grid2,.grid3{grid-template-columns:1fr;} }
  .field-row{display:flex; flex-direction:column; gap:4px; margin-bottom:10px;}
  .field-row label{font-size:0.72rem; color:var(--text-soft); font-weight:500; letter-spacing:0.02em;}
  .field-row input,.field-row select{width:100%;}
  .chip-toggle{
    display:inline-flex; align-items:center; gap:6px; padding:6px 11px; border:1px solid var(--line);
    background:var(--panel); font-size:0.8rem; margin:0 6px 6px 0; user-select:none;
  }
  .chip-toggle.on{background:var(--teal-bg); border-color:var(--teal); color:var(--teal);}
  .btn{
    display:inline-block; border:1px solid var(--ink); background:var(--ink); color:#fff;
    padding:9px 16px; font-size:0.85rem; font-weight:600; border-radius:3px;
  }
  .btn:hover{opacity:0.9;}
  .btn.secondary{background:var(--panel); color:var(--text); border-color:var(--line);}
  .btn.danger{background:var(--rust); border-color:var(--rust);}
  .btn.ghost{background:none; border:1px solid var(--line); color:var(--text-soft); padding:5px 10px; font-size:0.76rem;}
  .btn.small{padding:5px 10px; font-size:0.76rem;}
  .row{display:flex; gap:8px; flex-wrap:wrap; align-items:center;}

  /* ---- Timeline (Itinerary) ---- */
  .day-tabs{display:flex; gap:6px; overflow-x:auto; margin-bottom:14px; padding-bottom:2px;}
  .day-tab{
    flex-shrink:0; padding:8px 13px; border:1px solid var(--line); background:var(--panel); font-size:0.8rem; text-align:center;
  }
  .day-tab .d1{font-weight:600;} .day-tab .d2{font-size:0.68rem; color:var(--text-soft);}
  .day-tab.active{border-color:var(--amber); background:var(--amber-bg);}
  .daybar{display:flex; justify-content:space-between; align-items:baseline; border-bottom:2px solid var(--ink); padding-bottom:8px; margin-bottom:4px;}
  .daybar h3{margin:0; font-size:1.05rem;}
  .daybar .cost{font-family:'IBM Plex Mono',monospace; font-size:0.85rem;}
  .daybar .cost.over{color:var(--rust);}
  .timeline{position:relative; margin:10px 0 22px; padding-left:64px;}
  .titem{position:relative; padding:10px 0 10px 16px; border-left:1px solid var(--line); margin-left:2px;}
  .titem::before{content:""; position:absolute; left:-4.5px; top:16px; width:8px; height:8px; border-radius:50%; background:var(--ink);}
  .titem.meal::before, .titem.transport::before{background:var(--text-soft);}
  .titem .time{position:absolute; left:-80px; top:9px; width:66px; text-align:right; font-family:'IBM Plex Mono',monospace; font-size:0.72rem; color:var(--text-soft);}
  .titem .tname{font-weight:600; font-size:0.92rem;}
  .titem .tmeta{font-size:0.76rem; color:var(--text-soft); margin-top:2px;}
  .titem .tcost{font-family:'IBM Plex Mono',monospace; font-size:0.78rem; margin-top:2px;}
  .titem .actions{margin-top:6px; display:flex; gap:6px; flex-wrap:wrap;}
  .tag{display:inline-block; font-size:0.68rem; padding:1px 7px; border-left:3px solid var(--text-soft); background:var(--paper); margin-right:4px;}
  .conflict-banner{background:var(--rust-bg); border-left:3px solid var(--rust); padding:9px 12px; font-size:0.82rem; margin-bottom:10px;}
  .empty-note{color:var(--text-soft); font-size:0.85rem; padding:10px 0;}
  .suggest-box{background:var(--teal-bg); border-left:3px solid var(--teal); padding:10px 12px; font-size:0.82rem; margin-top:8px;}
  .suggest-box .sitem{display:flex; justify-content:space-between; align-items:center; padding:4px 0; gap:8px;}

  /* ---- Places tab ---- */
  table{width:100%; border-collapse:collapse; font-size:0.82rem;}
  th{text-align:left; font-size:0.68rem; color:var(--text-soft); font-weight:600; letter-spacing:0.03em; border-bottom:1px solid var(--line); padding:6px 8px;}
  td{padding:7px 8px; border-bottom:1px solid var(--line); vertical-align:top;}
  tr.cancelled td{opacity:0.45; text-decoration:line-through;}
  .status-pill{font-size:0.68rem; padding:2px 7px; border-radius:2px;}
  .status-pill.scheduled{background:var(--teal-bg); color:var(--teal);}
  .status-pill.backup{background:var(--amber-bg); color:var(--amber);}
  .status-pill.cancelled{background:var(--rust-bg); color:var(--rust);}

  /* ---- Ask tab ---- */
  .ask-box{display:flex; gap:8px; margin-bottom:14px;}
  .ask-box input{flex:1;}
  .examples{display:flex; flex-wrap:wrap; gap:6px; margin-bottom:18px;}
  .ex-chip{font-size:0.76rem; padding:5px 10px; border:1px solid var(--line); background:var(--panel); color:var(--text-soft);}
  .ex-chip:hover{border-color:var(--amber); color:var(--text);}
  .answer{background:var(--ink); color:#EDEFEF; padding:16px 18px; font-family:'IBM Plex Mono',monospace; font-size:0.86rem; line-height:1.6; white-space:pre-wrap; border-left:3px solid var(--amber); margin-bottom:14px;}
  .answer b{color:var(--amber);}
  .qa-log{display:flex; flex-direction:column-reverse; gap:14px;}
  .qa-item .q{font-weight:600; margin-bottom:6px; font-size:0.88rem;}

  /* ---- Dashboard ---- */
  .dash-grid{display:grid; grid-template-columns:1fr 1fr; gap:16px;}
  @media (max-width:700px){.dash-grid{grid-template-columns:1fr;}}
  .stat{display:flex; justify-content:space-between; padding:6px 0; border-bottom:1px dashed var(--line); font-size:0.85rem;}
  .stat .n{font-family:'IBM Plex Mono',monospace; font-weight:600;}
  .bignum{font-family:'IBM Plex Mono',monospace; font-size:1.6rem; font-weight:600;}
  @media print{
    .rail,.board .spacer,.no-print,.ask-box,.examples{display:none !important;}
    body{background:#fff; color:#000;}
    .panel{border:1px solid #999; break-inside:avoid;}
    .shell{display:block;}
    .main{padding:0;}
  }
  .notice{position:fixed; bottom:calc(16px + env(safe-area-inset-bottom,0px)); left:50%; transform:translateX(-50%); background:var(--ink); color:#fff; padding:10px 18px; font-size:0.82rem; z-index:50; border-left:3px solid var(--amber); max-width:90vw;}
</style>
</head>
<body>
<div id="app"></div>

<script>
/* ============================== CONSTANTS ============================== */
const CATEGORIES = ['History','Food','Art','Nature','Nightlife','Shopping','Adventure','Relaxation','Family'];
const PACE_MINUTES = { relaxed:300, moderate:420, packed:540 };
const TRAVEL_SAME = { min:15, cost:0 };
const TRAVEL_DIFF = { min:40, cost:150 };
const MEAL_LUNCH_COST = 250;
const MEAL_DINNER_COST = 350;
const CAT_COLOR = {
  History:'#8A5A34', Food:'#B8462F', Art:'#5B4B8A', Nature:'#2F6F62', Nightlife:'#6A3B6E',
  Shopping:'#946A2B', Adventure:'#B8462F', Relaxation:'#2F6F62', Family:'#3B5C8A', Transit:'#51606F'
};

/* ============================== HELPERS ============================== */
function uid(p){ return p+'_'+Math.random().toString(36).slice(2,9); }
function pad2(n){ return String(n).padStart(2,'0'); }
function toISO(d){ return d.getFullYear()+'-'+pad2(d.getMonth()+1)+'-'+pad2(d.getDate()); }
function addDays(iso,n){ const d=new Date(iso+'T00:00:00'); d.setDate(d.getDate()+n); return toISO(d); }
function diffDays(iso1,iso2){ const a=new Date(iso1+'T00:00:00'), b=new Date(iso2+'T00:00:00'); return Math.round((b-a)/86400000)+1; }
function timeStrToMin(s){ if(!s) return 0; const p=s.split(':'); return (+p[0])*60+(+p[1]); }
function minToTimeStr(m){ const h=Math.floor(m/60)%24, mm=m%60; return pad2(h)+':'+pad2(mm); }
function fmtTime(min){ let h=Math.floor(min/60), m=min%60; const ap=h>=12?'PM':'AM'; let h12=h%12; if(h12===0)h12=12; return h12+':'+pad2(m)+' '+ap; }
const WD=['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
const MO=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
function fmtDateLabel(iso){ const d=new Date(iso+'T00:00:00'); return WD[d.getDay()]+', '+d.getDate()+' '+MO[d.getMonth()]; }
function fmtDateShort(iso){ const d=new Date(iso+'T00:00:00'); return d.getDate()+' '+MO[d.getMonth()]; }
function fmtMoney(n){ return (state.currency||'₹') + Math.round(n||0).toLocaleString('en-IN'); }
function escapeHtml(s){ return String(s==null?'':s).replace(/[&<>"']/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }
function catColor(c){ return CAT_COLOR[c] || '#51606F'; }
function sumCosts(items){ return items.reduce((s,i)=>s+(i.cost||0),0); }

/* ============================== SEED DATA ============================== */
function seedPois(){
  const P = [
    ['Old Fort Walking Tour','Old Town','History','08:00','18:00',90,300,4],
    ['Spice Market Wander','Old Town','Food','09:00','20:00',60,0,3],
    ['Old Town Cathedral','Old Town','History','09:00','17:00',45,100,3],
    ['Rooftop Sunset Café','Old Town','Relaxation','16:00','23:00',60,400,3],
    ['Riverside Art Museum','Riverside','Art','10:00','18:00',120,500,4],
    ['River Cruise','Riverside','Nature','11:00','19:00',90,800,5],
    ['Riverside Night Market','Riverside','Nightlife','18:00','23:00',90,200,3],
    ['Botanical Gardens','Riverside','Nature','07:00','19:00',100,150,4],
    ['Modern Art Gallery','Uptown','Art','10:00','19:00',75,350,3],
    ['Uptown Shopping District','Uptown','Shopping','10:00','21:00',90,0,2],
    ['Adventure Zipline Park','Uptown','Adventure','09:00','17:00',150,1200,5],
    ['Skyline Observation Deck','Uptown','Relaxation','09:00','22:00',60,600,3],
    ['Uptown Food Street','Uptown','Food','12:00','23:00',75,350,4],
    ['Family Aquarium','Uptown','Family','09:00','18:00',120,450,3]
  ];
  return P.map(r=>({
    id: uid('poi'), name:r[0], area:r[1], category:r[2],
    openMin:timeStrToMin(r[3]), closeMin:timeStrToMin(r[4]),
    durationMin:r[5], cost:r[6], priority:r[7], cancelled:false
  }));
}

function defaultState(){
  const today = new Date();
  const start = new Date(today); start.setDate(start.getDate()+14);
  const startIso = toISO(start);
  const endIso = addDays(startIso,3);
  return {
    currency:'₹',
    trip:{
      destination:'Kyoto, Japan', startDate:startIso, endDate:endIso,
      totalBudget:60000, pace:'moderate', dayStartMin:540, dayEndMin:1290,
      interests:['History','Food','Art','Nature']
    },
    accommodation:{ name:'Riverside Inn', area:'Riverside', costPerNight:3000 },
    transport:[
      { id:uid('tr'), name:'Airport Arrival Transfer', date:startIso, startMin:540, endMin:600, cost:800, area:'Riverside' },
      { id:uid('tr'), name:'Airport Departure Transfer', date:endIso, startMin:1020, endMin:1080, cost:800, area:'Riverside' }
    ],
    pois: seedPois(),
    days:[],
    backupPool:[],
    currentDayIndex:0,
    activeTab:'setup',
    editingPoiId:null,
    editingTransportId:null,
    qaLog:[],
    notice:null
  };
}

/* ============================== STATE LOAD/SAVE ============================== */
let state;
function save(){ try{ localStorage.setItem('travelpilot_v1', JSON.stringify(state)); }catch(e){ /* storage unavailable */ } }
function load(){
  try{
    const raw = localStorage.getItem('travelpilot_v1');
    if(raw){ state = JSON.parse(raw); return true; }
  }catch(e){ /* ignore */ }
  return false;
}

/* ============================== SCHEDULING ENGINE ============================== */
function activityItemFromPoi(poi,startMin){
  return { id:uid('it'), type:'activity', name:poi.name, category:poi.category, area:poi.area,
    startMin, endMin:startMin+poi.durationMin, cost:poi.cost, status:'planned', poiId:poi.id, source:{...poi} };
}
function transferItem(fromArea,toArea,startMin,durMin,cost){
  return { id:uid('tf'), type:'transport', name:'Transfer: '+fromArea+' → '+toArea, category:'Transit', area:toArea,
    startMin, endMin:startMin+durMin, cost, status:'auto', isTransfer:true };
}
function mealItem(name,startMin,durMin,cost){
  return { id:uid('ml'), type:'meal', name, category:'Food', area:null, startMin, endMin:startMin+durMin, cost, status:'auto' };
}
function transportBookingItem(tr){
  return { id:tr.id, type:'transport', name:tr.name, category:'Transit', area:tr.area,
    startMin:tr.startMin, endMin:tr.endMin, cost:tr.cost, status:'booked', locked:true };
}

function scoreOf(poi,currentArea,interests){
  let s=0;
  if(poi.area===currentArea) s+=3;
  if(interests && interests.includes(poi.category)) s+=2;
  s += (poi.priority||3)*0.5;
  s -= (poi.cost||0)/1000;
  return s;
}
function pickBestCandidate(pool,area,cursor,segEnd,interests,budgetRemaining){
  const eligible = pool.filter(p=>{
    const travelMin = p.area===area ? TRAVEL_SAME.min : TRAVEL_DIFF.min;
    const arrival = Math.max(cursor+travelMin, p.openMin);
    return arrival+p.durationMin<=segEnd && arrival+p.durationMin<=p.closeMin;
  });
  if(!eligible.length) return null;
  const withinBudget = eligible.filter(p=>{
    const travelCost = p.area===area ? TRAVEL_SAME.cost : TRAVEL_DIFF.cost;
    return (travelCost+p.cost) <= budgetRemaining;
  });
  const candidates = withinBudget.length ? withinBudget : eligible;
  candidates.sort((a,b)=>scoreOf(b,area,interests)-scoreOf(a,area,interests));
  return candidates[0];
}
function fillSegment(pool, cursor, area, segEnd, budgetRemaining, interests, paceRemaining, mealState){
  const items=[]; let spent=0, used=0; let workPool=pool.slice(); let guard=0;
  while(cursor<segEnd && used<paceRemaining && guard<30){
    guard++;
    if(!mealState.lunch && cursor>=690 && cursor<840 && cursor+60<=segEnd){
      items.push(mealItem('Lunch break',cursor,60,MEAL_LUNCH_COST)); spent+=MEAL_LUNCH_COST; cursor+=60; mealState.lunch=true; continue;
    }
    if(!mealState.dinner && cursor>=1140 && cursor<1290 && cursor+60<=segEnd){
      items.push(mealItem('Dinner',cursor,60,MEAL_DINNER_COST)); spent+=MEAL_DINNER_COST; cursor+=60; mealState.dinner=true; continue;
    }
    const candidate = pickBestCandidate(workPool, area, cursor, segEnd, interests, budgetRemaining-spent);
    if(!candidate) break;
    const travel = candidate.area===area ? TRAVEL_SAME : TRAVEL_DIFF;
    const arrival = Math.max(cursor+travel.min, candidate.openMin);
    if(arrival+candidate.durationMin>segEnd || arrival+candidate.durationMin>candidate.closeMin){
      workPool = workPool.filter(p=>p.id!==candidate.id); continue;
    }
    if(candidate.area!==area){ items.push(transferItem(area,candidate.area,cursor,travel.min,travel.cost)); spent+=travel.cost; }
    items.push(activityItemFromPoi(candidate,arrival));
    spent += candidate.cost; used += candidate.durationMin;
    cursor = arrival+candidate.durationMin; area = candidate.area;
    workPool = workPool.filter(p=>p.id!==candidate.id);
  }
  return { items, spent, used, pool:workPool };
}
function scheduleDay(fixedAnchors, pool, tripSettings, startArea, dayBudget){
  let segments=[]; let prevEnd=tripSettings.dayStartMin; let prevArea=startArea;
  for(const a of fixedAnchors){ segments.push({start:prevEnd,end:a.startMin,area:prevArea}); prevEnd=a.endMin; prevArea=a.area||prevArea; }
  segments.push({start:prevEnd,end:tripSettings.dayEndMin,area:prevArea});
  let allItems = fixedAnchors.map(a=>a.item);
  let spentTotal=0, usedTotal=0, workPool=pool.slice();
  const mealState={lunch:false,dinner:false};
  for(const seg of segments){
    if(seg.start>=seg.end) continue;
    const res = fillSegment(workPool, seg.start, seg.area, seg.end, dayBudget-spentTotal, tripSettings.interests, tripSettings.paceMinutes-usedTotal, mealState);
    allItems = allItems.concat(res.items);
    spentTotal += res.spent; usedTotal += res.used; workPool = res.pool;
  }
  allItems.sort((a,b)=>a.startMin-b.startMin);
  return { 
