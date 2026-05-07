<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>KING STORE</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root{--gold:#d4af37}
body{margin:0;font-family:Tahoma;background:#fff}

header{text-align:center;border-bottom:2px solid var(--gold);padding:15px}
header h1{color:var(--gold)}

.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:15px;padding:15px}

.product{border:1px solid var(--gold);padding:5px;cursor:pointer}
.product img{width:100%;height:200px;object-fit:cover}
.product h3{text-align:center;color:var(--gold)}

.modal{
display:none;
position:fixed;
inset:0;
background:rgba(0,0,0,.6);
justify-content:center;
align-items:center;
}

.modal-content{
background:#fff;
padding:15px;
width:95%;
max-width:420px;
}

input,select{
width:100%;
padding:10px;
margin:5px 0;
box-sizing:border-box;
}

button{
width:100%;
padding:12px;
background:var(--gold);
border:none;
color:#fff;
cursor:pointer;
}

.close{
background:red;
margin-top:5px;
}
</style>
</head>

<body>

<header>
<h1>KING STORE</h1>
</header>

<div class="products" id="products"></div>

<!-- MODAL -->
<div class="modal" id="modal">

<div class="modal-content">

<h3 id="pName"></h3>
<p id="pDesc"></p>

<input id="name" placeholder="الاسم الكامل">
<input id="phone" placeholder="رقم الهاتف">
<input id="address" placeholder="العنوان">

<select id="size">
<option value="">اختر المقاس</option>
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<select id="color">
<option value="">اختر اللون</option>
<option>أسود</option><option>أبيض</option><option>ذهبي</option>
</select>

<button onclick="sendOrder()">تأكيد الطلب</button>
<button class="close" onclick="closeModal()">إغلاق</button>

</div>
</div>

<script>

/* 🔥 Google Apps Script */
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzZinwF4vGZjI35_AVAOXGfjhje0PvVGnfU2YftRAi3SNhx9gC3Vgg6f7RZZYPElphf/exec";

/* WhatsApp Admin */
const adminPhone = "212677669487";

/* Products */
const data = {
men:[{
name:"قميص رجالي فاخر",
desc:"قميص قطني بجودة عالية",
imgs:["https://images.unsplash.com/photo-1521334884684-d80222895322"]
}]
};

let currentProduct = null;

/* SHOW PRODUCTS */
function showProducts(){
let box = document.getElementById("products");
box.innerHTML="";

data.men.forEach((p,i)=>{
box.innerHTML += `
<div class="product" onclick="openModal(${i})">
<img src="${p.imgs[0]}">
<h3>${p.name}</h3>
</div>`;
});
}

/* OPEN MODAL */
function openModal(i){
currentProduct = data.men[i];

document.getElementById("pName").innerText = currentProduct.name;
document.getElementById("pDesc").innerText = currentProduct.desc;

document.getElementById("modal").style.display="flex";
}

/* CLOSE */
function closeModal(){
document.getElementById("modal").style.display="none";
}

/* SEND ORDER */
function sendOrder(){

/* validation */
if(!name.value || !phone.value || !address.value || !size.value || !color.value){
alert("❗ عمر جميع الخانات");
return;
}

/* order text */
const msg = `
🛒 طلب جديد - KING STORE

👤 الاسم: ${name.value}
📞 الهاتف: ${phone.value}
📦 المنتج: ${currentProduct.name}
📏 المقاس: ${size.value}
🎨 اللون: ${color.value}
🏠 العنوان: ${address.value}
`;

/* send to Google Sheets */
fetch(SCRIPT_URL,{
method:"POST",
body:JSON.stringify({
name:name.value,
phone:phone.value,
product:currentProduct.name,
size:size.value,
color:color.value,
address:address.value
})
});

/* WhatsApp alert */
const url = "https://wa.me/" + adminPhone + "?text=" + encodeURIComponent(msg);
window.open(url,"_blank");

/* success */
alert("تم إرسال الطلب بنجاح ✅");
closeModal();

}

/* init */
showProducts();

</script>

</body>
</html>
