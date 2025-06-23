<template>
  <div class="min-h-[60vh] flex flex-col items-center justify-center bg-[#23272f] py-12">
    <div class="w-full max-w-3xl bg-[#32363f] rounded-xl shadow-lg p-6">
      <h2 class="text-2xl font-bold text-white mb-6 text-center">{{ $t('cart.title') }}</h2>
      <div v-if="cartItems.length > 0" class="flex flex-col gap-4">
        <div v-for="item in cartItems" :key="item.id" class="flex items-center w-full bg-[#32363f] rounded-lg p-4 border-b border-[#393d47] shadow-sm">
          <div class="w-16 h-16 rounded bg-[#393d47] flex items-center justify-center overflow-hidden mr-4">
            <img :src="item.image" alt="Package Image" class="object-cover w-full h-full" />
          </div>
          <div class="flex-1">
            <p class="font-bold text-white text-base mb-1">{{ item.name }}</p>
            <p class="text-orange-500 font-semibold text-sm mb-1">{{ $n(item.in_basket.price, 'currency') }}</p>
            <p v-if="item.in_basket.gift_username" class="text-xs text-orange-400 mt-1">
              <i18n-t keypath="cart.item.gift_to" tag="span">
                <template #username>
                  <span class="font-bold">{{ item.in_basket.gift_username }}</span>
                </template>
              </i18n-t>
            </p>
          </div>
          <div class="flex flex-col items-end gap-2 ml-4">
            <span class="text-sm text-orange-300 font-semibold">{{ $t('cart.item.quantity', { quantity: item.in_basket.quantity }) }}</span>
            <button
              @click="removeItem(item.id)"
              :disabled="itemsLoading.has(item.id)"
              class="text-orange-500 hover:text-orange-600 p-2 rounded transition disabled:opacity-50"
            >
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>
        <div class="flex flex-col md:flex-row justify-between items-center mt-8 gap-4 border-t border-orange-500 pt-6">
          <div class="text-lg text-white font-semibold">
            {{ $t('cart.total') }}:
            <span class="text-orange-500">{{ $n(basketStore.basket.total_price, 'currency') }}</span>
          </div>
          <button
            :disabled="!basketStore.basket?.ident || cartItems.length === 0"
            @click="checkout"
            class="w-full md:w-auto bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 px-8 rounded-lg shadow-lg transition disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {{ $t('buttons.checkout') }}
          </button>
        </div>
      </div>
      <div v-else class="flex flex-col items-center justify-center py-16 text-white opacity-60">
        <span class="text-3xl mb-4">🛒</span>
        <span class="text-lg">{{ $t('cart.empty') }}</span>
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