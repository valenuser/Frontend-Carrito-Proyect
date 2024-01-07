<template>
    <nav>
        <div>
            <i class="fa-solid fa-cart-shopping"></i>
        </div>
        <div class="routes">
            <p>{{ user }}</p>
            <button @click="close">Cerrar Sesion</button>
        </div>
  </nav>
</template>
<script>
export default{
    name:'NavMainComponent',
    methods:{
      close(){
        localStorage.clear()
        window.location.replace('http://localhost:8080/#/')
      }
    },
    props:{
      user:String
    }
}

</script>
<style lang="sass" scoped>

nav 
  display: flex
  align-items: center
  justify-content: space-between
  width: 100%
  height: 10vh
  background: black

  i
    color: orange
    font-size: 40px
    margin-left: 10px

  p 
    font-weight: bold
    color: white
    text-decoration: none

    &.router-link-exact-active 
      color: #feb200

.routes
    width: 400px
    display: flex
    justify-content: space-around
    font-size: 20px
    
    button
      background: red
      border: none
      border-radius: 5px
      padding: 5px
      color: white
      font-weight: bold
</style>