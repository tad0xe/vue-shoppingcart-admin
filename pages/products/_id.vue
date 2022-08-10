<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="a-text-center">update {{ product.title }}</h2>
            <form action>
              <div class="a-spacing-top-medium">
                <label for>Category</label>
                <select name v-model="categoryID" class="a-select-option" id>
                  <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                  >{{ category.type }}</option>
                </select>
              </div>
              <div class="a-spacing-top-medium">
                <label for>Owner</label>
                <select name v-model="ownerID" class="a-select-option" id>
                  <option
                    v-for="owner in owners"
                    :value="owner._id"
                    :key="owner._id"
                  >{{ owner.name }}</option>
                </select>
              </div>
              <div class="a-spacing-top-medium">
                <label for>Title</label>
                <input type="text" class="a-input-text" v-model="title" :placeholder="product.title">
              </div>
              <div class="a-spacing-top-medium">
                <label for>description</label>
                <input type="text" class="a-input-text" v-model="description">
              </div>
              <div class="a-spacing-top-medium">
                <label for>price</label>
                <input type="number" class="a-input-text" v-model="price">
              </div>
              <div class="a-spacing-top-medium">
                <label for>stock quantity</label>
                <input type="number" class="a-input-text" v-model="stockQuantity">
              </div>
              <hr>
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct">Add product</span>
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3">!</div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ $axios, params }) {
    try {
      let categories = $axios.$get('http://localhost:5000/api/categories')
      let owners = $axios.$get('http://localhost:5000/api/owners')
      let product = $axios.$get(
        `http://localhost:5000/api/products/${params.id}`
      )
      const [catResponse, ownerResponse, productResponse] = await Promise.all([
        categories,
        owners,
        product
      ])
      console.log(productResponse)
      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product
      }
    } catch (error) {
      console.log(error)
    }
  },
  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: '',
      price: 0,
      description: '',
      stockQuantity: ''
    }
  },
  methods: {
    async onUpdateProduct() {
      // POST request using axios with async/await
      const article = { title: this.title, price: this.price }
      const response = await axios
        .put(`http://localhost:5000/api/products/${this.$route.params.id}`, article)
        .then(() => {
          this.$router.push('/')
        })
    }
  }
}
</script>