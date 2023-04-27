<template>
    <div class="row align-items-center border-bottom">
        <div class="col">{{ item.name }}</div>
        <div class="col">{{ item.quantity }}</div>
        <div class="col">Rp. {{ ribuan(item.price) }}</div>
        <div class="col">
            <button @click="$event => $emit('deleteOne')" class="btn btn-danger me-1">Delete One</button>
            <button @click="$event => $emit('deleteAll')" class="btn btn-danger">Delete All</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ItemChart',
    props: {
        item: Object
    },
    methods: {
        ribuan(x) {
            return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }
    }
}
</script>