<template>
  <v-container fluid>
    <v-col col="12">
      <v-row justify="center">
        <h1 class="font-weight-thin">{{ text }}</h1>
      </v-row>
      <v-row justify="center">
        <v-btn
          large
          outlined
          color="primary"
          v-show="!state"
          @click="startSpeech"
          >️️️⚡️go⚡️</v-btn
        >
        <v-btn large outlined color="warning" v-show="state" @click="stopSpeech"
          >✋stop✋</v-btn
        >
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */

export default {
  name: "textPresenter",
  data: function() {
    return {
      state: Boolean,
      text: null,
      // eslint-disable-next-line no-undef
      recognition: new webkitSpeechRecognition()
    };
  },
  props: {
    font: String
  },
  created: function() {
    this.speechRecognize();
    this.state = false;
  },
  watch: {
    state: function() {
      if (!this.state) {
        this.stopSpeech();
      }
    }
  },
  methods: {
    speechRecognize: function() {
      this.recognition.onstart = () => {
        console.log("onstart");
      };
      this.recognition.onspeechstart = () => {
        console.log("onspeechstart");
      };
      this.recognition.onspeechend = () => {
        console.log("onspeechend");
      };
      this.recognition.onend = () => {
        if (this.state) {
          this.startSpeech();
        } else {
          this.stopSpeech();
        }
        console.log("onend");
      };
      this.recognition.onresult = event => {
        let interimTranscript = ""; // 暫定(灰色)の認識結果
        let finalTranscript = ""; // 確定した(黒の)認識結果
        for (let i = event.resultIndex; i < event.results.length; i++) {
          let transcript = event.results[i][0].transcript;
          if (event.results[i].isFinal) {
            finalTranscript += transcript;
          } else {
            interimTranscript = transcript;
          }
          this.text = finalTranscript + interimTranscript;
        }
      };
    },
    startSpeech: function() {
      this.recognition.lang = "ja-JP";
      this.recognition.interimResults = true;
      this.state = true;
      this.recognition.start();
    },
    stopSpeech: function() {
      this.state = false;
      this.recognition.stop();
    }
  }
};
</script>
