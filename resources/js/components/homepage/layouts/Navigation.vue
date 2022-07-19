<template>
    <nav class="navbar navbar-expand-lg bg-light fixed-top green" :class="{ scrolled: scrollPosition > 400 }">

        <v-overlay :value="isLoading">
            <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>

        <div class="container">
            <a class="navbar-brand text-white" href="#">Digital Archive System
                <span class="badge bg-success">Beta</span>
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                aria-label="Toggle navigation"
                @click="show = !show" icon>
                <!-- <span class="navbar-toggler-icon"></span> -->
                <v-icon>{{ !show ? 'mdi-menu' : 'mdi-close' }}</v-icon>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ms-auto mb-2 mb-lg-0" v-scroll-spy-active="{ class: 'active' }" v-scroll-spy-link>
                    <li class="nav-item" :key="items" v-for="items in navLink">
                        <a class="nav-link active text-white text-subtitle-1">{{ items }}</a>
                    </li>

                    <li class="text-decoration-none menu">
                        <div v-if="auth.authenticated">
                            <v-menu offset-y>
                                <template v-slot:activator="{ on, attrs }">
                                    <v-btn class="grey darken-4" v-bind="attrs" v-on="on" elevation="0">
                                        <span class="text-button grey--text text--lighten-5">Account</span>
                                    </v-btn>
                                </template>
                                <v-list>
                                    <v-list-item link @click="redirect()">
                                        <v-list-item-title
                                            v-if="auth.user.user_type === 'Staff' || auth.user.user_type === 'Chief'">
                                            <v-icon>mdi-view-dashboard-outline</v-icon>
                                            Dashboard
                                        </v-list-item-title>
                                        <v-list-item-title v-else>
                                            <v-icon>mdi-view-dashboard-outline</v-icon>
                                            Dashboard
                                        </v-list-item-title>
                                    </v-list-item>
                                    <v-list-item @click="logout">
                                        <v-list-item-title>
                                            <v-icon>mdi-logout</v-icon>
                                            Logout
                                        </v-list-item-title>
                                    </v-list-item>
                                </v-list>
                            </v-menu>
                        </div>

                        <div v-else>
                            <v-menu offset-y>
                                <template v-slot:activator="{ on, attrs }">
                                    <v-btn class="grey darken-4" large v-bind="attrs" v-on="on">
                                        <span class="text-button grey--text text--lighten-5">Account</span>
                                    </v-btn>
                                </template>
                                <v-list>
                                    <v-list-item v-for="(item, index) in forAuthenticate" :key="index" :to="item.path"
                                        link>
                                        <v-list-item-title>
                                            <v-icon small v-text="item.icon"></v-icon>
                                            {{ item.title }}
                                        </v-list-item-title>
                                    </v-list-item>
                                </v-list>
                            </v-menu>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    <!-- Navbar -->
    <!-- <nav id="navbar-main" class="navbar fixed-top navbar-expand-lg navbar-dark navbg py-2"
        :class="{ scrolled: scrollPosition > 400 }">
        <v-overlay :value="isLoading">
            <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>
        <div class="container" id="navbar">
            <a class="navbar-brand mr-lg-5" href="">
                <img :src="logo" />
            </a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbar_global"
                aria-controls="navbar_global" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse" id="navbar_global">
                <div class="navbar-collapse-header">
                    <div class="row">
                        <div class="col-6 collapse-brand">
                            <a href="">
                                <img :src="logo" />
                            </a>
                        </div>

                        <div class="col-6 collapse-close">
                            <button type="button" class="navbar-toggler" data-toggle="collapse"
                                data-target="#navbar_global" aria-controls="navbar_global" aria-expanded="false"
                                aria-label="Toggle navigation">
                                <span></span>
                                <span></span>
                            </button>
                        </div>
                    </div>
                </div>
                <div class="container">
                    <div class="float-right">
                        <ul class="nav navbar-nav navbar-nav-hover" v-scroll-spy-active="{ class: 'active' }"
                            v-scroll-spy-link>
                            <li class="nav-item" :key="items" v-for="items in navLink">
                                <a class="nav-link" role="button">
                                    <i class="ni ni-collection d-lg-none"></i>
                                    <span class="nav-link-inner--text">{{ items }}</span>
                                </a>
                            </li>

                            <li class="nav-item">
                                <div v-if="auth.authenticated">
                                    <ul class="navbar-nav align-items-lg-center mt-2">
                                        <li class="nav-item ">
                                            <v-btn>My Account</v-btn>
                                            <div class="" aria-labelledby="navbar-default_dropdown_1">
                                                <router-link :to="redirect" class="">
                                                    <i class="fa fa-tachometer-alt"></i> Dashboard
                                                </router-link>
                                                <a class="" @click="logout">
                                                    <i class="fa fa-sign-out-alt"></i> Logout</a>
                                                <div class=""></div>
                                            </div>
                                        </li>
                                    </ul>
                                </div>
                                <div v-else>
                                    <ul class="navbar-nav align-items-lg-center mt-2">
                                        <li class="nav-item ">
                                            <v-btn>My Account</v-btn>

                                            <div class="" aria-labelledby="">
                                                <router-link :to="redirectRegister" class="">
                                                    <i class="fa fa-address-card"></i> Register
                                                </router-link>
                                                <router-link :to="redirectLogin" class="">
                                                    <i class="fa fa-sign-in-alt"></i> Login
                                                </router-link>

                                                <div class="dropdown-divider"></div>
                                            </div>
                                        </li>
                                    </ul>
                                </div>

                            </li>
                        </ul>
                    </div>

                </div>
            </div>
        </div>
    </nav> -->
    <!-- End Navbar -->
