<template>
  <div class="cart-page">
    <h1 class="cart-title">Shopping Cart</h1>

    <div class="cart-layout" v-if="hasCartItems">
      <!-- Cart Items Table -->
      <div class="cart-items">
        <table class="shopping-cart-table">
          <thead>
            <tr>
              <th>Product</th>
              <th>Qty</th>
              <th>Unit Price</th>
              <th>Total</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cartItems" :key="item.product.id">
              <td class="product-name">{{ item.product.name }}</td>
              <td>{{ item.quantity }}</td>
              <td>${{ Number(item.product.price).toFixed(2) }}</td>
              <td class="item-total">${{ getItemTotal(item) }}</td>
              <td>
                <button class="remove-btn" @click="removeFromCart(item)">✕</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- Order Summary -->
      <div class="order-summary">
        <h2>Order Summary</h2>
        <div class="summary-row">
          <span>Subtotal</span>
          <span>${{ cartSubtotal }}</span>
        </div>
        <div class="summary-row">
          <span>Shipping</span>
          <span class="free-shipping">FREE</span>
        </div>
        <div class="summary-row">
          <span>Tax (13% HST)</span>
          <span>${{ cartTax }}</span>
        </div>
        <div class="summary-total">
          <span>Total</span>
          <span>${{ cartTotal }}</span>
        </div>
        <button class="checkout-button" @click="submitOrder">
          Checkout
        </button>
        <router-link to="/" class="continue-link">← Continue Shopping</router-link>
      </div>
    </div>

    <!-- Empty Cart -->
    <div class="empty-cart" v-else>
      <div class="empty-cart-icon">🛒</div>
      <h2>Your cart is empty</h2>
      <p>Looks like you haven't added anything yet.</p>
      <router-link to="/" class="shop-btn">Start Shopping</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShoppingCart',
  props: ['cartItems'],
  computed: {
    hasCartItems() {
      return this.cartItems.length > 0
    },
    cartSubtotal() {
      return this.cartItems
        .reduce((sum, item) => sum + item.quantity * item.product.price, 0)
        .toFixed(2)
    },
    cartTax() {
      return (this.cartSubtotal * 0.13).toFixed(2)
    },
    cartTotal() {
      return (parseFloat(this.cartSubtotal) + parseFloat(this.cartTax)).toFixed(2)
    }
  },
  methods: {
    getItemTotal(item) {
      return (item.quantity * item.product.price).toFixed(2)
    },
    removeFromCart(item) {
      const index = this.cartItems.indexOf(item)
      if (index > -1) {
        this.$emit('removeFromCart', index)
      }
    },
    submitOrder() {
      this.$emit('submitOrder')
    }
  }
}
</script>

<style scoped>
.cart-page {
  max-width: 1100px;
  margin: 0 auto;
  padding: 1.5rem 2rem;
}

.cart-title {
  font-size: 1.6rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  color: #1B1B1B;
  border-bottom: 3px solid #FFE000;
  padding-bottom: 0.5rem;
  display: inline-block;
}

.cart-layout {
  display: flex;
  gap: 2rem;
  align-items: flex-start;
}

/* ── Cart Table ───────────────────────────────────────────────────────────── */
.cart-items {
  flex: 1;
  background: #fff;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  overflow: hidden;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
}

.shopping-cart-table th {
  background-color: #003DA5;
  color: #fff;
  padding: 12px 14px;
  text-align: left;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.shopping-cart-table td {
  padding: 14px;
  border-bottom: 1px solid #F0F0F0;
  font-size: 0.95rem;
}

.shopping-cart-table tr:last-child td {
  border-bottom: none;
}

.product-name {
  font-weight: 600;
}

.item-total {
  font-weight: 700;
  color: #1B1B1B;
}

.remove-btn {
  background: none;
  border: 1px solid #E0E0E0;
  color: #888;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 0.75rem;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.remove-btn:hover {
  background-color: #ff4444;
  border-color: #ff4444;
  color: #fff;
}

/* ── Order Summary ────────────────────────────────────────────────────────── */
.order-summary {
  width: 300px;
  background: #fff;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
  padding: 1.5rem;
  flex-shrink: 0;
}

.order-summary h2 {
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 1.25rem;
  padding-bottom: 0.75rem;
  border-bottom: 2px solid #FFE000;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.75rem;
  font-size: 0.9rem;
  color: #444;
}

.free-shipping {
  color: #008000;
  font-weight: 700;
}

.summary-total {
  display: flex;
  justify-content: space-between;
  font-size: 1.15rem;
  font-weight: 800;
  color: #1B1B1B;
  border-top: 2px solid #E0E0E0;
  padding-top: 0.75rem;
  margin-top: 0.75rem;
  margin-bottom: 1.25rem;
}

.checkout-button {
  width: 100%;
  padding: 14px;
  background-color: #FFE000;
  color: #1B1B1B;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 800;
  font-size: 1rem;
  height: auto;
  margin-bottom: 1rem;
  transition: background-color 0.2s;
}

.checkout-button:hover {
  background-color: #e6ca00;
}

.continue-link {
  display: block;
  text-align: center;
  color: #003DA5;
  font-size: 0.85rem;
  text-decoration: none;
}

.continue-link:hover {
  text-decoration: underline;
}

/* ── Empty Cart ───────────────────────────────────────────────────────────── */
.empty-cart {
  text-align: center;
  padding: 4rem;
  background: #fff;
  border: 1px solid #E0E0E0;
  border-radius: 4px;
}

.empty-cart-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.empty-cart h2 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.empty-cart p {
  color: #666;
  margin-bottom: 1.5rem;
}

.shop-btn {
  display: inline-block;
  padding: 12px 32px;
  background-color: #FFE000;
  color: #1B1B1B;
  border-radius: 4px;
  font-weight: 700;
  text-decoration: none;
  transition: background-color 0.2s;
}

.shop-btn:hover {
  background-color: #e6ca00;
}

@media (max-width: 768px) {
  .cart-layout {
    flex-direction: column;
  }
  .order-summary {
    width: 100%;
  }
}
</style>