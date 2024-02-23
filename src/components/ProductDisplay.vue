<template>
  <div class="bg" :class="bgClass"></div>
  <div v-if="dataProduct">
    <div class="container product-container">
      <img :src="dataProduct.image" alt="Product Image" />
      <div class="data-product">
        <div>
          <h3 class="title men-color">{{ dataProduct.title }}</h3>
          <div class="rating-container">
            <p class="category">{{ dataProduct.category }}</p>
            <div>
              <p class="rating">{{ dataProduct.rating.rate }}/5</p>
            </div>
          </div>
          <p class="description">{{ dataProduct.description }}</p>
        </div>
        <div class="bottom">
          <h4 class="price men-color">${{ dataProduct.price }}</h4>
          <div class="button-container">
            <button class="buy-btn men-background men-border-color">
              Buy Now
            </button>
            <button
              class="next-btn men-border-color men-color"
              @click="increment"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div class="container unavailable-container">
      <p>This product is unavailable to show</p>
      <button @click="increment">Next Product</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1,
      dataProduct: null,
    };
  },
  computed: {
    bgClass() {
      if (this.dataProduct) {
        return this.dataProduct.category.includes("women")
          ? "pattern women"
          : "pattern men";
      }
      return "unavailable";
    },
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.index}`
        );
        const data = await response.json();
        data.category.includes("clothing")
          ? (this.dataProduct = data)
          : (this.dataProduct = null);
      } catch (error) {
        console.error(`Error fetcing data: ${error}`);
      }
    },
    increment() {
      this.index == 20 ? (this.index = 1) : this.index++;
      this.fetchData();
    },
  },
  beforeMount() {
    this.fetchData();
  },
};
</script>

<style scoped>
.bg {
  height: 548px;
}

.pattern {
  background-image: url(../assets/pattern.svg);
  background-repeat: no-repeat;
  background-size: cover;
}

.women {
  background-color: var(--light-pink);
}

.men {
  background-color: var(--light-blue);
}

.unavailable {
  background-color: var(--grey);
}

.container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;

  width: 1034px;
  height: 580px;

  background-color: #fff;
  border-radius: 10px;
  margin: 125px auto;
}

.product-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 68px;

  box-shadow: 2px 4px 21px rgba(0, 0, 0, 0.2), 0px 4px 4px rgba(0, 0, 0, 0.25);
  padding: 50px 56px 46px;
}

.product-container img {
  width: 300px;
}

.product-container .data-product {
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  height: 100%;
}

.data-product .title {
  font-weight: 600;
  font-size: 28px;
  line-height: 34px;

  margin-bottom: 17px;
}

.data-product .rating-container {
  display: flex;
  justify-content: space-between;
  align-items: center;

  font-weight: 400;
  font-size: 18px;
  color: var(--dark-brown);

  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  margin-bottom: 21px;
  padding-bottom: 11px;
}

.data-product .description {
  font-weight: 400;
  font-size: 20px;
  line-height: 24px;
  color: var(--dark);
}

.data-product .bottom {
  padding-top: 16px;
  border-top: 1px solid rgba(0, 0, 0, 0.2);
}

.bottom .price {
  font-weight: 600;
  font-size: 28px;
  line-height: 34px;

  margin-bottom: 15px;
}

.bottom .button-container {
  display: flex;
  justify-content: space-between;
}

.button-container .buy-btn {
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;
  color: #fff;

  border-radius: 4px;
  padding: 9px 88px;
}

.button-container .next-btn {
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;

  border-radius: 4px;
  border: 3px solid;
  padding: 9px 66px;
}

.men-color {
  color: var(--dark-blue);
}

.men-background {
  background-color: var(--dark-blue);
}

.men-border-color {
  border-color: var(--dark-blue);
}

.unavailable-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 13px;

  background-image: url(../assets/sad-face.png);
  background-repeat: no-repeat;
  background-position: 65px center;
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25);
}

.unavailable-container p {
  font-weight: 400;
  font-size: 20px;
  line-height: 24px;
}

.unavailable-container button {
  font-weight: 600;
  font-size: 20px;
  color: var(--dark-brown);

  background-color: transparent;
  border: 3px solid #000;
  border-radius: 4px;
  padding: 9px 169px;

  cursor: pointer;
}
</style>
