import React, { useState, useEffect } from 'react';
import { createClient } from '@supabase/supabase-js';
import { motion, AnimatePresence } from 'framer-motion';
import { ShoppingBag, User, Home, Plus, X, Sparkles, Gift, Flame, Send } from 'lucide-react';

// 1. KONFIGURASI SUPABASE (Menggunakan Kredensial Anda)
const supabase = createClient(
  'https://jjgwpnyymwutowhlpsjh.supabase.co',
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpqZ3dwbnl5bXd1dG93aGxwc2poIiwicm9sZSI6ImFub24iLCJpYXQiOjE3Nzc3MzUyNjgsImV4cCI6MjA5MzMxMTI2OH0.e-Sj0e2bcVqVcK4ua6sOIjtCXlvHBRwswbdrt9Tdgoo'
);

const KeikeuApp = () => {
  const [products, setProducts] = useState([]);
  const [cart, setCart] = useState([]);
  const [loading, setLoading] = useState(true);
  const [showPopUp, setShowPopUp] = useState(true);
  const [activeTab, setActiveTab] = useState('home');

  // 2. FETCH DATA PRODUK LIVE DARI BACKEND
  useEffect(() => {
    const fetchProducts = async () => {
      const { data } = await supabase.from('products').select('*');
      setProducts(data || []);
      setLoading(false);
    };
    fetchProducts();
  }, []);

  // 3. LOGIKA SMART BAG (Otomatis Tambah Tas jika > 9 Pcs)
  const addToCart = (product) => {
    setCart(prev => {
      const newCart = [...prev, product];
      const mainItemsCount = newCart.filter(item => item.category === 'main').length;
      const bagNeeded = Math.ceil(mainItemsCount / 9);
      const currentBags = newCart.filter(item => item.isBag).length;

      if (bagNeeded > currentBags) {
        return [...newCart, { id: 'bag-auto', name: 'Tas Belanja Tambahan', price: 5000, isBag: true }];
      }
      return newCart;
    });
  };

  // 4. SMART WHATSAPP LINK
  const handleCheckout = () => {
    const items = cart.map(i => `- ${i.name}`).join('\n');
    const total = cart.reduce((sum, i) => sum + i.price, 0);
    const waUrl = `https://wa.me/628123456789?text=Halo Keikeu! Saya mau pesan:\n${items}\n\nTotal: Rp${total.toLocaleString()}`;
    window.open(waUrl, '_blank');
  };

  if (loading) return <SkeletonLoader />;

  return (
    <div className="min-h-screen bg-[#FDFBF7] text-[#445588] font-sans pb-32 overflow-x-hidden">
      {/* POP-UP SIGN UP ESTETIK */}
      <AnimatePresence>
        {showPopUp && <SignUpModal onClose={() => setShowPopUp(false)} />}
      </AnimatePresence>

      {/* HEADER & LOYALTY STATUS */}
      <header className="fixed top-0 w-full bg-white/80 backdrop-blur-md z-40 px-6 py-4 flex justify-between items-center border-b border-[#445588]/10">
        <h1 className="text-2xl font-black tracking-tighter italic">KEIKEU.</h1>
        <div className="bg-[#445588]/5 px-3 py-1 rounded-full flex items-center gap-2">
          <Gift size={14} className="text-[#F472B6]" />
          <span className="text-xs font-bold">12 Poin</span>
        </div>
      </header>

      {/* HERO SECTION */}
      <section className="pt-24 px-6 mb-8">
        <motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} className="bg-[#445588] rounded-[32px] p-8 text-white relative h-64 flex flex-col justify-end shadow-xl shadow-[#445588]/20">
          <div className="relative z-10">
            <span className="bg-white/20 backdrop-blur-md px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-widest">Premium Dessert</span>
            <h2 className="text-4xl font-black mt-2 leading-tight">Frozen <br/> Cheesecake.</h2>
          </div>
          <div className="absolute top-[-20%] right-[-10%] w-64 h-64 bg-white/5 rounded-full blur-3xl" />
        </motion.div>
      </section>

      {/* CATALOG LIST */}
      <main className="px-6 space-y-6">
        <h3 className="text-xl font-bold">Menu Pilihan</h3>
        <div className="grid grid-cols-1 gap-6">
          {products.map(product => (
            <ProductCard key={product.id} product={product} onAdd={addToCart} />
          ))}
        </div>
      </main>

      {/* BOTTOM NAVIGATION (APP-LIKE) */}
      <nav className="fixed bottom-6 left-1/2 -translate-x-1/2 w-[92%] bg-[#445588] rounded-[28px] p-4 shadow-2xl flex justify-around items-center z-50">
        <button onClick={() => setActiveTab('home')} className={activeTab === 'home' ? "text-white" : "text-white/40"}><Home size={24} /></button>
        <button onClick={handleCheckout} className="bg-white/20 p-3 rounded-2xl text-white relative">
          <ShoppingBag size={24} />
          {cart.length > 0 && <span className="absolute -top-1 -right-1 bg-[#F472B6] text-[10px] px-1.5 py-0.5 rounded-full font-bold">{cart.length}</span>}
        </button>
        <button onClick={() => setActiveTab('profile')} className={activeTab === 'profile' ? "text-white" : "text-white/40"}><User size={24} /></button>
      </nav>
    </div>
  );
};

