<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="a-text-center">Add a new category</h2>
            <form action>
              <div class="a-spacing-top-medium">
                <label for>Type</label>
                <input type="text" class="a-input-text" v-model="type">
              </div>

              <hr>
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddCategory">Add category</span>
                  </span>
                </span>
              </div>
            </form>
            <ul class="list-group-item">
              <li v-for="category in categories" :key="category._id">{{ category.type }}</li>
            </ul>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>
<script>
import axios from 'axios'
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get('http://localhost:5000/api/categories');
       return {
        categories: response.categories,
        
      }
    } catch (error) {
      console.log(error)
    }
  },
  data() {
    return {
      type: ''
    }
  },
  methods: {
    async onAddCategory() {
      try {
        // POST request using axios with async/await
        const article = { type: this.type }
        const response = await axios.post(
          'http://localhost:5000/api/categories',
          article
        );
        if(response.status){
            this.categories.push(article)
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>
