<template>
  <v-app>
    <v-main>
      <v-container>
        <Header />
        <Inputs
          :duration="duration"
          :distance="distance"
          :wait="wait"
          @change="onChange"
        />
        <Table
          :duration="duration"
          :distance="distance"
          :wait="wait"
          :tariffs="tariffs"
          :results="results"
        />
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
  data: () => ({
    duration: 30,
    distance: 30,
    wait: 10,
    tariffs: null,
  }),
  mounted() {
    fetch("/tariffs.json")
      .then((data) => data.json())
      .then((json) => (this.tariffs = json));
  },
  computed: {
    results() {
      var results = {};
      var min = Number.MAX_VALUE;

      if (this.tariffs) {
        this.tariffs.providers.forEach((provider) => {
          provider.tariffs.forEach((tariff) => {
            var price = tariff.prices.price || 0;
            var priceDuration = tariff.prices.duration || 0;
            var priceDistance = tariff.prices.distance || 0;
            var priceWait = tariff.prices.wait || 0;
            var includesDuration = tariff.includes.duration || 0;
            var includesDistance = tariff.includes.distance || 0;
            var includesWait = tariff.includes.wait || 0;

            if (includesWait === -1) {
              includesWait = Math.max(0, includesDuration - this.duration);
            }

            var result =
              price +
              Math.max(0, this.duration - includesDuration) * priceDuration +
              Math.max(0, this.distance - includesDistance) * priceDistance +
              Math.max(0, this.wait - includesWait) * priceWait;

            result = result || 0;
            min = result < min ? result : min;
            results[provider.id + ":" + tariff.id] = result;
          });
        });
      }

      results["$min"] = min;
      return results;
    },
  },
  methods: {
    onChange(key, value) {
      this[key] = parseFloat(value) || null;
    },
  },
};
</script>
