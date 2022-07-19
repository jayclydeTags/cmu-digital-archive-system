<template>
    <v-app id="app">
        <navigation />
        <div v-scroll-spy="{ time: 200, steps: 30 }">
            <section>
                <home />
            </section>
            <section>
                <about />
            </section>
            <section>
                <faqs />
            </section>
            <section>
                <contact />
            </section>
        </div>
        <footer-layout />
    </v-app>
</template>
<script>
import Navigation from './Navigation.vue'
import FooterLayout from './Footer.vue'
import About from '../about/About.vue'
import Contact from '../contact/Contact.vue'
import Faqs from '../faqs/Faqs.vue'
import Home from '../home/Home.vue'
import Account from '../account/AccountManagement.vue'
export default {
    components: {
        Navigation,
        FooterLayout,
        Home,
        About,
        Contact,
        Faqs,
        Account

    },
    data() {
        return {
            token: localStorage.getItem('token'),
            options: {
                afterLoad: this.afterLoad
            },
            scrollSpy: {
                section: 0
            }
        }
    },
    methods: {
        afterLoad() {
            console.log("Emitted 'after load' event")
        }
    },
    mounted() {
        window.axios.defaults.headers.common['Authorization'] = `Bearer ${this.token}`
        if (this.token) {
            return this.$store.dispatch("getUser")
        }

    }
}
</script>
<style scoped>
.scroll-spy-container {
    position: relative;
}

section {
    padding-top: 4rem;
    /* padding-bottom: 4rem; */
}

section:nth-child(1) {
    padding-top: 6rem;
}


/* small size */
@media (min-width: 960px) {
    section {
        padding-top: 8rem;
        /* padding-bottom: 8rem; */
    }

    section:nth-child(1) {
        padding-top: 10rem;
    }
}
</style>

