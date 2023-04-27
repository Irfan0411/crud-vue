<template>
        <div class="row border-bottom align-items-center">
            <div class="col-3">{{ item.name }}</div>
            <div class="col-4">{{ item.desc }}</div>
            <div class="col-1">{{ item.stock }}</div>
            <div class="col-2 price">Rp. {{ ribuan(item.price) }}</div>
            <div class="col-2 button">
                <button @click=" $emit('addOne')" v-if="item.stock !== 0" class="btn btn-primary me-1">Add One</button>
                <button @click=" $emit('addAll')" v-if="item.stock !== 0" class="btn btn-primary">Add All</button>
            </div>
        </div>
</template>

<script>
    export default {
        name: 'ItemProduct',
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