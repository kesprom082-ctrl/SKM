<html lang="id">
<head>
<meta charset="utf-8">

<!-- ANDROID APP META -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<meta name="theme-color" content="#16a34a">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">

<title>SKM RSUD Naibonat</title>

<script src="https://cdn.tailwindcss.com"></script>

<style>
html,body{
 height:100%;
 margin:0;
 background:#f3f4f6;
}
.app{
 display:flex;
 flex-direction:column;
 height:100%;
}
header{
 background:#16a34a;
 color:white;
 padding:14px;
 text-align:center;
 font-weight:700;
 font-size:18px;
}
main{
 flex:1;
 overflow:auto;
 padding:16px;
}
input,select,textarea,button{
 font-size:16px;
}
button{
 padding:14px !important;
 border-radius:12px !important;
}
.card{
 background:white;
 border-radius:16px;
 padding:16px;
 box-shadow:0 6px 16px rgba(0,0,0,.08);
}
</style>
</head>

<body>

<div class="app">

<header>
 RSUD NAIBONAT<br>
 <span style="font-size:14px;font-weight:400">Survei Kepuasan Masyarakat</span>
</header>

<main>

<div class="max-w-3xl mx-auto card">

<div class="text-center mb-4">
<img src="ChatGPT Image Mar 5, 2026, 08_57_39 AM.png"
 class="w-full max-h-60 object-cover rounded-xl mb-3">
<img src="https://iili.io/CCEYxLX.th.png" class="mx-auto h-16">
</div>

<h1 class="text-xl font-bold mb-4 text-center">Survei Kepuasan</h1>

<form id="surveyForm">

<!-- STEP 1 -->
<div id="step1">
<h2 class="font-bold mb-3">Langkah 1 - Data Responden</h2>

<input class="border p-3 w-full mb-3 rounded-xl" name="nama" placeholder="Nama Lengkap" required>
<input class="border p-3 w-full mb-3 rounded-xl" name="usia" placeholder="Usia" required>

<select class="border p-3 w-full mb-3 rounded-xl" name="jk" required>
<option value="">Jenis Kelamin</option>
<option>Laki-laki</option>
<option>Perempuan</option>
</select>

<input class="border p-3 w-full mb-3 rounded-xl" name="rm" placeholder="Nomor RM" required>
<input class="border p-3 w-full mb-3 rounded-xl" name="tanggal" type="date" required>

<select class="border p-3 w-full mb-4 rounded-xl" id="jenisKunjungan" required>
<option value="">Jenis Kunjungan</option>
<option>Rawat Jalan</option>
<option>Rawat Inap</option>
<option>IGD</option>
</select>

<button type="button" onclick="nextStep()" class="bg-green-600 text-white w-full">
LANJUT
</button>
</div>

<!-- STEP 2 -->
<div id="step2" style="display:none">
<h2 class="font-bold mb-3">Langkah 2 - Pertanyaan SKM</h2>
<div id="questions"></div>

<div class="flex gap-3 mt-4">
<button type="button" onclick="prevStep()" class="bg-gray-500 text-white w-1/2">
KEMBALI
</button>
<button type="button" onclick="nextStep2()" class="bg-green-600 text-white w-1/2">
LANJUT
</button>
</div>
</div>

<!-- STEP 3 -->
<div id="step3" style="display:none">
<h2 class="font-bold mb-3">Langkah 3 - Saran</h2>

<textarea class="border p-3 w-full mb-4 rounded-xl" name="saran" rows="4"
 placeholder="Tulis saran Anda (opsional)"></textarea>

<div class="flex gap-3">
<button type="button" onclick="prevStep2()" class="bg-gray-500 text-white w-1/2">
KEMBALI
</button>
<button type="submit" class="bg-blue-600 text-white w-1/2">
KIRIM
</button>
</div>
</div>

</form>

</div>
</main>

</div>

<!-- SCRIPT ASLI ANDA (TIDAK DIUBAH LOGIKA) -->
<script>
/* === SCRIPT ANDA TETAP === */
const rawatJalan=[
"Apakah ada kemudahan syarat pelayanan?",
"Apakah alur pelayanan mudah atau berbelit-belit?",
"Apakah pelayanan cepat atau lambat?",
"Apakah biaya sesuai ketentuan?",
"Apakah hasil pelayanan sesuai dengan standar yang ditetapkan?",
"Apakah petugas memberi pelayanan dengan baik, terampil, dan kompeten?",
"Apakah petugas berperilaku sopan, ramah, dan responsif kepada pasien?",
"Apakah sarana dan prasarana memadai?",
"Apakah tersedia media pengaduan (Kotak Saran, Barcode, Buku Pengaduan)?"
];

