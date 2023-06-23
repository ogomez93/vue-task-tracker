<template>
  <button
    class="btn"
    :style="{ background: color }"
    @click="onClick"
  >{{ text }}</button>
</template>

<script lang='ts'>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'AppButton',
  props: {
    text: String,
    color: String
  },
  methods: {
    onClick() {
      console.log('click')
    }
  }
})
</script>
