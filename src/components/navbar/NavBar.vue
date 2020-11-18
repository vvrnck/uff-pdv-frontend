<template>
     <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <div>Projeto de Software</div>
      </div>
      <v-spacer></v-spacer>

      <v-btn icon @click="changeTheme()">
        <v-icon>mdi-theme-light-dark</v-icon>
      </v-btn>
    </v-app-bar>
</template>

<script>
export default {
    name: "NavBar",
    methods: {
        changeTheme() {
            this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
        }
    }
}
</script>