// SUB-KOMPONEN CARD PRODUK
const ProductCard = ({ product, onAdd }) => (
  <div className="bg-white rounded-[32px] p-2 border border-[#445588]/5 shadow-sm">
    <div className="relative h-56 rounded-[26px] bg-[#E9D5C3]/20 overflow-hidden">
      <img src={product.image_url} alt={product.name} className="w-full h-full object-cover" />
      {product.stock <= 5 && (
        <motion.div animate={{ opacity: [0.5, 1, 0.5] }} transition={{ repeat: Infinity, duration: 2 }} className="absolute top-4 left-4 bg-white/90 px-3 py-1 rounded-full flex items-center gap-1 shadow-sm">
          <Flame size={12} className="text-[#D97706] fill-[#D97706]" />
          <span className="text-[10px] font-black text-[#D97706]">SISA {product.stock}!</span>
        </motion.div>
      )}
    </div>
    <div className="p-4 flex justify-between items-end">
      <div>
        <h4 className="text-[10px] font-bold text-[#A3B1CC] uppercase tracking-widest">Frozen Cheesecake</h4>
        <h3 className="text-lg font-bold text-[#445588]">{product.name}</h3>
        <p className="text-xl font-black text-[#445588]">Rp {product.price.toLocaleString()}</p>
      </div>
      <motion.button whileTap={{ scale: 0.9 }} onClick={() => onAdd(product)} className="bg-[#445588] text-white p-4 rounded-2xl shadow-lg">
        <Plus size={24} />
      </motion.button>
    </div>
  </div>
);

// SUB-KOMPONEN MODAL SIGN UP
const SignUpModal = ({ onClose }) => (
  <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }} className="fixed inset-0 bg-[#445588]/40 backdrop-blur-sm z-[100] flex items-center justify-center p-6">
    <motion.div initial={{ scale: 0.9, y: 20 }} animate={{ scale: 1, y: 0 }} className="bg-white rounded-[40px] p-8 w-full max-w-sm relative text-center shadow-2xl">
      <div className="w-20 h-20 bg-[#FDFBF7] rounded-3xl mx-auto mb-6 flex items-center justify-center">
        <Sparkles size={40} className="text-[#F472B6]" />
      </div>
      <h3 className="text-2xl font-black text-[#445588]">Halo David!</h3>
      <p className="text-[#A3B1CC] text-sm mt-4">Masuk untuk kumpulkan poin dan nikmati Frozen Cheesecake favoritmu.</p>
      <button onClick={onClose} className="w-full bg-[#445588] text-white py-4 rounded-2xl font-bold mt-8 shadow-lg">Mulai Menjelajah</button>
    </motion.div>
  </motion.div>
);

const SkeletonLoader = () => <div className="p-8 space-y-6 animate-pulse"><div className="h-64 w-full bg-gray-200 rounded-[32px]" /><div className="h-64 w-full bg-gray-200 rounded-[32px]" /></div>;

export default KeikeuApp;
