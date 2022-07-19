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
        <v-card>
            <!-- <v-card-title>
                <v-spacer></v-spacer>
                <v-text-field v-model="search" label="Search" outlined dense prepend-inner-icon="mdi-magnify">
                </v-text-field>
            </v-card-title> -->



            <!-- User Table -->
            <v-data-table :headers="headers" :items="fetchFilesForDisposal" :search="search" :loading="isLoading"
                class=" table-striped">
                <template v-slot:item.retention_date="{ item }">
                    <span>{{ new Date(item.retention_date).toLocaleDateString() }}</span>
                </template>
                <template v-slot:top>
                    <v-toolbar flat>
                        <v-toolbar-title>List of Records Pending for Disposal</v-toolbar-title>
                        <v-divider class="mx-4" inset vertical></v-divider>
                        <v-spacer></v-spacer>


                        <!-- User Management Modal -->

                        <v-dialog v-model="dialog" max-width="550px">
                            <v-card>
                                <v-toolbar color="error" dark>
                                    <v-toolbar-title>Record Disposal</v-toolbar-title>
                                </v-toolbar>
                                <v-container>
                                    <v-form ref="form" @submit.prevent="save" v-model="rules.isValid" lazy-validation>
                                        <v-alert text type="warning">
                                            This will delete permanently the record, make sure to review the record. Do
                                            you want to proceed?
                                        </v-alert>
                                        <v-row>
                                            <v-col cols="12">
                                                <v-select v-model="form.retention_status" label="Select Status"
                                                    :items="selectStatus" required>
                                                </v-select>
                                            </v-col>
                                        </v-row>
                                    </v-form>
                                </v-container>

                                <!-- Form Buttons -->
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="error" dark @click="close"> Cancel </v-btn>
                                    <v-btn :disabled="!rules.isValid" color="success" dark @click="save"
                                        :loading="isLoading">
                                        Confirm
                                    </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>

                    </v-toolbar>
                </template>

                <!-- Table Actions Buttons -->
                <template v-slot:item.actions="{ item }" >
                    <v-btn-toggle v-model="icon" borderless v-show="auth.user_type === 'Staff'">
                        <v-btn small value="left" color="error" @click="editItem(item)">
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

            //Dialog Property
            dialog: false,
            dialogDelete: false,
            selectStatus: ['Active', 'Dispose'],


            //NOTIFY PROPERTIES
            error: "",
            msgStatus: false,
            load: false,

            //TABLE HEADERS PROPERTIES
            headers: [
                {
                    text: "Code",
                    align: "start",
                    sortable: true,
                    value: "code",
                },
                {
                    text: "File Name",
                    value: "filename",
                },
                {
                    text: "Description",
                    value: "filename",
                },
                {
                    text: "Status",
                    value: "retention_status",
                },
                {
                    text: "Date",
                    value: "retention_date",
                },
                {
                    text: "Actions",
                    value: "actions",
                    sortable: false,
                },
            ],

            //Category  PROPERTIES

            editedIndex: -1,
            selectedItem: null,



            //FORM PROPERTIES
            form: {
                retention_status: null,
                file_id: null
            },

            //RULES VALIDATION PROPERTIES
            rules: {
                isValid: true,
                category: [v => !!v || "Category Name is required"],

            },

            //DEFAULT FORM DATA
            defaultItem: {
                retention_status: null,
                file_id: null
            },
        }
    },
    computed: {

        //FETCH CATEGORY DATA FROM STATE MANANGEMENT COMPUTED
        fetchCategory() {
            const category = this.$store.getters.getCategory
            return this._.orderBy(category, ["created_at"], ["desc"]);
        },
        fetchFilesForDisposal() {
            const fileDisposal = this.$store.state.files.file_disposal
            return this._.orderBy(fileDisposal, ["created_at"], ["desc"]);
        },

        //FORM TITLE COMPUTED
        formTitle() {
            return this.editedIndex === -1 ? "New Category" : "Update Category";
        },

        //ISLOADING COMPUTED
        isLoading: {
            get: function () {
                return this.$store.state.base.isLoading
            },
            set: function (newVal) {
                return newVal
            }
        },

        //USER
        auth() {
            return this.$store.state.auth.user
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

        //EDIT FILE CATEGORY DATA
        editItem(item) {
            this.form = Object.assign({}, item);
            this.dialog = true;
            this.editedIndex = 1
        },


        //MODAL CLOSE
        close() {
            this.dialog = false;
            this.$nextTick(() => {
                this.form = Object.assign({}, this.defaultItem);
                this.editedIndex = -1
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
        async updateFileForDisposal() {
            this.$store.dispatch("updateFile", this.form)
        },

        //SAVE BUTTON ( SEND FORM DATA TO DATABASE)
        save() {
            this.msgStatus = true;
            this.updateFileForDisposal()


        },
    },


};
</script>
<style scoped>
::v-deep .v-data-table-header {
    background-color: #4CAF50;
}
</style>
