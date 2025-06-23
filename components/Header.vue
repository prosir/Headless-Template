<template>
    <header class="sticky top-0 z-50 w-full bg-[#23272f] shadow-md">
        <div class="flex items-center justify-between max-w-screen-xl mx-auto px-6 py-3">
            <!-- Logo -->
            <NuxtImg class="h-10 w-auto" src="/logo.svg" alt="FiveM Scripts Shop" />

            <!-- Desktop Nav -->
            <nav class="hidden lg:flex flex-1 justify-center">
                <ul class="flex gap-8">
                    <li><a href="/" class="text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Home</a></li>
                    <li><a href="/scripts" class="text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Scripts</a></li>
                    <li><a href="/vehicles" class="text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Vehicles</a></li>
                    <li><a href="/maps" class="text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Maps</a></li>
                    <li><a href="/support" class="text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Support</a></li>
                </ul>
            </nav>

            <!-- Right Actions -->
            <div class="flex items-center gap-3">
                <Button tag="a" :href="appConfig.discordUrl" variant="clear" icon="discord" aria-label="Discord" class="text-orange-500 hover:text-orange-400" />
                <NuxtLink to="/cart" class="flex items-center justify-center bg-orange-500 hover:bg-orange-600 text-white rounded-full p-2 shadow transition">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13l-1.35 2.7A1 1 0 007.5 17h9a1 1 0 00.85-1.53L17 13M7 13V6a1 1 0 011-1h5a1 1 0 011 1v7" />
                    </svg>
                </NuxtLink>
                <Button variant="clear" :to="authStore.loginRoute" aria-label="User" class="rounded-full p-2">
                    <Avatar :user="authStore.user" />
                </Button>
                <!-- Hamburger -->
                <button @click="toggleMobileMenu" class="lg:hidden flex items-center justify-center w-10 h-10 rounded-md text-white hover:bg-orange-500/10 transition" aria-label="Menu">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-7 h-7">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                    </svg>
                </button>
            </div>
        </div>
        <!-- Mobile Menu -->
        <transition name="fade">
            <nav v-if="mobileMenuOpen" class="lg:hidden bg-[#23272f] shadow-md border-t border-orange-500">
                <ul class="flex flex-col gap-2 px-6 py-4">
                    <li><a href="/" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Home</a></li>
                    <li><a href="/scripts" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Scripts</a></li>
                    <li><a href="/vehicles" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Vehicles</a></li>
                    <li><a href="/maps" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Maps</a></li>
                    <li><a href="/support" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition">Support</a></li>
                    <li><NuxtLink to="/cart" class="block text-white font-semibold px-3 py-2 rounded-md hover:bg-orange-500/10 hover:text-orange-500 transition flex items-center gap-2"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-5 h-5"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13l-1.35 2.7A1 1 0 007.5 17h9a1 1 0 00.85-1.53L17 13M7 13V6a1 1 0 011-1h5a1 1 0 011 1v7" /></svg>Cart</NuxtLink></li>
                </ul>
            </nav>
        </transition>
    </header>
</template>

<script setup lang="ts">
const uiStore = useUIStore();
const authStore = useAuthStore();
const appConfig = useAppConfig();
const mobileMenuOpen = ref(false);
const toggleMobileMenu = () => {
    mobileMenuOpen.value = !mobileMenuOpen.value;
};
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
    transition: opacity 0.2s;
}
.fade-enter-from, .fade-leave-to {
    opacity: 0;
}
</style>
