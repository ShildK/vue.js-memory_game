<script>
import { GameStatus } from "../consts/gameStatus.js";
import { FieldStatus } from "../consts/fieldStatus.js";

export default {
   name: "BoardItem",
   props: {
      field: {
         type: Object,
         required: true,
      },
      currentGameStatus: {
         type: Number,
         required: false,
      },
   },
   data: () => {
      return {
         gameStatus: GameStatus,
         fieldStatus: FieldStatus,
      };
   },
   methods: {
      select() {
         if (this.currentGameStatus === this.gameStatus.PLAY) {
            this.$emit("selectField", this.field.id);
         }
      },
   },
};
</script>

<template>
   <span
      @click="select"
      :class="
         'item ' +
         (currentGameStatus === gameStatus.SHOW_CORRECT // начало раунда, отображение правильных полей
            ? field.value === fieldStatus.CORRECT // если правильное поле
               ? 'item_active'
               : ''
            : field.clicked === true // выбор поля во время раунда
               ? field.value === fieldStatus.CORRECT // если правильное поле
                  ? 'item_active'
                  : 'item_wrong'
               : '')
      "
   ></span>
</template>

<style scoped>
.item {
   width: 70px;
   height: 70px;
   margin: 5px;

   background: #ccc;
   border-radius: 2px;
   border: none;

   display: inline-block;

   position: relative;
   cursor: pointer;

   transition: 0.4s;
   transform-style: preserve-3d;
}
.item:hover {
   transition: 0s;
   border: 2px solid rgb(182, 182, 182);
}
.item_active {
   background: #42b983cc;
   transform: rotateX(180deg);
}
.item_wrong {
   background: #9f330fcc;
   transform: rotateX(180deg);
}
</style>
