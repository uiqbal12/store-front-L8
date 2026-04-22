<template>
  <TopNav :cartItemCount="cartItemCount"/>
  <router-view
    :products="products"
    :cartItems="cartItems"
    @addToCart="addToCart"
    @removeFromCart="removeFromCart"
    @submitOrder="submitOrder"
  ></router-view>
  <footer>
    <p>© 2024 Best Buy Canada Ltd. All Rights Reserved.</p>
  </footer>
</template>

<script>
import TopNav from './components/TopNav.vue'

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      cartItems: [],
      products: [],
    }
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    getProducts() {
      fetch('/products')
        .then(response => response.json())
        .then(products => {
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    addToCart({ productId, quantity }) {
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        existingCartItem.quantity += quantity
      } else {
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      fetch(`/order`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(order)
      })
        .then(response => {
          if (!response.ok) {
            alert('Error occurred while submitting order')
          } else {
            this.cartItems = []
            alert('Order submitted successfully!')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while submitting order')
        })
    }
  },
}
</script>

<style>
/* ── Best Buy Brand Colors ────────────────────────────────────────────────── */
:root {
  --bb-blue:   #003DA5;
  --bb-yellow: #FFE000;
  --bb-dark:   #1B1B1B;
  --bb-gray:   #F5F5F5;
  --bb-border: #E0E0E0;
  --bb-text:   #1B1B1B;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: var(--bb-gray);
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  color: var(--bb-text);
  margin: 0;
  padding: 0;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 80px;
  min-height: 100vh;
}

/* ── Footer ──────────────────────────────────────────────────────────────── */
footer {
  background-color: var(--bb-dark);
  color: #aaa;
  text-align: center;
  padding: 1.5rem;
  font-size: 0.85rem;
  margin-top: 4rem;
}

/* ── Buttons ─────────────────────────────────────────────────────────────── */
button {
  padding: 10px 18px;
  background-color: var(--bb-yellow);
  color: var(--bb-dark);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 700;
  font-size: 0.9rem;
  height: 42px;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #e6ca00;
}

/* ── Product Grid ─────────────────────────────────────────────────────────── */
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 1.25rem;
  padding: 1.5rem 2rem;
  max-width: 1400px;
  margin: 0 auto;
}

/* ── Product Cards ───────────────────────────────────────────────────────── */
.product-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 1.25rem;
  border: 1px solid var(--bb-border);
  border-radius: 4px;
  background-color: #fff;
  box-shadow: 0 1px 4px rgba(0,0,0,0.08);
  transition: box-shadow 0.2s;
}

.product-card:hover {
  box-shadow: 0 4px 16px rgba(0,0,0,0.14);
}

.product-card img {
  width: 180px;
  height: 180px;
  object-fit: contain;
  margin-bottom: 1rem;
}

.product-card a {
  text-decoration: none;
  color: var(--bb-blue);
}

.product-card a:hover {
  text-decoration: underline;
}

.product-card h2 {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  text-align: center;
}

.product-card p {
  font-size: 0.85rem;
  color: #555;
  margin-bottom: 0.75rem;
  text-align: center;
}

.product-details {
  width: 100%;
}

.product-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 0.75rem;
}

.product-price {
  font-weight: 700;
  font-size: 1.3rem;
  color: var(--bb-dark);
  text-align: center;
  margin-bottom: 0.25rem;
}

.price::before {
  content: '$';
}

.quantity-input {
  width: 52px;
  height: 38px;
  border: 1px solid var(--bb-border);
  border-radius: 4px;
  padding: 5px 8px;
  font-size: 0.95rem;
  text-align: center;
}

/* ── Shopping Cart ───────────────────────────────────────────────────────── */
.shopping-cart {
  max-width: 900px;
  margin: 2rem auto;
  padding: 2rem;
  background-color: #fff;
  border: 1px solid var(--bb-border);
  border-radius: 4px;
}

.shopping-cart h2 {
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
  color: var(--bb-blue);
  border-bottom: 3px solid var(--bb-yellow);
  padding-bottom: 0.5rem;
}

.shopping-cart h3 {
  text-align: center;
  color: #666;
  padding: 2rem;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
}

.shopping-cart-table th,
.shopping-cart-table td {
  padding: 12px 10px;
  text-align: left;
  border-bottom: 1px solid var(--bb-border);
}

.shopping-cart-table th {
  font-weight: 700;
  background-color: var(--bb-gray);
  color: var(--bb-dark);
}

.checkout-button {
  margin-top: 1.5rem;
  padding: 12px 32px;
  background-color: var(--bb-yellow);
  color: var(--bb-dark);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 700;
  font-size: 1rem;
  display: block;
  margin-left: auto;
}

.checkout-button:hover {
  background-color: #e6ca00;
}

/* ── Nav ─────────────────────────────────────────────────────────────────── */
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ul {
  display: flex;
  list-style: none;
}

li {
  margin: 0 1rem;
}

a {
  text-decoration: none;
}
</style>