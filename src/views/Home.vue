<template>

  <header id="sidhuvud">
    <h1>Hej och välkommen till Joels resurang</h1>
    <img src="https://media1.boostcafe.se/2020/01/rcvud6u47ju0vzidtsrsla-e1579624522887.jpg" id="sidhuvudbild"  >
  </header>

  <main>
    <section id="sektion1">
      <div id="rubrik2">
        <h2> Hungrig? välj mat här!!!</h2>
      </div>

      <div class="rad">
        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>

      </div>
    </section>

    <section id="betalninagsaltenativ" style="clear: left">
      <form>
        <h2>
          Kund infromation
        </h2>
        <p>
          Här skriver du nödvändig information
        </p>
        <h3>
          leverans uppgifter
        </h3>
        <p>
          <label for="namn">Namn</label><br>
          <input type="text" id="namn" v-model="fn" required="required" placeholder="namn">
        </p>
        <p>
          <label for="email">E-mail</label><br>
          <input type="text" id="email" v-model="ln" placeholder="email">
        </p>

        <p>
          <label for="betalninagsaltenativ">Betalninagsaltenativ</label><be></be><br>
          <select id="betal" v-model="rcp">
            <option>Kort</option>
            <option>Swich</option>
            <option>klarna</option>
            <option>paypal</option>
            <option>kach</option>
          </select><br>
        </p>



        <p>KÖN:</p>
        <input type="radio" id="man" v-model="gender" value="man">
        <label for="man">Man</label><br>
        <input type="radio" id="Kvinna" v-model="gender" value="kvinna">
        <label for="Kvinna">Kvinna</label><br>
        <input type="radio" id="vill inte svara" v-model="gender" value="Vill inte svara">
        <label for="vill inte svara">Vill inte svara</label><br>
      </form><br>
      <p> Välj din adress i kartan nedan</p>
      <div id="maphanteraren">

      <div id="map" v-on:click="setLocation">
        <div id="punkt">

          <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
            T
          </div>

        </div>
      </div>
    </div>
    </section>

    <button v-on:click=klar type="submit" class="button" >
      <img src="https://image.similarpng.com/very-thumbnail/2020/12/Hamburger-cheeseburger-cartoon-character-on-transparent-background-PNG.png" style="width: 20px">
      <input type="submit" value="skicka">
    </button>
  </main>
  <footer>
    <hr> &copy; 2021 Securitas
  </footer>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();


const BurgarsArray = menu


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: BurgarsArray,
      fn:'',
      ln:'',
      ad:'',
      hs:'',
      rcp:'Kort',
      gender:'Vill inte svara',
      orderedBurgers:{},
      location: { x: 0, y: 0}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    setLocation: function(event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    klar: function (){
      console.log(this.fn)
      console.log(this.ln)
      console.log(this.gender)
      console.log(this.rcp)
      console.log(this.orderedBurgers)

      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x, y: this.location.y,
          fn: this.fn, ln: this.ln,rcp: this.rcp, gender: this.gender
        },
        orderItems: [this.orderedBurgers]
        }
      );
    },
  }
}
</script>

<style>

#maphanteraren {
  width: 600px;
  height: 400px;
  overflow: scroll;
}

#map {
  width:1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
}
#punkt div{
  position: absolute;
  background: black;
  color: beige;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
#punkt {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}
#sidhuvud{
  padding: 5px;
  margin: 5px 10px;
  border-radius: 25px;

}
#sidhuvudbild{
  opacity: 0.5;
  width: 100%;
  height: 250px;
}

h1{
  position: absolute;
  padding-left: 350px;

}
#rubrik2{
   padding-left: 475px;
 }
body {
  font-family: 'Times New Roman', sans-serif;
  font-size: 23px;
  font-style: italic;
}

#sektion1{
  background-color: black;
  color: beige;
  padding: 5px;
  margin: 5px 10px;
  border: 10px solid #00FFFF;
  border-style: groove;
  border-radius: 25px
}
#betalninagsaltenativ{
  background-color: darkolivegreen;
  color: beige;
  padding: 5px;
  margin: 5px 10px;
  border: 5px solid #DAA520;
  border-style: dotted;
  border-radius: 25px
}

.button:hover {
  background-color: darkblue; cursor:progress;

}

a[href], input[type="submit"], input[type="image"], lable[for], select, button, pointer{
  cursor: pointer;
}

.rad{
  display: grid;
  padding: 5px;
  grid-template-columns: repeat(3, 1fr);
}



</style>
