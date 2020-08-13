<template>
    <v-popover trigger="click">
        <span class="tag reservation"
            :class="classes"
            @click="fetch">
            {{ product.stock }}
        </span>
        <template v-slot:popover>
            <loader size="small"
                v-if="loading"/>
            <ul v-else-if="positions.length > 0">
                <li v-for="({ id, label, quantity }) in positions"
                    :key="id">
                    <span class="icon is-small has-text-info has-padding-large">
                        <fa icon="tag"/>
                    </span>
                    <strong>{{ label }}</strong>
                    <span class="icon is-small has-text-info has-padding-large">
                        <fa icon="boxes"/>
                    </span>
                    <strong>{{ quantity }}</strong>
                </li>
            </ul>
            <span v-else>
                {{ i18n('No locations found') }}
            </span>
        </template>
    </v-popover>
</template>

<script>
import { VPopover } from 'v-tooltip';
import Loader from '@enso-ui/loader/bulma';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faBoxes, faTag } from '@fortawesome/free-solid-svg-icons';

library.add(faBoxes, faTag);

export default {
    name: 'ProductLocations',

    components: { VPopover, Loader },

    inject: ['errorHandler', 'i18n', 'route'],

    props: {
        classes: {
            type: String,
            default: null,
        },
        product: {
            type: Object,
            required: true,
        },
    },

    data: () => ({
        positions: [],
        loading: false,
    }),

    methods: {
        fetch() {
            this.loading = true;

            axios.get(this.route('inventory.positions.forProduct', this.product.id))
                .then(({ data }) => {
                    this.positions = data;
                    this.loading = false;
                }).catch(this.errorHandler);
        },
    },
};
</script>

<style lang="scss">
    .tag.reservation {
        padding: 0 5px;
        font-size: 0.9em;
        height: unset;
    }
</style>
