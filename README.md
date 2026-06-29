
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Sistem Rekomendasi Teman</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial, Helvetica, sans-serif;
}

body{
    background:linear-gradient(135deg,#3f87ff,#5d54a4,#7f53ac);
    min-height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    padding:30px;
}

.container{
    width:900px;
    max-width:100%;
    background:#ffffff;
    border-radius:20px;
    box-shadow:0 10px 30px rgba(0,0,0,.25);
    padding:30px;
}

h2{
    text-align:center;
    color:#3949ab;
    margin-bottom:30px;
    font-size:32px;
}

.section{
    background:#f5f7ff;
    border-left:6px solid #3949ab;
    padding:20px;
    margin-bottom:20px;
    border-radius:12px;
}

.section h3{
    margin-bottom:15px;
    color:#3949ab;
}

.form{
    display:flex;
    gap:10px;
    flex-wrap:wrap;
}

input{
    flex:1;
    min-width:180px;
    padding:12px;
    border:1px solid #ccc;
    border-radius:8px;
    font-size:15px;
    outline:none;
}

input:focus{
    border-color:#3949ab;
    box-shadow:0 0 8px rgba(57,73,171,.3);
}

button{
    padding:12px 20px;
    border:none;
    border-radius:8px;
    color:white;
    cursor:pointer;
    font-size:15px;
    transition:.3s;
}

button:hover{
    transform:scale(1.05);
}

.btn-green{
    background:#28a745;
}

.btn-blue{
    background:#007bff;
}

.btn-purple{
    background:#6f42c1;
}

#output{
    margin-top:15px;
    border:2px solid #d6e4ff;
    background:#eef5ff;
    border-radius:10px;
    padding:20px;
    min-height:80px;
    font-size:16px;
}

.footer{
    text-align:center;
    margin-top:20px;
    color:#666;
}
</style>

</head>
<body>

<div class="container">

<h2>Sistem Rekomendasi Teman</h2>

<div class="section">
<h3>Tambah User</h3>

<div class="form">
<input type="text" id="user" placeholder="Nama User">
<button class="btn-green" onclick="tambahUser()">Tambah User</button>
</div>

</div>

<div class="section">
<h3>Tambah Pertemanan</h3>

<div class="form">
<input type="text" id="user1" placeholder="User 1">
<input type="text" id="user2" placeholder="User 2">
<button class="btn-blue" onclick="tambahTeman()">Tambah Pertemanan</button>
</div>

</div>

<div class="section">
<h3>Cari Rekomendasi Teman</h3>

<div class="form">
<input type="text" id="cariUser" placeholder="Nama User">
<button class="btn-purple" onclick="rekomendasiTeman()">Cari</button>
</div>

</div>

<h3 style="color:#3949ab;">Hasil</h3>

<div id="output">
Belum ada data.
</div>

<div class="footer">
Project Graph - Sistem Rekomendasi Teman
</div>

</div>

<script>

class Graph{

    constructor(){
        this.list={};
    }

    tambahUser(user){
        if(user && !this.list[user]){
            this.list[user]=[];
        }
    }

    tambahTeman(user1,user2){

        this.tambahUser(user1);
        this.tambahUser(user2);

        if(!this.list[user1].includes(user2)){
            this.list[user1].push(user2);
        }

        if(!this.list[user2].includes(user1)){
            this.list[user2].push(user1);
        }
    }

    rekomendasi(user){

        let rekom=new Set();

        if(!this.list[user]){
            return [];
        }

        for(let teman of this.list[user]){

            for(let calon of this.list[teman]){

                if(
                    calon!==user &&
                    !this.list[user].includes(calon)
                ){
                    rekom.add(calon);
                }

            }

        }

        return [...rekom];

    }

}

const graph=new Graph();

function tambahUser(){

    let user=document.getElementById("user").value.trim();

    if(user==""){
        alert("Masukkan nama user.");
        return;
    }

    graph.tambahUser(user);

    document.getElementById("output").innerHTML=
    "User <b>"+user+"</b> berhasil ditambahkan.";

    document.getElementById("user").value="";

}

function tambahTeman(){

    let user1=document.getElementById("user1").value.trim();
    let user2=document.getElementById("user2").value.trim();

    if(user1=="" || user2==""){
        alert("Lengkapi nama user.");
        return;
    }

    graph.tambahTeman(user1,user2);

    document.getElementById("output").innerHTML=
    "<b>"+user1+"</b> dan <b>"+user2+"</b> sekarang berteman.";

    document.getElementById("user1").value="";
    document.getElementById("user2").value="";

}

function rekomendasiTeman(){

    let user=document.getElementById("cariUser").value.trim();

    let hasil=graph.rekomendasi(user);

    document.getElementById("output").innerHTML=
    "<b>Rekomendasi teman untuk "+user+"</b><br><br>"+
    (hasil.length>0 ? hasil.join(", ") : "Tidak ada rekomendasi.");

    document.getElementById("cariUser").value="";

}

</script>

</body>
</html>
