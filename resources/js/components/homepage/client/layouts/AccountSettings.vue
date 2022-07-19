<template>
    <div class="text-center d-flex">
        <v-btn depressed plain @click="redirect" class="pa-0 px-sm-2" min-width="40">
            <v-icon>mdi-home-variant</v-icon>
            <span class="d-none d-sm-flex">Homepage</span>
        </v-btn>
        <v-btn plain @click="logout()" class="pa-0 px-sm-2" min-width="40">
            <v-icon>mdi-logout-variant</v-icon>
            <span class="d-none d-sm-flex">Log out</span>
        </v-btn>
    </div>
    <!-- <div class="dropdown-menu dropdown-menu-arrow dropdown-menu-right account-settings">
        <a @click="redirect" class="dropdown-item">
            <i class="fa fa-home"></i>
            <span>Home</span>
        </a>
        <div class="dropdown-divider"></div>
        <a @click="logout()" class="dropdown-item">
            <i class="fa fa-sign-out-alt"></i>
            <span>Logout</span>
        </a>
    </div> -->
</template>
<script>
export default {
    data() {
        return {

        }
    },
    methods: {
        async logout() {
            await axios.post('/api/logout').then((response) => {
                this.$store.dispatch("userLogout")
                this.$router.push('/')
                console.log(response.data)
            }).catch((err) => {
                console.log(err.response.data)
            });
        },
        redirect() {
            this.$router.push('/')
        }
    }
}
</script>
<style scoped>
</style>
