<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Navigation Bar -->
    <nav class="nav-bar">
      <div class="container">
        <div class="nav-content">
          <div class="nav-brand">
            <h1 class="brand-title">FashionHub</h1>
            <span class="brand-subtitle">Premium Fashion Store</span>
          </div>      
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero-section">
      <div class="container">
        <div class="hero-content">
          <h2 class="hero-title">Discover Premium Fashion</h2>
          <p class="hero-subtitle">Curated collections for modern style enthusiasts</p>        
        </div>
      </div>
    </section>

    <!-- Main Content -->
    <main class="main-content">
      <div class="container">
        <!-- Collections Grid -->
        <div class="collections-grid">
          <!-- Women's Collection - Left Side -->
          <div class="collection-card women-collection">
            <div class="collection-header">
              <div class="collection-title">              
                <div>
                  <h3>Women's Collection</h3>
                  <p>{{ womenProducts.length }} premium items</p>
                </div>
              </div>
              <div class="collection-actions">
                <button 
                  class="action-btn secondary"
                  @click="nextProduct('women')"
                  :disabled="loading.women || womenProducts.length === 0"
                >
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M4,11V13H16L10.5,18.5L11.92,19.92L19.84,12L11.92,4.08L10.5,5.5L16,11H4Z"/>
                  </svg>
                  Next
                </button>
              </div>
            </div>
            
            <div class="product-showcase">
              <!-- Loading State -->
              <div v-if="loading.women" class="loading-state">
                <div class="loading-spinner women-spinner"></div>
                <p>Loading women's collection...</p>
              </div>
              
              <!-- Product Available -->
              <div v-else-if="currentWomenProduct" class="product-card">
                <!-- Add loading overlay -->
                <div v-if="changingProduct.women" class="product-changing-overlay">
                  <div class="changing-spinner"></div>
                  <p>Changing product...</p>
                </div>
                
                <div class="product-image-container">
                  <img 
                    :src="currentWomenProduct.image" 
                    :alt="currentWomenProduct.title"
                    class="product-image"
                    @error="handleImageError"
                  />
                  <div class="product-overlay">
                    <div class="product-category">{{ currentWomenProduct.category }}</div>
                  </div>
                </div>
                
                <div class="product-details">
                  <h4 class="product-title">{{ currentWomenProduct.title }}</h4>
                  
                  <div class="product-rating">
                    <div class="stars">
                      <span 
                        v-for="i in 5" 
                        :key="i"
                        class="star"
                        :class="{ active: i <= Math.round(currentWomenProduct.rating?.rate || 0) }"
                      >
                        ★
                      </span>
                    </div>
                    <span class="rating-text">
                      {{ currentWomenProduct.rating?.rate?.toFixed(1) || '0.0' }} 
                      ({{ currentWomenProduct.rating?.count || 0 }} reviews)
                    </span>
                  </div>
                  
                  <p class="product-description">{{ currentWomenProduct.description }}</p>
                  
                  <div class="product-footer">
                    <div class="product-price">${{ currentWomenProduct.price }}</div>
                    <button 
                      class="buy-btn women-buy"
                      @click="buyNow('women')"
                    >
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19 7H16V6A4 4 0 0 0 8 6V7H5A1 1 0 0 0 4 8V19A3 3 0 0 0 7 22H17A3 3 0 0 0 20 19V8A1 1 0 0 0 19 7ZM10 6A2 2 0 0 1 14 6V7H10V6ZM18 19A1 1 0 0 1 17 20H7A1 1 0 0 1 6 19V9H8V10A1 1 0 0 0 10 10A1 1 0 0 0 10 10V9H14V10A1 1 0 0 0 16 10A1 1 0 0 0 14 10V9H18V19Z"/>
                      </svg>
                      Buy Now
                    </button>
                  </div>
                </div>
              </div>
              
              <!-- No Product Available -->
              <div v-else class="empty-state">
                <div class="empty-icon">
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M23,12L20.56,9.22L20.9,5.54L17.29,4.72L15.4,1.54L12,3L8.6,1.54L6.71,4.72L3.1,5.53L3.44,9.21L1,12L3.44,14.78L3.1,18.47L6.71,19.29L8.6,22.47L12,21L15.4,22.46L17.29,19.28L20.9,18.46L20.56,14.78L23,12M13,17H11V15H13V17M13,13H11V7H13V13Z"/>
                  </svg>
                </div>
                <h4>Product Not Available</h4>
                <p>Women's collection is currently unavailable. Please try again later.</p>
                <button 
                  class="action-btn secondary"
                  @click="nextProduct('women')"
                  :disabled="womenProducts.length === 0"
                >
                  Try Again
                </button>
              </div>
            </div>
          </div>

          <!-- Men's Collection - Right Side -->
          <div class="collection-card men-collection">
            <div class="collection-header">
              <div class="collection-title">              
                <div>
                  <h3>Men's Collection</h3>
                  <p>{{ menProducts.length }} premium items</p>
                </div>
              </div>
              <div class="collection-actions">
                <button 
                  class="action-btn secondary"
                  @click="nextProduct('men')"
                  :disabled="loading.men || menProducts.length === 0"
                >
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M4,11V13H16L10.5,18.5L11.92,19.92L19.84,12L11.92,4.08L10.5,5.5L16,11H4Z"/>
                  </svg>
                  Next
                </button>
              </div>
            </div>
            
            <div class="product-showcase">
              <!-- Loading State -->
              <div v-if="loading.men" class="loading-state">
                <div class="loading-spinner men-spinner"></div>
                <p>Loading men's collection...</p>
              </div>
              
              <!-- Product Available -->
              <div v-else-if="currentMenProduct" class="product-card">
                <!-- Add loading overlay -->
                <div v-if="changingProduct.men" class="product-changing-overlay">
                  <div class="changing-spinner"></div>
                  <p>Changing product...</p>
                </div>
                
                <div class="product-image-container">
                  <img 
                    :src="currentMenProduct.image" 
                    :alt="currentMenProduct.title"
                    class="product-image"
                    @error="handleImageError"
                  />
                  <div class="product-overlay">
                    <div class="product-category">{{ currentMenProduct.category }}</div>
                  </div>
                </div>
                
                <div class="product-details">
                  <h4 class="product-title">{{ currentMenProduct.title }}</h4>
                  
                  <div class="product-rating">
                    <div class="stars">
                      <span 
                        v-for="i in 5" 
                        :key="i"
                        class="star"
                        :class="{ active: i <= Math.round(currentMenProduct.rating?.rate || 0) }"
                      >
                        ★
                      </span>
                    </div>
                    <span class="rating-text">
                      {{ currentMenProduct.rating?.rate?.toFixed(1) || '0.0' }} 
                      ({{ currentMenProduct.rating?.count || 0 }} reviews)
                    </span>
                  </div>
                  
                  <p class="product-description">{{ currentMenProduct.description }}</p>
                  
                  <div class="product-footer">
                    <div class="product-price">${{ currentMenProduct.price }}</div>
                    <button 
                      class="buy-btn men-buy"
                      @click="buyNow('men')"
                    >
                      <svg viewBox="0 0 24 24" fill="currentColor">
                        <path d="M19 7H16V6A4 4 0 0 0 8 6V7H5A1 1 0 0 0 4 8V19A3 3 0 0 0 7 22H17A3 3 0 0 0 20 19V8A1 1 0 0 0 19 7ZM10 6A2 2 0 0 1 14 6V7H10V6ZM18 19A1 1 0 0 1 17 20H7A1 1 0 0 1 6 19V9H8V10A1 1 0 0 0 10 10A1 1 0 0 0 10 10V9H14V10A1 1 0 0 0 16 10A1 1 0 0 0 14 10V9H18V19Z"/>
                      </svg>
                      Buy Now
                    </button>
                  </div>
                </div>
              </div>
              
              <!-- No Product Available -->
              <div v-else class="empty-state">
                <div class="empty-icon">
                  <svg viewBox="0 0 24 24" fill="currentColor">
                    <path d="M23,12L20.56,9.22L20.9,5.54L17.29,4.72L15.4,1.54L12,3L8.6,1.54L6.71,4.72L3.1,5.53L3.44,9.21L1,12L3.44,14.78L3.1,18.47L6.71,19.29L8.6,22.47L12,21L15.4,22.46L17.29,19.28L20.9,18.46L20.56,14.78L23,12M13,17H11V15H13V17M13,13H11V7H13V13Z"/>
                  </svg>
                </div>
                <h4>Product Not Available</h4>
                <p>Men's collection is currently unavailable. Please try again later.</p>
                <button 
                  class="action-btn secondary"
                  @click="nextProduct('men')"
                  :disabled="menProducts.length === 0"
                >
                  Try Again
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-brand">
            <h3>FashionHub</h3>
            <p>Your trusted fashion destination</p>
          </div>
          <div class="footer-links">
            <a href="#" class="footer-link">Privacy Policy</a>
            <a href="#" class="footer-link">Terms of Service</a>
            <a href="#" class="footer-link">Contact Us</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'EcommerceDashboard',
  data() {
    return {
      allProducts: [],
      womenProducts: [],
      menProducts: [],
      currentWomenIndex: 0,
      currentMenIndex: 0,
      loading: {
        women: true,
        men: true
      },
      // Add new states for transition loading
      changingProduct: {
        women: false,
        men: false
      }
    }
  },
  computed: {
    currentWomenProduct() {
      return this.womenProducts[this.currentWomenIndex] || null
    },
    currentMenProduct() {
      return this.menProducts[this.currentMenIndex] || null
    },
    totalProducts() {
      return this.allProducts.length
    },
    averageRating() {
      if (this.allProducts.length === 0) return '0.0'
      const totalRating = this.allProducts.reduce((sum, product) => {
        return sum + (product.rating?.rate || 0)
      }, 0)
      return (totalRating / this.allProducts.length).toFixed(1)
    }
  },
  async mounted() {
    await this.fetchProducts()
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await fetch('https://fakestoreapi.com/products')
        this.allProducts = await response.json()
        
        // Filter products by category
        this.womenProducts = this.allProducts.filter(product => 
          product.category === "women's clothing"
        )
        this.menProducts = this.allProducts.filter(product => 
          product.category === "men's clothing"
        )
        
        this.loading.women = false
        this.loading.men = false
      } catch (error) {
        console.error('Error fetching products:', error)
        this.loading.women = false
        this.loading.men = false
      }
    },
    nextProduct(section) {
      // Set loading state for the section
      this.changingProduct[section] = true;
      
      // Use setTimeout to simulate loading and provide visual feedback
      setTimeout(() => {
        if (section === 'women') {
          this.currentWomenIndex = (this.currentWomenIndex + 1) % this.womenProducts.length
        } else if (section === 'men') {
          this.currentMenIndex = (this.currentMenIndex + 1) % this.menProducts.length
        }
        
        // Reset loading state after a short delay
        setTimeout(() => {
          this.changingProduct[section] = false;
        }, 300);
      }, 500);
    },
    buyNow(section) {
      const product = section === 'women' ? this.currentWomenProduct : this.currentMenProduct
      alert(`Purchasing: ${product.title} for $${product.price}`)
    },
    handleImageError(event) {
      event.target.src = 'https://via.placeholder.com/400x400?text=No+Image'
    }
  }
}
</script>

