<!DOCTYPE html>  <html lang="id">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>Fun Facts Manager</title>  <style>  
:root{  
    --bg:#0f172a;  
    --card:#1e293b;  
    --text:#fff;  
    --accent:#3b82f6;  
}  
  
[data-theme="light"]{  
    --bg:#f1f5f9;  
    --card:#ffffff;  
    --text:#111827;  
    --accent:#2563eb;  
}  
  
*{  
    box-sizing:border-box;  
    font-family:Segoe UI,sans-serif;  
}  
  
body{  
    margin:0;  
    background:var(--bg);  
    color:var(--text);  
    padding:20px;  
    transition:.3s;  
}  
  
.container{  
    max-width:1200px;  
    margin:auto;  
}  
  
.card{  
    background:var(--card);  
    padding:20px;  
    border-radius:16px;  
    margin-bottom:20px;  
}  
  
h1,h2,h3{  
    margin-top:0;  
}  
  
button{  
    border:none;  
    border-radius:8px;  
    padding:10px 15px;  
    cursor:pointer;  
    margin:2px;  
    background:var(--accent);  
    color:white;  
}  
  
button:hover{  
    opacity:.9;  
}  
  
input,select,textarea{  
    width:100%;  
    padding:10px;  
    margin-bottom:10px;  
    border-radius:8px;  
    border:1px solid #ccc;  
}  
  
textarea{  
    min-height:100px;  
}  
  
.fact-box{  
    font-size:1.2rem;  
    padding:20px;  
    text-align:center;  
    background:rgba(255,255,255,.08);  
    border-radius:12px;  
    margin:15px 0;  
}  
  
.badge{  
    display:inline-block;  
    padding:5px 12px;  
    border-radius:999px;  
    background:#6366f1;  
    color:white;  
}  
  
.history-item{  
    padding:12px;  
    border-radius:10px;  
    margin-bottom:10px;  
}  
  
.pending{border-left:5px solid orange;}  
.approved{border-left:5px solid green;}  
.rejected{border-left:5px solid red;}  
.cancelled{border-left:5px solid gray;}  
  
.version-box{  
    margin-top:10px;  
    padding:10px;  
    border-radius:8px;  
    background:rgba(255,255,255,.05);  
}  
  
.status{  
    font-weight:bold;  
}  
  
.small{  
    font-size:.8rem;  
    opacity:.7;  
}  
  
.action-group{  
    margin-top:10px;  
}  
  
@media(max-width:768px){  
    button{  
        width:100%;  
    }  
}  
</style>  </head>  
<body data-theme="dark">  <div class="container">  <div class="card">  
<h1>🤯 Fun Facts Manager</h1>  <button onclick="toggleTheme()" id="themeBtn">  
☀️ Mode Terang  
</button>  
</div>  <div class="card">  
<h2>Random Fun Fact</h2>  <select id="factCategory">  
<option value="all">Semua Kategori</option>  
<option value="sains">Sains</option>  
<option value="hewan">Hewan</option>  
<option value="sejarah">Sejarah</option>  
<option value="angkasa">Luar Angkasa</option>  
</select>  <div id="categoryBadge" class="badge">  
Semua Kategori  
</div>  <div id="randomFact" class="fact-box"></div>  <button onclick="showRandomFact()">  
🎲 Fakta Baru  
</button>  
</div>  <div class="card">  
<h2>Tambah Fakta Baru</h2>  <select id="newCategory">  
<option value="sains">Sains</option>  
<option value="hewan">Hewan</option>  
<option value="sejarah">Sejarah</option>  
<option value="angkasa">Luar Angkasa</option>  
</select>  <textarea  
id="newFact"  
placeholder="Masukkan fakta menarik..."  
></textarea>  <button onclick="submitFact()">  
Validasi & Submit  
</button>  <div id="validationResult"></div>  
</div>  <div class="card">  
<h2>Riwayat Pengajuan</h2>  
<div id="history"></div>  
</div>  </div>  <script>  
  
