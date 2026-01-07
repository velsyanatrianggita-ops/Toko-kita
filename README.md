<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TokoKita - E Commerce</title><style>
    body { font-family: Arial, sans-serif; margin: 0; background: linear-gradient(135deg,#ffb3ec,#d6b3ff); }
    header {
        background: linear-gradient(90deg,#ff5fd8,#9b4dff); padding: 15px; color: white;
        display: flex; justify-content: space-between; align-items: center;
        box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    }
    header h1 { margin: 0; }
    nav a {
        color: white; text-decoration: none; margin: 0 10px; font-weight: bold;
    }
    nav a:hover { text-decoration: underline; }

    .container { padding: 20px; }
    .hidden { display: none; }

    /* Produk */
    .grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
        gap: 15px;
    }
    .card {
        background: white; padding: 10px; border-radius: 12px;
        box-shadow: 0 4px 8px rgba(0,0,0,0.15); text-align: center;
        transition: transform .3s, box-shadow .3s;
        animation: fadeIn .8s ease;
    }
    .card:hover {
        transform: translateY(-6px);
        box-shadow: 0 8px 15px rgba(0,0,0,0.2);
    }
    @keyframes fadeIn {
        from { opacity: 0; transform: scale(.9); }
        to { opacity: 1; transform: scale(1); }
    }

    .card img {
        width: 100%; height: 140px; object-fit: cover; border-radius: 10px;
    }
    .btn {
        background: #c44bff; color: white; border: none;
        padding: 8px 12px; border-radius: 6px; margin-top: 8px; cursor: pointer;
        transition: .3s;
    }
    .btn:hover {
        background: #9d2fff;
    }

    /* Box keranjang */
    .cart-box {
        background: white; padding: 15px; border-radius: 10px;
        margin-top: 25px; box-shadow: 0 4px 8px rgba(0,0,0,0.15);
        animation: fadeIn 1s;
    }
    .cart-item { border-bottom: 1px solid #ddd; padding: 5px 0; }

    /* Section lain */
    section { background: white; padding: 20px; border-radius: 10px;
             box-shadow: 0 4px 8px rgba(0,0,0,0.15); animation: fadeIn 1s; }

    /* Login form */
    input {
        width: 100%; padding: 10px; margin-bottom: 10px;
        border-radius: 6px; border: 1px solid #aaa;
    }
</style></head>
<body><header>
    <h1>TokoKita</h1>
    <nav>
        <a href="#" onclick="showPage('home')">Home</a>
        <a href="#" onclick="showPage('about')">About</a>
        <a href="#" onclick="showPage('contact')">Contact</a>
        <a href="#" onclick="showPage('login')">Login</a>
    </nav>
</header><div class="container"><!-- HOME (produk + keranjang) -->
<div id="home">
    <h2 style="color:#8117ff;">Produk Kami</h2>
    <div id="productList" class="grid"></div>

    <div class="cart-box">
        <h3 style="color:#c44bff;">Keranjang</h3>
        <div id="cart"></div>
        <p><b>Total: </b><span id="total">Rp 0</span></p>
        <button class="btn" onclick="checkout()">Checkout</button>
    </div>
</div>

<!-- ABOUT -->
<div id="about" class="hidden">
    <section>
        <h2 style="color:#8117ff;">Tentang TokoKita</h2>
        <p>
            TokoKita adalah website e-commerce modern dengan tema pink-ungu
            aesthetic, dibuat untuk tugas Uprak TIK.
        </p>
    </section>
</div>

<!-- CONTACT -->
<div id="contact" class="hidden">
    <section>
        <h2 style="color:#8117ff;">Contact</h2>
        <ul>
            <li>Email: <b>tokokita@gmail.com</b></li>
            <li>Instagram: <b>@tokokita.shop</b></li>
            <li>WhatsApp: <b>0812-3456-7890</b></li>
        </ul>
    </section>
</div>

<!-- LOGIN -->
<div id="login" class="hidden">
    <section>
        <h2 style="color:#8117ff;">Login</h2>
        <input type="text" placeholder="Username">
        <input type="password" placeholder="Password">
        <button class="btn" style="width:100%">Login</button>
    </section>
</div>

</div><script>
    // Sistem Halaman
    function showPage(page) {
        document.getElementById("home").classList.add("hidden");
        document.getElementById("about").classList.add("hidden");
        document.getElementById("contact").classList.add("hidden");
        document.getElementById("login").classList.add("hidden");
        document.getElementById(page).classList.remove("hidden");
    }

    // Produk tambahan + baru aesthetic pink-ungu
    const products = [
        {id:1, name:"Stiker Cute", price: 5000, img:"https://i.imgur.com/UQj5jYp.jpeg"},
        {id:2, name:"Kaos Thrift", price: 25000, img:"https://i.imgur.com/V7oQZ4S.jpeg"},
        {id:3, name:"Gelang Handmade", price: 10000, img:"https://i.imgur.com/gpQmSeJ.jpeg"},
        {id:4, name:"Notebook Aesthetic", price: 12000, img:"https://i.imgur.com/zsArS2T.jpeg"},
        {id:5, name:"Lip Gloss Pink", price: 15000, img:"https://i.imgur.com/nj6vZrw.jpeg"},
        {id:6, name:"Tas Mini Ungu", price: 30000, img:"https://i.imgur.com/fxPCspC.jpeg"},
        {id:7, name:"Scrunchie", price: 7000, img:"https://i.imgur.com/TZpUx8V.jpeg"}
    ];

    let cart = {};

    function renderProducts() {
        const list = document.getElementById("productList");
        list.innerHTML = "";
        products.forEach(p => {
            list.innerHTML += `
                <div class="card">
                    <img src="${p.img}">
                    <h4>${p.name}</h4>
                    <p>Rp ${p.price}</p>
                    <button class="btn" onclick="addToCart(${p.id})">Tambah</button>
                </div>
            `;
        });
    }

    function addToCart(id) {
        if (!cart[id]) cart[id] = 0;
        cart[id]++;
        renderCart();
    }

    function renderCart() {
        const cartDiv = document.getElementById("cart");
        const totalDiv = document.getElementById("total");
        cartDiv.innerHTML = "";
        let total = 0;

        for (let id in cart) {
            const p = products.find(x => x.id == id);
            const qty = cart[id];
            total += p.price * qty;

            cartDiv.innerHTML += `
                <div class="cart-item">
                    ${p.name} (${qty}) — Rp ${p.price * qty}
                </div>
            `;
        }
        totalDiv.innerText = "Rp " + total;
    }

    function checkout() {
        alert("Checkout Berhasil! (Simulasi)");
    }

    renderProducts();
</script></body>
</html>
