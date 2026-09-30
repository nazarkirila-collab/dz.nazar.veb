<template>
  <div class="cart-container">
    <h2>Кошик</h2>
    
    <div v-if="cartItems.length === 0" class="empty-cart">
      Кошик порожній
    </div>
    
    <div v-else>
      <ul class="cart-list">
        <li v-for="item in cartItems" :key="item.cartId" class="cart-item">
          <div class="item-info">
            <strong>{{ item.product.name }}</strong> — {{ item.product.price }} грн
            <br>
            <span class="time-stamp">Додано: {{ item.addedAt }}</span>
          </div>
          <button @click="$emit('remove-item', item.cartId)" class="btn-remove">
            Видалити
          </button>
        </li>
      </ul>
      
      <div class="cart-summary">
        <p>Всього товарів: <strong>{{ totalCount }}</strong></p>
        <p>Загальна сума: <strong>{{ totalPrice }} грн</strong></p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Product } from './ProductCard.vue'

export interface CartItem {
  cartId: string;
  product: Product;
  addedAt: string;
}

defineProps<{
  cartItems: CartItem[];
  totalCount: number;
  totalPrice: number;
}>();

defineEmits<{
  (e: 'remove-item', cartId: string): void
}>();
</script>

<style scoped>
.cart-container {
  margin-top: 2rem;
  padding: 1.5rem;
  background: #ecf0f1;
  border-radius: 8px;
}
.cart-list {
  list-style: none;
  padding: 0;
}
.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 4px;
}
.time-stamp {
  font-size: 0.85rem;
  color: #7f8c8d;
}
.btn-remove {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}
.btn-remove:hover {
  background: #c0392b;
}
.cart-summary {
  margin-top: 1rem;
  border-top: 2px solid #bdc3c7;
  padding-top: 1rem;
  font-size: 1.1rem;
}
</style>