<template>
  <v-app dark>
      <v-app-bar app>
      <v-toolbar-title>
        <nuxt-link to="/">Metacast </nuxt-link>
      </v-toolbar-title>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {}
  }
}
</script>
