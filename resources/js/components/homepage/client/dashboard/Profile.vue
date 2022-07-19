<template>
    <div class="grey lighten-3 action-tab--shadow">
        <!-- Alert Message -->
        <div v-if="msgStatus">
            <alert-component />
        </div>

        <v-row>

            <v-col cols="12" sm="4">
                <div class="text-center pa-10">
                    <v-img :src="userlogo" width="100%" />
                </div>
            </v-col>
            <!-- <v-col cols="12" md="4" sm="2">
                <div class="text-center">
                    <v-avatar size="260">
                        <img :src="userlogo" />
                    </v-avatar>
                    <v-divider></v-divider>
                </div>
            </v-col> -->

            <v-col cols="12" sm="8">
                <v-list flat color="transparent">
                    <!-- <h2 class="text-uppercase personal-info">Personal Information</h2> -->



                    <v-form ref="form" @submit.prevent="save" v-model="rules.isValid" lazy-validation class="mt-5">
                        <v-container>

                            <v-text-field v-model="form.name" label="Name" prepend-inner-icon="mdi-account"
                                :rules="rules.editProfileRules.name" required>
                            </v-text-field>


                            <v-text-field v-model="form.email" label="Email" disabled prepend-inner-icon="mdi-email"
                                :rules="rules.editProfileRules.email" required>
                            </v-text-field>



                            <v-text-field v-model="form.address" label="Address" prepend-inner-icon="mdi-map-marker"
                                :rules="rules.editProfileRules.address" required></v-text-field>


                            <v-text-field v-model="form.phone_no" label="Contact No." prepend-inner-icon="mdi-phone"
                                :rules="rules.editProfileRules.phone_no" required type="number"></v-text-field>

                            <!--
                <v-text-field
                  label="Current Password"
                  v-model="form.password"


                  :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="showpass ? 'text' : 'password'"
                  @click:append="showpass = !showpass"
                  :rules="rules.editProfileRules.password"
                  required
                >
                </v-text-field>
              </v-col> -->
                        </v-container>
                    </v-form>
                    <!-- <v-list-item-group>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-account</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ getUserProfile.name }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-email</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ getUserProfile.email }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-map-marker</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ getUserProfile.address }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-cellphone</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ getUserProfile.phone_no }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group> -->
                </v-list>

                <v-row class="mt-4">
                <!-- PASSWORD DIALOG -->
                <v-dialog v-model="passworddialog" max-width="550px"    >
                    <template v-slot:activator="{ on, attrs }">
                        <!-- <v-btn color="info" class="mt-10" v-bind="attrs" v-on="on" @click="editProfile(getUserProfile)">
                            <v-icon> mdi-pencil </v-icon>
                            Save Changes
                        </v-btn> -->
                        <v-col cols="12" sm="6" md="3">
                            <v-btn block color="success" v-bind="attrs" v-on="on" @click="editProfile(getUserProfile)">
                                Save changes
                            </v-btn>
                        </v-col>
                    </template>

                    <v-card>
                        <v-card-title class="grey darken-3 grey--text text--lighten-5">
                            Update Profile
                        </v-card-title>

                        <v-container>

                            <v-text-field label="Enter your password" v-model="form.password"
                                :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                                :type="showpass ? 'text' : 'password'" @click:append="showpass = !showpass"
                                :rules="rules.editProfileRules.password" required>
                            </v-text-field>
                        </v-container>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="error" dark @click="close()">
                                Cancel
                            </v-btn>
                            <v-btn color="success" dark @click="save()">
                                Save
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>

                <!-- UPDATE PASSWORD DIALOG -->
                <v-dialog v-model="changepassworddialog" max-width="550px">
                    <template v-slot:activator="{ on, attrs }">
                        <v-col cols="12" sm="6" md="3">
                            <v-btn block color="success" text v-bind="attrs" v-on="on"
                                @click="updatePassword(getUserProfile)">
                                <!-- <v-icon> mdi-pencil </v-icon> -->
                                Update Password
                            </v-btn>
                        </v-col>
                        <!-- <v-btn color="warning" class="mt-10" v-bind="attrs" v-on="on"
                            @click="updatePassword(getUserProfile)">
                            <v-icon> mdi-pencil </v-icon>
                            Update Password
                        </v-btn> -->
                    </template>

                    <v-card>
                        <v-card-title class="grey darken-3 grey--text text--lighten-5">
                            Update Password
                        </v-card-title>
                        <v-container>
                            <v-form ref="form" @submit.prevent="save" v-model="rules.isValid" lazy-validation
                                v-show="isUpdatePassword">
                                <div>
                                    <v-text-field label="Current Password" v-model="form.password"
                                        :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                                        :type="showpass ? 'text' : 'password'" @click:append="showpass = !showpass"
                                        :rules="rules.updatePasswordRules.password" required>
                                    </v-text-field>

                                    <v-text-field label="New Password" v-model="form.newpass"
                                        :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                                        :type="showpass ? 'text' : 'password'" @click:append="showpass = !showpass"
                                        :rules="rules.updatePasswordRules.password" required>
                                    </v-text-field>

                                    <v-text-field label="Confirm New Password" v-model="form.newpass_confirmation"
                                        :append-icon="
                                            showconfirmpass ? 'mdi-eye' : 'mdi-eye-off'
                                        " :type="showconfirmpass ? 'text' : 'password'"
                                        @click:append="showconfirmpass = !showconfirmpass"
                                        :rules="rules.updatePasswordRules.newpass_confirmation" required>
                                    </v-text-field>
                                </div>
                            </v-form>
                        </v-container>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="error" dark @click="close()">
                                Cancel
                            </v-btn>
                            <v-btn color="success" dark @click="save()" :loading="isLoading" :disabled="!rules.isValid">
                                Save
                            </v-btn>

                        </v-card-actions>
                    </v-card>
                </v-dialog>
                </v-row>

            </v-col>
        </v-row>
    </div>
