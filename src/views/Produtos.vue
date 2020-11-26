<template>
    <div class="produtos">
        <NavBar :user="getUser()" :title="menu" />
        <v-main>
             <v-container>
                    <NovoProduto :dialog="dialog" :close="close" />
                    <v-row>
                        <v-btn
                            rounded
                            color="primary"
                            outlined
                            raised
                            text
                            elevation="2"
                            @click="dialog = !dialog"
                        >
                            
                            Adicionar
                            <v-icon
                                right
                                dark
                            >
                                mdi-plus
                            </v-icon>
                        </v-btn>
                    </v-row>
                  <v-flex d-flex>
                    <v-layout wrap>
                        <v-flex md3 v-for="produto in produtos" :key="produto.id">
                            <Produto :produto="produto" />
                        </v-flex>
                    </v-layout>
                  </v-flex>
             </v-container>
        </v-main>
    </div>
</template>

<script>
import { NavBar, Produto, NovoProduto } from "../components/index";
import { getUser, getApiURL } from "../utils/utils";
import axios from "axios";

export default {
    name: "Produtos",
    components: { NavBar, Produto, NovoProduto },
    data() {
        return {
            menu: "Produtos",
            produtos: [],
            dialog: false,
        }
    },
    mounted() {
        this.getAllProducts();
    },
     watch: {
        // call again the method if the route changes
        '$route': 'getAllProducts'
    },
    methods: {
        getAllProducts() {
            const url = getApiURL() + 'produto/all';
            axios.get(url).then(response => {
                const { data } = response;
                this.produtos = data;
            })
        },
        close() {
            this.dialog = false;
        },
        getUser(){ return getUser(); },
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