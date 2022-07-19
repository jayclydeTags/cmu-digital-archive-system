<template>
    <v-container>
        <v-row>
            <v-col cols="12" sm="8">
                <div data-aos="fade-right" data-aos-offset="300" data-aos-easing="ease-in-sine">
                    <h5 class="text-overline font-weight-bold text-uppercase green--text">Central Mindanao University
                    </h5>
                </div>
                <div data-aos="fade-left" data-aos-offset="300" data-aos-easing="ease-in-sine">
                    <h1 class="text-sm-h3 text-h4 font-weight-bold grey--text text--darken-4 text-uppercase head">
                        Digital Archive System
                    </h1>
                </div>
                <div>
                    <p class="text-body-1"><span class="green--text">Students</span>, <span class="green--text">Faculty</span> and <span
                            class="green--text">Alumnus</span> can now access records through this online platform.
                    </p>
                </div>
                <div>
                    <!-- <ul class="list-unstyled pa-0 pl-md-4">
                        <li v-for="(item, i) in items" :key="i" class="d-flex mb-md-4">
                            <v-icon v-text="item.icon" color="yellow darken-3" class="mr-2 mr-md-4"></v-icon>
                            <p class="subtitle-2 ma-0" v-text="item.text"></p>
                        </li>
                    </ul> -->
                    <v-list dense>
                        <v-list-item v-for="(item, i) in items" :key="i">

                            <v-list-item-icon class="mr-3">
                                <v-icon v-text="item.icon" color="yellow darken-3"></v-icon>
                            </v-list-item-icon>

                            <v-list-item-content>
                                <v-list-item-subtitle class="text-wrap text-body-1" v-text="item.text"></v-list-item-subtitle>
                            </v-list-item-content>

                        </v-list-item>
                    </v-list>
                </div>

                <div class="mt-4 mt-md-10" data-aos="fade-up">
                    <v-row>
                        <v-col cols="12" sm="3">
                            <v-btn block large class="green">
                                <router-link :to="redirectLogin" class="text-decoration-none">
                                    <span class="grey--text text--lighten-5">LOGIN</span>
                                </router-link>
                            </v-btn>
                        </v-col>
                        <v-col cols="12" sm="3">
                            <v-btn block large outlined>
                                <router-link :to="redirectRegister" class="text-decoration-none">
                                    <span class=" grey--text text--darken-4">REGISTER</span>
                                </router-link>
                            </v-btn>
                        </v-col>
                    </v-row>
                </div>

            </v-col>

            <v-col cols="12" sm="4">
                <div data-aos="fade-left" class="d-none d-sm-block">
                    <figure>
                        <v-img :src="personalfiles" class="figure-img img-fluid" width="100%" />
                    </figure>
                </div>
            </v-col>
        </v-row>
    </v-container>
    <!-- <div>
    <div class="section section-hero section-shaped">
      <div class="page-header">
        <div class="container shape-container align-items-center py-lg">
          <div class="row">

            <div class="col-md-6 mt-15">
                <div class="col-lg-12">
                  <div
                    data-aos="fade-right"
                    data-aos-offset="300"
                    data-aos-easing="ease-in-sine"
                  >
                    <h6 class="display-8 title">Central Mindanao University</h6>
                  </div>
                  <div
                    data-aos="fade-left"
                    data-aos-offset="300"
                    data-aos-easing="ease-in-sine"
                  >
                    <h3 class="display-6 subtitle">Digital Archiving System</h3>
                  </div>
                  <div data-aos="fade-up" data-aos-duration="3000">
                    <p
                      class="
                        font-weight-normal
                        text-black
                        animate__animated animate__fadeIn
                      "
                    >
                      <span class="sub-text">Students</span>
                      ,<span class="sub-text">Faculty</span> and
                      <span class="sub-text">Alumnus</span>
                      "Lorem ipsum dolor sit amet, consectetur adipiscing elit,
                      sed do eiusmod tempor incididun t ut labore et dolore
                    </p>

                    <v-list dense>
                      <v-list-item v-for="(item,i) in items" :key="i">

                        <v-list-item-icon class="mt-3">
                          <v-icon v-text="item.icon" color="success"></v-icon>
                        </v-list-item-icon>

                        <v-list-item-content>
                          <v-list-item-title
                          class="mt-n2"
                            v-text="item.text"
                          ></v-list-item-title>
                        </v-list-item-content>

                        <v-list-item-avatar>
                          <v-img :src="item.avatar"></v-img>
                        </v-list-item-avatar>
                      </v-list-item>
                    </v-list>
                  </div>
                </div>


            </div>
            <div class="col-md-6">
              <img :src="personalfiles" height="85%" />
            </div>


          </div>
        </div>
      </div>
    </div>
  </div> -->
</template>

<script>

import Login from "./../account/Login.vue"
import Register from "./../account/Register.vue"
// import office from "../../../../../public/assets/img/office/office.png";
import personalfiles from "./../../../../assets/img/personalfiles.svg"
export default {
    components: {
        Login,
        Register,
    },
    data() {
        return {
            // officeicon: office,
            personalfiles: personalfiles,
            selectedItem: 1,
            lockSelection: true,
            inactiveClass: true,
            items: [
                {
                    text: "Students can request records related to their studies.",
                    title: "Jason Oner",
                    icon: "mdi-circle-double",
                },
                {
                    text: "Available records for research and extension.",
                    title: "Travis Howard",
                    icon: "mdi-circle-double",
                },
                {
                    text: "Faculties records available to help improve their learning and providing unique materials for quality education.",
                    title: "Ali Connors",
                    icon: "mdi-circle-double",
                },
                // {
                //   text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit",
                //   title: "Cindy Baker",
                //   icon: "mdi-circle-double",
                // },
            ],
            /* items: [
              { text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit', icon: 'mdi-circle-double',disabled:'disabled'},
              { text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit', icon: 'mdi-circle-double',disabled:'disabled'},
              { text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit', icon: 'mdi-circle-double',disabled:'disabled'},
            ], */
        };
    },
    computed: {
        auth() {
            return this.$store.state.auth;
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
    },
};
</script>

<style scoped>
/* .home {
    background-image: url("../../../../../public/assets/img/banner2.jpg");
    background-repeat: no-repeat;
    background-size: cover;
} */

.sub-text {
    color: #21c65e;
}

.btn-bg-green {
    background-color: #21c65e;
}

.btn-bg-yellow {
    background-color: #fecf14;
}

.bg-green {
    background: linear-gradient(150deg, #2dce89 15%, #21c65e 70%, #21c65e 94%);
    opacity: 0.3;
}

.bg-yellow {
    background: linear-gradient(150deg, #d4eb0d 15%, #ddbd0a 70%, #ddc409 94%);
    opacity: 0.3;
}

.title {
    color: #fecf14;
    text-transform: uppercase;
    font-size: 3.5rem;
    font-weight: bold;
    -webkit-text-stroke: 2px #21c65e;
    -webkit-text-fill-color: #fecf14;
}

.subtitle {
    color: #21c65e;
    text-transform: uppercase;
    font-size: 3rem;
    font-weight: bold;
    color: #000000;
}

.office-img {
    width: 450px;
    height: 250px;
}

.personalfiles {
    width: 100%;
    height: 80%;
}
</style>

