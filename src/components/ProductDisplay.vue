<template>
  <div class="background" :class="backgroundClass"></div>
  <div v-if="dataProduct">
    <div class="container product-container flex-align-center">
      <img :src="dataProduct.image" alt="Product Image" />
      <div class="data-product" :class="genderClass">
        <div>
          <h3 class="title">{{ dataProduct.title }}</h3>
          <div class="rating-container flex-align-center">
            <p class="category">{{ dataProduct.category }}</p>
            <div class="rating-point flex-align-center">
              <p class="rating">{{ dataProduct.rating.rate }}/5</p>
              <div class="rating-bullet-container flex-align-center">
                <div class="rating-bullet" v-for="i in 5"></div>
              </div>
            </div>
          </div>
          <p class="description">{{ dataProduct.description }}</p>
        </div>
        <div class="bottom">
          <h4 class="price">${{ dataProduct.price }}</h4>
          <div class="button-container">
            <button class="buy-btn btn">Buy Now</button>
            <button
              class="next-btn btn men-border-color men-color"
              @click="nextProduct"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div class="container unavailable-container flex-align-center">
      <p>This product is unavailable to show</p>
      <button class="btn" @click="nextProduct">Next Product</button>
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
    backgroundClass() {
      if (this.dataProduct) {
        return this.dataProduct.category.includes("women")
          ? "pattern light-pink"
          : "pattern light-blue";
      }
      return "unavailable";
    },
    genderClass() {
      if (this.dataProduct) {
        return this.dataProduct.category.includes("women") ? "women" : "men";
      }
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
    nextProduct() {
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
.btn {
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;

  border-radius: 4px;

  cursor: pointer;
}

.flex-align-center {
  display: flex;
  align-items: center;
}

.container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;

  width: 1034px;
  height: 580px;

  background-color: var(--white);
  border-radius: 10px;
  margin: 125px auto;
}
.background {
  height: 548px;
}

.pattern {
  background-image: url(../assets/pattern.svg);
  background-repeat: no-repeat;
  background-size: cover;
}

.light-pink {
  background-color: var(--light-pink);
}

.light-blue {
  background-color: var(--light-blue);
}

.unavailable {
  background-color: var(--grey);
}

.product-container {
  justify-content: space-between;
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

.men .title {
  color: var(--dark-blue);
}

.women .title {
  color: var(--purple);
}

.data-product .rating-container {
  justify-content: space-between;

  font-weight: 400;
  font-size: 18px;
  color: var(--dark-brown);

  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  margin-bottom: 21px;
  padding-bottom: 11px;
}

.rating-container .rating-point {
  gap: 5px;
}

.rating-point .rating-bullet-container {
  gap: 2px;
}

.rating-point .rating-bullet {
  width: 18px;
  height: 18px;

  border-radius: 50%;
  background-color: var(--white);
}

.men .rating-point .rating-bullet {
  border: 1px solid var(--dark-blue);
}

.women .rating-point .rating-bullet {
  border: 1px solid var(--purple);
}

.data-product .description {
  display: -webkit-box;
  -webkit-line-clamp: 8;
  -webkit-box-orient: vertical;

  font-weight: 400;
  font-size: 20px;
  line-height: 24px;
  color: var(--dark);

  overflow: hidden;
}

.data-product .bottom {
  border-top: 1px solid rgba(0, 0, 0, 0.2);
  margin-top: 21px;
  padding-top: 16px;
}

.bottom .price {
  font-weight: 600;
  font-size: 28px;
  line-height: 34px;

  margin-bottom: 15px;
}

.men .bottom .price {
  color: var(--dark-blue);
}

.women .bottom .price {
  color: var(--purple);
}

.bottom .button-container {
  display: flex;
  justify-content: space-between;
}

.button-container .buy-btn {
  color: var(--white);

  padding: 9px 88px;
}

.men .button-container .buy-btn {
  background-color: var(--dark-blue);
}

.women .button-container .buy-btn {
  background-color: var(--purple);
}

.button-container .next-btn {
  background-color: transparent;

  border: 3px solid;
  padding: 9px 66px;
}

.men .button-container .next-btn {
  color: var(--dark-blue);
  border-color: var(--dark-blue);
}

.women .button-container .next-btn {
  color: var(--purple);
  border-color: var(--purple);
}

.unavailable-container {
  flex-direction: column;
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
  color: var(--dark-brown);

  background-color: transparent;
  border: 3px solid #000;
  padding: 9px 169px;
}
</style>
