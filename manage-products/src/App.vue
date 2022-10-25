<template>
  <v-app id="app">
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',

  data () {
    return {
      //
    }
  },
})
</script>
<style lang="scss">
  #app {
    background-color: #f1f5f7;
    background-repeat: no-repeat;
    background-size: cover;
  }
</style>
