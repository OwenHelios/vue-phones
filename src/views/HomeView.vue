<template>
  <div class="home">{{ phoneDisplayed }}</div>
</template>

<script>
import { useStore } from '@/store'

export default {
  name: 'HomeView',
  components: {},
  setup() {
    const store = useStore()
    const phoneDisplayed = store.currentPhone
    return {
      store,
      phoneDisplayed,
    }
  },
}
</script>