</template>
<script>
import AlertComponent from "./../../../AlertComponent.vue";
import userlogo from './../../../../../assets/img/undraw_profile.svg'
export default {
    components: { AlertComponent },
    computed: {
        getUserProfile() {
            return this.$store.state.auth.user;
        },
        isLoading() {
            return this.$store.state.base.isLoading;
        },
    },
    data() {
        const user = this.getUserProfile;
        return {
            passworddialog: false,
            changepassworddialog: false,
            user: user,
            dialog: false,
            showForm: false,
            showInfo: true,
            showpass: false,
            showconfirmpass: false,
            isEditProfile: false,
            isUpdatePassword: false,
            userlogo: userlogo,
            msgStatus: false,

            items: [
                {
                    icon: "mdi-account",
                    text: "name",
                },
                {
                    icon: "mdi-mail",
                    text: "Email",
                },
                {
                    icon: "mdi-map-marker",
                    text: "Address",
                },
                {
                    icon: "mdi-cellphone",
                    text: "Contact No.",
                },
            ],

            //FORM PROPERTIES
            form: {
                name: this.$store.state.auth.user.name,
                email: this.$store.state.auth.user.email,
                address: this.$store.state.auth.user.address,
                phone_no: this.$store.state.auth.user.phone_no,
                password: "",
                newpass: "",
                newpass_confirmation: "",
                user_type: this.$store.state.auth.user.user_type,
                user_id: this.$store.state.auth.user.user_id,
                updateType: "",
            },

            //RULES VALIDATION PROPERTIES
            rules: {
                isValid: true,
                updatePasswordRules: {
                    password: [(v) => !!v || "Password is required"],
                    newpass: [(v) => !!v || "Password is required"],
                    newpass_confirmation: [
                        (v) => !!v || "Password confirmation is required",
                        (v) => v === this.form.newpass || "The password must be match",
                    ],
                },
                editProfileRules: {
                    name: [(v) => !!v || "Name is required"],
                    email: [
                        (v) => !!v || "E-mail is required",
                        (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
                    ],
                    address: [(v) => !!v || "Address is required"],
                    phone_no: [(v) => !!v || "Phone No. is required"],
                    user_type: [(v) => !!v || "User type is required"],
                    password: [(v) => !!v || "Password is required"],
                },
            },

            //DEFAULT FORM DATA
            defaultItem: {
                name: "",
                email: "",
                address: "",
                phone_no: "",
                user_type: "Client",
            },
        };
    },
    methods: {
        editProfile(user) {
            this.passworddialog = true;
            this.updateType = 'editprofile'
            /* /* this.showForm = true
            this.showInfo = false */

            this.isEditProfile = true
        },
        updateProfile() {

        },

        updatePassword(user) {
            this.changepassworddialog = true
            this.user_type = "updatepassword"
            this.showForm = true;
            this.isUpdatePassword = true;
            this.showInfo = false;
        },
        backtoProfile() {
            this.showForm = false;
            this.showInfo = true;
            (this.isEditProfile = false), (this.isUpdatePassword = false);
        },
        //MODAL CLOSE
        close() {
            this.updateType = ""
            this.passworddialog = false
            this.changepassworddialog = false
            this.$nextTick(() => {
                this.passworddialog = false
                this.changepassworddialog = false
                this.form.password = ""
                this.form.newpass = ""
                this.form.newpass_confirmation = ""
                this.updateType = ""
                this.editedIndex = -1;
            });
        },
        async downloadRequestForm({ url, label }) {
            const response = await axios.get(url, {
                responseType: "blob",
            });
            const blob = new Blob([response.data], {
                type: "application/pdf",
            });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = label;
            link.click();
            URL.revokeObjectURL(link.href);
        },
        save() {
            this.msgStatus = true;
            if (this.updateType === "editprofile") {
                this.close()
                this.$store.dispatch("updateCurrentUser", this.form);
            } else {
                this.close()
                this.$store.dispatch("updateCurrentUserPassword", this.form);
            }
        },
    },
};
</script>
<style>
.action-tab--shadow {
    box-shadow: 6px 6px 12px #cacaca,
        -6px -6px 12px #ffffff !important;
}
</style>
