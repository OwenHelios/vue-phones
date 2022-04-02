<template>
  <el-container class="container">
    <SideBar />
    <el-container>
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
  height: 100vh;
}
</style>
