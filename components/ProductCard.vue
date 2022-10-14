<template>
  <div class="card bordered">
    <figure class="px-8 pt-10">
      <img
        :src="product.image"
        alt="Card Image"
        class="object-contain w-full h-64"
      />
    </figure>
    <div class="card-body">
      <h2 class="card-title">
        <router-link class="link link-hover" :to="`/product/${product.id}`">{{
          product.title
        }}</router-link>
      </h2>
      <p>{{ toCurrency(product.price) }}</p>
      <div class="justify-end card-actions">
        <button class="btn btn-primary" @click="cart.addToCart(product)">
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Product } from '../stores/products'
import { toCurrency } from '../shared/utils'
import { userCart } from '@/stores/newcart'

const cart = userCart()

defineProps<{
  product: Product
}>()
</script>
