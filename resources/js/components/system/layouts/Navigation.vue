<template>
    <nav>
        <v-app-bar flat app elevatio="6" elevate-on-scroll>
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-app-bar-title>
                <span class="text-h6">Digital Archive System</span>
                <v-chip color="success" small label>
                    Beta
                </v-chip>
            </v-app-bar-title>

            <v-spacer></v-spacer>

            <account-settings></account-settings>
        </v-app-bar>

        <v-navigation-drawer app v-model="drawer" color="blue-grey darken-4" dark>
            <v-list-item class="ma-2 text-center green accent-3 rounded">
                <v-list-item-avatar>
                    <v-img :src="logo"></v-img>
                </v-list-item-avatar>
                <v-list-item-title class="text-h4 text-left blue-grey--text text--darken-4 font-weight-bold">
                    DAS
                </v-list-item-title>
            </v-list-item>

            <v-divider></v-divider>

            <v-list-item>
                <v-list-item-avatar>
                    <v-img :src="avatar"></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                    <v-list-item-title>
                        <span class="green--text text--accent-3">
                            {{ $store.state.auth.user.name }}
                        </span>
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        <span class="grey--text">
                            {{ $store.state.auth.user.email }}
                        </span>
                    </v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list flat dense nav>
                <v-list-item-group v-model="selectedItem"
                    active-class="blue-grey darken-4 green--text text--accent-3 nav--shadow">
                    <v-list-item v-for="(item, i) in navigation" :key="i" route :to="item.itemPath">
                        <v-list-item-icon>
                            <v-icon>{{ item.itemIcon }}</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>{{ item.itemText }}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list-item-group>
            </v-list>

        </v-navigation-drawer>

    </nav>
    <!-- <nav class="navbar navbar-vertical fixed-left navbar-expand-md navbar-light bg-dark" id="sidenav-main">

    <div class="container-fluid">
      Toggler
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#sidenav-collapse-main" aria-controls="sidenav-main" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon">
          <i class="fas fa-bars" style="color:#fff; font-size:28px;"></i>
        </span>
      </button>
      Brand
      <a class="navbar-brand pt-0" href="#">
        <img :src="logo" class="navbar-brand-img logo" alt="...">
      </a>
      User
      <ul class="nav align-items-center d-md-none">

        <li class="nav-item dropdown">
          <a class="nav-link" href="#" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            <div class="media align-items-center">

              <span class="avatar avatar-sm rounded-circle">
                <img alt="Image placeholder" :src="userlogo">
              </span>
            </div>
          </a>
          <account-settings></account-settings>
        </li>
      </ul>
      Collapse
      <div class="collapse navbar-collapse bg-dark" id="sidenav-collapse-main">
        Collapse header
        <div class="navbar-collapse-header d-md-none">
          <div class="row">
            <div class="col-6 collapse-brand">
              <a href="">
                <img src="./assets/img/brand/blue.png">
              </a>
            </div>
            <div class="col-6 collapse-close">
              <button type="button" class="navbar-toggler" data-toggle="collapse" data-target="#sidenav-collapse-main" aria-controls="sidenav-main" aria-expanded="false" aria-label="Toggle sidenav">
                <span></span>
                <span></span>
              </button>
            </div>
          </div>
        </div>

        Navigation
          <ul class="navbar-nav ml-n11">
          <li class="nav-item"
          v-for="(item,i) in navigation" :key="i">
            <router-link class="nav-link" :to="item.itemPath">
              <i :class="[item.itemIcon,item.itemIconColor]"></i>
              <v-icon color="success" v-text="item.itemIcon"></v-icon><span class="link">{{item.itemText}}</span>
            </router-link>
          </li>
        </ul>
        Divider
        <hr class="my-3">
        Navigation
      </div>
    </div>
  </nav> -->
</template>
<script>
import logo from '../../../../../public/images/cmulogo.png'
import avatar from './../../../../assets/img/undraw_profile.svg'
import AccountSettings from './AccountSettings.vue'
export default {
    components: {
        AccountSettings
    },
    data() {
        return {
            logo: logo,
            avatar: avatar,
            navigation: null,
            user_type: localStorage.getItem("user_type"),

            drawer: false, //navigation drawer

            selectedItem: 0,

        }
    },
    computed: {
        auth() {
            return this.$store.state.auth
        },
        authUsertype() {
            this.user_type = this.$store.state.auth.user.user_type
            return this.$store.state.auth.user.user_type
        }
    },
    methods: {
        checkUserType() {
            if (this.user_type === 'Chief') {
                this.navigation = [
                    {
                        itemText: 'Dashboard',
                        itemIcon: 'mdi-view-dashboard-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/dashboard'
                    },
                    {
                        itemText: 'Records',
                        itemIcon: 'mdi-file-document-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/files'
                    },
                    {
                        itemText: 'Archive',
                        itemIcon: 'mdi-archive-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/archive'
                    },
                    {
                        itemText: 'Client Manangement',
                        itemIcon: 'mdi-account-group-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/clients'
                    },
                    {
                        itemText: 'Staff Manangement',
                        itemIcon: 'mdi-account-supervisor-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/user'
                    },
                    {
                        itemText: 'Reports',
                        itemIcon: 'mdi-chart-box-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/reports'
                    }
                ]
            } else {
                this.navigation = [
                    {
                        itemText: 'Dashboard',
                        itemIcon: 'mdi-view-dashboard-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/dashboard'
                    },
                    {
                        itemText: 'Records Manangement',
                        itemIcon: 'mdi-file-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/files'
                    },
                    {
                        itemText: 'Request Manangement',
                        itemIcon: 'mdi-swap-horizontal',
                        itemIconColor: 'text-success',
                        itemPath: '/system/requests'
                    },
                    {
                        itemText: 'Archive',
                        itemIcon: 'mdi-archive-outline',
                        itemIconColor: 'text-success',
                        itemPath: '/system/archive'
                    },

                ]
            }
        }
    },
    created() {
        this.checkUserType()
    }
}
</script>
<style scoped>
.icon-color {
    color: #21c65e;
}

.link {
    color: #fff;
}

.nav-item {
    width: 100%
}

.nav-item:hover {
    background: #d0fae4;
    color: #000;
    transition: 0.6s ease-in-out;
}

.active {
    background: #d0fae4;

}

.active span {
    color: #000;
}

.active li {
    cursor: not-allowed !important;
}

.nav--shadow {
    box-shadow: 5px 5px 8px #1e282c,
        -5px -5px 8px #2e3d44 !important;
}
</style>
