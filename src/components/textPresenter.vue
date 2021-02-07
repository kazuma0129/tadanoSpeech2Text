<template>
  <v-container fluid>
    <v-col col="12">
      <v-row justify="center">
        <h1 class="font-weight-thin">
          {{ text }}
        </h1>
      </v-row>
      <v-row justify="center">
        <v-btn
          v-show="!state"
          large
          outlined
          color="primary"
          @click="startSpeech"
          >⚡️go⚡️</v-btn
        >
        <v-btn v-show="state" large outlined color="warning" @click="stopSpeech"
          >✋stop✋
        </v-btn>
      </v-row>
      <v-col class="mt-10">
        <v-row
          justify="center"
          v-for="(str, key) in results"
          :key="key"
          class="ma-3"
        >
          <p>{{ str }}</p>
        </v-row>
      </v-col>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */

export default {
  name: 'TextPresenter',
  props: {
    font: null
  },
  data: function () {
    return {
      state: Boolean,
      text: null,
      // eslint-disable-next-line no-undef
      recognition: new webkitSpeechRecognition(),
      results: []
    }
  },
  watch: {
    state: function () {
      if (!this.state) {
        this.stopSpeech()
      }
    }
  },
  created: function () {
    this.speechRecognize()
    this.state = false
  },
  methods: {
    speechRecognize: function () {
      this.recognition.onstart = () => {}
      this.recognition.onspeechstart = () => {}
      this.recognition.onspeechend = () => {}
      this.recognition.onend = () => {
        if (this.state) {
          this.startSpeech()
        } else {
          this.stopSpeech()
        }
      }
      this.recognition.onresult = (event) => {
        let interimTranscript = '' // 暫定(灰色)の認識結果
        let finalTranscript = '' // 確定した(黒の)認識結果
        for (let i = event.resultIndex; i < event.results.length; i++) {
          let transcript = event.results[i][0].transcript
          if (event.results[i].isFinal) {
            finalTranscript += transcript
            this.results = [...this.results, finalTranscript]
          } else {
            interimTranscript = transcript
          }
          this.text = finalTranscript + interimTranscript
        }
      }
    },
    startSpeech: function () {
      this.recognition.lang = 'ja-JP'
      this.recognition.interimResults = true
      this.state = true
      this.recognition.start()
    },
    stopSpeech: function () {
      this.state = false
      this.recognition.stop()
    }
  }
}
</script>
