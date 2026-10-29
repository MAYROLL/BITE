
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kedai Rasa | Pesan Makanan</title>

  <style>
    /* ===== 1. PENGATURAN WARNA ===== */
    :root {
      --cokelat: #603b2a;
      --krem: #fff8ef;
      --peach: #f1dfcd;
      --putih: #ffffff;
      --hijau: #23734b;
      --merah: #b83232;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: var(--krem);
      color: #30251f;
    }

    button, input, textarea, select {
      font: inherit;
    }

    button {
      cursor: pointer;
    }

    header {
      background: var(--cokelat);
      color: white;
      padding: 16px;
    }

    .topbar {
      max-width: 1100px;
      margin: auto;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
    }

    .container {
      max-width: 1100px;
      margin: auto;
      padding: 20px;
    }

    .hero {
      background: var(--peach);
      padding: 35px 20px;
      border-radius: 18px;
      margin-bottom: 24px;
    }

    h1, h2, h3 {
      color: var(--cokelat);
    }

    header h2 {
      color: white;
      margin: 0;
    }

    .layout {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 320px;
      gap: 20px;
      align-items: start;
    }

    .menu-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 16px;
    }

    .card, .panel {
      background: var(--putih);
      border: 1px solid #eadaca;
      border-radius: 16px;
      overflow: hidden;
    }

    .food-image {
      width: 100%;
      height: 170px;
      object-fit: cover;
      background: var(--peach);
      display: block;
    }

    .food-placeholder {
      height: 170px;
      background: var(--peach);
      display: grid;
      place-items: center;
      font-size: 60px;
    }

    .card-content, .panel-content {
      padding: 14px;
    }

    .price {
      color: var(--cokelat);
      font-size: 18px;
      font-weight: bold;
    }

    .muted {
      color: #766960;
      font-size: 13px;
    }

    .button {
      padding: 10px 14px;
      border: none;
      border-radius: 9px;
      background: var(--cokelat);
      color: white;
      margin: 3px 0;
    }

    .button-light {
      background: #f1dfcd;
      color: var(--cokelat);
    }

    .button-red {
      background: var(--merah);
      color: white;
    }

    .button-green {
      background: var(--hijau);
      color: white;
    }

    .full {
      width: 100%;
    }

    input, textarea, select {
      width: 100%;
      padding: 10px;
      border: 1px solid #d8c8b8;
      border-radius: 8px;
      margin: 5px 0 12px;
      background: white;
    }

    label {
      font-size: 14px;
      font-weight: bold;
    }

    .cart {
      position: sticky;
      top: 15px;
    }

    .cart-item {
      padding: 10px 0;
      border-bottom: 1px solid #eee0d4;
    }

    .row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
    }

    .admin {
      margin-top: 25px;
      padding: 20px;
    }

    .admin-item, .order-item {
      border: 1px solid #eadaca;
      border-radius: 10px;
      padding: 12px;
      margin: 10px 0;
      overflow-wrap: anywhere;
    }

    .hidden {
      display: none !important;
    }

    .queue-number {
      font-size: 30px;
      font-weight: bold;
      color: var(--cokelat);
    }

    footer {
      background: var(--cokelat);
      color: white;
      text-align: center;
      padding: 22px;
      margin-top: 30px;
    }

    @media (max-width: 760px) {
      .layout {
        grid-template-columns: 1fr;
      }

      .cart {
        position: static;
      }

      .menu-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }

      .food-image, .food-placeholder {
        height: 125px;
      }
    }

    @media (max-width: 380px) {
      .menu-grid {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>

<body>

  <!-- ===== 2. HEADER WEBSITE ===== -->
  <header>
    <div class="topbar">
      <div>
        <h2>🍽️ Kedai Rasa</h2>
        <small>Pesan sendiri, antre lebih mudah</small>
      </div>

      <button class="button button-light"
              onclick="showAdmin()">
        Panel Admin
      </button>
    </div>
  </header>

  <main class="container">

    <!-- ===== 3. BAGIAN PEMBUKA ===== -->
    <section class="hero">
      <h1>Selamat Datang di Kedai Rasa!</h1>
      <p>
        Pilih makanan favoritmu, masukkan ke keranjang,
        lalu pesan dengan mudah.
      </p>
      <a href="#menu">Lihat menu makanan ↓</a>
    </section>

    <!-- ===== 4. KATALOG DAN KERANJANG ===== -->
    <div class="layout">

      <section id="menu">
        <h2>Daftar Menu</h2>

        <input
          id="search"
          type="search"
          placeholder="Cari nama makanan..."
          oninput="renderMenu()"
        >

        <div id="menuGrid" class="menu-grid"></div>
      </section>

      <aside class="panel cart">
        <div class="panel-content">
          <h2>🛒 Keranjang</h2>
          <div id="cartItems">Keranjang masih kosong.</div>

          <hr>

          <div class="row">
            <strong>Total</strong>
            <strong id="cartTotal">Rp0</strong>
          </div>

          <h3>Data Pemesan</h3>

          <form id="orderForm">
            <label for="customerName">Nama pemesan</label>
            <input id="customerName"
                   required
                   maxlength="80"
                   placeholder="Nama kamu">

            <label for="customerPhone">Nomor HP</label>
            <input id="customerPhone"
                   required
                   maxlength="20"
                   placeholder="08xxxxxxxxxx">

            <label for="payment">Pembayaran</label>
            <select id="payment">
              <option value="Tunai">Tunai di kasir</option>
              <option value="QRIS">QRIS</option>
            </select>

            <label for="note">Catatan pesanan</label>
            <textarea id="note"
                      maxlength="300"
                      placeholder="Contoh: tidak pedas"></textarea>

            <button class="button full" type="submit">
              Pesan Sekarang
            </button>
          </form>

          <div id="orderResult"></div>
        </div>
      </aside>
    </div>

    <!-- ===== 5. PAPAN ANTREAN ===== -->
    <section class="panel admin">
      <div class="panel-content">
        <h2>🔔 Papan Antrean</h2>
        <div id="queueBoard">Belum ada pesanan.</div>
      </div>
    </section>

    <!-- ===== 6. PANEL ADMIN ===== -->
    <section id="adminPanel" class="panel admin hidden">
      <div class="panel-content">
        <h2>⚙️ Panel Admin</h2>

        <!-- Form login -->
        <div id="loginArea">
          <p>Masukkan sandi untuk mengelola menu.</p>

          <form id="loginForm">
            <label for="adminPassword">Sandi admin</label>
            <input id="adminPassword"
                   type="password"
                   required
                   placeholder="Masukkan sandi">

            <button class="button" type="submit">
              Masuk
            </button>

            <button class="button button-light"
                    type="button"
                    onclick="hideAdmin()">
              Tutup
            </button>
          </form>

          <p id="loginMessage"></p>
        </div>

        <!-- Konten setelah login -->
        <div id="adminContent" class="hidden">

          <button class="button button-light"
                  onclick="adminLogout()">
            Keluar Admin
          </button>

          <hr>

          <h3>Tambah atau Edit Menu</h3>

          <form id="menuForm">
            <input type="hidden" id="editId">

            <label for="menuName">Nama makanan</label>
            <input id="menuName"
                   required
                   maxlength="100"
                   placeholder="Contoh: Nasi Goreng">

            <label for="menuDescription">Deskripsi</label>
            <textarea id="menuDescription"
                      maxlength="300"
                      placeholder="Deskripsi makanan"></textarea>

            <label for="menuPrice">Harga (rupiah)</label>
            <input id="menuPrice"
                   type="number"
                   min="0"
                   step="500"
                   required
                   placeholder="15000">

            <label for="menuStock">Jumlah stok</label>
            <input id="menuStock"
                   type="number"
                   min="0"
                   step="1"
                   required
                   placeholder="20">

            <label for="menuImage">Foto makanan</label>
            <input id="menuImage"
                   type="file"
                   accept="image/jpeg,image/png,image/webp">

            <p class="muted">
              Foto baru akan mengganti foto lama.
              Jika tidak memilih foto saat mengedit,
              foto lama tetap digunakan.
            </p>

            <button class="button" type="submit">
              Simpan Menu
            </button>

            <button class="button button-light"
                    type="button"
                    onclick="resetMenuForm()">
              Batal Edit
            </button>
          </form>

          <hr>

          <h3>Kelola Menu dan Stok</h3>
          <div id="adminMenuList"></div>

          <hr>

          <h3>Pesanan Masuk</h3>
          <div id="adminOrders"></div>

        </div>
      </div>
    </section>

  </main>

  <footer>
    <strong>Kedai Rasa</strong>
    <p>Terima kasih sudah memesan!</p>
  </footer>

  <script>
    // ===== 7. PENGATURAN AWAL =====

    // Sandi demo. Tidak aman untuk website publik.
    const DEMO_ADMIN_PASSWORD = "KedaiAdmin#2026";

    // Data menu contoh.
    const DEFAULT_MENU = [
      {
        id: 1,
        name: "Nasi Goreng",
        description: "Nasi goreng dengan bumbu pilihan.",
        price: 15000,
        stock: 20,
        image: "",
        emoji: "🍛"
      },
      {
        id: 2,
        name: "Mie Goreng",
        description: "Mie goreng gurih dan lezat.",
        price: 12000,
        stock: 15,
        image: "",
        emoji: "🍜"
      },
      {
        id: 3,
        name: "Ayam Geprek",
        description: "Ayam renyah dengan sambal.",
        price: 18000,
        stock: 10,
        image: "",
        emoji: "🍗"
      },
      {
        id: 4,
        name: "Es Teh",
        description: "Teh dingin menyegarkan.",
        price: 5000,
        stock: 30,
        image: "",
        emoji: "🧋"
      }
    ];

    // ===== 8. PENYIMPANAN DEMO =====

    function loadData(key, fallback) {
      try {
        const saved = localStorage.getItem(key);
        return saved ? JSON.parse(saved) : fallback;
      } catch (error) {
        console.error("Gagal membaca data:", error);
        return fallback;
      }
    }

    let menu = loadData("kedai_menu", DEFAULT_MENU);
    let orders = loadData("kedai_orders", []);
    let cart = {};
    let isAdmin = false;
    let nextOrderNumber =
      loadData("kedai_next_number", 1);

    function saveData() {
      try {
        localStorage.setItem("kedai_menu", JSON.stringify(menu));
        localStorage.setItem("kedai_orders", JSON.stringify(orders));
        localStorage.setItem("kedai_next_number",
                             JSON.stringify(nextOrderNumber));
        return true;
      } catch (error) {
        alert("Penyimpanan penuh atau tidak tersedia. Coba gunakan foto yang lebih kecil.");
        return false;
      }
    }

    // ===== 9. FORMAT RUPIAH =====

    function rupiah(value) {
      return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        maximumFractionDigits: 0
      }).format(value);
    }

    function escapeHTML(value) {
      return String(value).replace(/[&<>"']/g, function(char) {
        return {
          "&": "&amp;",
          "<": "&lt;",
          ">": "&gt;",
          '"': "&quot;",
          "'": "&#39;"
        }[char];
      });
    }

    // ===== 10. MENAMPILKAN MENU =====

    function renderMenu() {
      const grid = document.getElementById("menuGrid");
      const search = document.getElementById("search")
        .value.toLowerCase().trim();

      const filtered = menu.filter(item =>
        item.name.toLowerCase().includes(search)
      );

      if (filtered.length === 0) {
        grid.innerHTML = "<p>Menu tidak ditemukan.</p>";
        return;
      }

      grid.innerHTML = filtered.map(item => {
        const soldOut = item.stock <= 0;

        const image = item.image
          ? `<img class="food-image"
                  src="${item.image}"
                  alt="${escapeHTML(item.name)}">`
          : `<div class="food-placeholder">${item.emoji || "🍽️"}</div>`;

        return `
          <article class="card">
            ${image}

            <div class="card-content">
              <h3>${escapeHTML(item.name)}</h3>

              <p class="muted">
                ${escapeHTML(item.description || "")}
              </p>

              <p class="price">${rupiah(item.price)}</p>

              <p class="muted">
                ${soldOut ? "Stok habis" : "Stok: " + item.stock}
              </p>

              <button class="button full"
                ${soldOut ? "disabled" : ""}
                onclick="addToCart(${item.id})">
                ${soldOut ? "Habis" : "Tambah ke Keranjang"}
              </button>
            </div>
          </article>
        `;
      }).join("");
    }

    // ===== 11. KERANJANG =====

    function addToCart(id) {
      const item = menu.find(product => product.id === id);

      if (!item || item.stock <= 0) {
        alert("Menu sedang habis.");
        return;
      }

      const quantity = cart[id] || 0;

      if (quantity >= item.stock) {
        alert("Jumlah melebihi stok yang tersedia.");
        return;
      }

      cart[id] = quantity + 1;
      renderCart();
    }

    function changeQuantity(id, change) {
      const item = menu.find(product => product.id === id);
      if (!item) return;

      const newQuantity = (cart[id] || 0) + change;

      if (newQuantity <= 0) {
        delete cart[id];
      } else if (newQuantity <= item.stock) {
        cart[id] = newQuantity;
      } else {
        alert("Jumlah melebihi stok yang tersedia.");
      }

      renderCart();
    }

    function renderCart() {
      const container = document.getElementById("cartItems");
      let total = 0;
      let html = "";

      Object.keys(cart).forEach(id => {
        const item = menu.find(product => product.id === Number(id));
        const quantity = cart[id];

        if (!item || quantity <= 0) return;

        const subtotal = item.price * quantity;
        total += subtotal;

        html += `
          <div class="cart-item">
            <strong>${escapeHTML(item.name)}</strong>
            <p>${rupiah(subtotal)}</p>

            <button class="button button-light"
                    onclick="changeQuantity(${item.id}, -1)">
              −
            </button>

            <strong>${quantity}</strong>

            <button class="button button-light"
                    onclick="changeQuantity(${item.id}, 1)">
              +
            </button>
          </div>
        `;
      });

      container.innerHTML = html || "Keranjang masih kosong.";
      document.getElementById("cartTotal").textContent = rupiah(total);
    }

    // ===== 12. MEMBUAT PESANAN =====

    document.getElementById("orderForm")
      .addEventListener("submit", function(event) {
        event.preventDefault();

        if (Object.keys(cart).length === 0) {
          alert("Keranjang kamu masih kosong.");
          return;
        }

        // Periksa kembali stok sebelum menyimpan.
        for (const id of Object.keys(cart)) {
          const item = menu.find(product => product.id === Number(id));

          if (!item || cart[id] > item.stock) {
            alert("Stok " + (item ? item.name : "menu") +
                  " tidak mencukupi. Periksa keranjang.");
            renderMenu();
            renderCart();
            return;
          }
        }

        const items = Object.keys(cart).map(id => {
          const item = menu.find(product => product.id === Number(id));

          return {
            id: item.id,
            name: item.name,
            price: item.price,
            quantity: cart[id]
          };
        });

        const total = items.reduce((sum, item) =>
          sum + item.price * item.quantity, 0
        );

        const order = {
          number: nextOrderNumber,
          customer: document.getElementById("customerName").value.trim(),
          phone: document.getElementById("customerPhone").value.trim(),
          payment: document.getElementById("payment").value,
          note: document.getElementById("note").value.trim(),
          items: items,
          total: total,
          status: "Menunggu",
          time: new Date().toLocaleString("id-ID")
        };

        if (!order.customer || !order.phone) {
          alert("Lengkapi nama dan nomor HP.");
          return;
        }

        // Kurangi stok dan simpan pesanan.
        items.forEach(orderItem => {
          const item = menu.find(product => product.id === orderItem.id);
          item.stock -= orderItem.quantity;
        });

        orders.unshift(order);
        nextOrderNumber++;

        if (!saveData()) return;

        document.getElementById("orderResult").innerHTML = `
          <div class="panel-content">
            <h3>Pesanan berhasil dibuat!</h3>
            <p>Nomor antrean kamu:</p>
            <div class="queue-number">
              ${String(order.number).padStart(3, "0")}
            </div>
            <p>Total: <strong>${rupiah(order.total)}</strong></p>
            <p>Status: ${escapeHTML(order.status)}</p>
            <p class="muted">
              Pembayaran: ${escapeHTML(order.payment)}.
              ${order.payment === "QRIS"
                ? "Silakan konfirmasi pembayaran kepada kasir."
                : "Silakan bayar di kasir."}
            </p>
          </div>
        `;

        cart = {};
        document.getElementById("orderForm").reset();

        renderAll();
      });

    // ===== 13. PAPAN ANTREAN =====

    function renderQueue() {
      const board = document.getElementById("queueBoard");

      if (orders.length === 0) {
        board.textContent = "Belum ada pesanan.";
        return;
      }

      board.innerHTML = orders.slice(0, 10).map(order => `
        <div class="order-item">
          <div class="row">
            <strong>
              Antrean ${String(order.number).padStart(3, "0")}
            </strong>
            <strong>${escapeHTML(order.status)}</strong>
          </div>

          <p>${escapeHTML(order.customer)}</p>
          <p>Total: ${rupiah(order.total)}</p>
        </div>
      `).join("");
    }

    // ===== 14. LOGIN ADMIN DEMO =====

    function showAdmin() {
      document.getElementById("adminPanel")
        .classList.remove("hidden");

      document.getElementById("adminPanel")
        .scrollIntoView({ behavior: "smooth" });
    }

    function hideAdmin() {
      document.getElementById("adminPanel")
        .classList.add("hidden");
    }

    document.getElementById("loginForm")
      .addEventListener("submit", function(event) {
        event.preventDefault();

        const password =
          document.getElementById("adminPassword").value;

        if (password === DEMO_ADMIN_PASSWORD) {
          isAdmin = true;

          document.getElementById("loginArea")
            .classList.add("hidden");

          document.getElementById("adminContent")
            .classList.remove("hidden");

          document.getElementById("loginMessage")
            .textContent = "";

          renderAll();
        } else {
          document.getElementById("loginMessage")
            .textContent = "Sandi salah. Silakan coba lagi.";

          document.getElementById("adminPassword").value = "";
        }
      });

    function adminLogout() {
      isAdmin = false;

      document.getElementById("adminContent")
        .classList.add("hidden");

      document.getElementById("loginArea")
        .classList.remove("hidden");

      document.getElementById("adminPassword").value = "";
      hideAdmin();
    }

    // ===== 15. TAMBAH DAN EDIT MENU =====

    document.getElementById("menuForm")
      .addEventListener("submit", async function(event) {
        event.preventDefault();

        if (!isAdmin) {
          alert("Silakan login sebagai admin.");
          return;
        }

        const editId = document.getElementById("editId").value;
        const file = document.getElementById("menuImage").files[0];

        // Batasi ukuran foto demo supaya localStorage
        // tidak cepat penuh.
        if (file && file.size > 700 * 1024) {
          alert("Ukuran foto maksimal 700 KB untuk demo ini.");
          return;
        }

        let image = "";

        const existing = editId
          ? menu.find(item => item.id === Number(editId))
          : null;

        if (existing) image = existing.image || "";

        if (file) {
          if (!["image/jpeg", "image/png", "image/webp"].includes(file.type)) {
            alert("Gunakan gambar JPG, PNG, atau WEBP.");
            return;
          }

          try {
            image = await new Promise((resolve, reject) => {
              const reader = new FileReader();
              reader.onload = () => resolve(reader.result);
              reader.onerror = reject;
              reader.readAsDataURL(file);
            });
          } catch (error) {
            alert("Gambar gagal dibaca.");
            return;
          }
        }

        const name = document.getElementById("menuName").value.trim();
        const description =
          document.getElementById("menuDescription").value.trim();
        const price = Number(document.getElementById("menuPrice").value);
        const stock = Number(document.getElementById("menuStock").value);

        if (!name || price < 0 || stock < 0 ||
            !Number.isFinite(price) || !Number.isInteger(stock)) {
          alert("Periksa nama, harga, dan stok.");
          return;
        }

        if (existing) {
          existing.name = name;
          existing.description = description;
          existing.price = price;
          existing.stock = stock;
          existing.image = image;
        } else {
          menu.push({
            id: Date.now(),
            name,
            description,
            price,
            stock,
            image,
            emoji: "🍽️"
          });
        }

        if (!saveData()) return;

        resetMenuForm();
        renderAll();
        alert("Menu berhasil disimpan.");
      });

    function editMenu(id) {
      if (!isAdmin) return;

      const item = menu.find(product => product.id === id);
      if (!item) return;

      document.getElementById("editId").value = item.id;
      document.getElementById("menuName").value = item.name;
      document.getElementById("menuDescription").value =
        item.description || "";
      document.getElementById("menuPrice").value = item.price;
      document.getElementById("menuStock").value = item.stock;
      document.getElementById("menuImage").value = "";

      document.getElementById("menuForm")
        .scrollIntoView({ behavior: "smooth" });
    }

    function resetMenuForm() {
      document.getElementById("menuForm").reset();
      document.getElementById("editId").value = "";
    }

    function deleteMenu(id) {
      if (!isAdmin) return;

      const item = menu.find(product => product.id === id);
      if (!item) return;

      if (!confirm("Hapus menu " + item.name + "?")) return;

      menu = menu.filter(product => product.id !== id);
      delete cart[id];

      if (saveData()) {
        renderAll();
        resetMenuForm();
      }
    }

    // ===== 16. DAFTAR MENU ADMIN =====

    function renderAdminMenu() {
      if (!isAdmin) return;

      const container = document.getElementById("adminMenuList");

      if (menu.length === 0) {
        container.textContent = "Belum ada menu.";
        return;
      }

      container.innerHTML = menu.map(item => `
        <div class="admin-item">
          <strong>${escapeHTML(item.name)}</strong>
          <p>Harga: ${rupiah(item.price)}</p>
          <p>Stok: ${item.stock}</p>

          <button class="button button-light"
                  onclick="editMenu(${item.id})">
            Edit
          </button>

          <button class="button button-red"
                  onclick="deleteMenu(${item.id})">
            Hapus
          </button>
        </div>
      `).join("");
    }

    // ===== 17. KELOLA STATUS PESANAN =====

    function renderAdminOrders() {
      if (!isAdmin) return;

      const container = document.getElementById("adminOrders");

      if (orders.length === 0) {
        container.textContent = "Belum ada pesanan.";
        return;
      }

      const statuses = [
        "Menunggu",
        "Diproses",
        "Siap Diambil",
        "Selesai",
        "Dibatalkan"
      ];

      container.innerHTML = orders.map(order => `
        <div class="order-item">
          <strong>
            Antrean ${String(order.number).padStart(3, "0")}
          </strong>

          <p>Nama: ${escapeHTML(order.customer)}</p>
          <p>HP: ${escapeHTML(order.phone)}</p>
          <p>Total: ${rupiah(order.total)}</p>
          <p>Waktu: ${escapeHTML(order.time)}</p>

          <label>Status pesanan</label>
          <select onchange="changeStatus(${order.number}, this.value)">
            ${statuses.map(status => `
              <option value="${status}"
                ${order.status === status ? "selected" : ""}>
                ${status}
              </option>
            `).join("")}
          </select>
        </div>
      `).join("");
    }

    function changeStatus(number, status) {
      if (!isAdmin) return;

      const order = orders.find(item => item.number === number);
      if (!order) return;

      order.status = status;

      if (saveData()) {
        renderQueue();
        renderAdminOrders();
      }
    }

    // ===== 18. MEMPERBARUI TAMPILAN =====

    function renderAll() {
      renderMenu();
      renderCart();
      renderQueue();

      if (isAdmin) {
        renderAdminMenu();
        renderAdminOrders();
      }
    }

    // ===== 19. JALANKAN WEBSITE =====

    renderAll();
  </script>

</body>
</html>
