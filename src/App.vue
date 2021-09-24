<template>
  <div id="app">
    <div class="con-im-vue">
      <div class="con-img">
         <img src="https://vuejsexamples.com/assets/vue.png" alt="Vue.js Examples">
      </div>
    </div>
    <Calculator />
  </div>
</template>

<script>
import Calculator from './component/Calculator.vue'
export default {
  name: 'app',
  components:{
    Calculator
  }
}
</script>

<style lang="scss">
#app {
  width:310px;height:520px;display:flex;padding:3px;
  text-align: center;justify-content:space-between;flex-direction:column;
  align-items:center;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  //transform:translate(100%,10%);
  margin:20px auto;
  background:#333c5a;
  color: #2c3e50;
  .con-im-vue{
    display:flex;width:80px;height:80px;margin:5px;border-radius:50%;background:#329869;text-align:center;
    justify-content:center;align-items:center;animation:fade 2s infinite;
    @keyframes fade {
     10% {background:#329869;}
     50% {background:#5ddba0;}
     90% {background:#329869}
    }

    .con-img
  {
    display:flex;width:50px;height:50px;background:#6cdda1;text-align:center;justify-content:center;z-index:5;
    border-radius:50%;//#329869
    img{width:38px;height:38px;display:inline-block;margin-top:10px;}

   }
  }

}


</style>
