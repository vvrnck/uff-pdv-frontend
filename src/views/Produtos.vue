<template>
    <div class="produtos">
        <NavBar :user="getUser()" :title="menu" />
        <v-main>
            <div class="produtos-items">
                <div v-for="i in 5" :key="i">
                    <Produto />
                </div>
            </div>
        </v-main>
    </div>
</template>

<script>
import { NavBar, Produto } from "../components/index";
import { getUser, getApiURL } from "../utils/utils";
import axios from "axios";

export default {
    name: "Produtos",
    components: { NavBar, Produto },
    data() {
        return {
            menu: "Produtos",
            produtos: [],
        }
    },
    mounted() {
        const url = getApiURL + 'produto/all';
        axios.get(url).then(response => {
            const { data } = response;
            this.produtos = data;
        })
    },
    methods: {
        getUser(){ return getUser(); }
    }
}
</script>

<style>

.produtos-items {
    display: grid;
    grid-auto-flow: column;
    grid-gap: 20px;
    margin: 20px;
}

</style>