<template>
  <div class="audio-player">
    <audio
        controls
        preload="auto"
        crossorigin="anonymous"
        @loadeddata="onLoaded"
        @timeupdate="onTimeUpdate"
        ref="audioRef" style="display: none">
      <source  :src="props.audio">
      <track  default :src="props.track" kind="subtitles" />
    </audio>
    <button @click="onPlay">Play</button>
    <slot></slot>
  </div>

</template>

<script setup>
import {onMounted, reactive, ref, provide} from "vue";

const audioRef = ref(null)
const player =  reactive({
  loadedTrack: false,
  track: null,
  status: null,
  muted: false,
  duration: 0,
  currentTime: 0
})

// eslint-disable-next-line no-undef
const props = defineProps({
  audio: {
    type: String,
    required: true
  },
  track: {
    type: String,
    required: true
  }
})

// eslint-disable-next-line no-undef
const emit = defineEmits(['on-play', 'on-muted', 'on-time-update'])

provide('playerStatus', player)
provide('audio', audioRef)

onMounted(()=>{
  onLoadedTrack()
})

const onLoaded = (e) => {
  const target = e.target
  if (target.readyState >= 2) {
    player.status = 'loaded'
    player.duration = parseFloat(target.duration)
  }
}

const onLoadedTrack =(tries=0)=> {
  tries += 1
  const currentTrack = audioRef.value.textTracks[0]
  if (currentTrack && currentTrack.cues && currentTrack.cues.length > 0) {
    player.track = currentTrack
    player.loadedTrack = true
  } else if (!player.loadedTrack) {
    const wait = 25 * Math.pow(tries, 2)
    setTimeout(onLoadedTrack, wait, tries)
  }
}

const onTimeUpdate = (e) => {
  const target = e.target
  player.currentTime = parseFloat(target.currentTime)
  emit('on-time-update', {element: audioRef.value, currentTime: player.currentTime})
}

const onPlay = () =>{
  if(player.status === 'playing' && !player.muted){
    audioRef.value.muted = true
    player.muted = audioRef.value.muted
    emit('on-muted', {element: audioRef.value})
  }else{
    audioRef.value.muted = false
    player.muted = audioRef.value.muted
    player.status = 'playing'
    audioRef.value.play()
    emit('on-play', {element: audioRef.value})
  }
}

// eslint-disable-next-line no-undef
defineExpose({
  onPlay,
  duration: player.duration,
  currentTime: player.currentTime
})
</script>

<style scoped>

</style>