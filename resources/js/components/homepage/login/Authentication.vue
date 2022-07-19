<template>
<v-app class="auth__background">
        <v-container class="my-auto">
            <v-overlay :value="isLoading">
                <v-progress-circular indeterminate size="64"></v-progress-circular>
            </v-overlay>
            <v-btn text class="grey--text text--darken-4" @click="$router.push({ name: 'homepage' })" max-width="200px">
                <v-icon class="mr-2">mdi-arrow-left</v-icon>
                Back to Home
            </v-btn>
            <v-container class="d-flex justify-center align-center ">
                <v-row class="my-auto shadow rounded-xl">
                    <v-col cols="12" sm="8" md="6"
                        class="background-left d-flex justify-center align-center pa-2 pa-sm-6 pa-md-10">
                        <div v-if="action === 'login'">
                            <login></login>
                        </div>
                        <div v-else-if="action === 'forgot-password'">
                            <forgot-password></forgot-password>
                        </div>
                        <div v-else-if="action === 'reset-password'">
                            <reset-password></reset-password>
                        </div>
                        <div v-else>
                            <register></register>
                        </div>
                    </v-col>
                    <v-col sm="4" md="6" class="d-none d-sm-flex background-right justify-center align-center pa-10">
                        <div class="d-flex justify-center align-center">
                            <img :src="loginInfoGraphic" width="100%" />
                        </div>
                    </v-col>
                </v-row>
            </v-container>
        </v-container>
    </v-app>
    <!-- <div class="mt-3">
        <v-overlay :value="isLoading">
            <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>
        <v-row class="flex">
            <v-col cols="12" md="6" sm="8" class="pa-3 d-flex flex-column mt-15">
                <div v-if="action === 'login'">
                    <login></login>
                </div>
                <div v-else-if="action === 'forgot-password'">
                    <forgot-password></forgot-password>
                </div>
                <div v-else-if="action === 'reset-password'">
                    <reset-password></reset-password>
                </div>
                <div v-else>
                    <register></register>
                </div>


            </v-col>
            <v-col cols="12" md="6" sm="8" class="pa-3 d-flex flex-column right-container">
                <img :src="loginInfoGraphic" class="mt-n15 img" width="100%" />
            </v-col>
        </v-row>
    </div> -->
</template>
<script>
import Login from './Login.vue'
import Register from './Register.vue'
import ForgotPassword from './ForgotPassword.vue'
import ResetPassword from './ResetPassword.vue'
import loginInfoGraphic from "../../../../assets/img/authentication.svg";
export default {
    components: { Login, Register, ForgotPassword, ResetPassword },
    data() {
        return {
            loginInfoGraphic: loginInfoGraphic,
        };
    },
    computed: {
        //ISLOADING COMPUTED
        action() {
            return this.$route.params.action
        },
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading;
            },

            set: function (newVal) {
                return newVal;
            },
        },
    },
};
</script>
<style scoped>
/* .container-height {
    height: 90vh;
} */
.auth__background {
    background: rgb(243, 245, 145) !important;
    background: linear-gradient(122deg, rgba(243, 245, 145, 1) 0%, rgba(127, 209, 127, 1) 100%) !important;
}
.shadow {
    box-shadow:
        0.8px 1.4px 5px -2px rgba(0, 0, 0, 0.033),
        9px 15px 80px -2px rgba(0, 0, 0, 0.2);
}
.background-left {
    background-color: #f5f5f5;
    border-radius: 12px;
}
/* MD */
@media (min-width: 600px) {
    .background-left {
        border-radius: 20px 0 0 20px;
    }
}
.background-right {
    /* background: rgb(56,248,142);
    background: linear-gradient(325deg, rgba(56,248,142,1) 0%, rgba(7,231,59,1) 50%, rgba(0,252,84,1) 100%); */
    background: rgb(33, 33, 33);
    background: linear-gradient(342deg, rgba(33, 33, 33, 1) 0%, rgba(38, 50, 56, 1) 100%);
    border-radius: 0 20px 20px 0;
}
/*
::v-deep .v-btn {
    padding-left: 12px;
    padding-right: 12px;
    width: 100%;
} */
</style>
