<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cool Summer Palette</title>

<style>
*{box-sizing:border-box;}

body{
font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
margin:0;
background:#f5f2f1;
}

.container{
max-width:900px;
margin:auto;
padding:8px 16px 16px;
}

.header{
text-align:center;
margin-bottom:8px;
}

.logo{
display:block;
max-width:220px;
margin:0 auto 6px;
}

.header h1{
font-family:Georgia,serif;
font-weight:500;
margin:0;
}

.grid{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:14px;
}

.section-title{
grid-column:1/-1;
font-size:12px;
letter-spacing:.15em;
color:#68707c;
margin-top:14px;
}

.tile{
aspect-ratio:1;
border-radius:20px;
cursor:pointer;
}

.lightbox{
position:fixed;
inset:0;
display:none;
z-index:999;
}

.lightbox.show{
display:block;
}

.lightbox-color{
position:absolute;
inset:0;
}

.close-btn{
position:absolute;
top:32px;
right:32px;
width:36px;
height:36px;
border-radius:50%;
border:none;
background:rgba(255,255,255,.25);
color:white;
font-size:22px;
display:flex;
align-items:center;
justify-content:center;
backdrop-filter:blur(6px);
-webkit-backdrop-filter:blur(6px);
cursor:pointer;
}

.controls{
position:absolute;
top:32px;
right:84px;
display:flex;
gap:8px;
}

.btn{
width:36px;
height:36px;
border-radius:50%;
border:none;
background:rgba(255,255,255,.25);
color:white;
font-size:16px;
display:flex;
align-items:center;
justify-content:center;
backdrop-filter:blur(6px);
-webkit-backdrop-filter:blur(6px);
cursor:pointer;
}

.swipe{
position:absolute;
bottom:18px;
left:50%;
transform:translateX(-50%);
color:white;
letter-spacing:.15em;
font-size:12px;
}

@media (max-width:640px){
.logo{
max-width:180px;
}

.grid{
gap:12px;
}

.tile{
border-radius:18px;
}

.close-btn{
top:28px;
right:28px;
}

.controls{
top:28px;
right:76px;
}
}
</style>
</head>

<body>

<div class="container">

<div class="header">
<img class="logo" src="logo.png" alt="Shades of Hue logo">
<h1>Cool Summer</h1>
<div>Tap color • Swipe to browse</div>
</div>

<div id="grid" class="grid"></div>

</div>

<div id="lightbox" class="lightbox">
<div id="lightboxColor" class="lightbox-color"></div>

<button id="close" class="close-btn">×</button>

<div class="controls">
<button id="prev" class="btn">←</button>
<button id="next" class="btn">→</button>
</div>

<div class="swipe">SWIPE</div>
</div>

<script>

const groups = [

{
title:"COOL SUMMER NEUTRALS",
colors:[
"#F2F2F0",
"#E2DBD7",
"#D7D3D8",
"#CACBD1",
"#B8B8B5",
"#AAA5A7",
"#97A4B4",
"#8897A7",
"#7E8280",
"#5D5B6D"
]
},

{
title:"COOL SUMMER LIGHTS",
colors:[
"#D7DAE8",
"#C6CBE0",
"#A9B4E0",
"#E8B5D6",
"#F1C9DB",
"#E7E39A"
]
},

{
title:"PINKS + ROSES",
colors:[
"#F05F8E",
"#E33F6B",
"#E07A9B",
"#D05F8D",
"#C05079",
"#B04E7E",
"#C86DA0",
"#D97AAE"
]
},

{
title:"ORCHIDS + VIOLETS",
colors:[
"#9A64CC",
"#8A6FCD",
"#7B7FDB",
"#A58FDC",
"#8A54A3",
"#95469C"
]
},

{
title:"COOL BLUES",
colors:[
"#9FC5E9",
"#7FB0DB",
"#6799CA",
"#4F84B6",
"#477BB5",
"#395FA6",
"#2F69A8",
"#214B8A"
]
},

{
title:"DENIM + NAVY",
colors:[
"#7A88A7",
"#5E85B8",
"#465FA3",
"#3A4F8C",
"#2A247E"
]
},

{
title:"BLUE GREENS + TEALS",
colors:[
"#A9D4D8",
"#7CB7C8",
"#60B1C5",
"#4BA0B8",
"#4597A5",
"#3D8F92",
"#2C98A2",
"#1E7A84"
]
},

{
title:"GREENS",
colors:[
"#91D4BE",
"#7DC0AF",
"#59AF98",
"#4AA392",
"#3E9482",
"#33775A",
"#5DAD8F"
]
},

{
title:"COOL GRAYS + PORPOISE",
colors:[
"#D9DAE1",
"#B8C1D1",
"#9BABB8",
"#7E91A2",
"#667A84",
"#5D7480"
]
}

];

const grid=document.getElementById("grid");
const lightbox=document.getElementById("lightbox");
const lightboxColor=document.getElementById("lightboxColor");

let all=[];
let index=0;
let startX=0;

groups.forEach(group=>{

const title=document.createElement("div");
title.className="section-title";
title.textContent=group.title;
grid.appendChild(title);

group.colors.forEach(color=>{

all.push(color);

const tile=document.createElement("div");
tile.className="tile";
tile.style.background=color;

tile.onclick=()=>{
index=all.indexOf(color);
open();
};

grid.appendChild(tile);

});

});

function open(){
lightbox.classList.add("show");
render();
}

function render(){
lightboxColor.style.background=all[index];
}

function next(){
index=(index+1)%all.length;
render();
}

function prev(){
index=(index-1+all.length)%all.length;
render();
}

document.getElementById("close").onclick=()=>lightbox.classList.remove("show");
document.getElementById("next").onclick=next;
document.getElementById("prev").onclick=prev;

lightbox.addEventListener("touchstart",e=>{
startX=e.touches[0].clientX;
});

lightbox.addEventListener("touchend",e=>{
let diff=e.changedTouches[0].clientX-startX;
if(Math.abs(diff)<40)return;
diff<0?next():prev();
});

</script>

</body>
</html>
