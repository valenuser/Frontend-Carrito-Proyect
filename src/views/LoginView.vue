<template>
<section id="main">
        <div class="foto">
            <img src="../assets/Online Groceries-cuate.png" alt="">
        </div>
        <div class="data">
            <div class="datos">
                <div class="titulo">
                    <h1>CARRITO</h1>
                </div>
                <div class="form">
                        <label for="#">Name</label>
                        <input type="text" placeholder="Username" v-model="username" minlength="10" maxlength="10" autocomplete="username">
                        <label for="#">Password</label>
                        <input type="password" placeholder="Password" v-model="password" minlength="10" maxlength="10" autocomplete="current-password">
                        <input type="submit" value="Login" class="button" @click="login"/>
                </div>
            </div>
        </div>
</section>
</template>
<script>
import NavComponent from '../components/NavComponent.vue';
import Swal from 'sweetalert2'
import axios from 'axios'
export default{
    name:'InicioView',
    data(){
        return{
            username:'',
            password:''
        }
    },
    methods:{
        login(){
            if(this.username.includes(' ') || this.password.includes(' ')){
                Swal.fire({
                    icon: "error",
                    title: "Oops...",
                    text: "Rellene los campos correctamaente"
                });
            }else{
                const datos = {
                    username:this.username,
                    password_user:this.password
                }
                axios.post('http://localhost:3000/login',datos)
                .then(resp =>{
                    localStorage.setItem('token',resp.data.token)
                    NavComponent.methods.change()
                    window.location.reload()
                    window.location.replace('http://localhost:8080/#/main')
                })
                .catch(error =>{
                    console.log(error);
                    Swal.fire({
                        icon: "error",
                        title: "Oops...",
                        text: "Usuario no encontrado"
                    });
                    this.username = ''
                    this.password = ''
                })
            }
        }
    },
    mounted(){
        if(localStorage.getItem('token')){
            NavComponent.methods.change()
            window.location.replace('http://localhost:8080/?#/main')
        }
    }
}
</script>

<style lang="sass" scoped>

#main
    width: 100%
    height: 90vh
    display: flex
    flex-direction: row
    overflow: hidden

.foto
    width: 50%
    height: 100vh
    overflow: hidden
    img 
        width: 750px

.titulo
    margin-top: 80px
    margin-bottom: 90px
    font-size: 40px

.data
    width: 50%
    height: 100vh
    display: flex
    align-items: start
    margin-top: 55px
    justify-content: center

.datos
    display: flex
    flex-direction: column
    align-items: center
    justify-content: start
    width: 500px
    height: 600px
    border-radius: 10px
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px



    .form
        display: flex
        flex-direction: column
        align-items: center


        input
            width: 300px
            padding: 10px
            border-radius: 10px
            border: 1px solid gray
            outline: none
            margin-bottom: 30px

        .button
            width: 100px
            padding: 10px
            border-radius: 5px
            border: none
            background: black
            color: white
            font-weight: bold
</style>