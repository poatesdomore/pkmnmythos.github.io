# pkmnmythos.github.io
Mythos Chart
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Pokémon Mythos — C-Gear Constellation</title>
<link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg-deep: #050d14;
  --bg-grid: #0a1726;
  --panel-bg: rgba(6, 15, 25, 0.95);
  --text-main: #E8F0F8;
  --text-muted: #8AA4C4;
  --c-cyan: #00F0FF;
  --c-magenta: #FF0055;
  --c-yellow: #FFD700;
  --border-dim: rgba(0, 240, 255, 0.3);
}
* { margin:0; padding:0; box-sizing:border-box; }
body {
  background-color: var(--bg-deep);
  background-image: linear-gradient(var(--bg-grid) 1px, transparent 1px), linear-gradient(90deg, var(--bg-grid) 1px, transparent 1px);
  background-size: 30px 30px;
  background-position: center center;
  color: var(--text-main);
  font-family: 'Chakra Petch', sans-serif;
  height: 100vh; height: 100dvh;
  overflow: hidden;
  display: flex; flex-direction: column;
}
header {
  padding: 12px 20px;
  border-bottom: 2px solid var(--c-cyan);
  background: rgba(5, 13, 20, 0.95);
  display: flex; align-items: center; justify-content: space-between;
  flex-shrink: 0; z-index: 20;
  box-shadow: 0 4px 25px rgba(0, 240, 255, 0.15);
}
h1 { font-size: 1.3rem; font-weight: 700; letter-spacing: 0.2em; color: #FFF; text-transform: uppercase; text-shadow: 0 0 10px rgba(0, 240, 255, 0.5); }
h1 span { color: var(--c-cyan); }
.tabs { display: flex; gap: 8px; }
.tab {
  padding: 6px 16px;
  border: 1px solid rgba(0, 240, 255, 0.2);
  background: rgba(0, 240, 255, 0.05);
  color: var(--text-muted);
  font-family: 'Chakra Petch', sans-serif;
  font-size: 0.8rem; font-weight: 600;
  letter-spacing: 0.15em; text-transform: uppercase;
  cursor: pointer; border-radius: 4px; transition: all 0.2s;
  clip-path: polygon(10px 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%, 0 10px);
}
.tab:hover { border-color: var(--c-cyan); color: #FFF; }
.tab.active { background: rgba(0, 240, 255, 0.15); border-color: var(--c-cyan); color: var(--c-cyan); text-shadow: 0 0 8px var(--c-cyan); }
.main { flex: 1; display: flex; position: relative; overflow: hidden; }
.canvas-area { flex: 1; position: relative; overflow: hidden; }
#mainSvg { width: 100%; height: 100%; display: block; touch-action: none; cursor: grab; }
#mainSvg:active { cursor: grabbing; }
@keyframes spin { 100% { transform: rotate(360deg); } }
.radar-ring { transform-origin: 900px 500px; }
.spin-slow { animation: spin 45s linear infinite; }
.spin-slow-rev { animation: spin 60s linear infinite reverse; }
.spin-fast { animation: spin 25s linear infinite; }
.node-group { cursor: pointer; }
.node-group:hover .node-hex { filter: brightness(1.3); }
.connection { fill: none; transition: opacity 0.25s, stroke-width 0.25s; }
.filter-bar {
  position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
  display: flex; gap: 8px;
  background: rgba(5,13,20,0.95); backdrop-filter: blur(8px);
  padding: 10px 15px;
  border: 1px solid var(--border-dim); border-radius: 8px;
  white-space: nowrap; max-width: 90%; overflow-x: auto;
  -webkit-overflow-scrolling: touch; z-index: 100;
  box-shadow: 0 5px 20px rgba(0,0,0,0.5);
}
.filter-bar::-webkit-scrollbar { display: none; }
.filter-btn {
  padding: 6px 12px;
  font-family: 'Chakra Petch', sans-serif;
  font-size: 0.75rem; font-weight: 600;
  letter-spacing: 0.1em; text-transform: uppercase;
  border: 1px solid transparent;
  background: rgba(255,255,255,0.05);
  cursor: pointer; transition: all 0.2s; color: var(--text-muted); border-radius: 4px;
  display: flex; align-items: center; gap: 5px;
}
.filter-btn.active { border-color: currentColor; color: inherit; background: rgba(255,255,255,0.1); }
.filter-btn:not(.active) { opacity: 0.4; }
.details-panel {
  position: absolute; top: 0; right: 0; bottom: 0; width: 400px;
  background: var(--panel-bg); border-left: 2px solid var(--c-cyan);
  box-shadow: -5px 0 40px rgba(0,0,0,0.9);
  display: flex; flex-direction: column;
  transform: translateX(105%);
  transition: transform 0.4s cubic-bezier(0.2, 0.8, 0.2, 1);
  z-index: 200; backdrop-filter: blur(10px);
}
.details-panel.open { transform: translateX(0); }
.details-panel.minimized { transform: translateX(calc(100% - 30px)); }
#panelContent { padding: 24px; padding-left: 45px; overflow-y: auto; flex: 1; opacity: 1; transition: opacity 0.3s; }
.details-panel.minimized #panelContent { opacity: 0; pointer-events: none; }
#panelContent::-webkit-scrollbar { width: 4px; }
#panelContent::-webkit-scrollbar-track { background: transparent; }
#panelContent::-webkit-scrollbar-thumb { background: var(--c-cyan); }
.panel-toggle-btn {
  position: absolute; left: 0; top: 50%; transform: translateY(-50%);
  width: 30px; height: 100px;
  background: rgba(0, 240, 255, 0.1); border: 1px solid var(--c-cyan); border-left: none;
  color: var(--c-cyan); display: flex; align-items: center; justify-content: center;
  font-weight: bold; cursor: pointer; z-index: 10;
  border-radius: 0 8px 8px 0; transition: background 0.2s, color 0.2s;
}
.panel-toggle-btn:hover { background: var(--c-cyan); color: #000; }
.details-panel.minimized .panel-toggle-btn {
  left: -30px; border-left: 1px solid var(--c-cyan); border-right: none;
  border-radius: 8px 0 0 8px; background: rgba(6, 15, 25, 0.95);
}
.panel-close-btn {
  position: absolute; top: 15px; right: 20px;
  font-size: 1.5rem; color: var(--c-cyan); cursor: pointer; z-index: 10;
  line-height: 1; transition: transform 0.2s; background: none; border: none;
}
.panel-close-btn:hover { transform: scale(1.2); color: #FFF; }
.details-panel h2 {
  font-size: 1.6rem; font-weight: 700; color: #FFF; margin-bottom: 2px;
  text-transform: uppercase; letter-spacing: 0.1em; text-shadow: 0 0 10px rgba(0,240,255,0.4);
}
.details-panel .role { font-size: 0.75rem; font-weight: 600; letter-spacing: 0.15em; text-transform: uppercase; color: var(--c-magenta); margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px dashed rgba(255,255,255,0.15); }
.details-panel .desc { font-size: 0.9rem; line-height: 1.6; color: var(--text-main); margin-bottom: 20px; }
.section-head { font-size: 0.75rem; font-weight: 700; letter-spacing: 0.2em; text-transform: uppercase; color: var(--text-muted); margin-bottom: 10px; margin-top: 15px; border-left: 3px solid var(--c-yellow); padding-left: 8px; }
.conn-item { display: flex; align-items: flex-start; gap: 10px; margin-bottom: 10px; font-size: 0.85rem; color: #CCC; line-height: 1.4; background: rgba(255,255,255,0.03); padding: 6px; border-radius: 4px; }
.conn-dot { width: 12px; height: 12px; clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%); flex-shrink: 0; margin-top: 3px; }
.legend-btn {
  position: absolute; top: 20px; left: 20px;
  background: var(--panel-bg); border: 1px solid var(--c-cyan); color: var(--c-cyan);
  padding: 8px 16px; font-family: 'Chakra Petch', sans-serif;
  font-size: 0.8rem; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase;
  cursor: pointer; z-index: 30;
  clip-path: polygon(10px 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%, 0 10px);
}
.legend-btn:hover { background: rgba(0,240,255,0.1); }
@media (max-width: 768px) {
  header { flex-direction: column; gap: 10px; align-items: flex-start; }
  .tabs { width: 100%; overflow-x: auto; padding-bottom: 4px; }
  .details-panel { top: auto; left: 0; right: 0; bottom: 0; width: 100%; height: 65vh; border-left: none; border-top: 2px solid var(--c-cyan); transform: translateY(105%); border-radius: 15px 15px 0 0; }
  .details-panel.open { transform: translateY(0); }
  .details-panel.minimized { transform: translateY(calc(100% - 30px)); }
  .panel-toggle-btn { left: 50%; top: 0; transform: translateX(-50%); width: 100px; height: 30px; border-radius: 0 0 8px 8px; border: 1px solid var(--c-cyan); border-top: none; }
  .details-panel.minimized .panel-toggle-btn { top: -30px; left: 50%; border-top: 1px solid var(--c-cyan); border-bottom: none; border-radius: 8px 8px 0 0; }
  #panelContent { padding-left: 24px; padding-top: 45px; }
}
</style>
</head>
<body>
<header>
  <h1>Pokémon <span>Mythos</span></h1>
  <div class="tabs">
    <button class="tab active" data-view="characters">Characters</button>
    <button class="tab" data-view="themes">Themes</button>
    <button class="tab" data-view="narrative">Threads</button>
  </div>
</header>
<div class="main">
  <button class="legend-btn" id="legendBtn">[≡] Legend</button>
  <div class="canvas-area" id="canvasArea">
    <svg id="mainSvg" viewBox="0 0 1800 1000" preserveAspectRatio="xMidYMid meet">
      <defs>
        <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
          <feGaussianBlur stdDeviation="5" result="blur"/>
          <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
        </filter>
      </defs>
      <g id="viewportGroup">
        <g id="c-gear-radar" opacity="0.4">
          <line x1="900" y1="-1000" x2="900" y2="3000" stroke="rgba(0,240,255,0.2)" stroke-width="1" stroke-dasharray="4 4"/>
          <line x1="-1000" y1="500" x2="3000" y2="500" stroke="rgba(0,240,255,0.2)" stroke-width="1" stroke-dasharray="4 4"/>
          <circle cx="900" cy="500" r="200" class="radar-ring spin-slow" fill="none" stroke="var(--c-cyan)" stroke-width="2" stroke-dasharray="10 30" opacity="0.6"/>
          <circle cx="900" cy="500" r="340" class="radar-ring spin-slow-rev" fill="none" stroke="var(--c-magenta)" stroke-width="1.5" stroke-dasharray="20 40 5 10" opacity="0.4"/>
          <circle cx="900" cy="500" r="480" class="radar-ring spin-fast" fill="none" stroke="var(--c-yellow)" stroke-width="1" stroke-dasharray="2 15" opacity="0.3"/>
        </g>
        <g id="connections-layer"></g>
        <g id="nodes-layer"></g>
      </g>
    </svg>
    <div class="filter-bar" id="filterBar">
      <button class="filter-btn active" data-filter="ally" style="color:#00FF88">■ Ally</button>
      <button class="filter-btn active" data-filter="family" style="color:#FF0055">■ Family</button>
      <button class="filter-btn active" data-filter="mentor" style="color:#FFD700">■ Mentor</button>
      <button class="filter-btn active" data-filter="manipulation" style="color:#9D00FF">■ Manipulation</button>
      <button class="filter-btn active" data-filter="betrayal" style="color:#FF6600">■ Betrayal</button>
      <button class="filter-btn active" data-filter="conflict" style="color:#FF3333">■ Conflict</button>
      <button class="filter-btn active" data-filter="cosmic" style="color:#FFFFFF">■ Cosmic</button>
    </div>
  </div>
  <div class="details-panel" id="detailsPanel">
    <button class="panel-toggle-btn" id="toggleBtn">〉</button>
    <button class="panel-close-btn" id="closeBtn">✕</button>
    <div id="panelContent"></div>
  </div>
</div>
<script>
const NS = 'http://www.w3.org/2000/svg';

// ── State ──
let selectedId = null;
let activeFilters = new Set(['ally','family','mentor','manipulation','betrayal','conflict','cosmic','parallel']);
let currentView = 'characters';
let panX = 0, panY = 0, scale = 1;
let isPanning = false, hasDragged = false;
let dragStartPanX = 0, dragStartPanY = 0, dragOriginX = 0, dragOriginY = 0;
let initialPinchDist = null, initialPinchScale = 1;

// ── Colour map ──
const C = {
  hero:'#00F0FF', ally:'#00FF88', antagonist:'#FF0055',
  jade:'#FFD700', cosmic:'#FFFFFF', redeemed:'#9D00FF',
  cAlly:'#00FF88', cFamily:'#FF0055', cMentor:'#FFD700',
  cManip:'#9D00FF', cBetrayal:'#FF6600', cConflict:'#FF3333',
  cCosmic:'#FFFFFF', cParallel:'#0088FF',
};
function getColor(g){ return C[g]||'#888'; }
function getConnColor(t){ return C['c'+t.charAt(0).toUpperCase()+t.slice(1)]||'#888'; }

// ── Data ──
const CHARS=[
  {id:'arceus',label:'ARCEUS',sub:'The Creator',x:900,y:100,r:22,group:'cosmic',role:'Divine Entity',desc:'The creator of the universe whose true nature is genuinely alien rather than simply benevolent. Considers a cosmic reset at the story\'s climax based on pure preference.',themes:['Divine vs Human Ethics','Cosmic Horror of Indifference','Religion & Fundamentalism (shatters)'],threads:['The Yuragi Crisis','Divine Reckoning']},
  {id:'giratina',label:'GIRATINA',sub:'The Renegade',x:1150,y:130,r:18,group:'cosmic',role:'Cosmic Antagonist',desc:'Resentful of its banishment. Uses Volo as a centuries-long plate-gathering instrument while remaining hidden.',themes:['Cosmic Horror of Indifference','Blind Ambition (resentment)'],threads:['Yuragi Crisis','Galactic Inc. Conspiracy','Divine Reckoning']},
  {id:'player',label:'PLAYER',sub:'Wendi / Wilford',x:600,y:250,r:20,group:'hero',role:'Protagonist',desc:'An 18-year-old Unovan on scholarship at Oak University. Accepts the chosen one mantle by choice rather than destiny.',themes:['Legacy vs Self-Determination','Value of Bonds','Identity'],threads:['Gym Challenge','Team Rocket Investigation','The Prophecy Arc','Yuragi Crisis','Divine Reckoning']},
  {id:'nat',label:'NAT',sub:"N's Daughter",x:420,y:200,r:20,group:'hero',role:'Rival / Best Friend',desc:'Intellectually brilliant and deeply resentful of being defined by N\'s legacy. Vehemently rejects the Jade Clan prophecy.',themes:['Legacy vs Self-Determination','Identity','Parental Relationships','Value of Bonds'],threads:['The Prophecy Arc','Team Rocket Investigation','Divine Reckoning','Post-Game: Alder']},
  {id:'n',label:'N',sub:'Plasma Foundation',x:300,y:320,r:17,group:'hero',role:"Nat's Father",desc:'Globally respected head of the Plasma Foundation. Arrives at the Act 3 crisis peak.',themes:['Legacy vs Self-Determination','Parental Relationships','Value of Bonds'],threads:['Divine Reckoning','Post-Game: Alder']},
  {id:'cynthia',label:'CYNTHIA',sub:'Professor / Mentor',x:400,y:480,r:17,group:'ally',role:'Professor of Battle & Archaeology',desc:'Former Sinnoh Champion, now Oak University professor. Her archaeological expertise drives the discovery of Clay\'s ruins.',themes:['Value of Truth','Value of Bonds'],threads:['Team Rocket Investigation','Clay\'s Ruins Revelation','Yuragi Crisis','Divine Reckoning']},
  {id:'duco',label:'DUCO',sub:'Jade Clan — Scholar',x:350,y:620,r:14,group:'jade',role:'Scholarly Prophet',desc:'Measured and open to reinterpreting prophecy based on emerging evidence. Aligned with Cynthia\'s archaeological approach.',themes:['Religion & Fundamentalism','Value of Truth','Divine vs Human Ethics'],threads:['The Prophecy Arc','Clay\'s Ruins Revelation','Divine Reckoning']},
  {id:'lexi',label:'LEXI',sub:'Jade Clan — Dogmatist',x:320,y:720,r:14,group:'jade',role:'Dogmatic Prophet',desc:'Fervent and insistent on the prophecy\'s literal fulfilment. Her final confrontation is a tragic portrait of fundamentalism.',themes:['Religion & Fundamentalism','Identity','Blind Ambition'],threads:['The Prophecy Arc','The Ascent to Spear Pillar','Divine Reckoning']},
  {id:'dawn',label:'DAWN',sub:'Champion / Interim PM',x:650,y:560,r:17,group:'ally',role:'Co-Champion of Nohin',desc:'Co-Champion with deep institutional faith. Her arc is structured around the systematic destruction of that faith.',themes:['Political Corruption','Value of Bonds','Identity'],threads:['Gym Challenge','Team Rocket Investigation','Political Betrayal','Divine Reckoning']},
  {id:'ethan',label:'ETHAN',sub:'Co-Champion',x:750,y:680,r:14,group:'ally',role:'Investigation Team',desc:'Witnessed Silver\'s original moral growth. Provides tactical support and experienced battling acumen throughout Act 3.',themes:['Redemption','Value of Bonds'],threads:['Team Rocket Investigation','Silver\'s Redemption Arc','Divine Reckoning']},
  {id:'lillie',label:'LILLIE',sub:'Plasma Foundation Intel',x:880,y:280,r:15,group:'ally',role:'Primary Investigator',desc:'Conducts the most thorough independent investigation of the entire story. Without Lillie, the conspiracy is never fully exposed.',themes:['Value of Truth','Political Corruption'],threads:['Galactic Inc. Conspiracy','Political Betrayal']},
  {id:'colress',label:'COLRESS',sub:'Scientist / Defector',x:1020,y:280,r:15,group:'ally',role:'Galactic Inc. Defector',desc:'His defection is driven by scientific horror at the catastrophic scale of Volo/Saturn\'s plan.',themes:['Blind Ambition','Value of Bonds','Redemption'],threads:['Galactic Inc. Conspiracy','Mega Evolution Arc','Divine Reckoning']},
  {id:'lucas',label:'LUCAS',sub:'Hisui Expert / Faller',x:1150,y:500,r:15,group:'ally',role:'Time-Displaced Protagonist',desc:'PLA protagonist, sent to ancient Hisui by Arceus and returned to the present through a space-time rift.',themes:['Legacy vs Self-Determination','Divine vs Human Ethics'],threads:['Yuragi Crisis','Volo Identification','Post-Game: Alder']},
  {id:'clay',label:'CLAY',sub:'Mining Magnate',x:600,y:700,r:13,group:'ally',role:'Logistics Ally',desc:'His mining operations accidentally unearth the ancient ruin complex that triggers the story\'s most important revelation.',themes:['Value of Bonds'],threads:["Clay's Ruins Revelation",'Act 3 Underground Resistance']},
  {id:'silver',label:'SILVER',sub:"Giovanni's Son",x:920,y:700,r:15,group:'redeemed',role:'Redeemed Antagonist',desc:'Son of Giovanni, leading a new inept Team Rocket out of misguided filial duty. Ethan\'s appeal breaks him free.',themes:['Legacy vs Self-Determination','Redemption','Parental Relationships','Grief as Corruption'],threads:['Team Rocket Investigation',"Silver's Redemption Arc",'Divine Reckoning']},
  {id:'saturn',label:'SATURN',sub:'CEO Galactic Inc.',x:1350,y:350,r:18,group:'antagonist',role:'Tragic Antagonist',desc:'Built an entire corporate empire as an elaborate grief ritual for Cyrus. Manipulated by Volo specifically through that grief.',themes:['Grief as Corruption','Blind Ambition','Political Corruption'],threads:['Galactic Inc. Conspiracy',"Saturn's Breaking Point",'Divine Reckoning']},
  {id:'palmer',label:'PALMER',sub:'Prime Minister',x:1300,y:580,r:15,group:'antagonist',role:'Corrupt Politician',desc:'Privately colluding with Saturn. His betrayal represents systemic institutional corruption rather than individual villainy.',themes:['Political Corruption','Blind Ambition','Corruption of Power'],threads:['Galactic Inc. Conspiracy','Political Betrayal']},
  {id:'volo',label:'VOLO',sub:'Ancient Antagonist',x:1380,y:200,r:17,group:'antagonist',role:'Ancient Manipulator',desc:'An ancient figure from Hisui who has spent centuries plotting to confront Arceus. Patient, ruthless, and convinced of his own mastery.',themes:['Blind Ambition','Corruption of Power','Divine vs Human Ethics'],threads:['Galactic Inc. Conspiracy','Volo Identification','Spear Pillar Confrontation']},
];

const CONNECTIONS=[
  {from:'n',to:'nat',type:'family',label:"Father / Daughter — the central parental relationship."},
  {from:'silver',to:'player',type:'family',label:"Both children of famous/infamous fathers seeking identity."},
  {from:'cynthia',to:'player',type:'mentor',label:'Primary mentor relationship. Academic and battle.'},
  {from:'cynthia',to:'nat',type:'mentor',label:'Mentee relationship. Scholarly rigour without prophecy.'},
  {from:'ethan',to:'silver',type:'mentor',label:'Redemption catalyst using shared history.'},
  {from:'player',to:'nat',type:'ally',label:'Best friend and rival. Mirror arcs of legacy vs choice.'},
  {from:'player',to:'dawn',type:'ally',label:'Ally through investigation and league progression.'},
  {from:'player',to:'ethan',type:'ally',label:'Investigation team. Ethan brings legacy context.'},
  {from:'player',to:'clay',type:'ally',label:"Clay's discovery enables Arceus revelation."},
  {from:'player',to:'colress',type:'ally',label:'Colress provides Mega Rings and Keystones.'},
  {from:'player',to:'lucas',type:'ally',label:'Lucas arrives as living proof of faller mechanism.'},
  {from:'player',to:'silver',type:'ally',label:"Silver becomes one of Act 3's most determined fighters."},
  {from:'nat',to:'lillie',type:'ally',label:'Plasma Foundation connection.'},
  {from:'n',to:'player',type:'ally',label:'N joins the team at the crisis peak.'},
  {from:'lillie',to:'cynthia',type:'ally',label:'Parallel investigations converging.'},
  {from:'duco',to:'cynthia',type:'ally',label:'Scholarly alignment on evidence-based lore.'},
  {from:'duco',to:'player',type:'ally',label:"Supports Player as agent of prophecy's spirit."},
  {from:'clay',to:'cynthia',type:'ally',label:'Intersection of commerce and scholarship.'},
  {from:'dawn',to:'ethan',type:'ally',label:'Co-Champions with friendly rivalry.'},
  {from:'lucas',to:'n',type:'ally',label:'Post-game partnership for the Unovan Yuragi.'},
  {from:'colress',to:'cynthia',type:'ally',label:'Research collaboration.'},
  {from:'giratina',to:'volo',type:'manipulation',label:'Patient, total manipulation across centuries.'},
  {from:'volo',to:'saturn',type:'manipulation',label:"Exploits Saturn's specific emotional wound."},
  {from:'volo',to:'silver',type:'manipulation',label:'Exploits filial guilt for a disposable distraction.'},
  {from:'saturn',to:'colress',type:'manipulation',label:'Contracts research under false premises.'},
  {from:'saturn',to:'palmer',type:'manipulation',label:'Corrupt partnership shaping political decisions.'},
  {from:'palmer',to:'dawn',type:'betrayal',label:'Executive order shattering institutional faith.'},
  {from:'volo',to:'saturn',type:'betrayal',label:"Destroys Saturn's entire reason for existing callously."},
  {from:'lexi',to:'nat',type:'conflict',label:'Prophetic pressure Nat specifically resents.'},
  {from:'lexi',to:'player',type:'conflict',label:'Accuses the Player of being a false prophet.'},
  {from:'lexi',to:'duco',type:'conflict',label:'Dogma vs Scholarship.'},
  {from:'player',to:'volo',type:'conflict',label:'The final human battle at Spear Pillar.'},
  {from:'player',to:'giratina',type:'conflict',label:'The cosmic battle against an entity outside strategy.'},
  {from:'arceus',to:'player',type:'cosmic',label:'Judgment test. Universe continues ambiguously.'},
  {from:'arceus',to:'giratina',type:'cosmic',label:'Creator and banished cosmic tension.'},
  {from:'arceus',to:'lucas',type:'cosmic',label:'Proof of the faller mechanism.'},
  {from:'arceus',to:'n',type:'cosmic',label:'Inheritance of the Hero of Hisui lineage.'},
  {from:'arceus',to:'nat',type:'cosmic',label:'Latent abilities developing despite her rejection.'},
  {from:'arceus',to:'lexi',type:'cosmic',label:'Proclamation shatters her literal theology entirely.'},
  {from:'arceus',to:'duco',type:'cosmic',label:'Provides evidence for scholarly recontextualisation.'},
  {from:'giratina',to:'saturn',type:'cosmic',label:'Yuragi feedback loop of cosmic and human damage.'},
  {from:'nat',to:'silver',type:'parallel',label:'PARALLEL: Both finding identity beyond inheritance.'},
  {from:'saturn',to:'silver',type:'parallel',label:'PARALLEL: Corrupted by devotion to an absent figure.'},
  {from:'lexi',to:'saturn',type:'parallel',label:'PARALLEL: Refusing to revise a broken belief system.'},
  {from:'player',to:'lucas',type:'parallel',label:'PARALLEL: Chosen agents across different eras.'},
  {from:'volo',to:'giratina',type:'parallel',label:'PARALLEL: Manipulators manipulating each other.'},
];

const THEMES=[
  {id:'th_legacy',label:'LEGACY VS\nSELF-DETERMINATION',x:400,y:150,chars:['nat','silver','n','player'],desc:"The story's most developed theme. Active negotiation with inherited identity rather than passive acceptance."},
  {id:'th_ambition',label:'BLIND AMBITION\n& GRIEF',x:900,y:150,chars:['volo','saturn','giratina','silver'],desc:"Devotion that has calcified beyond its original purpose into something that causes damage regardless of intent."},
  {id:'th_religion',label:'RELIGION &\nFUNDAMENTALISM',x:1400,y:200,chars:['lexi','duco','arceus'],desc:"What happens when the divine itself appears and violates every theological expectation."},
  {id:'th_divine',label:'DIVINE VS\nHUMAN ETHICS',x:1500,y:500,chars:['arceus','player','n','nat','lexi','duco'],desc:"Arceus operates outside human ethical frameworks. The horror is category difference, not malevolence."},
  {id:'th_bonds',label:'VALUE OF\nBONDS',x:1300,y:750,chars:['player','nat','n','ethan','silver','dawn'],desc:"The counter-weight to cosmic indifference. Mega Evolution is canonically rooted in the bond."},
  {id:'th_corruption',label:'POLITICAL\nCORRUPTION',x:900,y:780,chars:['palmer','saturn','dawn','lillie'],desc:"Systemic rather than individual corruption — a system that enables and rewards complicity."},
  {id:'th_redemption',label:'REDEMPTION',x:500,y:750,chars:['silver','colress','lexi'],desc:"Characters at different stages of redemption's arc. Silver completes it, Colress defects, Lexi shatters."},
  {id:'th_grief',label:'GRIEF AS\nCORRUPTION',x:250,y:450,chars:['saturn','silver','volo'],desc:"Grief as a vector for external manipulation and catastrophic decisions."},
  {id:'th_identity',label:'IDENTITY',x:280,y:280,chars:['nat','player','silver','lexi'],desc:"Who you are when separated from what you're supposed to be. The central dramatic engine."},
  {id:'th_cosmic',label:'COSMIC HORROR\nOF INDIFFERENCE',x:1050,y:420,chars:['arceus','giratina','player','lexi','duco'],desc:"Not the horror of malevolence but the horror of pure preference unbothered by individual effort."},
];

const THREADS=[
  {id:'gym',label:'GYM CHALLENGE',color:'#0088FF',chars:['player','nat','dawn','ethan'],actPresence:[true,true,true,true],desc:'The structural backbone. Gaining full weight in the post-crisis Tournament.',intersects:['Team Rocket Investigation','Divine Reckoning']},
  {id:'rocket',label:'TEAM ROCKET INVESTIGATION',color:'#FFD700',chars:['player','nat','cynthia','ethan','silver','dawn','clay'],actPresence:[true,true,false,false],desc:'The gateway investigation linking petty theft to global conspiracies.',intersects:['Galactic Inc. Conspiracy',"Silver's Redemption Arc"]},
  {id:'prophecy',label:'THE PROPHECY ARC',color:'#00FF88',chars:['nat','player','lexi','duco','cynthia','lucas'],actPresence:[true,true,true,false],desc:"Jade Clan's prophecy targets Nat. Her rejection drives the self-determination theme.",intersects:['Yuragi Crisis','Divine Reckoning',"Clay's Ruins Revelation"]},
  {id:'ruins',label:"CLAY'S RUINS REVELATION",color:'#FFD700',chars:['clay','cynthia','duco','player','n'],actPresence:[false,true,false,false],desc:"Discovery of Arceus's historical pattern of intervention.",intersects:['The Prophecy Arc','Galactic Inc. Conspiracy','Yuragi Crisis']},
  {id:'galactic',label:'GALACTIC INC. CONSPIRACY',color:'#FF0055',chars:['saturn','volo','palmer','lillie','colress','giratina','silver'],actPresence:[true,true,true,false],desc:"Saturn's grief-driven empire and Volo's centuries of patience.",intersects:['Team Rocket Investigation','Political Betrayal','Yuragi Crisis']},
  {id:'political',label:'POLITICAL BETRAYAL',color:'#FF6600',chars:['palmer','saturn','dawn','lillie','player'],actPresence:[false,true,false,false],desc:'Palmer invokes emergency powers to confiscate the Plates.',intersects:['Galactic Inc. Conspiracy','Divine Reckoning']},
  {id:'yuragi',label:'THE YURAGI CRISIS',color:'#FFFFFF',chars:['giratina','volo','saturn','lucas','arceus','player'],actPresence:[true,true,true,false],desc:'Space-time distortions escalating from minor anomalies to chaos.',intersects:['All threads — escalating throughout']},
  {id:'divine',label:'DIVINE RECKONING',color:'#FFFFFF',chars:['arceus','giratina','volo','player','nat','n','lexi','duco','dawn','silver','cynthia'],actPresence:[false,false,true,false],desc:'The convergence of all threads. Volo defeated, Giratina emerges, Arceus descends.',intersects:['All threads — resolution']},
  {id:'postgame',label:'POST-GAME: ALDER',color:'#9D00FF',chars:['player','nat','n','lucas'],actPresence:[false,false,false,true],desc:'Tracking Unovan Yuragi leads to the ultimate historical revelation.',intersects:['The Prophecy Arc','Yuragi Crisis']},
];

// ── SVG helpers ──
function el(tag, attrs={}){
  const e = document.createElementNS(NS, tag);
  for(const [k,v] of Object.entries(attrs)) e.setAttribute(k,v);
  return e;
}
function makeHex(x,y,r){
  const pts=[];
  for(let i=0;i<6;i++){
    const a=(i*60)*Math.PI/180;
    pts.push(`${x+r*Math.cos(a)},${y+r*Math.sin(a)}`);
  }
  return pts.join(' ');
}
function makeCurve(x1,y1,x2,y2){
  const dx=x2-x1, dy=y2-y1, mx=(x1+x2)/2, my=(y1+y2)/2;
  return `M${x1},${y1} Q${mx-dy*0.15},${my+dx*0.15} ${x2},${y2}`;
}

// ── Pan / zoom ──
const svg = document.getElementById('mainSvg');
const vg = document.getElementById('viewportGroup');

function applyTransform(){
  vg.setAttribute('transform',`translate(${panX},${panY}) scale(${scale})`);
}

svg.addEventListener('wheel', e=>{
  e.preventDefault();
  const factor = Math.exp(e.deltaY * -0.002);
  const ns = Math.min(Math.max(0.15, scale*factor), 5);
  const rect = svg.getBoundingClientRect();
  const rx = (e.clientX-rect.left)*(1800/rect.width);
  const ry = (e.clientY-rect.top)*(1000/rect.height);
  panX = rx-(rx-panX)*(ns/scale);
  panY = ry-(ry-panY)*(ns/scale);
  scale = ns;
  applyTransform();
},{passive:false});

svg.addEventListener('pointerdown', e=>{
  // Only start pan on the background (not on child nodes)
  if(e.target !== svg && e.target.closest('.node-group')) return;
  isPanning = true;
  hasDragged = false;
  dragOriginX = e.clientX;
  dragOriginY = e.clientY;
  dragStartPanX = panX;
  dragStartPanY = panY;
  svg.style.cursor = 'grabbing';
});

window.addEventListener('pointermove', e=>{
  if(!isPanning) return;
  const dx = e.clientX - dragOriginX;
  const dy = e.clientY - dragOriginY;
  if(Math.abs(dx)>4||Math.abs(dy)>4) hasDragged = true;
  panX = dragStartPanX + dx;
  panY = dragStartPanY + dy;
  applyTransform();
});

window.addEventListener('pointerup', ()=>{
  isPanning = false;
  svg.style.cursor = 'grab';
  // Reset hasDragged after a brief delay so the click event can still read it
  setTimeout(()=>{ hasDragged = false; }, 50);
});

// Pinch zoom
svg.addEventListener('touchstart', e=>{
  if(e.touches.length===2){
    initialPinchDist = Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
    initialPinchScale = scale;
  }
},{passive:false});

svg.addEventListener('touchmove', e=>{
  if(e.touches.length===2){
    e.preventDefault();
    const d = Math.hypot(e.touches[0].clientX-e.touches[1].clientX, e.touches[0].clientY-e.touches[1].clientY);
    const ns = Math.min(Math.max(0.15, initialPinchScale*(d/initialPinchDist)), 5);
    const cx=(e.touches[0].clientX+e.touches[1].clientX)/2;
    const cy=(e.touches[0].clientY+e.touches[1].clientY)/2;
    const rect=svg.getBoundingClientRect();
    const rx=(cx-rect.left)*(1800/rect.width), ry=(cy-rect.top)*(1000/rect.height);
    panX=rx-(rx-panX)*(ns/scale); panY=ry-(ry-panY)*(ns/scale);
    scale=ns; applyTransform();
  }
},{passive:false});

// Click on blank SVG background — close panel
svg.addEventListener('click', e=>{
  if(hasDragged) return;
  if(e.target === svg || e.target === vg) closePanel();
});

// ── Panel ──
const panel = document.getElementById('detailsPanel');
const toggleBtn = document.getElementById('toggleBtn');

function openPanel(html){
  document.getElementById('panelContent').innerHTML = html;
  panel.classList.remove('minimized');
  panel.classList.add('open');
  toggleBtn.textContent = window.innerWidth<=768 ? 'v' : '〉';
}
function closePanel(){
  panel.classList.remove('open','minimized');
  selectedId = null;
  resetHighlight();
}
function toggleMinimize(){
  if(panel.classList.contains('minimized')){
    panel.classList.remove('minimized');
    toggleBtn.textContent = window.innerWidth<=768 ? 'v' : '〉';
  } else {
    panel.classList.add('minimized');
    toggleBtn.textContent = window.innerWidth<=768 ? 'ʌ' : '〈';
  }
}

toggleBtn.addEventListener('click', toggleMinimize);
document.getElementById('closeBtn').addEventListener('click', closePanel);
document.addEventListener('keydown', e=>{ if(e.key==='Escape') closePanel(); });

// Legend
document.getElementById('legendBtn').addEventListener('click', ()=>{
  openPanel(`
    <h2>SYSTEM LEGEND</h2>
    <div class="role">C-GEAR REFERENCE</div>
    <div class="section-head">NODE CLASSIFICATION</div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.hero}"></div><span style="color:#FFF">Hero / Protagonist</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.ally}"></div><span style="color:#FFF">Ally / Support</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.antagonist}"></div><span style="color:#FFF">Antagonist</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.jade}"></div><span style="color:#FFF">Jade Clan</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cosmic}"></div><span style="color:#FFF">Cosmic / Divine</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.redeemed}"></div><span style="color:#FFF">Redeemed</span></div>
    <div class="section-head">LINK TYPES</div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cAlly}"></div><span style="color:#CCC">Ally Link</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cFamily}"></div><span style="color:#CCC">Family Tie</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cMentor}"></div><span style="color:#CCC">Mentor Link</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cManip}"></div><span style="color:#CCC">Manipulation Path</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cBetrayal}"></div><span style="color:#CCC">Betrayal Path</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cConflict}"></div><span style="color:#CCC">Conflict Path</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cCosmic}"></div><span style="color:#CCC">Cosmic Link</span></div>
    <div class="conn-item"><div class="conn-dot" style="background:${C.cParallel}"></div><span style="color:#CCC">Parallel Arc</span></div>
  `);
});

// ── Tabs ──
document.querySelectorAll('.tab').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    currentView = btn.dataset.view;
    closePanel();
    panX=0; panY=0; scale=1; applyTransform();
    document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
    btn.classList.add('active');
    if(currentView==='characters') renderCharacters();
    else if(currentView==='themes') renderThemes();
    else renderNarrative();
  });
});

