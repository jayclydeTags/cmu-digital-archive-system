<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5">
                        Profile
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <v-card flat color="grey lighten-4">
                    <profile></profile>
                </v-card>
            </v-col>
        </v-row>
    </div>
    <!-- <div class="mt-15">
            <div class="container shadow p-3 mb-5 bg-white">
                <h1><v-icon size="50" color="info">mdi-account</v-icon>Profile</h1>
                <profile></profile>


            </div>

        </div> -->
</template>

<script>
import Profile from './Profile.vue'
export default {
    components: { Profile }
}
</script>

<style scoped>
.content {
    background: #fff;
}
</style>
