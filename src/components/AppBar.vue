<template>
  <v-app-bar app dark flat color="blue">
    <v-container fluid class="px-0">
      <v-row class="flex" justify="space-between" align="center">
        <v-col cols="auto" class="d-flex align-center text-h5">
          <v-icon large class="mr-2">mdi-alpha-t-box</v-icon>Type Test
        </v-col>
         <v-col class="text-h5 px-0 d-flex justify-end" cols="auto">
          <timer :status="status" v-on="$listeners"></timer>
        </v-col>
      </v-row>
    </v-container>
  </v-app-bar>
</template>

<script>
import Timer from './Timer.vue';

export default {
  props: ['status'],

  components: { Timer }

};
</script>

<style>
</style>