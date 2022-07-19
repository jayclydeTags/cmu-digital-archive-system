<template>
    <v-container>
        <v-row>
            <v-col cols="12">
                <div class="border-left mb-10" data-aos="fade-up">
                    <h2 class="faqs-heading text-uppercase">FREQUENTLY ASK QUESTIONS</h2>
                </div>
            </v-col>

            <!-- <v-col cols="12" md="6">
                <div data-aos="fade-right">
                    <h5 class="text-uppercase faqs-title">Faculty and alumnus</h5>
                    <v-expansion-panels>
                        <v-expansion-panel v-for="(item, i) in extentionPanelFacultyAlumnus" :key="i">
                            <v-expansion-panel-header :color="item.qitemBackground">
                                {{ item.question }}
                            </v-expansion-panel-header>
                            <v-expansion-panel-content>
                                {{ item.answer }}
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </div>
            </v-col> -->
            <v-col cols="12" sm="4" class="d-block">
                <div data-aos="fade-right">
                    <figure>
                        <v-img :src="FAQs_image" class="figure-img img-fluid" width="100%" />
                    </figure>
                </div>
            </v-col>

            <v-col cols="12" sm="8">
                <div data-aos="fade-left">
                    <!-- <h5 class="text-uppercase faqs-title">Students</h5> -->
                    <v-expansion-panels>
                        <v-expansion-panel v-for="(item, i) in extentionPanelStudents" :key="i">
                            <v-expansion-panel-header class="green lighten-5">
                                {{ item.question }}
                            </v-expansion-panel-header>
                            <v-expansion-panel-content>
                                <p class="text-body-1">{{ item.answer }}</p>
                                <v-list>
                                    <v-list-item v-for="(item, i) in item.lists" :key="i">
                                        <v-list-item-content>
                                            <v-list-item-subtitle class="text-wrap" v-text="item.list">
                                            </v-list-item-subtitle>
                                        </v-list-item-content>
                                    </v-list-item>
                                </v-list>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-expansion-panels>
                </div>
            </v-col>
        </v-row>
    </v-container>
    <!-- <div class="section section-hero section-shaped">
        <div class="shape shape-style-3 search"></div>
        <div class="page-header">
            <div class="container shape-container align-items-center py-lg">
                <div class="row">
                    <h3 class="display-8 text-uppercase">| Frequently Ask Questions</h3>
                    <div class="col-md-6" data-aos="zoom-in-right">
                        <h5 class="text-uppercase text-green">Faculty and alumnus</h5>
                        <v-expansion-panels>
                            <v-expansion-panel v-for="(item, i) in extentionPanelFacultyAlumnus" :key="i">
                                <v-expansion-panel-header :color="item.qitemBackground"> Question #{{ i + 1 }}
                                </v-expansion-panel-header>
                                <v-expansion-panel-content>
                                    {{ item.questionText }}
                                </v-expansion-panel-content>
                            </v-expansion-panel>
                        </v-expansion-panels>
                    </div>
                    <div class="col-md-6" data-aos="zoom-in-left">
                        <h5 class="text-uppercase text-green">Students</h5>
                        <v-expansion-panels>
                            <v-expansion-panel v-for="(item, i) in extentionPanelStudents" :key="i">
                                <v-expansion-panel-header :color="item.qitemBackground"> Question #{{ i + 1 }}
                                </v-expansion-panel-header>
                                <v-expansion-panel-content>
                                    {{ item.questionText }}
                                </v-expansion-panel-content>
                            </v-expansion-panel>
                        </v-expansion-panels>
                    </div>
                </div>
            </div>
        </div> -->
    <!-- <div class="separator separator-bottom separator-skew zindex-100">
      <svg
        x="0"
        y="0"
        viewBox="0 0 2560 100"
        preserveAspectRatio="none"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
      >
        <polygon class="fill-white" points="2560 0 2560 100 0 100"></polygon>
      </svg>
    </div> -->
    <!-- </div> -->
</template>
<script>
import FAQs from "../../../../assets/img/FAQs.svg";
// import office from "../../../../../public/assets/img/office/office.png";
export default {
    data() {
        return {
            FAQs_image: FAQs,
            // extentionPanelFacultyAlumnus: [
            //     {
            //         qitemBackground: '#E2E8F0',
            //         question: 'How to make request for Faculty and Staff?',
            //         answer: 'Question Content Faculty & Alumnus 1'
            //     },
            //     {
            //         qitemBackground: '#E2E8F0',
            //         question: 'How to make request for Faculty and Staff?',
            //         answer: 'Question Content Faculty & Alumnus 2'
            //     },
            //     {
            //         qitemBackground: '#E2E8F0',
            //         question: 'How to make request for Faculty and Staff?',
            //         answer: 'Question Content Faculty & Alumnus 3'
            //     },
            //     {
            //         qitemBackground: '#E2E8F0',
            //         question: 'How to make request for Faculty and Staff?',
            //         answer: 'Question Content Faculty & Alumnus 4'
            //     }
            // ],
            extentionPanelStudents: [
                {
                    question: 'How to login into the system?',
                    answer: 'You have to register into the system to be able to login, just click the Register button if you have no account yet. Inpu all neccessary fields in the registration form, it is recommended to use your Institutional Email for registration.'
                },
                {
                    question: 'What are the requirements in creating a request?',
                    answer: 'You are required to provide the following documents:',
                    lists: [
                        {
                            list: 'For Students you have to secure a COMMUNICATION LETTER and make sure it is approved by your adviser.',
                        },
                        {
                            list: 'For Faculty or Alumni you have to secure LETTER OF INTENT and make sure it is addressed to the RMU Chief.'
                        }
                    ]
                },
                {
                    question: 'How to request a record?',
                    answer: 'The system has a straight forward process, just follow the steps.',
                    lists: [
                        {
                            list: 'Step 1. Search your desired records on the search field. You can use the Category filter or keywords for your related record.'
                        },
                        {
                            list: 'Step 2. When you found your desired record just click the Request Records Button a form will pop-up. '
                        },
                        {
                            list: 'Step 3. Fill out the form for Request and specify your pupose of request and dont forget to attach the required document. Then Click the Submit button.'
                        },
                        {
                            list: 'Step 4. You can view your requests in the Request Menu and the status of your request.'
                        },
                        {
                            list: 'Step 5. By default your request is pending, once your request status is approved you are now able to view your requested record.'
                        }
                    ]
                },
                // {
                //     question: 'How to make request for students?',
                //     answer: 'Question Content Students 3'
                // },
                // {
                //     question: 'How to make request for students?',
                //     answer: 'Question Content Students 4'
                // }
            ]




        };
    },
};
</script>
<style scoped>
.faqs-heading,
.faqs-title {
    font-weight: 700;
    font-family: 'Roboto Mono', monospace;

}

.border-left {
    border-left: 4px solid #1E293B;
    padding-left: 0.8rem;
}

.faqs-title {
    color: #16A34A;
}

/* .office-img {
    width: 570px;
    height: 300px;
} */

/* .question-item {
    background-color: ;
} */
/* .search {
    background-color: #fecf14;
}

.content-bg-green {
    background-color: #1e8133;
}

.content-bg-yellow {
    background-color: #fecf14;
} */
</style>
