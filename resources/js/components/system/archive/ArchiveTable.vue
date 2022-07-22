<template>


    <div>
        <!-- Alert Message -->
        <div v-if="msgStatus">
            <alert-component />
        </div>

        <v-card flat class="mb-2 pa-4 rounded-lg">
            <v-row>
                <v-col cols="12" sm="6" md="4">
                    <v-text-field  dense v-model="search" label="Search" append-icon="mdi-magnify"
                        hint="Search for keywords">
                    </v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                    <select-file-category @selectcategory="getCategory"></select-file-category>
                </v-col>
            </v-row>
        </v-card>


        <v-card flat class="rounded-lg">
            <!-- FILES TABLE  -->
            <v-data-table
                :headers="headers"
                :items="fetchDocuments"
                :search="search"
                class="table-striped"> -->
                <!-- CHANGE DATE FORMAT FROM DATABASE -->
                <template v-slot:item.created_at="{ item }">
                    <span>{{ new Date(item.created_at).toLocaleDateString() }}</span>
                </template>
                <template v-slot:item.retention_date="{ item }">
                    <span>{{ new Date(item.retention_date).toLocaleDateString() }}</span>
                </template>
                <template v-slot:item.archive="{ item }">
                    <v-chip small :color="getColor(item.archive)" dark>
                        {{ item.archive }}
                    </v-chip>
                </template>
                <template v-slot:top>
                    <v-toolbar flat>
                        <v-toolbar-title>Archived Records</v-toolbar-title>

                        <!-- FILES MANAGEMENT MODALS -->
                        <v-dialog v-model="dialog" max-width="450px">
                            <v-card>
                                <v-toolbar color="success" dark>
                                    <v-toolbar-title> {{ formTitle }} </v-toolbar-title>
                                </v-toolbar>
                                <v-container>
                                    <v-alert text type="info" class="text-body-2">
                                        Once this record is archived, update and delete feature for this records is
                                        disabled unless you remove this record from the Archive.
                                        It's recommended to Archive those permanent records.
                                    </v-alert>
                                    <v-form ref="form" @submit.prevent="save">
                                        <v-select
                                            v-model="form.archive"
                                            append-icon="mdi-archive"
                                            :items="selectItem"
                                            required
                                            select
                                            return-object>
                                        </v-select>
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
                    </v-toolbar>
                </template>

                <!-- Table Actions Buttons -->
                <template v-slot:item.actions="{ item }">
                    <v-btn-toggle v-model="icon" borderless>
                        <v-btn small value="left" color="success" @click="editItem(item)">
                            <v-icon small class="text-white"> mdi-archive-outline </v-icon>
                        </v-btn>
                    </v-btn-toggle>
                </template>
            </v-data-table>
        </v-card>
    </div>








</template>
<script>
import SelectFileCategory from "./../files/SelectFileCategory.vue";
import AlertComponent from "./../../AlertComponent.vue";
export default {
    components: { AlertComponent, SelectFileCategory },
    data() {
        return {
            category_id: 0,
            icon: "justify",
            //TABLE SEARCH PROPERTY
            search: "",
            file_id: null,
            selectedFile: null,
            selectedItem: [],
            selectItem: ["Archive", "Unarchive"],

            //Dialog Property
            dialog: false,
            dialogDelete: false,

            //NOTIFY PROPERTIES
            error: "",
            msgStatus: false,
            load: false,

            //TABLE HEADERS PROPERTIES
            headers: [
                {
                    text: "Ref. Code",
                    align: "start",
                    sortable: true,
                    value: "code",
                    class: "green text-black",
                },
                { text: "File Name", value: "filename", class: "green text-black" },
                // { text: "Slug", value: "slug", class: "green text-black" },
                { text: "Category", value: "category", class: "green text-black" },
                { text: "Uploaded", value: "created_at", class: "green text-black" },
                {
                    text: "Retention",
                    value: "retention_date",
                    class: "green text-black",
                },
                { text: "Status", value: "archive", class: "green text-black" },

                {
                    text: "Actions",
                    value: "actions",
                    sortable: false,
                    class: "green text-black",
                },
            ],

            //FILES PROPERTIES
            editedIndex: -1,

            //FORM PROPERTIES
            form: {
                file_id: null,
                archive: null,
            },

            //RULES VALIDATION PROPERTIES
            rules: {
                isValid: true,
                file_location: [(v) => !!v || "Select is File"],
            },

            //DEFAULT FORM DATA
            defaultItem: {
                file_id: null,
                archive: null,
            },
        };
    },
    computed: {
        category() {
            return this.$store.state.filecategory;
        },

        //FETCH FILES FROM STATE MANANGEMENT COMPUTED
        getUserId() {
            return this.$store.state.auth.user.user_id;
        },
        fetchDocuments() {

            if (this.category_id === 0) {
                const documents = this.$store.getters.getApprovedDocuments
                return this._.orderBy(documents, ["created_at"], ["desc"]);
            } else {
                const documents = this.$store.getters.filterFilesByCategory(
                    this.category_id
                );
                return this._.orderBy(documents, ["created_at"], ["desc"]);
            }
        },

        //FORM TITLE COMPUTED
        formTitle() {
            if (this.editedIndex === 0) {
                return "Archive Record";
            }

            return this.editedIndex === -1 ? "New File" : "Archive Record";
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

        // LOADING
        isLoading(val) {
            val || this.close();
        },
    },

    methods: {
        getCategory(category) {
            this.category_id = category;
        },
        getColor(item) {
            if (item === "Unarchive") return "red";
            else return "green";
        },

        editItem(item) {
            this.editedIndex = this.fetchDocuments.indexOf(item);
            this.form = Object.assign({}, item);
            this.dialog = true;
        },

        //MODAL CLOSE
        close() {
            this.dialog = false;

            this.$nextTick(() => {
                this.form = Object.assign({}, this.defaultItem);
                this.editedIndex = -1;
            });
        },
        async updateDocument() {
            await this.$store.dispatch("updateFile", this.form);
        },

        //SAVE BUTTON ( SEND FORM DATA TO DATABASE)
        save() {
            this.msgStatus = true;
            this.$refs.form.validate();
            this.updateDocument();
        },
    },
};
</script>
