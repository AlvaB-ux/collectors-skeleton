<template>
  <div class = "itemPool">
    <div class= "titleItemPool" > Item Pool</div>

   <div class="buttons" v-for="(p, placementId) in placement" :key="'Item Button'+placementId">
     <button id="oneCoinButton"
       v-if="p.playerId===null && p.cost===1 && p.placementId === 0"
       :disabled="cannotAfford(p.cost)"
       @click="placeBottle(p)">
       <div class = "ItemBottleCoinOne">
       </div>
      </button>

      <button id="oneCoinButton"
        v-if="p.playerId===null && p.cost===1 && p.placementId === 1"
        :disabled="cannotAfford(p.cost)"
        @click="placeBottle(p)">
        <div class = "ItemBottleCoinOne">
        </div>
       </button>

      <button id="twoCoinButton"
        v-if="p.playerId===null && p.cost===2 && p.placementId === 2"
        :disabled="cannotAfford(p.cost)"
        @click="placeBottle(p)">
        <div class = "ItemBottleCoinTwo">
        </div>
     </button>

     <button id="twoCoinButton"
       v-if="p.playerId===null && p.cost===2 && p.placementId === 3"
       :disabled="cannotAfford(p.cost)"
       @click="placeBottle(p)">
       <div class = "ItemBottleCoinTwo">
       </div>
    </button>

     <button id="threeCoinButton"
       v-if="p.playerId===null && p.cost===3 && p.placementId === 4"
       :disabled="cannotAfford(p.cost)"
       @click="placeBottle(p)">
       <div class = "ItemBottleCoinThree">
       </div>
    </button>
   </div>

     <div class="itemCard" v-for="(card, index) in itemsOnSale" :key="'Item Card'+index">
       <CollectorsCard
         :card="card"
         :availableAction="card.available"
         @doAction="buyCard(card)"/>
     </div>
  </div>

</template>
<script>
import CollectorsCard from '@/components/CollectorsCard.vue'
export default {
  name: 'CollectorsBuyItem',
  components: {
    CollectorsCard
  },
  props: {
    labels: Object,
    player: Object,
    itemsOnSale: Array,
    marketValues: Object,
    placement: Array
  },
  methods: {
    cannotAfford: function (cost) {
      let minCost = 100;
      for(let key in this.marketValues) {
        if (cost + this.marketValues[key] < minCost)
          minCost = cost + this.marketValues[key]
      }
      return (this.player.money < minCost);
    },
    cardCost: function (card) {
      return this.marketValues[card.market];
    },
    placeBottle: function (p) {
      if(this.player.playerBottles>0){
      this.$emit('placeBottle', p);
      this.highlightAvailableCards(p.cost);
    }
    },
    highlightAvailableCards: function (cost=100) {
      for (let i = 0; i < this.itemsOnSale.length; i += 1) {
        if (this.marketValues[this.itemsOnSale[i].item] <= this.player.money - cost) {
          this.$set(this.itemsOnSale[i], "available", true);
        }
        else {
          this.$set(this.itemsOnSale[i], "available", false);
        }
      }
      for (let i = 0; i < this.player.hand.length; i += 1) {
        if (this.marketValues[this.player.hand[i].item] <= this.player.money - cost) {
          this.$set(this.player.hand[i], "available", true);
        }
        else {
          this.$set(this.player.hand[i], "available", false);
        }
        this.chosenPlacementCost = cost;
      }
    },
    buyCard: function (card) {
      if (card.available) {
        this.$emit('buyCard', card);
        this.highlightAvailableCards();
      }
     }
    }
   }
</script>

<style scoped>
.titleItemPool {
  grid-column: 6 / span 2;
  font-style: italic;
  font-size: 50px;
  text-shadow: 2px 2px 4px red;
  font-size: 20px;
}
.itemPool{
  grid-column: 3/span 5 ;
  grid-row: 2/span 4;
  width: auto;
  height: auto;
  background-color: #f0d9cc ;
  color: black;
  display: grid;
  grid-template-columns: repeat(6, 50px);
  grid-template-rows: repeat(3,50px);
  grid-column-gap: 25px;
  grid-auto-flow: row;
  padding-left: 10px;
  padding-top: 10px;
  padding-bottom: 10px;
  border-top: 2px solid #4C7B80;
}
.buttons{
  grid-row: 1;
  place-self: stretch;
  background-color: #f0d9cc;
}
.buttons div:hover {
  transform: scale(1.5)translate(0,0);
  z-index: 1;
}
.itemCard {
  grid-row: 2;
  transform: scale(0.25);
}
.itemCard div:hover{
  transform: scale(1.5)translate(-25%,0);
  z-index: 1;
}
.ItemBottleCoinOne{
  width:50px;
  height:50px;
  background-image:  url('/images/item-bottle-coin-one.png');
  background-size: cover;
}
#oneCoinButton {
  background-color: #f9dcce;
  border-radius: 5px;
  border: 2px solid #E3A688;
}
#twoCoinButton {
  background-color: #f9dcce;
  border-radius: 5px;
  border: 2px solid #E3A688;
}
#threeCoinButton {
  background-color: #f9dcce;
  border-radius: 5px;
  border: 2px solid #E3A688;
}
.ItemBottleCoinTwo{
  width:50px;
  height:50px;
  background-image:  url('/images/item-bottle-coin-two.png');
  background-size: cover;
}
.ItemBottleCoinThree{
  width:50px;
  height:50px;
  background-image:  url('/images/item-bottle-coin-three.png');
  background-size: cover;
}

@media screen and (max-width: 800px) {
.titleItemPool {
    grid-column: 6;
    grid-row: 3;
    place-self: end;
    font-style: italic;
    font-size: 50px;
    text-shadow: 2px 2px 4px red;
    font-size: 20px;
  }
  .itemPool{
    grid-column: 3/span 5 ;
    grid-row: 5/span 4;
    width: auto;
    height: auto;
    grid-template-rows: repeat(100,150px);
    background-color: #f0d9cc ;
    color: black;
    display: grid;
    grid-template-rows: repeat(3,50px);
    grid-column-gap: 15px;
    grid-auto-flow: row;
    padding-top: 10px;
    padding-bottom: 10px;
    border-top: 2px solid #4C7B80;
    border-right: 2px solid #4C7B80;
    grid-template-columns: repeat(auto-fit, minmax(-100px, 1fr));
}
  .itemCard {
    grid-row: 2;
    transform: scale(0.25);
  }
}
</style>
