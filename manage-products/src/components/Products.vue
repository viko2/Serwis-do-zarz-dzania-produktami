<template lang="">
    <main-header/>
    <v-container class="category container">
      <v-card
        v-for="(product, index) in products"
        :key="index"
        :loading="loading"
        class="mx-auto my-12 category__product"
        max-width="374"
      >
        <v-img
          height="250"
          class="category__image mt-5" 
          :src="product.images[0]"
        ></v-img>
        <v-card-title class="category__title">{{product.title}}</v-card-title>
        <v-card-text>
          <v-row
            align="center"
            class="mx-0"
          >
          <v-price class="category__price">{{product.price}} $</v-price>
          </v-row>

          <div class="my-4 text-subtitle-1">
             {{product.category.name}}
          </div>

          <div class="category__description">{{product.description}}</div>
        </v-card-text>

        <v-divider class="mx-4"></v-divider>

        <v-card-actions>
          <v-btn
            color="deep-purple lighten-2"
            text
            @click="deleteProduct(product)"
          >
            Delete Product
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-container>
    <div class="text-center mb-5">
        <v-pagination
          v-model="page"
          :length="6"
          :total-visible="5"
          circle
          @click="fetch()"
        ></v-pagination>
      </div>
</template>
<script>
import { ref } from '@vue/reactivity'
import axios from 'axios'
import { onMounted } from '@vue/runtime-core';
import { useRouter } from 'vue-router';
import MainHeader from './MainHeader.vue'

export default {
    name: 'ProductsPage',
    components: { MainHeader },
    setup () {
      
      const products = ref([])
      const router = useRouter()
      const page = ref(1)
      const deleteProduct = async (product) => {
        axios.delete(`https://api.escuelajs.co/api/v1/products/${product.id}`)
        products.value = products.value.filter(x => x.id !== product.id)
      }
      const fetch = async () => {
        let result = await axios.get(`https://api.escuelajs.co/api/v1/products?limit=8&offset=${page.value}`)
        products.value = result.data
      }
      onMounted(() => {
        fetch()
        let user = localStorage.getItem('user')
        if (!user) {
          router.push('/')
        }
      });
    return {
      products,
      deleteProduct,
      fetch,
      page
    }
  },
}

</script>
<style lang="scss" scoped>
  .category {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    padding: 25px;
    margin: auto;
  }
</style>