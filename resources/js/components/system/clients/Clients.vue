<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5">
                        Client Management
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <client-table />
        </v-row>
    </div>
    <!-- <div class="mt-15">
        <div class="container shadow p-3 mb-5 bg-white">
            <div class="row">
                <h1>
                    <v-icon size="50" color="info">mdi-account-group-outline</v-icon>Client Management
                </h1>
                <client-table />
            </div>
        </div>

    </div> -->
</template>
<script>
import ClientTable from './ClientTable.vue'
export default {
    components: { ClientTable }
}
</script>