// ── Filter bar ──
document.querySelectorAll('.filter-btn').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    const f = btn.dataset.filter;
    if(activeFilters.has(f)){ activeFilters.delete(f); btn.classList.remove('active'); }
    else { activeFilters.add(f); btn.classList.add('active'); }
    if(currentView==='characters') renderCharacters();
  });
});

// ── Character view ──
function renderCharacters(){
  document.getElementById('filterBar').style.display='flex';
  const CL=document.getElementById('connections-layer');
  const NL=document.getElementById('nodes-layer');
  CL.innerHTML=''; NL.innerHTML='';
  const charMap={};
  CHARS.forEach(c=>charMap[c.id]=c);

  CONNECTIONS.forEach(conn=>{
    if(!activeFilters.has(conn.type)) return;
    const f=charMap[conn.from], t=charMap[conn.to];
    if(!f||!t) return;
    const path=el('path',{
      d:makeCurve(f.x,f.y,t.x,t.y),
      fill:'none', stroke:getConnColor(conn.type),
      'stroke-width':2, opacity:0.35,
      'data-from':conn.from,'data-to':conn.to,'data-type':conn.type,
      class:'conn-path'
    });
    if(conn.type==='betrayal') path.setAttribute('stroke-dasharray','8 6');
    if(conn.type==='parallel') path.setAttribute('stroke-dasharray','2 6');
    CL.appendChild(path);
  });

  CHARS.forEach(node=>{
    const col=getColor(node.group);
    const g=el('g',{'data-id':node.id,class:'node-group'});

    // Outer ring
    g.appendChild(el('polygon',{
      points:makeHex(node.x,node.y,node.r+8),
      fill:'none',stroke:col,'stroke-width':1,opacity:0.3
    }));
    // Main hex
    const hex=el('polygon',{
      points:makeHex(node.x,node.y,node.r),
      fill:'rgba(5,13,20,0.9)',stroke:col,'stroke-width':2.5,
      class:'node-hex'
    });
    if(node.group==='cosmic') hex.setAttribute('filter','url(#glow)');
    g.appendChild(hex);

    // Label
    const fs = node.r>18?'11px':'10px';
    const lblBg=el('text',{x:node.x,y:node.y+4,'text-anchor':'middle','font-family':'Chakra Petch','font-size':fs,'font-weight':700,'pointer-events':'none',fill:'none',stroke:'#050d14','stroke-width':4,'stroke-linejoin':'round'});
    lblBg.textContent=node.label; g.appendChild(lblBg);
    const lbl=el('text',{x:node.x,y:node.y+4,'text-anchor':'middle','font-family':'Chakra Petch','font-size':fs,'font-weight':700,'pointer-events':'none',fill:col});
    lbl.textContent=node.label; g.appendChild(lbl);

    // Sub-label
    if(node.sub){
      const subBg=el('text',{x:node.x,y:node.y+node.r+14,'text-anchor':'middle','font-family':'Chakra Petch','font-size':'9px','font-weight':600,'pointer-events':'none',fill:'none',stroke:'#050d14','stroke-width':3,'stroke-linejoin':'round'});
      subBg.textContent=node.sub; g.appendChild(subBg);
      const subLbl=el('text',{x:node.x,y:node.y+node.r+14,'text-anchor':'middle','font-family':'Chakra Petch','font-size':'9px','font-weight':600,'pointer-events':'none',fill:'#8AA4C4'});
      subLbl.textContent=node.sub; g.appendChild(subLbl);
    }

    // Large transparent hit area
    g.appendChild(el('circle',{cx:node.x,cy:node.y,r:Math.max(node.r+20,42),fill:'transparent',stroke:'none'}));

    g.addEventListener('click', e=>{
      if(hasDragged) return;
      e.stopPropagation();
      selectedId=node.id;
      hoverChar(node.id);
      selectChar(node);
    });
    g.addEventListener('pointerenter', ()=>{ if(!selectedId) hoverChar(node.id); });
    g.addEventListener('pointerleave', ()=>{ if(!selectedId) resetHighlight(); });

    NL.appendChild(g);
  });
}

