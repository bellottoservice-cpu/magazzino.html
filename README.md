<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Magazzino Cella</title>

<style>
body{
  font-family: Arial;
  padding:15px;
}

table{
  width:100%;
  border-collapse: collapse;
  margin-top:10px;
}

th, td{
  border:1px solid #ccc;
  padding:6px;
  text-align:center;
}

button{
  padding:5px 8px;
}

input{
  width:70px;
}
</style>
</head>

<body>

<h2>MAGAZZINO CELLA</h2>

<div id="tabella"></div>

<hr>

<button onclick="pdfGiacenze()">📄 PDF Giacenze</button>
<button onclick="pdfOrdine('martedi')">📄 PDF Ordine Martedì</button>
<button onclick="pdfOrdine('venerdi')">📄 PDF Ordine Venerdì</button>

<script>

let prodotti = [

{nome:"MOZZARELLA", kg:0, ordineMartedi:0, ordineVenerdi:0},
{nome:"POMODORO", kg:0, ordineMartedi:0, ordineVenerdi:0},
{nome:"FARINA", kg:0, ordineMartedi:0, ordineVenerdi:0},
{nome:"RADICCHIO", kg:0, ordineMartedi:0, ordineVenerdi:0},
{nome:"ZUCCHINE", kg:0, ordineMartedi:0, ordineVenerdi:0}

];

function generaTabella(){

  let html="<table><tr><th>Prodotto</th><th>Kg Attuali</th><th>+ Kg</th><th>- Kg</th><th>Ordine Martedì</th><th>Ordine Venerdì</th></tr>";

  prodotti.forEach((p,i)=>{

    html+="<tr>";
    html+="<td>"+p.nome+"</td>";
    html+="<td>"+p.kg.toFixed(2)+"</td>";
    html+="<td><button onclick='modificaKg("+i+",1)'>➕</button></td>";
    html+="<td><button onclick='modificaKg("+i+",-1)'>➖</button></td>";
    html+="<td><input type='number' value='"+p.ordineMartedi+"' onchange='setOrdine("+i+",\"martedi\",this.value)'></td>";
    html+="<td><input type='number' value='"+p.ordineVenerdi+"' onchange='setOrdine("+i+",\"venerdi\",this.value)'></td>";
    html+="</tr>";
  });

  html+="</table>";

  document.getElementById("tabella").innerHTML=html;
}

function modificaKg(index,tipo){

  let valore = prompt("Quanti kg?");
  if(!valore) return;

  valore=parseFloat(valore);

  if(tipo===1){
    prodotti[index].kg += valore;
  } else {
    prodotti[index].kg -= valore;
    if(prodotti[index].kg<0) prodotti[index].kg=0;
  }

  generaTabella();
}

function setOrdine(index,giorno,valore){
  valore=parseFloat(valore)||0;

  if(giorno==="martedi"){
    prodotti[index].ordineMartedi=valore;
  } else {
    prodotti[index].ordineVenerdi=valore;
  }
}

function pdfGiacenze(){

  let tab="<h1>Giacenze Cella</h1><table border='1' cellpadding='5'><tr><th>Prodotto</th><th>Kg</th></tr>";

  prodotti.forEach(p=>{
    tab+="<tr><td>"+p.nome+"</td><td>"+p.kg.toFixed(2)+"</td></tr>";
  });

  tab+="</table>";

  stampa(tab);
}

function pdfOrdine(giorno){

  let titolo = giorno==="martedi" ? "Ordine Martedì" : "Ordine Venerdì";

  let tab="<h1>"+titolo+"</h1><table border='1' cellpadding='5'><tr><th>Prodotto</th><th>Kg</th></tr>";

  prodotti.forEach(p=>{
    let q = giorno==="martedi" ? p.ordineMartedi : p.ordineVenerdi;
    if(q>0){
      tab+="<tr><td>"+p.nome+"</td><td>"+q+"</td></tr>";
    }
  });

  tab+="</table>";

  stampa(tab);
}

function stampa(contenuto){

  let originale=document.body.innerHTML;

  document.body.innerHTML=contenuto;
  window.print();
  document.body.innerHTML=originale;

  generaTabella();
}

generaTabella();

</script>

</body>
</html>
