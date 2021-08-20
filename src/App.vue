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
import vueConfig from "../vue.config";
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
    duration: localStorage.getItem("duration") || 30,
    distance: localStorage.getItem("distance") || 30,
    wait: localStorage.getItem("wait") || 10,
    tariffs: null,
  }),
  created() {
    this.PUBLIC_PATH = vueConfig.publicPath;
  },
  mounted() {
    fetch(this.PUBLIC_PATH + "tariffs.json")
      .then((data) => data.json())
      .then((json) => (this.tariffs = json));
  },
  computed: {
    results() {
      var results = {};
      var minAll = Number.MAX_VALUE;

      if (this.tariffs) {
        this.tariffs.providers.forEach((provider) => {
          var minProvider = Number.MAX_VALUE;

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

            minAll = result < minAll ? result : minAll;
            minProvider = result < minProvider ? result : minProvider;

            results[provider.id + ":" + tariff.id] = result;
          });

          results[provider.id + ":$min"] = minProvider;
        });
      }

      results["$min"] = minAll;
      return results;
    },
  },
  methods: {
    onChange(key, value) {
      var parsed = parseFloat(value) || null;
      this[key] = parsed;

      if (parsed) {
        localStorage.setItem(key, parsed);
      } else {
        localStorage.removeItem(key);
      }
    },
  },
};
</script>
