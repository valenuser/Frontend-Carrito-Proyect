<template>
  <NavComponent/>
  <router-view/>
</template>
<script>
import NavComponent from './components/NavComponent.vue';
export default{
  components:{
    NavComponent
  },
  data(){
    return{
      NavMain:false,
      NavStart:Boolean
    }
  }
}
</script>
<style lang="sass">
*
  padding: 0
  margin: 0
  font-family: Avenir, Helvetica, Arial, sans-serif


#app 
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50


#main
  display: none

</style>
