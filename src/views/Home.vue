<template>
  <div></div>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable no-unused-vars */
import {
  fontSelecter,
  startButton,
  stopButton,
  textPresenter
} from "@/components";

navigator.getUserMedia =
  navigator.getUserMedia ||
  navigator.webkitGetUserMedia ||
  navigator.mozGetUserMedia;

export default {
  name: "home",
  data: function() {
    return {
      status: "",
      isDuration: false,
      text: null,

      // eslint-disable-next-line no-undef
      recognition: new webkitSpeechRecognition(),
      recognitionText: "Start",
      notes: []
    };
  },
  watch: {
    status: function() {
      if (this.status === "onend") {
        this.startSpeech();
      }
    }
  },
  created: function() {
    this.speechRecognize();
  },
  methods: {
    speechRecognize: function() {
      this.recognition.onstart = () => {
        this.status = "onstart";
        console.log(this.status);
        this.isDuration = true;
        this.recognitionText = "音声入力中...";
      };
      this.recognition.onspeechstart = () => {
        this.status = "onspeechstart";
        console.log(this.status);
      };
      this.recognition.onspeechend = () => {
        this.status = "onspeechend";
        console.log(this.status);
      };
      this.recognition.onend = () => {
        this.status = "onend";
        console.log(this.status);
        // this.recognitionText = "Start";
      };
      this.recognition.onresult = event => {
        this.status = "onresult";
        console.log(this.status);
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
      this.status = "startSpeech";
      console.log(this.status);
      this.isDuration = true;
      this.recognition.lang = "ja-JP";
      this.recognition.interimResults = true;
      this.recognition.start();
    },
    stopSpeech: function() {
      this.status = "stopSpeech";
      console.log(this.status);
      this.recognition.stop();
      this.status = "stop";
      console.log(this.status);
    }
  }
};
</script>