const facts = {  
  
sains:[  
"Petir lebih panas daripada permukaan matahari.",  
"Tubuh manusia terdiri dari sekitar 37 triliun sel."  
],  
  
hewan:[  
"Gurita memiliki tiga jantung.",  
"Koala bisa tidur hingga 20 jam sehari."  
],  
  
sejarah:[  
"Piramida Mesir lebih dekat ke zaman modern daripada ke zaman dinosaurus."  
],  
  
angkasa:[  
"Satu hari di Venus lebih lama daripada satu tahun di Venus."  
]  
  
};  
  
let submissions = [];  
  
let theme =  
localStorage.getItem("theme") ||  
"dark";  
  
document.body.setAttribute(  
"data-theme",  
theme  
);  
  
updateThemeButton();  
  
document  
.getElementById("factCategory")  
.addEventListener("change",()=>{  
  
const selected =  
document.getElementById(  
"factCategory"  
);  
  
document.getElementById(  
"categoryBadge"  
).innerText =  
selected.options[  
selected.selectedIndex  
].text;  
  
});  
  
function toggleTheme(){  
  
theme =  
theme==="dark"  
?"light"  
:"dark";  
  
document.body.setAttribute(  
"data-theme",  
theme  
);  
  
localStorage.setItem(  
"theme",  
theme  
);  
  
updateThemeButton();  
}  
  
function updateThemeButton(){  
  
document.getElementById(  
"themeBtn"  
).textContent =  
  
theme==="dark"  
? "☀️ Mode Terang"  
: "🌙 Mode Gelap";  
}  
  
function showRandomFact(){  
  
const category =  
document.getElementById(  
"factCategory"  
).value;  
  
let available=[];  
  
if(category==="all"){  
available =  
Object.values(facts).flat();  
}  
else{  
available =  
facts[category];  
}  
  
const random =  
available[  
Math.floor(  
Math.random() *  
available.length  
)  
];  
  
document.getElementById(  
"randomFact"  
).innerText =  
random;  
}  
  
function validateFact(text){  
  
if(!text){  
return "Fakta tidak boleh kosong.";  
}  
  
if(text.length < 20){  
return "Minimal 20 karakter.";  
}  
  
if(!text.endsWith(".")){  
return "Fakta harus diakhiri titik.";  
}  
  
const allFacts =  
Object.values(facts)  
.flat()  
.map(x=>x.toLowerCase());  
  
if(  
allFacts.includes(  
text.toLowerCase()  
)  
){  
return "Fakta sudah tersedia.";  
}  
  
return null;  
}  
  
function submitFact(){  
  
const fact =  
document  
.getElementById(  
"newFact"  
)  
.value  
.trim();  
  
const category =  
document  
.getElementById(  
"newCategory"  
)  
.value;  
  
const error =  
validateFact(fact);  
  
if(error){  
  
document  
.getElementById(  
"validationResult"  
)  
.innerHTML =  
"❌ " + error;  
  
addHistory(  
fact,  
category,  
"rejected",  
error  
);  
  
return;  
}  
  
const id =  
Date.now();  
  
const timestamp =  
new Date()  
.toLocaleString(  
"id-ID"  
);  
  
submissions.push({  
  
id,  
  
fact,  
  
category,  
  
status:"pending",  
  
createdAt:timestamp,  
  
updatedAt:timestamp,  
  
reason:"",  
  
versions:[  
{  
version:1,  
action:"CREATE",  
fact,  
category,  
timestamp  
}  
]  
  
});  
  
document.getElementById(  
"validationResult"  
).innerHTML =  
"✅ Validasi berhasil, menunggu approval.";  
  
document.getElementById(  
"newFact"  
).value="";  
  
renderHistory();  
}  
  
