<template>
<div class="audio-player__captions">{{textContentCue}}</div>
</template>

<script setup>
import {watch, inject, ref, computed, onBeforeUnmount} from "vue";

const player =  inject('playerStatus')
const currentCue =  ref(null)

const textContentCue = computed(()=>{
  if(!currentCue.value) return null
  return currentCue.value.getCueAsHTML().textContent
})

const handlerCueChange = (e) => {
  currentCue.value = e.target.activeCues[0]
}

watch(
    () => player.track,
    (newCurrentTrack) => {
      if(newCurrentTrack){
        newCurrentTrack.addEventListener('cuechange', handlerCueChange)
      }
    }, {immediate:true}
)

onBeforeUnmount(()=>{
  player.track.removeEventListener('cuechange', handlerCueChange)
})

</script>

<style scoped>

</style>