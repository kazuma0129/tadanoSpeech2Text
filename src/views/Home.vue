<template>
  <v-container fluid fill-height>
    <v-col col="12">
      <v-row justify="center" align="center">
        <textPresenter v-show="isAvailavle" :font="font" />
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable no-unused-vars */
import textPresenter from '../components/textPresenter'

export default {
  name: 'Home',
  components: {
    textPresenter
  },
  data() {
    return {
      font: '',
      // eslint-disable-next-line no-undef
      isAvailavle: !!webkitSpeechRecognition
    }
  },
  watch: {},
  created() {},
  mounted() {},
  methods: {}
}
</script>
