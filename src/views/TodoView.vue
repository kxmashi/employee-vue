<template>
  <div>
    <h1>MGA TO DO</h1>
    <!-- <el-input v-model="inputTodo" placeholder="Please input"> </el-input>
    <el-button type="Primary" @click="addTodo">Mga puno</el-button> -->
    {{ specificPipil }}
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      inputTod: 'Test',
      mgaTodo: ['1', 'mukaon', 'matulog'],
    }
  },

  methods: {
    addTodo() {
      let angIadd = this.inputTodo
      this.mgaTod.push(angIadd)
    },
  },
})
</script>
