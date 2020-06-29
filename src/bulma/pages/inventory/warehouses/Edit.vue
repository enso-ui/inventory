<template>
    <div class="columns is-centered">
        <div class="column is-half-desktop is-full-touch">
            <enso-form class="box has-background-light raises-on-hover"/>
            <accessories>
                <tab keep-alive
                     id="Address">
                    <div class="columns is-centered">
                        <div class="column">
                            <addresses :id="$route.params.warehouse"
                                type="warehouse"
                                ref="addresses"/>
                        </div>
                    </div>
                </tab>
            </accessories>
        </div>
    </div>
</template>

<script>
import { Tab } from '@enso-ui/tabs/bulma';
import { Accessories, Addresses } from '@enso-ui/bulma';
import { EnsoForm } from '@enso-ui/forms/bulma';

export default {
    components: { Accessories, Addresses, EnsoForm, Tab },
};
</script>
