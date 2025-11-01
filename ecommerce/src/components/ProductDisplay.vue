<script>
export default {
  data() {
    return {
      allProducts: [],
      menProducts: [],
      womenProducts:[],
      currentView: "men",
      menIndex: 0,
      womenIndex: 0,
      isLoading: false,
    };
  },

  computed: {
    currentProduct() {
      if(this.currentView === 'men') {
        return this.menProducts[this.menIndex];
      } else {
        return this.womenProducts[this.womenIndex];
      }
    },

    mainClass() {
      if (!this.currentProduct) {
        return 'Page-Unavailable-product';
      }

      if(this.currentProduct.category === "men's clothing") {
        return 'Page-Men-section';
      } else if (this.currentProduct.category === "women's clothing"){
        return 'Page-Women-section';
      }

      return 'Page-Unavailable-product';
    },

    isUnavailable() {
      return !this.currentProduct;
    }
  },

  methods: {
    async fetchAllProduct() {
      this.isLoading = true;

      try {
        const products = [];
        for (let i = 1; i <= 20; i++) {
          const response = await fetch(`https://fakestoreapi.com/products/${i}`);
          const data = await response.json();
          products.push(data);
        }

        this.allProducts = products;
        this.menProducts = products.filter(p => p.category === "men's clothing");
        this.womenProducts = products.filter(p => p.category === "women's clothing");
      } catch (error) {
        console.error("Error fetching all products:", error);
      } finally {
        this.isLoading = false;
      }

    },
    
    switchView(view) {
      this.currentView = view;
    },

    nextProduct() {
      this.isLoading = true;

      setTimeout(() => {
        if (this.currentView === 'men') {
          this.menIndex = (this.menIndex + 1) % this.menProducts.length;
        } else {
          this.womenIndex = (this.womenIndex + 1) % this.womenProducts.length;
        }

        this.isLoading = false;
      }, 500)
    }
  },

  mounted() {
    this.fetchAllProduct();
  }
}
</script>

<template>
  <div>
    <div class="category-switcher">
      <button 
        @click="switchView('men')" 
        :class="{ active: currentView === 'men' }"
      >
        Men's Clothing
      </button>
      <button 
        @click="switchView('women')" 
        :class="{ active: currentView === 'women' }"
      >
        Women's Clothing
      </button>
    </div>

    <div :class="mainClass">
      
      <div v-if="isLoading" class="skeleton-card">
        <div class="skeleton-line skeleton-title"></div>
        <div class="skeleton-line skeleton-category"></div>
        <div class="skeleton-line skeleton-image"></div>
        <div class="skeleton-line skeleton-description-line"></div>
        <div class="skeleton-line skeleton-description-line"></div>
        <div class="skeleton-line skeleton-description-line"></div>
        <div class="skeleton-line skeleton-price"></div>
        <div class="skeleton-line skeleton-button"></div>
        <div class="skeleton-line skeleton-button"></div> 
      </div>
      <div v-else-if="isUnavailable" class="product-card Page-Unavailable-product">
        <button class="next-button" @click="nextProduct">Next Product</button>
      </div>

      <div v-else class="product-card">
        <h2>{{ currentProduct.title }}</h2>
        <p>Category: {{ currentProduct.category }}</p>
        <img :src="currentProduct.image" :alt="currentProduct.title" class="product-image">
        <p>{{ currentProduct.description }}</p>
        <h3>${{ currentProduct.price }}</h3>
        
        <button class="buy-button">Buy Now</button>
        <button class="next-button" @click="nextProduct">Next Product</button>
      </div>

    </div>
  </div>
</template>