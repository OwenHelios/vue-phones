<template>
  <div class="text-center">
    <div>
      API by
      <a
        class="text-blue-300"
        href="https://api-mobilespecs.azharimm.site/v2/"
        target="_blank"
        >azharimm</a
      >
      <p>{{ store.currentPhone }}</p>
    </div>
    <el-select v-model="value" filterable placeholder="Select">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
  </div>
</template>

<script>
import { ref } from 'vue'
import { useStore } from '@/store'

export default {
  name: 'Header',
  setup() {
    const store = useStore()
    const value = ref('')
    const options = [
      {
        value: 'Option1',
        label: 'Option1',
      },
      {
        value: 'Option2',
        label: 'Option2',
      },
      {
        value: 'Option3',
        label: 'Option3',
      },
      {
        value: 'Option4',
        label: 'Option4',
      },
      {
        value: 'Option5',
        label: 'Option5',
      },
    ]
    return {
      value,
      options,
      store,
    }
  },
}
</script>

<style scoped></style>