<style scoped>
/* Variables for color palette - Light Theme */
:root {
  --primary-blue: #002772;
  --primary-purple: #720060;
  --women-bg: #FDE2FF; /* Women's background color as requested */
  --men-bg: #010101;
  --women-button: #720060; /* Women's button color as requested */
  --men-button: #720060; /* Changed to match women's button color */
  --dark-gray: #1E1E1E;
  --medium-gray: #3F3F3F;
  --light-gray: #F9F9F9;
  --border-light: #EFEFEF;
  --white: #FFFFFF;
  --text-primary: #000000; /* Changed to pure black */
  --text-secondary: #454545; /* Made darker for better contrast with black text */
  --shadow-light: rgba(0, 0, 0, 0.05);
  --shadow-medium: rgba(0, 0, 0, 0.08);
}

/* Global Styles */
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: var(--white);
  color: var(--text-primary);
  line-height: 1.6;
}

.min-h-screen {
  min-height: 100vh;
  background-color: var(--white);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 16px;
}

@media (min-width: 768px) {
  .container {
    padding: 0 24px;
  }
}

@media (min-width: 1024px) {
  .container {
    padding: 0 32px;
  }
}

/* Navigation Bar */
.nav-bar {
  background: var(--white);
  color: var(--text-primary);
  padding: 16px 0;
  box-shadow: 0 2px 10px var(--shadow-medium);
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.nav-brand {
  display: flex;
  flex-direction: column;
}

.brand-title {
  font-size: 28px;
  font-weight: 700;
  margin: 0;
  background: linear-gradient(45deg, var(--primary-blue) 0%, var(--primary-purple) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.brand-subtitle {
  font-size: 14px;
  color: var(--text-secondary);
  margin-top: 4px;
}

.nav-stats {
  display: flex;
  align-items: center;
  gap: 24px;
  background: var(--light-gray);
  padding: 8px 16px;
  border-radius: 30px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.stat-number {
  font-size: 20px;
  font-weight: 700;
  line-height: 1;
  color: var(--text-primary);
}

.stat-label {
  font-size: 12px;
  color: var(--text-secondary);
  margin-top: 4px;
}

.stat-divider {
  width: 1px;
  height: 30px;
  background-color: var(--border-light);
}

/* Hero Section */
.hero-section {
  background: var(--white);
  padding: 40px 0;
  margin-bottom: 40px;
  border-bottom: 1px solid var(--border-light);
}

.hero-content {
  text-align: center;
  max-width: 800px;
  margin: 0 auto;
}

.hero-title {
  font-size: 42px;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0 0 16px 0;
  line-height: 1.2;
}

.hero-subtitle {
  font-size: 18px;
  color: var(--text-secondary);
  margin: 0 0 40px 0;
}

.hero-features {
  display: flex;
  justify-content: center;
  gap: 32px;
  flex-wrap: wrap;
}

.feature-badge {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--white);
  padding: 16px 24px;
  border-radius: 12px;
  font-weight: 600;
  color: var(--text-primary);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  box-shadow: 0 4px 12px var(--shadow-light);
  border: 1px solid var(--border-light);
}

.feature-badge:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px var(--shadow-medium);
}

.feature-badge span:first-child {
  font-size: 20px;
}

/* Main Content */
.main-content {
  padding: 0 0 60px 0;
  background-color: var(--white);
}

.collections-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
  min-height: 700px;
}

/* Collection Cards */
.collection-card {
  background: var(--white);
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px var(--shadow-medium);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  border: 1px solid var(--border-light);
}

.collection-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 30px var(--shadow-medium);
}

