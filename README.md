# Toko-Online-Dapa.
DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Toko Online Dapa</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f8f8f8; }
  header { background: #007bff; color: white; padding: 15px; text-align: center; }
  .container { padding: 15px; }
  .produk { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 15px; }
  .card { background: white; border-radius: 8px; padding: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); text-align: center; }
  .card img { width: 100%; height: 120px; object-fit: cover; border-radius: 5px; }
  .card button { background: #007bff; color: white; border: none; padding: 5px 10px; border-radius: 5px; cursor: pointer; }
  .card button:hover { background: #0056b3; }

  #cart { background: white; padding: 10px; margin-top: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
  #cart ul { list-style: none; padding: 0; }
  #cart li { padding: 5px 0; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
  #total { font-weight: bold; margin-top: 10px; }
  #checkoutForm { display: none; background: white; padding: 15px; border-radius: 8px; margin-top: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
  input, textarea { width: 100%; padding: 8px; border-radius: 5px; border: 1px solid #ccc; margin-bottom: 10px; }
  button { cursor: pointer; }
  .hapus-btn { background: red; color: white; border: none; padding: 4px 8px; border-radius: 5px; }
  .hapus-btn:hover { background: darkred; }
</style>
</head>
<body>

<header>
  <h1>Toko Online Dapa</h1>
</header>

<div class="container" id="belanjaSection">
  <h2>Produk</h2>
  <div class="produk" id="produk"></div>

  <div id="cart">
    <h3>Keranjang Belanja</h3>
    <ul id="cartItems"></ul>
    <p id="total">Total: Rp 0</p>
    <button onclick="checkout()">Checkout</button>
  </div>
</div>

<!-- FORMULIR PENGIRIMAN -->
<div class="container" id="checkoutForm">
  <h2>Formulir Pengiriman</h2>
  <form onsubmit="kirimPesanan(event)">
    <label>Nama Lengkap:</label>
    <input type="text" id="nama" required>

    <label>Alamat Lengkap:</label>
    <textarea id="alamat" required placeholder="Masukkan alamat lengkap..."></textarea>

    <label>No. Telepon:</label>
    <input type="text" id="telepon" required>

    <p id="ringkasanBelanja"></p>

    <button type="submit" style="background:#007bff; color:white; border:none; padding:10px 20px; border-radius:5px;">Kirim Pesanan</button>
  </form>
</div>

<!-- HALAMAN SUKSES -->
<div class="container" id="suksesSection" style="display:none; text-align:center;">
  <h2>✅ Pesanan Berhasil!</h2>
  <p>Terima kasih sudah berbelanja di <b>Toko Online Dapa</b>.</p>
  <button onclick="kembaliKeBelanja()">Kembali ke Belanja</button>
</div>

<script>
const produk = [
  { id: 1, nama: "Jam Graff Diamonds Hallucination", harga: 100000, gambar: "https://via.placeholder.com/150" },
  { id: 2, nama: "Vas Pinner Dinasti Qing", harga: 1420137500000, gambar: "https://via.placeholder.com/150" },
  { id: 3, nama: "Ducati GP 25", harga: 100000000000, gambar: "https://via.placeholder.com/150" },
  { id: 4, nama: "Lukisan Monalisa", harga: 16714000000000, gambar: "https://via.placeholder.com/150" }
];

let cart = [];

function tampilkanProduk() {
  const container = document.getElementById("produk");
  produk.forEach(p => {
    const div = document.createElement("div");
    div.className = "card";
    div.innerHTML = `
      <img src="${p.gambar}" alt="${p.nama}">
      <h4>${p.nama}</h4>
      <p>Rp ${p.harga.toLocaleString()}</p>
      <button onclick="tambahKeCart(${p.id})">Tambah</button>
    `;
    container.appendChild(div);
  });
}

function tambahKeCart(id) {
  const p = produk.find(item => item.id === id);
  cart.push(p);
  tampilkanCart();
}

function hapusDariCart(index) {
  cart.splice(index, 1);
  tampilkanCart();
}

function tampilkanCart() {
  const list = document.getElementById("cartItems");
  list.innerHTML = "";
  let total = 0;

  cart.forEach((item, i) => {
    const li = document.createElement("li");
    li.innerHTML = `
      ${item.nama} - Rp ${item.harga.toLocaleString()}
      <button class="hapus-btn" onclick="hapusDariCart(${i})">X</button>
    `;
    list.appendChild(li);
    total += item.harga;
  });

  document.getElementById("total").textContent = "Total: Rp " + total.toLocaleString();
}

function checkout() {
  if (cart.length === 0) {
    alert("Keranjang masih kosong!");
    return;
  }

  // Tampilkan formulir dan ringkasan
  document.getElementById("belanjaSection").style.display = "none";
  document.getElementById("checkoutForm").style.display = "block";

  let ringkasan = "<h4>Ringkasan Belanja:</h4><ul>";
  let total = 0;
  cart.forEach(item => {
    ringkasan += `<li>${item.nama} - Rp ${item.harga.toLocaleString()}</li>`;
    total += item.harga;
  });
  ringkasan += `</ul><strong>Total: Rp ${total.toLocaleString()}</strong>`;
  document.getElementById("ringkasanBelanja").innerHTML = ringkasan;
}

function kirimPesanan(event) {
  event.preventDefault();

  const nama = document.getElementById("nama").value.trim();
  const alamat = document.getElementById("alamat").value.trim();
  const telepon = document.getElementById("telepon").value.trim();

  if (!nama || !alamat || !telepon) {
    alert("Harap isi semua data pengiriman!");
    return;
  }

  // Kosongkan keranjang
  cart = [];
  tampilkanCart();

  document.getElementById("checkoutForm").style.display = "none";
  document.getElementById("suksesSection").style.display = "block";
}

function kembaliKeBelanja() {
  document.getElementById("suksesSection").style.display = "none";
  document.getElementById("belanjaSection").style.display = "block";
}

tampilkanProduk();
</script>

</body>
</html>
