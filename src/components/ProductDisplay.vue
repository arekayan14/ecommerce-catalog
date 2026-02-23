<template>
  <div :class="['page-wrapper', containerClass]">
    
    <div v-if="loading" class="loader">Tunggu bentar ya...</div>

    <div v-else-if="!isUnavailable" class="product-card">
      <div class="product-image">
        <img :src="product.image" alt="product image" />
      </div>
      <div class="product-details">
        <h1 :class="themeText">{{ product.title }}</h1>
        <div class="product-meta">
          <span>{{ product.category }}</span>
          <span>{{ product.rating.rate }}/5</span>
        </div>
        <hr />
        <p class="description">{{ product.description }}</p>
        <hr />
        <div :class="['price', themeText]">${{ product.price }}</div>
        <div class="button-group">
          <button :class="['btn-buy', themeBg]">Buy now</button>
          <button @click="getNextProduct" :class="['btn-next', themeBorder, themeText]">Next product</button>
        </div>
      </div>
    </div>

    <div v-else class="unavailable-card">
      <p>This product is unavailable to show</p>
      <button @click="getNextProduct" class="btn-next-gray">Next product</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 0, // Hint API 4: Start index
      product: {}, // Hint API 5: Variable buat simpen balasan
      loading: false,
      isUnavailable: false
    };
  },
  computed: {
    // Logika nentuin desain (Hint Desain 4)
    containerClass() {
      if (this.isUnavailable) return 'bg-unavailable';
      return this.product.category === "men's clothing" ? 'bg-men' : 'bg-women';
    },
    themeText() { return this.product.category === "men's clothing" ? 'text-men' : 'text-women'; },
    themeBg() { return this.product.category === "men's clothing" ? 'btn-men' : 'btn-women'; },
    themeBorder() { return this.product.category === "men's clothing" ? 'border-men' : 'border-women'; }
  },
  methods: {
    async getNextProduct() {
      this.loading = true;
      
      // Kembali ke produk 1 jika sudah lewat produk ke 20
      this.index = this.index >= 20 ? 1 : this.index + 1;

      try {
        // Fakestore API untuk ambil produk berdasarkan index
        const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        const data = await response.json();

        // Condition, cek kategori
        if (data.category === "men's clothing" || data.category === "women's clothing") {
          this.product = data;
          this.isUnavailable = false;
        } else {
        // Jika kategori lain, disimpan menjadi unavailable produk
          this.isUnavailable = true;
        }
      } catch (error) {
        console.error("Waduh, koneksi putus:", error);
      } finally {
        this.loading = false;
      }
    }
  },
  mounted() {
    this.getNextProduct(); // Jalanin pertama kali pas web dibuka
  }
};
</script>

<style>
/* Panggil sesuai urutan struktur folder lo */
@import '../assets/style/color-palette.css';
@import '../assets/style/men-section.css';
@import '../assets/style/women-section.css';
@import '../assets/style/unavail-product.css';

/* Tambahkan styling umum untuk layout card-nya */
.page-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  transition: 0.3s;
}

.product-card {
  background: white;
  display: flex;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  width: 80%;
  max-width: 1000px;
  gap: 30px;
}

.product-image img {
  width: 300px;
  height: auto;
}
</style>