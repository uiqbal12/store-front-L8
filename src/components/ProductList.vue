<template>
  <div class="product-list-page">
    <!-- Hero Banner -->
    <div class="hero-banner">
      <div class="hero-content">
        <h1>Today's Top Deals</h1>
        <p>Free shipping on orders over $35 · Price match guarantee</p>
      </div>
    </div>

    <!-- Category Header -->
    <div class="section-header">
      <h2>Featured Electronics</h2>
      <span class="product-count">{{ products.length }} products</span>
    </div>

    <!-- Product Grid -->
    <div class="product-list">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @addToCart="addToCart"
      />
    </div>
  </div>
</template>

<script>
import ProductCard from '../components/ProductCard.vue'

export default {
  name: 'ProductList',
  props: ['products'],
  components: {
    ProductCard
  },
  methods: {
    addToCart({ productId, quantity }) {
      this.$emit('addToCart', { productId, quantity })
    }
  }
}
</script>

<style scoped>
.product-list-page {
  max-width: 1400px;
  margin: 0 auto;
  padding-bottom: 2rem;
}

/* ── Hero Banner ─────────────────────────────────────────────────────────── */
.hero-banner {
  background: linear-gradient(135deg, #003DA5 0%, #0055CC 100%);
  color: white;
  padding: 3rem 2rem;
  margin-bottom: 2rem;
  position: relative;
  overflow: hidden;
}

.hero-banner::after {
  content: '';
  position: absolute;
  right: -50px;
  top: -50px;
  width: 300px;
  height: 300px;
  background: rgba(255, 224, 0, 0.1);
  border-radius: 50%;
}

.hero-content h1 {
  font-size: 2.2rem;
  font-weight: 900;
  margin-bottom: 0.5rem;
  position: relative;
  z-index: 1;
}

.hero-content h1 span {
  color: #FFE000;
}

.hero-content p {
  font-size: 1rem;
  opacity: 0.85;
  position: relative;
  z-index: 1;
}

/* ── Section Header ──────────────────────────────────────────────────────── */
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 2rem 1rem;
  border-bottom: 3px solid #FFE000;
  margin: 0 0 1.5rem;
}

.section-header h2 {
  font-size: 1.3rem;
  font-weight: 700;
  color: #1B1B1B;
}

.product-count {
  font-size: 0.85rem;
  color: #666;
}

/* ── Grid ────────────────────────────────────────────────────────────────── */
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 1.25rem;
  padding: 0 2rem;
}
</style>