function hoverChar(id){
  const connected=new Set([id]);
  document.querySelectorAll('.conn-path').forEach(p=>{
    const from=p.getAttribute('data-from'), to=p.getAttribute('data-to');
    if(from===id||to===id){ p.setAttribute('opacity',1); p.setAttribute('stroke-width',3.5); connected.add(from); connected.add(to); }
    else { p.setAttribute('opacity',0.05); }
  });
  document.querySelectorAll('.node-group').forEach(ng=>{
    ng.style.opacity = connected.has(ng.getAttribute('data-id'))?'1':'0.15';
  });
}

function resetHighlight(){
  document.querySelectorAll('.conn-path').forEach(p=>{ p.setAttribute('opacity',0.35); p.setAttribute('stroke-width',2); });
  document.querySelectorAll('.node-group').forEach(ng=>ng.style.opacity='1');
  document.querySelectorAll('.theme-node-g,.theme-char-node').forEach(n=>n.style.opacity='1');
  document.querySelectorAll('.theme-conn').forEach(p=>p.setAttribute('opacity',0.25));
  document.querySelectorAll('.thread-row').forEach(r=>r.style.opacity='1');
}

function selectChar(node){
  const charMap={}; CHARS.forEach(c=>charMap[c.id]=c);
  const myConns=CONNECTIONS.filter(c=>c.from===node.id||c.to===node.id);
  const themeItems=(node.themes||[]).map(t=>`<div class="conn-item"><div class="conn-dot" style="background:${C.hero}"></div><span style="color:#FFF">${t}</span></div>`).join('');
  const threadItems=(node.threads||[]).map(t=>`<div class="conn-item"><div class="conn-dot" style="background:${C.ally}"></div><span style="color:#FFF">${t}</span></div>`).join('');
  const connItems=myConns.map(c=>{
    const other=charMap[c.from===node.id?c.to:c.from];
    const dir=c.from===node.id?'►':'◄';
    return `<div class="conn-item">
      <div class="conn-dot" style="background:${getConnColor(c.type)}"></div>
      <div><strong style="color:${other?getColor(other.group):C.hero}">${dir} ${other?other.label:'?'}</strong><br>
      <span style="color:#888;font-size:0.75rem">[${c.type.toUpperCase()}]</span> <span style="color:#CCC">${c.label}</span></div>
    </div>`;
  }).join('');
  openPanel(`
    <h2>${node.label}</h2>
    <div class="role">${node.role}</div>
    <div class="desc">${node.desc}</div>
    ${themeItems?`<div class="section-head">THEMATIC RESONANCE</div>${themeItems}`:''}
    ${threadItems?`<div class="section-head">NARRATIVE THREADS</div>${threadItems}`:''}
    ${connItems?`<div class="section-head">DATALINKS</div>${connItems}`:''}
  `);
}