function addHistory(  
fact,  
category,  
status,  
reason  
){  
  
submissions.push({  
  
id:Date.now(),  
  
fact,  
  
category,  
  
status,  
  
reason,  
  
createdAt:  
new Date()  
.toLocaleString(  
"id-ID"  
),  
  
versions:[]  
  
});  
  
renderHistory();  
}  
  
function approveSubmission(id){  
  
const item =  
submissions.find(  
x=>x.id===id  
);  
  
if(!item) return;  
  
item.status="approved";  
  
item.updatedAt=  
new Date()  
.toLocaleString(  
"id-ID"  
);  
  
facts[item.category]  
.push(item.fact);  
  
item.versions.push({  
version:  
item.versions.length+1,  
action:"APPROVE",  
fact:item.fact,  
category:item.category,  
timestamp:item.updatedAt  
});  
  
renderHistory();  
}  
  
function cancelSubmission(id){  
  
const item =  
submissions.find(  
x=>x.id===id  
);  
  
if(!item) return;  
  
if(item.status!=="pending")  
return;  
  
item.status="cancelled";  
  
item.reason=  
"Dibatalkan oleh pengguna.";  
  
item.updatedAt=  
new Date()  
.toLocaleString(  
"id-ID"  
);  
  
renderHistory();  
}  
  
function editSubmission(id){  
  
const item =  
submissions.find(  
x=>x.id===id  
);  
  
if(!item) return;  
  
const newText =  
prompt(  
"Edit fakta:",  
item.fact  
);  
  
if(!newText) return;  
  
const validation =  
validateFact(  
newText  
);  
  
if(validation){  
  
alert(validation);  
return;  
}  
  
item.fact =  
newText;  
  
item.updatedAt =  
new Date()  
.toLocaleString(  
"id-ID"  
);  
  
item.versions.push({  
version:  
item.versions.length+1,  
action:"EDIT",  
fact:newText,  
category:item.category,  
timestamp:item.updatedAt  
});  
  
renderHistory();  
}  
  
function toggleVersions(id){  
  
const box =  
document.getElementById(  
"versions-"+id  
);  
  
box.style.display =  
box.style.display==="none"  
? "block"  
: "none";  
}  
  
function renderHistory(){  
  
const container =  
document.getElementById(  
"history"  
);  
  
if(!submissions.length){  
  
container.innerHTML =  
"Belum ada pengajuan.";  
  
return;  
}  
  
container.innerHTML =  
submissions  
.slice()  
.reverse()  
.map(item=>{  
  
return `  
<div class="history-item ${item.status}">  
  
<div class="status">  
Status:  
${item.status.toUpperCase()}  
</div>  
  
<div>  
<strong>Kategori:</strong>  
${item.category}  
</div>  
  
<div>  
<strong>Fakta:</strong>  
${item.fact}  
</div>  
  
<div class="small">  
Dibuat:  
${item.createdAt}  
</div>  
  
${item.reason  
? `<div>Alasan: ${item.reason}</div>`  
: ''}  
  
<div class="action-group">  
  
${item.status==='pending'  
? `  
<button onclick="approveSubmission(${item.id})">  
Approve  
</button>  
  
<button onclick="editSubmission(${item.id})">  
Edit  
</button>  
  
<button onclick="cancelSubmission(${item.id})">  
Cancel  
</button>  
`  
: ''}  
  
<button onclick="toggleVersions(${item.id})">  
Riwayat Versi  
</button>  
  
</div>  
  
<div  
id="versions-${item.id}"  
style="display:none"  
>  
  
${item.versions.map(v=>`  
  
<div class="version-box">  
  
<strong>  
V${v.version}  
</strong>  
  
(${v.action})  
  
<br>  
  
${v.timestamp}  
  
<br>  
  
${v.fact}  
  
</div>  
  
`).join('')}  
  
</div>  
  
</div>  
`;  
  
})  
.join("");  
}  
  
showRandomFact();  
  
</script>  </body>  
</html>  
