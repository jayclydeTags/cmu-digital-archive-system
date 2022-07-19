<template>
    <v-container>
        <!-- <v-layout row justify-center align-center>
            <div class="float-left">
                <v-col cols="12" md="3" sm="5">
                    <v-btn text @click="$router.push({ name: 'homepage' })">
                        <v-icon>mdi-arrow-left</v-icon>Back to Home
                    </v-btn>
                </v-col>
            </div>
        </v-layout> -->
        <v-layout row justify-center align-center>
            <v-card max-width="768" flat class="grey lighten-4">

                <!-- Alert Message -->
                <div v-if="msgStatus">
                    <alert-component />
                </div>


                <v-card-text>
                    <h3 class="text-h5 font-weight-bold text-uppercase grey--text text--darken-4">Create Account</h3>
                    <p class="text-body-2">Get access to CMU Archive by creating an account.</p>
                    <v-alert text type="info" class="text-body-2">
                        Your password must contain the following. <br>
                        <ul>
                            <li>At least 8 characters</li>
                            <li>At least one number</li>
                            <li>At least one special character</li>
                            <li>At least one uppercase letter</li>
                        </ul>
                    </v-alert>
                    <v-container>
                        <v-form ref="form" @submit.prevent="save" v-model="rules.isValid" lazy-validation>
                            <v-row>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field v-model="form.name" label="Name"
                                        prepend-inner-icon="mdi-account" :rules="rules.name" required
                                        placeholder="Your Name">
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field v-model="form.email" label="Email"
                                        prepend-inner-icon="mdi-email" :rules="rules.email" required
                                        hint="Use your Institutional Email"
                                        placeholder="user@cmu.edu.ph"></v-text-field>
                                </v-col>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field v-model="form.address" label="Address"
                                        prepend-inner-icon="mdi-map-marker" :rules="rules.address" required>
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field v-model="form.phone_no" label="Contact No."
                                        prepend-inner-icon="mdi-phone" :rules="rules.phone_no" type="number" required>
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field label="Password" v-model="form.password"
                                        prepend-inner-icon="mdi-lock"
                                        :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                                        :type="showpass ? 'text' : 'password'" @click:append="showpass = !showpass"
                                        :rules="rules.password" required>
                                    </v-text-field>
                                </v-col>
                                <v-col cols="12" class="my-n3 px-0">
                                    <v-text-field label="Password" v-model="form.password_confirmation"
                                        prepend-inner-icon="mdi-lock" :append-icon="
                                            showconfirmpass ? 'mdi-eye' : 'mdi-eye-off'
                                        " :type="showconfirmpass ? 'text' : 'password'"
                                        @click:append="showconfirmpass = !showconfirmpass"
                                        :rules="rules.password_confirmation" required>
                                    </v-text-field>
                                </v-col>
                            </v-row>
                        </v-form>
                    </v-container>
                </v-card-text>
                <!-- Form Buttons -->
                <v-card-actions class="px-4">
                    <v-row>
                        <v-col cols="12" lg="6">
                            <v-btn :disabled="!rules.isValid" @click="save"
                                class="grey darken-4 text-button grey--text text--lighten-5">
                                Register
                            </v-btn>
                        </v-col>

                        <v-col cols="12" lg="6">
                            <v-btn class="text-subtitle-2" text
                                @click="$router.push({ name: 'authentication', params: { action: 'login' } })">
                               login</v-btn>
                        </v-col>
                    </v-row>
                </v-card-actions>
            </v-card>
        </v-layout>
    </v-container>
    <!-- <div class="mt-3">
        <v-layout row justify-center align-center>
          <div class="float-left">
            <v-col cols="12" md="3" sm="5">
              <v-btn text @click="$router.push({ name: 'homepage' })">
                <v-icon>mdi-arrow-left</v-icon>Back to Home</v-btn
              >
            </v-col>
          </div>
        </v-layout>
        <div>
          <v-layout row justify-center align-center class="mt-7">
            <v-card max-width="800">


              Alert Message
              <div v-if="msgStatus">
                <alert-component />
              </div>


              <v-card-text>
                <h3 class="display-7 text-uppercase">Create Account</h3>
                <p>Get access to CMU Archive by creating an account.</p>
                <v-container>
                  <v-form
                    ref="form"
                    @submit.prevent="save"
                    v-model="rules.isValid"
                    lazy-validation
                  >
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          v-model="form.name"
                          label="Name"
                          outlined
                          dense
                          prepend-inner-icon="mdi-account"
                          :rules="rules.name"
                          required
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col cols="12">
                        <v-text-field
                          v-model="form.email"
                          label="Email"
                          outlined
                          dense
                          prepend-inner-icon="mdi-email"
                          :rules="rules.email"
                          required
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col cols="12">
                        <v-text-field
                          v-model="form.address"
                          label="Address"
                          outlined
                          dense
                          prepend-inner-icon="mdi-map-marker"
                          :rules="rules.address"
                          required
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col cols="12">
                        <v-text-field
                          v-model="form.phone_no"
                          label="Contact No."
                          outlined
                          dense
                          prepend-inner-icon="mdi-phone"
                          :rules="rules.phone_no"
                          type="number"
                            maxlength="11"
                          required
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col cols="12">
                        <v-text-field
                          label="Password"
                          v-model="form.password"
                          outlined
                          dense
                          prepend-inner-icon="mdi-lock"
                          :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                          :type="showpass ? 'text' : 'password'"
                          @click:append="showpass = !showpass"
                          :rules="rules.password"
                          required
                        >
                        </v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col cols="12">
                        <v-text-field
                          label="Password"
                          v-model="form.password_confirmation"
                          outlined
                          dense
                          prepend-inner-icon="mdi-lock"
                          :append-icon="
                            showconfirmpass ? 'mdi-eye' : 'mdi-eye-off'
                          "
                          :type="showconfirmpass ? 'text' : 'password'"
                          @click:append="showconfirmpass = !showconfirmpass"
                          :rules="rules.password_confirmation"
                          required
                        >
                        </v-text-field>
                      </v-col>
                    </v-row>
                  </v-form>
                </v-container>
              </v-card-text>
              Form Buttons
              <v-card-actions>
                <v-row>
                  <v-col cols="12" md="6" sm="8">
                    <v-btn
                      x-large
                      :disabled="!rules.isValid"
                      color="success"
                      dark
                      @click="save"
                    >
                      Create my Account
                    </v-btn>
                  </v-col>

                  <v-col cols="12" md="6" sm="8">
                    <v-btn x-large text @click="$router.push({ name: 'authentication',params:{action:'login'} })"
                      >Already have an Account?</v-btn
                    >
                  </v-col>
                </v-row>
              </v-card-actions>
            </v-card>
          </v-layout>
        </div>
  </div> -->
