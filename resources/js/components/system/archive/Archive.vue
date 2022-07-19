<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5">
                        Archive
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <archive-table />
        </v-row>
    </div>

</template>
<script>
import ArchiveTable from './ArchiveTable.vue'
export default {
    components: {
        ArchiveTable
    },
    data() {
        return {
            tabs: null
        }
    },
    created() {
        this.$store.dispatch("getFileList")
        this.$store.dispatch("getFileLocations")
    }
};
</script>
