<script>
import BoardItem from "./BoardItem.vue";
import ModalWindow from "./ModalWindow.vue";
import { GameStatus } from "../consts/gameStatus.js";
import { FieldStatus } from "../consts/fieldStatus.js";

export default {
   name: "Board",
   components: { BoardItem, ModalWindow },
   data: () => {
      return {
         numberFields: 25,
         fields: [],
         difficult: 3,
         gameStatus: GameStatus.NONE,
         isDisabled: false,
         gameSpeed: 2000,
         isWin: false,
         isModalShown: false,
      };
   },
   mounted() {
      this.init();
   },
   methods: {
      init() {
         this.fields = [];
         for (let i = 0; i < this.numberFields; i++) {
            this.fields.push({
               id: i,
               clicked: false,
               value: FieldStatus.INCORRECT,
            });
         }
      },

      startRound() {
         this.init();
         this.prepareGame();
      },

      restartGame() {
         this.difficult = 3;
         this.isModalShown = false;
         this.startRound()
      },

      prepareGame() {
         this.isDisabled = true;
         this.gameStatus = GameStatus.SHOW_CORRECT;
         for (let i = 0; i < this.difficult; i++) {
            const index = this.rand(0, this.numberFields - 1);

            if (this.fields[index].value !== FieldStatus.CORRECT) {
               this.fields[index].value = FieldStatus.CORRECT;
            } else {
               i--;
            }
         }
         setTimeout(() => {
            this.gameStatus = GameStatus.PLAY;
            this.isDisabled = false;
         }, this.gameSpeed);
      },

      rand(min, max) {
         return Math.floor(Math.random() * (max - min)) + min;
      },

      selectField(id) {
         const index = this.fields.findIndex((field) => {
            return field.id === id;
         });

         if (index >= 0 && !this.fields[index].clicked) {
            this.fields[index].clicked = true;
            this.checkGame();
         }
      },

      checkGame() {
         const errorIndex = this.fields.findIndex((field) => {
            return field.clicked && field.value === FieldStatus.INCORRECT;
         });

         if (errorIndex >= 0) {
            this.setGameOver();
            return;
         }

         const notFoundItemIndex = this.fields.findIndex((field) => {
            return !field.clicked && field.value === FieldStatus.CORRECT;
         });

         if (notFoundItemIndex === -1) {
            this.setWin();
         }
      },
      setGameOver() {
         this.gameStatus = GameStatus.LOSE;
         setTimeout(() => {
            this.showModal();
         }, 1000);
      },
      setWin() {
         this.gameStatus = GameStatus.WIN;
         setTimeout(() => {
            this.difficult += 1;
            if (this.difficult > 5) {
               this.showModal();
            } else {
               this.startRound();
            }
         }, 1000);
      },
      showModal() {
         this.isModalShown = true;
         this.isWin = this.gameStatus === GameStatus.WIN;
      },
   },
};
</script>

<template>
   <div class="board-wrapper">
      <ModalWindow :isWin="isWin" :show="isModalShown" :close="restartGame" />
      <div class="board">
         <BoardItem
            v-for="field in fields"
            :field="field"
            :currentGameStatus="gameStatus"
            :key="'item-' + field.id"
            @selectField="selectField"
         />
         <p class="difficult">Сложность: {{ difficult }}</p>
      </div>
      <button @click="restartGame" :disabled="isDisabled" class="btn">Старт</button>
   </div>
</template>

<style scoped>
.board-wrapper {
   margin-bottom: 100px;
}

.board {
   width: 420px;
   margin: 0 auto;
   background: #eee;
   padding: 10px;
}
.difficult {
   font-weight: 700;
   margin-bottom: 10px;
}
.btn {
   background: #42b983cc;
   border: none;
   border-radius: 2px;

   padding: 10px 35px;
   margin: 10px 0;

   cursor: pointer;
   outline: none;

   color: #fff;
   font-weight: 600;
}
.btn:hover {
   background: #42b983;
}
.btn:disabled {
   opacity: 0.5;
}
</style>
