<template>
  <main>
    <div class="a-spacing-large"></div>
    <div class="container-fluid browsing-history">
      <div class="row">
        <div class="col-sm-8 col-8">
          <h1 class="a-size-large a-spacing-none a-text-center-normal">All produts</h1>
          <nuxt-link to="/products" class="a-button-buy-again">Add a new product</nuxt-link>
          <nuxt-link to="/category" class="a-button-history">add a new category</nuxt-link>
          <nuxt-link to="/owner" class="a-button-history">add a new owner</nuxt-link>
        </div>
      </div>
    </div>
    <div class="container-fluid browsing-history">
      <div class="row">
        <div
          v-for="(product, index ) in products"
          :key="product._id"
          class="col-xs-2 col-lg-2 col-md-3 col-sm-6 br bb"
        >
          <div class="history-box">
            <a href class="a-link-normal">
                
                 <img :src="product.photo" class="img-fluid" alt srcset>
              </a>
            <div class="text-center">
              
              <a href="">
                <img src="" class="img-fluid" alt="" srcset="">
              </a>
              <div class="a-spacing-tap-base asin-title">
                <span class="a-text-normal">
                  <div class="p13n-sc-truncated">{{ product.title }}</div>
                </span>
              </div>
              <div class="a-row">
                <a href>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                  <i class="fas fa-star"></i>
                </a>
                <span class="a-letter-space">
                  <span class="a-color-tertiary a-size-small asin-reviews">(1732)</span>
                </span>
              </div>
              <div class="a-row">
                <span class="a-size-base a-color-price">
                  <span class="p13n-sc-price">${{ product.price }}</span>
                </span>
              </div>
              <div class="a-row">
                <nuxt-link
                  :to="`/products/${product._id}`"
                  class="a-button-history margin-right-110"
                >update</nuxt-link>
                <a
                  href
                  class="a-button-history margin-right-110"
                  @click="onDeleteProduct(product._id, index)"
                >delete</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    try {
      let response = await $axios.$get('http://localhost:5000/api/products')
console.log(response)
      return {
        products: response.products
      }
    } catch (error) {}
  },
  methods: {
    async onDeleteProduct(id, index) {
      try {
        let response = await this.$axios.$delete(
          `http://localhost:5000/api/products/${id}`
        )

        if (response.status) {
          this.products.splice(index, 1)
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>
