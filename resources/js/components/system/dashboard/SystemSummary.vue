<template>
    <v-row>
        <v-col cols="12">
            <v-card flat dark color="grey lighten-3">
                <v-card-subtitle class="font-weight-bold text-uppercase green--text text--accent-4">
                    System Summary
                </v-card-subtitle>
            </v-card>
        </v-col>
        <v-col cols="6" sm="4" v-for="(items, i) in totalDashboard" :key="i">
            <v-hover v-slot="{ hover }">
                <v-card flat :class="hover ? 'transition-fast-in-fast-out green lighten-4' : 'grey lighten-3 '"
                    class=" rounded-lg system-card--shadow">
                    <div class="pa-4 d-flex justify-space-between align-center">
                        <h6 class="text-subtitle-2 grey--text text--darken-2">
                            {{ items.title }}
                        </h6>
                        <span>
                            <v-icon :color="items.iconColor" v-text="items.icon"></v-icon>
                        </span>
                    </div>

                    <v-card-text>
                        <div class="d-flex justify-center align-center">
                            <h2 class="text-h3 font-weight-bold grey--text text--darken-4">
                                {{ items.count }}
                            </h2>
                        </div>
                    </v-card-text>
                </v-card>
            </v-hover>

        </v-col>
    </v-row>
    <!-- <div class="dashboard text-center">
        <div class="header pb-5 pt-2">
            <div class="container">

                <div class="header-body">
                    Card stats
                    <div class="row">
                        <h4>System Summary</h4>
                        <div class="col-md-6" v-for="(items, i) in totalDashboard" :key="i">
                            <div class="card card-stats mb-3 mb-sm-0" :class="items.bgColor">
                                <div class="card-body">
                                    <div class="row">
                                        <h6 class="
                          card-title
                          font-weight-bold
                          text-uppercase
                          mb-0
                          text-black
                        ">
                                            {{ items.title }}
                                        </h6>

                                        <span class="counter font-weight-bold text-black">{{
                                                items.count
                                        }}</span>
                                        <v-icon size="45" :color="items.iconColor" v-text="items.icon"></v-icon>
                                    </div>


                                    <div class="mx-auto">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div> -->
</template>
<script>
export default {
    data() {
        return {
            dashboard: null,
            user_type: localStorage.getItem("user_type"),
            totalDocs: null,

        };
    },
    computed: {
        auth() {
            return this.$store.state.auth.user
        },
        totalDashboard() {
            let data = []
            return data = [
                {
                    title: "Uploaded",
                    count: this.$store.getters.totalDocuments,
                    // bgColor: "dashboard-item",
                    iconColor: 'secondary',
                    icon: "mdi-folder-outline",
                },
                {
                    title: "Archived",
                    count: this.$store.getters.totalArchiveDocuments,
                    // bgColor: "dashboard-item",
                    iconColor: 'secondary',
                    icon: "mdi-archive-lock-outline",
                },
                {
                    title: "Unarchived",
                    count: this.$store.getters.totalUnarchivedDocuments,
                    // bgColor: "dashboard-item",
                    iconColor: 'secondary',
                    icon: "mdi-archive-outline",
                },
                {
                    title: "Retention",
                    count: this.$store.getters.getDocumentsForDisposal.length,
                    // bgColor: "dashboard-item",
                    iconColor: 'warning',
                    icon: "mdi-clock-time-five-outline",
                },
                {
                    title: "Disposed",
                    count: this.$store.getters.totalDisposedDocuments,
                    // bgColor: "dashboard-item",
                    iconColor: 'error',
                    icon: "mdi-delete-outline",
                },


            ]

        },
        fetchRequests() {
            const files = this.$store.getters.getPendingRequests
            return this._.orderBy(files, ["created_at"], ["desc"]);
        },
    },
    watch: {
        totalDashboard: function () {

        }
    },


};
</script>
<style scoped>
.system-card--shadow {
    box-shadow: 6px 6px 12px #cacaca,
        -6px -6px 12px #ffffff !important;
}
</style>
