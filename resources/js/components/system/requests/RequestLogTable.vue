<template>
    <div>
        <!-- Alert Message -->
        <div v-if="msgStatus">
            <alert-component />
        </div>

        <v-card flat class="mb-2 pa-4 rounded-lg">
            <v-row>
                <v-col cols="12" sm="6" md="4">
                    <v-text-field class="rounded" dense v-model="search" label="Search" append-icon="mdi-magnify"
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



            <!-- REQUESTS TABLE -->
            <v-data-table :headers="headers" v-model="selected" :items="fetchRequests" :search="search"
                item-key="request_id" :single-select="singleSelect" :loading="isLoading" show-select
                class="table-striped">
                <template v-slot:item.request_date="{ item }">
                    <span>{{ new Date(item.request_date).toLocaleDateString() }}</span>
                </template>
                <template v-slot:item.expiration_date="{ item }">
                    <span>{{ new Date(item.expiration_date).toLocaleDateString() }}</span>
                </template>
                <template v-slot:item.status="{ item }">
                    <v-chip small :color="getColor(item.status)" dark>
                        {{ item.status }}
                    </v-chip>
                </template>

                <template v-slot:top>
                    <v-toolbar flat>
                        <v-toolbar-title>List of Requests</v-toolbar-title>
                        <v-spacer></v-spacer>
                        <v-btn-toggle v-model="icon" borderless>
                            <v-btn color="error" :loading="isLoading" @click="deleteItem">
                                <v-icon center class="text-white"> mdi-delete </v-icon>
                                <span class="hidden-sm-and-down">Delete</span>

                            </v-btn>
                        </v-btn-toggle>


                        <!-- <div class="d-flex" v-show="auth.user_type === 'Staff'">
                            <v-switch v-model="singleSelect" label="Single Select">
                            </v-switch>
                            <v-btn-toggle v-model="icon" borderless>
                                <v-btn color="error" :loading="isLoading" @click="deleteItem">
                                    <span class="hidden-sm-and-down">Delete</span>

                                    <v-icon right class="text-white"> mdi-delete </v-icon>
                                </v-btn>
                            </v-btn-toggle>
                        </div> -->


                        <!-- Delete Confirmation Modal -->
                        <v-dialog v-model="dialogDelete" max-width="500px">
                            <v-card>
                                <v-toolbar color="error" dark>
                                    <v-toolbar-title class="text-h6">
                                        Confirmation
                                    </v-toolbar-title>
                                </v-toolbar>
                                <v-container>
                                    <v-alert text type="error">
                                        Are you sure you want to delete this request(s)? This action can will remove all the data in the report request table. <br>
                                        Are you still sure you want to delete this request?
                                    </v-alert>
                                </v-container>

                                <!-- <v-card-title class="text-h5">Are you sure you want to delete this item(s)?
                                </v-card-title> -->
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
            </v-data-table>
        </v-card>
    </div>
</template>
<script>
import AlertComponent from "../../AlertComponent.vue";
export default {
    components: { AlertComponent },
    data() {
        return {
            icon: "justify",
            //TABLE SEARCH PROPERTY
            search: "",

            //Dialog Property
            dialog: false,
            dialogDelete: false,

            //NOTIFY PROPERTIES
            error: "",
            msgStatus: false,
            load: false,

            singleSelect: false,
            selected: [],

            //TABLE HEADERS PROPERTIES
            headers: [
                {
                    text: "File Name",
                    value: "filename"
                },
                {
                    text: "Code",
                    value: "code"
                },
                {
                    text: "Status",
                    value: "status"
                },
                {
                    text: "Request Date",
                    value: "request_date"
                },
                {
                    text: "Expiration Date",
                    value: "expiration_date"
                },
            ],

            //REQUEST PROPERTIES
            editedIndex: -1,
            selectedItem: null,
            item: ["Pending", "Approved", "Expired"],

            //FORM PROPERTIES
            form: {
                request_id: null,
                status: "",
            },

            //RULES VALIDATION PROPERTIES
            rules: {
                isValid: true,
                filename: [(v) => !!v || "Filename is required"],
                description: [(v) => !!v || "Description is required"],
                file_location: [(v) => !!v || "File Location is required"],
            },

            //DEFAULT FORM DATA
            defaultItem: {
                request_id: null,
                status: "",
            },
        };
    },
    computed: {
        //FETCH FILE REQUESTS FROM STATE MANANGEMENT COMPUTED
        fetchRequests() {
            const files = this.$store.getters.getRequestsLogStaff;
            return this._.orderBy(files, ["created_at"], ["desc"]);
        },
        selectedRequest() {
            let request_id = this.selected.map((item) => {
                return item.request_id;
            });
            return request_id;
        },
        auth() {
            return this.$store.state.auth.user;
        },

        //FORM TITLE COMPUTED
        formTitle() {
            return this.editedIndex === -1 ? "New File" : "Update File";
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
            val || this.close();
        },
    },

    methods: {
        getColor(status) {
            if (status === "Approved") return "green";
            else if (status === "Denied") return "red";
            else return "orange";
        },

        //DELETE REQUESTS DATA
        deleteItem() {
            /* this.selectedItem = item; */
            if (this.selected.length <= 0) {
                alert("Select Request to delete");
            } else {
                this.dialogDelete = true;
            }
        },

        //CONFIRM DELETE FILE REQUEST
        async deleteItemConfirm() {
            this.msgStatus = true;
            await this.$store.dispatch("deleteMultipleRequest", this.selectedRequest);
            //await this.$store.dispatch("deleteRequest", this.selectedItem.request_id);
            this.closeDelete();
        },

        //MODAL CLOSE
        close() {
            this.dialog = false;
            this.$nextTick(() => {
                this.form = Object.assign({}, this.defaultItem);
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
    },
    mounted() { },
};
</script>
<style scoped>
::v-deep .v-data-table-header {
    background-color: #4CAF50;
}
</style>
