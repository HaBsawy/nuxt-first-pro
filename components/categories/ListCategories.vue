<template>
  <section class="container">
    <h2>Categories</h2>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Control</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in categories" :key="item.id">
            <td>{{ item.name }}</td>
            <td>
              <v-btn @click.stop="openEditModal(item)">
                edit
              </v-btn>
              <v-btn @click.stop="openDeleteModal(item)">
                delete
              </v-btn>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Edit Category</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="12" md="12">
                <v-text-field
                  label="Name*"
                  v-model="selectedItem.name"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false"
            >Close</v-btn
          >
          <v-btn color="blue darken-1" text @click="editCategory">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="deleteDialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Edit Category</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <h4 class="text-center">
                Do you want to delete {{ selectedItem.name }} ??
              </h4>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="deleteDialog = false"
            >Close</v-btn
          >
          <v-btn color="blue darken-1" text @click="deleteCategory">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </section>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      dialog: false,
      deleteDialog: false,
      selectedItem: {},
      categories: [],
    }
  },
  methods: {
    openEditModal(item) {
      this.dialog = true
      this.selectedItem = item
    },
    openDeleteModal(item) {
      this.deleteDialog = true
      this.selectedItem = item
    },
    editCategory() {
      let url = 'http://localhost:9000/api/categories/' + this.selectedItem.id
      axios.put(url, this.selectedItem).then((response) => {
        this.selectCategory = {}
        this.dialog = false
      })
    },
    deleteCategory() {
      axios
        .delete('http://localhost:9000/api/categories/' + this.selectedItem.id)
        .then((response) => {
          this.selectCategory = {}
          this.deleteDialog = false
        })
    },
  },
  created() {
    axios.get('http://localhost:9000/api/category').then((response) => {
      this.categories = response.data
    })
  },
}
</script>

<style lang="scss" scoped>
.v-dialog__container {
  display: block;
}
</style>