.women-collection {
  border-top: 4px solid var(--women-button);
}

.men-collection {
  border-top: 4px solid var(--men-button);
}

.collection-header {
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid var(--border-light);
  background-color: var(--white);
}

.collection-title {
  display: flex;
  align-items: center;
  gap: 16px;
}

.collection-icon {
  width: 48px;
  height: 48px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--white);
}

.women-icon {
  background: linear-gradient(135deg, var(--women-button) 0%, #FF79B0 100%);
}

.men-icon {
  background: linear-gradient(135deg, var(--men-button) 0%, #64B5F6 100%);
}

.collection-icon svg {
  width: 24px;
  height: 24px;
}

.collection-title h3 {
  font-size: 18px;
  font-weight: 700;
  margin: 0;
  color: var(--text-primary);
}

.collection-title p {
  font-size: 14px;
  color: var(--text-secondary);
  margin: 4px 0 0 0;
}

.collection-actions .action-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  border-radius: 8px;
  font-weight: 600;
  border: 1px solid var(--border-light);
  cursor: pointer;
  transition: all 0.2s ease;
  background-color: var(--white);
  color: var(--text-primary);
}

.action-btn.secondary:hover:not(:disabled) {
  background: var(--light-gray);
}

.action-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.action-btn svg {
  width: 16px;
  height: 16px;
}

/* Product Showcase */
.product-showcase {
  padding: 24px;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: var(--white);
}

.women-collection .product-showcase {
  background: var(--women-bg); /* Using the specified color */
  position: relative;
}

.women-collection .product-showcase::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 120px;
  background: linear-gradient(180deg, var(--women-bg) 0%, rgba(255,255,255,0) 100%);
  z-index: 0;
  border-radius: 0 0 50% 50% / 20px;
}

