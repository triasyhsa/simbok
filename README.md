import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { ShoppingCart, Phone, Store, Trash2 } from 'lucide-react';

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

export default function GandhokSimbok() {
  const [cart, setCart] = useState([]);
  const [paymentMethod, setPaymentMethod] = useState('Cash');

  const addToCart = (item) => {
    const existingIndex = cart.findIndex((cartItem) => cartItem.name === item.name);
    if (existingIndex !== -1) {
      const updatedCart = [...cart];
      updatedCart[existingIndex].quantity += 1;
      setCart(updatedCart);
    } else {
      setCart([...cart, { ...item, quantity: 1 }]);
    }
  };

  const removeFromCart = (index) => {
    const updatedCart = [...cart];
    if (updatedCart[index].quantity > 1) {
      updatedCart[index].quantity -= 1;
    } else {
      updatedCart.splice(index, 1);
    }
    setCart(updatedCart);
  };

  const totalPrice = () => {
    return cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  };

  const handleCheckout = (method) => {
    const orderText = cart.map(
      (item, index) =>
        `${index + 1}. ${item.name} (${item.quantity}x) - Rp${(item.price * item.quantity).toLocaleString()}`
    ).join('%0A');

    const paymentText = `%0AMetode%20pembayaran%3A%20${paymentMethod}`;

    if (method === 'whatsapp') {
      window.open(
        `https://wa.me/08371253839?text=Halo%2C%20saya%20ingin%20pesan%3A%0A${orderText}%0ATotal%3A%20Rp${totalPrice().toLocaleString()}${paymentText}`
      );
    } else if (method === 'gofood') {
      window.open('https://gofood.link/a/PQXft4o');
    } else if (method === 'shopee') {
      window.open('https://shopee.co.id'); // Ganti dengan link Shopee toko kamu
    }
  };

  return (
    <div className="p-6 bg-red-700 min-h-screen text-white">
      <h1 className="text-4xl font-bold mb-2">Gandhok Simbok</h1>
      <p className="mb-6 italic text-lg">Rasanya Nampol, Pedesnya Nagih!</p>

      <div className="grid gap-6 md:grid-cols-3">
        {menuData.map((menu) => (
          <Card key={menu.name} className="bg-white text-black rounded-2xl shadow-lg">
            <CardContent className="p-4">
              <h2 className="text-xl font-bold mb-2">{menu.name}</h2>
              <ul className="mb-4 space-y-1">
                {menu.variants.map((variant) => (
                  <li key={variant.name} className="flex justify-between items-center">
                    <span>{variant.name} - Rp{variant.price.toLocaleString()}</span>
                    <Button
                      variant="outline"
                      onClick={() =>
                        addToCart({ name: `${menu.name} - ${variant.name}`, price: variant.price })
                      }
                    >
                      Tambah
                    </Button>
                  </li>
                ))}
              </ul>
            </CardContent>
          </Card>
        ))}
      </div>

      <div className="mt-10 bg-white text-black p-4 rounded-2xl shadow-lg">
        <h3 className="text-xl font-semibold mb-2 flex items-center gap-2">
          <ShoppingCart className="text-red-700" /> Keranjang Belanja
        </h3>
        {cart.length === 0 ? (
          <p className="text-gray-600">Keranjang masih kosong</p>
        ) : (
          <>
            <ul className="list-disc list-inside space-y-2">
              {cart.map((item, index) => (
                <li key={index} className="flex justify-between items-center">
                  <span>{item.name} ({item.quantity}x) - Rp{(item.price * item.quantity).toLocaleString()}</span>
                  <Button size="icon" variant="ghost" onClick={() => removeFromCart(index)}>
                    <Trash2 size={18} className="text-red-600" />
                  </Button>
                </li>
              ))}
            </ul>
            <p className="mt-2 font-bold">Total: Rp{totalPrice().toLocaleString()}</p>

            <div className="mt-4">
              <label className="font-semibold mr-2">Metode Pembayaran:</label>
              <select
                value={paymentMethod}
                onChange={(e) => setPaymentMethod(e.target.value)}
                className="border px-2 py-1 rounded"
              >
                <option value="Cash">Cash</option>
                <option value="Transfer">Transfer</option>
              </select>
            </div>
          </>
        )}

        {cart.length > 0 && (
          <div className="mt-4 space-x-4 flex flex-wrap gap-2">
            <Button onClick={() => handleCheckout('whatsapp')} className="bg-green-600 text-white">
              <Phone className="inline-block mr-2" size={18} /> Pesan via WhatsApp
            </Button>
            <Button onClick={() => handleCheckout('gofood')} className="bg-yellow-500 text-black">
              <Store className="inline-block mr-2" size={18} /> Pesan via GoFood
            </Button>
            <Button onClick={() => handleCheckout('shopee')} className="bg-orange-500 text-white">
              🛒 Checkout via Shopee
            </Button>
          </div>
        )}
      </div>
    </div>
  );
}
