<template>
  <div class="product-card">
    <div class="product-img-wrap">
      <img :src="product.image" alt="Product Image" />
    </div>
    <div class="product-body">
      <router-link :to="`/product/${product.id}`">
        <h2>{{ product.name }}</h2>
      </router-link>
      <p class="product-desc">{{ product.description }}</p>
    </div>
    <div class="product-footer">
      <div class="product-price">${{ Number(product.price).toFixed(2) }}</div>
      <div class="product-controls">
        <input type="number" v-model="quantity" min="1" class="quantity-input" />
        <button @click="addToCart">Add to Cart</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductCard',
  props: ['product'],
  data() {
    return {
      quantity: 1
    }
  },
  methods: {
    addToCart() {
      this.$emit('addToCart', {
        productId: this.product.id,
        quantity: this.quantity
      })
    }
  }
}
</script>

<style scoped>
.product-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 1.25rem;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  background-color: #fff;
  box-shadow: 0 1px 4px rgba(0,0,0,0.07);
  transition: box-shadow 0.2s, transform 0.2s;
  height: 100%;
}

.product-card:hover {
  box-shadow: 0 6px 20px rgba(0,0,0,0.13);
  transform: translateY(-2px);
}

.product-img-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 180px;
  margin-bottom: 1rem;
}

.product-img-wrap img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.product-body {
  flex: 1;
}

.product-body a {
  text-decoration: none;
  color: #003DA5;
}

.product-body a:hover {
  text-decoration: underline;
}

.product-body h2 {
  font-size: 0.95rem;
  font-weight: 600;
  margin-bottom: 0.4rem;
  line-height: 1.3;
}

.product-desc {
  font-size: 0.8rem;
  color: #666;
  line-height: 1.4;
  margin-bottom: 0.75rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.product-footer {
  border-top: 1px solid #F0F0F0;
  padding-top: 0.75rem;
}

.product-price {
  font-size: 1.4rem;
  font-weight: 800;
  color: #1B1B1B;
  margin-bottom: 0.6rem;
}

.product-controls {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.quantity-input {
  width: 52px;
  height: 38px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 5px 8px;
  font-size: 0.95rem;
  text-align: center;
}

button {
  flex: 1;
  height: 38px;
  background-color: #FFE000;
  color: #1B1B1B;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.85rem;
  transition: background-color 0.2s;
  padding: 0;
}

button:hover {
  background-color: #e6ca00;
}
</style>