.men-collection .product-showcase {
  background: var(--white);
  position: relative;
}

.men-collection .product-showcase::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 120px;
  background: linear-gradient(180deg, var(--men-bg) 0%, rgba(255,255,255,0) 100%);
  z-index: 0;
  border-radius: 0 0 50% 50% / 20px;
}

/* Loading State */
.loading-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 400px;
  text-align: center;
  position: relative;
  z-index: 1;
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 3px solid var(--light-gray);
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 16px;
}

.women-spinner {
  border-top-color: var(--women-button);
}

.men-spinner {
  border-top-color: var(--men-button);
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.loading-state p {
  color: var(--text-secondary);
  font-weight: 500;
  margin: 0;
}

/* Product Card */
.product-card {
  background: var(--white);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 16px var(--shadow-light);
  position: relative;
  z-index: 1;
  border: 1px solid var(--border-light);
}

.product-image-container {
  position: relative;
  height: 280px;
  background: var(--white);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  border-bottom: 1px solid var(--border-light);
}

.product-image {
  max-width: 100%;
  max-height: 240px;
  object-fit: contain;
  transition: transform 0.3s ease;
  padding: 10px;
}

.product-card:hover .product-image {
  transform: scale(1.05);
}

.product-overlay {
  position: absolute;
  top: 16px;
  left: 16px;
  display: flex;
  gap: 8px;
}

.product-category {
  background: var(--text-primary);
  color: var(--white);
  padding: 6px 12px;
  border-radius: 30px;
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.product-details {
  padding: 20px;
}

.product-title {
  font-size: 16px;
  font-weight: 600;
  color: var(--text-primary); /* This will now be pure black */
  margin: 0 0 12px 0;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  height: 44px;
}

.product-rating {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 12px;
}

.stars {
  display: flex;
  gap: 2px;
}

.star {
  font-size: 16px;
  color: #e5e7eb;
  transition: color 0.2s ease;
}

.star.active {
  color: #FFC107;
}

.rating-text {
  font-size: 12px;
  color: var(--text-secondary);
  font-weight: 500;
}

.product-description {
  font-size: 13px;
  color: var(--text-secondary);
  line-height: 1.6;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  height: 62px;
  height: 62px;
}

.product-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
  padding-top: 12px;
  border-top: 1px solid var(--border-light);
}

.product-price {
  font-size: 24px;
  font-weight: 700;
  color: var(--text-primary);
}

.women-collection .product-price {
  color: var(--women-button);
}

.men-collection .product-price {
  color: var(--men-button);
}

.buy-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  border-radius: 8px;
  font-weight: 600;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  color: white;
  /* background: white; Direct color setting for all buy buttons */
}

