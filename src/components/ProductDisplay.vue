<template>
  <div class="bg" :class="bgClass"></div>
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
</style>
