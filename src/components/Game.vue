<template>
  <div class="d-flex justify-content-center">
    <div>
      <Header :totalPoints="totalPoints" />
      <div class="d-flex justify-content-center mb-4">
        <input type="text" v-model="name" class="mx-4 form-control" placeholder="Your name" />
        <div class="mr-4">
          <button
            class="btn btn-secondary dropdown-toggle"
            type="button"
            id="dropdownMenuButton"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >Size</button>
          <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
            <a class="dropdown-item" @click="mapSize=5">5X5</a>
            <a class="dropdown-item" @click="mapSize=6">6X6</a>
            <a class="dropdown-item" @click="mapSize=7">7X7</a>
            <a class="dropdown-item" @click="mapSize=8">8X8</a>
          </div>
        </div>
        <div class="btn btn-primary mr-4" @click="createCells">Start</div>
      </div>
      <div class="d-flex justify-content-center ml-3" v-if="mapSize &&ready">
        <MineTable :tableItems="tableItems" @onPress="press" />
        <ScoreBoard :highScores="highScores" />
      </div>
      <div v-if="endGameModal">
        <transition name="modal">
          <div class="modal-mask">
            <div class="modal-wrapper">
              <div class="modal-dialog end-game-modal">
                <div class="modal-content">
                  <div class="modal-header">
                    <h4 class="modal-title text-center">GAME OVER</h4>
                  </div>
                  <div class="modal-body d-flex justify-content-between">
                    <div class>Your Score</div>
                    <div class>{{totalPoints}}</div>
                  </div>
                  <div class="align-center mb-3">
                    <div class="btn btn-warning btn-lg mr-2" @click="tryAgain()">Try Again</div>
                    <div class="btn btn-success btn-lg" @click="newGame()">New Game</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";
import ScoreBoard from "@/components/ScoreBoard";
import MineTable from "@/components/MineTable";

export default {
  data: () => ({
    endGameTimeout: null,
    ready: false,
    endGameModal: false,
    name: "",
    highScores: [],
    totalPoints: 0,
    mapSize: null,
    tableItems: []
  }),

  methods: {
    press(item) {
      if (item.type == "tick") this.totalPoints += 5;
      else if (item.type == "mine") this.failed();
      item.selected = !item.selected;
    },
    createCells() {
      this.tableItems = [];
      this.totalPoints = 0;
      this.ready = false;
      const mapSize = this.mapSize;
      const mineArray = [];

      //unique mayın boşlukları oluşturmak için
      while (mineArray.length < mapSize) {
        const r = Math.floor(Math.random() * mapSize * mapSize);
        if (mineArray.indexOf(r) === -1) mineArray.push(r);
      }

      let counter = 0;
      const tableColumns = [];
      for (let i = 0; i < mapSize; i++) {
        const tableRowItems = [];
        for (let cell = 0; cell < mapSize; cell++) {
          if (mineArray.includes(counter))
            tableRowItems.push({ type: "mine", selected: false });
          else tableRowItems.push({ type: "tick", selected: false });
          counter++;
        }
        this.tableItems.push(tableRowItems);
      }
      console.log(JSON.stringify(this.tableItems));
      this.ready = true;
    },
    failed() {
      this.tableItems.flat().map(i => (i.selected = true));
      clearTimeout(this.endGameTimeout);
      this.endGameTimeout = setTimeout(() => {
        this.endGameModal = true;
      }, 5000);
    },
    endGame() {
      this.highScores.push({ name: this.name, score: this.totalPoints });
      this.highScores.sort((a, b) => b.score - a.score);
      if (this.highScores.length > 10) this.highScores.pop();
    },
    tryAgain() {
      this.endGame();
      this.createCells();
      this.endGameModal = false;
    },
    newGame() {
      this.highScores = [];
      this.createCells();
      this.name = "";
      this.endGameModal = false;
    }
  },
  components: {
    Header,
    ScoreBoard,
    MineTable
  }
};
</script>

<style scoped>
.end-game-modal .modal-header {
  padding: 0;
}
.end-game-modal .modal-title {
  background-color: #00bce5;
  padding: 10px;
  color: #fff;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  font-weight: bold;
}
.text-center {
  width: 100%;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
</style>