.women-buy {
  background: #720060 ;
}

.men-buy {
  background: #002772; /* Changed to match women's button */
}

.buy-btn:hover {
  filter: brightness(2);
}

.buy-btn svg {
  width: 16px;
  height: 16px;
}

/* Empty State */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  height: 400px;
  padding: 40px;
  position: relative;
  z-index: 1;
}

.empty-icon {
  width: 60px;
  height: 60px;
  background: var(--light-gray);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  color: var(--text-secondary);
}

.empty-icon svg {
  width: 30px;
  height: 30px;
}

.empty-state h4 {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0 0 8px 0;
}

.empty-state p {
  font-size: 14px;
  color: var(--text-secondary);
  margin: 0 0 20px 0;
  line-height: 1.5;
}

/* Footer */
.footer {
  background: var(--white);
  color: var(--text-primary);
  padding: 40px 0;
  margin-top: 60px;
  border-top: 1px solid var(--border-light);
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 24px;
}

.footer-brand h3 {
  font-size: 24px;
  font-weight: 700;
  margin: 0 0 8px 0;
  background: linear-gradient(45deg, var(--primary-blue) 0%, var(--primary-purple) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.footer-brand p {
  color: var(--text-secondary);
  margin: 0;
}

.footer-links {
  display: flex;
  gap: 32px;
}

.footer-link {
  color: var(--text-secondary);
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s ease;
}

.footer-link:hover {
  color: var(--text-primary);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .collections-grid {
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    height: auto;
  }
  
  .hero-title {
    font-size: 36px;
  }
  
  .container {
    max-width: 100%;
  }
}

@media (max-width: 968px) {
  .collections-grid {
    grid-template-columns: 1fr;
    gap: 24px;
    height: auto;
  }
}

@media (max-width: 768px) {
  .container {
    padding: 0 16px;
  }
  
  .collections-grid {
    grid-template-columns: 1fr;
    gap: 24px;
    height: auto;
  }
  
  .collection-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
    padding: 20px;
  }
  
  .collection-actions {
    width: 100%;
    display: flex;
    justify-content: flex-end;
  }
  
  .hero-title {
    font-size: 28px;
  }
  
  .hero-subtitle {
    font-size: 16px;
  }
  
  .hero-features {
    gap: 16px;
    flex-direction: column;
    align-items: center;
  }
  
  .feature-badge {
    padding: 12px 20px;
    width: 100%;
    max-width: 280px;
    justify-content: center;
  }
  
  .nav-content {
    flex-direction: column;
    text-align: center;
    gap: 16px;
  }
  
  .nav-stats {
    gap: 16px;
  }
  
  .stat-item {
    min-width: 80px;
  }
  
  .footer-content {
    flex-direction: column;
    text-align: center;
    gap: 20px;
  }
  
  .footer-links {
    flex-direction: column;
    gap: 16px;
  }
  
  .product-image-container {
    height: 240px;
  }
  
  .product-details {
    padding: 20px;
  }
  
  .product-title {
    font-size: 16px;
  }
  
  .product-price {
    font-size: 24px;
  }
}

@media (max-width: 480px) {
  .container {
    padding: 0 12px;
  }
  
  .product-showcase {
    padding: 16px;
  }
  
  .product-details {
    padding: 16px;
  }
  
  .product-footer {
    flex-direction: column;
    align-items: stretch;
    gap: 16px;
  }
  
  .buy-btn {
    justify-content: center;
    padding: 16px 24px;
    font-size: 16px;
  }
  
  .collection-header {
    padding: 16px;
  }
  
  .collection-title {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }
  
  .collection-icon {
    width: 40px;
    height: 40px;
  }
  
  .collection-icon svg {
    width: 20px;
    height: 20px;
  }
  
  .hero-section {
    padding: 40px 0;
  }
  
  .hero-title {
    font-size: 24px;
  }
  
  .hero-subtitle {
    font-size: 14px;
  }
  
  .brand-title {
    font-size: 24px;
  }
  
  .brand-subtitle {
    font-size: 12px;
  }
  
  .stat-number {
    font-size: 20px;
  }
  
  .stat-label {
    font-size: 10px;
  }
  
  .product-image-container {
    height: 200px;
  }
  
  .loading-state {
    height: 300px;
  }
  
  .empty-state {
    height: 300px;
    padding: 20px;
  }
  
  .empty-icon {
    width: 60px;
    height: 60px;
  }
  
  .empty-icon svg {
    width: 30px;
    height: 30px;
  }
  
  .empty-state h4 {
    font-size: 18px;
  }
  
  .empty-state p {
    font-size: 14px;
  }
}

/* Touch and Accessibility Improvements */
@media (hover: none) and (pointer: coarse) {
  .collection-card:hover {
    transform: none;
  }
  
  .feature-badge:hover {
    transform: none;
  }
  
  .buy-btn:hover {
    transform: none;
  }
  
  .action-btn {
    min-height: 44px;
    min-width: 44px;
  }
  
  .buy-btn {
    min-height: 48px;
  }
}

/* High DPI Display Support */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .product-image {
    image-rendering: -webkit-optimize-contrast;
    image-rendering: crisp-edges;
  }
}

