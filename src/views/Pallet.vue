<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Pallet</h1>
            </div>
            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="palletTotalLength">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <PalletItem
                            v-for="item in pallet.items"
                            v-bind:key="item.product.id"
                            v-bind:initialItem="item"
                            v-on:removeFromPallet="removeFromPallet"
                        />
                    </tbody>
                </table>
                <p v-else>You don't have any prodcuts in your pallet..</p>
            </div>
            <div class="column is-12 box">
                <h2 class="title">Summary</h2>
                <strong>{{ palletTotalPrice.toFixed(2) }} €</strong>, {{ palletTotalLength }} items
                <hr>
                <router-link to="/pallet/order" class="button is-dark">Order items</router-link>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios';
import PalletItem from '@/components/PalletItem.vue'


export default{
    name: 'Pallet',
    components: {
        PalletItem
    },
    data(){
        return {
            pallet: {
                items: []
            }
        }
    },
    mounted(){
        this.pallet=this.$store.state.cart
    },
    methods:{
        removeFromPallet(item){
            this.pallet.items=this.pallet.items.filter(i=>i.product.id!=item.product.id)
        }
    },
    computed:{
        palletTotalLength(){
            return this.pallet.items.reduce((acc,curVal)=>{
                return acc +=curVal.quantity
            },0)
        },
        palletTotalPrice(){
            return this.pallet.items.reduce((acc,curVal)=>{
                return acc +=curVal.product.price * curVal.product.pack_quantity * curVal.quantity
            },0)
        }
    }
}
</script>