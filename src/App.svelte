<script lang="ts">
  import "./app.css";
  let userMediaStrema: MediaStream;
  let screenMediaStrema: MediaStream;
  let videoTag: HTMLVideoElement;
  let screenVideoTag: HTMLVideoElement;
  let videoActive = true;
  let audioActive = true;
  let shareScreenActive = false;

  const shareScreen = () => {
    //
    shareScreenActive = !shareScreenActive;
    if (!shareScreenActive) {
      screenMediaStrema = null
      return;
    }
    navigator.mediaDevices
      .getDisplayMedia({ audio: true, video: true })
      .then((res) => {
        screenMediaStrema = res;
        screenVideoTag.srcObject = res;
        screenVideoTag.onloadedmetadata = () => {
          screenVideoTag.play();
        };
      });
  };

  const toggleVideo = () => {
    videoActive = !videoActive;
  };
  const toggleAudio = () => {
    audioActive = !audioActive;
  };
  navigator.mediaDevices
    .getUserMedia({ video: true, audio: true })
    .then((res) => {
      userMediaStrema = res;
      videoTag.srcObject = res;
      videoTag.onloadedmetadata = () => {
        videoTag.play();
        videoTag.muted = true;
      };
    })
    .catch((err) => {
      console.log(err);
    });
</script>

<main>
  <div class="videos">
    {#if shareScreenActive}
      <video bind:this={screenVideoTag} />
    {/if}
    <video bind:this={videoTag} />
  </div>
  <button on:click={toggleVideo}>
    {#if !videoActive}
      <div class="line" />
    {/if}
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-6 w-6"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
      stroke-width="2"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z"
      />
    </svg>
  </button>

  <button on:click={toggleAudio}>
    {#if !audioActive}
      <div class="line" />
    {/if}
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-6 w-6"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
      stroke-width="2"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"
      />
    </svg>
  </button>

  <button on:click={shareScreen}>
    {#if !shareScreenActive}
      <div class="line" />
    {/if}
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-6 w-6"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
      stroke-width="2"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
      />
    </svg>
  </button>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }
  video {
    max-width: 50%;
  }
  button {
    position: relative;
    background: none;
    text-align: center;
    border: none;
    color: white;
    width: 3rem;
    height: 3rem;
    border-radius: 0.5rem;
    background-color: #222;
    padding: 0.5rem;
    overflow: hidden;
  }
  button:hover {
    background-color: #444;
  }
  .videos {
    display: flex;
    margin-bottom: 0.5rem;
  }
  .line {
    position: absolute;
    top: 0;
    width: 2px;
    height: 150%;
    background-color: red;
    transform: rotate(40deg);
    animation: show 0.5s;
  }
  @keyframes show {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }
</style>
