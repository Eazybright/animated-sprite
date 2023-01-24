<script setup>
  import { useRafFn } from '@vueuse/core';
  import {ref} from "vue";

  const activePosition = ref(0);
  const framesComplete = ref(0); // keep up with how many passed frames
  const speed = ref(10); // change speed default to better match new methodology

  const { pause, resume, isActive } = useRafFn(() => {
    // increment framesComplete each animation frame
    framesComplete.value++;
    // return early if frames complete is not a multiple of 10 (or speed.value)
    // so that the postition is not altered until every 10th animation frame
    if (framesComplete.value % speed.value) return;

    if (activePosition.value > -525) {
      activePosition.value -= 75;
    }else {
      activePosition.value = 0;
    }
  });
</script>

<template>
  <!--div to display the sprite in-->
  <div>
    <input type="range" step="1" min="1" max="20" v-model="speed" />
    <button @click="isActive ? pause() : resume()">
        {{ isActive ? 'Pause' : 'Resume' }}
    </button>
  </div>
  <div 
    class="sprite" 
    :style="{ 'background-position': activePosition + 'px 50%' }"
  ></div>
</template>

<style>
  input[type='range'] {
    transform: scaleX(-1);
  }
  .sprite {
    /* display the image*/
    background: url(https://freesvg.org/img/1525205509.png) no-repeat;

    /* each frame is 75px wide so limit container to display one at a time */
    width: 75px; 

    /* main is roughly 150px tall */
    height: 150px;

    /* the image has some space on top and bottom so this accounts for that */
    background-position: 0px 50%;

  }
</style>
