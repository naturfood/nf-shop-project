<template>
    <tr>
        <td><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></td>
        <td>{{ Number(item.product.price * item.product.pack_quantity).toFixed(2) }} €</td>
        <td>
            <a @click="decrementQuantity(item)"><i class="fas fa-minus"></i></a>
            {{ item.quantity }}
            <a @click="inccrementQuantity(item)"><i class="fas fa-plus"></i></a>
        </td>
        <td>{{ getItemTotal(item).toFixed(2) }} €</td>
        <td><button class="button delete is-medium is-danger" @click="removeFromPallet(item)"></button></td>
    </tr>
</template>

<script>
export default {
    name: 'PalletItem',
    props:{
        initialItem: Object
    },
    data(){
        return {
            item: this.initialItem
        }
    },
    methods:{
        getItemTotal(item){
            return item.quantity * item.product.price * item.product.pack_quantity
        },
        decrementQuantity(item){
            item.quantity -=1
            if (item.quantity===0) {
                this.$emit('removeFromPallet',item)
            }
            this.updatePallet()
        },
        inccrementQuantity(item){
            item.quantity +=1
            this.updatePallet()
        },
        updatePallet(){
            localStorage.setItem('cart',JSON.stringify(this.$store.state.cart))
        },
        removeFromPallet(item){
            this.$emit('removeFromPallet',item)
            this.updatePallet()
        }
    }
}
</script>