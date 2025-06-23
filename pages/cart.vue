<template>
  <div class="min-h-[80vh] w-full bg-gradient-to-br from-[#23272f] via-[#282c34] to-[#23272f] py-12 flex flex-col">
    <h2 class="text-3xl font-extrabold text-white mb-8 text-center tracking-tight drop-shadow-lg">{{ $t('cart.title') }}</h2>
    <div class="flex-1 grid grid-cols-1 md:grid-cols-3 gap-8 w-full px-4 md:px-12">
      <!-- Cart Items -->
      <div class="md:col-span-2 flex flex-col gap-4">
        <transition-group name="cart-item" tag="div" class="flex flex-col gap-4 relative z-10" v-if="cartItems.length > 0">
          <div v-for="item in cartItems" :key="item.id" class="flex items-center w-full bg-white/10 rounded-xl p-5 border-b border-[#393d47] shadow-lg backdrop-blur-md group">
            <div class="w-16 h-16 rounded-xl bg-[#393d47] flex items-center justify-center overflow-hidden mr-4 shadow-md">
              <img :src="item.image" alt="Package Image" class="object-cover w-full h-full transition-transform duration-300 group-hover:scale-105" />
            </div>
            <div class="flex-1">
              <p class="font-bold text-white text-lg mb-1 tracking-wide">{{ item.name }}</p>
              <p class="text-orange-400 font-semibold text-base mb-1">{{ $n(item.in_basket.price, 'currency') }}</p>
              <p v-if="item.in_basket.gift_username" class="text-xs text-orange-300 mt-1">
                <i18n-t keypath="cart.item.gift_to" tag="span">
                  <template #username>
                    <span class="font-bold">{{ item.in_basket.gift_username }}</span>
                  </template>
                </i18n-t>
              </p>
            </div>
            <div class="flex flex-col items-end gap-2 ml-4">
              <span class="text-sm text-orange-200 font-semibold">{{ $t('cart.item.quantity', { quantity: item.in_basket.quantity }) }}</span>
              <button
                @click="removeItem(item.id)"
                :disabled="itemsLoading.has(item.id)"
                class="text-orange-400 hover:text-orange-500 p-2 rounded-full transition disabled:opacity-50 bg-white/10 hover:bg-orange-500/10 shadow-md"
              >
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>
          </div>
        </transition-group>
        <div v-else class="flex flex-col items-center justify-center py-20 text-white opacity-80 relative z-10">
          <svg width="120" height="120" fill="none" viewBox="0 0 120 120" class="mb-6">
            <rect x="10" y="30" width="100" height="60" rx="16" fill="#393d47"/>
            <rect x="30" y="50" width="60" height="20" rx="6" fill="#eb5526"/>
            <circle cx="40" cy="90" r="8" fill="#eb5526"/>
            <circle cx="80" cy="90" r="8" fill="#eb5526"/>
          </svg>
          <span class="text-2xl font-semibold">{{ $t('cart.empty') }}</span>
        </div>
      </div>
      <!-- Cart Summary -->
      <div class="flex flex-col justify-between bg-white/10 rounded-2xl shadow-2xl p-8 backdrop-blur-md border border-white/20 h-fit sticky top-24">
        <div class="mb-8">
          <h3 class="text-xl font-bold text-white mb-4">{{ $t('cart.summary') || 'Summary' }}</h3>
          <div class="flex items-center justify-between text-lg text-white font-semibold mb-2">
            <span>{{ $t('cart.total') }}</span>
            <span class="text-orange-400">{{ $n(basketStore.basket.total_price, 'currency') }}</span>
          </div>
        </div>
        <button
          :disabled="!basketStore.basket?.ident || cartItems.length === 0"
          @click="checkout"
          class="w-full bg-orange-500 hover:bg-orange-600 text-white font-extrabold py-4 px-10 rounded-xl shadow-xl transition-all duration-200 transform hover:scale-105 focus:scale-105 focus:outline-none focus:ring-4 focus:ring-orange-400/40 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          {{ $t('buttons.checkout') }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const basketStore = useBasketStore();
const { t } = useI18n();

const cartItems = computed(() => basketStore.basket?.packages ?? []);
const itemsLoading = computed(() => basketStore.packagesLoading);

const removeItem = (id: number) => {
  basketStore.removePackageFromBasket(id);
};

const appConfig = useAppConfig();

const checkout = async () => {
  if (!basketStore.basket?.ident) return;
  const config = {
    ...appConfig.tebexJsConfig,
    ident: basketStore.basket?.ident,
  } as TebexCheckoutConfig;
  if (typeof window !== 'undefined' && window.Tebex) {
    window.Tebex.checkout.init(config);
    window.Tebex.checkout.launch();
  }
};
</script>

<style scoped>
.cart-item-enter-active, .cart-item-leave-active {
  transition: all 0.35s cubic-bezier(.4,2,.6,1);
}
.cart-item-enter-from {
  opacity: 0;
  transform: translateY(30px) scale(0.96);
}
.cart-item-leave-to {
  opacity: 0;
  transform: translateY(-30px) scale(0.96);
}
</style> 