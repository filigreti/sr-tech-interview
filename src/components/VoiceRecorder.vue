<template>
  <div>
    <button class="p-2 my-2 bg-gray-100" @click="toggleRecord">
      {{ !active ? "Record" : "Stop" }}
    </button>
  </div>
</template>

<script>
export default {
  data: () => ({
    stream: null,
    active: false,
    recordedChunks: [],
    mediaRecorder: null,
  }),
  mounted() {},
  methods: {
    async toggleRecord() {
      this.active = !this.active;
      await this.getMicMediaStream();

      // ...
    },
    download(blob) {
      var url = URL.createObjectURL(new Blob(blob));
      var a = document.createElement("a");
      document.body.appendChild(a);
      a.style = "display: none";
      a.href = url;
      a.download = "test.mp3";
      a.click();
      window.URL.revokeObjectURL(url);
    },
    async getMicMediaStream() {
      try {
        if (this.active) {
          this.stream = await navigator.mediaDevices.getUserMedia({
            audio: true,
            video: false,
          });
          this.mediaRecorder = new MediaRecorder(this.stream, {
            mimeType: "audio/webm",
          });
          let that = this;
          this.mediaRecorder.addEventListener("dataavailable", function(e) {
            if (e.data.size > 0) {
              console.log(e);
              that.recordedChunks.push(e.data);
              console.log(that.recordedChunks, "audio");
            }
          });
          this.mediaRecorder.start();
        } else {
          this.mediaRecorder.stop();
          this.download(this.recordedChunks);
        }
      } catch (e) {
        window.alert(e);
      }
    },
  },
};
</script>

if (!this.active) { console.log(this.recordedChunks, "data");
mediaRecorder.stop(); }

<style scoped></style>
