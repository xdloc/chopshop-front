<script>
import axios from "axios";

let BACKEND = 'https://siberia.vps.webdock.cloud/';
let API_URL = BACKEND + `?controller=`
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
    watch: {},
    methods: {
        addItem() {
            if (this.newItem.length > 0) {
                //this.items.push({id: id++, name: this.newItem, created_at: '', updated_at: '', status: 0})
                axios
                    // ListController ADD
                    .get(API_URL + 'list&method=add&name=' + this.newItem, config)
                    .then(response => {
                        if (response) {
                            this.newItem = ''
                        }
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
                // ListController REMOVE
                .get(API_URL + 'list&method=remove&id=' + item.id, config)
                .then(response => {
                    this.fetchData()
                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        },
        markItem(item) {
            axios
                // todo ListController Mark
                .get(API_URL + 'list&method=mark&id=' + item.id + '&value=' + item.status, config)
                .then(response => {

                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        },
        async fetchData() {
            axios
                .get(API_URL + 'list&method=list', config)
                .then(response => {
                    console.log(response);
                    this.items = response.data
                    return response.data;
                })
                .catch(error => {
                    console.log(error)
                    this.failure = true;
                })
        }
    },
    ready: function () {
        this.fetchData()
    },
    created: function () {
        this.fetchData()
    },
}
</script>

<template>
    <section>
        <div>
            <form @submit.prevent="addItem" class="row row-cols-lg-auto g-3 align-items-center">

            <div class="col-12">
                <input v-model="newItem" class="form-control form-control-lg" placeholder="Apples, Milk, Pineapple juice e.g.">
            </div>

            <div class="col-12">
                <button @click="addItem()" type="button" class="btn btn-lg btn-outline-primary btn-add"><i class="fa-solid fa-plus"></i>Add Item</button>
            </div>
            </form>
        </div>
    </section>

    <section v-if="failure">
        <div>
            <b-alert show dismissible transition="expand">
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
                <div class="container text-left">
                    <div class="row">
                        <div class="col col-lg-1 col-md-2">
                            <input class="form-check-input me-1" type="checkbox" v-model="item.checked" @click="markItem(item)"
                                   :id="'shop-list-row-'+item.id+''">
                        </div>
                        <div class="col col-lg-9 col-md-8 col-sm-auto ">
                            <div class="label">
                                <label class="form-check-label stretched-link" :for="'shop-list-row-'+item.id+''">{{ item.name }}
                                </label>
                            </div>
                        </div>
                        <div class="col col-lg-2 align-content-lg-end">
                            <!--<button @click="editItem(item)" type="button" class="btn btn-dark btn-change"><i class="fa-solid fa-pen-to-square"></i>CHANGE</button>-->
                            <button @click="removeItem(item)" type="button" class="btn btn-dark btn-change float-end"
                                    :id="'shop-list-remove-item-'+item.id+''"><i
                                    class="fa-solid fa-pen-to-square"></i>REMOVE
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

section {
    margin-top: 1em;
}

.btn {
    border-radius: 18px;
    padding: 6px;
}
.btn-dark{
    margin-top: 0.5em;
}
.btn-add{
    margin-top: -0.5em;
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
    min-width: fit-content;
}

.btn-outline-secondary {
    border-color: #00bd7e !important;
    min-width: fit-content;
}

.list-group {
    margin-top: 1em;
}

.list-group-item {
    background: none;
    background-color: #33485e38 !important;
    backdrop-filter: blur(50);
}

.label {
    color: whitesmoke;
    padding-top: 13px !important;
}

.form-check-input[type=checkbox] {
    border-radius: .25em;
    margin-top: 11px;
    margin-bottom: 11px;
}

.form-control {
    margin-bottom: 0.5em;
    padding: 5px 35px;
}

.form-check-input {
    width: 2em !important;
    height: 2em !important;
}
</style>