/* Improved Focus States for Accessibility */
.action-btn:focus,
.buy-btn:focus {
  outline: 2px solid var(--primary-blue);
  outline-offset: 2px;
}

.women-collection .action-btn:focus,
.women-collection .buy-btn:focus {
  outline-color: var(--women-button);
}

.men-collection .action-btn:focus,
.men-collection .buy-btn:focus {
  outline-color: var(--men-button);
}

/* Print Styles */
@media print {
  .nav-bar,
  .footer,
  .action-btn,
  .buy-btn {
    display: none;
  }
  
  .collection-card {
    break-inside: avoid;
    box-shadow: none;
    border: 1px solid var(--border-light);
  }
  
  .hero-section {
    background: none;
    color: var(--dark-gray);
  }
}

Line Clamp Utilities
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  line-clamp: 2;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
  line-clamp: 3;
}

/* Custom scrollbar for light theme */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: var(--white);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: var(--border-light);
  border-radius: 4px;
  transition: background 0.3s ease;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--text-secondary);
}

::-webkit-scrollbar-corner {
  background: var(--white);
}

/* Smooth scroll behavior */
html {
  scroll-behavior: smooth;
}

/* Selection styling */
::selection {
  background: var(--light-gray);
  color: var(--text-primary);
}

::-moz-selection {
  background: var(--light-gray);
  color: var(--text-primary);
}

/* Product changing overlay styling */
.product-changing-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.8);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 10;
  backdrop-filter: blur(4px);
  animation: fadeIn 0.3s ease;
}

.women-collection .product-changing-overlay {
  background: rgba(253, 226, 255, 0.9);
}

.men-collection .product-changing-overlay {
  background: rgba(1, 1, 1, 0.1);
}

.changing-spinner {
  width: 50px;
  height: 50px;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-radius: 50%;
  border-top-color: #720060;
  animation: spin 1s infinite linear;
  margin-bottom: 16px;
}

.product-changing-overlay p {
  font-weight: 600;
  color: #000000;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>