const rawatInap=[
"Apakah prosedur masuk ruang rawat inap mudah dan jelas?",
"Apakah pelayanan administrasi rawat inap cepat dan mudah?",
"Apakah perawat memberikan pelayanan dengan cepat saat dibutuhkan?",
"Apakah dokter memberikan penjelasan yang mudah dipahami mengenai kondisi pasien?",
"Apakah petugas memberikan pelayanan yang baik, terampil, dan kompeten?",
"Apakah petugas bersikap sopan, ramah, dan menghargai pasien?",
"Apakah kebersihan ruang rawat inap memadai?",
"Apakah fasilitas ruang rawat inap memadai?",
"Apakah tersedia media pengaduan yang mudah digunakan?"
];

function nextStep(){

    const form = document.getElementById('surveyForm');

    if(!form.nama.value.trim()){
        alert('Nama Lengkap belum diisi!');
        form.nama.focus();
        return;
    }

    if(!form.usia.value.trim()){
        alert('Usia belum diisi!');
        form.usia.focus();
        return;
    }

    if(!form.jk.value){
        alert('Jenis Kelamin belum dipilih!');
        form.jk.focus();
        return;
    }

    if(!form.rm.value.trim()){
        alert('Nomor RM belum diisi!');
        form.rm.focus();
        return;
    }

    if(!form.tanggal.value){
        alert('Tanggal belum dipilih!');
        form.tanggal.focus();
        return;
    }

    const jenis = document.getElementById('jenisKunjungan').value;

    if(!jenis){
        alert('Jenis Kunjungan belum dipilih!');
        document.getElementById('jenisKunjungan').focus();
        return;
    }

    buildQuestions(jenis);
    step1.style.display='none';
    step2.style.display='block';
}
function prevStep(){step2.style.display='none';step1.style.display='block';}
function nextStep2(){

    const jenis = document.getElementById('jenisKunjungan').value;
    const qs = (jenis === 'Rawat Inap') ? rawatInap : rawatJalan;

    for(let i=1; i<=qs.length; i++){

        const jawaban = document.querySelector(`input[name="q${i}"]:checked`);

        if(!jawaban){

            alert('Pertanyaan nomor ' + i + ' belum dijawab!');

            document.querySelector(`input[name="q${i}"]`).scrollIntoView({
                behavior:'smooth',
                block:'center'
            });

            return;
        }
    }

    step2.style.display='none';
    step3.style.display='block';
}
function prevStep2(){step3.style.display='none';step2.style.display='block';}

function buildQuestions(jenis){
 const qs=(jenis==='Rawat Inap')?rawatInap:rawatJalan;
 let html='';
 qs.forEach((q,i)=>{
   html+=`<div class="mb-4">
   <p class="font-semibold mb-2">${i+1}. ${q}</p>
   <label><input type="radio" name="q${i+1}" value="1" required> Tidak Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="2"> Kurang Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="3"> Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="4"> Sangat Baik</label>
   </div>`;
 });
 document.getElementById('questions').innerHTML=html;
}
document.getElementById('surveyForm').addEventListener('submit', async (e) => {
    e.preventDefault();

    const form = document.getElementById('surveyForm');

    const data = {
        nama: form.nama.value,
        usia: form.usia.value,
        jk: form.jk.value,
        rm: form.rm.value,
        tanggal: form.tanggal.value,
        jenisKunjungan: document.getElementById('jenisKunjungan').value,
        saran: form.saran.value,
        jawaban: []
    };

    document.querySelectorAll('input[type="radio"]:checked').forEach(item => {
        data.jawaban.push(item.value);
    });

    try {

        const response = await fetch(
            'https://script.google.com/macros/s/AKfycbxmwAEcaHGriiqDmlSxdwdlBTypdNPl9HRelM7UUs2E-v3WvF7ZcUaPrQbCromidlPSAw/exec',
            {
                method: 'POST',
                headers: {
                    'Content-Type': 'text/plain;charset=utf-8'
                },
                body: JSON.stringify(data)
            }
        );

        const result = await response.json();

        if(result.success){
            alert('Terima kasih, survei berhasil dikirim.');
            form.reset();
            location.reload();
        }else{
            alert('Data gagal dikirim.');
        }

    } catch(error){
        console.error(error);
        alert('Koneksi ke server gagal.');
    }
});</script><script>
