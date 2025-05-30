import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { toast } from "sonner";

const products = [ { id: 1, name: "AirPods Pro 2", price: 89.99, image: "/images/airpods_pro_2.jpg" }, { id: 2, name: "AirPods Gen 3", price: 89.99, image: "/images/airpods_gen3.jpg" }, { id: 3, name: "AirPods Gen 4", price: 89.99, image: "/images/airpods_gen4.jpg" }, { id: 4, name: "Yeezy (svi modeli i brojevi)", price: 110.0, image: "/images/yeezy.jpg" }, { id: 5, name: "JBL Xtreme 3", price: 120.0, image: "/images/jbl_xtreme3.jpg" }, { id: 6, name: "Polo Ralph Lauren Majice (sve boje i veličine)", price: 55.0, image: "/images/polo_majice.jpg" }, ];

export default function RoyalOutfitStore() { const [cart, setCart] = useState([]); const [customer, setCustomer] = useState("");

const addToCart = (product) => { const existing = cart.find((item) => item.id === product.id); if (existing) { setCart( cart.map((item) => item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item ) ); } else { setCart([...cart, { ...product, quantity: 1 }]); } };

const removeFromCart = (productId) => { setCart(cart.filter((item) => item.id !== productId)); };

const handleOrder = () => { if (!customer) { toast.error("Unesite ime ili kontakt!"); return; } const orderDetails = cart .map((item) => ${item.name} x${item.quantity} - €${(item.price * item.quantity).toFixed(2)}) .join("\n"); console.log(Nova narudžba od: ${customer}\n${orderDetails}); toast.success("Narudžba poslana!"); setCart([]); setCustomer(""); };

return ( <div className="p-6 max-w-5xl mx-auto"> <h1 className="text-4xl font-bold mb-6 text-center">Royal-Outfit Trgovina</h1> <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mb-8"> {products.map((product) => ( <Card key={product.id}> <CardContent className="p-4"> <img src={product.image} alt={product.name} className="w-full h-48 object-cover rounded-xl mb-2" /> <h2 className="text-xl font-semibold">{product.name}</h2> <p className="text-gray-700">Cijena: €{product.price.toFixed(2)}</p> <Button className="mt-2" onClick={() => addToCart(product)}>Dodaj u košaricu</Button> </CardContent> </Card> ))} </div>

<div className="bg-gray-100 p-4 rounded-xl">
    <h2 className="text-2xl font-bold mb-4">Košarica</h2>
    {cart.length === 0 ? (
      <p>Košarica je prazna.</p>
    ) : (
      <ul className="mb-4">
        {cart.map((item) => (
          <li key={item.id} className="flex justify-between mb-2 items-center">
            <span>{item.name} x{item.quantity} - €{(item.price * item.quantity).toFixed(2)}</span>
            <Button variant="outline" onClick={() => removeFromCart(item.id)}>Ukloni</Button>
          </li>
        ))}
      </ul>
    )}
    <Input
      placeholder="Vaše ime ili kontakt"
      value={customer}
      onChange={(e) => setCustomer(e.target.value)}
      className="mb-2"
    />
    <Button onClick={handleOrder} disabled={cart.length === 0}>Pošalji narudžbu</Button>
  </div>
</div>

); }

