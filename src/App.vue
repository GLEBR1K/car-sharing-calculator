<template>
  <v-app>
    <v-main>
      <v-container>
        <Header />
        <Inputs />
        <Table />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Header from "./components/Header.vue";
import Inputs from "./components/Inputs.vue";
import Table from "./components/Table.vue";

export default {
  name: "App",
  components: {
    Header,
    Inputs,
    Table,
  },
};
</script>
