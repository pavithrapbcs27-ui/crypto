<!DOCTYPE html>
<html>
<head>
<title>DES Step by Step (HEX + CHAR Output)</title>
<style>
body{font-family:Arial;background:#eef;padding:20px}
textarea{width:100%;height:60px}
button{padding:10px;margin:5px}
pre{background:white;padding:15px;border-radius:8px;overflow:auto}
</style>
</head>
<body>

<h2>DES Encryption & Decryption (Step-By-Step)</h2>

Plaintext (8 chars):
<textarea id="plain"></textarea>

Key (8 chars):
<textarea id="key"></textarea>

<button onclick="encryptDES()">Encrypt</button>
<button onclick="decryptDES()">Decrypt</button>

<pre id="output"></pre>

<script>
function toBytes(str){ return Array.from(str).map(c=>c.charCodeAt(0)); }

function toHex(arr){
 return arr.map(b=>b.toString(16).padStart(2,"0")).join(" ");
}

function hexToChar(arr){
 return arr.map(b=>String.fromCharCode(b)).join("");
}

function xor(a,b){ return a.map((v,i)=>v^b[i]); }

function feistel(R,key){ return xor(R,key.slice(0,4)); }

function encryptDES(){

let plain=document.getElementById("plain").value;
let key=document.getElementById("key").value;

if(plain.length!==8||key.length!==8){
 alert("Enter exactly 8 characters for Plaintext and Key");
 return;
}

let bytes=toBytes(plain);
let keyB=toBytes(key);

let L=bytes.slice(0,4);
let R=bytes.slice(4,8);

let out="PLAINTEXT: "+plain+"\n";
out+="KEY: "+key+"\n\n";

for(let r=1;r<=16;r++){
 out+="--- Round "+r+" ---\n";
 let f=feistel(R,keyB);
 out+="F(R,K): "+toHex(f)+"\n";

 let newR=xor(L,f);
 out+="New R: "+toHex(newR)+"\n";

 L=R;
 R=newR;

 out+="L"+r+": "+toHex(L)+"\n";
 out+="R"+r+": "+toHex(R)+"\n\n";
}

window.desCipher=L.concat(R);

out+="CIPHERTEXT (HEX): "+toHex(window.desCipher)+"\n";
out+="CIPHERTEXT (CHAR): "+hexToChar(window.desCipher)+"\n";

document.getElementById("output").innerText=out;
}

function decryptDES(){

if(!window.desCipher){
 alert("Encrypt first");
 return;
}

let key=document.getElementById("key").value;
let keyB=toBytes(key);

let bytes=[...window.desCipher];

let L=bytes.slice(0,4);
let R=bytes.slice(4,8);

let out="CIPHERTEXT (HEX): "+toHex(bytes)+"\n";
out+="CIPHERTEXT (CHAR): "+hexToChar(bytes)+"\n";
out+="KEY: "+key+"\n\n";

for(let r=16;r>=1;r--){
 out+="--- Round "+r+" ---\n";

 let f=feistel(L,keyB);
 out+="F(L,K): "+toHex(f)+"\n";

 let newL=xor(R,f);

 R=L;
 L=newL;

 out+="L"+r+": "+toHex(L)+"\n";
 out+="R"+r+": "+toHex(R)+"\n\n";
}

let recovered=L.concat(R);

out+="Recovered Plaintext (HEX): "+toHex(recovered)+"\n";
out+="Recovered Plaintext (CHAR): "+
recovered.map(b=>String.fromCharCode(b)).join("");

document.getElementById("output").innerText=out;
}
</script>

</body>
</html>
