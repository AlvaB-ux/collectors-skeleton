<template>
  <div class = "workPool" >
    <div class= "titleWorkPool" >Work Pool</div>
    <div class= "workdeck"> </div>

    <div class="buttons" v-for="(p, workActionId) in placement" :key="'Work Action'+ workActionId">
      <button id="alt1" v-if="p.playerId===null && workActionId === 0"
      :disabled="cannotAfford(p.cost)"
      @click = "placeBottle(p)">
        <div class = "Alt1">
        </div>
      </button>

      <button id="alt2" v-if="p.playerId===null && workActionId === 1 "
      :disabled="cannotAfford(p.cost)"
      @click="placeBottle(p)">
        <div class = "Alt2">
        </div>
      </button>

        <button id="alt3" v-if="p.playerId===null && workActionId === 2"
        :disabled="cannotAfford(p.cost)"
        @click="placeBottle(p)">
          <div class = "Alt3">
          </div>
        </button>

        <button id="alt4" v-if="p.playerId===null && workActionId === 3"
        :disabled="cannotAfford(p.cost)"
        @click="placeBottle(p)">
          <div class = "Alt4">
          </div>
        </button>
    </div>

    <div class = "quarterImage" >
      <button class= "quarter1" v-if="quarterPlacement[0].playerId===null && currentRound === 1"
      :disabled="cannotAfford(quarterPlacement[0].cost)"
      @click = "placeQuarterBottle(quarterPlacement[0])">
      </button>

      <button class= "quarter2" v-if="quarterPlacement[1].playerId===null && currentRound === 2"
      :disabled="cannotAfford(quarterPlacement[1].cost)"
      @click = "placeQuarterBottle(quarterPlacement[1])">
      </button>

      <button class= "quarter3" v-if="quarterPlacement[2].playerId===null && currentRound === 3"
      :disabled="cannotAfford(quarterPlacement[2].cost)"
      @click = "placeQuarterBottle(quarterPlacement[2])">
      </button>

      <button class= "quarter4" v-if="quarterPlacement[3].playerId===null && currentRound === 4"
      :disabled="cannotAfford(quarterPlacement[3].cost)"
      @click = "placeQuarterBottle(quarterPlacement[3])">
      </button>
    </div>
  </div>
</template>
<script>

export default {
  name: 'CollectorsWork',
  components: {
  },
  props: {
    labels: Object,
    player: Object,
    currentRound: Number,
    placement: Array,
    quarterPlacement: Array,
    skillsOnSale: Array,
    marketValues: Object,
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
      this.$emit('placeBottleWork', p);
    },
    placeQuarterBottle: function (p) {
      console.log("nu körs placeQuarterBottle i work"+this.currentRound)
      this.$emit('placeQuarterBottle', p);
    },
    highlightAvailableCards: function (cost=100) {
      for (let i = 0; i < this.skillsOnSale.length; i += 1) {
        if (this.marketValues[this.skillsOnSale[i].item] <= this.player.money - cost) {
          this.$set(this.skillsOnSale[i], "available", true);
        }
        else {
          this.$set(this.skillsOnSale[i], "available", false);
        }
        this.chosenPlacementCost = cost;
      }
    },
    drawCard: function (card) {
      if (card.available) {
        this.$emit('drawCard', card);
     }
    }
   }
  }
</script>

<style scoped>
.workPool{
  grid-column: 3/span 5;
  grid-row: 6/span 5;
  width: auto;
  height: auto;
  display:grid;
  grid-template-columns: repeat(3, 149px);
  grid-template-rows: repeat(4,55px);
  grid-auto-flow: column;
  background-color: #f5f2cc;
  color: black;
  border-top: 2px solid #4C7B80;
}
.titleWorkPool {
  font-style: italic;
  font-size: 50px;
  text-shadow: 2px 2px 4px yellow;
  font-size: 20px;
  grid-column: 2;
}
.quarterImage {
  grid-column: 3;
  grid-row: 2;
}
.quarter1 {
  grid-column: 2 ;
  grid-row: 1;
  width: 140px;
  height: 62px;
  background-image: url('/images/quarterTile1.png');
  background-size: cover;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.quarter2 {
  grid-column: 2 ;
  grid-row: 1;
  width: 140px;
  height: 62px;
  background-image: url('/images/quarterTile2.png');
  background-size: cover;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.quarter3 {
  grid-column: 2 ;
  grid-row: 1;
  width: 140px;
  height: 62px;
  background-image: url('/images/quarterTile3.png');
  background-size: cover;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.quarter4 {
  grid-column: 2 ;
  grid-row: 1;
  width: 140px;
  height: 62px;
  background-image: url('/images/quarterTile4.png');
  background-size: cover;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.Alt1 {
  width: 120px;
  height: 45px;
  background-image: url('/images/WorkPoolAlt1.jpg');
  background-size: cover;
}
#alt1 {
  background-color: #f5f2cc;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.Alt2 {
  width: 120px;
  height: 45px;
  background-image: url('/images/WorkPoolAlt2.jpg');
  background-size: cover;
}
#alt2 {
  background-color: #f5f2cc;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.Alt3 {
  width: 120px;
  height: 45px;
  background-image: url('/images/WorkPoolAlt3.jpg');
  background-size: cover;
}
#alt3 {
  background-color: #f5f2cc;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.Alt4 {
  width: 120px;
  height: 45px;
  background-image: url('/images/WorkPoolAlt4.jpg');
  background-size: cover;
}
#alt4 {
  background-color: #f5f2cc;
  border-radius: 5px;
  border: 2px solid #DDD359;
}
.workdeck{
  grid-column: 2;
  grid-row: 3;
  background-image: url('/images/BackOfCard.PNG');
  background-size: cover;
  width: 67.5px;
  height: 95.5px;
}
.work-Card {
  grid-row: 2;
  transform: scale(0.25);
}
.work-Card div:hover{
  transform: scale(1.5)translate(-25%,0);
  z-index: 1;
}

@media screen and (max-width: 800px) {
  .workPool{
    grid-column: 3/span 5;
    grid-row: 9/span 5;
    width: auto;
    height: auto;
    display:grid;
    grid-template-rows: repeat(4,55px);
    grid-auto-flow: column;
    background-color: #f5f2cc;
    color: black;
    border-right: 2px solid #4C7B80;
    border-top: 2px solid #4C7B80;
    grid-template-columns: repeat(auto-fit, minmax(40px, 1fr));
  }
}
</style>
