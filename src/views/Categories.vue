<template>
  <div>
    <div class="page-title">
      <h3>Категории</h3>
    </div>
    <section>
      <Loader v-if="loading" />

      <div class="row" v-else>

        <CategoryCreate @created="addNewCategory" />

        <CategoryEdit 
          v-if="categoreis.length"
          :categoreis="categoreis" 
          :key="categoreis.length + updateCount"
          @updated="updateCategories"
        />

        <p v-else class="center">Категорий пока нет</p>

      </div>
    </section>
  </div>
</template>

<script>
import CategoryCreate from '@/components/CategoryCreate'
import CategoryEdit from '@/components/CategoryEdit'

export default {
  name: 'categoreis',
  metaInfo() {
    return {
      title: this.$title('Menu_Categories')
    }
  },
  data: () => ({
    categoreis: [],
    loading: true,
    updateCount: 0
  }),
  async mounted() {
    this.categoreis = await this.$store.dispatch('fetchCategories')
    this.loading = false
  },
  components: {
    CategoryCreate, CategoryEdit
  },
  methods: {
    addNewCategory(category) {
      this.categoreis.push(category)
    },
    updateCategories(category) {
      const idx = this.categoreis.findIndex(c => c.id === category.id)
      this.categoreis[idx].title = category.title
      this.categoreis[idx].limit = category.limit
      this.updateCount++
    }
  }
}
</script>