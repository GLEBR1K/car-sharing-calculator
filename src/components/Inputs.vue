<template>
  <v-row dence>
    <v-col>
      <v-form>
        <v-container>
          <v-row class="justify-center">
            <v-col cols="12" md="2">
              <v-text-field
                v-model="duration"
                label="Время"
                suffix="мин."
                required
                dense
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="2">
              <v-text-field
                v-model="distance"
                label="Расстояние"
                suffix="км."
                required
                dense
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="2">
              <v-text-field
                v-model="wait"
                label="Ожидание"
                suffix="мин."
                required
                dense
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "Inputs",
  data: () => ({
    duration: 30,
    distance: 30,
    wait: 10,
  }),
};
</script>
