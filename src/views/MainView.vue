<template>
    <section>
        <div class="containerProds">
            <h1>Products</h1>
            <div v-if="productos.length >= 1" class="container" id="products">
                <cardsComponent v-for="prod in productos" :key="prod.id" :imgProduct="prod.product_picture" :nameProduct="prod.product_name" :precio="prod.precio" :productId="prod.id"/>
            </div>
            <div v-else-if="productos.length == 1" class="container" id="products">
                <cardsComponent v-for="prod in productos" :key="prod.id" :imgProduct="prod.product_picture" :nameProduct="prod.product_name" :precio="prod.precio" :productId="prod.id"/>
            </div>
        </div>
        <div v-if="compras.length != 0">
            <div class="title">
                <h1>Selected products </h1>
                <button @click="pay">Pay</button>
            </div>
            <div class="containerCarrito">
                <div class="comprasLoad">
                    <cardCompra v-for="prod in compras" :key="prod.id" :compra_img="prod.product_picture" :compra_name="prod.product_name" :compra_precio="prod.precio" :compra_id="prod.id"/>
                </div>
            </div>
        </div>
        <div v-else>
            <h1>Selected products</h1>
            <div class="compras">
                <p>There are no products selected</p>
            </div>
        </div>
    </section>
</template>
<script>
import cardCompra from '@/components/cardCompra.vue'
import cardsComponent from '@/components/cardsComponent.vue'
import axios from 'axios'
import Swal from 'sweetalert2'


export default{
    name:'MainView',
    components:{
        cardsComponent,
        cardCompra
    },
    data(){
        return{
            productos:[],
            compras:[],
            precioFinal: 0
        }
    },
    mounted(){

        if(!localStorage.getItem('token')){
            window.location.replace('http://localhost:8080/#/')
        }
    
        axios.get('http://localhost:3000/productos')
        .then(resp => this.productos = resp.data)
        .catch(error => console.log(error))

        setInterval(()=>{
            if(localStorage.getItem('compra')){
                let compras = localStorage.getItem('compra')
                compras = compras.split(';')
                compras.pop()
                let product_comprados = []
                compras.forEach(el =>{
                    let dato  = el.replace("'","")
                    product_comprados.push(JSON.parse(dato));
                })
    
                this.compras = product_comprados
            }else{
                localStorage.removeItem('compra')
                this.compras = []
            }
        },1000)


    },
    methods:{
        pay(){
            let compras = localStorage.getItem('compra')

            let pay = 0

            let listBuy = []

            compras = compras.split(';')
            compras.pop()

            compras.forEach(el =>{
                let dato  = el.replace("'","")
                let jsonDato = JSON.parse(dato)

                let quantity = document.getElementById(jsonDato.product_name+jsonDato.id).value
                pay = pay + (jsonDato.precio * quantity)

                jsonDato.quantity = Number(document.getElementById(jsonDato.product_name+jsonDato.id).value)

                jsonDato.precio = jsonDato.precio * quantity

                listBuy.push(jsonDato)
            })

            const token = localStorage.getItem('token')

            listBuy.push({'token':token,'precioFinal':pay})


            Swal.fire({
            title: `Confirm the purchase`,
            text: `Final price:  ${pay}€`,
            footer:'(The receipt will be sent to the registered email)',
            imageWidth: 200,
            imageHeight: 200,
            showCancelButton: true,
            confirmButtonText: "Buy",
            }).then((result) => {
            if (result.isConfirmed){
                axios.post('http://localhost:3000/comprar',listBuy)
                .then(resp =>{
                    if(resp){
                        Swal.fire({
                            position: "center",
                            icon: "success",
                            title: "Thank you for your purchase!",
                            showConfirmButton: false,
                            timer: 2000
                        });

                        localStorage.removeItem('compra')

                        resp.headers.get()
                    }
                })
                .catch(error => console.log(error))
            }
            });
        },
        seeProducts(){
            if(localStorage.getItem('compra') == ''){
                localStorage.removeItem('compra')
                this.compras = []
            }else{
                let compras = localStorage.getItem('compra')

                compras = compras.split(';')
                compras.pop()
                let product_comprados = []
                compras.forEach(el =>{
                    let dato  = el.replace("'","")

                    product_comprados.push(JSON.parse(dato));
                })
    
                this.compras = product_comprados
            }
        },
        verifyUser(clave){
            if(localStorage.getItem('compra')){
                let compras = localStorage.getItem('compra')
    
                compras = compras.split(';')
                compras.pop()
                let product_comprados = []
                compras.forEach(el =>{
                    let dato  = el.replace("'","")
                    product_comprados.push(JSON.parse(dato));
                })

                if(product_comprados.filter(el => el.id == clave).length == 0){
                    return true
                }else{
                    return false
                }
            }else{
                return true
            }
        },
        cargeProducts(dato){
            if(this.verifyUser(dato)){
                axios.get(`http://localhost:3000/productos/${dato}`)
                .then(resp =>{
                    if(localStorage.getItem('compra')){
                        let carro = localStorage.getItem('compra')

                        localStorage.removeItem('compra')
                        localStorage.setItem('compra',carro+JSON.stringify(resp.data[0])+';')

                        this.seeProducts()
                        
                    }else{
                        localStorage.setItem('compra',JSON.stringify(resp.data[0])+';')

                        this.seeProducts()
                    }
                    
                })
                .catch(error =>{
                    if(error){
                        Swal.fire({
                            icon: "error",
                            title: "Oops...",
                            text: "Producto no disponible"
                        })
                        console.log(error);
                    }
                } )   
            }else{
                Swal.fire({
                        icon: "error",
                        title: "Oops...",
                        text: "Producto ya seleccionado."
                        })
            }
        },
        chargeProduct(dato){
            Swal.fire({
            title: `${dato.product_name}`,
            text: `${dato.precio}€`,
            imageUrl: `${dato.product_picture}`,
            imageWidth: 200,
            imageHeight: 200,
            imageAlt: "Custom image",
            showCancelButton: true,
            confirmButtonText: "Buy",
            }).then((result) => {
            if (result.isConfirmed) {
                this.cargeProducts(dato.id)
            }
            });
        }
    }
}
</script>
<style lang="sass" scoped>

.containerProds
    width: 95.5%
section
    display: flex

.imagenes
    width: 300px

.container
    width: 95.5%
    display: flex
    flex-wrap: wrap
    justify-content: start

.title
    display: flex
    justify-content: space-between
    margin-top: 5px
    button
        margin-right: 20px
        border: none
        width: 100px
        background: green
        color: white
        font-weight: bold
        border-radius: 5px
    
    h1
        margin-left: 10px
.compras
    width: 380px
    height: 80vh
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px
    border-radius: 10px
    margin: 5px
    margin-top: 10px
    display: flex
    flex-direction: column
    justify-content: center

.containerCarrito
    overflow: scroll
    width: 400px
    height: 90vh

    ::-webkit-scrollbar
        display: none
    
.comprasLoad
    width: 400px
    height: 700vh
    margin: 5px
    margin-top: 10px
    display: flex
    flex-direction: column
    overflow: hidden
    border: none
</style>