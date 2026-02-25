<template>
  <div :class="['page-wrapper', containerClass]">
    
    <div v-if="loading" class="loader-container">
      <div class="loader"></div>
      <p>Please wait...</p>
    </div>

    <div v-else-if="!isUnavailable && product.id" class="product-card">
      <div class="product-image">
        <img :src="product.image" :alt="product.title" />
      </div>
      <div class="product-details">
        <h1 :class="['product-title', themeTextClass]">{{ product.title }}</h1>
        <div class="product-meta">
          <span class="category">{{ product.category }}</span>
          <div class="rating">
            <span>{{ product.rating.rate }}/5</span>
            <div class="rating-dots">
              <span v-for="n in 5" :key="n" 
                :class="['dot', n <= Math.round(product.rating.rate) ? themeBgClass : '']">
              </span>
            </div>
          </div>
        </div>
        <hr />
        <p class="description">{{ product.description }}</p>
        <hr />
        <div :class="['price', themeTextClass]">${{ product.price }}</div>
        <div class="button-group">
          <button :class="['btn-buy', themeBgClass]">Buy now</button>
          
          <button @click="getNextProduct" :class="['btn-next', themeBorderClass, themeTextClass]">
            Next product
          </button>
        </div>
      </div>
    </div>

    <div v-else class="unavailable-card">
      <div class="overlay-image">
        <img src="../assets/sad-img.png" alt="Unavailable background" class="sad-face-img" />
      </div>

      <div class="unavailable-content">
        <p>This product is unavailable to show</p>
        <button @click="getNextProduct" class="btn-next-unavailable">
          Next product
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
      index: 0, 
      loading: false,
      isUnavailable: false
    };
  },
  computed: {
    containerClass() {
      if (this.isUnavailable) return 'bg-unavailable';
      if (this.product.category === "men's clothing") return 'bg-men';
      if (this.product.category === "women's clothing") return 'bg-women';
      return 'bg-unavailable';
    },
    // Menambahkan class untuk warna teks tema judul & harga
    themeBgClass() {
    if (this.product.category === "men's clothing") return 'bg-men-btn';
    return 'bg-women-btn';
  },
  
  // Memberikan class warna teks & border untuk tombol Next Product
  themeTextClass() {
    if (this.product.category === "men's clothing") return 'text-men';
    return 'text-women';
  },
  
  themeBorderClass() {
    if (this.product.category === "men's clothing") return 'border-men';
    return 'border-women';
  }
},
  methods: {
    async getNextProduct() {
      this.loading = true;
      this.index = this.index >= 20 ? 1 : this.index + 1;

      try {
        const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        const data = await response.json();

        // Cek kategori
        if (data.category === "men's clothing" || data.category === "women's clothing") {
          this.product = data;
          this.isUnavailable = false;
        } else {
          this.isUnavailable = true;
        }
      } catch (error) {
        this.isUnavailable = true;
      } finally {
        setTimeout(() => { this.loading = false; }, 500); // Simulasi delay untuk loader
      }
    }
  },
  mounted() {
    this.getNextProduct();
  }
}
</script>

<style scoped>
/* Import CSS dari folder assets */
@import "../assets/style/color-palette.css";
@import "../assets/style/men-section.css";
@import "../assets/style/women-section.css";
@import "../assets/style/unavail-product.css";

/* CSS untuk Loader Spinner */
.loader-container {
  text-align: center;
}
.loader {
  border: 8px solid #f3f3f3;
  border-top: 8px solid #3498db;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  animation: spin 2s linear infinite;
  margin: 0 auto 20px;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Layout dasar */
.page-wrapper {
  min-height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.3s ease;
}
.product-card {
  background: white;
  display: flex;
  padding: 50px;
  border-radius: 10px;
  width: 90%;
  max-width: 1000px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
}
.product-image img {
  max-width: 300px;
  max-height: 400px;
}
.product-title {
  font-size: 1.8rem;
  margin-bottom: 10px;
}
.category {
  font-size: 0.9rem;
  color: gray;
}
.dot {
  height: 12px;
  width: 12px;
  border-radius: 50%;
  display: inline-block;
  border: 1px solid currentColor;
}
.product-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.description {
  margin: 10px 0 90px 0;
}
.product-details {
  padding-left: 40px;
  flex: 1;
}
.price {
  font-size: 1.5rem;
  font-weight: bold;
  margin: 10px 0 20px 0;
}

/* Responsive */
@media (max-width: 768px) {
  .product-grid {
    grid-template-columns: 1fr !important;
    gap: 1rem;
  }
}
</style>