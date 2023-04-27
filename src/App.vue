<template>
    <div class="container">
        <span class="fs-1">Semua Produk</span>
        <div class="row fw-semibold border-bottom align-items-center">
            <div class="col">Name</div>
            <div class="col-4">Description</div>
            <div class="col-1">Stock</div>
            <div class="col-2">Price</div>
            <div class="col-2"></div>
        </div>
        <ItemProduct v-for="(item, index) in product" :key="index" :item="item" @addAll="addAll(item, index)" @addOne="addOne(item, index)" />
        <br>
        <span class="fs-1">Keranjang Belanja</span>
        <div class="row fw-semibold border-bottom align-items-center">
            <div class="col">Name</div>
            <div class="col">Quantity</div>
            <div class="col">Price</div>
            <div class="col"></div>
        </div>
        <ItemChart v-for="(item, index) in cart" :key="index" :item="item" @deleteAll="deleteAll(item, index)" @deleteOne="deleteOne(item, index)" />
        <div class="row fw-semibold align-items-center border-bottom">
            <div class="col">Total:</div>
            <div class="col">Rp. {{ ribuan(totalBayar) }}</div>
        </div>
        <br>
        <button @click="checkOut()" class="btn btn-success">Checkout</button>
    </div>
</template>

<script>
import ItemProduct from './components/ItemProduct.vue';
import ItemChart from './components/ItemChart.vue';
import swal from 'sweetalert';


export default {
    name: "App",
    data() {
        return {
            product: [
            { name: "Indomie goreng rendang", desc: "Makanan instan terenak di dunia", stock: 10, price: 3900 },
            { name: "Mie gelas rendang", desc: "Mie instan khusus anak kosan", stock: 3, price: 1500 },
            { name: "Bakmi mewah", desc: "Kalau anak kosan hangan macam2 deh", stock: 90, price: 10000 }
            ],
            cart: [
                {name: "Mie gelas rendang", quantity: 1, price: 1500}
            ]
        }
    },
    components: { 
        ItemProduct, ItemChart
    },
    methods: {
        addOne(item, index){
            this.product[index].stock--
            const i = this.cart.findIndex(e=> e.name === item.name)
            if (i === -1) {
                this.cart = [...this.cart, {name: item.name, quantity: 1, price: item.price}]
            } else {
                this.cart[i].quantity++
                this.cart[i].price += item.price
            }
        },
        addAll(item, index){
            const i = this.cart.findIndex(e=> e.name === item.name)
            if (i === -1) {
                this.cart = [...this.cart, {name: item.name, quantity: item.stock, price: item.price*item.stock}]
            } else {
                this.cart[i].quantity += item.stock
                this.cart[i].price += item.price*item.stock
            }
            this.product[index].stock = 0
        },
        deleteAll(item, index){
            const i = this.product.findIndex(e=> e.name === item.name)
            this.product[i].stock += item.quantity
            this.cart.splice(index, 1)
        },
        deleteOne(item, index){
            const i = this.product.findIndex(e=> e.name === item.name)
            this.product[i].stock++
            this.cart[index].quantity--
            this.cart[index].price -= this.product[i].price
            if (this.cart[index].quantity === 0) this.deleteAll(item, index)
        },
        ribuan(x) {
            return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        },
        checkOut() {
            swal("Success", "total yang harus dibayar Rp. "+this.ribuan(this.totalBayar), "success")
            this.cart = []
        }
    },
    computed: {
        totalBayar() {
            const total = this.cart.reduce((sum, value)=>{
                return sum + value.price
            }, 0)
            return total
        }
    }
}
</script>

<style>
</style>
