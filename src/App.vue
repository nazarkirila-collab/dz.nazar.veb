<template>
  <main class="app-wrapper">
    <h1>Міні-каталог товарів</h1>
    <p class="stock-counter">Доступно товарів на складі: {{ availableProductsCount }}</p>

    <div class="products-grid">
      <ProductCard 
        v-for="product in products" 
        :key="product.id" 
        :product="product"
        @add-to-cart="handleAddToCart"
      />
    </div>

    <Cart 
      :cart-items="cart"
      :total-count="cartTotalCount"
      :total-price="cartTotalPrice"
      @remove-item="handleRemoveFromCart"
    />
  </main>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import ProductCard, { type Product } from './components/ProductCard.vue'
import Cart, { type CartItem } from './components/Cart.vue'

const products = ref<Product[]>([
  { id: 1, name: 'Ноутбук ASUS', price: 32000, description: 'Ігровий ноутбук 16GB RAM', inStock: true },
  { id: 2, name: 'Миша Logitech', price: 1500, description: 'Бездротова оптична миша', inStock: true },
  { id: 3, name: 'Клавіатура Keychron', price: 4200, description: 'Механічна клавіатура', inStock: false },
  { id: 4, name: 'Монітор Dell', price: 9500, description: '27" 4K IPS монітор', inStock: true }
]);

const cart = ref<CartItem[]>([]);

const availableProductsCount = computed(() => {
  return products.value.filter(p => p.inStock).length;
});

const cartTotalCount = computed(() => cart.value.length);

const cartTotalPrice = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.product.price, 0);
});

const handleAddToCart = (product: Product) => {
  const now = new Date();
  const timeString = now.toLocaleTimeString('uk-UA', { 
    hour: '2-digit', minute: '2-digit', second: '2-digit' 
  });

  cart.value.push({
    cartId: crypto.randomUUID(), 
    product: product,
    addedAt: timeString
  });
};

const handleRemoveFromCart = (cartId: string) => {
  cart.value = cart.value.filter(item => item.cartId !== cartId);
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f5f6fa;
  color: #2c3e50;
  margin: 0;
  padding: 20px;
}
.app-wrapper {
  max-width: 900px;
  margin: 0 auto;
}
.stock-counter {
  font-size: 1.1rem;
  font-weight: bold;
  color: #34495e;
  margin-bottom: 1.5rem;
}
.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
</style>