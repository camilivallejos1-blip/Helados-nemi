# Helados-nemi
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Heladería Nemi</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:linear-gradient(#ffd6de,#fff);
}

.container{
  max-width:420px;
  margin:auto;
  padding:15px;
}

.card{
  background:#fff;
  border-radius:20px;
  padding:15px;
  box-shadow:0 5px 20px rgba(0,0,0,0.1);
}

h1{
  text-align:center;
  color:#ff6f91;
}

.select-card{
  border:2px solid #eee;
  padding:10px;
  border-radius:15px;
  margin:5px 0;
  cursor:pointer;
}

.select-card.active{
  border-color:#ff8fa3;
  background:#ffeef2;
}

.btn{
  width:100%;
  padding:14px;
  border:none;
  border-radius:15px;
  margin-top:10px;
  font-size:16px;
}

.whatsapp{
  background:#25D366;
  color:white;
}

.counter{
  display:flex;
  justify-content:center;
  gap:10px;
  align-items:center;
}

.counter button{
  padding:10px;
  border-radius:10px;
  border:none;
}

.sabores{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:5px;
}

.sabor{
  padding:8px;
  border-radius:10px;
  background:#ffeef2;
  cursor:pointer;
}

.sabor.active{
  background:#ff8fa3;
  color:white;
}

input{
  width:100%;
  padding:10px;
  margin-top:8px;
  border-radius:10px;
  border:none;
  background:#ffeef2;
}

.total{
  font-size:22px;
  text-align:center;
  margin-top:10px;
}
</style>

</head>

<body>

<div class="container">

<div class="card">
<h1>🍦 Heladería Nemi</h1>

<!-- POTES -->
<div id="potes">
  <div class="select-card" onclick="selectPote(5500,2,this)">1/4 kg - $5500</div>
  <div class="select-card" onclick="selectPote(8000,3,this)">1/2 kg - $8000</div>
  <div class="select-card" onclick="selectPote(14000,4,this)">1 kg - $14000</div>
</div>

<!-- CANTIDAD -->
<div class="counter">
  <button onclick="cambiarCantidad(-1)">-</button>
  <span id="cantidad">1</span>
  <button onclick="cambiarCantidad(1)">+</button>
</div>

<label><input type="checkbox" id="promo"> Promo 3x2 (solo 1/4)</label>

<!-- SABORES -->
<div class="sabores" id="sabores"></div>

<!-- DATOS -->
<input placeholder="Nombre" id="nombre">
<input placeholder="Teléfono" id="telefono">
<input placeholder="Dirección" id="direccion">

<div class="total" id="total">$0</div>

<button class="btn whatsapp" onclick="pedir()">Pedir por WhatsApp</button>

</div>

</div>

<script>
let precio=0;
let maxSabores=0;
let cantidad=1;
let seleccion=[];

const lista=[
"Banana split","Cadbury","Cereza a la crema","Chocotorta","Chocolate",
"Chocolate kinder","Chocolate marroc","Crema del cielo","Crema oreo",
"Dulce de leche bombón","Dulce de leche granizado","Frutilla a la crema",
"Frutilla especial","Frutos del bosque","Flan con caramelo","Granizado",
"Limón","Mascarpone con frutos","Menta granizada","Mantecol","Tramontana"
];

function cargarSabores(){
 let cont=document.getElementById("sabores");
 lista.forEach(s=>{
  let div=document.createElement("div");
  div.className="sabor";
  div.innerText=s;
  div.onclick=()=>toggleSabor(div,s);
  cont.appendChild(div);
 });
}

function toggleSabor(el,s){
 if(seleccion.includes(s)){
  seleccion=seleccion.filter(x=>x!=s);
  el.classList.remove("active");
 }else{
  if(seleccion.length>=maxSabores){
    alert("Solo podés elegir "+maxSabores+" sabores");
    return;
  }
  seleccion.push(s);
  el.classList.add("active");
 }
 calcular();
}

function selectPote(p,s,el){
 precio=p;
 maxSabores=s;
 document.querySelectorAll(".select-card").forEach(e=>e.classList.remove("active"));
 el.classList.add("active");
 seleccion=[];
 document.querySelectorAll(".sabor").forEach(e=>e.classList.remove("active"));
 calcular();
}

function cambiarCantidad(v){
 cantidad=Math.max(1,cantidad+v);
 document.getElementById("cantidad").innerText=cantidad;
 calcular();
}

function calcular(){
 let total=precio*cantidad;

 if(document.getElementById("promo").checked && precio==5500){
   let promo=Math.floor(cantidad/3);
   total-=promo*5500;
 }

 document.getElementById("total").innerText="$"+total;
}

function pedir(){
 if(seleccion.length!=maxSabores){
  alert("Elegí los sabores correctos");
  return;
 }

 let msg=`🍦 Pedido Heladería Nemi
Nombre: ${nombre.value}
Teléfono: ${telefono.value}
Dirección: ${direccion.value}

Pote: $${precio}
Cantidad: ${cantidad}
Sabores: ${seleccion.join(", ")}

Total: ${total.innerText}
🚚 Envío gratis`;

 window.open("https://wa.me/5491139344495?text="+encodeURIComponent(msg));
}

cargarSabores();
</script>

</body>
</html>
