<template>
  <div class="navbar">
     <v-app-bar
      app
      color="primary"
      dark
    >
      <v-app-bar-nav-icon v-if="user" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>
          {{ title }}
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <div v-if="user">
        {{ user.login }}
      </div>
      <v-btn icon @click="changeTheme()">
        <v-icon>mdi-theme-light-dark</v-icon>
      </v-btn>
    </v-app-bar>

     <v-navigation-drawer
                v-model="drawer"
                absolute
                temporary
                v-if="user"
        >
            <v-list-item>
                <v-list-item-avatar>
                    <v-img src="https://randomuser.me/api/portraits/men/78.jpg"></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                    <v-list-item-title>{{ user.nome }}</v-list-item-title>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense>
                <v-list-item link v-for="item in itens" :key="item.label" @click="changeMenu(item.label)">
                    <v-list-item-icon>
                      <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.label }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
             
                <v-divider></v-divider>
                <v-list-item link @click="logout()">
                    <v-list-item-icon><v-icon>mdi-keyboard-return</v-icon></v-list-item-icon>

                    <v-list-item-content>
                        <v-list-item-title>Sair</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
  </div>
</template>

<script>
import { clearUserSession } from "../../utils/utils"
export default {
    name: "NavBar",
    props: ["user", "title"],
    data() {
      return {
        drawer: false,
        itens: [
          { icon: "mdi-store", label: "Produtos" },
          { icon: "mdi-shopping", label: "Vendas" },
          { icon: "mdi-cash-multiple", label: "Promoções" },
          { icon: "mdi-account-multiple", label: "Usuários" }          
        ]
      }
    },
    methods: {
        changeTheme() {
            this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
        },
        logout() {
          clearUserSession();
        },
        changeMenu(item) {
          if (item === "Produtos") return this.$router.push("produtos");
          else if (item === "Vendas") return this.$router.push("vendas");
          else if (item === "Promoções") return this.$router.push("promocoes");
          else return this.$router.push("Usuários");
        }
    }
}
</script>