<script setup>
  import { useIntervalFn } from '@vueuse/core';
  import {ref} from "vue";

  const activePosition = ref(0);
  const speed = ref(200);

  const { pause, resume, isActive } = useIntervalFn(() => {
    if (activePosition.value > -525) {
      activePosition.value -= 75;
    }else {
      activePosition.value = 0;
    }
  }, speed);
</script>

<template>
  <!--div to display the sprite in-->
  <div>
    <input type="range" step="20" min="20" max="200" v-model="speed" />
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
