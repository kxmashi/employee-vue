<template>
  <div>Mao ni ako template</div>
  <div>Ako prop nadawat {{ prop1 }}</div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      numberRa: 1,
    }
  },
  props: ['prop1'],
})
</script>