// ── Themes view ──
function renderThemes(){
  document.getElementById('filterBar').style.display='none';
  const CL=document.getElementById('connections-layer');
  const NL=document.getElementById('nodes-layer');
  CL.innerHTML=''; NL.innerHTML='';

  CHARS.forEach(node=>{
    const col=getColor(node.group);
    const g=el('g',{'data-id':node.id,class:'theme-char-node',style:'cursor:pointer'});
    g.appendChild(el('polygon',{points:makeHex(node.x,node.y,14),fill:'#0a1726',stroke:col,'stroke-width':1.5,opacity:0.8}));
    const nLabel=node.label.split(' ')[0];
    const bg=el('text',{x:node.x,y:node.y+3,'text-anchor':'middle','font-family':'Chakra Petch','font-size':'9px','font-weight':700,'pointer-events':'none',fill:'none',stroke:'#050d14','stroke-width':3});
    bg.textContent=nLabel; g.appendChild(bg);
    const lb=el('text',{x:node.x,y:node.y+3,'text-anchor':'middle','font-family':'Chakra Petch','font-size':'9px','font-weight':700,'pointer-events':'none',fill:col});
    lb.textContent=nLabel; g.appendChild(lb);
    g.appendChild(el('circle',{cx:node.x,cy:node.y,r:25,fill:'transparent'}));
    g.addEventListener('click', e=>{
      if(hasDragged) return;
      e.stopPropagation();
      const myThemes=THEMES.filter(th=>th.chars.includes(node.id));
      document.querySelectorAll('.theme-node-g').forEach(tg=>tg.style.opacity=myThemes.some(th=>th.id===tg.getAttribute('data-id'))?'1':'0.15');
      document.querySelectorAll('.theme-char-node').forEach(ng=>ng.style.opacity=(ng.getAttribute('data-id')===node.id||myThemes.some(th=>th.chars.includes(ng.getAttribute('data-id'))))?'1':'0.15');
      document.querySelectorAll('.theme-conn').forEach(p=>p.setAttribute('opacity',p.getAttribute('data-chars').includes(node.id)?'0.8':'0.05'));
      openPanel(`<h2>${node.label}</h2><div class="role">${node.role}</div><div class="section-head">THEMATIC NODES</div>${myThemes.map(th=>`<div class="conn-item"><div class="conn-dot" style="background:${C.hero}"></div><span style="color:#FFF">${th.label.replace('\n',' ')}</span></div>`).join('')}`);
    });
    NL.appendChild(g);
  });

  THEMES.forEach(theme=>{
    theme.chars.forEach(charId=>{
      const char=CHARS.find(c=>c.id===charId);
      if(!char) return;
      CL.appendChild(el('path',{d:makeCurve(theme.x,theme.y,char.x,char.y),fill:'none',stroke:C.hero,'stroke-width':1.5,opacity:0.25,'stroke-dasharray':'4 4',class:'theme-conn','data-theme':theme.id,'data-chars':theme.chars.join(',')}));
    });
    const lines=theme.label.split('\n');
    const w=155, h=lines.length*16+18;
    const g=el('g',{'data-id':theme.id,class:'theme-node-g',style:'cursor:pointer'});
    g.appendChild(el('rect',{x:theme.x-w/2,y:theme.y-h/2,width:w,height:h,rx:4,fill:'rgba(5,13,20,0.95)',stroke:C.hero,'stroke-width':2}));
    lines.forEach((line,li)=>{
      const t=el('text',{x:theme.x,y:theme.y-(lines.length-1)*8+li*16+4,'text-anchor':'middle',fill:'#FFF','font-family':'Chakra Petch','font-size':'12px','font-weight':700,'letter-spacing':'0.1em','pointer-events':'none'});
      t.textContent=line; g.appendChild(t);
    });
    g.appendChild(el('rect',{x:theme.x-w/2,y:theme.y-h/2,width:w,height:h,fill:'transparent'}));
    g.addEventListener('click', e=>{
      if(hasDragged) return;
      e.stopPropagation();
      document.querySelectorAll('.theme-node-g').forEach(tg=>tg.style.opacity=tg.getAttribute('data-id')===theme.id?'1':'0.2');
      document.querySelectorAll('.theme-char-node').forEach(ng=>ng.style.opacity=theme.chars.includes(ng.getAttribute('data-id'))?'1':'0.1');
      document.querySelectorAll('.theme-conn').forEach(p=>p.setAttribute('opacity',p.getAttribute('data-theme')===theme.id?'1':'0.05'));
      openPanel(`<h2>${theme.label.replace('\n',' ')}</h2><div class="role">THEME SEQUENCE</div><div class="desc">${theme.desc}</div><div class="section-head">LINKED ENTITIES</div>${theme.chars.map(cid=>{const ch=CHARS.find(c=>c.id===cid);return ch?`<div class="conn-item"><div class="conn-dot" style="background:${getColor(ch.group)}"></div><strong style="color:${getColor(ch.group)}">${ch.label}</strong></div>`:''}).join('')}`);
    });
    NL.appendChild(g);
  });
}

