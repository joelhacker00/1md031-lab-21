<template>



  <div class="rad">
    <Burger v-for = "burger in BurgarsArray"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-bind:src="burger.image"/>

    <div class="burbox">

      <h2>{{ burger.name }}</h2>

      <img v-bind:src="burger.image" style="width: 400px;height: 300px" >
      <ul>
        <li>{{ burger.kCal}} kCal</li>
        <li>{{ burger.calories }}</li>
        <li v-if="burger.kött"> Innehåller <span class="alogener"> KÖTT</span></li>
        <li v-if="burger.gluten"> Innehåller <span class="alogener">gluten </span></li>
        <li v-if="burger.lactose"> Innehåller <span class="alogener">lactose</span></li>

      </ul>

      <div>
        <button class="burbutton" v-on:click="addera">
          <img id ="plus" src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Ambox_emblem_plus.svg/1200px-Ambox_emblem_plus.svg.png">
        </button>

          <button class ="burbutton" v-on:click="ta_bort">
            <img id ="minus" src="https://www.pngall.com/wp-content/uploads/5/Red-Minus-PNG-Image.png">
          </button>


      <p> antal:{{ amountOrdered }} </p>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    addera: function () {
      this.amountOrdered ++;
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    },

    ta_bort: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', {
              name: this.burger.name,
              amount: this.amountOrdered
            }
        );
      }
    }
  }
}
</script>

<style scoped>

.burbox{
  padding: 5px;
  margin: 10px;

}
.alogener{
  text-transform: uppercase; font-weight: bold;  color: #8B0000;
}
.rad{
  display: grid;
  padding: 5px;
  grid-template-columns: repeat(3, 1fr);
}
.burbutton{

}
#plus{
  height: 20px;
  width: 20px;
}
#minus{
  height: 20px;
  width: 20px;

}

</style>


