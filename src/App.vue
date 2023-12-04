<template>
  <div :class="getClassForCategory">
    <div class="background">
      <div class="bg-product">
        <div class="unavailable">
          <img src="https://svgur.com/i/10Pi.svg" />
          <div class="unavailable-content">
            <p>This product is unavailable to show</p>
            <button class="next" @click="getNextProduct">Next product</button>
          </div>
        </div>
        <div class="container-info">
          <div class="image">
            <img :src="product.image" :alt="product.title" />
          </div>
          <div class="product-info">
            <div class="atas">
              <h2>{{ product.title }}</h2>
              <div class="category-rate">
                <p>{{ product.category }}</p>
                <div class="rating-rate">
                  <div class="rate">
                    <p>{{ rating.rate }}/5</p>
                  </div>
                  <svg
                    v-for="(circle, index) in 5"
                    :key="index"
                    xmlns="http://www.w3.org/2000/svg"
                    width="18"
                    height="18"
                    viewBox="0 0 18 18"
                    fill="none"
                  >
                    <circle
                      :cx="9"
                      :cy="9"
                      :r="9"
                      :fill="getCircleColor(index)"
                    />
                  </svg>
                </div>
              </div>
              <div class="hr1"></div>
            </div>
            <div class="desc">
              <p>{{ product.description }}</p>
            </div>
            <div class="bawah">
              <div class="hr2"></div>
              <p class="price">${{ product.price }}</p>
              <div class="button">
                <button class="buy" @click="getNextProduct">Buy now</button>
                <button class="next" @click="getNextProduct">
                  Next product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
      rating: {},
      currentIndex: 1,
    };
  },
  methods: {
    async getProductData(index) {
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${index}`
        );
        const data = await response.json();
        this.product = data;
        this.rating = data.rating;
      } catch (error) {
        console.error("Error fetching product data:", error);
      }
    },
    getNextProduct() {
      this.currentIndex++;
      if (this.currentIndex > 20) {
        this.currentIndex = 1;
      }
      this.getProductData(this.currentIndex);
    },
    getCircleColor(index) {
      if (index < this.rating.rate) {
        if (this.product.category === "men's clothing") {
          return "var(--color1)"; // Warna untuk kategori "man"
        } else if (this.product.category === "women's clothing") {
          return "var(--color4)"; // Warna untuk kategori "woman"
        }
      } else {
        return "transparent"; // Untuk lingkaran yang tidak diisi
      }
    },
  },

  computed: {
    getClassForCategory() {
      if (this.product.category === "men's clothing") {
        return "page-men";
      } else if (this.product.category === "women's clothing") {
        return "page-women";
      } else {
        return "page-unavailable-product";
      }
    },
  },
  mounted() {
    this.getProductData(this.currentIndex);
  },
};
</script>

<style>
@import "./assets/style/page.css";
</style>
