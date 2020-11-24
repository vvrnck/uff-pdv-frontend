<template>
  <div class="home">
    <NavBar :user="getUser()" :title="menu" />
    <v-main>
      <v-container>
          <v-col
              cols="12"
              md="3"
          >
            Look, a home screen =)
          </v-col>
      </v-container>
    </v-main>
  </div>
</template>

<script>

import { NavBar } from '@/components'
import { getUser } from '../utils/utils';

export default {
  name: 'Home',
  data() {
    return {
      menu: "Home",
    }
  },
  components: {
    NavBar
  },
  methods: {
    getUser(){ return getUser(); }
  }
}
</script>
