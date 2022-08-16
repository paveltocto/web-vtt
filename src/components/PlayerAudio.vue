<template>
  <div class="audio-player">
    <audio
        crossorigin="anonymous"
        controls
        autoplay="autoplay"
        preload="auto"
        @loadeddata="onLoaded"
        ref="audioRef"
        style="display: none" >
      <source src="../assets/audio.mp3" />
      <track default
             kind="subtitles"
             src="../assets/transcript.vtt"/>
    </audio>
    <slot></slot>
  </div>

</template>

<script setup>
import {onMounted, reactive, ref, provide} from "vue";

// eslint-disable-next-line no-undef
const props = defineProps({
  preload: {
    type: Boolean,
    default: true
  },
  autoPlay: {
    type: Boolean,
    default: false
  }
});

const audioRef = ref(null)

const player =  reactive({
  loaded: false,
  track: null,
  status: null,
  duration: 0,
  currentTime: 0
})

provide('player', player)
provide('audioPlayer', audioRef)

onMounted(()=>{
  onLoadedTrack()
})

const onLoaded = (e) => {
  const target = e.target
  if (target.readyState >= 2) {
    player.status = 'loaded'
    player.duration = parseInt(target.duration)
    console.log(player)
    if (props.autoPlay) this.play()
  }
}

const onLoadedTrack =(tries=0)=> {
  tries += 1
  const currentTrack = audioRef.value.textTracks[0]
  if (currentTrack && currentTrack.cues && currentTrack.cues.length > 0) {
    player.track = currentTrack
    player.loaded = true
  } else if (!player.loaded) {
    const wait = 25 * Math.pow(tries, 2)
    setTimeout(onLoadedTrack, wait, tries)
  }
}
</script>

<style scoped>

</style>