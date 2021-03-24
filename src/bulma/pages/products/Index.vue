<template>
    <div>
        <div class="columns is-centered is-multiline"
             v-if="ready">
            <div class="column is-narrow">
                <enso-date-filter class="box raises-on-hover"
                    v-model="params.dateInterval"
                    default="thisMonth"
                    :name="i18n('Updated')"
                    :interval="intervals.products.updated_at"/>
            </div>
            <div class="column is-narrow">
                <enso-interval-filter class="box raises-on-hover narrow-filter"
                    :name="i18n('Quantity')"
                    type="number"
                    v-model="intervals.inventory_stocks.quantity"/>
            </div>
        </div>
        <enso-table class="box is-paddingless raises-on-hover"
            ref="products"
            :intervals="intervals"
            :params="params"
            id="products"
            @manage-positions="productId=$event.id"
            @reset="$refs.filterState.reset()">
            <template v-slot:stock="{ row }">
                <product-locations :product="row"
                    :classes="'is-info is-raised is-bold is-clickable'"/>
            </template>
            <template v-slot:pictureUrl="{ row }">
                <figure class="image product-image is-48x48 has-vertically-centered-content">
                    <a :href="row.pictureUrl" target="_blank">
                        <img :src="row.pictureUrl" alt="cover">
                    </a>
                </figure>
            </template>
        </enso-table>
        <filter-state :api-version="apiVersion"
            name="product_filters"
            :intervals="intervals"
            :params="params"
            @ready="ready = true"
            ref="filterState"/>
        <modal show
            v-if="productId"
            @keyup.esc="close"
            @close="close">
            <positions-manager :product-id="productId"/>
        </modal>
    </div>
</template>
<script>
import { mapState } from 'vuex';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faBoxOpen } from '@fortawesome/free-solid-svg-icons';
import {
    Modal, EnsoDateFilter, EnsoFilter,
    EnsoSelectFilter, EnsoIntervalFilter, FilterState,
} from '@enso-ui/bulma';
import { EnsoTable } from '@enso-ui/tables/bulma';
import PositionsManager from '@enso-ui/inventory/src/bulma/components/PositionsManager.vue';
import ProductLocations from '@enso-ui/inventory/src/bulma/components/ProductLocations.vue';
library.add(faBoxOpen);
export default {
    name: 'Index',
    components: {
        ProductLocations,
        Modal,
        PositionsManager,
        EnsoTable,
        EnsoDateFilter,
        EnsoFilter,
        EnsoSelectFilter,
        EnsoIntervalFilter,
        FilterState,
    },
    inject: ['i18n', 'route'],
    data() {
        return {
            apiVersion: 1.0,
            ready: false,
            productId: null,
            params: {
                dateInterval: 'thisMonth',
            },
            intervals: {
                inventory_stocks: {
                    quantity: {
                        min: null,
                        max: null,
                    },
                },
                products: {
                    updated_at: {
                        min: null,
                        max: null,
                        dateFormat: null,
                    },
                },
            },
        };
    },
    computed: {
        ...mapState(['meta', 'enums']),
    },
    methods: {
        close() {
            this.productId = null;
            this.$refs.products.fetch();
        },
    },
};
</script>
<style lang="scss">
.image.product-image.is-48x48 > a {
    width: 48px;
    height: 48px;
    img {
        margin: auto;
        width: auto;
        height: auto;
        max-width: 48px;
        max-height: 48px;
    }
}
</style>
