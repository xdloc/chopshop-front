<script>
import axios from "axios";

const API_URL = `https://siberia.vps.webdock.cloud/?method=list%2Flist`
let id = 0;
export default {
    data () {
        return {
            items: [
                { id: id++, name: 'this.newItem 1', created_at: '25.04.2022', updated_at: '25.04.2022', status: 1 },
                { id: id++, name: 'this.newItem 2', created_at: '15.02.1525', updated_at: '23.04.2022', status: 1 }
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
        editItem(item){

        },
        created() {
            // fetch on init
            this.fetchData()
        },
        mounted () {
            axios
                .get(API_URL)
                .then(response => (this.items = response))
        },
        async fetchData() {
            //this.items = await (await fetch(API_URL)).json()
            axios
                .get(API_URL)
                .then(response => (this.items = response))
        }
    }
}
</script>

<template>
    <form @submit.prevent="addItem">
        <input v-model="newItem">
        <button @click="addItem()" type="button" class="btn btn-primary">Add Item</button>
        <button @click="editItem(item)" type="button" class="btn btn-secondary">Refresh</button>
    </form>

    <ul  class="list-group">
        <li  class="list-group-item" v-for="item in items" :key="item.id">
            <input class="form-check-input me-1" type="checkbox" value="" id="list_{{ item.id}}">
            <label class="form-check-label stretched-link" for="list_{{ item.id}}">{{ item.name }}
            </label>
            <button @click="editItem(item)"  type="button" class="btn btn-dark">CHANGE</button>
            <button @click="removeItem(item)"  type="button" class="btn btn-dark">X</button>

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
.btn{
    border-radius: 2px;
}
.list-group .label{
    width: 62%;
}
</style>