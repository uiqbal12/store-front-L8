<template>
  <nav class="topnav">
    <!-- Left: Logo -->
    <div class="topnav-logo">
      <router-link to="/">
        <span class="logo-best">Best</span><span class="logo-buy">Buy</span>
      </router-link>
      <span class="logo-tagline">Tech &amp; Electronics</span>
    </div>

    <!-- Center: Search bar (cosmetic) -->
    <div class="topnav-search">
      <input type="text" placeholder="Search Best Buy..." class="search-input" />
      <button class="search-btn">&#128269;</button>
    </div>

    <!-- Right: Nav links + Cart -->
    <div class="topnav-right">
      <router-link to="/" class="nav-link">Products</router-link>
      <router-link to="/cart" class="nav-link cart-link">
        <span class="cart-icon">🛒</span>
        <span class="cart-label">Cart</span>
        <span class="cart-badge" v-if="cartItemCount > 0">{{ cartItemCount }}</span>
      </router-link>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'TopNav',
  props: ['cartItemCount']
}
</script>

<style scoped>
.topnav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background-color: #003DA5;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2rem;
  height: 70px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.3);
}

/* ── Logo ─────────────────────────────────────────────────────────────────── */
.topnav-logo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  line-height: 1;
}

.topnav-logo a {
  text-decoration: none;
  font-size: 1.8rem;
  font-weight: 900;
  letter-spacing: -1px;
}

.logo-best {
  color: #FFE000;
}

.logo-buy {
  color: #ffffff;
}

.logo-tagline {
  font-size: 0.65rem;
  color: rgba(255,255,255,0.6);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-top: 2px;
}

/* ── Search ───────────────────────────────────────────────────────────────── */
.topnav-search {
  display: flex;
  flex: 1;
  max-width: 480px;
  margin: 0 2rem;
}

.search-input {
  flex: 1;
  height: 40px;
  padding: 0 14px;
  border: none;
  border-radius: 4px 0 0 4px;
  font-size: 0.95rem;
  outline: none;
}

.search-btn {
  width: 48px;
  height: 40px;
  background-color: #FFE000;
  color: #1B1B1B;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
  font-size: 1.1rem;
  padding: 0;
}

.search-btn:hover {
  background-color: #e6ca00;
}

/* ── Right Links ──────────────────────────────────────────────────────────── */
.topnav-right {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

.nav-link {
  color: #ffffff;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  transition: color 0.2s;
}

.nav-link:hover {
  color: #FFE000;
}

/* ── Cart ─────────────────────────────────────────────────────────────────── */
.cart-link {
  display: flex;
  align-items: center;
  gap: 6px;
  position: relative;
  background-color: rgba(255,255,255,0.12);
  padding: 8px 14px;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.cart-link:hover {
  background-color: rgba(255,255,255,0.22);
  color: #FFE000;
}

.cart-icon {
  font-size: 1.2rem;
}

.cart-label {
  font-weight: 600;
}

.cart-badge {
  position: absolute;
  top: -6px;
  right: -6px;
  background-color: #FFE000;
  color: #1B1B1B;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 0.7rem;
  font-weight: 900;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>