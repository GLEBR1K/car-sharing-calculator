<template>
  <v-row dence>
    <v-col>
      <v-form>
        <v-container>
          <v-row class="justify-center">
            <v-col xl="1" lg="2" md="2" sm="3" cols="12">
              <v-text-field
                :value="duration"
                @keyup="$emit('change', 'duration', $event.target.value)"
                label="Время"
                suffix="мин."
                required
                dense
              ></v-text-field>
            </v-col>

            <v-col xl="1" lg="2" md="2" sm="3" cols="12">
              <v-text-field
                :value="distance"
                @keyup="$emit('change', 'distance', $event.target.value)"
                label="Расстояние"
                suffix="км."
                required
                dense
              ></v-text-field>
            </v-col>

            <v-col xl="1" lg="2" md="2" sm="3" cols="12">
              <v-text-field
                :value="wait"
                @keyup="$emit('change', 'wait', $event.target.value)"
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
  props: {
    duration: Number,
    distance: Number,
    wait: Number,
  },
};
</script>
