<template>
  <el-aside
    class="sidebar"
    v-loading="loading"
    element-loading-text="Loading..."
    element-loading-background="rgba(0,0,0,0.6)"
  >
    <el-scrollbar ref="scrollbar">
      <header>
        <button>HBG</button>
      </header>
      <div class="brand">
        <el-select
          v-model="selectedBrand"
          @change="handleSelectBrand"
          filterable
          clearable
          placeholder="Select Brand"
          size="large"
        >
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <br />
      </div>
      <div class="phones">
        <el-menu v-if="phoneList">
          <el-menu-item
            v-for="phone in phoneList"
            :key="phone.slug"
            @click="handleSelectPhone(phone.slug)"
            >{{ phone.phone_name }}</el-menu-item
          >
        </el-menu>
        <el-pagination
          v-if="pages > 1"
          v-model:current-page="currentPage"
          @current-change="handlePageChange"
          background
          layout="prev, pager, next"
          :total="pages"
        />
      </div>
    </el-scrollbar>
  </el-aside>
</template>

<script>
import { ref } from 'vue'
import { useStore } from '@/store'

export default {
  name: 'SideBar',
  setup() {
    const selectedBrand = ref('')
    const options = ref([])
    const phoneList = ref([])
    const pages = ref(1)
    const currentPage = ref(1)
    const loading = ref(false)
    const store = useStore()
    const selectedPhone = ref('')

    const fetchBrandData = async () => {
      const res = await fetch('https://api-mobilespecs.azharimm.site/v2/brands')
      const result = await res.json()
      return result.data
    }
    const baseSetup = async () => {
      const data = await fetchBrandData()
      options.value = data.map(({ brand_name, brand_slug }) => {
        return { value: brand_slug, label: brand_name }
      })
    }
    const fetchPhoneList = async page => {
      let url =
        'https://api-mobilespecs.azharimm.site/v2/brands/' + selectedBrand.value
      if (page) {
        url =
          'https://api-mobilespecs.azharimm.site/v2/brands/' +
          selectedBrand.value +
          '?page=' +
          page
      }
      const res = await fetch(url)
      const result = await res.json()
      pages.value = result.data.last_page * 10
      return result.data.phones
    }
    const handleSelectBrand = async () => {
      loading.value = true
      currentPage.value = 1
      if (selectedBrand.value.length < 1) return
      const data = await fetchPhoneList()
      phoneList.value = data.map(({ phone_name, slug }) => {
        return { phone_name, slug }
      })
      loading.value = false
    }
    baseSetup()
    return {
      loading,
      selectedBrand,
      options,
      pages,
      baseSetup,
      handleSelectBrand,
      fetchPhoneList,
      phoneList,
      currentPage,
      store,
      selectedPhone,
    }
  },
  async mounted() {
    this.loading = true
    await this.baseSetup()
    this.loading = false
  },
  methods: {
    async handlePageChange(page) {
      this.loading = true
      const data = await this.fetchPhoneList(page)
      this.phoneList = data.map(({ phone_name, slug }) => {
        return { phone_name, slug }
      })
      this.loading = false
      this.$refs.scrollbar.setScrollTop(0)
    },
    handleSelectPhone(slug) {
      this.store.$patch({
        currentPhone: slug,
      })
    },
  },
}
</script>

<style scoped>
.sidebar {
  background-color: #999;
}
</style>
