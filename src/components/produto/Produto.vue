<template>
  <v-card
    class="ma-3"
    max-width="400"
  >
    <v-img
      :src="produto.urlImg"
      height="170px"
    ></v-img>

    <v-card-title>
      {{ produto.nome }}
    </v-card-title>

    <v-card-subtitle>
      Category
    </v-card-subtitle>

    <v-card-actions>
      <v-btn
        color="primary"
        dark
        text
        @click="reveal = true"
      >
        Mais
      </v-btn>

      <v-spacer></v-spacer>

      <!-- <v-btn
        icon
        @click="show = !show"
      >
        <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
      </v-btn> -->
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          {{ produto.descricao }}
        </v-card-text>
      </div>
    </v-expand-transition>

     <v-expand-transition>
      <v-card
        v-if="reveal"
        class="transition-fast-in-fast-out v-card--reveal d-flex flex-column"
        style="height: 100%;"
      >
        <v-card-text class="pb-0">
          <p class="display-1 text--primary">
            {{ produto.nome }}
          </p>
          <p>{{ produto.descricao }} </p>
        </v-card-text>
        <v-spacer></v-spacer>
        <v-card-actions class="pt-0">
          <v-btn
            text
            color="primary accent-4"
            @click="reveal = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-expand-transition>
  </v-card>
</template>

<script>
  export default {
    name: "Produto",
    props: ["produto"],
    data: () => ({
      show: false,
      reveal: false
    }),
  }
</script>

<style>
.v-card--reveal {
  bottom: 0;
  opacity: 1 !important;
  position: absolute;
  width: 100%;
}
</style>