</template>
<script>
import AlertComponent from "./../../AlertComponent.vue";
// import loginInfoGraphic from "../../../../assets/img/authentication.svg";
export default {
    components: { AlertComponent },
    data() {
        return {
            //   loginInfoGraphic: loginInfoGraphic,
            //Password Property
            showpassForm: false,
            showpass: false,
            showconfirmpass: false,
            //Error Handlings Property
            error: "",
            msgStatus: false,

            //Form Properties
            form: {
                name: "",
                email: "",
                address: "",
                phone_no: "",
                password: "",
                password_confirmation: "",
                user_type: "Client",
            },

            //Rules Validation Property
            rules: {
                isValid: true,
                name: [(v) => !!v || "Name is required"],
                email: [
                    (v) => !!v || "E-mail is required",
                    (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
                ],
                address: [(v) => !!v || "Address is required"],
                phone_no: [
                    (v) => !!v || "Phone No. is required",
                    (v) => (v && v.length <= 11) || "Phone No. limit is 11",
                ],
                user_type: [(v) => !!v || "User type is required"],
                password: [
                    (v) => !!v || "Password is required",
                    (v) => (v && v.length >= 8) || "Passowrd must atleast 8 characters",
                    //(v) => (v && /\d/.test(v)) || "Password must have atleast one number",
                    //(v) => (v && /[A-Z]{1}/.test(v) || "Password must have atleast one capital letter"),
                    //(v) => (v && /[^A-Za-z0-9]/.test(v) || "Password must have atleast one special character")
                ],
                password_confirmation: [
                    (v) => !!v || "Password confirmation is required",
                    (v) => v === this.form.password || "The password must be match",
                ],
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
    },
    methods: {
        limitText(limitField, limitNum) {
            if (limitField.target.value.toString().length < limitNum) {
                limitField = limitField.target.value.substring(0, limitNum);
            }
            this.form.phone_no = limitField;
        },
        redirectToHome() {
            this.$router.push({ name: "homepage" });
        },
        redirectLogin() {
            this.$router.push({ name: "authentication", params: { action: 'login' } });
        },
        //SAVE FORM
        async registerUser() {
            try {
                await this.$store.dispatch("addUser", this.form);
            } catch (error) {
                console.log(error);
            }
        },
        save() {
            this.msgStatus = true;
            this.$refs.form.validate();
            this.registerUser();
            this.$refs.form.reset();
        },
    },
};
</script>
<style scoped>
.img {
    height: 150vh;
}

.right-container {
    background-image: url('../../../../../public/images/authentication.svg');
    background-color: #21c65e;
    background-size: 120vh;
}

::v-deep .v-btn {
    padding-left: 12px;
    padding-right: 12px;
    width: 100%;
}
</style>
