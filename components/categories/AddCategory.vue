<template>
  <section class="form">
    <h3>Create New Category</h3>
    <ValidationObserver ref="observer" v-slot="{ validate }">
      <form>
        <ValidationProvider
          v-slot="{ errors }"
          name="Name"
          rules="required|max:10"
        >
          <v-text-field
            v-model="name"
            :counter="10"
            :error-messages="errors"
            label="Name"
            required
          ></v-text-field>
        </ValidationProvider>
        <v-btn class="mr-4" @click="submit">submit</v-btn>
      </form>
    </ValidationObserver>
  </section>
</template>

<script>
import axios from 'axios'
import { required, email, max } from 'vee-validate/dist/rules'
import {
  extend,
  ValidationObserver,
  ValidationProvider,
  setInteractionMode,
} from 'vee-validate'

setInteractionMode('eager')

extend('required', {
  ...required,
  message: '{_field_} can not be empty',
})

extend('max', {
  ...max,
  message: '{_field_} may not be greater than {length} characters',
})

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
  },
  data: () => ({
    name: '',
  }),

  methods: {
    submit() {
      this.$refs.observer.validate()
      axios
        .post('http://localhost:9000/api/categories/', { name: this.name })
        .then((response) => {
          this.name = ''
        })
    },
  },
}
</script>

<style lang="scss" scoped>
.form {
  width: 600px;
  margin: 50px auto;
  border: 2px solid seagreen;
  padding: 20px;
}
</style>
