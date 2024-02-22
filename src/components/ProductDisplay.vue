<template>
  <div class="bg" :class="bgClass"></div>
  <div v-if="dataProduct">
    <div class="container">
      <button @click="increment">Next Product</button>
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

.unavailable-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 13px;

  background-image: url(../assets/sad-face.png);
  background-repeat: no-repeat;
  background-position: 65px center;
  box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.25);
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
