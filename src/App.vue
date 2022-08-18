<template>
  <h3>Vue Audio VTT</h3>
  <player-audio audio="https://biztownmediadev.blob.core.windows.net/audios/33/c7/15/33c71515-e1b1-42c4-bae5-e722b37834bc/ja-bt-civics-adv-citizen-audio001.mp3"
                track="https://biztownmediadev.blob.core.windows.net/subtitles/30/67/3b/30673bac-f0c7-4188-8794-775f1b186044/ja-bt-civics-adv-citizen-audio001.vtt" >
    <player-audio-caption></player-audio-caption>
  </player-audio>
</template>

<script setup>
import {PlayerAudio, PlayerAudioCaption} from "@/components/PlayerAudio";
</script>

<style>
</style>
