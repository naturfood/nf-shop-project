<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Checkout</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                            v-for="item in pallet.items"
                            v-bind:key="item.product.id"
                        >
                            <td>{{ item.product.name }}</td>
                            <td>{{ Number(item.product.price * item.product.pack_quantity).toFixed(2) }} €</td>
                            <td>{{ item.quantity }}</td>
                            <td>{{ getItemTotal(item).toFixed(2) }} €</td>

                        </tr>
                    </tbody>
                    <tfoot>
                        <tr>
                            <td colspan="2">Total</td>
                            <td>{{ palletTotalLength }}</td>
                            <td>{{ palletTotalPrice.toFixed(2) }} €</td>
                        </tr>
                    </tfoot>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Checkout',
    data(){
        return {
            pallet:{
                items: []
            },
            stripe:{},
            card:{},
            first_name: '',
            last_name: '',
            email:  '',
            phone: '',
            address: '',
            zipcode: '',
            place: '',
            errors: []
        }
    },
    mounted(){
        document.title='Checkout | Natur Food GmbH'

        this.pallet=this.$store.state.cart
    },
    methods:{
        getItemTotal(item){
            return item.quantity * item.product.pack_quantity * item.product.price
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