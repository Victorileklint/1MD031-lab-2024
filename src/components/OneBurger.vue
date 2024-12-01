<template>
  
  <div class="burgare">
     <h3>
        {{ burger.name }}
     </h3>
     <img :src="burger.url" :alt="'Bild av ' + burger.name" height="300" />
     <ul>
        <li>{{ burger.kCal }} kCal</li>
        <li v-if="burger.lactose">Innehåller <span class="allergi">laktos</span></li>
        <li v-if="burger.gluten">Innehåller <span class="allergi">gluten</span></li>
     </ul>
          <p>
            Antal beställt: {{ amountOrdered }}
          </p>
           <button @click="increaseOrder"> + </button>
           <button @click="decreaseOrder"> - </button>
  </div>

</template>

<script>
  export default {
    name: "OneBurger",
    props: {
      burger: Object,
    },
    data: function () {
      return {
        amountOrdered: 0,
      };
    },
    methods: {
      increaseOrder() {
        this.amountOrdered++;
        this.$emit("orderedBurger", { name: this.burger.name, amount: this.amountOrdered });
      },
      decreaseOrder() {
        if (this.amountOrdered > 0) {
          this.amountOrdered--;
          this.$emit("orderedBurger", { name: this.burger.name, amount: this.amountOrdered });
        }
      },
    },
  };
</script>



<style scoped>
  .allergi {
      font-weight: bold;
    
  }
  #meny img {
      margin: 30px; 
  }
  .burgare{
      display: grid;
  }

</style>