<script setup lang="ts">
import { computed } from 'vue'
import { Check, Heart, Plus, Star } from 'lucide-vue-next'
import { RouterLink } from 'vue-router'
import { useCartStore } from '@/stores/cart'
import { useWishlistStore } from '@/stores/wishlist'
import type { Product } from '@/types/product'
import { UiBadge, UiButton, UiCard } from '@/components/ui'

const props = defineProps<{ product: Product }>()
const cart = useCartStore()
const wishlist = useWishlistStore()
const quantity = computed(() => cart.quantity(props.product.id))
const saved = computed(() => wishlist.has(props.product.id))

function addToCart() {
  cart.add(props.product)
}

function toggleSaved() {
  wishlist.toggle(props.product.id)
}
</script>

<template>
  <UiCard as="article" class="group relative min-w-0 overflow-hidden rounded-[22px] p-2 transition duration-300 hover:-translate-y-1 hover:border-white/[0.14] hover:bg-[#181c22]">
    <div class="relative block aspect-[1.06] overflow-hidden rounded-[17px] bg-[#20252C]">
      <RouterLink :to="`/product/${product.slug}`" class="absolute inset-0" :aria-label="`View ${product.name}`"><img :src="product.image" :alt="product.name" class="h-full w-full object-cover opacity-65 mix-blend-screen transition duration-500 group-hover:scale-105 group-hover:opacity-80" /></RouterLink>
      <div class="absolute inset-0 bg-gradient-to-t from-[#14171C] via-transparent to-black/20"></div>
      <div class="absolute left-3 top-3 flex gap-1.5">
        <UiBadge v-if="product.isBestseller" tone="orange">Bestseller</UiBadge>
        <UiBadge v-else-if="product.isNew" tone="cyan">New</UiBadge>
      </div>
      <button aria-label="Save product" :aria-pressed="saved" class="absolute right-3 top-3 grid h-8 w-8 place-items-center rounded-full bg-[#0B0D10]/55 text-white/70 backdrop-blur transition hover:bg-[#0B0D10]/80 hover:text-white" @click.stop="toggleSaved"><Heart :size="15" :fill="saved ? 'currentColor' : 'none'" /></button>
      <div class="absolute bottom-3 left-3 right-3 flex items-end justify-between">
        <div><p class="mb-1 text-[10px] font-bold uppercase tracking-[0.13em] text-white/60">{{ product.brand }}</p><p class="rounded-md bg-white/10 px-2 py-1 text-xs font-bold text-white backdrop-blur">{{ product.viscosity }}</p></div>
        <div class="flex items-center gap-1 text-[11px] font-semibold text-white"><Star :size="12" fill="#F5A710" stroke="none" />{{ product.rating }}</div>
      </div>
    </div>
    <div class="px-2 pb-1 pt-4">
      <div class="mb-1 flex items-start justify-between gap-2"><RouterLink :to="`/product/${product.slug}`" class="line-clamp-2 text-sm font-semibold leading-snug text-white hover:text-[#F5A710]">{{ product.name }}</RouterLink><span class="shrink-0 text-[10px] text-[#8E96A3]">{{ product.volume }}</span></div>
      <p class="mb-4 text-[11px] text-[#8E96A3]">{{ product.base }} · {{ product.reviewCount }} reviews</p>
      <div class="flex items-center justify-between"><p class="display-font text-lg font-bold tracking-tight text-white">${{ product.price.toFixed(2) }}</p><UiButton size="icon-sm" class="w-auto px-2.5" :aria-label="`Add ${product.name} to cart`" @click="addToCart"><Check v-if="quantity" :size="17" stroke-width="3" /><Plus v-else :size="18" stroke-width="2.5" /></UiButton></div>
    </div>
  </UiCard>
</template>
