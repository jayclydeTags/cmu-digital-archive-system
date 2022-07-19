<template>
    <div>
        <!-- Alert Message -->
        <div v-if="msgStatus">
            <alert-component />
        </div>

        <v-card flat class="mb-2 pa-4 rounded-lg">
            <v-row>
                <v-col cols="12" sm="6" md="4">
                    <v-text-field dense v-model="search" label="Search" append-icon="mdi-magnify"
                        hint="Search for keywords">
                    </v-text-field>
                </v-col>
            </v-row>
        </v-card>

        <v-card flat class="rounded-lg">
            <!-- <v-card-title>
                <v-spacer></v-spacer>
                <v-text-field v-model="search" label="Search" dense outlined prepend-inner-icon="mdi-magnify">
                </v-text-field>
            </v-card-title> -->



            <!-- User Table -->
            <v-data-table :headers="headers" :items="fetchUser" :search="search" class="elevation-1 table-striped">
                <template v-slot:top>
                    <v-toolbar flat>
                        <v-toolbar-title>List of Staff</v-toolbar-title>
                        <v-divider class="mx-4" inset vertical></v-divider>
                        <v-spacer></v-spacer>


                        <!-- User Management Modal -->

                        <v-dialog v-model="dialog" max-width="550px">
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn color="primary" dark v-bind="attrs" v-on="on">
                                    <v-icon dark> mdi-plus-circle </v-icon>
                                    Add Staff
                                </v-btn>
                            </template>
                            <v-card>
                                <v-toolbar color="primary" dark>
                                    <v-toolbar-title>{{ formTitle }}</v-toolbar-title>
                                </v-toolbar>
                                <v-container>
                                    <v-form ref="form" @submit.prevent="save" v-model="rules.isValid" lazy-validation>
                                            <v-text-field
                                                v-model="form.name" label="Name"
                                                prepend-inner-icon="mdi-account"  :rules="rules.name" required>
                                            </v-text-field>

                                            <v-text-field v-model="form.email" label="Email"
                                                prepend-inner-icon="mdi-email"   :rules="rules.email"
                                                required></v-text-field>

                                            <v-text-field v-model="form.address" label="Address"
                                                prepend-inner-icon="mdi-map-marker"
                                                :rules="rules.address" required></v-text-field>

                                            <v-text-field v-model="form.phone_no" label="Contact No."
                                                prepend-inner-icon="mdi-phone"   :rules="rules.phone_no"
                                                required type="number"></v-text-field>

                                            <v-text-field v-if="formTitle === 'New User'" label="Password"
                                                v-model="form.password"  prepend-inner-icon="mdi-lock"
                                                :append-icon="showpass ? 'mdi-eye' : 'mdi-eye-off'"
                                                :type="showpass ? 'text' : 'password'"
                                                @click:append="showpass = !showpass" :rules="rules.password" required>
                                            </v-text-field>

                                            <v-text-field v-if="formTitle === 'New User'" label="Confirm Password"
                                                v-model="form.password_confirmation" prepend-inner-icon="mdi-lock"
                                                  :append-icon="
                                                    showconfirmpass ? 'mdi-eye' : 'mdi-eye-off'
                                                " :type="showconfirmpass ? 'text' : 'password'"
                                                @click:append="showconfirmpass = !showconfirmpass"
                                                :rules="rules.password_confirmation" required>
                                            </v-text-field>

                                            <v-select :items="usertype" label="User Type"
                                                prepend-inner-icon="mdi-account" v-model="form.user_type"
                                                :rules="rules.user_type" required  ></v-select>
                                    </v-form>
                                </v-container>

                                <!-- Form Buttons -->
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="error" dark @click="close"> Cancel </v-btn>
                                    <v-btn :disabled="!rules.isValid" color="success" dark @click="save"
                                        :loading="isLoading">
                                        Save
                                    </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>

                        <!-- Delete Confirmation Modal -->
                        <v-dialog v-model="dialogDelete" max-width="550px">
                            <v-card>
                                <v-toolbar color="error" dark>
                                    <v-toolbar-title class="text-h6">
                                        Delete Staff
                                    </v-toolbar-title>
                                </v-toolbar>
                                <v-container>
                                    <v-alert text type="error" >
                                        Once this Staff deleted it can no longer access the system.
                                    </v-alert>
                                    <h5>Are you sure you want to delete this staff?</h5>
                                </v-container>
                                <!-- <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title> -->
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="error" dark @click="closeDelete">Cancel</v-btn>
                                    <v-btn color="success" dark :loading="isLoading" @click="deleteItemConfirm">Confirm</v-btn>
                                    <!-- <v-spacer></v-spacer> -->
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                    </v-toolbar>
                </template>

                <!-- Table Actions Buttons -->
                <template v-slot:item.actions="{ item }">
                    <v-btn-toggle v-model="icon" borderless>
                        <v-btn small value="left" color="info" class="ma-1 rounded-sm" @click="editItem(item)">
                            <v-icon small class="text-white"> mdi-pencil-outline </v-icon>
                        </v-btn>
                        <v-btn value="center" small color="error" class="ma-1 rounded-sm" @click="deleteItem(item)">
                            <v-icon small class="text-white"> mdi-delete </v-icon>
                        </v-btn>
                    </v-btn-toggle>
                </template>

            </v-data-table>
        </v-card>
    </div>
