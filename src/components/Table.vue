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
            <td>{{ tariff.name }}</td>

            <td>{{ valueOrDefault(tariff.prices.price) }}</td>

            <td>{{ valueOrDefault(tariff.includes.duration) }}</td>
            <td>{{ valueOrDefault(tariff.prices.duration) }}</td>

            <td>{{ valueOrDefault(tariff.includes.distance) }}</td>
            <td>{{ valueOrDefault(tariff.prices.distance) }}</td>

            <td>{{ valueOrDefault(tariff.includes.wait) }}</td>
            <td>{{ valueOrDefault(tariff.prices.wait) }}</td>

            <td
              class="font-weight-bold"
              :class="{
                'green--text': isMininalRange(
                  results[provider.id + ':' + tariff.id]
                ),
              }"
            >
              {{ results[provider.id + ":" + tariff.id].toFixed(2) }}
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
    isMininalRange(value) {
      return (
        value > this.results["$min"] && value <= this.results["$min"] * 1.1
      );
    },
  },
};
</script>
