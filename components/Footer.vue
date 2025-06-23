<template>
    <footer class="footer">
        <div class="footer__container">
            <div class="footer__left">
                <p>
                    {{
                        $t("footer.copyright", {
                            storeName: appConfig.storeName,
                        })
                    }}
                </p>
            </div>
            <div class="footer__right">
                <Button
                    v-if="appConfig.discordUrl"
                    tag="a"
                    :href="appConfig.discordUrl"
                    variant="clear"
                    icon="discord"
                >
                    Discord
                </Button>
                <a href="https://checkout.tebex.io/terms">Terms</a>
                <a href="https://checkout.tebex.io/privacy">Privacy</a>
            </div>
        </div>
        <div class="footer__accent"></div>
    </footer>
</template>

<script setup lang="ts">
const appConfig = useAppConfig();
</script>

<style lang="scss" scoped>
@use "~/assets/styles/settings" as *;
.footer {
    width: 100%;
    background: $background-100;
    border-top: 2px solid $primary;
    .footer__container {
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 1200px;
        margin: 0 auto;
        padding: 1.5rem 1.5rem 1rem 1.5rem;
        @media (max-width: 600px) {
            flex-direction: column;
            gap: 1rem;
            padding: 1rem;
        }
    }
    .footer__left {
        color: $text-medium-emphasis-color;
        font-size: 1rem;
    }
    .footer__right {
        display: flex;
        align-items: center;
        gap: 1.5rem;
        a, .button {
            color: $primary;
            font-weight: 600;
            text-decoration: none;
            transition: color 0.2s;
            &:hover, &:focus {
                color: darken($primary, 10%);
            }
        }
    }
    .footer__accent {
        width: 100%;
        height: 4px;
        background: linear-gradient(90deg, $primary 0%, $secondary 100%);
        margin-top: 1rem;
    }
}
</style>
