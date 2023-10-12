<template>
  <div class="grid-container">
    <button @click="loadPreviousVideo" :disabled="currentVideoIndex === 0" class="previous-button">
      <span class="button-text">〈</span>
    </button>
    <video ref="videoPlayer" class="video-player" controls autoplay autofocus preload="auto" @ended="loadVideo"></video>
    <button @click="loadVideo" class="next-button">
      <span class="button-text">〉</span>
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiKey: import.meta.env.VITE_API_KEY,
      apiUrl: import.meta.env.VITE_API_URL,
      videoUrls: [], // Store video URLs
      currentVideoIndex: -1, // Index of the currently displayed video
    };
  },
  mounted() {
    this.loadVideo();
  },
  methods: {
    async loadVideo() {
      try {
        const response = await fetch(this.apiUrl, {
          headers: {
            'X-API-Key': this.apiKey,
          },
        });
        const data = await response.json();
        if (data.url) {
          // Add the video URL to the array
          this.videoUrls.push(data.url);
          this.currentVideoIndex = this.videoUrls.length - 1;
          this.$refs.videoPlayer.src = data.url;
        }
      } catch (error) {
        console.error('Error fetching video:', error);
      }
    },
    loadPreviousVideo() {
      if (this.currentVideoIndex > 0) {
        this.currentVideoIndex -= 1;
        this.$refs.videoPlayer.src = this.videoUrls[this.currentVideoIndex];
      }
    },
  },
};
</script>


<style scoped>
button {
  border: none;
  color: rgba(142, 129, 129, 0.86);
  font-size: 1.5rem;
  padding: 1rem;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  background:black;

}

.button-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 0;
  opacity: 0;
  transition: font-size 0.3s ease, opacity 0.3s ease;
}


button:hover .button-text {
  font-size: 3rem;
  opacity: 1;
}

*:focus {
  outline: none;
}

.video-player {
  width: 100%;
  height: 100vh;
}

.grid-container {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  justify-content: center;
  width: 100%;
}

.previous-button {
  grid-column: 1;
}

.next-button {
  grid-column: 3;

}
</style>