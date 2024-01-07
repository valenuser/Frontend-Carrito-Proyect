<template>
    <nav>
        <div>
            <i class="fa-solid fa-cart-shopping"></i>
        </div>
        <div class="routes" id="login">
            <router-link to="/">Login</router-link> 
            <router-link to="/register">Register</router-link>
        </div>
        <div class="routes" id="main">
          <div>
            <input type="text" name="" v-model="buscador"  minlength="50"  maxlength="50"  placeholder="Find the product you want..." v-on:keyup.enter="search">
            <i class="fa-solid fa-magnifying-glass"></i>
          </div>
          <p>{{ user }}</p>
          <button @click="close">Login out</button>
        </div>
  </nav>
</template>
<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import MainView from '@/views/MainView.vue'
export default{
    name:'NavComponent',
    data(){
      return{
        user:'',
        buscador:''
      }
    },
    mounted(){
      window.addEventListener('load',()=>{
        if(window.location.href.includes('http://localhost:8080/#/main') || window.location.href.includes('http://localhost:8080/?#/main') ){


          axios.get(`http://localhost:3000/login/${localStorage.getItem('token')}`)
          .then(resp =>{
            if(resp.data == false){
              document.getElementById('main').style.display = 'none'
              document.getElementById('login').style.display = 'flex'
            }else{
              this.user = resp.data
              document.getElementById('main').style.display = 'flex'
              document.getElementById('login').style.display = 'none'
            }
          })
          .catch(error => console.log(error))

        }else{
          document.getElementById('main').style.display = 'none'
          document.getElementById('login').style.display = 'flex'
        }
      })  
    },
    methods:{
        search(){
          axios.get(`http://localhost:3000/productos/buscar/${this.buscador}`)
          .then(resp =>{
            if(resp.data.length == 0){
              Swal.fire({
                            icon: "error",
                            title: "Oops...",
                            text: "Producto no encontrado."
                        })
            }else{
              MainView.methods.chargeProduct(resp.data[0])
              this.buscador=''
            }
          })
          .catch(error => console.log(error))
        },
        close(){
          localStorage.clear()
          window.location.reload()
          window.location.replace('http://localhost:8080/#/')
        },
        change(){
          window.addEventListener('load',()=>{
              if(window.location.href.includes('http://localhost:8080/#/main') || window.location.href.includes('http://localhost:8080/?#/main') ){


                axios.get(`http://localhost:3000/login/${localStorage.getItem('token')}`)
                .then(resp =>{
                  if(resp.data == false){
                    document.getElementById('main').style.display = 'none'
                    document.getElementById('login').style.display = 'flex'
                  }else{
                    this.user = resp.data
                    document.getElementById('main').style.display = 'flex'
                    document.getElementById('login').style.display = 'none'
                  }
                })
                .catch(error => console.log(error))

              }else{
                document.getElementById('main').style.display = 'none'
                document.getElementById('login').style.display = 'flex'
              }
          })  
        }
    }
}
</script>
<style lang="sass" scoped>

.main
  display: none

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

  a 
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

    a 
    font-weight: bold
    color: white
    text-decoration: none

    button
      background: red
      border: none
      border-radius: 5px
      padding: 5px
      color: white
      font-weight: bold

    input
      width: 550px
      height: 30px
      position: absolute
      right: 480px
      border-radius: 5px
      border: none
      outline: none
      text-align: center

    i
      position: absolute
      right: 500px
      top: 30px
      font-size: 20px
      color: grey
    
</style>