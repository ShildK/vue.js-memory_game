<script>
export default {
   name: "ModalWindow",
   props: {
      show: {
         type: Boolean,
         required: true,
         default: false,
      },
      close: {
         type: Function,
         required: true,
      },
      isWin: {
         type: Boolean,
         required: true,
      },
   },
   data: () => {
      return {};
   },

};
</script>

<template>
   <div class="modal" v-if="show" @click.self="close">
      <div class="modal-content">
         <div class="message lose" v-if="!isWin">
            <p>Вы проиграли :(</p>
         </div>
         <div class="message win" v-else>
            <p>Поздравляю с победой! :D</p>
         </div>
         <button class="close" @click="close">Закрыть</button>
      </div>
   </div>
</template>

<style scoped>
.modal {
   top: 0;
   bottom: 0;
   right: 0;
   left: 0;
   position: fixed;
   display: flex;
   background: rgba(0, 0, 0, 0.5);
   z-index: 1;
}
.modal-content {
   margin: auto;
   background: white;
   border-radius: 12px;
   min-height: 50px;
   min-width: 200px;
   padding: 20px 30px;
}
.close {
    margin-top: 15px;
    padding: 10px 20px;

    border: none;
    border-radius: 5px;
    box-shadow: 2px 3px 5px rgb(192, 192, 192);
    background-color: #d4d4d4;

    font-weight: 500;

}
.close:hover{
    background-color: #b6b6b6;
}
.message {
   padding: 20px 30px;
}
.lose p{
    color: #9f330fcc;
    font-weight: 700;
    font-size: 24px;
}
.win p{
    color: #42b983;
    font-weight: 700;
    font-size: 24px;
}
</style>
