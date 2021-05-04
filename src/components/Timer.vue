<template>
  <v-container v-if="status !== 'Not Ready'" class="pa-0">
    <v-row class="pa-0">
      <v-tooltip v-model="tooltip" left>
        <template v-slot:activator="{ on, attrs }">
          <v-col
            class="text-right font-weight-light pa-0"
            v-on="on"
            v-bind="attrs"
          >
            {{ timer | time }}
          </v-col>
        </template>
        <span>Start Typing</span>
      </v-tooltip>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: ["status"],

  data() {
    return {
      timer: null,
      timerInterval: null,
    };
  },

  computed: {
    tooltip() {
      if (this.status === "Prompt Set") {
        return true;
      } else {
        return false;
      }
    },
  },

  methods: {
    startTimer() {
      this.timerInterval = setInterval(() => {
        this.timer--;
      }, 1000);
    },
  },

  filters: {
    time(num) {
      let minutes = parseInt(num / 60),
        seconds = num % 60;

      minutes = "0" + String(minutes);

      if (String(seconds).length < 2) {
        seconds = "0" + String(seconds);
      }

      return `${minutes}:${seconds}`;
    },
  },

  watch: {
    status: function () {
      if (this.status === 'Prompt Set') {
        return this.timer = 60;
      }

      if (this.status === "Testing") {
        return this.startTimer();
      }
    },
    timer: function () {
      if (this.timer === 0) {
        clearInterval(this.timerInterval);
        return this.$emit("time-expired");
      }
    },
  },
};
</script>

<style>
</style>