<template>
  <v-app>
    <!-- <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
        <span class="font-weight-light">MATERIAL DESIGN</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-toolbar> -->

    <v-content>
      
      <new-tuan/>
    </v-content>
  </v-app>
</template>

<script>
import NewTuan from "@/components/NewTuan.vue";
import config from "./config";

export default {
  name: "App",
  components: {
    NewTuan
  },
  data() {
    return {};
  },
  mounted() {}
};
</script>
