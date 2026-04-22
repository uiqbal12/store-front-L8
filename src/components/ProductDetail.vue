<template>
  <div class="product-detail-page">
    <div class="breadcrumb">
      <router-link to="/">Products</router-link> &rsaquo; {{ product?.name }}
    </div>

    <div class="product-detail" v-if="productExists">
      <div class="product-image">
        <img :src="product.image" :alt="product.name" />
      </div>
      <div class="product-info">
        <h1>{{ product.name }}</h1>
        <small class="product-id">SKU: {{ product.id }}</small>

        <div class="rating">
          <span class="stars">★★★★☆</span>
          <span class="review-count">(128 reviews)</span>
        </div>

        <p class="product-desc">{{ product.description }}</p>

        <div class="price-box">
          <span class="price-label">Our Price</span>
          <span class="price-value">${{ Number(product.price).toFixed(2) }}</span>
        </div>

        <div class="product-controls">
          <input type="number" v-model="quantity" min="1" class="quantity-input" />
          <button class="add-to-cart-btn" @click="addToCart">Add to Cart</button>
        </div>

        <div class="product-badges">
          <span class="badge">✓ Free Shipping</span>
          <span class="badge">✓ Free Returns</span>
          <span class="badge">✓ Price Match Guarantee</span>
        </div>
      </div>
    </div>

    <div class="not-found" v-else>
      <div class="not-found-content">
        <h2>Product Not Found</h2>
        <p>Sorry, we couldn't find that product.</p>
        <router-link to="/" class="back-link">← Back to Products</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductDetail',
  props: ['products'],
  data() {
    return {
      quantity: 1
    }
  },
  computed: {
    product() {
      return this.products.find(product => product.id == this.$route.params.id)
    },
    productExists() {
      return !!this.product
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
.product-detail-page {
  max-width: 1100px;
  margin: 0 auto;
  padding: 1.5rem 2rem;
}

.breadcrumb {
  font-size: 0.85rem;
  color: #666;
  margin-bottom: 1.5rem;
}

.breadcrumb a {
  color: #003DA5;
  text-decoration: none;
}

.breadcrumb a:hover {
  text-decoration: underline;
}

.product-detail {
  display: flex;
  gap: 3rem;
  background: #fff;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  padding: 2rem;
}

.product-image {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #F0F0F0;
  border-radius: 4px;
  padding: 1.5rem;
  max-height: 420px;
}

.product-image img {
  max-width: 100%;
  max-height: 380px;
  object-fit: contain;
}

.product-info {
  flex: 1;
  text-align: left;
}

.product-info h1 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.4rem;
  color: #1B1B1B;
}

.product-id {
  color: #888;
  font-size: 0.8rem;
  display: block;
  margin-bottom: 0.75rem;
}

.rating {
  margin-bottom: 1rem;
}

.stars {
  color: #FFE000;
  font-size: 1.1rem;
  letter-spacing: 2px;
}

.review-count {
  color: #003DA5;
  font-size: 0.85rem;
  margin-left: 6px;
}

.product-desc {
  font-size: 0.95rem;
  color: #444;
  line-height: 1.6;
  margin-bottom: 1.5rem;
  border-bottom: 1px solid #F0F0F0;
  padding-bottom: 1.5rem;
}

.price-box {
  margin-bottom: 1.25rem;
}

.price-label {
  display: block;
  font-size: 0.8rem;
  color: #666;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-bottom: 2px;
}

.price-value {
  font-size: 2rem;
  font-weight: 800;
  color: #1B1B1B;
}

.product-controls {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 1.25rem;
}

.quantity-input {
  width: 60px;
  height: 46px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 5px 10px;
  font-size: 1rem;
  text-align: center;
}

.add-to-cart-btn {
  flex: 1;
  height: 46px;
  background-color: #FFE000;
  color: #1B1B1B;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 700;
  font-size: 1rem;
  transition: background-color 0.2s;
}

.add-to-cart-btn:hover {
  background-color: #e6ca00;
}

.product-badges {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.badge {
  background-color: #F0F7FF;
  color: #003DA5;
  border: 1px solid #C8DCFF;
  border-radius: 4px;
  padding: 4px 10px;
  font-size: 0.78rem;
  font-weight: 600;
}

/* ── Not Found ───────────────────────────────────────────────────────────── */
.not-found {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 4rem;
  background: #fff;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
}

.not-found-content {
  text-align: center;
}

.not-found-content h2 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.not-found-content p {
  color: #666;
  margin-bottom: 1.5rem;
}

.back-link {
  color: #003DA5;
  font-weight: 600;
  text-decoration: none;
}

.back-link:hover {
  text-decoration: underline;
}

@media (max-width: 768px) {
  .product-detail {
    flex-direction: column;
  }
}
</style>