<template>
  <div>
    <v-btn icon plain @click="share()">
      <v-icon>mdi-share-variant</v-icon>
    </v-btn>
    <v-snackbar
      bottom
      right
      text
      color="primary"
      transition="slide-y-reverse-transition"
      elevation="10"
      v-model="snackbar"
      :timeout="3000"
    >
      {{ message }}
    </v-snackbar>
  </div>
</template>

<script>
export default {
  name: "Share",
  props: {
    duration: Number,
    distance: Number,
    wait: Number,
  },
  data() {
    return {
      snackbar: false,
      message: null,
    };
  },
  methods: {
    share() {
      const url = encodeURI(
        `${window.location.origin}${window.location.pathname}?duration=${
          this.duration || 0
        }&distance=${this.distance || 0}&wait=${this.wait || 0}`
      );
      navigator.clipboard.writeText(url);
      this.message = `Ссылка скопирована: ${url}`;
      this.snackbar = true;
    },
  },
};
</script>
