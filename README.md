# HoopCastAI-v1.1
from pathlib import Path
import zipfile, textwrap, shutil

root = Path("/mnt/data/HoopCast-AI-v1.0")
if root.exists():
    shutil.rmtree(root)

files = {
"package.json": r'''{
  "name": "hoopcast-ai",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "lucide-react": "^0.468.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.1",
    "vite": "^5.4.8"
  }
}''',
"vite.config.js": r'''import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()]
});''',
"index.html": r'''<!doctype html>
<html>
<head><meta charset="UTF-8"/><meta name="viewport" content="width=device-width,initial-scale=1.0"/><title>HoopCast AI</title></head>
<body><div id="root"></div><script type="module" src="/src/main.jsx"></script></body>
</html>''',
"src/main.jsx": r'''import React from "react";
import {createRoot} from "react-dom/client";
import "./styles.css";

const games=[
 {name:"Pembrook Hill", result:"W", points:12, assists:1, rebounds:10, steals:3, blocks:2, minutes:24},
 {name:"NLE vs Spurs", result:"L", points:8, assists:2, rebounds:7, steals:3, blocks:1, minutes:22},
 {name:"Weekend Total", result:"3-0", points:32, assists:5, rebounds:19, steals:8, blocks:3, minutes:62}
];

function App(){
 const season=games[games.length-1];
 return <div className="app">
  <header><h1>🏀 HoopCast AI v1.0</h1><p>Joey Hedge Recruiting Analytics Platform</p></header>
  <section className="card hero">
   <h2>Player Profile</h2>
   <h3>Joey Hedge #30</h3>
   <p>Incoming freshman basketball analytics dashboard</p>
  </section>
  <div className="grid">
   {["Points","Assists","Rebounds","Steals","Blocks","Minutes"].map((x,i)=>
    <div className="card stat" key={x}><b>{x}</b><span>{[season.points,season.assists,season.rebounds,season.steals,season.blocks,season.minutes][i]}</span></div>
   )}
  </div>
  <section className="card">
   <h2>Game Log</h2>
   {games.map(g=><div className="game" key={g.name}>
    <strong>{g.name}</strong> — {g.result}<br/>
    {g.points} PTS | {g.assists} AST | {g.rebounds} REB | {g.steals} STL | {g.blocks} BLK | {g.minutes} MIN
   </div>)}
  </section>
 </div>
}
createRoot(document.getElementById("root")).render(<App/>);''',
"src/styles.css": r'''body{margin:0;font-family:Arial,sans-serif;background:#0b1020;color:white}.app{max-width:1000px;margin:auto;padding:24px}.card{background:#151d35;border-radius:16px;padding:20px;margin:16px 0}.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:16px}.stat{display:flex;flex-direction:column;align-items:center}.stat span{font-size:32px;font-weight:bold;margin-top:10px}.game{padding:12px;border-bottom:1px solid #334}.hero{background:#1c2c52}'''
}
for p,c in files.items():
    path=root/p
    path.parent.mkdir(parents=True, exist_ok=True)
    path.write_text(textwrap.dedent(c))

(root/"README.md").write_text("""# HoopCast AI v1.0

React + Vite basketball analytics prototype.

Run:
npm install
npm run dev

Deploy:
Connect repository to Vercel.

Includes:
- Player dashboard
- Joey Hedge sample data
- Game log
- Analytics foundation
""")

zip_path = Path("/mnt/data/HoopCast-AI-v1.0.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for p in root.rglob("*"):
        z.write(p, p.relative_to(root.parent))

str(zip_path)

