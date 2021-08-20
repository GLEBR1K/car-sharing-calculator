<template>
  <v-row dense v-if="tariffs">
    <v-col>
      <v-simple-table dense>
        <thead>
          <tr>
            <th>Тариф</th>

            <th>Цена</th>

            <th>Время</th>
            <th>руб./мин.</th>

            <th>Расстояние</th>
            <th>руб./км.</th>

            <th>Ожидание</th>
            <th>руб./мин.</th>

            <th>Сумма</th>
          </tr>
        </thead>

        <tbody v-for="provider in tariffs.providers" :key="provider.id">
          <tr>
            <td colspan="9" class="font-weight-black">
              {{ provider.name }}
            </td>
          </tr>

          <tr v-for="tariff in provider.tariffs" :key="tariff.id">
            <td :class="colorize(provider, tariff, results)">
              {{ tariff.name }}
            </td>

            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.price) }}
            </td>

            <td :class="colorize(provider, tariff, results)">
              {{ valueOrDefault(tariff.includes.duration) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.duration) }}
            </td>

            <td :class="colorize(provider, tariff, results)">
              {{ valueOrDefault(tariff.includes.distance) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.distance) }}
            </td>

            <td :class="colorize(provider, tariff, results)">
              {{ valueOrDefault(tariff.includes.wait) }}
            </td>
            <td :class="colorize(provider, tariff, results)">
              {{ formatCurrency(tariff.prices.wait) }}
            </td>

            <td
              class="font-weight-bold"
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
export default {
  name: "Table",
  props: {
    duration: Number,
    distance: Number,
    wait: Number,
    tariffs: Object,
    results: Object,
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

      if (result == results["$min"]) {
        return ["green", "lighten-4"];
      }

      if (result == results[provider.id + ":$min"]) {
        return ["light-blue", "lighten-5"];
      }

      return {};
    },
  },
};
</script>
