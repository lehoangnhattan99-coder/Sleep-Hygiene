# Sleep-Hygiene
<!doctype html>
</div>


<script>
// Sample data
const lessons = [
{id:1,title:'Bài 1: Động học chất điểm (Vật lý 10)',topic:'ly',level:'10',desc:'Tốc độ, vận tốc, gia tốc; bài tập có hướng dẫn giải.'},
{id:2,title:'Bài 2: Hàm số bậc nhất (Toán 10)',topic:'toan',level:'10',desc:'Đồ thị, nghiệm, áp dụng phương trình đường thẳng.'},
{id:3,title:'Bài 3: Tính chất phản ứng hóa học (Hóa học 10)',topic:'hoa',level:'10',desc:'Phản ứng oxi hoá — khử, cân bằng phương trình.'},
{id:4,title:'Bài 4: Phân tích tác phẩm (Ngữ văn)',topic:'van',level:'10',desc:'Hướng dẫn phân tích đoạn trích, luận điểm và dẫn chứng.'}
];


const cardsEl = document.getElementById('cards');
const qEl = document.getElementById('q');
const topicFilter = document.getElementById('topicFilter');
const modalRoot = document.getElementById('modalRoot');
const modalContent = document.getElementById('modalContent');


function render(list){
cardsEl.innerHTML='';
if(list.length===0){cardsEl.innerHTML='<div style="grid-column:1/-1" class="card">Không tìm thấy bài học. Thử từ khoá khác.</div>';return}
list.forEach(item=>{
const div = document.createElement('div'); div.className='card';
div.innerHTML = `<h3>${item.title}</h3><div class="meta">Lớp ${item.level} • ${topicName(item.topic)}</div><p>${item.desc}</p><div style="display:flex;gap:8px"><button class="open" data-id="${item.id}">Mở bài</button><button class="btn" onclick="bookmark(${item.id})">Lưu</button></div>`;
cardsEl.appendChild(div);
})
}


function topicName(k){
return {toan:'Toán',ly:'Vật lý',hoa:'Hóa',van:'Ngữ văn'}[k]||'Khác'
}


function openLesson(id){
const item = lessons.find(l=>l.id===id);
if(!item) return;
modalContent.innerHTML = `<h2>${item.title}</h2><p class="meta">Lớp ${item.level} • ${topicName(item.topic)}</p><p>Giải thích ngắn:</p><p>Lorem ipsum dolor sit amet, in hoc discere...</p><hr><h3>Bài tập</h3><ol><li>Bài tập 1 — có hướng dẫn</li><li>Bài tập 2 — lời giải tóm tắt</li></ol>`;
modalRoot.style.display='flex';
}


function bookmark(id){
alert('Đã lưu bài ' + id + ' vào mục yêu thích (tạm).');
}


document.addEventListener('click',e=>{
if(e.target.matches('.open')) openLesson(Number(e.target.dataset.id));
})


document.getElementById('closeModal').addEventListener('click',()=>{modalRoot.style.display='none'})
modalRoot.addEventListener('click',e=>{ if(e.target===modalRoot) modalRoot.style.display='none' })


function filterAndSearch(){
const q = qEl.value.trim().toLowerCase();
const tp = topicFilter.value;
let out = lessons.filter(l=>{
if(tp!=='all' && l.topic!==tp) return false;
if(!q) return true;
return (l.title + '\n' + l.desc).toLowerCase().includes(q);
})
render(out);
}


qEl.addEventListener('input',filterAndSearch);
topicFilter.addEventListener('change',filterAndSearch);
document.getElementById('clearBtn').addEventListener('click',()=>{qEl.value='';topicFilter.value='all';filterAndSearch()});


// add sample lesson
document.getElementById('addSample').addEventListener('click',()=>{
const nid = lessons.length + 1;
lessons.push({id:nid,title:`Bài mẫu ${nid}: Ôn tập nhanh`,topic:'toan',level:'10',desc:'Bài ôn tập nhanh nhiều ví dụ.'});
filterAndSearch();
})


// simple download (save current HTML)
document.getElementById('downloadBtn').addEventListener('click',()=>{
const blob = new Blob([document.documentElement.outerHTML],{type:'text/html'});
const url = URL.createObjectURL(blob);
const a = document.createElement('a'); a.href = url; a.download = 'hoc-cung-tan.html'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
})


// initial render
render(lessons);
</script>
</body>
</html>
