<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <div class="border-left mb-10" data-aos="fade-up">
                    <h2 class="about-heading text-uppercase">Connect with Us</h2>
                </div>
            </v-col>

            <v-col cols="12" md="4">
                <v-row>
                    <v-col cols="12">
                        <v-card flat class="pa-4" color="green darken-1" data-aos="fade-right">
                            <v-card-title class="grey--text text--lighten-5">
                                <v-icon class="grey--text text--lighten-5">mdi-map-marker</v-icon>
                                <span>Address</span>
                            </v-card-title>
                            <v-card-text class="grey--text text--lighten-5">
                                <p class="body-1 ma-0">Sayre Hwy, Maramag, Bukidnon </p>
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12">
                        <v-card flat class="pa-4" color="yellow darken-2" data-aos="fade-right">
                            <v-card-title class="grey--text text--darken-4">
                                <v-icon class="grey--text text--darken-4">mdi-phone</v-icon>
                                <span>Phone</span>
                            </v-card-title>
                            <v-card-text class="grey--text text--darken-4">
                                <p class="body-1 ma-0"> (888) 999 999</p>
                                <p class="body-1 ma-0"> (888) 999 999</p>
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12">
                        <v-card flat class="pa-4" color="yellow darken-2" data-aos="fade-right">
                            <v-card-title class="grey--text text--darken-4">
                                <v-icon class="grey--text text--darken-4">mdi-email</v-icon>
                                <span>Email</span>
                            </v-card-title>
                            <v-card-text class="grey--text text--darken-4">
                                <p class="body-1 ma-0"> sample.rmu@cmu.edu.ph</p>
                            </v-card-text>
                        </v-card>
                    </v-col>
                </v-row>

            </v-col>

            <v-col cols="12" md="8">
                <v-card
                    elevation="4"
                    class="pa-md-4 pa-1 rounded-lg"
                    data-aos="fade-left">

                    <!-- ALERT MESSAGE -->
                    <div v-if="msgStatus">
                        <alert-component />
                    </div>

                    <v-card-title>Message Us</v-card-title>
                    <v-card-text>
                        <v-form
                            ref="form"
                            @submit.prevent="save"
                            v-model="rules.isValid"
                            lazy-validation
                        >
                            <v-text-field
                                v-model="form.name"
                                :rules="rules.name"
                                label="Name"
                                placeholder="Your name">
                            </v-text-field>


                            <v-text-field
                                v-model="form.email"
                                :rules="rules.email"
                                label="Email"
                                placeholder="sample@cmu.edu.ph">
                            </v-text-field>
                            <v-text-field
                                v-model="form.subject"
                                :rules="rules.subject"
                                label="Subject" >
                            </v-text-field>
                            <v-textarea
                                v-model="form.message"
                                :rules="rules.message"
                                filled
                                name="input-7-4"
                                label="Message">
                            </v-textarea>
                        </v-form>
                    <v-btn color="info"
                        dark @click="sendemail"
                        :disabled="!rules.isValid"
                        :loading="isLoading">
                        Send Email
                    </v-btn>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>

        <!-- <h3 class="display-6 text-center text-uppercase">Connect With Us</h3>
        <br /> -->

        <!-- <div class="row mt-8">
            Alert Message
            <div v-if="msgStatus">
                <alert-component />
            </div>


            <div class="col-md-4">
                <div class="col-md-12 our-address" data-aos="flip-down">
                    <div>
                        <h6 class="info-title text-uppercase text-black text-center">
                            Our Address
                        </h6>
                        <p class="description opacity-8 text-black text-center">
                            Sayre Hwy, Maramag, Bukidnon
                        </p>
                    </div>
                </div>
                <div class="col-md-12 mt-3 contact" data-aos="flip-down">
                    <div>
                        <h6 class="info-title text-uppercase text-black text-center">
                            Our Phone
                        </h6>
                        <p class="description opacity-8 text-black text-center">
                            (088) 356 1910
                        </p>
                        <p class="description opacity-8 text-black text-center">
                            (088) 356 1910
                        </p>
                    </div>
                </div>
                <div class="col-md-12 mt-3 contact" data-aos="flip-down">
                    <div>
                        <h6 class="info-title text-uppercase text-black text-center">
                            Our Email
                        </h6>
                        <p class="description opacity-8 text-black text-center">Address</p>
                    </div>
                </div>
            </div>

            <div class="col-md-8">
                <v-row>
                    <v-form
                        ref="form"
                        @submit.prevent="save"
                        v-model="rules.isValid"
                        lazy-validation>
                        <v-text-field
                            v-model="form.name"
                            :rules="rules.name"
                            label="Name">
                        </v-text-field>
                        <v-text-field
                            v-model="form.email"
                            :rules="rules.email"
                            label="Email">
                        </v-text-field>
                        <v-text-field
                            v-model="form.subject"
                            :rules="rules.subject"
                            label="Subject">
                        </v-text-field>
                        <v-textarea
                            v-model="form.message"
                            :rules="rules.message"
                            filled
                            name="input-7-4"
                            label="Message">
                        </v-textarea>
                    </v-form>
                </v-row>
                <v-btn color="info" dark @click="sendemail" :disabled="!rules.isValid" :loading="isLoading">Submit
                </v-btn>
            </div>
        </div> -->
    </v-container>
</template>
<script>
import AlertComponent from "./../../AlertComponent.vue";
export default {
    components: { AlertComponent },
    data() {
        return {
            msgStatus: false,
            form: {
                name: "",
                email: "",
                subject: "",
                message: "",
            },
            defaultForm: {
                name: "",
                email: "",
                subject: "",
                message: "",
            },
            rules: {
                isValid: true,
                name: [(v) => !!v || "Name is required"],
                email: [
                    (v) => !!v || "Email is required",
                    (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
                ],
                subject: [(v) => !!v || "Subject is required"],
                message: [(v) => !!v || "Messsage is required"],
            },
        };
    },
    computed: {
        //ISLOADING COMPUTED
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading;
            },
            set: function (newVal) {
                return newVal;
            },
        },
        isError: {
            get: function () {
                return this.$store.state.base.global.status;
            },
            set: function (newVal) {
                return newVal;
            },
        },
    },
    methods: {
        sendemail() {
            this.msgStatus = true;
            this.$store.dispatch("sendEmail", this.form);
            if (this.isError === "Success") {
                this.$refs.form.reset();
            } else {
                this.$refs.form.validate();
            }
        },
    },
};
</script>
<style scoped>
.our-address {
    background-color: #fef08a;
}

.contact {
    background-color: #d1fae5;
}

.contact-info {
    font-size: 16px;
}

.contact-link {
    font-size: 16px;
}
</style>
