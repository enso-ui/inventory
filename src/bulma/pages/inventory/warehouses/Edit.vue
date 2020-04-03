<template>
    <div class="columns is-centered">
        <div class="column is-half-desktop is-full-touch">
            <enso-form class="box has-background-light raises-on-hover"/>
            <accessories>
                <template slot-scope="{ count }">
                    <tab keep-alive
                         id="Addresses">
                        <div class="columns is-centered">
                            <div class="column">
                                <ro-addresses :id="warehouseId"
                                    type="LaravelEnso\Inventory\App\Models\Warehouse"
                                    @update="$set(count, 'Addresses', $refs.addresses.count)"
                                    ref="addresses"/>
                            </div>
                        </div>
                    </tab>
                </template>
            </accessories>
        </div>
    </div>
</template>

<script>
import { Tab } from '@enso-ui/tabs/bulma';
import { Accessories, RoAddresses } from '@enso-ui/accessories/bulma';
import { EnsoForm } from '@enso-ui/forms/bulma';

export default {
    components: { Accessories, RoAddresses, EnsoForm, Tab },

    computed: {
        warehouseId() {
            return Number.parseInt(this.$route.params.warehouse, 10);
        },
    },
};
</script>
