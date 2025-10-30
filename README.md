# Catalogo-de-video
// src/features/Cart/cartStore.js

import { create } from 'zustand';

const useCartStore = create((set, get) => ({
    // Estado Inicial
    items: JSON.parse(localStorage.getItem('cartItems')) || [],
    
    // Ações (métodos para modificar o estado)
    addItem: (product, quantity = 1) => {
        set((state) => {
            const existingItem = state.items.find(item => item.id === product.id);
            let newItems;

            if (existingItem) {
                // Lógica complexa: Atualiza quantidade se já existe
                newItems = state.items.map(item =>
                    item.id === product.id 
                        ? { ...item, quantity: item.quantity + quantity } 
                        : item
                );
            } else {
                // Adiciona novo item com validações
                newItems = [...state.items, { ...product, quantity }];
            }
            
            // Persistência complexa: Salva no localStorage sempre que muda
            localStorage.setItem('cartItems', JSON.stringify(newItems));
            return { items: newItems };
        });
    },

    updateQuantity: (productId, newQuantity) => {
        set((state) => {
            if (newQuantity <= 0) {
                // Lógica de remoção se a quantidade for zero ou menos
                const filteredItems = state.items.filter(item => item.id !== productId);
                localStorage.setItem('cartItems', JSON.stringify(filteredItems));
                return { items: filteredItems };
            }
            
            // Atualiza a quantidade
            const updatedItems = state.items.map(item =>
                item.id === productId ? { ...item, quantity: newQuantity } : item
            );
            localStorage.setItem('cartItems', JSON.stringify(updatedItems));
            return { items: updatedItems };
        });
    },
    
    // ... Outras ações: removeItem, clearCart
}));

export default useCartStore;