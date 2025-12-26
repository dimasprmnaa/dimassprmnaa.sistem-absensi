
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sistem Absensi - SMP Negeri 5 Panggarangan</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
:root{
  --navy:#0a1f44; --blue:#2b4eff; --soft:#f4f6ff; --dark:#222; --red:#e55039; --green:#2ed573; --yellow:#ffa502;
}
body{font-family:Poppins,sans-serif;background:var(--soft);margin:0;color:var(--dark);}

/* LOGIN */
#loginContainer{height:100vh;display:flex;justify-content:center;align-items:center;flex-direction:column;}
.login-box{background:white;width:340px;padding:28px;border-radius:18px;box-shadow:0 8px 25px rgba(0,0,0,0.12);animation:fade 0.5s ease;}
@keyframes fade{from{opacity:0;transform:translateY(15px)}to{opacity:1;transform:translateY(0)}}
.input-group{position:relative;margin-bottom:15px;}
.input-group input, .input-group select{width:100%;padding:12px 14px 12px 38px;border:1px solid #e0e5ff;border-radius:10px;font-size:14px;background:var(--soft);}
.input-group i{position:absolute;left:14px;top:50%;transform:translateY(-50%);color:#7a85c5;font-size:13px;}
button{width:100%;padding:12px;background:var(--blue);color:white;border:none;border-radius:10px;font-weight:600;cursor:pointer;transition:0.3s;}
button:hover{transform:scale(1.03);opacity:0.92;}

/* LAYOUT UTAMA */
.sidebar{width:230px;background:var(--navy);height:100vh;position:fixed;top:0;left:0;padding:22px 14px;box-sizing:border-box;transition:0.3s;}
.sidebar.hide{transform:translateX(-260px);}
.sidebar h3{text-align:center;font-size:16px;margin-bottom:22px;color:white;}
.sidebar a{display:flex;align-items:center;gap:10px;padding:12px 14px;border-radius:10px;color:white;text-decoration:none;font-size:14px;margin-bottom:6px;background:rgba(255,255,255,0.07);transition:0.3s;}
.sidebar a:hover, .sidebar a.active{background:var(--blue);}
.topbar{background:white;height:55px;position:fixed;margin-left:230px;width:calc(100% - 230px);top:0;display:flex;align-items:center;justify-content:space-between;padding:0 22px;box-shadow:0 2px 10px rgba(0,0,0,0.06);box-sizing:border-box;border-radius:0 0 12px 12px;transition:0.3s;}
.topbar.full{margin-left:0;width:100%;}
.user-name{font-size:14px;font-weight:600;background:rgba(43,78,255,0.15);padding:6px 12px;border-radius:8px;color:var(--navy);}
.main{margin-left:230px;padding:90px 28px;transition:0.3s;padding-bottom:90px;}
.main.full{margin-left:0;}

/* STATISTIK */
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:12px;margin-bottom:20px;}
.stat-card{padding:16px;border-radius:14px;backdrop-filter:blur(8px);box-shadow:0 3px 12px rgba(0,0,0,0.08);text-align:center;font-size:13px;font-weight:600;color:var(--navy);transition:0.3s;}
.stat-card:hover{transform:translateY(-4px);opacity:0.9;}
.stat-card p{font-size:20px;font-weight:800;margin:6px 0 0;color:var(--dark);}
.stat-card:nth-child(1){ background: rgba(46,213,115,0.2); border:1px solid rgba(46,213,115,0.4); }
.stat-card:nth-child(2){ background: rgba(43,78,255,0.2); border:1px solid rgba(43,78,255,0.4); }
.stat-card:nth-child(3){ background: rgba(255,99,72,0.2); border:1px solid rgba(255,99,72,0.4); }
.stat-card:nth-child(4){ background: rgba(255,165,2,0.2); border:1px solid rgba(255,165,2,0.4); }
.stat-card:nth-child(5){ background: rgba(255,234,167,0.3); border:1px solid rgba(255,234,167,0.6); }
.stat-card:nth-child(6){ background: rgba(30,55,153,0.15); border:1px solid rgba(30,55,153,0.35); }

/* TABEL */
table{width:100%;background:white;border-collapse:collapse;border-radius:14px;overflow:hidden;box-shadow:0 3px 12px rgba(0,0,0,0.08);}
th{background:var(--soft);font-weight:600;padding:12px;font-size:13px;color:var(--navy);}
td{padding:12px;font-size:13px;border-bottom:1px solid #eef2ff;}
tr:last-child td{border-bottom:none;}
button.edit{background:var(--yellow);padding:5px 10px;border-radius:6px;color:white;font-size:12px;width:auto;}
button.hapus{background:var(--red);padding:5px 10px;border-radius:6px;color:white;font-size:12px;width:auto;}

/* WATERMARK */
footer{position:fixed;bottom:0;left:0;width:100%;text-align:center;padding:12px 0;font-size:13px;background:rgba(255,255,255,0.4);backdrop-filter:blur(6px);color:rgba(0,0,0,0.6);z-index:9;font-weight:600;}
</style>
</head>
<body>

<!-- LOGIN -->
<div id="loginContainer">
  <div class="login-box">
    <h3 style="text-align:center;color:var(--navy)">SMP Negeri 5 Panggarangan</h3>
    <p style="text-align:center;font-size:13px;margin-bottom:22px">Sistem Absensi Digital</p>

    <div id="siswaLogin">
      <div class="input-group"><input type="text" id="loginNama" placeholder="Nama Siswa"><i class="fa-solid fa-user"></i></div>
      <div class="input-group"><input type="text" id="loginNISN" placeholder="NISN"><i class="fa-solid fa-id-badge"></i></div>
      <div class="input-group"><input type="text" id="loginKelas" placeholder="Kelas"><i class="fa-solid fa-school"></i></div>
      <button onclick="loginSiswa()">Login Siswa</button>
      <div style="text-align:center;margin-top:12px;font-size:13px;color:var(--blue);cursor:pointer;text-decoration:underline" onclick="showAdminLogin()">Login Admin</div>
    </div>

    <div id="adminLogin" style="display:none">
      <div class="input-group"><input type="text" id="adminUser" placeholder="Username Admin"><i class="fa-solid fa-user-gear"></i></div>
      <div class="input-group"><input type="password" id="adminPass" placeholder="Password"><i class="fa-solid fa-lock"></i></div>
      <button onclick="loginAdmin()">Masuk sebagai Admin</button>
      <div style="text-align:center;margin-top:12px;font-size:13px;color:var(--blue);cursor:pointer;text-decoration:underline" onclick="backToLogin()">Kembali</div>
    </div>
  </div>
</div>

<!-- APLIKASI -->
<div id="app" style="display:none">

  <div class="sidebar" id="sidebar">
    <h3>SMPN 5 Panggarangan</h3>
    <a href="#" class="active" onclick="menuDashboard()"><i class="fa-solid fa-chart-line"></i> Dashboard</a>
    <a href="#" onclick="menuAbsensi()" id="btnAbsensi">Isi Absensi</a>
    <a href="#" onclick="menuPanelAdmin()" id="btnPanelAdmin">Panel Admin</a>
    <a href="#" onclick="logout()"><i class="fa-solid fa-right-from-bracket"></i> Logout</a>
  </div>

  <div class="topbar" id="topbar">
    <i class="fa-solid fa-bars menu-btn" onclick="toggleSidebar()"></i>
    <h4 id="topTitle">Dashboard</h4>
    <span class="user-name" id="userPanel"></span>
  </div>

  <!-- DASHBOARD SISWA -->
  <div class="main" id="dashboardSiswa" style="display:none">
    <h3>Rekap Absensi Saya</h3>
    <table>
      <thead>
        <tr><th>No</th><th>Nama</th><th>NISN</th><th>Kelas</th><th>Status</th><th>Tanggal</th></tr>
      </thead>
      <tbody id="rekapSiswa"></tbody>
    </table>
  </div>

  <!-- HALAMAN ISI ABSENSI SISWA -->
  <div class="main" id="absensiSiswa" style="display:none">
    <h3>Isi Absensi</h3>
    <div class="login-box" style="margin:0 auto;text-align:left">
      <div class="input-group"><input id="ahNama" readonly><i class="fa-solid fa-user"></i></div>
      <div class="input-group"><input id="ahNISN" readonly><i class="fa-solid fa-id-badge"></i></div>
      <div class="input-group"><input id="ahKelas" readonly><i class="fa-solid fa-school"></i></div>
      <div class="input-group">
        <select id="ahStatus">
          <option value="">Pilih Kehadiran</option>
          <option>Hadir</option><option>Sakit</option><option>Izin</option><option>Terlambat</option><option>Alfa</option>
        </select>
        <i class="fa-solid fa-list"></i>
      </div>
      <button onclick="tambah()">Kirim Absensi</button>
    </div>
  </div>

  <!-- PANEL ADMIN -->
  <div class="main" id="panelAdmin" style="display:none">
    <h3>Panel Admin</h3>
    <div class="filter-box" style="margin-bottom:12px;">
      <label>Filter Tanggal:</label>
      <input type="date" id="filterTanggal" onchange="renderPanelAdmin()">
    </div>
    <table>
      <thead>
        <tr><th>No</th><th>Nama</th><th>NISN</th><th>Kelas</th><th>Status</th><th>Tanggal</th><th>Aksi</th></tr>
      </thead>
      <tbody id="rekapPanelAdmin"></tbody>
    </table>
    <button onclick="downloadExcel()" style="width:auto;margin-top:12px;padding:10px 16px;background:var(--green);border-radius:10px;color:white;font-weight:600;cursor:pointer">
      <i class="fa-solid fa-file-excel"></i> Download Excel
    </button>
  </div>

</div>

<footer>© Sistem Absensi 2025 - By Dimas Permana Putra</footer>

<script>
let data=JSON.parse(localStorage.getItem("absen")||"[]");

// LOGIN
function showAdminLogin(){siswaLogin.style.display="none";adminLogin.style.display="block";}
function backToLogin(){adminLogin.style.display="none";siswaLogin.style.display="block";}

function loginSiswa(){
  if(loginNama.value && loginNISN.value && loginKelas.value){
    sessionStorage.setItem("role","siswa");
    sessionStorage.setItem("namaUser",loginNama.value);
    sessionStorage.setItem("nisn",loginNISN.value);
    sessionStorage.setItem("kelas",loginKelas.value);
    loginContainer.style.display="none"; app.style.display="block"; userPanel.innerText=loginNama.value;
    btnPanelAdmin.style.display="none"; btnAbsensi.style.display="flex";
    ahNama.value=loginNama.value; ahNISN.value=loginNISN.value; ahKelas.value=loginKelas.value;
    menuDashboard();
  } else alert("Isi semua data!");
}

function loginAdmin(){
  if(adminUser.value==="Dimas Permana Putra" && adminPass.value==="1234"){
    sessionStorage.setItem("role","admin");
    sessionStorage.setItem("namaUser","Admin");
    loginContainer.style.display="none"; app.style.display="block"; userPanel.innerText="Admin";
    btnPanelAdmin.style.display="flex"; btnAbsensi.style.display="none";
    menuDashboard();
  } else alert("Login gagal!");
}

function logout(){sessionStorage.clear();location.reload();}
function toggleSidebar(){sidebar.classList.toggle("hide");document.querySelectorAll(".main").forEach(m=>m.classList.toggle("full"));topbar.classList.toggle("full");}

// MENU
function menuDashboard(){
  document.querySelectorAll(".main").forEach(m=>m.style.display="none");
  topTitle.innerText="Dashboard";
  if(sessionStorage.getItem("role")==="admin"){panelAdmin.style.display="none"; dashboardAdmin.style.display="block"; renderPanelAdmin(); updateStat();}
  else{dashboardSiswa.style.display="block"; absensiSiswa.style.display="none"; renderRekapSiswa();}
}

function menuAbsensi(){document.querySelectorAll(".main").forEach(m=>m.style.display="none"); absensiSiswa.style.display="block"; topTitle.innerText="Isi Absensi";}
function menuPanelAdmin(){document.querySelectorAll(".main").forEach(m=>m.style.display="none"); panelAdmin.style.display="block"; topTitle.innerText="Panel Admin"; renderPanelAdmin();}

// ABSENSI SISWA
function tambah(){
  const status=ahStatus.value; if(!status){alert("Pilih status dulu!");return;}
  const tgl=new Date().toISOString().split("T")[0];
  data.push({nama:ahNama.value, nisn:ahNISN.value, kelas:ahKelas.value, status, tanggal:tgl});
  localStorage.setItem("absen",JSON.stringify(data)); ahStatus.value=""; menuDashboard();
}

function renderRekapSiswa(){
  const nisn=sessionStorage.getItem("nisn");
  rekapSiswa.innerHTML=data.filter(d=>d.nisn===nisn).map((d,i)=>`
    <tr><td>${i+1}</td><td>${d.nama}</td><td>${d.nisn}</td><td>${d.kelas}</td><td>${d.status}</td><td>${d.tanggal}</td></tr>
  `).join("");
}

// PANEL ADMIN
function renderPanelAdmin(){
  const tgl=filterTanggal.value;
  const tampil= tgl? data.filter(d=>d.tanggal===tgl) : data;
  rekapPanelAdmin.innerHTML=tampil.map((d,i)=>`
    <tr>
      <td>${i+1}</td><td>${d.nama}</td><td>${d.nisn}</td><td>${d.kelas}</td><td>${d.status}</td><td>${d.tanggal}</td>
      <td>
        <button class="edit" onclick="editPanel(${i})">Edit</button>
        <button class="hapus" onclick="hapusPanel(${i})">Hapus</button>
      </td>
    </tr>
  `).join("");
}

function editPanel(i){
  const s=prompt("Ubah Status:", data[i].status);
  if(s){data[i].status=s; localStorage.setItem("absen",JSON.stringify(data)); renderPanelAdmin(); updateStat();}
}

function hapusPanel(i){if(confirm("Hapus data ini?")){data.splice(i,1);localStorage.setItem("absen",JSON.stringify(data)); renderPanelAdmin(); updateStat();}}

function updateStat(){
  if(sessionStorage.getItem("role")==="admin"){
    let jmlMengisi=[...new Set(data.map(d=>d.nisn))].length;
    let jmlHadir=data.filter(d=>d.status==="Hadir").length;
    let jmlSakit=data.filter(d=>d.status==="Sakit").length;
    let jmlIzin=data.filter(d=>d.status==="Izin").length;
    let jmlTelat=data.filter(d=>d.status==="Terlambat").length;
    let jmlAlfa=data.filter(d=>d.status==="Alfa").length;
    dashboardAdmin.innerHTML=`<h3>Statistik Absensi</h3><div class="cards">
      <div class="stat-card"><span>Siswa Mengisi</span><p>${jmlMengisi}</p></div>
      <div class="stat-card"><span>Hadir</span><p>${jmlHadir}</p></div>
      <div class="stat-card"><span>Sakit</span><p>${jmlSakit}</p></div>
      <div class="stat-card"><span>Izin</span><p>${jmlIzin}</p></div>
      <div class="stat-card"><span>Terlambat</span><p>${jmlTelat}</p></div>
      <div class="stat-card"><span>Alfa</span><p>${jmlAlfa}</p></div>
    </div>`;
  }
}

// DOWNLOAD EXCEL
function downloadExcel(){
  const ws=XLSX.utils.json_to_sheet(data);
  const wb=XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb,ws,"Absensi");
  XLSX.writeFile(wb,"absensi.xlsx");
}
</script>

</body>
</html>
