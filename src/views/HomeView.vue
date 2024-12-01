<template>

    <header class="header-content">
            <img src="https://qul.imgix.net/ca169046-430e-4b8d-b218-3f0f73c446be/625936_sld.jpg" alt="Bildbeskrivning" class="header-image">
        <h1>
            Välkomna till Victors Burgare!
        </h1>
    </header>
    
      
        <main>
            
            <section id="meny">
                <h2>
                    Meny
                </h2>
                <p>
                    Här är de burgare vi serverar hos Victors Burgare:
                </p>
                

                    <div id="burgare-grid">
                    
                        <div id="burger-list">
                                <Burger v-for="burger in burgers"
                                v-bind:burger="burger" 
                                v-bind:key="burger.name"
                                v-on:orderedBurger="addToOrder($event)"/>
                        </div>
                    </div>
                                    
                
            </section>
        
            <section id="Omdinbeställning">
                <h2>
                    Om din beställning
                </h2>
                <p>
                    Vi finns på Lägerhyddsvägen 1 men erbjuder även hemkörning direkt till din dörr
                </p>

                <h3>Leveransinformation</h3>
              
                    <p>
                        <label for="Namn">Fullständigt namn</label>
                    </p>
                    <p>
                        <input type="text" id="Namn" required = "required" v-model="Namn" placeholder="För och efternamn">
                    </p>

                    <p>
                        <label for="Email">E-mail</label>
                    </p>
                    <p>
                        <input type="email" id="Email" required = "required" v-model="Email" placeholder="E-mail address">
                    </p>

                    <!-- Betalning -->
                    <p>
                        <label for="betalning">Betalning:</label>
                    </p>
                    <p>
                        <select id="betalning" v-model="betalning">
                            <option value="credit-kort">Kredit-kort</option>
                            <option value="paypal">PayPal</option>
                            <option value="swish">Swish</option>
                        </select>
                    </p>

                    <!-- Kön -->
                    <p>
                        Kön:
                    </p>
                    <p>
                        <input type="radio" id="man" v-model="kön" value="man">
                        <label for="man">Man</label>
                    </p>
                    <p>
                        <input type="radio" id="kvinna" v-model="kön" value="kvinna">
                        <label for="kvinna">Kvinna</label>
                    </p>
                    <p>
                        <input type="radio" id="icke-binär" v-model="kön" value="icke-binär">
                        <label for="icke-binär">Icke-binär</label>
                    </p>
                    <p>
                        <input type="radio" id="annat" v-model="kön" value="annat" checked>
                        <label for="annat">Annat</label>
                    </p>
            <h3> 
                Vart ska maten levereras? Klicka på kartan nedan för att välja plats:
            </h3>           
        <div id="map-container">
                <div id="map" @click="setLocation">
                    <div
                        v-if="clickPosition.x !== 0 || clickPosition.y !== 0"
                        :style="{
                        position: 'absolute',
                        top: clickPosition.y + 'px',
                        left: clickPosition.x + 'px',
                        color: 'red',
                        fontSize: '20px',
                        fontWeight: 'bold',
                        transform: 'translate(-50%, -50%)',
                            }" >T
                    </div>
                </div>
         </div>
            </section>                    
                    <!-- Beställnings-knapp-->
                    <p>
                      <button type="button" v-on:click="placeOrder">
                          <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQqp5219Jpz8IY-VXNY560a59SWBZf8P1Mu9Q&s" alt="checkbox" height="15">
                         Beställ
                      </button>
                    </p>
        </main>

        <footer>
            <hr>
            <p>
                &copy; 2024 Victors Burgare.
            </p>
        </footer>   

</template>

<script>
    import menu from "../assets/menu.json";
    import Burger from "../components/OneBurger.vue";
    import io from "socket.io-client";

    const socket = io("localhost:3000");

function menuitems(name, url, kCal, lactose, gluten) {
  this.name = name;
  this.url = url;
  this.kCal = kCal;
  this.lactose = lactose;
  this.gluten = gluten;
}

export default {
  name: "HomeView",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu,
      Namn: "", 
      Email: "", 
      betalning: "credit-kort", 
      kön: "annat", 
      orderedBurgers: {}, 
      clickPosition: { x: 0, y: 0 }, 
    };
  },
  methods: {

    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    },
    getOrderNumber() {
      return Math.floor(Math.random() * 100000);
    },


    setLocation(event) {
      const offset = event.currentTarget.getBoundingClientRect();
      this.clickPosition = {
        x: event.clientX - offset.left,
        y: event.clientY - offset.top,
      };
    },
   
    placeOrder() {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          position: this.clickPosition,
          customer: {
            Namn: this.Namn,
            Email: this.Email,
            betalning: this.betalning,
            kön: this.kön,
            orderItems: this.orderedBurgers,
          },
        },
        
      });
     
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-size: 12pt;
    font-family: arial;
}
main, header, footer, nav ul {
    max-width: 85rem;
    margin: 0 auto 0 auto;
}
.allergi {
    font-weight: bold;
   
}

#meny {
    background-color: black; 
    color: white; 
    padding: 20px; 
    margin: 20px;
    border: 3px dashed white;
}
#meny p {
    color: white;
}
#meny img {
    margin: 30px; 
}

#Omdinbeställning {
    border: 3px dashed black; 
    padding: 10px; 
    margin: 20px; 
}

header {
    
    background-size: cover;
    overflow: hidden;
    width: 100%;
    height: 200px;
    opacity: 0.5;
}
header h1 {
    width:40rem;
    margin: 0 auto;
    text-align: center;
}
button:hover {
    background-color:chartreuse;
    cursor: pointer;
 }
 section {
    margin: 10px; 
}
button {
    margin-top: 5px;
    margin: 20px;
}
.header-content {
    position: relative;
    max-width: 82rem;
    height: 200px; 
    overflow: hidden;
}
.header-image {
    width: 100%;
    height: auto;
    opacity: 0.5;
}
.header-content h1 {
    position: absolute;
    top: 50px; 
    left: 350px; 
    color:black;
    font-size: 3rem;
    padding: 10px;
}
#burgare-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px; 
    padding: 20px; 
}
#burger-list {
    display: grid;
    grid-template-columns: repeat(3, 1fr); 
    gap: 20px; 
    padding: 20px; 
}  
#map-container {
  position: relative; 
  width: 800px;
  height: 400px;
  overflow: scroll; 
  border: 1px solid black;
}
#map {
  position: relative; 
  width: 1920px;
  height: 1078px;
  background: url('/img/polacks.jpg') no-repeat center center;
  background-size: cover;
}

</style>