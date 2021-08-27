<template>
  <v-row dense justify="center">
    <v-col xl="8" lg="10">
      <v-fade-transition>
        <v-simple-table dense v-if="tariffs">
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
                <img
                  :src="PUBLIC_PATH + provider.image"
                  width="12"
                  height="12"
                />
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
              <td>
                {{ tariff.name }}
                <v-scroll-x-transition leave-absolute>
                  <v-badge
                    dot
                    inline
                    color="blue"
                    v-if="isMinByProvider(provider, tariff, results)"
                  >
                  </v-badge>
                </v-scroll-x-transition>
                <v-scroll-x-transition leave-absolute>
                  <v-badge
                    dot
                    inline
                    color="green"
                    v-if="isMinTotal(provider, tariff, results)"
                  >
                  </v-badge>
                </v-scroll-x-transition>
              </td>

              <td class="border-left">
                {{ formatCurrency(tariff.prices.price) }}
              </td>

              <td class="border-left">
                {{ valueOrDefault(tariff.includes.duration) }}
              </td>
              <td>
                {{ formatCurrency(tariff.prices.duration) }}
              </td>

              <td class="border-left">
                {{ valueOrDefault(tariff.includes.distance) }}
              </td>
              <td>
                {{ formatCurrency(tariff.prices.distance) }}
              </td>

              <td class="border-left">
                {{ valueOrDefault(tariff.includes.wait) }}
              </td>
              <td>
                {{ formatCurrency(tariff.prices.wait) }}
              </td>

              <td class="border-left font-weight-bold">
                {{ formatCurrency(results[provider.id + ":" + tariff.id]) }}
              </td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-fade-transition>
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
    getResult(provider, tariff) {
      return this.results[provider.id + ":" + tariff.id];
    },
    getMin(provider) {
      if (provider) {
        return this.results[provider.id + ":$min"];
      }
      return this.results["$min"];
    },
    isMinTotal(provider, tariff) {
      return this.getResult(provider, tariff) == this.getMin();
    },
    isMinByProvider(provider, tariff) {
      return this.getResult(provider, tariff) == this.getMin(provider);
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
