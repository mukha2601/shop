<template>
  <teleport to="#modal">
    <div class="modal-box">
      <div class="modal">
        <button @click="closeModal" class="closeBtn">x</button>
        <div class="card" v-for="product in locStorArray" :key="product.id">
          <img
            :src="product.image"
            alt=""
            :key="product.id"
            width="300"
            height="300"
          />
          <h1>{{ product.name }}</h1>
          <p>
            <del>{{ product.old_price }}</del> | {{ product.price }} UZS
          </p>
          <div class="card-input">
            <button @click="decrement(product)">-</button>
            <div>
              <input
                type="number"
                :value="product.count"
                min="0"
                @input="updateCount(product, $event)"
              />
            </div>
            <!-- <button @click="updateProduct()">ok</button> -->
            <button @click="increment(product)">+</button>
          </div>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script>
export default {
  props: {
    locStorArray: {
      type: Array,
    },
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
    increment(product) {
      product.count++;
      this.updateLocalStorage();
    },
    decrement(product) {
      if (product.count > 0) {
        product.count--;
        this.updateLocalStorage();
      }
    },
    updateProduct() {
      this.updateLocalStorage(); // Lokal saqlash joyini yangilaydi
    },
    updateCount(product, event) {
      const newValue = event.target.value; // Kiritilgan qiymatni butun son qilib olish
      let decimalPart = newValue.toString().slice(0, 3);

      product.count = +decimalPart; // Qiymatni o'zgartirish
      this.updateLocalStorage(); // LocalStorage'ni yangilash
    },

    updateProduct() {
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      const updatedArray = JSON.stringify(this.locStorArray);
      localStorage.setItem("products", updatedArray);
    },
  },
  emits: ["closeModal"],
};
</script>

<style scoped>
.card {
  border: 2px solid black;
  padding: 5px;
  margin-top: 10px;
}
.modal-box {
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  min-width: 400px;
  height: 90%;
  display: flex;
  flex-direction: column;
  padding: 50px;
  background-color: bisque;
  position: relative;
  overflow-y: scroll;
}

.closeBtn {
  position: absolute;
  top: 0;
  right: 0;
  padding: 5px 10px;
}

.card-input {
  display: flex;
}
</style>
