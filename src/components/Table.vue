<template>
  <v-row dense>
    <v-col>
      <v-simple-table dense>
        <thead>
          <tr>
            <th>Тариф</th>

            <th>Цена</th>

            <th>Минуты</th>
            <th>Руб./Мин.</th>

            <th>Километры</th>
            <th>Руб./Км.</th>

            <th>Минуты (ожидание)</th>
            <th>Руб./Мин. (ожидание)</th>

            <th>Сумма</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td colspan="9" class="font-weight-black">Anytime</td>
          </tr>
          <tr>
            <td>Минутный</td>

            <td>–</td>

            <td>–</td>
            <td>0.31</td>

            <td>–</td>
            <td>–</td>

            <td>–</td>
            <td>0.09</td>

            <td class="font-weight-bold green--text">10.20</td>
          </tr>

          <tr>
            <td colspan="9" class="font-weight-black">Hello</td>
          </tr>
          <tr>
            <td>Минутный</td>

            <td>–</td>

            <td>–</td>
            <td>0.33</td>

            <td>–</td>
            <td>–</td>

            <td>–</td>
            <td>0.08</td>

            <td class="font-weight-bold">10.70</td>
          </tr>
        </tbody>
      </v-simple-table>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "Table",
};
</script>
