<template>
  <div class="filter">
    <div class="brand-filter">
      <label for="brand">Brand:</label>
      <input
        ref="brandInput"
        type="text"
        id="brand"
        :value="brandInputValue"
        @input="handleInput"
      />
      <button class="dropdown-btn" @click="toggleDropdown">
        <i class="fa-solid fa-angle-down"></i>
      </button>
      <ul ref="menu" class="dropdown-menu hidden">
        <template v-for="brandItem in brandList" :key="brandItem.brand_id">
          <li
            v-if="searchMatch(brandItem.brand_name)"
            @click="setBrand(brandItem)"
          >
            {{ brandItem.brand_name }}
          </li>
        </template>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'Filter',
  props: {},
  setup() {
    const loading = ref(true)
    const brandList = ref([])

    const fetchBrandData = async () => {
      const res = await fetch('https://api-mobilespecs.azharimm.site/v2/brands')
      const result = await res.json()
      return result.data
    }
    const baseSetup = async () => {
      const data = await fetchBrandData()
      brandList.value = data.map(({ brand_id, brand_name }) => {
        return { brand_id, brand_name }
      })
      loading.value = false
    }
    baseSetup()
    return {
      loading,
      brandList,
    }
  },
  data() {
    return { selectedBrand: null, brandInputValue: '' }
  },
  methods: {
    toggleDropdown() {
      this.$refs.menu.classList.toggle('hidden')
    },
    setBrand(brandItem) {
      this.selectedBrand = brandItem
      this.$refs.brandInput.value = brandItem.brand_name
    },
    searchMatch(name) {
      if (this.brandInputValue == '') return true
      return name
        .toLowerCase()
        .includes(this.brandInputValue.trim().toLowerCase())
    },
    handleInput(e) {
      if (e.target.value == '') {
        this.$refs.menu.classList.add('hidden')
        return
      }
      this.$refs.menu.classList.remove('hidden')
      this.brandInputValue = e.target.value
    },
  },
}
</script>

<style scoped>
.filter {
  margin: 40px 0;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.brand-filter {
  display: flex;
  align-items: center;
  position: relative;
}

label {
  margin-right: 5px;
}

#brand {
  color: #000;
}

button {
  font-size: 16px;
  padding: 0 3px;
}
button i {
  color: #000;
}

.dropdown-menu {
  position: absolute;
  top: 25px;
  left: 53px;
  width: 160px;
  margin: 0;
  padding: 0;
  border: 1px solid rgba(211, 211, 211, 0.8);
  border-radius: 2px;
  user-select: none;
  max-height: 200px;
  overflow: auto;
  scrollbar-width: thin;
}

.dropdown-menu li {
  padding: 3px 10px;
  opacity: 0.8;
}

.dropdown-menu li:hover {
  opacity: 1;
}

.dropdown-menu li.selected {
  opacity: 1;
}

.dropdown-menu li:not(:last-child) {
  border-bottom: 1px solid rgba(211, 211, 211, 0.8);
}

.dropdown-menu li:nth-child(even) {
  background-color: #555;
}
.dropdown-menu li:nth-child(odd) {
  background-color: #333;
}
</style>
