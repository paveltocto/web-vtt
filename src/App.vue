<template>
  <h3>Web VTT</h3>
  <player-audio :audio="require('@/assets/ja-bt-civics-adv-citizen-audio001.mp3')"
                :track="require('@/assets/ja-bt-civics-adv-citizen-audio001.vtt')" >
    <player-audio-caption></player-audio-caption>
  </player-audio>
</template>

<script setup>
import {PlayerAudio, PlayerAudioCaption} from "@/components/PlayerAudio";
</script>

<style>
</style>
