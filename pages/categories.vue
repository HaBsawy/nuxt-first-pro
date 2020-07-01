<template>
  <section class="category">
    <AddCategory></AddCategory>
    <ListCategories></ListCategories>
  </section>
</template>

<script>
import AddCategory from '../components/categories/AddCategory'
import ListCategories from '../components/categories/ListCategories'
export default {
  name: 'Categories',
  components: {
    AddCategory,
    ListCategories,
  },
}
</script>

<style lang="scss" scoped></style>
