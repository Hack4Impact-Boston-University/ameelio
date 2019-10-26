<template>
  <v-card
    height="100%"
    width="100%"
    class="mx-auto"
    flat
    tile
  >
    <v-navigation-drawer permanent
        class="light-blue darken-3"
        dark
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">
            Hello!
          </v-list-item-title>
          <v-list-item-subtitle>
            Username
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-card>
</template>

<script>
  export default {
    data () {
      return {
        items: [
          { title: 'Dashboard', icon: 'mdi-view-dashboard' },
          { title: 'Calls', icon: 'mdi-image' },
          { title: 'About', icon: 'mdi-help-box' },
        ],
        right: null,
      }
    },
  }
</script>

<style>
.v-card{
    margin:0;
    padding:0;
}
</style>