<template>
  <div class="search">
    <input type="text" />
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  props: {},
}
</script>

<style scoped>
.search {
  display: flex;
  align-items: center;
  justify-content: center;
}
input {
  font-size: 25px;
}
</style>