</template>
<script>
import logo from "../../../../../public/assets/img/brand/cmu.png";
export default {
    data() {
        return {
            show: false, // show/hide menu
            logo: logo,
            scrollPosition: null,
            scrolled: false,
            navLink: [
                "Home",
                "About",
                "FAQs",
                "Contact"
            ],
            forAuthenticate: [
                {
                    title: "Login",
                    icon: "mdi-login-variant",
                    path: "/authentication/login",
                },
                {
                    title: "Register",
                    icon: "mdi-account-box-multiple",
                    path: "/authentication/register",
                },
            ],
            forAuthenticated: [
                {
                    title: "Dashboard",
                    icon: "mdi-view-dashboard-outline",
                    path: this.$store.state.auth.user.user_type,
                },
                {
                    title: "Logout",
                    icon: "mdi-logout",
                },
            ],
        };
    },
    methods: {
        //Update Navigation when scroll
        updateScroll() {
            this.scrollPosition = window.scrollY;
            if (window.scrollY > 700) {
                this.scrolled = true;
            }
        },


        async logout() {
            await axios
                .post("/api/logout")
                .then((response) => {
                    this.$store.dispatch("userLogout");
                    this.$router.push("/").catch((error) => {
                        if (error.name != "NavigationDuplicated") {
                            throw error;
                        }
                    });
                })
                .catch((err) => {
                    console.log(err.response.data);
                })
                .finally(function () { });
        },
        redirect() {
            console.log(this.auth.user.user_type);
            if (
                this.auth.user.user_type === "Chief" ||
                this.auth.user.user_type === "Staff"
            ) {
                this.$router.push({
                    name: "systemdashboard",
                });
            } else {
                this.$router.push({
                    name: "clientsearch",
                });
            }
        },
    },
    computed: {
        auth() {
            return this.$store.state.auth;
        },
        //ISLOADING COMPUTED
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading;
            },
            set: function (newVal) {
                return newVal;
            },
        },
        redirectLogin() {
            return {
                name: "authentication",
                params: { action: 'login' }
            }
        },
        redirectRegister() {
            return {
                name: "authentication",
                params: { action: 'register' }
            }
        },
        // redirect() {
        //     let user_type = this.auth.user.user_type;
        //     if (user_type === "Chief" || user_type === "Staff") {
        //         return {
        //             name: "systemdashboard",
        //         };
        //     } else {
        //         return {
        //             name: "clientprofile",
        //         };
        //     }
        // },
    },
    mounted() {
        window.addEventListener("scroll", this.updateScroll);
    },
};
</script>
<style scoped>
#navbar {
    height: 50px;
}

.navbg {
    background: #21c65e !important;
}

.navbg.scrolled {
    background-color: #21c65e !important;
    transition: ease-in-out 0.6s;
}
</style>