// ── Narrative view ──
function renderNarrative(){
  document.getElementById('filterBar').style.display='none';
  const CL=document.getElementById('connections-layer');
  const NL=document.getElementById('nodes-layer');
  CL.innerHTML=''; NL.innerHTML='';
  const acts=[{label:'ACT 1',cx:300,width:500},{label:'ACT 2',cx:800,width:500},{label:'ACT 3',cx:1300,width:500},{label:'POST-GAME',cx:1675,width:250}];
  const marginTop=80, rowH=85;
  acts.forEach(act=>{
    const x=act.cx-act.width/2;
    CL.appendChild(el('rect',{x,y:marginTop-10,width:act.width,height:THREADS.length*rowH+30,fill:'rgba(0,240,255,0.02)',stroke:'rgba(0,240,255,0.1)','stroke-width':1}));
    const t=el('text',{x:act.cx,y:marginTop-20,'text-anchor':'middle',fill:C.hero,'font-family':'Chakra Petch','font-size':'16px','font-weight':700,'letter-spacing':'0.2em'});
    t.textContent=act.label; CL.appendChild(t);
  });
  THREADS.forEach((thread,i)=>{
    const y=marginTop+i*rowH+rowH/2;
    const firstAct=acts.findIndex((a,ai)=>thread.actPresence[ai]);
    const lastAct=thread.actPresence.map((a,ai)=>a?ai:-1).filter(x=>x>=0).pop();
    if(firstAct<0) return;
    const barX1=acts[firstAct].cx-acts[firstAct].width/2+20;
    const barX2=acts[lastAct].cx+acts[lastAct].width/2-20;
    const g=el('g',{'data-id':thread.id,class:'thread-row',style:'cursor:pointer'});
    g.appendChild(el('rect',{x:barX1,y:y-28,width:barX2-barX1,height:56,rx:8,fill:thread.color,opacity:0.15}));
    g.appendChild(el('rect',{x:barX1,y:y-28,width:barX2-barX1,height:56,rx:8,fill:'none',stroke:thread.color,'stroke-width':2,opacity:0.8}));
    const lbl=el('text',{x:barX1+30,y:y+5,fill:'#FFF','font-family':'Chakra Petch','font-size':'14px','font-weight':700,'letter-spacing':'0.2em','pointer-events':'none'});
    lbl.textContent=thread.label; g.appendChild(lbl);
    g.appendChild(el('rect',{x:barX1,y:y-30,width:barX2-barX1,height:60,fill:'transparent'}));
    g.addEventListener('click', e=>{
      if(hasDragged) return;
      e.stopPropagation();
      document.querySelectorAll('.thread-row').forEach(tr=>tr.style.opacity=tr.getAttribute('data-id')===thread.id?'1':'0.2');
      openPanel(`<h2>${thread.label}</h2><div class="role">${thread.actPresence.map((a,idx)=>a?['Act 1','Act 2','Act 3','Post-Game'][idx]:'').filter(Boolean).join(' · ')}</div><div class="desc">${thread.desc}</div><div class="section-head">INVOLVED SUBJECTS</div>${thread.chars.map(cid=>{const ch=CHARS.find(c=>c.id===cid);return ch?`<div class="conn-item"><div class="conn-dot" style="background:${getColor(ch.group)}"></div><strong style="color:${getColor(ch.group)}">${ch.label}</strong></div>`:''}).join('')}${thread.intersects&&thread.intersects.length?`<div class="section-head">INTERSECTS WITH</div>${thread.intersects.map(x=>`<div class="conn-item"><div class="conn-dot" style="background:${C.jade}"></div><span style="color:#CCC">${x}</span></div>`).join('')}`:''}`);
    });
    NL.appendChild(g);
  });
}

// Init
renderCharacters();
</script>
</body>
</html>
