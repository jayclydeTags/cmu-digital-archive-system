<template>
    <div>
        <v-row>
            <v-col cols="12">
                <v-card flat class="card-title--background rounded-lg">
                    <v-card-title class="text-uppercase grey--text text--lighten-5 ">
                        Dashboard
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" md="6">
                <system-summary></system-summary>
            </v-col>
            <v-col cols="12" md="6">
                <upload-chart :chartData="uploadReport" :options="options" />
            </v-col>

        </v-row>
        <v-divider></v-divider>
        <v-row>
            <v-col cols="12" md="6">
                <request-summary></request-summary>
            </v-col>
            <v-col cols="12" md="6">
                <request-reports-chart :chartData="requestReport" :options="options" />
            </v-col>
        </v-row>
    </div>
</template>
<script>
import SystemSummary from "./SystemSummary.vue";
import RequestReportsChart from './../reports/RequestChart.vue'
import RequestReportsTable from './../reports/ReportsTable.vue'
import RequestSummary from './RequestSummary.vue'
import UploadChart from './../reports/UploadChart.vue'
export default {
    components: {
        SystemSummary,
        RequestSummary,
        RequestReportsChart,
        RequestReportsTable,
        UploadChart
    },
    data() {
        return {
            period: 'Daily'
        }
    },
    computed: {
        getFileRequestReports() {
            return this.$store.state.requests.file_request_reports
        },

        fetchRequestReports() {
            return this.$store.state.requests.request_report
        },
        fetchFilesForDisposal() {
            return this.$store.getters.getDocumentsForDisposal
        },
        fetchUploadReports() {
            return this.$store.state.files.upload_reports_monthly
        },
        requestReport() {
            return this.requestReportChart()
        },
        uploadReport() {
            return this.uploadReportChart()
        },
        //CHART OPTIONS
        options() {
            return {
                responsive: true,
                maintainAspectRatio: false,
                title: {
                    display: true,
                    text: "Reports",
                },
            };
        },

    },

    methods: {
        requestReportChart() {
            let request_reports = this.fetchRequestReports
            let daily_date = request_reports.map((item) => item.date);
            let daily_total = request_reports.map((item) => item.total);
            let chartData = {
                labels: daily_date,
                datasets: [
                    {
                        label: "Requests",
                        backgroundColor: "#FFB74D",
                        data: daily_total,
                    },
                ],
            };
            return chartData
        },

        uploadReportChart() {

            let request_reports = this.fetchUploadReports
            let date = request_reports.map((item) => item.date)
            let totaluploaded = request_reports.map((item) => item.total_uploaded)
            let totalarchive = request_reports.map((item) => item.total_archive)
            let totaldispose = request_reports.map((item) => item.total_dispose)
            let chartData = {
                labels: date,
                datasets: [
                    {
                        label: "Uploads",
                        backgroundColor: "#1E88E5",
                        data: totaluploaded,
                    },
                    {
                        label: "Archived",
                        backgroundColor: "#FFB74D",
                        data: totalarchive,
                    },
                    {
                        label: "Disposed",
                        backgroundColor: "#EC407A",
                        data: totaldispose,
                    },
                ],
            };
            return chartData
        },
    },
    created() {
        this.$store.dispatch("getFileList")
        this.$store.dispatch("getRequests")
        this.$store.dispatch("getFileLocations")
        this.$store.dispatch("getRequestReportsDaily")
        this.$store.dispatch("getUploadReportsMonthly")
        this.$store.dispatch("getFileRequestReports")
    },
};
</script>
<style>

.card-title--background {
    background: rgb(15, 32, 39);
    background: linear-gradient(103deg, rgba(15, 32, 39, 1) 0%, rgba(32, 58, 67, 1) 50%, rgba(44, 83, 100, 1) 100%);
}
</style>

