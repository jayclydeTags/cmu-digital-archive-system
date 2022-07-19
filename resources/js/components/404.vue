<template>
<!-- <v-app class="grey lighten-2"> -->
        <v-container class="my-auto">
            <!-- <v-container>
            <div class="row mt-15">
                <div class="text-center mt-20">
                    <img :src="notfound" class="notfound">
                </div>
                <div class="text-center">
                    <v-btn @click="$router.go(-1)" dark color="info">
                        Return to page
                    </v-btn>
                </div>
            </div>
        </v-container> -->
            <v-row class="align-center grey lighten-2 page-not-found--shadow rounded-lg ma-2"
                justify="center">
                <v-col cols="12" sm="6" class="d-flex justify-center align-center pa-2 pa-sm-6 pa-md-10">
                    <v-card flat color="transparent">
                        <v-card-title class="text-h6 text-sm-h5 text-md-h4 font-weight-bold text-uppercase">
                            Page not found.
                        </v-card-title>
                        <v-card-text>
                                Sorry, the page you are looking for doesn't exist
                                or has been removed. Here some helful link.
                        </v-card-text>
                        <!-- Form Buttons -->

                        <v-card-actions>
                            <v-btn text @click="redirectBack">
                                <v-icon>mdi-arrow-left</v-icon>Back
                            </v-btn>
                            <v-btn outlined color="success" dark @click="redirectToHome">
                                <v-icon>mdi-home</v-icon>Take me home
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-col>
                <v-col cols="12" sm="6" class="d-flex background-right justify-center align-center pa-10">
                    <div class="d-flex justify-center align-center">
                        <img :src="notfound" width="100%" />
                    </div>
                </v-col>
            </v-row>

        </v-container>
    <!-- </v-app> -->
    <!-- <div class="mt-n1 notfound-container">
        <v-container>
            <div class="row mt-15">
                <div class="text-center mt-20">
                    <img :src="notfound" class="notfound">
                </div>
                <div class="text-center">
                    <v-btn @click="$router.go(-1)" dark color="info">
                        Return to page
                    </v-btn>
                </div>


            </div>

        </v-container>
        <v-row class="align-center" justify="center">
            <v-col cols="12" md="4" sm="10">
                <div class="float-right">
                    <v-layout row justify-center align-center class="mt-14">
                        <v-card outlined height="450px" :color="bgvcard">
                            <v-card-title>
                                <h1 class="text-uppercase">Page not found.</h1>
                            </v-card-title>
                            <v-card-text>
                                <p>
                                    Sorry, the page you are looking for doesn't exist<br>
                                    or has been removed. Here some helful link.
                                </p>
                            </v-card-text>
                            Form Buttons

                            <v-card-actions>
                                <v-btn text @click="redirectBack">
                                    <v-icon>mdi-arrow-left</v-icon>Back
                                </v-btn>
                                <v-btn color="success" dark @click="redirectToHome">
                                    <v-icon>mdi-home</v-icon>Take me home
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-layout>
                </div>
            </v-col>
            <v-col cols="12" md="8" sm="10" class="right-container">
                <img :src="notfound" class="mt-5" height="90%" width="100%" />
            </v-col>
        </v-row>
    </div> -->
</template>
<script>
import notfound from "./../../assets/img/404v2.svg"
export default {
    data() {
        return {
            bgvcard: '#d9effa',
            notfound: notfound,
        };
    },
    methods: {
        redirectBack() {
            this.$router.go(-1)
        },
        redirectToHome() {
            this.$router.push({
                name: 'homepage'
            })
        }
    }
};
</script>
<style scoped>
/* .right-container {
    height: 100vh;
}
.notfound-container {
    background-color: #d9effa;
    height: 100%;
} */
.page-not-found--shadow {
    box-shadow: 6px 6px 12px #cacaca,
        -6px -6px 12px #ffffff !important;
}
.page-not-found--background {
    background: rgb(243, 245, 145) !important;
    background: linear-gradient(122deg, rgba(243, 245, 145, 1) 0%, rgba(127, 209, 127, 1) 100%) !important;
}
</style>
