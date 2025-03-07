<template>
    <b-container fluid="lg" style="max-width: 1200px">
        <b-row>
            <b-col cols="12" lg="6">
                <div class="p-3">
                    <b-col>
                        <b-row>
                            <b-img src="~/assets/images/twix_logo.png" fluid alt="TWIX" width="360" />
                        </b-row>
                        <b-row>
                            <div>
                                <span class="subTitle">Claim tokens for development and testing purposes on the TWIX network.</span>
                            </div>
                        </b-row>

                        <b-row>
                            <FaucetForm
                                class="d-lg-none d-xl-none d-md-block mx-form"
                                :mosaic-id="networkInfo.nativeCurrencyId"
                                :filter-mosaics="filterMosaics"
                                :recipient-placeholder="recipientPlaceholder"
                                :amount-placeholder="amountPlaceholder"
                            />
                        </b-row>

                        <b-row>
                            <div class="info">
                                <span>Please send back claimed tokens when you no longer need them.</span>
                                <span>
                                    Faucet Address:

                                    <span class="highlight">
                                        <a target="_blank" :href="faucetAccountUrl">
                                            {{ networkInfo.address }}
                                        </a>
                                    </span>
                                </span>
                            </div>
                        </b-row>
                    </b-col>
                </div>
            </b-col>

            <b-col lg="6">
                <FaucetForm
                    class="d-lg-block d-none"
                    :mosaic-id="networkInfo.nativeCurrencyId"
                    :filter-mosaics="filterMosaics"
                    :recipient-placeholder="recipientPlaceholder"
                    :amount-placeholder="amountPlaceholder"
                />
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
import { Address } from 'twix-sdk';
import FaucetForm from '@/components/FaucetForm.vue';

export default {
    components: {
        FaucetForm,
    },
    computed: {
        filterMosaics() {
            return this.$store.getters.getFilterMosaics;
        },
        networkInfo() {
            return this.$store.getters.getNetworkInfo;
        },
        recipientPlaceholder() {
            return `Address start with a capital ${this.networkInfo.address[0]}`;
        },
        amountPlaceholder() {
            return `(Faucet will pay up to ${this.networkInfo.nativeCurrencyMaxOut}, or enter custom amount)`;
        },
        faucetAccountUrl() {
            return `${this.networkInfo.explorerUrl}/accounts/${Address.createFromRawAddress(this.networkInfo.address).plain()}`;
        },
    },
    created() {
        if (process.browser) {
            // inject method into $nuxt, allow access from store
            this.$nuxt.$makeToast = this.makeToast;
        }
    },
    methods: {
        makeToast(variant = null, message, config) {
            this.$bvToast.toast(message, {
                title: `Notification`,
                variant,
                solid: true,
                toaster: 'b-toaster-top-right',
                appendToast: true,
                ...config,
            });
        },
    },
};
</script>
<style lang="scss" scoped>
.container {
    margin-top: 20px;
}

.row {
    padding: 10px 0;
}

.subTitle {
    font-size: 18px;
    font-weight: bolder;
}

.mx-form {
    margin: 0 -0.7rem !important;
    padding: 0 0.7rem !important;
}

.info {
    span {
        display: block;
        padding: 5px 0;
        font-size: 14px;

        .highlight {
            display: inline;
            color: var(--secondary);
        }
    }
}
</style>
