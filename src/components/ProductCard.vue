<template>
  <div class="product-card" :class="{ 'out-of-stock': !product.inStock }">
    <h3>{{ product.name }}</h3>
    <p class="desc">{{ product.description }}</p>
    <p class="price">{{ product.price }} грн</p>
    
    <p v-if="!product.inStock" class="status-empty">Немає в наявності</p>
    <p v-else class="status-ok">В наявності</p>

    <button 
      :disabled="!product.inStock" 
      @click="$emit('add-to-cart', product)"
      :class="{ 'btn-disabled': !product.inStock }"
    >
      Додати до кошика
    </button>
  </div>
</template>

<script setup lang="ts">
export interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  inStock: boolean;
}

defineProps<{
  product: Product
}>();

defineEmits<{
  (e: 'add-to-cart', product: Product): void
}>();
</script>

<style scoped>
.product-card {
  border: 1px solid #ddd;
  padding: 1rem;
  border-radius: 8px;
  background: white;
  transition: 0.3s;
}
.out-of-stock {
  opacity: 0.7;
  background: #f9f9f9;
}
.price {
  font-weight: bold;
  font-size: 1.2rem;
  color: #2c3e50;
}
.status-empty {
  color: #e74c3c;
  font-weight: bold;
}
.status-ok {
  color: #27ae60;
}
button {
  width: 100%;
  padding: 10px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover:not(:disabled) {
  background: #2980b9;
}
.btn-disabled {
  background: #95a5a6;
  cursor: not-allowed;
}
</style>