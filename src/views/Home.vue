<template>
  <div class="home">
    <NavBar :user="getUser()" :title="menu" />
    <v-main>
      Look, a home screen =)
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
