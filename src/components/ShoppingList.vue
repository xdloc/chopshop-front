
<script>
const API_URL = `https://siberia.vps.webdock.cloud/?method=list%2Flist`
let id = 0;
export default {
    data() {
        return {
            newItem: '',
            items: [
                { id: id++, name: '1', created_at: '', updated_at: '', status: 1 },
                { id: id++, name: '2', created_at: '', updated_at: '', status: 0 },
                { id: id++, name: '3', created_at: '', updated_at: '', status: 1 }
            ]
        }
    },
    watch: {

    },
    methods: {
        addItem() {
            this.items.push({ id: id++, name: this.newItem, created_at: '', updated_at: '', status: 1 })
            this.newItem = ''
            this.fetchData()
        },
        removeItem(item) {
            this.items = this.items.filter((t) => t !== item)
            this.fetchData()
        },
        created() {
            // fetch on init
            this.fetchData()
        },
        async fetchData() {
            this.items = await (await fetch(API_URL)).json()
        }
    }
}
</script>

<template>
    <form @submit.prevent="addItem">
        <input v-model="newItem">
        <button>Add Item</button>
    </form>
    <ul>
        <li v-for="item in items" :key="item.id">
            {{ item.name }}
            <button @click="removeItem(item)">X</button>
        </li>
    </ul>
</template>

<style>
.container {
    position: relative;
    padding: 0;
}

.item {
    width: 100%;
    height: 30px;
    background-color: #f3f3f3;
    border: 1px solid #666;
    box-sizing: border-box;
}

/* 1. declare transition */
.fade-move,
.fade-enter-active,
.fade-leave-active {
    transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: scaleY(0.01) translate(30px, 0);
}

/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
.fade-leave-active {
    position: absolute;
}
</style>