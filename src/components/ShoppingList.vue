<script>
import axios from "axios";

let BACKEND = 'https://siberia.vps.webdock.cloud/';
let API_URL = BACKEND + `?method=`
let id = 1000;
let config = {
    headers: {
        'Content-Type': 'application/json',
        //'Origin': BACKEND,
    }
}

export default {
    data: () => ({
        items: []
    }),
    watch: {
        // todo update items on edit without save button
    },
    methods: {
        addItem() {
            if (this.newItem.length > 0) {
                this.items.push({id: id++, name: this.newItem, created_at: '', updated_at: '', status: 0})
                this.newItem = ''
                axios
                    // todo ListController Add
                    .get(API_URL + 'list%2Fadd', config)
                    .then(response => {
                        this.fetchData()

                    })
                    .catch(error => {
                        console.log(error)
                        this.failure = true;
                    })
            }
        },
        removeItem(item) {
            axios
                // todo ListController Remove
                .get(API_URL + 'list%2Fremove', config)
                .then(response => {
                    //this.fetchData()
                    if(response.data){
                        console.log(response);
                        this.items = this.items.filter((t) => t !== item)
                    }
                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        },
        markItem(item) {
            axios
                // todo ListController Mark
                .get(API_URL + 'list%2Fedit', config)
                .then(response => {

                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        },
        refresh() {
            this.fetchData()
        },
        created() {
            this.fetchData()
        },
        mounted() {
            this.fetchData()
        },
        async fetchData() {
            axios
                .get(API_URL + 'list%2Flist', config)
                .then(response => {
                    this.items = response.data
                    return response.data;
                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        }
    }
}
</script>

<template>
    <section>
        <div>
            <form @submit.prevent="addItem" class="">
                <input v-model="newItem" class="form-control" placeholder="Another name in the list">
                <div class="btn-group" role="group" aria-label="Basic outlined example">
                    <div class="d-grid gap-2 col-6 mx-auto">
                        <button @click="addItem()" type="button" class="btn btn-outline-primary"><i class="fa-solid fa-plus"></i>Add Item</button>
                        <button @click="refresh(item)" type="button" class="btn btn-outline-secondary"><i class="fa-solid fa-arrow-rotate"></i>Refresh</button>
                    </div>
                </div>
            </form>
        </div>
    </section>

    <section v-if="failure">
        <div>
            <b-alert show dismissible>
                <i class="fa-solid fa-bomb"></i> FAILED <b>&rArr;</b>
            </b-alert>
        </div>
    </section>

    <section v-else>
        <div v-if="loading" class="alert alert-dark" role="alert">
            <i class="fa-solid fa-arrow-rotate"></i> Loading...
        </div>
        <ul class="list-group" v-else>
            <li class="list-group-item" v-for="item in items" :key="item.id">
                <div class="container text-center">
                    <div class="row">
                        <div class="col col-lg-2">
                            <input class="form-check-input me-1" type="checkbox" value="" @click="markItem(item)" id="{{ item.id }}">
                        </div>
                        <div class="col col-lg-auto col-md-auto">
                            <label class="form-check-label stretched-link" for="{{ item.id }}">{{ item.name }}
                            </label>
                        </div>
                        <div class="col col-lg-2">
                            <!--<button @click="editItem(item)" type="button" class="btn btn-dark btn-change"><i class="fa-solid fa-pen-to-square"></i>CHANGE</button>-->
                            <button @click="removeItem(item)" type="button" class="btn btn-dark btn-change"><i class="fa-solid fa-pen-to-square"></i>REMOVE
                            </button>
                        </div>
                    </div>
                </div>
            </li>
        </ul>
    </section>

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

.btn {
    border-radius: 18px;
    padding: 6px;
}

.btn-remove, .btn-change {
    margin-right: 0.25em !important;
    margin-top: 9px !important;
}

.btn-add, .btn-refresh {
    margin-right: 0.65em !important;
    margin-bottom: 0.65em !important;
    margin-top: 0.65em !important;
}

.btn-add {
    background-color: #00bd7e !important;
}

.btn-outline-primary {
    border-color: #00bd7e !important;
    --bs-btn-color: #00bd7e !important;
    --bs-btn-border-color: --teal;
    --bs-btn-hover-color: #fff;
    --bs-btn-hover-bg: #00bd7e !important;
    --bs-btn-hover-border-color: #00bd7e !important;
    --bs-btn-focus-shadow-rgb: 13, 110, 253;
    --bs-btn-active-color: #fff;
    --bs-btn-active-bg: #00bd7e !important;
    --bs-btn-active-border-color: #00bd7e !important;
    --bs-btn-active-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
    --bs-btn-disabled-color: #00bd7e !important;
    --bs-btn-disabled-bg: transparent;
    --bs-btn-disabled-border-color: #00bd7e !important;
    --bs-gradient: none;
}

.btn-outline-secondary{
    border-color: #00bd7e !important;
}

.list-group {
    margin-top: .25em;
}

.list-group-item {
    background: none;
    background-color: rgb(119, 88, 122) !important;
    backdrop-filter: blur(50);
}

label {
    color: whitesmoke;
}

.form-check-input[type=checkbox] {
    border-radius: .25em;
    margin-top: 11px;
    margin-bottom: 11px;
}

.form-control {
    margin-bottom: 0.5em;
}

.form-check-input {
    width: 2em !important;
    height: 2em !important;
}
</style>