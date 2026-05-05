<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>KING | متجر ملكي</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{--gold:#d4af37}
body{margin:0;font-family:Tahoma;background:#fff}
header{text-align:center;border-bottom:2px solid var(--gold);padding:15px}
header h1{color:var(--gold)}
nav button{background:var(--gold);color:#fff;border:none;padding:10px 20px;margin:5px;cursor:pointer}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:20px;padding:20px}
.product{border:1px solid var(--gold);cursor:pointer}
.product img{width:100%;height:240px;object-fit:cover}
.product h3{text-align:center;color:var(--gold)}
footer{text-align:center;padding:10px;border-top:1px solid var(--gold)}

.modal{display:none;position:fixed;inset:0;background:rgba(0,0,0,.6);justify-content:center;align-items:center}
.modal-content{background:#fff;padding:15px;width:95%;max-width:450px}
.slider img{width:100%;height:250px;object-fit:cover}
input,select{width:100%;padding:10px;margin:6px 0}
button{width:100%;padding:12px;background:var(--gold);border:none;color:#fff;font-size:16px}
.close{color:red;cursor:pointer;text-align:left}
</style>
</head>

<body>

<header>
<h1>KING</h1>
<nav>
<button onclick="showProducts('men')">رجالي</button>
<button onclick="showProducts('women')">نسائي</button>
</nav>
</header>

<div class="products" id="products"></div>

<footer>
👁️ عدد الزوار: <span id="visits">0</span>
</footer>

<!-- نافذة تفاصيل المنتج -->
<div class="modal" id="modal">
<div class="modal-content">
<div class="close" onclick="closeModal()">✖</div>

<div class="slider">
<img id="mainImg">
</div>

<h3 id="pName"></h3>
<p id="pDesc"></p>

<input id="name" placeholder="الاسم الكامل">
<input id="phone" placeholder="رقم الهاتف">
<input id="address" placeholder="العنوان">

<select id="size">
<option>اختر المقاس</option>
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<select id="color">
<option>اختر اللون</option>
<option>أسود</option><option>أبيض</option><option>ذهبي</option>
</select>

<button onclick="sendOrder()">تأكيد الطلب (الدفع عند الاستلام)</button>
</div>
</div>

<script>
const data={
men:[
{
name:"قميص رجالي فاخر",
desc:"قميص قطني بجودة عالية، مناسب لكل المناسبات.",
imgs:[
"https://images.unsplash.com/photo-1521334884684-d80222895322",
"https://images.unsplash.com/photo-1520975916090-3105956dac38"
]
}
],
women:[
{
name:"فستان نسائي أنيق",
desc:"فستان عصري بتصميم أنيق وخامة ممتازة.",
imgs:[
"https://images.unsplash.com/photo-1520975698519-59f3c61ecf74",
"https://images.unsplash.com/photo-1517841905240-472988babdf9"
]
}
]
};

let currentProduct;

function showProducts(type){
const box=document.getElementById("products");
box.innerHTML="";
data[type].forEach((p,i)=>{
box.innerHTML+=`
<div class="product" onclick="openModal('${type}',${i})">
<img src="${p.imgs[0]}">
<h3>${p.name}</h3>
</div>`;
});
}

function openModal(type,index){
currentProduct=data[type][index];
document.getElementById("pName").innerText=currentProduct.name;
document.getElementById("pDesc").innerText=currentProduct.desc;
document.getElementById("mainImg").src=currentProduct.imgs[0];
document.getElementById("modal").style.display="flex";
}

function closeModal(){document.getElementById("modal").style.display="none"}

function sendOrder(){
const msg=`طلب جديد - KING
الاسم: ${name.value}
الهاتف: ${phone.value}
المنتج: ${currentProduct.name}
المقاس: ${size.value}
اللون: ${color.value}
العنوان: ${address.value}
الدفع: عند الاستلام`;

window.open("https://wa.me/212677669487?text="+encodeURIComponent(msg));
}

/* عداد الزوار */
let v=localStorage.getItem("visits")||0;
v++;localStorage.setItem("visits",v);
visits.innerText=v;

showProducts("men");
</script>

</body>
</html>
