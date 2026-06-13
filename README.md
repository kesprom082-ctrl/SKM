<html lang="id">
<head>
<meta charset="utf-8"/>
<meta content="width=device-width, initial-scale=1.0" name="viewport"/>
<title>SKM</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-6">
<div class="max-w-4xl mx-auto bg-white rounded-xl shadow p-6">
<div style="text-align:center;margin-bottom:20px">
<img src="ChatGPT Image Mar 5, 2026, 08_57_39 AM.png" style="width:100%;max-height:260px;object-fit:cover;border-radius:12px;margin-bottom:10px"/>
<div style="display:flex;justify-content:center;align-items:center;gap:15px;flex-wrap:wrap">
<img src="https://iili.io/CCEYxLX.th.png" alt="CCEYxLX.th.png" border="0">

</div>
</div>

<h1 class="text-2xl font-bold mb-4">Survei Kepuasan</h1>
<form id="surveyForm">
<div id="step1">
<h2 class="font-bold mb-3">Langkah 1 - Data Responden</h2>
<input class="border p-2 w-full mb-2" name="nama" placeholder="Nama Lengkap" required=""/>
<input class="border p-2 w-full mb-2" name="usia" placeholder="Usia" required=""/>
<select class="border p-2 w-full mb-2" name="jk" required="">
<option value="">Jenis Kelamin</option>
<option>Laki-laki</option>
<option>Perempuan</option>
</select>
<input class="border p-2 w-full mb-2" name="rm" placeholder="Nomor RM" required=""/>
<input class="border p-2 w-full mb-2" name="tanggal" required="" type="date"/>
<select class="border p-2 w-full mb-2" id="jenisKunjungan" required="">
<option value="">Jenis Kunjungan</option>
<option>Rawat Jalan</option>
<option>Rawat Inap</option>
<option>IGD</option>
</select>
<button class="bg-green-600 text-white px-4 py-2 rounded" onclick="nextStep()" type="button">Lanjut</button>
</div>
<div id="step2" style="display:none">
<h2 class="font-bold mb-3">Langkah 2 - Pertanyaan SKM</h2>
<div id="questions"></div>
<button class="bg-gray-500 text-white px-4 py-2 rounded" onclick="prevStep()" type="button">Kembali</button>
<button class="bg-green-600 text-white px-4 py-2 rounded" onclick="nextStep2()" type="button">Lanjut</button>
</div>
<div id="step3" style="display:none">
<h2 class="font-bold mb-3">Langkah 3 - Saran</h2>
<textarea class="border p-2 w-full mb-3" name="saran"></textarea>
<button class="bg-gray-500 text-white px-4 py-2 rounded" onclick="prevStep2()" type="button">Kembali</button>
<button class="bg-blue-600 text-white px-4 py-2 rounded" type="submit">Kirim</button>
</div>
</form>
</div>
<script>
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
 const jenis=document.getElementById('jenisKunjungan').value;
 if(!jenis){alert('Pilih jenis kunjungan');return;}
 buildQuestions(jenis);
 step1.style.display='none';
 step2.style.display='block';
}

function prevStep(){step2.style.display='none';step1.style.display='block';}
function nextStep2(){step2.style.display='none';step3.style.display='block';}
function prevStep2(){step3.style.display='none';step2.style.display='block';}

function buildQuestions(jenis){
 const qs=(jenis==='Rawat Inap')?rawatInap:rawatJalan;
 let html='';
 qs.forEach((q,i)=>{
   html+=`<div style="margin-bottom:15px">
   <p><b>${i+1}. ${q}</b></p>
   <label><input type="radio" name="q${i+1}" value="1" required> 1 Tidak Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="2"> 2 Kurang Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="3"> 3 Baik</label><br>
   <label><input type="radio" name="q${i+1}" value="4"> 4 Sangat Baik</label>
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
});</script>
