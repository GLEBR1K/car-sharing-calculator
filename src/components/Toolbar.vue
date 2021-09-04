<template>
  <div id="toolbar">
    <Theme />
    <Share :duration="duration" :distance="distance" :wait="wait" />
  </div>
</template>

<script>
import Share from "./Share.vue";
import Theme from "./Theme.vue";

export default {
  name: "Toolbar",
  components: {
    Share,
    Theme,
  },
  props: {
    duration: Number,
    distance: Number,
    wait: Number,
  },
};
</script>

<style lang="scss" scoped>
#toolbar {
  position: absolute;
  top: 10px;
  right: 10px;

  display: flex;
  flex-direction: column;
}
</style>
