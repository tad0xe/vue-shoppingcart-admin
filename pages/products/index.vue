<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="a-text-center">Add a new product</h2>
            <form>
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
                <input type="text" class="a-input-text" v-model="title">
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
              <!-- Photo file -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected">
                    <p style="margin-top: -70px">{{ fileName }}</p>
                  </label>
                </div>
              </div>
              <hr>
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onAddProduct">Add product</span>
                  </span>
                </span>
              </div>
            </form>
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
      let categories = await $axios.$get('http://localhost:5000/api/categories')
      let owners = await $axios.$get('http://localhost:5000/api/owners')
      const [catResponse, ownerResponse] = await Promise.all([
        categories,
        owners
      ])
      console.log(catResponse)
      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners
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
      selectedFile: null,
      stockQuantity: 1,
      fileName: '',
      photo:null
    }
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0]
      console.log(this.selectedFile)
      this.fileName = event.target.files[0].name
    },
    async onAddProduct() {
      
      // POST request using axios with async/await
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("ownerID", this.ownerID);
      data.append("stockQuantity", this.stockQuantity);
      data.append("categoryID", this.categoryID);
      
      data.append("photo", this.selectedFile,this.selectedFile.name);
      const response = await axios
        .post('http://localhost:5000/api/products', data)
        .then(() => {
          console.log(response)
          this.$router.push('/')
        })
    }
  }
}
</script>