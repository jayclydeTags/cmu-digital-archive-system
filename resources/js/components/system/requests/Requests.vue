<template>
<div>
    <v-row>
        <v-col cols="12">
            <v-card flat class="card-title--background rounded-lg">
                <v-card-title class="text-uppercase grey--text text--lighten-5">
                    Request Management
                </v-card-title>
            </v-card>
        </v-col>
    </v-row>
    <v-row>
        <v-col cols="12" class="pt-2 pb-1">
            <v-card flat color="white lighten-3 action-tab--shadow rounded-lg pa-2">
                <v-tabs hide-slider v-model="tabs" active-class="green lighten-2 grey--text text--darken-4 rounded"
                    background-color="transparent">
                    <!-- <v-tabs-slider></v-tabs-slider> -->
                    <v-tab href="#mobile-tabs-5-1" class="grey--text text--darken-1">
                        <!-- <v-icon>mdi-account-arrow-right-outline </v-icon>  -->
                        Requests
                    </v-tab>
                    <v-tab href="#mobile-tabs-5-2" class="grey--text text--darken-1">
                        <!-- <v-icon>mdi-table </v-icon>  -->
                        Request Log
                    </v-tab>
                </v-tabs>
            </v-card>
        </v-col>
        <v-col cols="12" class="pt-1">
            <!-- <v-card flat class="shadow rounded-lg"> -->
                <v-tabs-items v-model="tabs" class="transparent">
                    <v-tab-item v-for="i in 2" :key="i" :value="'mobile-tabs-5-' + i">
                        <div  v-if="i === 1">
                            <request-table></request-table>
                        </div>
                        <div  v-if="i === 2">
                            <request-log-table></request-log-table>
                        </div>
                    </v-tab-item>
                </v-tabs-items>
            <!-- </v-card> -->
        </v-col>
    </v-row>
    </div>
    <!-- <div class="mt-15">
        <div class="container shadow p-3 mb-5 bg-white">
            <div class="row">
                <h1>
                    <v-icon size="50" color="info">mdi-swap-horizontal</v-icon>Requests Management
                </h1>

                <v-card>
                    <v-toolbar flat>
                        <v-spacer></v-spacer>
                        <template v-slot:extension>
                            <v-tabs v-model="tabs">
                                <v-tabs-slider></v-tabs-slider>
                                <v-tab href="#mobile-tabs-5-1" class="primary--text">
                                    <v-icon>mdi-account-arrow-right-outline </v-icon> Requests
                                </v-tab>

                                <v-tab href="#mobile-tabs-5-2" class="primary--text">
                                    <v-icon>mdi-table </v-icon> Request Log
                                </v-tab>
                            </v-tabs>
                        </template>
                    </v-toolbar>
                    <v-tabs-items v-model="tabs">
                        <v-tab-item v-for="i in 2" :key="i" :value="'mobile-tabs-5-' + i">
                            <v-card flat v-if="i === 1">
                                <request-table />
                            </v-card>
                            <v-card flat v-if="i === 2">
                                <request-log-table />
                            </v-card>
                        </v-tab-item>
                    </v-tabs-items>
                </v-card>
            </div>
        </div>
    </div> -->
</template>
<script>
import RequestTable from "./RequestTable.vue";
import RequestLogTable from "./RequestLogTable.vue";
export default {
    components: { RequestTable, RequestLogTable },
    data() {
        return {
            tabs: null
        }
    },
    created() {
        this.$store.dispatch("getRequests")
    }
};
</script>
