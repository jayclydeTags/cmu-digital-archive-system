<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5">
                        welcome {{auth.user.name}}
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <file-list/>
        </v-row>
    </div>
    <!-- <div class="mt-15">
            <div class="container shadow p-3 mb-5 bg-white">
                <div class="row">
                    <h4 class="text-uppercase">welcome {{auth.user.name}} </h4>
                    <file-list/>

                </div>
            </div>

        </div> -->
</template>
<script>
import FileList from './FileList.vue'
export default {
    components: { FileList },
    computed: {
        auth() {
            return this.$store.state.auth
        }
    },
    created() {
        this.$store.dispatch("getFileList")
        this.$store.dispatch("getFileLocations")
        this.$store.dispatch("getFileCategory")

    }
}
</script>
<style scoped>
.content {
    background: #fff;
}
</style>
