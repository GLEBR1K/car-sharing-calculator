<template>
  <v-btn id="theme-toggle" icon plain @click="dark = !dark">
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

<style lang="scss" scoped>
#theme-toggle {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