</template>
<script>
import AlertComponent from "./../../AlertComponent.vue";
export default {
    components: { AlertComponent },
    data() {
        return {
            //TABLE SEARCH PROPERTY
            search: "",
            icon: "justify",

            //Dialog Property
            dialog: false,
            dialogDelete: false,

            //Password Property
            showpassForm: false,
            showpass: false,
            showconfirmpass: false,

            //NOTIFY PROPERTIES
            error: "",
            msgStatus: false,
            load: false,

            //TABLE HEADERS PROPERTIES
            headers: [
                {
                    text: "ID",
                    align: "start",
                    sortable: true,
                    value: "user_id",
                },
                {
                    text: "Name",
                    value: "name",
                },
                {
                    text: "Email",
                    value: "email",
                },
                {
                    text: "Address",
                    value: "address",
                },
                {
                    text: "Contact No.",
                    value: "phone_no",
                },
                {
                    text: "User Type",
                    value: "user_type",
                },
                {
                    text: "Actions",
                    value: "actions",
                    sortable: false,
                },
            ],

            //USERS PROPERTIES
            users: [],
            selectedUser: null,
            editedIndex: -1,
            usertype: ["Chief", "Staff"],

            //FORM PROPERTIES
            form: {
                name: "",
                email: "",
                address: "",
                phone_no: "",
                password: "",
                password_confirmation: "",
                user_type: "",
            },

            //RULES VALIDATION PROPERTIES
            rules: {
                isValid: true,
                name: [v => !!v || "Name is required"],
                email: [
                    v => !!v || "E-mail is required",
                    v => /.+@.+\..+/.test(v) || "E-mail must be valid",
                ],
                address: [v => !!v || "Address is required"],
                phone_no: [v => !!v || "Phone No. is required"],
                user_type: [v => !!v || "User type is required"],
                password: [
                    v => !!v || "Password is required",
                    v => (v && v.length >= 5) || "Passowrd must atleast 10 characters",
                ],
                password_confirmation: [
                    v => !!v || "Password confirmation is required",
                    v => v === this.form.password || "The password must be match",
                ]
            },

            //DEFAULT FORM DATA
            defaultItem: {
                name: "",
                email: "",
                address: "",
                phone_no: "",
                user_type: "",
            },
        }
    },
    computed: {
        //FETCH USER FROM STATE MANANGEMENT COMPUTED
        fetchUser() {
            const users = this.$store.getters.getUsers
            return this._.orderBy(users, ["created_at"], ["desc"]);
        },

        //FORM TITLE COMPUTED
        formTitle() {
            return this.editedIndex === -1 ? "New User" : "Update User";
        },

        //ISLOADING COMPUTED
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading
            },
            set: function (newVal) {
                return newVal
            }
        }
    },

    watch: {
        //CLOSE MODAL
        dialog(val) {
            val || this.close();
        },
        // DELETE MODAL
        dialogDelete(val) {
            val || this.closeDelete();
        },
        // LOADING
        isLoading(val) {
            val || this.close()
        }
    },

    methods: {

        //EDIT USER DATA
        editItem(item) {
            this.editedIndex = this.fetchUser.indexOf(item);
            this.form = Object.assign({}, item);
            this.dialog = true;
        },

        //DELETE USER DATA
        deleteItem(item) {
            this.selectedUser = item
            this.dialogDelete = true;
        },

        //CONFIRM DELETE USER
        async deleteItemConfirm() {
            this.msgStatus = true
            await this.$store.dispatch("deleteUser", this.selectedUser)
            this.closeDelete();
        },


        //MODAL CLOSE
        close() {
            this.dialog = false;
            this.$nextTick(() => {
                this.$refs.form.reset()
                this.editedIndex = -1;
            });
        },


        //CLOSE DELETE CONFIRMATION
        closeDelete() {
            this.dialogDelete = false;
            this.$nextTick(() => {
                this.form = Object.assign({}, this.defaultItem);
                this.editedIndex = -1;
            });
        },


        //CALL STORE MANANGEMENT DISPATCH FOR UPDATING DATA TO STATE MANANGEMENT
        async updateUser() {
            this.$store.dispatch("updateUser", this.form)
        },

        //CALL STORE MANANGEMENT DISPATCH FOR ADDING DATA TO STATES
        async addUser() {
            await this.$store.dispatch("addUser", this.form);
        },

        //SAVE BUTTON ( SEND FORM DATA TO DATABASE)
        save() {
            this.msgStatus = true;

            //Check if actions update or add
            if (this.editedIndex > -1) {
                this.updateUser()
            } else {
                this.$refs.form.validate();
                this.addUser();
            }

        },
    },

};
</script>

<style scoped>
::v-deep .v-data-table-header {
    background-color: #4CAF50;
}
</style>
