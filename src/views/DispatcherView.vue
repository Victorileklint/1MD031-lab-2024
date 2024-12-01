<template>
  <div id="orders">
    <div id="orderList">
      <h2>
        Orderlista
      </h2>
      <div v-for="(order, key) in orders" v-bind:key="'order'+key">
            <h3>
              Order #{{ key }}
            </h3>
                <ul>
              <!-- Visa burgare och antal -->
                    <li v-for="(amount, burgerName) in order.details.customer.orderItems" :key="burgerName">
                      {{ burgerName }}: {{ amount }} st
                    </li>
                </ul>
            <p>
              <strong>Kundinformation:</strong>
            </p>
                <ul>
                  <li>Namn: {{ order.details.customer.Namn }}</li>
                  <li>Email: {{ order.details.customer.Email }}</li>
                  <li>Betalningsmetod: {{ order.details.customer.betalning }}</li>
                  <li>Kön: {{ order.details.customer.kön }}</li>
                </ul>
      </div>
      <button v-on:click="clearQueue">Rensa ordrar</button>
    </div>

    <!-- Dots som visas på kartan -->
    <div id="dots">
      <div 
        v-for="(order, key) in orders" 
        v-bind:style="{ left: order.details.position.x + 'px', top: order.details.position.y + 'px' }" 
        v-bind:key="'dots' + key"
        class="dot">
        {{ key }}
      </div>
    </div>
  </div>
</template>

<script>
    import io from 'socket.io-client';
    const socket = io("localhost:3000");

    export default {
      name: 'DispatcherView',
      data() {
        return {
          orders: null,
        };
      },
      created() {
        socket.on("currentQueue", (data) => {
          console.log("Orders från servern:", data.orders);
          this.orders = data.orders;
        });
      },
      methods: {
        clearQueue() {
          socket.emit("clearQueue");
        },
      },
    };
</script>

<style>
#orderList {
  top: 1em;
  left: 1em;
  position: absolute;
  z-index: 2;
  color: black;
  background: rgba(255, 255, 255, 0.5);
  padding: 1em;
  border-radius: 5px;
}
#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
  background-image: url('/img/polacks.jpg');
}
#dots .dot {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
  line-height: 20px;
}
</style>


