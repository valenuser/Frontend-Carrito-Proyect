<template>
    <section id="main">
            <div class="foto">
                <img src="../assets/Sign up-rafiki.png" alt="">
            </div>
            <div class="data">
                <div class="datos">
                    <div class="titulo">
                        <h1>REGISTER</h1>
                    </div>
                    <div class="form">
                            <label for="#">Name</label>
                            <input type="text" placeholder="Username" minlength="10" maxlength="10" v-model="username"  autocomplete="username">
                            <label for="#">Email</label>
                            <input type="email" placeholder="Email" v-model="email" maxlength="256" autocomplete="email">
                            <label for="#">Password</label>
                            <input type="password" placeholder="Password" minlength="10" maxlength="10" v-model="password" autocomplete="current-password">
                            <input type="submit" class="button" value="Register" @click="register">

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
        name:'RegisterView',
        data(){
            return{
                username:'',
                email:'',
                password:''
            }
        },
        mounted(){
            if(localStorage.getItem('token')){
                NavComponent.methods.change()
                window.location.replace('http://localhost:8080/?#/main')
            }
        },
        methods:{
            validationEMail(email){
                let datos = email.split('@')
                const validate = ['gmail.com','gmail.es','yahoo.com','yahoo.es','hotmail.com','hotmail.es']

                if(validate.includes(datos[1]) == false){
                    return  false
                }

                if(datos[0].length < 4 || datos[0].length > 64){
                    return false
                }

                return true
            },
            register(){
                if(this.username.includes(' ') || this.validationEMail(this.email) == false || this.password.includes(' ')){
                    Swal.fire({
                        icon: "error",
                        title: "Oops...",
                        text: "Por favor introduce los datos correctamente."
                        });

                    this.username = ''
                    this.email = ''
                    this.password = ''
                }else{
                    const datos = {
                        username:this.username,
                        email:this.email,
                        password_user:this.password
                    }

                    axios.post('http://localhost:3000/register',datos)
                    .then(resp => {
                        console.log(resp.status)
                        if(resp){
                            Swal.fire({
                                icon: "success",
                                title: "Bienvenido!",
                                text: "Te has registrado con éxito.",
                                showConfirmButton: false,
                                timer: 1500
                            });
                            setTimeout(()=>{
                                window.location.replace('http://localhost:8080/#/')
                            },1500)
                        }
                    })
                    .catch(error => console.log(error))


                }
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
    </style>