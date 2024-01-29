<template>
  <div v-if="isLoading" class="container">
    <div class="product-container">
      <div class="product-image skeleton">image</div>
      <div class="product-content">
        <div>
          <h3 class="product-title skeleton">title</h3>
          <div class="product-rating skeleton">rating</div>
          <hr>
          <p class="product-description skeleton">description</p>
        </div>
        <div>
          <hr>
          <p class="product-price skeleton">price</p>
          <div class="product-action">
            <button class="primary-button skeleton">Buy Now</button>
            <button class="secondary-button skeleton">Next Product</button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div 
    v-else
    class="container"
    :class="
      !isProductAvailable
      ? 'bg-light-grey'
      : data.product.category === 'men\'s clothing'
      ? 'bg-light-blue font-blue'
      : 'bg-light-magenta font-magenta'
    "
  >
    <div v-if="!isProductAvailable" class="unavailable-container">
      <img src="../assets/image/sad-face.png" class="unavailable-img">
      <div class="unavailable-content">
        <p class="unavailable-text">This product is unavailable to show</p>
        <button class="unavailable-button" @click="getProductById()">Next Product</button>
      </div>
    </div>
    <div v-else class="product-container">
      <img :src="data.product.image" class="product-image" alt="product-image" />
      <div class="product-content">
        <div>
          <h3 class="product-title">{{ data.product.title }}</h3>
          <div style="display: flex; justify-content: space-between; align-items: center; font-size: 18px;">
            <p class="product-category">{{ data.product.category }}</p>
            <div class="product-rating">
              <span>{{ data.product.rating.rate }}</span>
              <div class="dots">
                <span 
                  v-for="dot in 5"
                  :key="dot"
                  :class="
                    data.product.category === 'men\'s clothing'
                    ? ['dot', { 'dot-blue': dot <= Math.round(data.product.rating.rate) }]
                    : ['dot', { 'dot-magenta': dot <= Math.round(data.product.rating.rate) }]
                  "
                >
                </span>
              </div>
            </div>
          </div>
          <hr>
          <textarea class="product-description" disabled>{{ data.product.description }}</textarea>
        </div>
        <div>
          <hr>
          <p class="product-price">${{ data.product.price }}</p>
          <div class="product-action">
            <button 
              class="primary-button" 
              :class="data.product.category === 'men\'s clothing' ? 'primary-btn-blue' : 'primary-btn-magenta'"
            >
              Buy Now
            </button>
            <button 
              class="secondary-button" 
              :class="data.product.category === 'men\'s clothing' ? 'secondary-btn-blue' : 'secondary-btn-magenta'"
              @click="getProductById()"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductDisplay',
  data () {
    return {
      data: {},
      index: 0,
      isLoading: false,
      isProductAvailable: false
    }
  },
  methods: {
    async callAPI() {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
      const result = await response.json();

      return result;
    },
    
    async getProductById() {
      this.isLoading = true;
      this.index == 20 ? (this.index = 1) : this.index++;

      let product = await this.callAPI();

      if(product.category === "men's clothing" || product.category === "women's clothing") {
        this.data = { product };
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }
      this.isLoading = false;
    },
  },

  created() {
    this.getProductById();
  }
}
</script>
<style>
@import '../assets/style/page.css';
</style>