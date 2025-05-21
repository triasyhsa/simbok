<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Gandhok Simbok</title>
  <style>
    body {
      font-family: sans-serif;
      background-color: #b91c1c;
      color: white;
      padding: 20px;
    }
    .card {
      background: white;
      color: black;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 20px;
    }
    button {
      margin-left: 10px;
    }
    .keranjang {
      background: white;
      color: black;
      padding: 15px;
      border-radius: 10px;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>Gandhok Simbok</h1>
  <p><em>Rasanya Nampol, Pedesnya Nagih!</em></p>

  <div id="menu"></div>

  <div class="keranjang">
    <h2>Keranjang Belanja</h2>
    <ul id="cart-list"></ul>
    <p id="total">Total: Rp0</p>

    <label for="payment">Metode Pembayaran: </label>
    <select id="payment">
      <option value="Cash">Cash</option>
      <option value="Transfer">Transfer</option>
    </select>

    <div style="margin-top: 10px;">
      <button onclick="checkout()">Pesan via WhatsApp</button>
    </div>
  </div>

  <script>
    const menuData = [
      {
        name: 'Kremesan',
        variants: [
          { name: 'Sambal Original', price: 12000 },
          { name: 'Sambal Terasi', price: 13000 },
          { name: 'Sambal Bawang', price: 13000 },
          { name: 'Sambal Campur', price: 13500 },
          { name: 'Sambal Kacang', price: 12000 },
          { name: 'Sambal Tomat', price: 12500 }
        ]
      },
      {
        name: 'Krispi Celup',
        variants: [
          { name: 'Saos Judes', price: 14000 },
          { name: 'Saus Keju', price: 14500 },
          { name: 'Saus Kari', price: 14500 },
          { name: 'Saus Lada Hitam', price: 15000 },
          { name: 'Saus Original', price: 13500 }
        ]
      },
      {
        name: 'Mie Jebew',
        variants: [
          { name: 'Mie Original', price: 10000 },
          { name: 'Mie Ayam Bawang', price: 12000 },
          { name: 'Mie Lada Hitam', price: 13000 },
          { name: 'Mie Rendang', price: 13500 },
          { name: 'Mie Kari', price: 13000 }
        ]
      }
    ];

    const cart = [];

    function renderMenu() {
      const menuDiv = document.getElementById('menu');
      menuData.forEach(menu => {
        const card = document.createElement('div');
        card.className = 'card';

        const title = document.createElement('h3');
        title.textContent = menu.name;
        card.appendChild(title);

        menu.variants.forEach(variant => {
          const item = document.createElement('p');
          item.innerHTML = `${variant.name} - Rp${variant.price.toLocaleString()} 
            <button onclick="addToCart('${menu.name} - ${variant.name}', ${variant.price})">Tambah</button>`;
          card.appendChild(item);
        });

        menuDiv.appendChild(card);
      });
    }

    function addToCart(name, price) {
      const index = cart.findIndex(item => item.name === name);
      if (index !== -1) {
        cart[index].quantity++;
      } else {
        cart.push({ name, price, quantity: 1 });
      }
      renderCart();
    }

    function removeFromCart(index) {
      if (cart[index].quantity > 1) {
        cart[index].quantity--;
      } else {
        cart.splice(index, 1);
      }
      renderCart();
    }

    function renderCart() {
      const cartList = document.getElementById('cart-list');
      cartList.innerHTML = '';
      let total = 0;

      cart.forEach((item, index) => {
        const li = document.createElement('li');
        li.innerHTML = `${item.name} (${item.quantity}x) - Rp${(item.price * item.quantity).toLocaleString()} 
          <button onclick="removeFromCart(${index})">Hapus</button>`;
        cartList.appendChild(li);
        total += item.price * item.quantity;
      });

      document.getElementById('total').textContent = `Total: Rp${total.toLocaleString()}`;
    }

    function checkout() {
      if (cart.length === 0) {
        alert('Keranjang kosong!');
        return;
      }
      const payment = document.getElementById('payment').value;
      const pesan = cart.map(
        (item, i) => `${i + 1}. ${item.name} (${item.quantity}x) - Rp${(item.price * item.quantity).toLocaleString()}`
      ).join('%0A');

      const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
      const text = `Halo%2C%20saya%20ingin%20pesan%3A%0A${pesan}%0ATotal%3A%20Rp${total.toLocaleString()}%0AMetode%20pembayaran%3A%20${payment}`;
      window.open(`https://wa.me/08371253839?text=${text}`);
    }

    renderMenu();
  </script>
</body>
</html>
