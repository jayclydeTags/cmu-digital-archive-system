<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <div class="border-left mb-10" data-aos="fade-up">
                    <h2 class="about-heading text-uppercase">About Us</h2>
                </div>
            </v-col>
            <v-col cols="12" md="5">
                <div data-aos="fade-up">
                    <p class="text-body-1 blue-grey--text text--darken-4">
                        Records Management Unit (RMU)- A unit of Central Mindanao University under the Administrative
                        Services Office that concerns on the management of records. It operates within the parameters of
                        the National Archives of the Philippines by making sure that all records management activities
                        within this institution are performed within the boundaries of the law.
                    </p>
                    <p class="text-body-1 blue-grey--text text--darken-4">
                        The <span class="emphasize">Digital Archive System</span> is an online tool that is part of CMU
                        Records Management Unit, this is developed to improve the RMU in providing services to its
                        constituents. This serves as the gateway for students, faculties and alumnus to access important
                        records or material to improve materials supporting their learning and providing unique
                        materials for quality education.
                    </p>
                </div>
            </v-col>
            <v-col cols="12" md="7">
                <v-row>
                    <v-col cols="12" >
                        <v-card class="mx-auto pa-xs-4 pa-md-6 rounded-lg"
                            :color="archives_mission"
                            flat
                            shaped
                            tile
                            data-aos="fade-up">
                            <v-card-title class="text-uppercase mission-heading">Archives Mission</v-card-title>
                            <v-card-text>
                                <p class="text-body-1 blue-grey--text text--darken-4">
                                    We, the Records Management Unit envisioned in creating and
                                    developing an electronic database filing system which would
                                    lessen the time needed in finding requested
                                    records/documents and to have a program that would let us
                                    store electronic copies of records with high archival value
                                    and those records that is vital for the University.
                                </p>
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12">
                        <v-card class="mx-auto pa-xs-4 pa-md-6 rounded-lg"
                            :color="rmu_mission"
                            flat
                            shaped
                            tile
                            data-aos="fade-up">
                                <v-card-title class="text-uppercase mission-heading">Mission of the RMU</v-card-title>
                                <v-card-text>
                                    <p class="text-body-1 blue-grey--text text--darken-4">
                                        To appraise or identify the records/documents as to what
                                        should be retained as archival value, which are to be kept
                                        for specific periods and which will be destroyed, in
                                        consultation with the higher office and in compliance with
                                        legal requirements as disposition statutes.
                                    </p>
                                </v-card-text>
                            </v-card>
                    </v-col>
                </v-row>
            </v-col>

        </v-row>
    </v-container>
    <!-- <div class="section features-6 about-content">
        <div class="container">
            <div class="row align-items-center">
                <div class="col-lg-6">
                    <div class="info-horizontal">
                        <h3 class="display-8 text-uppercase">| About Us</h3>
                        <div class="description pl-4 text-black p-3" data-aos="fade-up">
                            <p class="p-about">
                                Records Management Unit (RMU)- A unit of Central Mindanao
                                University under the Administrative Services Office that
                                concerns on the management of records. It operates within the
                                parameters of the National Archives of the Philippines by making
                                sure that all records management activities within this
                                institution are performed within the boundaries of the law.
                                <br />
                            </p>
                            <p>
                                The Digital Archive System is an online tool that is part of CMU
                                Records Management Unit, this is developed to improve the RMU in
                                providing services to its constituents. This serves as the
                                gateway for students, faculties and alumnus to access important
                                records or material to improve materials supporting their
                                learning and providing unique materials for quality education.
                            </p>
                        </div>
                    </div>
                </div>
                <div class="col-lg-6 col-10 mx-md-auto" data-aos="fade-up" data-aos-duration="1000">
                    <div class="row">
                        <div class="col-md-12">
                            <v-card class="mx-auto archives-mission" :color="archives_mission" elevation="2" outlined
                                shaped tile>
                                <v-card-title class="text-uppercase text-green">Archives Mission</v-card-title>
                                <v-card-text>
                                    <p>
                                        We, the Records Management Unit envisioned in creating and
                                        developing an electronic database filing system which would
                                        lessen the time needed in finding requested
                                        records/documents and to have a program that would let us
                                        store electronic copies of records with high archival value
                                        and those records that is vital for the University.
                                    </p>
                                </v-card-text>
                            </v-card>
                        </div>

                        <div class="col-md-12 mt-5">
                            <v-card class="mx-auto" elevation="2" :color="rmu_mission" outlined shaped tile>
                                <v-card-title class="text-uppercase text-green">Mission of the RMU</v-card-title>
                                <v-card-text>
                                    <p>
                                        To appraise or identify the records/documents as to what
                                        should be retained as archival value, which are to be kept
                                        for specific periods and which will be destroyed, in
                                        consultation with the higher office and in compliance with
                                        legal requirements as disposition statutes.
                                    </p>
                                </v-card-text>
                            </v-card>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div> -->
</template>
<script>
// import about from "../../../../../public/assets/img/about2.png";
export default {
    data() {
        return {
            // about: about,
            archives_mission: "#d1fae5",
            rmu_mission: "#fef08a",
            cardTitle: "#1e8133",
        };
    },
};
</script>
<style>

.about-heading, .emphasize, .mission-heading {
    font-family: 'Roboto Mono', monospace;
}

.about-heading, .mission-heading {
    font-weight: 700;
}

.border-left {
    border-left: 4px solid #1E293B;
    padding-left: 0.8rem;
}

.emphasize, .mission-heading {
    color: #16A34A;
}

.emphasize {
    color: #16A34A;
    font-weight: 600;
}
</style>
