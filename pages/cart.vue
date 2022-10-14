<template>
  <PageWrapper>
    <PageHeader>
      <PageTitle :text="$t('pages.cart.title')" class="capitalize" />
    </PageHeader>
    <PageBody>
      <PageSection>
        <p>{{ cart.cartItems }}</p>
        <div class="p-4 max-w-4xl mx-auto">
          <div v-if="!productStore.loaded" class="space-y-4">
            <CartCardSkeleton v-for="n in 15" :key="n" />
          </div>
          <div v-else-if="!cartItems.length">
            <h1 class="text-xl">Cart is empty.</h1>
          </div>
          <div v-else class="space-y-4">
            <CartCard
              v-for="(cartProduct, index) in cart.cartItems"
              :key="index"
              :cart-product="cartProduct"
            />
            <div class="text-right text-2xl md:text-4xl">
              Total: {{ toCurrency(cartStore.total) }}
            </div>
          </div>
        </div>
      </PageSection>
    </PageBody>
  </PageWrapper>
</template>

<script setup lang="ts">
// import { computed } from 'vue'
import { useProductStore } from '../stores/products'
import CartCard from '../components/CartCard.vue'
import CartCardSkeleton from '../components/CartCardSkeleton.vue'
import { toCurrency } from '../shared/utils'
import { userCart } from '@/stores/newcart'

const cart = userCart()
const productStore = useProductStore()

// const formattedCart = computed(() => cartStore.formattedCart)
</script>
