<template>
  <el-container class="container">
    <SideBar />
    <el-container class="right-container">
      <el-header><Header /></el-header>
      <el-main><router-view /></el-main>
    </el-container>
  </el-container>
</template>

<script>
import SideBar from '@/components/SideBar.vue'
import Header from '@/components/Header.vue'
export default {
  name: 'App',
  components: {
    SideBar,
    Header,
  },
}
</script>

<style>
.container {
  width: 100%;
}

.el-container {
  height: 100vh;
  transition: all 200ms ease-in-out;
}

.el-header {
  background-color: rgb(190, 37, 250);
}

@media (max-width: 640px) {
  .right-container {
    position: absolute;
    width: 100%;
  }
}
</style>
