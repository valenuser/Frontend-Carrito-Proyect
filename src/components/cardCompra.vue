<template>
    <div class="container">
        <div class="data">
            <img :src="compra_img" alt="">
            <p>{{ compra_name }}</p>
            <p>{{compra_precio}}€</p>
        </div>
        <div class="check">
            <div class="quantity">
                <button class="rest" @click="restar">-</button>
                <input type="number" value="1" min="1" max="10"  :id="compra_name+compra_id">
                <button class="sum" @click="sumar">+</button>
            </div>
            <button class="delete" @click="borrar" :name="compra_id">Borrar</button>
        </div>
    </div>
</template>
<script>
import MainView from '@/views/MainView.vue'
import Swal from 'sweetalert2'
export default{
    name:'cardCompra',
    props:{
        compra_img:String,
        compra_name:String,
        compra_precio:Number,
        compra_id:Number
    },
    methods:{
        restar(){
            if(document.getElementById(this.compra_name+this.compra_id).value == 1){
                Swal.fire({
                        icon: "error",
                        title: "Oops...",
                        text: "No puede poner una cantidad menor a 1"
                        })
            }else{
                document.getElementById(this.compra_name+this.compra_id).value = document.getElementById(this.compra_name+this.compra_id).value  - 1
            }
        },
        sumar(){
            if(document.getElementById(this.compra_name+this.compra_id).value  == 10){
                Swal.fire({
                        icon: "error",
                        title: "Oops...",
                        text: "Puede pedir hasta un maximo de 10 productos iguales."
                        })
            }else{
                document.getElementById(this.compra_name+this.compra_id).value = Number(document.getElementById(this.compra_name+this.compra_id).value)  + 1
            }
        },
        borrar(event){
            let compras = localStorage.getItem('compra')

            compras = compras.split(';')
            compras.pop()
            let product_comprados = []
            compras.forEach(el =>{
                let dato  = el.replace("'","")
                product_comprados.push(JSON.parse(dato));
            })

            if(product_comprados.length == 1){
                localStorage.removeItem('compra')
                localStorage.setItem('compra','')

                MainView.methods.seeProducts()
            }else{
                let listCompra =  product_comprados.filter(el => el.id != event.srcElement.name)
                console.log(listCompra);
                
                let stringLocal = ''
                listCompra.forEach(el =>{
                    stringLocal = stringLocal +JSON.stringify(el)+';'
                })
                
                localStorage.removeItem('compra')
                localStorage.setItem('compra',stringLocal)
                
                MainView.methods.seeProducts()
            }

        }
    }
}
</script>
<style lang="sass" scoped>
.container 
    display: flex
    flex-direction: row
    justify-content: space-between
    width: 100%
    height: 220px
    overflow: hidden
    border-radius: 10px
    //border: 1px solid black
    margin: 10px
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px

    img
        width: 150px

    input
        width: 60px
        border: 1px solid black 
        border-radius: 5px
        text-align: center
        margin: 2px

.data
    width: 40%
    display: flex
    flex-direction: column
    align-items: center

.check
    display: flex
    flex-direction: column
    align-items: end
    justify-content: center
    width: 200px
    // border: 1px solid black

.rest
    display: flex
    align-items: center
    justify-content: center
    width: 20px
    height: 20px
    background: black
    color: white
    border: none
    font-size: 25px
    font-weight: bold
    text-align: center
    border-radius: 50%
.sum
    display: flex
    align-items: center
    justify-content: center
    width: 20px
    height: 20px
    background: green
    color: white
    border: none
    font-size: 20px
    border-radius: 50%
    font-weight: bold

.quantity
    display: flex
    flex-direction: row
    align-items: center

.delete
    margin-top: 20px
    width: 100px
    height: 30px
    border: none
    color: white
    background: red
    border-radius: 5px
    font-weight: bold
</style>
