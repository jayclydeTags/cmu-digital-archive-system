<template>
    <div>
        <v-row>
            <v-col cols="12" sm="16" md="12">
                <v-card>
                    <v-data-table :headers="headers" :items="data" dense class="table-striped">
                        <template v-slot:top>
                            <v-toolbar flat>
                                <v-toolbar-title>{{ formattedTitle }}: {{ data.length }}</v-toolbar-title>
                                <v-divider class="mx-4" inset vertical></v-divider>
                                <v-spacer></v-spacer>
                            </v-toolbar>
                        </template>
                    </v-data-table>
                </v-card>
            </v-col>
            <!-- <v-col cols="12">
                <v-card v-show="report == 'request_reports'">
                    <v-data-table
                        :headers="fileRequestReportsTableHeader"
                        :items="filerequestreports"
                        dense
                        class="elevation-1 table-striped"

                    >
                        <template v-slot:top>
                        <v-toolbar flat>
                            <v-toolbar-title>File Requests Reports Table</v-toolbar-title>
                            <v-divider class="mx-4" inset vertical></v-divider>
                            <v-spacer></v-spacer>
                        </v-toolbar>
                        </template>
                    </v-data-table>
                    </v-card>
            </v-col> -->
        </v-row>
    </div>
</template>
<script>
import jsPDF from 'jspdf'
import 'jspdf-autotable'

export default {
    props: ["period", "data", "filerequestreports", "report"],
    data() {
        return {
            table_title: "",
            icon: "justify",
            //TABLE HEADERS PROPERTIES REQUEST REPORTS
            headers: [],
            //TABLE HEADERS PROPERTIES REQUEST REPORTS
            fileRequestReportsTableHeader: [
                {
                    text: "Filename",
                    align: "start",
                    sortable: true,
                    value: "filename",
                },
                {
                    text: "Category",
                    value: "category",
                },
                {
                    text: "Total of Requests",
                    value: "totalrequests",
                },
            ],

            //USERS PROPERTIES
            users: [],
            selectedUser: null,
            editedIndex: -1,
            usertype: ["Chief", "Staff"],
        };
    },
    computed: {
        formattedTitle() {
            switch (this.report) {
                case 'request_reports':
                    return 'Total No. Of Requested Records';
                    break;
                case 'upload_reports':
                    return `${this.period} Document Reports`;
                    break;
            }
        }
    },
    created() {
        if (this.report === 'request_reports') {
            this.headers.push({
                text: "User ID",
                align: "start",
                sortable: true,
                value: "date_uploaded",
            },
                {
                    text: "File name",
                    value: "filename",
                },
                {
                    text: "Category",
                    value: "category",
                },
                {
                    text: "Purpose",
                    value: "purpose",
                },
                {
                    text: "Attached File",
                    value: "file_location",
                },
                {
                    text: "Req. Date",
                    value: "request_date",
                })
        } else {
            this.headers.push({
                text: "Date Uploaded",
                align: "start",
                sortable: true,
                value: "date_uploaded",
            },
                {
                    text: "Ref code",
                    value: "code",
                },
                {
                    text: "File name",
                    value: "filename",
                },
                {
                    text: "Category",
                    value: "category",
                },
                {
                    text: "Retention date",
                    value: "retention_date",
                },
            )
        }
    }
};
</script>
<style scoped>

::v-deep .v-data-table-header {
    background-color: #4CAF50;
}
</style>
