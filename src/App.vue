<script>
import Board from "./components/Board.vue";
export default {
   components: {
      Board,
   },
   data: () => {
      return {};
   },
};
</script>

<template>
   <div class="container">
      <img src="./assets/memory-game.png" alt="Logo" class="logo" />
      <h1 class="title">Запомни меня</h1>
      <p>Запомни и открой все зеленые клетки</p>
      <Board></Board>
   </div>
</template>

<style>
.container {
   min-height: 100lvh;
   padding-top: 30px;
   text-align: center;
   background-image: url(../src/assets/bg-2.jpg);
}
.logo {
   max-width: 300px;
   margin-bottom: 20px;
}
.title {
   font-size: 40px;
   font-weight: 700;
}
p{
   font-size: 20px;
   font-weight: 500;
   margin-bottom: 10px;
}
</style>
