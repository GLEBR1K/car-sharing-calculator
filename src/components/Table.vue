<template>
  <v-row dense v-if="tariffs">
    <v-col>
      <v-simple-table dense>
        <thead>
          <tr>
            <th>Тариф</th>

            <th class="border-left">Цена</th>

            <th class="border-left">Время</th>
            <th>руб./мин.</th>

            <th class="border-left">Расстояние</th>
            <th>руб./км.</th>

            <th class="border-left">Ожидание</th>
            <th>руб./мин.</th>

            <th class="border-left">Сумма</th>
          </tr>
        </thead>

        <tbody v-for="provider in tariffs.providers" :key="provider.id">
          <tr>
            <td colspan="9" class="provider-row">
              <img :src="PUBLIC_PATH + provider.image" width="12" height="12" />
              <span class="font-weight-black">{{ provider.name }}</span>
              <span v-if="provider.website">
                •
                <a
                  class="text-decoration-none"
                  :href="provider.website.url"
                  target="_blank"
                  >{{ provider.website.title }}</a
                >
              </span>
            </td>
          </tr>

          <tr v-for="tariff in provider.tariffs" :key="tariff.id">
            <td :class="colorize(provider, tariff, results)">
              {{ tariff.name }}
            </td>

            <td
              class="border-left"
              :class="colorize(provider, tariff, results)"
            >
              {{ formatCurrency(tariff.prices.price) }}
            </td>

            <td
              class="border-left"
              :class="colorize(provider, tariff, results)"
            >
              {{ valueOrDefault(tariff.includes.duration) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.duration) }}
            </td>

            <td
              class="border-left"
              :class="colorize(provider, tariff, results)"
            >
              {{ valueOrDefault(tariff.includes.distance) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.distance) }}
            </td>

            <td
              class="border-left"
              :class="colorize(provider, tariff, results)"
            >
              {{ valueOrDefault(tariff.includes.wait) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.wait) }}
            </td>

            <td
              class="border-left font-weight-bold"
              :class="colorize(provider, tariff, results)"
            >
              {{ formatCurrency(results[provider.id + ":" + tariff.id]) }}
            </td>
          </tr>
        </tbody>
      </v-simple-table>
    </v-col>
  </v-row>
</template>

<script>
import vueConfig from "../../vue.config";

export default {
  name: "Table",
  props: {
    duration: Number,
    distance: Number,
    wait: Number,
    tariffs: Object,
    results: Object,
  },
  created() {
    this.PUBLIC_PATH = vueConfig.publicPath;
  },
  methods: {
    valueOrDefault(value) {
      return !value || value === -1 ? "–" : value;
    },
    formatCurrency(value) {
      if (value) {
        return value.toFixed(2);
      }
      return this.valueOrDefault(value);
    },
    colorize(provider, tariff, results) {
      var result = results[provider.id + ":" + tariff.id];

      if (result == 0) {
        return;
      }

      if (result == results["$min"]) {
        return ["green", "lighten-5"];
      }

      if (result == results[provider.id + ":$min"]) {
        return ["light-blue", "lighten-5"];
      }

      return;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~vuetify/src/styles/styles.sass";
thead tr th {
  border-top: thin solid map-get($material-light, "dividers");
  border-bottom: none !important;
}
.provider-row {
  border-top: thin solid map-get($material-light, "dividers");

  img {
    margin-right: 5px;
  }
}
.border-left {
  border-left: thin solid map-get($material-light, "dividers");
}
</style>
