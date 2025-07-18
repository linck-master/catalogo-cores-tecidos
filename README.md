<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Catálogo Linck Master</title>
<style>
body {background:#000;color:white;font-family:sans-serif;text-align:center;}
nav button {margin:5px;padding:10px;border:none;border-radius:15px;background:#444;color:white;cursor:pointer;}
.item {display:inline-block;margin:10px;background:#111;padding:10px;border-radius:20px;position:relative;}
.item.dourado {border:2px solid gold;}
.color-box {width:150px;height:150px;background-size:cover;background-position:center;border-radius:20px;}
img.thumb {width:150px;height:150px;object-fit:cover;border-radius:20px;}
.galeria img {width:80px;height:80px;margin:5px;border-radius:20px;border:2px solid #fff;cursor:pointer;}
.selecionada {border:3px solid gold;}
.copy-options {display:none;}
input,select {padding:5px;border-radius:10px;margin:5px;}
.section-title {background:#111;padding:10px;border-radius:10px;margin-bottom:10px;}
.preview-area {background:#111;padding:10px;margin-top:20px;border-radius:10px;}
.preview-area img {width:500px;height:500px;object-fit:cover;border-radius:20px;margin:10px;}
</style>
</head>
<body>

<h1>🔥 Catálogo Linck Master 🔥</h1>
<nav>
<button onclick="show('cores')">🎨 Cores</button>
<button onclick="show('tecidos')">🧵 Tecidos</button>
<button onclick="show('painel')">🔧 Painel Admin</button>
<button onclick="show('excluir')">🗑️ Excluir Itens</button>
</nav>

<div id="cores" class="content"></div>
<div id="tecidos" class="content" style="display:none"></div>

<div id="painel" class="content" style="display:none">
<h2>➕ Adicionar Cor ou Tecido</h2>
<input id="nomeItem" placeholder="Nome"><br>
<input id="hex" placeholder="Cor HEX (opcional)"><br>
<input id="obsItem" placeholder="Observações (opcional)"><br>

<label>📅 Estação do ano:</label>
<select id="estacao">
<option>Nenhuma delas</option><option>Verão</option><option>Inverno</option><option>Outono</option><option>Primavera</option>
</select><br>

<label>🎉 Feriado:</label>
<select id="feriado">
<option>Nenhuma delas</option>
<option>Dia do Serviço Público</option>
<option>Dia das Mulheres</option>
<option>Ano Novo</option>
<option>Carnaval</option>
<option>Quarta-feira de Cinzas</option>
<option>Páscoa</option>
<option>Paixão de Cristo</option>
<option>Tiradentes</option>
<option>Dia do Trabalho</option>
<option>Corpus Christi</option>
<option>Independência do Brasil</option>
<option>Nossa Senhora Aparecida</option>
<option>Finados</option>
<option>Proclamação da República</option>
<option>Consciência Negra</option>
<option>Natal</option>
<option>Dia das Mães</option>
<option>Dia dos Pais</option>
<option>São João</option>
<option>Dia dos Namorados</option>
<option>Aniversário</option>
</select><br>

<label>⭐ Borda Dourada (Top Venda):</label>
<select id="bordaDourada"><option>Não</option><option>Sim</option></select><br>

<button onclick="adicionarItem('cor')">Adicionar Cor</button>
<button onclick="adicionarItem('tecido')">Adicionar Tecido</button><br><br>

<h3>🖼️ Galeria de Imagens</h3>
<div class="galeria" id="galeria"></div>
<br>
<button onclick="abrirUpload()">📁 Abrir Galeria</button>
<input type="file" id="fileInput" style="display:none" accept="image/*" onchange="uploadNovaImagem()">
</div>

<div id="excluir" class="content" style="display:none">
<h2>🗑️ Excluir Itens</h2>
<div class="section-title">🎨 Cores</div>
<div id="excluirCores"></div>
<div class="section-title">🧵 Tecidos</div>
<div id="excluirTecidos"></div>
</div>

<!-- 🔥 🔥 🔥 Preview das imagens geradas -->
<div class="preview-area">
<h2>🖼️ Imagem Gerada (Segura e Compartilha)</h2>
<div id="previewArea"></div>
</div>

<canvas id="canvas" width="1080" height="1080" style="display:none"></canvas>

<script>
let imagens = [];
let cores = [];
let tecidos = [];
let imagemSelecionada = '';

function show(id){
document.querySelectorAll('.content').forEach(c=>c.style.display='none');
document.getElementById(id).style.display='block';
if(id==='cores') renderCores();
if(id==='tecidos') renderTecidos();
if(id==='painel') renderGaleria();
if(id==='excluir') renderExcluir();
}

function renderGaleria(){
const g = document.getElementById('galeria');
g.innerHTML = '';
imagens.forEach((img,i)=>{
g.innerHTML += `<img src="${img}" onclick="selecionarImagem(${i})" class="${img===imagemSelecionada?'selecionada':''}">`;
});
}

function selecionarImagem(i){
imagemSelecionada = imagens[i];
renderGaleria();
}

function abrirUpload(){
document.getElementById('fileInput').click();
}

function uploadNovaImagem(){
const file = document.getElementById('fileInput').files[0];
if(file){
const url = URL.createObjectURL(file);
imagens.push(url);
imagemSelecionada = url;
renderGaleria();
}
}

function adicionarItem(tipo){
const nome = document.getElementById('nomeItem').value.trim();
const hex = document.getElementById('hex').value.trim();
const obs = document.getElementById('obsItem').value.trim();
const estacao = document.getElementById('estacao').value;
const feriado = document.getElementById('feriado').value;
const borda = document.getElementById('bordaDourada').value === 'Sim';

if(!nome) return alert('Digite o nome!');
const img = imagemSelecionada || (hex ? gerarImagemCor(hex) : '');
if(tipo==='cor'){
cores.push({nome,imagem:img,obs,estacao:estacao==='Nenhuma delas'?'':estacao,feriado:feriado==='Nenhuma delas'?'':feriado,borda});
alert('Cor adicionada!');
}
if(tipo==='tecido'){
tecidos.push({nome,imagem:img});
alert('Tecido adicionado!');
}
renderCores();
renderTecidos();
}

function gerarImagemCor(hex){
const canvas = document.createElement('canvas');
canvas.width=500;canvas.height=500;
const ctx=canvas.getContext('2d');
ctx.fillStyle=hex;
ctx.fillRect(0,0,500,500);
const url = canvas.toDataURL();
imagens.push(url);
return url;
}

function renderCores(){
const el = document.getElementById('cores');
el.innerHTML='';
cores.forEach((c,i)=>{
el.innerHTML+=`
<div class="item ${c.borda?'dourado':''}">
<div class="color-box" style="background-image:url('${c.imagem}')"></div>
<div>${c.nome}</div>
<small>${c.obs||''}</small><br>
<small>${c.estacao||''} ${c.feriado?'- '+c.feriado:''}</small><br>
<button onclick="showCopyOptions(${i})">📤 Opções da Imagem</button>
<div id="copy${i}" class="copy-options">
<button onclick="abrirPreview(${i},true)">Abrir com Nome</button>
<button onclick="abrirPreview(${i},false)">Abrir Só a Cor</button><br>
<button onclick="baixar(${i},true)">Baixar com Nome</button>
<button onclick="baixar(${i},false)">Baixar Só Cor</button><br>
<button onclick="compartilhar(${i},true)">📤 Compartilhar com Nome</button>
<button onclick="compartilhar(${i},false)">📤 Compartilhar Só Cor</button>
</div>
</div>`;
});
}

function renderTecidos(){
const el = document.getElementById('tecidos');
el.innerHTML='';
tecidos.forEach(t=>{
el.innerHTML+=`
<div class="item">
<img src="${t.imagem}" class="thumb">
<div>${t.nome}</div>
</div>`;
});
}

function showCopyOptions(i){
document.querySelectorAll('.copy-options').forEach(e=>e.style.display='none');
document.getElementById('copy'+i).style.display='block';
}

function abrirPreview(i,comNome){
const canvas=document.createElement('canvas');
canvas.width=500;
canvas.height=500;
const ctx=canvas.getContext('2d');
const img=new Image();
img.crossOrigin="anonymous";
img.onload=()=>{
ctx.clearRect(0,0,500,500);
ctx.drawImage(img,0,0,500,500);
if(comNome) desenhaNome(ctx,cores[i].nome);

canvas.toBlob(blob=>{
const url = URL.createObjectURL(blob);
const p = document.getElementById('previewArea');
const imagem = new Image();
imagem.src = url;
p.innerHTML = '';
p.appendChild(imagem);
});
};
img.src=cores[i].imagem;
}

function baixar(i,comNome){
const canvas=document.getElementById('canvas');
const ctx=canvas.getContext('2d');
const img=new Image();
img.crossOrigin="anonymous";
img.onload=()=>{
ctx.clearRect(0,0,1080,1080);
ctx.drawImage(img,0,0,1080,1080);
if(comNome) desenhaNome(ctx,cores[i].nome);
const a=document.createElement('a');
a.href=canvas.toDataURL();
a.download=comNome?`${cores[i].nome}.png`:`${cores[i].nome}_cor.png`;
a.click();
};
img.src=cores[i].imagem;
}

function desenhaNome(ctx,txt){
ctx.font="bold 60px Impact";
ctx.fillStyle="white";
ctx.textAlign="center";
ctx.fillText(txt,250,280);
}

function compartilhar(i, comNome) {
  const canvas = document.createElement('canvas');
  canvas.width = 500;
  canvas.height = 500;
  const ctx = canvas.getContext('2d');
  const img = new Image();
  img.crossOrigin = "anonymous";

  img.onload = () => {
    ctx.clearRect(0, 0, 500, 500);
    ctx.drawImage(img, 0, 0, 500, 500);
    if (comNome) desenhaNome(ctx, cores[i].nome);

    canvas.toBlob(blob => {
      const file = new File([blob], `${cores[i].nome}.png`, { type: 'image/png' });

      if (navigator.canShare && navigator.canShare({ files: [file] })) {
        navigator.share({
          files: [file],
          title: `Cor: ${cores[i].nome}`,
          text: 'Compartilhando uma cor do Catálogo Linck Master!'
        }).catch(err => alert('Erro ao compartilhar: ' + err));
      } else {
        alert('Seu navegador não suporta compartilhamento de arquivos.');
      }
    }, 'image/png');
  };
  img.src = cores[i].imagem;
}

function renderExcluir(){
const elC = document.getElementById('excluirCores');
elC.innerHTML='';
cores.forEach((c,i)=>{
elC.innerHTML+=`
<div class="item ${c.borda?'dourado':''}" onclick="confirmarExclusao('cor',${i})">
<div class="color-box" style="background-image:url('${c.imagem}')"></div>
<div>${c.nome}</div>
</div>`;
});
const elT = document.getElementById('excluirTecidos');
elT.innerHTML='';
tecidos.forEach((t,i)=>{
elT.innerHTML+=`
<div class="item" onclick="confirmarExclusao('tecido',${i})">
<img src="${t.imagem}" class="thumb">
<div>${t.nome}</div>
</div>`;
});
}

function confirmarExclusao(tipo,i){
const item = tipo==='cor'?cores[i]:tecidos[i];
const conf = confirm(`Tem certeza que deseja excluir '${item.nome}'?`);
if(conf){
if(tipo==='cor'){cores.splice(i,1);renderCores();renderExcluir();}
if(tipo==='tecido'){tecidos.splice(i,1);renderTecidos();renderExcluir();}
alert(`${item.nome} excluído com sucesso!`);
}
}

window.onload=()=>show('cores');
</script>
</body>
</html>
