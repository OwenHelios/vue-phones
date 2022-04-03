<template>
  <div class="header">
    <a
      ref="hamburger"
      @click="toggleSideBar"
      id="hamburger"
      class="hide-for-desktop"
    >
      <span></span>
      <span></span>
      <span></span>
    </a>
    <span>
      API by
      <a
        class="text-blue-300"
        href="https://api-mobilespecs.azharimm.site/v2/"
        target="_blank"
        >azharimm</a
      >
    </span>
  </div>
</template>

<script>
import { ref } from 'vue'
import { useStore } from '@/store'

export default {
  name: 'Header',
  setup() {
    const store = useStore()

    return {
      store,
    }
  },
  methods: {
    toggleSideBar() {
      this.$refs.hamburger.classList.toggle('open')
      this.store.$patch({
        sideBarOpen: !this.store.sideBarOpen,
      })
    },
  },
}
</script>

<style scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100%;
}
#hamburger {
  cursor: pointer;
}

#hamburger > span {
  display: block;
  width: 26px;
  height: 2px;
  background-color: darkblue;
  transition: all 300ms ease-in-out;
}
#hamburger > span:not(:last-child) {
  margin-bottom: 5px;
}
#hamburger.open > span:first-child {
  transform: rotate(45deg);
  transform-origin: 3.1px center;
}
#hamburger.open > span:nth-child(2) {
  opacity: 0;
}
#hamburger.open > span:last-child {
  transform: rotate(-45deg);
  transform-origin: 3.1px center;
}
</style>
