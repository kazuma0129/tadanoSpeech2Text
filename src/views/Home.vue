<template>
  <v-container fluid fill-height>
    <v-col col="12">
      <v-row align="center" justify="center">
        <textPresenter :font="font"></textPresenter>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable no-unused-vars */
import textPresenter from "../components/textPresenter";

export default {
  name: "home",
  components: {
    textPresenter
  },
  data: function() {
    return {
      font: String
    };
  },
  created: function() {},
  watch: {},
  methods: {}
};
</script>
