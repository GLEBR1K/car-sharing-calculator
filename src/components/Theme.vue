<template>
  <v-btn icon plain @click="dark = !dark">
    <v-icon>mdi-brightness-6</v-icon>
  </v-btn>
</template>

<script>
export default {
  name: "Theme",
  data() {
    return {
      dark: this.getTheme(),
    };
  },
  created() {
    this.$vuetify.theme.dark = this.dark;
  },
  methods: {
    getTheme() {
      return localStorage.getItem("dark") === "true";
    },
    setTheme(val) {
      localStorage.setItem("dark", val);
    },
  },
  watch: {
    dark(val) {
      this.$vuetify.theme.dark = val;
      this.setTheme(val);
    },
  },
};
</script>
