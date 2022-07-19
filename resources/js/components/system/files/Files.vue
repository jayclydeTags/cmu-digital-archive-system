<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5">
                        Records Management
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" class="pt-2 pb-1">
                <v-card flat color="white lighten-3 action-tab--shadow rounded-lg pa-2">
                    <!-- <template v-slot:extension> -->
                        <v-tabs hide-slider v-model="tabs" active-class="green lighten-2 grey--text text--darken-4 rounded" background-color="transparent">
                            <v-tab href="#mobile-tabs-5-1" class="grey--text text--darken-1">
                                <!-- <v-icon>mdi-folder </v-icon> -->
                                Records
                            </v-tab>

                            <v-tab href="#mobile-tabs-5-2" class="grey--text text--darken-1">
                                <!-- <v-icon>mdi-paperclip </v-icon> -->
                                File Locations
                            </v-tab>
                            <v-tab href="#mobile-tabs-5-3" class="grey--text text--darken-1" v-show="auth.user_type === 'Chief' || 'Staff'">
                                <!-- <v-icon>mdi-delete </v-icon>  -->
                                File Disposal
                            </v-tab>
                            <v-tab href="#mobile-tabs-5-4" class="grey--text text--darken-1" v-show="auth.user_type === 'Chief'">
                                <!-- <v-icon>mdi-filter </v-icon>  -->
                                File Category
                            </v-tab>

                        </v-tabs>
                    <!-- </template> -->
                </v-card>
            </v-col>

            <v-col cols="12" class="pt-1">
                <!-- <v-card flat class="shadow"> -->
                    <v-tabs-items v-model="tabs" class="transparent">
                        <v-tab-item v-for="i in 4" :key="i" :value="'mobile-tabs-5-' + i">
                            <div  v-if="i === 1">
                                <file-table />
                            </div>
                            <div  v-if="i === 2">
                                <file-location-table />
                            </div>
                            <div  v-if="i === 3">
                                <file-disposal-table />
                            </div>
                            <div  v-if="i === 4">
                                <file-category-table />
                            </div>
                        </v-tab-item>
                    </v-tabs-items>
                <!-- </v-card> -->
            </v-col>
            <!-- <div class="row">
                <h1>
                    <v-icon size="50" color="info">mdi-file-outline</v-icon>Document Management
                </h1>
                <v-card>
                    <v-toolbar flat>

                        <v-spacer></v-spacer>
                        <template v-slot:extension>
                            <v-tabs v-model="tabs">
                                <v-tabs-slider></v-tabs-slider>
                                <v-tab href="#mobile-tabs-5-1" class="primary--text">
                                    <v-icon>mdi-folder </v-icon> Documents
                                </v-tab>

                                <v-tab href="#mobile-tabs-5-2" class="primary--text">
                                    <v-icon>mdi-file-cabinet </v-icon> File Locations
                                </v-tab>
                                <v-tab href="#mobile-tabs-5-3" class="primary--text"
                                    v-show="auth.user_type === 'Staff'">
                                    <v-icon>mdi-file-compare </v-icon> File Disposal
                                </v-tab>
                                <v-tab href="#mobile-tabs-5-4" class="primary--text"
                                    v-show="auth.user_type === 'Chief'">
                                    <v-icon>mdi-file-compare </v-icon> File Category
                                </v-tab>

                            </v-tabs>
                        </template>
                    </v-toolbar>

                    <v-tabs-items v-model="tabs">
                        <v-tab-item v-for="i in 4" :key="i" :value="'mobile-tabs-5-' + i">
                            <v-card flat v-if="i === 1">
                                <file-table />
                            </v-card>
                            <v-card flat v-if="i === 2">
                                <file-location-table />
                            </v-card>
                            <v-card flat v-if="i === 3">
                                <file-disposal-table />
                            </v-card>
                            <v-card flat v-if="i === 4">
                                <file-category-table />
                            </v-card>
                        </v-tab-item>
                    </v-tabs-items>
                </v-card>

            </div> -->
        </v-row>
    </div>
</template>

<script>
import FileTable from "./FilesTable.vue";
import FileLocationTable from "./FileLocationTable.vue"
import FileCategoryTable from './FileCategoryTable.vue'
import FileDisposalTable from './FileDisposalTable.vue'
export default {
    components: { FileTable, FileLocationTable, FileCategoryTable, FileDisposalTable },
    data() {
        return {
            tabs: null,
            category_id: 0,
        }
    },
    computed: {
        auth() {
            return this.$store.state.auth.user
        }
    },

    created() {
        this.$store.dispatch("getFileList")
        this.$store.dispatch("getFileLocations")
        this.$store.dispatch("getFileCategory")
        this.$store.dispatch("getFileDisposal")
    }
};
</script>
