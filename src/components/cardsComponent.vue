<template>
    <div class="card" >
        <div class="img">
            <img :src="imgProduct" alt="">
        </div>
        <div class="data">
            <p>{{nameProduct}}</p>
            <p>{{precio}}€</p>
        </div>
        <button v-on:click="buy" :id="productId">Buy Now</button>
    </div>

</template>
<script>
import MainView from '../views/MainView.vue'
export default{
    name:'cardsComponent',
    props:{
        productId:Number,
        imgProduct:String,
        precio:Number,
        nameProduct:String
    },
    methods:{
        buy(event){
            MainView.methods.cargeProducts(event.srcElement.__vnode.props.id)
        }
    }
}
</script>
<style lang="sass" scoped>
.card
    width: 320px
    height: 400px
    border-radius: 10px
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px
    margin: 5px
    margin-top: 10px
    display: flex
    flex-direction: column
    align-items: center
.img
    width: 100%
    height: 320px
img
    width: 300px

button
    width: 100px
    padding: 5px
    background: black
    color: white
    font-weight:bold
    border: none
    border-radius: 5px
    margin-top: 20px

.data
    width: 100%
    display: flex
    justify-content: space-around
</style>